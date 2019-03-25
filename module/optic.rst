Optical flow module 
=========================================================

.. image:: /_static/images/optical_nav.png
	:align: center

This module gives Pioneer the ability to position itself without using local or global navigation system. Optical sensor tracks object's shift.
The module is mounted below on the extension board. Connect it to the base board using flexible wire band. 

The module is also equipped with optical distance sensor. It can measure altitude above the floor or other objects from 0 to 1.5 m range. When Pioneer exceeds this value, the barometer is used for altitude control.

ap.goToLocalPoint(x, y z) is used for programmed flight. Pioneer sets the start point (0, 0, 0) when the motors are armed. Y - axes is directed forwards, X - axes is directed to the right. Z parameter represents the altitude. The (x, y, z) values set the coordinates where Pioneer will fly, based on start point. 

.. note:: Positioning error increases with flight distance.

Module can recognize the surface below it. The more details like ornament or tile junctures is visible, the more precise the flight will be. 

Select navigation system mode (Swb switch in middle position) to use optical flow module during manual-control flight. This will make it much more stabilized.


Optical flow module firmware update
-------------------------------------

To update the module firmware you should have Pioneer Station installed on your computer. Select "firmware update" in program's menu and follow the wizard instructions.
On "device choice" tab you will see both Pioneer Base and Optical flow module. Click the module checkbox and proceed. 
It is recommended to select "internal" firmware source. 
Wait for update to end, the quadcopter will be reloaded to standard mode.

Example
-------

This is an example program for optical flow module. Use Pioneer Station to `upload it to Pioneer`_. After that, turn the drone on, switch SwB in lower position and push start button on it. In 5 seconds it will lift off, fly 1 meter forward and return to launch point.

.. _upload it to Pioneer: ../programming/pioneer_station/pioneer_station_upload.html

::


	-- number of leds on base board (4)
	local ledNumber = 4
	-- create led control port
	local leds = Ledbar.new(ledNumber)
	-- speeding up function calls
	local unpack = table.unpack

	-- current state value
	local curr_state = "PREPARE_FLIGHT"

	-- function to change LEDs colour
	local function changeColor(color)
	    for i=0, ledNumber - 1, 1 do
	        leds:set(i, unpack(color))
	    end
	end 

	-- RGB colour code array to transfer changeColor
	local colors = {
	        {1, 0, 0}, -- red
	        {1, 1, 1}, -- white
	        {0, 1, 0}, -- green
	        {1, 1, 0}, -- yellow
	        {1, 0, 1}, -- purple
	        {0, 0, 1}, -- blue
	        {0, 0, 0}  -- black/LEDs turn off
	}

	-- array of functions, called depending on current condition
	action = {
	    ["PREPARE_FLIGHT"] = function(x)
	        changeColor(colors[2]) -- change LED colour to white
	        Timer.callLater(2, function () ap.push(Ev.MCE_PREFLIGHT) end) -- starting motors in 2 seconds
	        Timer.callLater(4, function () changeColor(colors[3]) end)-- change colour to green in 2 more seconds (4 seconds in total since timers start one after another right away)
	        Timer.callLater(6, function () 
	            ap.push(Ev.MCE_TAKEOFF) -- takoff in 2 more seconds (6 in total after all 3 timers are being set)
	            curr_state = "FLIGHT_TO_FIRST_POINT" -- next condition
	        end)
	    end,
	    ["FLIGHT_TO_FIRST_POINT"] = function (x) 
	        changeColor(colors[4]) -- change colour to yellow
	        Timer.callLater(2, function ()
	            ap.goToLocalPoint(0, 1, 1) -- go to point with coordinates (0 m, 1 m, 1 m)
	            curr_state = "FLIGHT_TO_SECOND_POINT" -- next condition
	        end) 
	    end,
	    ["FLIGHT_TO_SECOND_POINT"] = function (x) 
	        changeColor(colors[5]) -- change colour to purple
	        Timer.callLater(2, function ()
	            ap.goToLocalPoint(0, 0, 0.8) -- go to next start with coordinates (0 m, 0 m, 0.8 m)
	            curr_state = "PIONEER_LANDING" -- next condition
	        end)
	    end,
	    ["PIONEER_LANDING"] = function (x) 
	        changeColor(colors[6]) -- change colour to blue
	        Timer.callLater(2, function () 
	            ap.push(Ev.MCE_LANDING) -- perform landing
	        end)
	    end
	}

	-- condition processing function (created by autopilot automatically)
	function callback(event)
	    -- if set altitude reached, execute function from the array according to current condition
	    if (event == Ev.ALTITUDE_REACHED) then
	        action[curr_state]()
	    end
	    -- turn LEDs red in case of collision
	    if (event == Ev.SHOCK) then
	        changeColor(colors[1])

	    end
	    -- if set waypoint reached, execute function from the array according to current condition
	    if (event == Ev.POINT_REACHED) then
	        action[curr_state]()
	    end

	    -- turn off LEDs after landing
	    if (event == Ev.COPTER_LANDED) then
	        changeColor(colors[7])
	    end

	end

	-- turn red LED on
	changeColor(colors[1])
	-- start 2-second timer and execute first array function (flight preparation)
	Timer.callLater(2, function () action[curr_state]() end)
