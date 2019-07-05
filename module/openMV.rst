OpenMV camera
=============================

.. image:: /_static/images/openmv_module.png
	:align: center

OpenMV camera provides the ability for Pioneer to acquire video feed and process it onboard.

The camera is installed on the extension board using a dedicated module with X1 and X2 connectors. You may use two stands with screws to secure the camera.


For more details see `OpenMV documentation page`_

.. _OpenMV documentation page: http://docs.openmv.io/


 
OpenMV module communicates with the main Pioneer board using UART interface. You need two separate programs for Pioneer and camera to structure this protocol. Use the following code for Pioneer:

::

  -- initialize UART interface
  local uartNum = 4 -- UART interface number (USART4)
  local baudRate = 9600 -- data speed
  local stopBits = 1
  local parity = Uart.PARITY_NONE
  local uart = Uart.new(uartNum, baudRate, parity, stopBits)    



You should also activate the connection protocol on the camera. Для этого `Download and run the OpenMV IDE`_. Connect the camera to your PC with microUSB cable and click “Connect” button in the lower left corner of IDE window. From now the code from textbox may be uploaded to the camera by clicking “run” button.

Initialization code for UART interface on the camera:

.. _Download and run the OpenMV IDE: http://github.com/openmv/openmv-ide/releases/download/v2.0.0/openmv-ide-windows-2.0.0.exe

::

 uart = UART(3)[font=monospace][/font]
 uart.init(9600, bits=8, parity=None, stop=1, timeout_char=1000)



Comm protocol between Pioneer and the camera is created now.



An example code is established below. The program will change LED brightness depending on the brightness of OpenMV camera image. Both pieces of code (for Pioneer and the camera) contain UART protocol setting.

Code for Pioneer

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


Code for OpenMV

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





Use Pioneer station and OpenMV IDE to `upload`_ each program. Turn Pioneer ob and run the mission. Test how it works by pointing the camera towards objects with different brightness, or simply cover its lens by hand.

.. _upload: ../programming/pioneer_station/pioneer_station_upload.html





