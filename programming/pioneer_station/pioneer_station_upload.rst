Program upload to Pioneer
-----------------------------

After you developed program using TRIK Studio or Pioneer Station, it is time to upload it to Pioneer. Connect the drone to your PC.

Select “USB cable” connection in the bottom right corner of Pioneer Station window. Pioneer’s current parameters, lean angles and board number should appear on the screen.

.. image:: /_static/images/pioneer_station_connect.png
	:align: center

.. note:: 
	Try tilting the drone in different directions and make sure the angles are represented correctly. In case of error, check cable connection and firmware version on Pioneer, `update`_ it if necessary.

If program was created in TRIK studio, press “upload porgamm to Pioneer”. The code should appear in Pioneer Station window. Then press “Upload” button.

.. image:: /_static/images/pioneer_station_upload.png
	:align: center

"Receive" LED should blink on Pioneer board, and a message appear in the bootom right corner, indicating successful upload.

There are different ways to start mission execution. If flight is not required, don’t disconnect USB cable and press “Mission start” in Pioneer Station. In five seconds you will observe mission execution. USB power is enough to turn LEDs and some `extension modules`_ on.

--------------------------------------------------------------

**In other cases**, disconnect Pioneer from your PC and place it somewhere spacious enough to perform the mission. Plug in the battery. Wait for LEDs to stop blinking and press “Start” button on its board. The mission will execute in 5 seconds.

.. attention::
	To fly safely, it is recommended to have an RC transmitter ready and bind it to the drone. Activate mission flight by switching SwB in lower position. All other switches should be in upper position. If necessary, switch SwB in upper position - and now you have controls of the drone.


.. _update: ../../settings/firmware_upgrade.html


.. _extension modules: ../../module/module_main.html