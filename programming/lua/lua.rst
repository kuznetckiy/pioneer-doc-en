Lua API documentation
===========================

.. contents::
   :local:

.. highlight:: lua

Autopilot interaction API functions
==================================================

.. function:: time()

   Returns time since the moment of autopilot turn on

.. function:: deltaTime()

    Returns difference between :func:`time`, and GNSS time in seconds.


.. function:: launchTime()

    Returns launch time for navigation system.

.. function:: sleep(seconds)

    Pauses script execution for stated amount of time, argument may be fractional.
    **It is recommended  to use  :class:`Timer`, as "sleep" blocks further script execution.**

    :param seconds: Sleep time in seconds.

.. data:: boardNumber

    Returns aircraft ID - access using variable.

**Example**

.. code:: lua

    local boardNumber = boardNumber

.. function:: ap.push(Event)

   Add autopilot event (see :ref:`outluaevent`).

   :param Event: Event number or name (for example, ``Ev.COPTER_LANDED``).

**Example** 

.. code-block:: lua

    Ev.MCE_PREFLIGHT
    ap.push(Ev.MCE_PREFLIGHT)

.. function:: ap.goToPoint(latitude, longitude, altitude)

     for GPS mode flight.

    :param latitude: Set latitude in degrees, multiplied by :math:`10^{-7}`;
    :param longitude: Set longitude in degrees, multiplied by :math:`10^{-7}`;
    :param altitude:  Set altitude in meters.

    Example `ap.goToPoint(600859810, 304206500, 50)`

.. function:: ap.goToLocalPoint(x, y, z , time)

    For local positioning system:

    :param x: set point's `x` coordinate, meters.
    :param y: set point's `y`coordinate, meters.
    :param z: set point's `z`coordinate, meters.
    :param time: time required for copter to reach the next point, in seconds. If value is not stated, drone will fly with max speed.

    Example  ``ap.goToLocalPoint(1, 1, 1.2)`` или ``ap.goToLocalPoint(1, 1, 1.2, 10)``

.. function:: ap.updateYaw(angle)

    Set yaw.

    :param angle: angle in radian..

RGB LEDs
--------------

.. class:: Ledbar

    .. method:: new(Count)

        Create new Ledbar with stated amount of leds.

        :param Count: Led amount.

    .. method:: set(self, num, r, g, b)

        Set each led colour.

        :param num: Led number. Numeration 0 - 3 for main board, then for extra modules in series;
        :param r: red colour component intensity in [0;1] interval;
        :param g: green colour component intensity in [0;1] interval;
        :param b: blue colour component intensity in [0;1] interval;.

See also :func:`fromHSV`

**Example** 

Cargo module includes a magnet and 4 RGB leds.

.. code-block:: lua

    local leds = Ledbar.new(8)
    for i = 0, 3, 1 do
        leds:set(i, 1, 0, 0)
    end

    for i = 4, 7, 1 do
        leds:set(i, 0, 0.5, 0)
    end

.. function:: fromHSV(hue, saturation, value)

    Converts colour code from HSV to RGB. Can be used to set led colour.

    :param hue: set colour tone. Varies in [0;360] interval;
    :param saturation: set saturation. Varies in [0:100] interval;;
    :param value: set colour; Varies in [0:100] interval;
    :return: Returns RGB colour components.

GPIO (general-purpose input/output interface)
-------------------------------------------------

.. class:: Gpio

    .. method:: new(Port, Pin, Mode)

        create GPIO in settings port..

        :param Port: Gpio.A; Gpio.B; ... Gpio.E;
        :param Pin: pin number in port;
        :param Mode: Gpio.INPUT, Gpio.Output, Gpio.ALTFU.

    .. method:: read(self)

        Returns value.

    .. method:: set(self)

        Set value in 1.

    .. method:: reset(self)

        Set value in 0..

    .. method:: write(self, value)

        :param value: set value.

    .. method:: setFunction(self, num)

        Set alternative function number.

**Example** 

.. code-block:: lua

    local pin_name = Gpio.new(Gpio.A, 1, Gpio.OUTPUT)
    pin_name:read() -- get value
    pin_name:set() -- set value 1
    pin_name:reset() -- set value 0 
    pin_name:write(true) -- set value true
    pin_name:setFunction(1) -- set alternative function number


