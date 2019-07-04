Geoscan Locus indoor navigation system
=================================================
The system allows to create a controlled flyzone with max. dimensions of 10х10х4 meters. Safe and precise quadcopter control is provided without using global navigational systems (GPS/GLONASS) 

.. image:: /_static/images/indor_navigation.png
	:align: center

`Download LPS setting manual`_

The system includes:

* control module;
* 4 ultrasonic transducers;
* onboard module;
* connection cables;
* LPS software to sync with PC (`download latest version`_);

.. _download latest version: https://dl.geoscan.aero/pioneer/upload/LPS/Geoscan_LPS_en.exe
.. _Download LPS setting manual: https://dl.geoscan.aero/pioneer/upload/Docs/User_manual_Locus_en.pdf

Flyzone setting
----------------------------

.. image:: /_static/images/nav_system.PNG
	:align: center

Mount and connect the sensors in future flyzone. Transducers should be placed in the top corners of it in a way that they point in its centre. These conditions should be considered:

* minimum installation height - 2 m
* minimal distance  -3 m


.. note::
	System works better in rooms with soft floor cover

Using established cables, each sensor is connected to the corresponding socket of control module. The module itself should be placed outside of the zone and connected to USB port of a PC 

The control and onboard modules are communicating via radio channel. Measure the distance between the transducers and their installation height. A square is the most convenient and easy to set form of flight zone.

.. image:: /_static/images/indoor_prog.png
	:align: center

LPS user interface is shown on the picture. The program is used to calibrate the positioning system. Fill in the empty beacon coordinates:

* The distance (in meters) between 1 and 2 transducers is divided by 2. The "received value should be written in X string with a \"minus\" for №2 and №3 "beacons and without \"minus\"  for  №1 and №4.

* The distance between 2 and 3 transducers is also divided by 2 and is put in "the Y string with \"minus\" for  №3 and №4 beacons and without it for  №1 "and №2."

* String Z is for each beacon's height above floor level.

With all coordinates being set, the flyzone is ready and established in the right window with a green line. By default, its corners are 1 m away from the actual beacons. 


.. note::
	You can also set the flyzone parameters directly on the control module.

Push the start button next to spin selector once to turn it on. A green "\"status\" and white \"power\" light should appear. If they didn't, check the battery charge.

The top line of the screen shows a turn-on timer and battery voltage. If it drops lower than 3 V, a \"status\" light will start to blink. To charge it, just connect the module to PC or a 5V2A charger.

The coordinates are established in a same way as on LPS screen.

.. image:: /_static/images/indoor_board_int1.PNG
	:align: center

To select a menu item, twist the selector and push it like a button. Change the desired value by twisting it and push once again to confirm the changes and go back to the menu.

The second menu screen allows you to set indents from the flyzone border or toggle \"Auto\" mode for automatic setting. You can also turn the flyzone limits off.

.. image:: /_static/images/indoor_board_int2.PNG
	:align: center

.. note::
    You don't need PC for positioning system to operate. Just turn the control module on and place a Pioneer, equipped with onboard module, inside the flyzone

Indoor flight
---------------
To perform pre-programmed flight, connect battery, switch SwB to bottom position, and push start button on Pioneer base plate. The drone will begin script execution in 5 seconds. 
For manual flight control, switch  SwB in middle position. Pioneer will still maintain its position with Locus navigation system, so the flight will be smooth and easy to control.




Navigation system firmware update
---------------------------------------

To update the system firmware you will need Pioneer Station installed on your PC. Select Firmware update in the menu and follow the instructions on the screen.
Press and hold 'Menu' button on the control unit and connect it to your PC via USB cable. Select BeaconUSNav on the list and click 'next'

.. image:: /_static/images/usnav_upd.png
	:align: center
	:alt: version 4.1 update

It is recommended to choose default firmware source.
Flashing firmware may take up to 5 minutes, do not disconnect the control unit until it is finished.