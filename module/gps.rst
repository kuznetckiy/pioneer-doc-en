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

	-- Simplification and caching table.unpack calls
	local unpack = table.unpack
	
	-- Base pcb number of RGB LEDs
	local ledNumber = 4
	-- RGB LED control port initialize
	local leds = Ledbar.new(ledNumber)
	
	-- Function changes color on all LEDs
	local function changeColor(color)
	    -- Changing color on each LED one after another
	    for i=0, ledNumber - 1, 1 do
	        leds:set(i, unpack(color))
	    end
	end 
	
	-- Table of colors in RGB form for changeColor function
	local colors = {
	        {1, 0, 0}, -- red
	        {0, 1, 0}, -- green
	        {0, 0, 1}, -- blue
	        {1, 1, 0}, -- yellow
	        {1, 0, 1}, -- violet
	        {0, 1, 1}, -- cyan
	        {1, 1, 1}, -- white
	        {0, 0, 0}  -- black/switched off
	}
	
	-- Flight mission points table formatted as {x,y,z}
	local points = {
	        {0, 0, 0.7},
	        {0, 1, 0.7},
	        {0.5, 1, 0.7},
	        {0.5, 0, 0.7}
	}
	-- Current point variable
	local curr_point = 1
	
	-- Function that changes LEDs color and flies to the next point
	local function nextPoint()
	    -- Current color. % - modulo, # - table size
	    -- This expression is used in order to circle through color's 	table even it is not the same size as points table
	    curr_color = ((curr_point - 1) % (#colors - 2)) + 1
	    -- Changing LEDs color
	    changeColor(colors[curr_color])
	    -- Flying to the next point if its number is less than number of 	points in the table "points"
	    if(curr_point <= #points) then
	        Timer.callLater(1, function()
	            -- Fly to point command in Positioning system
	            ap.goToLocalPoint(unpack(points[curr_point]))
	            -- Current point variable increase
	            curr_point = curr_point + 1
	        end)
	    -- Landing initiate if number of current point exceeds number of 	points in total
	    else
	        Timer.callLater(1, function()
	            -- Landing command
	            ap.push(Ev.MCE_LANDING)
	        end)
	    end
	end
	
	-- Event processing function called automatically by autopilot
	function callback(event)
	    -- After Pioneer reaches Flight_com_takeoffAlt, it starts mission 	flight
	    if(event == Ev.TAKEOFF_COMPLETE) then
	        nextPoint()
	    end
	    -- When Pioneer reaches current point it initiates flight to next 	point
	    if(event == Ev.POINT_REACHED) then
	        nextPoint()
	    end
	    -- After Pioneer lands, it switches off LEDs
	    if (event == Ev.COPTER_LANDED) then
	        changeColor(colors[8])
	    end
	end
	
	
	
	-- Pre-start preparations
	ap.push(Ev.MCE_PREFLIGHT)
	-- Changing LEDs color to white
	changeColor(colors[7])
	-- Timer, that calls takeoff function after 2 seconds
	Timer.callLater(2, function() ap.push(Ev.MCE_TAKEOFF) end)
	