UART interface
-----------------

.. class:: Uart

    .. method:: new(num, rate, parity, stopBits)

        Create UART in settings port..

        :param num: UART number;;
        :param rate: speed;
        :param parity: Uart.PARITY_NONE, Uart.PARITY_EVEN, Uart.PARITY_ODD, not a necessary parameter, by default Uart.PARITY_NONE;
        :param stopBits: Uart.ONE_STOP, Uart.TWO_STOP, not a necessary parameter, by default Uart.ONE_STOP.

    .. method:: read(self, size)

        Read ``size`` byte.

    .. method:: write(self, data, size)

        Write (data) length of (size).

    .. method:: bytesToRead(self)

        Amount of data available to be read..

    .. method:: setBaudRate(self, rate)

        Set speed.

        :param rate: UART speed.


**Example**

.. code:: lua

    local uart = Uart.new(1, 115200)
    uart:read(10) -- read 10 bytes


SPI
-----

.. class:: Spi

    .. method:: new(num, rate, seq, mode)

        Create SPI in settings port.

        :param num: SPI number
        :param rate: speed
        :param seq: Spi.MSB, Spi.LSB, Spi.MSB_16, Spi.LSB_16, not a necessary parameter, by default Spi.MSB;
        :param mode: Spi.MODE0, Spi.MODE1, Spi.MODE2, Spi.MODE3, not a necessary parameter, by default Spi.MODE0.

    .. method:: read(self, size)

        read ``size`` byte.

    .. method:: write(self, data, size)

        Write (data) length of (size).

    .. method:: exchange(self, data, size)

        Write (data) length of (size) and read size.

**Example**

.. code:: lua

    local spi = Spi.new(2, 1000000)
    spi:exchange("hello", 5) -- Write (data) length of (size) and read size

Timers
-------

.. class:: Timer

    .. method:: new(sec, func)

        Create new Timer. 

        :param sec: Interval time in seconds.
        :param func: Function called with set interval.

    .. method:: start(self)

        Start timer.

    .. method:: stop(self)

        Stop timer. Full stop after last function execution.

    .. method:: callAt(local_time, func)

        Creates and starts new timer with function to be called ONCE.

        :param local_time: Local time (returns with :func:`time`), states function call moment.;
        :param func: Function to be called.

    .. method:: callLater(delay, func)

        Creates and starts new timer with function to be called ONCE.

        :param delay: Time before function start;
        :param func: Function to be called.

    .. method:: callAtGlobal(global_time, func)

        Creates and starts new timer with function to be called ONCE.

        :param global_time: Global time (:func:`time` + :func:`deltaTime`),  when to call function;
        :param func:  Function to be called.
        

.. note:: When using :func:`callAt()`, :func:`callLater()`, :func:`callAtGlobal()` functions, note that there can not be more than 16 waiting timers simultaneously. If this amount is exceeded, new tier will not be created.


**Example**

See :ref:`Example`

.. _OutLuaEvent:

Autopilot events
--------------------------------

Events a presented using constants with “Ev” prefix

+----------------+-------------------------------------------+
|    Name        |                  Description              |
+================+===========================================+
| MCE_PREFLIGHT  | Motors start and execute pre-flight check |
+----------------+-------------------------------------------+
| ENGINES_DISARM | Stop motors                               |
+----------------+-------------------------------------------+
| MCE_LANDING    | perform landing                           |
+----------------+-------------------------------------------+
| MCE_TAKEOFF    | perform takeoff                           |
+----------------+-------------------------------------------+
| --устаревшие-- |                                           |
+----------------+-------------------------------------------+
| ENGINES_ARM    | Start motors                              |
+----------------+-------------------------------------------+

Get autopilot data
==============================

To get autopilot data **Sensors** class is used

