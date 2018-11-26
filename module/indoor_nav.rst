Бортовой модуль навигации в помещении
=====================================


.. image:: /_static/images/drone_indor_module.png
	:align: center

Модуль входит в комплект системы навигации в помещении вместе с блоком управления и четырьмя ультразвуковыми излучателями. 
Монтаж осуществляется на основной плате "Пионера" сверху с помощью 4-х винтов М3. Кабель подключается в разъем Х2.

Модуль оснащен двумя микрофонами, которые позволяют контроллеру оценивать время прихода и разность фаз сигналов с излучателей. Далее происходит синхронизация с блоком управления по радиоканалу и определяется точное положение квадрокоптера в пространстве, а также его скорость.

Для работы модуля необходимо расположить включенный квадрокоптер в зоне действия ультразвуковых излучателей.

**Дополнительно:** `Навигация в промещении`_

.. _Навигация в промещении: ../indoor_nav.html

Полет в системе навигации может осуществляться как в ручном режиме, так и по заранее загруженной программе. Пример такой программы приведен ниже. ВЫполняя её, "Пионер" взлетает, набирает высоту 1.2 м, летит в угол полетной зоны с координатами (0:0), затем в точку с координатами (1:1) и приземляется. Чтобы `загрузить программу на "Пионер"`_, воспользуйтесь Pioneer Station.

.. _загрузить программу на "Пионер": ../programming/pioneer_station/pioneer_station_upload.html



::

 -- current status variable
 local curr_state = "PREPARE_FLIGHT"

  
 -- table of functions called on different status
 action = {
    ["PREPARE_FLIGHT"] = function(x)
            Timer.callLater(2, function () 
            ap.push(Ev.MCE_TAKEOFF) -- send takeoff commend in 2 sec
            curr_state = "FLIGHT_TO_FIRST_POINT" -- go to next piont
        end)
    end,
    ["FLIGHT_TO_FIRST_POINT"] = function (x) 
            Timer.callLater(2, function ()
            ap.goToLocalPoint(0, 0, 1.2) -- command to autopilot go to point with coordinates(0 м, 0 м, 1,2 м)
            curr_state = "FLIGHT_TO_SECOND_POINT" 
        end) 
    end,
    ["FLIGHT_TO_SECOND_POINT"] = function (x) 
            Timer.callLater(2, function ()
            ap.goToLocalPoint(1, 1, 1.2) -- command to autopilot go to point with coordinates (1 м, 1 м, 1,2 м)
            curr_state = "PIONEER_LANDING" -- execute landing
        end)
    end,
    ["PIONEER_LANDING"] = function (x) 
            Timer.callLater(2, function () 
            ap.push(Ev.MCE_LANDING) -- autopilot command to land
        end)
    end
 }
 

 -- turn on red light
 changeColor(colors[1])
 -- start single-looped timer for 2 sec. When it's over, execute first function from the table (PREPARE_FLIGHT)
 Timer.callLater(2, function () action[curr_state]() end)

   