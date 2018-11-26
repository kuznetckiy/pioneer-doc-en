Программируемая камера OpenMV
=============================

.. image:: /_static/images/openmv_module.png
	:align: center

Программируемая камера OpenMV позволяет обрабатывать видео поток на борту "Пионера" и осуществлять автономную навигацию по визуальным признакам.

Для установки камеры на квадрокоптер необходимо сначала установить модуль OpenMV. Крепление модуля на 4 винта М3 с подключением двух  разъемов. Затем, на установленный модуль крепится сама камера на два 8-пиновых разъема. Для дополнительной надежности можно использовать две стойки с винтами для соединения плат камеры и модуля.

 
Модуль OpenMV взаимодействует с базовой платой "Пионера" посредством интерфейса UART. При этом для квадрокоптера и камеры необходимо написать две отдельных программы с соответствующими командами, которые структурируют взаимодействие.
Для настройки соединения на "Пионере" используйте следующий код

::

  -- initialize UART interface
  local uartNum = 4 -- UART interface number (USART4)
  local baudRate = 9600 -- data speed
  local stopBits = 1
  local parity = Uart.PARITY_NONE
  local uart = Uart.new(uartNum, baudRate, parity, stopBits)    



Также необходимо указать протокол обмена на самой камере. Для этого `скачайте и запустите OpenMV IDE`_. Подключите камеру к компьютеру кабелем micro-USB и нажмите кнопку "Соединить" в левом нижнем углу OpenMV IDE. Теперь написанный в текстовом поле код можно загружать непосредственно в камеру, нажав кнопку "запустить" в левом нижнем углу.

Код для инициализации интерфейса  UART на камере

.. _скачайте и запустите OpenMV IDE: http://github.com/openmv/openmv-ide/releases/download/v2.0.0/openmv-ide-windows-2.0.0.exe

::

 uart = UART(3)[font=monospace][/font]
 uart.init(9600, bits=8, parity=None, stop=1, timeout_char=1000)



Протокол обмена между камерой и "Пионером" создан.



Далее приводится пример программ, меняющий яркость свечения светодиодов на "Пионере" в зависимости от яркости картинки, принимаемой камерой OpenMV. 
В коде обеих программ (для "Пионера" и OpenMV) уже содержатся блоки создания UART интерфейса.

код для "Пионера"

::

 -- -- https://learnxinyminutes.com/docs/lua/  - Lua  basic functional manual 
 -- number of leds on the baseboard
 local ledNumber = 4
 -- create led control port
 local leds = Ledbar.new(ledNumber)

 -- function to change base board led colour
 local function changeColor(red, green, blue)
    for i=0, ledNumber - 1, 1 do
        leds:set(i, red, green, blue)
    end
 end

 -- function to change leds colour red and turn off the timer
 local function emergency()
    takePhotoTimer:stop()
    -- As timer function executes once more after its stop, turn leds red in a second
    Timer.callLater(1, function () changeColor(1, 0, 0) end)
 end

 -- define function to analyse system events
 function callback(event)
    -- check battery voltage
    if (event == Ev.LOW_VOLTAGE2) then
        emergency()
    end
 end

 changeColor(1, 0, 0) -- red

 -- initialize UART interface
 local uartNum = 4 -- UART interface number (USART4)
 local baudRate = 9600 -- data speed
 local dataBits = 8
 local stopBits = 1
 local parity = Uart.PARITY_NONE
 local uart = Uart.new(uartNum, baudRate, parity, stopBits) -- create comm protocol

 changeColor(1, 0, 1) --purple

 --changeColor(1, 0.4, 0) --yellow

 local N = 10
 local i = 7
 local strUnpack = string.unpack
 function getData() -- function to get data byte
    --uart:write('abc', 3)
    --return 50
    --return uart:read(1) or 0
    i = i + 1
    if (i == N + 1) then i = 0 end
    buf = uart:read(uart:bytesToRead()) or '0'
    if (#buf == 0) then buf = '\0' end
    --buf = '0'
    leds:set(1, 0, i/N, 0.5 - 0.5*i/10)
    --local chr = buf[#buf]
    if (strUnpack ~= nil) then
        local b = strUnpack("B", buf)
        return b -- 
    else
        return 20
    end
    --string.byte(buf)
    --return #buf
    --return string.byte(string.sub(buf, -1)) or 0
    --return string.byte(buf) or 0
 end


 local takerFunction = function () -- function to read UART data 
  local intensity = getData() / 100.0
  changeColor(intensity, intensity, intensity)
 end
 local interval = 0.1
 getMeasureTimer = Timer.new(interval, takerFunction) -- photo timer
 getMeasureTimer:start()


 changeColor(1, 0.2, 0) -- orange


Код для openmv

::

 # Hello World Example
 #
 # Welcome to the OpenMV IDE! Click on the green run arrow button below to run the script!
 
 from pyb import UART, LED

 import sensor, lcd, image, time, utime

 ledBlue = LED(2)
 ledGreen = LED(3)

 ledBlue.on()
 sensor.reset()                      # Reset and initialize the sensor.
 sensor.set_pixformat(sensor.RGB565) # Set pixel format to RGB565 (or GRAYSCALE)
 sensor.set_framesize(sensor.LCD)   # Set frame size to QVGA (320x240)
 sensor.skip_frames(100)     # Wait for settings take effect.
 clock = time.clock()                # Create a clock object to track the FPS.
 lcd.init()
 #lcd.set_backlight(True)
 ledBlue.off()

 #Init uart

 uart = UART(3)
 uart.init(9600, bits=8, parity=None, stop=1, timeout_char=1000) # init with given parameters

 M_LED_COUNT = 10
 led_counter = M_LED_COUNT
 led_mode = 0
 while(True):
    clock.tick()                    # Update the FPS clock.
    clk = utime.ticks_ms()
    img = sensor.snapshot()         # Take a picture and return the image.
    #print(clock.fps())              # Note: OpenMV Cam runs about half as fast when connected
                                    # to the IDE. The FPS should increase once disconnected.

    for r in img.find_rects(threshold = 40000):
        img.draw_rectangle(r.x(), r.y(), r.w(), r.h(), (255, 0, 0))
        for p in r.corners():
            img.draw_circle(p[0], p[1], 5, color = (0, 255, 0))
        print(r)

    lcd.display(img)

    print(img.get_histogram().get_statistics().l_mean())
    uart.writechar(img.get_histogram().get_statistics().l_mean())
    led_counter = led_counter - 1
    if (led_counter == 0):
        if (led_mode == 0):
            ledGreen.on()
        else:
            ledGreen.off()
        led_mode = 1 - led_mode
        led_counter = M_LED_COUNT
    while (clk + 100 > utime.ticks_ms()):
        pass





Используя Pioneer Station и OpenMV IDE, `загрузите`_ соответсвующие программы на квадрокоптер и модуль камеры. Подключите аккумулятор к "Пионеру" и запустите выполнение программы. Протестируйте ее работу, направляя камеру на объекты с различной яркостью.

.. _загрузите: ../programming/pioneer_station/pioneer_station_upload.html





