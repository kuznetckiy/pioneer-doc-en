GPS/GLONASS module
======================


.. image:: /_static/images/gps_module.png
	:align: center

The module allows to track current quadcopter's speed and position using global navigation systems. For better accuracy the drone should perform an outdoor flight away from high buildings and metal constructions.

GPS module is mounted on the main board using two connectors and M3 screws. The connector provide \"run-through\" connection to plug in extra modules on the lower board with parallel cables. 
GPS module is also equipped with a compass to get orientation data. This can be corrupted near large metal objects and buildings.

When `connected to Pioneer Station`_, make sure the GPS positioning mode is selected in "autopilot parameters", otherwise activate it manually. You can observe compass working in aviahorizon window when connected to PC in standard mode. The quantity of connected satellites is also shown there. The more satellites is "visible", the more positioning accuracy you gain. First-time activation in new location (cold-start) requires 1-3 minutes for module to synch. In case of success, green light on the module will stop flashing. Now, if disabled and then turned on in the same location after some time, the synchronization process will be much shorter.

.. _connected to Pioneer Station: ../programming/pioneer_station/pioneer_station_upload.html

+---------------+----------------------+---------------------------+
| LED           | status               | todo                      |
+===============+======================+===========================+
| red and green | wrong parameters     | change parameters for GPS |
+---------------+----------------------+---------------------------+
| red           | satellite search     | wait 1-3 minutes, reboot  |
+---------------+----------------------+---------------------------+
| green         | satellites connected | takeoff and fly           |
+---------------+----------------------+---------------------------+

When flying with radio transmitter control, use navigation system mode (SwB switch in middle position) to use GPS module and make the flight significantly more comfortable.

**ap.goToLocalPoint(x, y, z)** function is used for programmed flight. X - axis is directed to the East, Y - axis to the North. Z represents the altitude. 

Pioneer gets its position from the switch-on moment. This point is considered to be (0, 0, 0) in ap.goToLocalPoint command. 

Also, you can use **ap.goToPoint(x, y, z)** function. Here, x and y show geographical coordinates (latitude, longitude) of a point where Pioneer will fly. Z is still for altitude at this point.

.. note:: If stated start point is more than 500 meters away from the actual, quadcopter will not takeoff. Also, Flight_com_flyAreaSize and Flight_com_maxAltitude parameters set possible distance and altitude from the start point. Look for parameters description in Pioneer Station for more info.




Example
-------------

Below is a example of GPS module program. Upload it to Pioneer, find a spacious site and connect the LiPo battery. Wait until module LED stops blinking, then switch SwB to lower position and push "Start" button on the quadcopter's base board. In 5 seconds Pioneer will take-off, fly 10 meters east and return to launch point. 

Pioneer acquires its position at the moment of motors arm. This point is considered (0, 0, 0) in ap.goToLocalPoint command.

GPS position and altitude error may reach 3 meters, keep this in mind while planning the flight. 



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
	            ap.goToLocalPoint(0, 10, 3) -- go to point with coordinates (0 m, 10 m, 3 m)
	            curr_state = "FLIGHT_TO_SECOND_POINT" -- next condition
	        end) 
	    end,
	    ["FLIGHT_TO_SECOND_POINT"] = function (x) 
	        changeColor(colors[5]) -- change colour to purple
	        Timer.callLater(2, function ()
	            ap.goToLocalPoint(0, 0, 1) -- go to next start with coordinates (0 m, 0 m, 1 m)
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
