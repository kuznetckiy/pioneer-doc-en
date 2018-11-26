Модуль LED
==========


.. image:: /_static/images/led_module.png
	:align: center


Модуль LED представляет собой плату с 25-ю программируемыми светодиодами, которая крепится к плате расширения снизу. Подключение через разъем X2 комплектным шлейфом.

Блок светодиодов может использоваться для подсветки или индикации событий в зависимости от заданной программы. 

В качестве примера приведена программа, случайно меняющая цвет блока светодиодов каждую секунду. Чтобы `загрузить программу на "Пионер"`_, воспользуйтесь Pioneer Station. 

.. _загрузить программу на "Пионер": ../programming/pioneer_station/pioneer_station_upload.html



::

 -- Number of leds on base board (4) + leds on module (25)
 local ledNumber = 29
 -- create led control port
 local leds = Ledbar.new(ledNumber)

 -- function to set leds colour
 local function changeColor(red, green, blue)
    for i=0, ledNumber - 1, 1 do
        leds:set(i, red, green, blue)
    end
 end

 -- function to turn off leds and timerRandomLED
 local function emergency()
    timerRandomLED:stop()
    -- As timer function executes once more after its stop, turn leds off in a second
    Timer.callLater(1, function () changeColor(0, 0, 0) end)
 end

 function callback(event)
    -- checks battery voltage
    if (event == Ev.LOW_VOLTAGE2) then
        emergency()
    end
 end

 -- create timer to change leds colour randomly every second
 timerRandomLED = Timer.new(1, function ()
    changeColor(math.random(), math.random(), math.random())
 end)
 -- timer start
 timerRandomLED:start()

