Autopilot parameters setting
=================================

Autopilot parameters can be set precisely using `Pioneer Station`_. Follow the `manual`_ to connect Pioneer to your PC. Switch to “autopilot parameters setting” tab.

.. _Pioneer Station: ../programming/pioneer_station/pioneer_station_main.html

.. _manual: ../programming/pioneer_station/pioneer_station_upload.html

To perform a setting, Pioneer should be connected to PC and its current parameters should be displayed in right part of Pioneer station window.

.. image:: /_static/images/autopilot.PNG
	:align: center

Switch to “autopilot parameters” tab. Left column represents parameters, their current values are displayed in the second column. To change any of them, click to the right of selected parameter and type the new value in, then press Enter. Don’t forget to save the changes by clicking **save changes to autopilot**

**LPS**, **GPS** and **OPT** buttons are situated in the top part of the window. Each is responsible for loading a standard set of parameters for indoor positioning system, GPS and optical flow mode respectively.

.. note::
	If you want to return to factory preset, click **return to default** button above the table and wait until Pioneer reboots.


Parameters setting for motors change
-----------------------------------------------

Changing stock motors on Pioneer to more powerful (or any other at all) may result in different electrical specs. This, in turn, will lead to pre-flight checks fail, and Pioneer will not be able to take off. Adjust the following parameters to reset pre-flight checks:

* **Copter_motorCheckTime** - duration (in seconds) of motors rpm check before takeoff. Set to zero to turn it off.
* **Copter_startRpmMax** - Max rpm for pre-flight check pass.
* **Copter_startRpmMin** - Min rpm for pre-flight check pass.
* **Copter_startRpmSigma** - Max allowed inconsistency between motors rpm speeds.

There is another check, run for the first 5 seconds after takeoff command:

* **Copter_stallRpm** - max rpm after which sync fail is declared and motors stop. For custom motors, use its max possible rpm value (calculated as motor's *kv* multiplied by *battery voltage*) 

You may also need to customize PID parameters for different engines. If done incorrectly, this may result in noticable vibrations and oscillations during flight. 

To customize PIDs, adjust the following parameters:

* **Copter_xyRate_ki** - controller's integral component. Lower it If the controls are sluggish. Increase it if low-frequency oscillations are present.
* **Copter_xyRate_kp** - controller's proportional component. Lower it if the drone wobbles when hovering. Increase it if the controls feel sluggish. 

.. note:: To evaluate necessary rpm values, look through **rpm** data chart in **motor-x** autopilot logs channel after manual-controlled flight. See :doc:`logs` section.