.. class:: Sensors

    .. method:: lpsPosition()

        :return: x, y, z

    .. method:: lpsVelocity()

        :return: vx, vy, vz

    .. method:: lpsYaw()

        :return: yaw

    .. method:: orientation()

        Position data.

        :return: roll, pitch, azimuth

    .. method:: altitude()

        Altitude data fro barometer.

        :return: altitude in meters

    .. method:: range()

        Data from proximity sensors/

        :return: Returns proximity sensor distance. Several values.

    .. method:: accel()

        Accelerometer data.

        :return: ax, ay, az

    .. method:: gyro()

        Gyroscope data.

        :return: gx, gy, gz

    .. method:: rc()

        RC transmitter data.

        :return: channel1, channel2, channel3, channel4, channel5, channel6, channel7, channel8

**Examples**

.. code:: lua

    local lpsPosition = Sensors.lpsPosition
    local lpsVelocity = Sensors.lpsVelocity
    local lpsYaw = Sensors.lpsYaw
    local orientation = Sensors.orientation
    local range = Sensors.range
    local accel = Sensors.accel
    local gyro = Sensors.gyro
    local rc = Sensors.rc

    lpsX, lpsY, lpsZ = lpsPosition()
    lpsVelX, lpsVelY, lpsVelZ = lpsVelocity()
    yaw = lpsYaw()

    roll, pitch, azimuth = orientation()

    range1, range2, _,_, range3 = range()

    ax, ay, az = accel()
    gx, gy, gz = gyro()
    aileron, _, _, _, _, _, _, ch8, = rc()


Service functions of the script
==============================================

.. code:: lua

    function callback(event) -- is called when autopilot event is received
    end

Autopilot events available:

+--------------------+-----------------------------------------------------------+
|      Name          |                          Description                      |
+====================+===========================================================+
| ENGINES_STARTED    | Engines started                                           |
+--------------------+-----------------------------------------------------------+
| COPTER_LANDED      | Quadcopter performed landing                              |
+--------------------+-----------------------------------------------------------+
| TAKEOFF_COMPLETE   | Quadcopter has reached takeoff altitude                   |
+--------------------+-----------------------------------------------------------+
| POINT_REACHED      | Quadcopter has reached destination point                  |
+--------------------+-----------------------------------------------------------+
| POINT_DECELERATION | Quadcopter starts decelerating near the point             |
+--------------------+-----------------------------------------------------------+
| LOW_VOLTAGE1       | low battery voltage, return home mode                     |
+--------------------+-----------------------------------------------------------+
| LOW_VOLTAGE2       | low battery voltage, landing mode                         |
+--------------------+-----------------------------------------------------------+
| SYNC_START         | Syncronised start signal received from positioning system |
+--------------------+-----------------------------------------------------------+
| SHOCK              | Collision or hard vibration                               |
+--------------------+-----------------------------------------------------------+
| CONTROL_FAIL       | Pitch angle exceeded maximum value                        |
+--------------------+-----------------------------------------------------------+
| ENGINE_FAIL        | Engine fail                                               |
+--------------------+-----------------------------------------------------------+

.. note:: Ev.ALTITUDE_REACHED ( quadcopter has reached destination point) event is no more used starting from autopilot version 1.5.6173.

.. _Example:

Script example
================

