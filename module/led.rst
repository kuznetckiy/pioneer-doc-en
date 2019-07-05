LED module
==========


.. image:: /_static/images/led_module.png
	:align: center


LED module includes a board with 25 programmable leds, which is attached to the extension board. The connection is provided using X2 connector with parallel cable.

Depending on uploaded program, a LED module is used for backlighting or colour indication.

As an example a random colour program is given below. Use Pioneer Station to `upload the program`_

.. _upload the program: ../programming/pioneer_station/pioneer_station_upload.html



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

