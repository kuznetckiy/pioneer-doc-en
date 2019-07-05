Takeoff-mission-landing
==========================

To create real flight mission you can use examples fly_square or fly-to-point from TRIK library. 

.. note::
	Note that fly-to-point mission requires local positioning provided by `indoor navigation system`_. The system should be deployed and activated beforehand.

To fly in global coordinates, a `GPS module`_ is required.

.. _indoor navigation system: ../../indoor_nav.html

.. _GPS module: ../../const/module/gps.html

.. image:: /_static/images/trik_programm.PNG
	:align: center

Add “Yaw” and “Led” blocks to diversify your flight.

Having created the diagram, don’t forget to generate Lua code and check it for errors. Mission upload is performed `similarly as before`_.


.. _similarly as before: ../pioneer_station/pioneer_station_upload.html