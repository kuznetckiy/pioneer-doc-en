Changelog
=============

20/06/2019
------------

1. Pioneer Station update `version 1.10.0`_

- Added new protocol (1.5) support for new autopilot integration
- Now you can download logs in 2 ways:

    - Using Payload (slow). You need bootloader capable of log downloading.
    - Using Plazlink protocol (new faster method). You need firmware with 1.5 protocol support.
- New Lua script examples for Pioneer, with extended comments and optimised structure
- New LED module scrips examples
- Now you can inspect autopilot logs ( "work with logs" section in MENU)
  
.. _version 1.10.0: https://dl.geoscan.aero/pioneer/upload/GCS/GEOSCAN_Pioneer_Station.exe 

2. Autopilot firmware update (1.5.6173 version)

- Protocol update to 1.5
- Log download function
- Updated takeoff algorithm
- Fixed bug when 2 events were generated during takeoff. Now it's just **Ev.TAKEOFF_COMPLETE**. ALTITUDE_REACHED event is no longer used.
- Added engines start check during programmed flight



11/02/2019
-----------

1. Pioneer Station update `version 1.9.1`_

- Fixed code editor and telemetry bug when resizing application's window
- Fixed autopilot version indication in telemetry table
- Fized update lps module notification
- Added link to changelog menu-documentation

.. _version 1.9.1: hhttps://dl.geoscan.aero/pioneer/upload/GCS/archive/1.9.1/GEOSCAN_Pioneer_Station.exe


1/02/2019
-----------

1. Pioneer Station update `version 1.9.0`_

- Added accelerometer calibration
- Added compass calibration
- Added installed on a pioneer module version output. if its firmware is outdated its version will be highlighted in a table and a window inviting to update it will pop-up
- Added English localisation, when installing this application you will now have an option to choose between Russian and English
- Fixed an error when switching between LPS/GPS/OPT
- Added a fix to blank window with difference between parameters in file and in autopilot

.. _version 1.9.0: https://dl.geoscan.aero/pioneer/upload/GCS/archive/1.9.0/GEOSCAN_Pioneer_Station.exe

2. Autopilot firmware update (version 1.4.4922)

- Updated direction hold algorithm
- Fized an error with autopilot reboot when launching several timers at once
- Changed barometer data output, we no longer output raw data but filtered by AP data for tasts on Robofest

3. Module USNav firmware update (version 2.1)

- Updated data acquisition algorithm from transducers