RC transmitter 
=======================

This section describes the basics of Pioneer controls using FlySky i6S.

.. image:: /_static/images/remote_control.png
	:align: center

By default the controller is ready to use and doesn't require any additional setting
The drone is compatible with PPM protocol based controllers and uses a FlySky receiver
Use both sticks to control the drone
Use the mode switch to change the flight mode
Push and hold both power buttons to turn the controller on/off.

.. image:: /_static/images/remote_control_2.png
	:align: center

**Changing the controller mode**


* Use SwB switch. The top position is for manual control mode. To set the flight mode, use SwC switch.
* SwB switch in middle position turns navigational system mode on. Quadcopter will use GPS or local positioning system.
* SWB switch in lower position activates a pre-programmed “mission” flight mode
* When performing a mission flight, the operator can take controls by witching in nav system or manual control using SwB switch.


**Flight mode set**


* Use SwC switch. In top position - stabilize mode on. In this mode throttle stick changes the power output of motors.
* SWC switch in middle position activates althold  mode. Throttle stick controls vertical velocity. 
* The lower SwC switch activates headless mode. If you leave the throttle stick in middle position, the drone will hold current altitude. If the stick is moved up, the quadcopter will gain altitude
* If throttle stick is moved down, the drone will loose altitude. SW& switch in lower position activates loiter mode
* Altitude control is the same as before, but quadcopter will remember yaw direction when armed
* You can spin the drone with yaw stick but it will maintain the primary orientation


.. attention:: Flight controller uses a barometric sensor to estimate the altitude. When the pressure is changed, the drone will react according to it, not actual altitude change.

