---
title: "Z-Wave Device Specific Settings"
description: "Notes for specific Z-Wave devices."
redirect_from: /getting-started/z-wave-device-specific/
---

## Device Categories

### Motion or alarm sensors

In order for Home Assistant to recognize the sensor properly, you will need to change its configuration from `Basic Set (default)` to `Binary Sensor report` or `Alarm report`.
These devices will either show as a binary sensor or a sensor called `Alarm xxxx` and will report a numeric value. Test to see what value is what. Sometimes this is noted in the device manual.

You can set the settings of the Z-Wave device through the Z-Wave control panel.

### Locks and other secure devices

These devices require a network key to be set for the Z-Wave network before they are paired, using the **Add Node Secure** option.

Home Assistant stores logs from Z-Wave in `OZW_log.txt` in the Home Assistant config directory, when you pair a secure device you should see communication from the node with lines starting with `info: NONCES` in `OZW_log.txt` when the device is paired successfully with a secure connection.

### Specific Devices

### Aeotec Z-Stick

It's totally normal for your Z-Wave stick to cycle through its LEDs (Yellow, Blue and Red) while plugged into your system. If you don't like this behavior it can be turned off.

Use the following example commands from a terminal session on your Pi where your Z-Wave stick is connected.

**Note:** You should only do this when Home Assistant has been stopped.

Turn off "Disco lights":

```bash
$ echo -e -n "\x01\x08\x00\xF2\x51\x01\x00\x05\x01\x51" > /dev/serial/by-id/usb-0658_0200-if00
```

Turn on "Disco lights":

```bash
$ echo -e -n "\x01\x08\x00\xF2\x51\x01\x01\x05\x01\x50" > /dev/serial/by-id/usb-0658_0200-if00
```

If the above two commands give errors about not having that device, you should try replacing the `/dev/serial/by-id/usb-0658_0200-if00` with `/dev/ttyACM0` or `/dev/ttyACM1` (depending on which tty your Aeotec stick is addressed to).

On some systems, such as macOS, you need to pipe the output of the `echo` command, rather than redirecting to the serial device, to something like `cu` (replacing `/dev/zstick` acccordingly) to properly set the baud rate to 115200 bps:

```bash
echo -e -n "...turn on/off string from examples above..." | cu -l /dev/zstick -s 115200
```

### Razberry Board

