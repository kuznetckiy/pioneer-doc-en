Autopilot logs 
================

From the moment of motors start, Pioneer's autopilot begins to log flight parameters in multiple channels and saves them in downloadable file. Log writing stops when motors are turned off. In case they are started again, previous log is erased to start a new one.

Log download
---------------------

To download the log file, connect Pioneer to the PC via USB and run Pioneer Station. Setup the connection and open **Logs - Download logs from AP by PlazLink** menu tab.

.. image:: /_static/images/log1.png
	:align: center 

After that, select saving folder and file name. Log download will start automatically. Pioneer will reboot when it's finished. 

Log preview
-------------------

To open log file, select **Logs - Open AP log** menu tab. 

.. image:: /_static/images/log2.png
	:align: center 

You can inspect autopilot log data in the opened tab. 

.. image:: /_static/images/log3.jpg
	:align: center 

* Select the channel you need and  **Add** it to the preview window.
* Customize data projection with checkboxes and plot color pallette.
* Use right mouse button and mouse wheel for plot navigation and scale change.
* To change time scale (horizontal), press and hold **Ctrl** and use mouse wheel. 
* To clear the preview window, select unnecessary channels and press **Remove** button. This will not erase the log, just remove plot image. 



+-------------------+------------------------------------------------------+
| Channel           | Description                                          |
+===================+======================================================+
| Altitude          | Copter altitude using baro sensor                    |
+-------------------+------------------------------------------------------+
| ControlData       | Autopilot control signals                            |
+-------------------+------------------------------------------------------+
| IntPowerStatus    | Battery voltage data                                 |
+-------------------+------------------------------------------------------+
| IntStaticPressure | Baro sensor data                                     |
+-------------------+------------------------------------------------------+
| LpsState          | Ultrasonic position system data                      |
+-------------------+------------------------------------------------------+
| MagHeading        | Magnetometer data                                    |
+-------------------+------------------------------------------------------+
| Motor-6X          | Motors speed controllers data                        |
+-------------------+------------------------------------------------------+
| NavPosition       | GPS position data                                    |
+-------------------+------------------------------------------------------+
| NavPrecision      | GPS precision data                                   |
+-------------------+------------------------------------------------------+
| NavSatInfo        | Amount of satellites and their status                |
+-------------------+------------------------------------------------------+
| NavVelocity       | 3-axis speed data using GPS                          |
+-------------------+------------------------------------------------------+
| OpticalFlow       | Optical flow data                                    |
+-------------------+------------------------------------------------------+
| Orientation       | Copter orientation data                              |
+-------------------+------------------------------------------------------+
| RawAccelGyroData  | RAW accelerometer and gyroscope data and temperature |
+-------------------+------------------------------------------------------+
| RemoteControlData | Control signals from RC unit                         |
+-------------------+------------------------------------------------------+
