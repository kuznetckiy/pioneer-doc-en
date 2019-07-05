Creating mission in TRIK studio
================================
Open TRIK studio and create new project (button in the top left corner). To create mission, drag and drop action blocks from Pallet to robot performance diagram, and then connect them.

**TRIK studio basics**


* Every mission should include “start” and “end” blocks. If the mission has multiple branches, each branch has to end with “end” block.
* To connect two blocks, use right mouse button. Press RMB on the first block, hold it, drag towards another block and release above it. Now these blocks are connected with a visible line.
* To tune selected block, use “Properties editor” window.
* To delete unnecessary block, select it with left mouse click and press del. key
* If you need to select multiple blocks, circle them holding left mouse button, or hold ctrl key and select them one by one.
* The diagram should not have red arrows and disconnected blocks. For better visibility arrange all blocks from top left to bottom right corner.
* Learn “hot keys” manual in “settings” tab. **Help (F1)** tab may be useful too.


**Pioneer programming blocks**


* **Condition** - creates two action scenarios depending on logical condition. The block should have two outgoing connections, one of which has to contain “condition” parameter (True or False)
* **End if** - marks merging point for two conditional operator branches. Provides structural integrity for diagram.
* **Variable initialisation** - states new variable. Use property editor to name and change it.
* **Random initialisation** - randomises selected value. Random range may be set.
* **Comments** - includes comments to provide better structure understanding.
* **Timer**- set time delay in milliseconds before next block will be executed.
* **Takeoff , Landing** - first and last block to perform flight mission.
* **Go to point** - set destination point coordinates. Don’t use dots or commas for latitude and longitude lines. “Altitude” shows distance to the ground (in meters) at the point.
* **Go to local point** - similar to previous, but destination point is set in local coordinates. Takoff point is considered for (0,0,0). Offset is set in meters.
* **LED** - controls Pioneer main board leds. Change each colour value to tune it precisely. Pair with “timer” block to set glowing duration.
* **Magnet** - controls `cargo module`_ operation. To turn magnet on, place a mark in a property checkbox.
* **System** - executes given instruction. Use Lua to create instruction in the editor and place a mark in a checkbox to run it.
* **Yaw** - controls Pioneer’s horizontal flight direction. To turn clockwise, set the angle with a “minus” sign.


Combining these blocks and connections, you can create any kind of flight mission imaginable.

.. _cargo module: ../../const/module/cargo.html