You need to disable the on-board Bluetooth since the board requires the use of the hardware UART (and there's only one on the Pi3). You do this by adding the following to the end of `/boot/config.txt`:

```text
dtoverlay=pi3-disable-bt
```

Then disable the Bluetooth modem service:

```bash
$ sudo systemctl disable hciuart
```

Once Bluetooth is off, enable the serial interface via the `raspi-config` tool. After reboot run:

```bash
$ sudo systemctl mask serial-getty@ttyAMA0.service
```

so that your serial interface looks like:

```text
crw-rw---- 1 root dialout 204, 64 Sep  2 14:38 /dev/ttyAMA0
```
at this point simply add your user (homeassistant) to the dialout group:

```bash
$ sudo usermod -a -G dialout homeassistant
```

<div class='note'>

  If you've installed the Z-Way software, you'll need to ensure you disable it before you install Home Assistant or you won't be able to access the board. Do this with `sudo /etc/init.d/z-way-server stop; sudo update-rc.d z-way-server disable`.

</div>

### Aeon Minimote

Here's a handy configuration for the Aeon Labs Minimote that defines all possible button presses. Put it into `automation.yaml`.

```yaml
  - id: mini_1_pressed
    alias: 'Minimote Button 1 Pressed'
    trigger:
      - platform: event
        event_type: zwave.scene_activated
        event_data:
          entity_id: zwave.aeon_labs_minimote_1
          scene_id: 1
  - id: mini_1_held
    alias: 'Minimote Button 1 Held'
    trigger:
      - platform: event
        event_type: zwave.scene_activated
        event_data:
          entity_id: zwave.aeon_labs_minimote_1
          scene_id: 2
  - id: mini_2_pressed
    alias: 'Minimote Button 2 Pressed'
    trigger:
      - platform: event
        event_type: zwave.scene_activated
        event_data:
          entity_id: zwave.aeon_labs_minimote_1
          scene_id: 3
  - id: mini_2_held
    alias: 'Minimote Button 2 Held'
    trigger:
      - platform: event
        event_type: zwave.scene_activated
        event_data:
          entity_id: zwave.aeon_labs_minimote_1
          scene_id: 4
  - id: mini_3_pressed
    alias: 'Minimote Button 3 Pressed'
    trigger:
      - platform: event
        event_type: zwave.scene_activated
        event_data:
          entity_id: zwave.aeon_labs_minimote_1
          scene_id: 5
  - id: mini_3_held
    alias: 'Minimote Button 3 Held'
    trigger:
      - platform: event
        event_type: zwave.scene_activated
        event_data:
          entity_id: zwave.aeon_labs_minimote_1
          scene_id: 6
  - id: mini_4_pressed
    alias: 'Minimote Button 4 Pressed'
    trigger:
      - platform: event
        event_type: zwave.scene_activated
        event_data:
          entity_id: zwave.aeon_labs_minimote_1
          scene_id: 7
  - id: mini_4_held
    alias: 'Minimote Button 4 Held'
    trigger:
      - platform: event
        event_type: zwave.scene_activated
        event_data:
          entity_id: zwave.aeon_labs_minimote_1
          scene_id: 8
```

### Zooz Toggle Switches

Some models of the Zooz Toggle switches ship with an instruction manual with incorrect instruction for Z-Wave inclusion/exclusion. The instructions say that the switch should be quickly switched on-off-on for inclusion and off-on-off for exclusion. However, the correct method is on-on-on for inclusion and off-off-off for exclusion.

## Central Scene configuration

To provide Central Scene support you need to **shutdown Home Assistant** and modify your `zwcfg_*.xml` file according to the following guides.

### Inovelli Scene Capable On/Off and Dimmer Wall Switches

For Inovelli switches, you'll need to update (or possibly add) the `COMMAND_CLASS_CENTRAL_SCENE` for each node in your `zwcfg` file with the following:

```xml
			<CommandClass id="91" name="COMMAND_CLASS_CENTRAL_SCENE" version="1" request_flags="4" innif="true" scenecount="0">
				<Instance index="1" />
				<Value type="int" genre="system" instance="1" index="0" label="Scene Count" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="2" />
				<Value type="int" genre="user" instance="1" index="1" label="Bottom Button Scene" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="3" />
				<Value type="int" genre="user" instance="1" index="2" label="Top Button Scene" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="3" />
			</CommandClass>
```

Once this is complete, you should see the follow `zwave.scene_activated` events:

**Action**|**scene\_id**|**scene\_data**
:-----:|:-----:|:-----:
Double tap off|1|3
Double tap on|2|3
Triple tap off|1|4
Triple tap on|2|4
4x tap off|1|5
4x tap on|2|5
5x tap off|1|6
5x tap on|2|6

### Zooz Scene Capable On/Off and Dimmer Wall Switches (Zen26 & Zen27 - Firmware 2.0+)

Many Zooz Zen26/27 switches that have been sold do not have firmware 2.0+. Contact Zooz to obtain the over the air firmware update instructions and new user manual for the switches.

Once the firmware is updated, the the new configuration parameters will have to be added to the `zwcfg` file. Replace the existing `COMMAND_CLASS_CONFIGURATION` with the one of the following options (depending on your model of switch):

Zen26 (On/Off Switch):
```xml
<CommandClass id="112" name="COMMAND_CLASS_CONFIGURATION" version="1" request_flags="4" innif="true">
	<Instance index="1" />
	<Value type="list" genre="config" instance="1" index="1" label="Paddle Control" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="2" vindex="0" size="1">
		<Help>Normal mode: Upper paddle turns the light on, lower paddle turns the light off. Reverse will reverse those functions. Any will toggle the light regardless of which button is pushed.</Help>
		<Item label="Normal" value="0" />
		<Item label="Reverse" value="1" />
		<Item label="Any" value="2" />
	</Value>
	<Value type="list" genre="config" instance="1" index="2" label="LED Indication Control" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="3" vindex="0" size="1">
		<Help>LED Indication light function. Normal has the LED Indication on when the switch is off, off when the switch is on.</Help>
		<Item label="Normal" value="0" />
		<Item label="Reverse" value="1" />
		<Item label="Always Off" value="2" />
		<Item label="Always On" value="3" />
	</Value>
	<Value type="list" genre="config" instance="1" index="3" label="Enable Turn-Off Timer" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="1" vindex="0" size="1">
		<Help>Enable or disable the auto turn-off timer function.</Help>
		<Item label="Disabled (Default)" value="0" />
		<Item label="Enabled" value="1" />
	</Value>
	<Value type="int" genre="config" instance="1" index="4" label="Turn-Off Timer Duration" units="minutes" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="1" max="65535" value="1">
		<Help>Time, in seconds, for auto-off timer delay. 60 (default).</Help>
	</Value>
	<Value type="list" genre="config" instance="1" index="5" label="Enable Turn-On Timer" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="1" vindex="0" size="1">
		<Help>Enable or disable the auto turn-on timer function.</Help>
		<Item label="Disabled (Default)" value="0" />
		<Item label="Enabled" value="1" />
	</Value>
	<Value type="int" genre="config" instance="1" index="6" label="Turn-On Timer Duration" units="minutes" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="1" max="65535" value="55">
		<Help>Time, in minutes, for auto-on timer delay. 60 (default).</Help>
	</Value>
	<Value type="list" genre="config" instance="1" index="8" label="On Off Status After Power Failure" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="2" vindex="2" size="1">
		<Help>Status after power on after power failure. OFF will always turn light off. ON will always turn light on. Restore will remember the latest state and restore that state.</Help>
		<Item label="OFF" value="0" />
		<Item label="ON" value="1" />
		<Item label="Restore" value="2" />
	</Value>
	<Value type="list" genre="config" instance="1" index="10" label="Scene Control" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="1" vindex="0" size="1">
		<Help>Enable or disable scene control functionality for quick double tap triggers.</Help>
		<Item label="Disabled (Default)" value="0" />
		<Item label="Enabled" value="1" />
	</Value>
	<Value type="list" genre="config" instance="1" index="11" label="Enable/Disable Paddle Control" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="1" vindex="1" size="1">
		<Help>Enable or disable local on/off control. If enabled, you&apos;ll only be able to control the connected light via Z-Wave.</Help>
		<Item label="Disabled" value="0" />
		<Item label="Enabled (Default)" value="1" />
	</Value>
</CommandClass>
```

Zen27 (Dimmer):
```xml
<CommandClass id="112" name="COMMAND_CLASS_CONFIGURATION" version="1" request_flags="4" innif="true">
	<Instance index="1" />
	<Value type="list" genre="config" instance="1" index="1" label="Paddle Control" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="2" vindex="0" size="1">
		<Help>Normal mode: Upper paddle turns the light on, lower paddle turns the light off. Reverse will reverse those functions. Any will toggle the light regardless of which button is pushed.</Help>
		<Item label="Normal" value="0" />
		<Item label="Reverse" value="1" />
		<Item label="Any" value="2" />
	</Value>
	<Value type="list" genre="config" instance="1" index="2" label="LED Indication Control" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="3" vindex="0" size="1">
		<Help>LED Indication light function. Normal has the LED Indication on when the switch is off, off when the switch is on.</Help>
		<Item label="Normal" value="0" />
		<Item label="Reverse" value="1" />
		<Item label="Always Off" value="2" />
		<Item label="Always On" value="3" />
	</Value>
	<Value type="list" genre="config" instance="1" index="3" label="Enable Auto Turn-Off Timer" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="1" vindex="0" size="1">
		<Item label="Disabled" value="0" />
		<Item label="Enabled" value="1" />
	</Value>
	<Value type="int" genre="config" instance="1" index="4" label="Auto Turn-Off Timer Duration" units="minutes" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="1" max="65535" value="60">
		<Help>Time, in minutes, for auto-off timer delay.</Help>
	</Value>
	<Value type="list" genre="config" instance="1" index="5" label="Enable Auto Turn-On Timer" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="1" vindex="0" size="1">
		<Item label="Disabled" value="0" />
		<Item label="Enabled" value="1" />
	</Value>
	<Value type="int" genre="config" instance="1" index="6" label="Auto Turn-On Timer Duration" units="minutes" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="1" max="65535" value="60">
		<Help>Time, in minutes, for auto-off timer delay.</Help>
	</Value>
	<Value type="list" genre="config" instance="1" index="8" label="On Off Status After Power Failure" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="2" vindex="2" size="1">
		<Help>Status after power on after power failure. OFF will always turn light off. ON will always turn light on. Restore will remember the latest state and restore that state.</Help>
		<Item label="OFF" value="0" />
		<Item label="ON" value="1" />
		<Item label="Restore" value="2" />
	</Value>
	<Value type="byte" genre="config" instance="1" index="9" label="Ramp Rate Control" units="seconds" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="1" max="99" value="3">
		<Help>Adjust the ramp rate for your dimmer (fade-in / fade-out effect for on / off operation). Values correspond to the number of seconds it take for the dimmer to reach full brightness or turn off when operated manually.</Help>
	</Value>
	<Value type="byte" genre="config" instance="1" index="10" label="Minimum Brightness" units="%" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="1" max="99" value="99">
		<Help>Set the minimum brightness level for your dimmer. You won&apos;t be able to dim the light below the set value.</Help>
	</Value>
	<Value type="byte" genre="config" instance="1" index="11" label="Maximum Brightness" units="%" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="1" max="99" value="99">
		<Help>Set the maximum brightness level for your dimmer. You won&apos;t be able to add brightness to the light beyond the set value. Note: if Parameter 12 is set to value &quot;Full&quot;, Parameter 11 is automatically disabled.</Help>
	</Value>
	<Value type="list" genre="config" instance="1" index="12" label="Double Tap Function Brightness" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="1" vindex="0" size="1">
		<Help>Double Tap action. When set to Full, turns light on to 100%. If set to Maximum Level, turns light on to % set in Parameter 11.</Help>
		<Item label="Full" value="0" />
		<Item label="Maximum Level" value="1" />
	</Value>				
	<Value type="list" genre="config" instance="1" index="13" label="Scene Control" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="1" vindex="0" size="1">
		<Help>Enable or disable scene control functionality for quick double tap triggers.</Help>
		<Item label="Disabled" value="0" />
		<Item label="Enabled" value="1" />
	</Value>
	<Value type="list" genre="config" instance="1" index="14" label="Enable Double Tap Function" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="2" vindex="0" size="1">
		<Help>Enable the double tap or disable the double tap function and assign brightness level to single tap.</Help>
		<Item label="Enabled" value="0" />
		<Item label="Disabled - Single Tap To Last Brightness" value="1" />
		<Item label="Disabled - Single Tap To Max Brightness" value="2" />
	</Value>
	<Value type="list" genre="config" instance="1" index="15" label="Enable/Disable Paddle Control" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="0" max="1" vindex="1" size="1">
		<Help>Enable or disable local on/off control. If enabled, light will only be able to be controlled via Z-Wave.</Help>
		<Item label="Disabled" value="0" />
		<Item label="Enabled" value="1" />
	</Value>
</CommandClass>
```

For Zooz switches, you'll need to update (or possibly add) the `COMMAND_CLASS_CENTRAL_SCENE` for each node in your `zwcfg` file with the following:
```xml
<CommandClass id="91" name="COMMAND_CLASS_CENTRAL_SCENE" version="1" request_flags="4" innif="true" scenecount="0">
	<Instance index="1" />
	<Value type="int" genre="system" instance="1" index="0" label="Scene Count" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="2" />
	<Value type="int" genre="user" instance="1" index="1" label="Bottom Button Scene" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="3" />
	<Value type="int" genre="user" instance="1" index="2" label="Top Button Scene" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="3" />
</CommandClass>
```

Go to the Z-Wave Network Management section in the Home Assistant Configuration, select the node which has just been updated and enable the scene support configuration parameter.

Once this is complete, you should see the following `zwave.scene_activated` events:

**Action**|**scene\_id**|**scene\_data**
:-----:|:-----:|:-----:
Single tap off|1|7680
Single tap on|2|7680
Double tap off|1|7860
Double tap on|2|7860
Triple tap off|1|7920
Triple tap on|2|7920
4x tap off|1|7980
4x tap on|2|7980
5x tap off|1|8040
5x tap on|2|8040

### HomeSeer Switches

For the HomeSeer devices specifically, you may need to update the `COMMAND_CLASS_CENTRAL_SCENE` for each node in your `zwcfg` file with the following:

```xml
<CommandClass id="91" name="COMMAND_CLASS_CENTRAL_SCENE" version="1" request_flags="4" innif="true" scenecount="0">
  <Instance index="1" />
  <Value type="int" genre="system" instance="1" index="0" label="Scene Count" units="" read_only="true" write_only="false"   verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="2" />
  <Value type="int" genre="user" instance="1" index="1" label="Top Button Scene" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
  <Value type="int" genre="user" instance="1" index="2" label="Bottom Button Scene" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
</CommandClass>
```

Below is a table of the action/scenes for the HomeSeer devices (as a reference for other similar devices):

**Action**|**scene\_id**|**scene\_data**
:-----:|:-----:|:-----:
Single tap on|1|0
Single tap off|2|0
Double tap on|1|3
Double tap off|2|3
Triple tap on|1|4
Triple tap off|2|4
Tap and hold on|1|2
Tap and hold off|2|2

Some installations will see those details:

**Top button ID: 1, Bottom ID: 2**

**Action**|**scene\_data**
:-----:|:-----:
Single Press|7800
Hold Button|7740
2x Tap|7860
3x Tap|7920
4x Tap|7980
5x Tap|8040


### Fibaro Button FGPB-101-6 v3.2

<!-- from https://hastebin.com/esodiweduq.cs -->

For the Button, you may need to update the `COMMAND_CLASS_CENTRAL_SCENE` for each node in your `zwcfg` file with the following:

```xml
      <CommandClass id="91" name="COMMAND_CLASS_CENTRAL_SCENE" version="1" request_flags="4" innif="true" scenecount="0">
        <Instance index="1" />
          <Value type="int" genre="system" instance="1" index="0" label="Scene Count" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
          <Value type="int" genre="system" instance="1" index="1" label="Scene Count" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="3" />
      </CommandClass>
```

Below is a table of the action/scenes for the Button (as a reference for other similar devices):

**Action**|**scene\_id**|**scene\_data**
:-----:|:-----:|:-----:
Single tap on|1|0
Double tap on|1|3
Triple tap on|1|4

Tap and hold wakes up the Button.

### Fibaro Keyfob FGKF-601


For the Fibaro Keyfob, you may need to update the `COMMAND_CLASS_CENTRAL_SCENE` for each node in your `zwcfg` file with the following:

```xml
      <CommandClass id="91" name="COMMAND_CLASS_CENTRAL_SCENE" version="1" request_flags="4" innif="true" scenecount="6">
	<Instance index="1" />
	<Value type="int" genre="system" instance="1" index="0" label="Scene Count" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="6" />
	<Value type="int" genre="user" instance="1" index="1" label="Square" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
	<Value type="int" genre="user" instance="1" index="2" label="Circle" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
	<Value type="int" genre="user" instance="1" index="3" label="X" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
	<Value type="int" genre="user" instance="1" index="4" label="Triangle" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
	<Value type="int" genre="user" instance="1" index="5" label="Minus" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
	<Value type="int" genre="user" instance="1" index="6" label="Plus" units="" read_only="false" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
</CommandClass>
```

Below is a table of the action/scenes for the Keyfob (as a reference for other similar devices):

**Action**|**scene\_id**|**scene\_data**
:-----:|:-----:|:-----:
Button one (Square) single tap|1|7680
Button one (Square) hold|1|7800
Button one (Square) release|1|7740
Button two (Circle) single tap|2|7680
Button two (Circle) hold|2|7800
Button two (Circle) release|2|7740
Button three (X) single tap|3|7680
Button three (X) hold|3|7800
Button three (X) release|3|7740
Button four (Triangle) single tap|4|7680
Button four (Triangle) hold|4|7800
Button four (Triangle) release|4|7740
Button five (Triangle) single tap|5|7680
Button five (Triangle) hold|5|7800
Button five (Triangle) release|5|7740
Button six (Triangle) single tap|6|7680
Button six (Triangle) hold|6|7800
Button six (Triangle) release|6|7740

Press circle and plus simultaneously to wake up the device.

### Aeotec NanoMote Quad

<!-- from https://products.z-wavealliance.org/products/2817 -->

Once you've added the NanoMote to your z-wave network, you'll need to update your zwcfg_\*.xml file with the below xml data. Stop Home Assistant and open your zwcfg_\*.xml file (located in your config folder). Find the NanoMote device section and then its corresponding `CommandClass` section with id="91". Replace the entire CommandClass section with the below xml data. Save the file and restart Home Assistant.  

```xml
    <CommandClass id="91" name="COMMAND_CLASS_CENTRAL_SCENE" version="1" request_flags="4" innif="true" scenecount="0">
        <Instance index="1" />
        <Value type="int" genre="system" instance="1" index="0" label="Scene Count" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
        <Value type="int" genre="system" instance="1" index="1" label="Button One" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
        <Value type="int" genre="system" instance="1" index="2" label="Button Two" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
        <Value type="int" genre="system" instance="1" index="3" label="Button Three" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
        <Value type="int" genre="system" instance="1" index="4" label="Button Four" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
    </CommandClass>
```

Below is a table of the action/scenes for the NanoMote Quad:

**Action**|**scene\_id**|**scene\_data**
:-----:|:-----:|:-----:
Button one single tap|1|7680
Button one hold|1|7800
Button one release|1|7740
Button two single tap|2|7680
Button two hold|2|7800
Button two release|2|7740
Button three single tap|3|7680
Button three hold|3|7800
Button three release|3|7740
Button four single tap|4|7680
Button four hold|4|7800
Button four release|4|7740

Example Event:

```yaml
    "event_type": "zwave.scene_activated",
    "data": {
        "entity_id": "zwave.nanomote",
        "scene_id": 2,
        "scene_data": 7680
    }
```

### Aeotec Wallmote

<!-- from https://hastebin.com/esodiweduq.cs -->

For the Aeotec Wallmote, you may need to update the `COMMAND_CLASS_CENTRAL_SCENE` for each node in your `zwcfg` file with the following:

```xml
      <CommandClass id="91" name="COMMAND_CLASS_CENTRAL_SCENE" version="1" request_flags="5" innif="true" scenecount="0">
        <Instance index="1" />
          <Value type="int" genre="system" instance="1" index="0" label="Scene Count" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
          <Value type="int" genre="system" instance="1" index="1" label="Button One" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
          <Value type="int" genre="system" instance="1" index="2" label="Button Two" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
          <Value type="int" genre="system" instance="1" index="3" label="Button Three" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
          <Value type="int" genre="system" instance="1" index="4" label="Button Four" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
          <Value type="int" genre="system" instance="1" index="5" label="Other" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
      </CommandClass>
```

Below is a table of the action/scenes for the Wallmote (as a reference for other similar devices):

**Action**|**scene\_id**|**scene\_data**
:-----:|:-----:|:-----:
Button one single tap|1|0
Button one hold|1|2
Button one release|1|1
Button two single tap|2|0
Button two hold|2|2
Button two release|2|1
Button three single tap|3|0
Button three hold|3|2
Button three release|3|1
Button four single tap|4|0
Button four hold|4|2
Button four release|4|1

### WallC-S Switch

Use the same configuration as for the Aeotec Wallmote.

### HANK One-key Scene Controller HKZN-SCN01/HKZW-SCN01

For the HANK One-key Scene Controller, you may need to update the `COMMAND_CLASS_CENTRAL_SCENE` for each node in your `zwcfg` file with the following:

```xml
      <CommandClass id="91" name="COMMAND_CLASS_CENTRAL_SCENE" version="1" request_flags="1" innif="true" scenecount="0">
        <Instance index="1" />
        <Value type="int" genre="system" instance="1" index="0" label="Scene Count" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
        <Value type="int" genre="system" instance="1" index="1" label="Button One" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
      </CommandClass>
```

Below is a table of the action/scenes for the Button (as a reference for other similar devices):

**Action**|**scene\_id**|**scene\_data**
:-----:|:-----:|:-----:
Button single tap|1|0
Button hold|1|2
Button release|1|1

### HANK Four-key Scene Controller HKZN-SCN04

For the HANK Four-key Scene Controller, you may need to update the `COMMAND_CLASS_CENTRAL_SCENE` for each node in your `zwcfg` file with the following:

```xml
      <CommandClass id="91" name="COMMAND_CLASS_CENTRAL_SCENE" version="1" request_flags="5" innif="true" scenecount="0">
        <Instance index="1" />
        <Value type="int" genre="system" instance="1" index="0" label="Scene Count" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
        <Value type="int" genre="system" instance="1" index="1" label="Button One" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
        <Value type="int" genre="system" instance="1" index="2" label="Button Two" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="1" />
        <Value type="int" genre="system" instance="1" index="3" label="Button Three" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="1" />
        <Value type="int" genre="system" instance="1" index="4" label="Button Four" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="1" />
        <Value type="int" genre="system" instance="1" index="5" label="Other" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
      </CommandClass>
```

Below is a table of the action/scenes for the Buttons and associated Pictogram:

**Action**|**Pictogram**|**scene\_id**|**scene\_data**
:-----:|:-----:|:-----:|:-----:
Button one tap|Moon and Star|1|0
Button one hold|Moon and Star|1|2
Button one release|Moon and Star|1|1
Button two tap|People|2|0
Button two hold|People|2|2
Button two release|People|2|1
Button three tap|Circle|3|0
Button three hold|Circle|3|2
Button three release|Circle|3|1
Button four tap|Circle with Line|4|0
Button four hold|Circle with Line|4|2
Button four release|Circle with Line|4|1

### Remotec ZRC-90 Scene Master

To get the ZRC-90 Scene Master working in Home Assistant, you must first edit the `COMMAND_CLASS_CENTRAL_SCENE` in your `zwcfg` file.

1. Go the Z-Wave control panel in Home Assistant and make a note of the node number your ZRC-90 has been assigned.
2. *Stop* Home Assistant.
3. Make a backup of your `zwfcg` file, just in case.
4. In the `zwcfg` file, find the `Node id` that corresponds to the number you noted in the first step.
5. Within the `Node id` you identified, highlight everything between `<CommandClass id="91"` and `</CommandClass>` (inclusive) and paste in the following:

    ```xml
    <CommandClass id="91" name="COMMAND_CLASS_CENTRAL_SCENE" version="1" request_flags="5" innif="true" scenecount="0">
      <Instance index="1" />
      <Value type="int" genre="system" instance="1" index="0" label="Scene Count" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
      <Value type="int" genre="system" instance="1" index="1" label="Scene 1" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="3" />
      <Value type="int" genre="system" instance="1" index="2" label="Scene 2" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
      <Value type="int" genre="system" instance="1" index="3" label="Scene 3" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
      <Value type="int" genre="system" instance="1" index="4" label="Scene 4" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="1" />
      <Value type="int" genre="system" instance="1" index="5" label="Scene 5" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
      <Value type="int" genre="system" instance="1" index="6" label="Scene 6" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
      <Value type="int" genre="system" instance="1" index="7" label="Scene 7" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
      <Value type="int" genre="system" instance="1" index="8" label="Scene 8" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
      <Value type="int" genre="system" instance="1" index="9" label="Other" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
    </CommandClass>
    ```

6. Save the changes you made the `zwcfg` file and start Home Assistant back up.

Button presses will trigger `zwave.scene_activated` with the following:

- `node_id`: the node of your Scene Master (useful if you have more than one)
- `scene_id`: the number button you press (1-8)
- `scene_data`: the type of press registered (see below)

The Scene Master has eight buttons which can send four actions.
The type of action is reflected in the `scene_data` parameter:

**Action**|**scene\_data**
:-----:|:-----:
Single press | 0
Long press (2s) | 1
Release from hold | 2
Double-press | 3

Let's see how this works in an automation for a Scene Master that's assigned as Node 7:

```yaml
- id: '1234567890'
  alias: Double-press Button 2 to toggle all lights
  trigger:
  - platform: event
    event_type: zwave.scene_activated
    event_data:
      node_id: 7
      scene_id: 2
      scene_data: 3  
  condition: []
  action:
  - data:
    service: light.toggle
      entity_id: group.all_lights
```

### RFWDC Cooper 5-button Scene Control Keypad

For the RFWDC Cooper 5-button Scene Control Keypad, you may need to update the `COMMAND_CLASS_CENTRAL_SCENE` for each node in your `zwcfg` file with the following:

```xml
<CommandClass id="91" name="COMMAND_CLASS_CENTRAL_SCENE" version="1" request_flags="5" innif="true" scenecount="0">
  <Instance index="1" />
  <Value type="int" genre="system" instance="1" index="0" label="Scene Count" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
  <Value type="int" genre="system" instance="1" index="1" label="Button One" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
  <Value type="int" genre="system" instance="1" index="2" label="Button Two" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
  <Value type="int" genre="system" instance="1" index="3" label="Button Three" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
  <Value type="int" genre="system" instance="1" index="4" label="Button Four" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
  <Value type="int" genre="system" instance="1" index="5" label="Button Five" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
</CommandClass>
```

Below is a table of the action/scenes for the Buttons:

**Action**|**scene\_id**
:-----:|:-----:
Button one tap|1
Button two tap|2
Button three tap|3
Button four tap|4
Button five tap|5

When a button turns off, the controller sends `basic_set` in a generic `node_event` and does not specify which button was pressed. The status of the buttons is encoded into the `indicator` value, so in order to determine the status of each button, you need to refresh the indicator value. You can also control the LEDs for each button by setting the indicator value. For responsiveness, automations should be triggered with `zwave.scene_activated` events rather than the switch status.

Here is an example configuration needed for the scene controller:

{% raw %}
```yaml
automation:
  - alias: Sync the indicator value on button events
    trigger:
      - platform: event
        event_type: zwave.scene_activated
        event_data:
          entity_id: zwave.scene_contrl
      - platform: event
        event_type: zwave.node_event
        event_data:
          entity_id: zwave.scene_contrl
    action:
      - service: zwave.refresh_node_value
        data_template: 
          node_id: 3
          value_id: "{{ state_attr('sensor.scene_contrl_indicator','value_id') }}"
switch:
  - platform: template
    switches:
      button_1_led:
        value_template: "{{ states('sensor.scene_contrl_indicator')|int|bitwise_and(1) > 0 }}"
        turn_on:
          service: zwave.set_node_value
          data_template:
            node_id: 3
            value_id: "{{ state_attr('sensor.scene_contrl_indicator','value_id') }}"
            value: "{{ states('sensor.scene_contrl_indicator')|int + 1 }}"
        turn_off:
          service: zwave.set_node_value
          data_template:
            node_id: 3
            value_id: "{{ state_attr('sensor.scene_contrl_indicator','value_id') }}"
            value: "{{ states('sensor.scene_contrl_indicator')|int - 1 }}"
      button_2_led:
        value_template: "{{ states('sensor.scene_contrl_indicator')|int|bitwise_and(2) > 0 }}"
        turn_on:
          service: zwave.set_node_value
          data_template:
            node_id: 3
            value_id: "{{ state_attr('sensor.scene_contrl_indicator','value_id') }}"
            value: "{{ states('sensor.scene_contrl_indicator')|int + 2 }}"
        turn_off:
          service: zwave.set_node_value
          data_template:
            node_id: 3
            value_id: "{{ state_attr('sensor.scene_contrl_indicator','value_id') }}"
            value: "{{ states('sensor.scene_contrl_indicator')|int - 2 }}"
      button_3_led:
        value_template: "{{ states('sensor.scene_contrl_indicator')|int|bitwise_and(4) > 0 }}"
        turn_on:
          service: zwave.set_node_value
          data_template:
            node_id: 3
            value_id: "{{ state_attr('sensor.scene_contrl_indicator','value_id') }}"
            value: "{{ states('sensor.scene_contrl_indicator')|int + 4 }}"
        turn_off:
          service: zwave.set_node_value
          data_template:
            node_id: 3
            value_id: "{{ state_attr('sensor.scene_contrl_indicator','value_id') }}"
            value: "{{ states('sensor.scene_contrl_indicator')|int - 4 }}"
      button_4_led:
        value_template: "{{ states('sensor.scene_contrl_indicator')|int|bitwise_and(8) > 0 }}"
        turn_on:
          service: zwave.set_node_value
          data_template:
            node_id: 3
            value_id: "{{ state_attr('sensor.scene_contrl_indicator','value_id') }}"
            value: "{{ states(scene_contrl_indicator)|int + 8 }}"
        turn_off:
          service: zwave.set_node_value
          data_template:
            node_id: 3
            value_id: "{{ state_attr('sensor.scene_contrl_indicator','value_id') }}"
            value: "{{ states('sensor.scene_contrl_indicator')|int - 8 }}"
      button_5_led:
        value_template: "{{ states('sensor.scene_contrl_indicator')|int|bitwise_and(16) > 0 }}"
        turn_on:
          service: zwave.set_node_value
          data_template:
            node_id: 3
            value_id: "{{ state_attr('sensor.scene_contrl_indicator','value_id') }}"
            value: "{{ states('sensor.scene_contrl_indicator')|int + 16 }}"
        turn_off:
          service: zwave.set_node_value
          data_template:
            node_id: 3
            value_id: "{{ state_attr('sensor.scene_contrl_indicator','value_id') }}"
            value: "{{ states('sensor.scene_contrl_indicator')|int - 16 }}"
```

### HeatIt/ThermoFloor Z-Push Button 2/8 Wall Switch

To get the Z-Push Button 2 or the Z-Push Button 8 working in Home Assistant, you must first edit the `COMMAND_CLASS_CENTRAL_SCENE` in your `zwcfg` file.

1. Go the Z-Wave control panel in Home Assistant and make a note of the node number your wall switch has been assigned.
2. *Stop* Home Assistant.
3. Make a backup of your `zwfcg` file, just in case.
4. In the `zwcfg` file, find the `Node id` that corresponds to the number you noted in the first step.
5. Within the `Node id` you identified, highlight everything between `<CommandClass id="91"` and `</CommandClass>` (inclusive) and paste in the following:
    - 5.1 For the Z-Push Button 2:

    ```xml
        <CommandClass id="91" name="COMMAND_CLASS_CENTRAL_SCENE" version="1" request_flags="4" innif="true" scenecount="0">				<Instance index="1" />
	    <Value type="int" genre="system" instance="1" index="0" label="Scene Count" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
	    <Value type="int" genre="user" instance="1" index="1" label="Button 1" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
	    <Value type="int" genre="user" instance="1" index="2" label="Button 2" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
        </CommandClass>
    ```

    - 5.2 For the Z-Push Button 8:

    ```xml
        <CommandClass id="91" name="COMMAND_CLASS_CENTRAL_SCENE" version="1" request_flags="4" innif="true" scenecount="0">				<Instance index="1" />
	    <Value type="int" genre="system" instance="1" index="0" label="Scene Count" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
	    <Value type="int" genre="user" instance="1" index="1" label="Button 1" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
	    <Value type="int" genre="user" instance="1" index="2" label="Button 2" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
	    <Value type="int" genre="user" instance="1" index="3" label="Button 3" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
	    <Value type="int" genre="user" instance="1" index="4" label="Button 4" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
	    <Value type="int" genre="user" instance="1" index="5" label="Button 5" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
	    <Value type="int" genre="user" instance="1" index="6" label="Button 6" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
	    <Value type="int" genre="user" instance="1" index="7" label="Button 7" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
	    <Value type="int" genre="user" instance="1" index="8" label="Button 8" units="" read_only="true" write_only="false" verify_changes="false" poll_intensity="0" min="-2147483648" max="2147483647" value="0" />
        </CommandClass>
    ```

6. Save the changes you made the `zwcfg` file and start Home Assistant back up.

Button presses will trigger `zwave.scene_activated` with the following:

- `scene_id`: the number of the button you press from top left (1) to bottom right (8)

{% endraw %}
