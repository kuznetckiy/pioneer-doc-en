Extension modules board
========================================

Extension modules board provides the ability to plug in different module to the base board.

.. image:: /_static/images/modules_board.png
	:align: center

Use the arrow markers to install the board properly (they should point in the same direction). The board itself is mounted on the lower part of the drone instead of battery bay cover. Use 4 stands and M3 screws.

There are two connectors on top of the extension board. They are used to connect with the base board using two parallel cables. Two connectors on the bottom are used to plug in different modules. The connection is established “through” the board.

The board is equipped with laser range sensor as well. It provides the ability to precisely estimate the altitude up to 1.5 m above the surface. The sensor is activated in alt.hold mode. Pioneer will remain the same altitude regarding the surface. If the distance between the drone and the surface changes, Pioneer will react, trying to compensate the change. 

You can acquire the sensors data to control the flight. As an example, here is a program which will change the colour of Pioneer LEDs depending on its distance to the ground. Use Pioneer Station to `upload the program`_ to Pioneer.

.. note::
	The range sensor is not active in GPS flight mode. If the program doesn’t work, open the "`autopilot parameters`_" tab in Pioneer Station window and select LPS or OPT parameters setting. You can also change the BoardPioneer_modules_gnss parameter to 0.0 manually. 


.. _upload the program: ../programming/pioneer_station/pioneer_station_upload.html 
.. _autopilot parameters: ../settings/autopilot_parameters.html

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
