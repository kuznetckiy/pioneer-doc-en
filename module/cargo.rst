Модуль захвата груза
====================

.. image:: /_static/images/cargo_module.png
	:align: center

Модуль захвата груза предназначен для  захвата предметов, обладающих магнитным свойствами, оснащен управляемым электромагнитом на гибком подвесе и RGB светодиодами.

Модуль монтируется на плату подключения дополнительных модулей на 4 винта М3, при этом обеспечивается надежное соединение разъемов.

Модулем удобно управлять по 8 каналу с помощью двухпозиционного переключателя на пульте. Для настройки канала на нем необходимо во вкладке function найти пункт Aux. channels, выбрать channel 8 и установить переключатель SwA (крайний левый) как управляющий. 
Теперь необходимо `загрузить на "Пионер"`_ программу для модуля груза. В качестве примера используйте код, представленный ниже. После его загрузки вы сможете управлять работой магнита с помошью крайнего левого тумблера на пульте.

.. _загрузить на "Пионер": ../programming/pioneer_station/pioneer_station_upload.html 

::

    -- https://learnxinyminutes.com/docs/lua/  - Lua  basic functional manual 
    -- initialise PC3 port to control the cargo module (board version 1.2) 
    local magneto = Gpio.new(Gpio.C, 3, Gpio.OUTPUT)
    -- initialise PC3 port to control the cargo module (board version 1.2) (uncomment the string below and comment the string above)
    -- local magneto = Gpio.new(Gpio.A, 1, Gpio.OUTPUT)
    -- set led number (4 on the main board and 4 on the cargo module)
    local led_number = 8
    -- initialise leds
    local leds = Ledbar.new(led_number)
    local rc = Sensors.rc
    local blink = 0
    function callback(event)
    end

    local function changeColor(red, green, blue)
        for i=0, led_number - 1, 1 do
            leds:set(i, red, green, blue)
        end
    end

    cargoTimer = Timer.new(0.1, function () -- create timer to call the funcion every 0.1 second
        _, _, _, _, _, _, _, ch8 = rc() -- get signal from channel 8 on the transmitter, range from -1 to +1
        if(ch8 < 0) then  -- if singnal is -1 (upper position), turn off
            magneto:reset()
            changeColor(0, 1, 0) -- and colour leds green to indicate
        else if(ch8 > 0) then -- if signal is +1 (lower position), activate magnet
            magneto:set()
            changeColor(1, 0, 0) -- colour led red to indicate 
        else -- blinking blue to indicate loss of control signal 
            if(blink < 5) then
                changeColor(0, 0, 1)
               blink = blink + 1
            else
                changeColor(0, 0, 0)
                blink = 0
            end
        end
    end
    end)
     -- timer start
    cargoTimer:start()





