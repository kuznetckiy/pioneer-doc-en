Cargo module
====================

.. image:: /_static/images/cargo_module.png
	:align: center

Cargo module is designed to hold magnetic objects. It is equipped with electromagnet on a flexible gimbal and 4 RGB LEDs.

Cargo module is mounted on the extension board using 4 M3 screws.

Use 8 channel of the RC unit to operate the module. A free SwA flipswitch can be used. To set the channel, turn on the RC unit, use touchscreen to select function-AUX Channels - 8 and choose SwA as a primary. Now you should `upload the program`_ for Pioneer’s cargo module. Use the given code, which allows to control the magnet with SwA switch.

.. _upload the program: ../programming/pioneer_station/pioneer_station_upload.html 

::

    -- Magnet control port (PC3) initialize for Pioneer_Base v 1.2
    local magnet = Gpio.new(Gpio.C, 3, Gpio.OUTPUT)
    -- Magnet control port (PA1) initialize for Pioneer_Base v 1.1 (    uncomment line below and comment line above) 
    -- local magnet = Gpio.new(Gpio.A, 1, Gpio.OUTPUT)
    
    -- Total number of LEDs (4 on base pcb and 4 on a cargo module)
    local led_number = 8 
    -- RGB LED control port initialize
    local leds = Ledbar.new(led_number) 
    -- Magnet state (switched on initially)
    local magnet_state = true
    
    -- Function that sets LEDs color based of magnet state
    local function setLed(state)
        if (state == true) then
            color = {1,1,1}                  -- If magnet is on, color is   white
        else
            color = {0,0,0}                  -- If magnet is off, color is  black (LEDs are off)
        end
        for i = 4, led_number - 1, 1 do      -- Set color for each of LEDs
            leds:set(i, table.unpack(color)) 
        end
    end
    
    -- Magnet switch function
    local function toggleMagnet()
        if (magnet_state == true) then  -- If magnet is on, we switch it off
            magnet:reset()
        else                            -- If magnet is off, we switch it on
            magnet:set()
        end
        magnet_state = not magnet_state -- Changing magnet state value to   appropriate state
    end
    
    -- Required callback function to process events
    function callback(event)
    end
    
    -- Timer creation, that calls our function each second
    cargoTimer = Timer.new(1, function ()
        toggleMagnet()
        setLed(magnet_state)
    end)
    -- Timer start
    cargoTimer:start()




