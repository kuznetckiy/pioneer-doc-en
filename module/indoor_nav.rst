Onboard indoor navigation module
=====================================


.. image:: /_static/images/drone_indor_module.png
	:align: center

The module is included in indoor navigation system kit along with control module and 4 ultrasonic transducers. It is mounted on top of  the main board.

The module is quipped with two microphones that allow the controller estimate phases and delays of ultrasonic signals. It is synced with the control module via radio channel and locates the drone's speed and position.

Turn Pioneer on and place it in ultrasonic beacon's operation area to activate the module.

**Additional:** `Indoor navigation`_

.. _Indoor navigation: ../indoor_nav.html

Indoor flight can be performed both in manual or mission mode. Example of such mission program is established below. According to it, Pioneer will take-off, gains 1.2 m altitude, then flies to the corner of flyzone with (0:0) coordinates, and lands in the (1:1) point. Use Pioneer station to `upload the program`_

.. _upload the program: ../programming/pioneer_station/pioneer_station_upload.html



::

 -- current state variable
 local curr_state = "PREPARE_FLIGHT"

  
 -- table of functions, depending on state
 action = {
    ["PREPARE_FLIGHT"] = function(x)
            Timer.callLater(2, function () 
            ap.push(Ev.MCE_TAKEOFF) -- send start comend to autopilot in 2 seconds
            curr_state = "FLIGHT_TO_FIRST_POINT" -- next state
        end)
    end,
    ["FLIGHT_TO_FIRST_POINT"] = function (x) 
            Timer.callLater(2, function ()
            ap.goToLocalPoint(0, 0, 1.2) -- send command to autopilot to fly to piont (0 m, 0 m, 1,2 m)
            curr_state = "FLIGHT_TO_SECOND_POINT" -- next state
        end) 
    end,
    ["FLIGHT_TO_SECOND_POINT"] = function (x) 
            Timer.callLater(2, function ()
            ap.goToLocalPoint(1, 1, 1.2) -- send command to autopilot to fly to piont (1 m, 1 m, 1,2 m)
            curr_state = "PIONEER_LANDING" -- next state
        end)
    end,
    ["PIONEER_LANDING"] = function (x) 
            Timer.callLater(2, function () 
            ap.push(Ev.MCE_LANDING) -- send command to autopilot to land
        end)
    end
 }
 

 -- turn on the red LED)
 changeColor(colors[1])
 -- Start single 2 seconds timer, then execute first function from the table (preflight)
 Timer.callLater(2, function () action[curr_state]() end)

   
Navigation module firmware update
-------------------------------------

To update module's firmware, you will need Pioneer Station installed on your PC. Select Firmware update in the menu and follow the instructions on the screen.
At 'choose device' step, both Pioner base board and Navigation module will be displayed on the list. Select the module and click 'next'

It is recommended to choose default firmware source. 
If module version is not defuned automatically, read the marking on the backside of the plate and select the corresponding firmware file from Pioneer Station folder.
Wait for the update to finish. Quadcopter will reboot in standard mode after that.