.. code:: lua

    local boardNumber = boardNumber
    local unpack = table.unpack
    local points = {
            {-0.6, 0.3, 0.2},
            {0.6, 0.3,  0.2},
            {0, 0, 0.5},
            {0.6, -0.3, 0.2}
    }

    local curr_point = 1

    local function nextPoint()
        if(#points >= curr_point) then
            ap.goToLocalPoint(unpack(points[curr_point]))
            curr_point = curr_point + 1
        else
            ap.push(Ev.MCE_LANDING)
        end
    end

    function callback(event)
        if(event == Ev.TAKEOFF_COMPLETE) then
            nextPoint()
        end
        if(event == Ev.POINT_REACHED) then
            nextPoint()
        end
    end


    local leds = Ledbar.new(1)
    local blink = 0
    leds:set(0,1,1,1)
    timerBlink = Timer.new(1, function ()
            if(blink == 1) then
                blink = 0
            else
                blink = 1
            end
            leds:set(0, blink, 0, 0)
    end)
    timerBlink:start()
    ap.push(Ev.MCE_PREFLIGHT)
    Timer.callLater(1, function() ap.push(Ev.MCE_TAKEOFF) end)

Description for modules pin connectors
========================================

``Pioneer_Base_v.1.0-v.1.1`` autopilot controller pins

+--------------------+---------------------------+---------------------------+
| X1 (MC pin)        |          Function         |          Description      |
+====================+===========================+===========================+
| 1                  | 5V power (only with LiPo) 2A max.                     |
+--------------------+---------------------------+---------------------------+
| 2                  | 3.3V power  2A max.                                   |
+--------------------+---------------------------+---------------------------+
| 3 (PA12)           | USART1_RTS                |                           |
+--------------------+---------------------------+---------------------------+
| 4 (PA11)           | USART1_CTS, TIM1_CH4      | Module_OpenMV             |
+--------------------+---------------------------+---------------------------+
| 5 (PA10)           | USART1_RX, TIM1_CH3       | Module_GPS, Module_USNav  |
+--------------------+---------------------------+---------------------------+
| 6 (PA9)            | USART1_TX, TIM1_CH2       | Module_GPS, Module_USNav  |
+--------------------+---------------------------+---------------------------+
| 7 (PA15)           | SPI3_NSS, TIM2_CH1        | Module_GPS, Module_OpenMV |
+--------------------+---------------------------+---------------------------+
| 8 (PC10)           | SPI3_SCK                  | Module_GPS, Module_OpenMV |
+--------------------+---------------------------+---------------------------+
| 9 (PC11)           | SPI3_MISO                 | Module_GPS, Module_OpenMV |
+--------------------+---------------------------+---------------------------+
| 10 (PB5)           | SPI3_MOSI, TIM3_CH2       | Module_GPS, Module_OpenMV |
+--------------------+---------------------------+---------------------------+
| 11                 | Земля                     |                           |
+--------------------+---------------------------+---------------------------+
| 12                 | Земля                     |                           |
+--------------------+---------------------------+---------------------------+

|

+--------------------+------------------------------+------------------------------+
| X2 (MC pin)        |          Function            |            Description       |
+====================+==============================+==============================+
| 1                  | 5V power (only with LiPo) 2A max.                           |
+--------------------+------------------------------+------------------------------+
| 2                  | 3.3V power  2A max.                                         |
+--------------------+------------------------------+------------------------------+
| 3 (PC2)            | ADCx_IN12                    |                              |
+--------------------+------------------------------+------------------------------+
| 4 (PC3)            | ADCx_IN13                    |                              |
+--------------------+------------------------------+------------------------------+
| 5 (PA1)            | ADCx_IN1, TIM2_CH2, TIM5_CH2 | Module_Cargo (упр. магнитом) |
+--------------------+------------------------------+------------------------------+
| 6 (PB7)            | I2C1_SDA, TIM4_CH2           | Module_ToF, Module_OpenMV    |
+--------------------+------------------------------+------------------------------+
| 7 (PB6)            | I2C1_SCL, TIM4_CH1           | Module_ToF, Module_OpenMV    |
+--------------------+------------------------------+------------------------------+
| 8 (PA0)            | DATA WS2812B                 | Уровень 5 В. Module_LED      |
+--------------------+------------------------------+------------------------------+
| 9                  | Земля                        |                              |
+--------------------+------------------------------+------------------------------+
| 10                 | Земля                        |                              |
+--------------------+------------------------------+------------------------------+

For ``Pioneer_Base_v.1.2``:

+--------------------+-----------------------------------------+------------------------------+
| X2 (MC pin)        |                 Function                |           Description        |
+====================+=========================================+==============================+
| 3 (PA0)            | USART4_TX, ADCx_IN0, TIM2_CH1, TIM5_CH1 | Module_OpenMV                |
+--------------------+-----------------------------------------+------------------------------+
| 4 (PA1)            | USART4_RX, ADCx_IN1, TIM2_CH2, TIM5_CH2 | Module_OpenMV                |
+--------------------+-----------------------------------------+------------------------------+
| 5 (PС3)            | ADCx_IN13, SPI2_MOSI                    | Module_Cargo (упр. магнитом) |
+--------------------+-----------------------------------------+------------------------------+
| 8 (PC12)           | DATA WS2812B                            | 5 V level. Module_LED        |
+--------------------+-----------------------------------------+------------------------------+
