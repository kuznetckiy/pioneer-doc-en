Плата подключения дополнительных модулей
========================================

Плата подключения дополнительных модулей обеспечивает возможность расширения функционала "Пионера" за счет установки полезной нагрузки.

.. image:: /_static/images/modules_board.png
	:align: center

При установке следует ориентировать плату в соответствии с маркировкой в центре (стрелка).
Плата крепится на нижнюю часть квадрокоптера вместо крышки отсека аккумулятора через стойки винтами М3.

Сверху на плате расположены два разъема для соединения с базовой платой квадрокоптера входящими в комплект шлейфами. На нижней поверхности  - два разъема для подключения модулей. Подключение модулей осуществляется "насквозь" через плату.
 
Также плата подключения дополнительных модулей оснащена лазерным дальномером, позволяющим точно определять высоту над уровнем пола в пределах  0 - 1.5 м. Дальномер активируется в режиме удержания высоты. При этом "Пионер" будет сохранять одинаковое расстояние от поверхности, даже если она неровная. Пролет над препятствием или впадиной повлечет за собой соответствующее изменение высоты полета квадрокоптера. Если закрыть датчик рукой или другим предметом, "Пионер" будет набирать высоту до тех пор, пока расстояние до препятствия не вернется к прежнему значению, или пока квадрокоптер не упрется в потолок. 

Данные с дальномера можно считывать и использовать для управления квадрокоптером. В качестве примера приведена программа, меняющая цвет светодиодов на "Пионере" в зависимости от расстояния до земли. Используйте Pioner Station, чтобы  `загрузить программу`_ на "Пионер".

.. note::
	Дальномер неактивен в режиме полета по GPS. Если программа не работает, откройте вкладку "`параметры автопилота`_ " в Pioneer Station и выберите набор параметров "LPS" или "OPT". Также можно вручную поменять значение параметра BoardPioneer_modules_gnss на 0.0. 


.. _загрузить программу: ../programming/pioneer_station/pioneer_station_upload.html 
.. _параметры автопилота: ../settings/autopilot_parameters.html

::

    -- https://learnxinyminutes.com/docs/lua/  - Lua  basic functional manual 
    -- simpligy function call for laser range sensor distance data
    local range = Sensors.range
    -- number of leds on the baseboard
    local ledNumber = 4
    -- creating led control port
    local leds = Ledbar.new(ledNumber)

    -- led colour change function
    local function changeColor(red, green, blue)
	    -- changes colour of reach led 
        for i = 0, ledNumber - 1, 1 do
            leds:set(i, red, green, blue)
        end
    end

    function callback(event)
    end

    -- create a timer to get altitude from the sensor every 0.1 second
    timerRangeRead = Timer.new(0.1, function ()
        -- get altitude on meters from laser sensor (comes №5 in line) 
        _, _, _, _, distance = range()
        r, g, b = 0, 0, 0
        -- if range is beyond maximum value , it will show constatnt altitude of 8.19 mп
        if (distance >= 8.19) then
            -- in that case leds will colour red
            r = 1
        else
            -- Change leds brightness according to distance ( ~1.5 m is a maxinum range fir laser sensor on the extension board.)
            g = math.abs(distance / 1.5)
        end
        -- change led colour to stated above
        changeColor(r, g, b)
    end)
    -- timer start
    timerRangeRead:start()
