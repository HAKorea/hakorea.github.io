---
title: Keyboard Remote
description: Instructions on how to use a keyboard to remote control Home Assistant.
logo: keyboard.png
ha_category:
  - Other
ha_release: 0.29
ha_iot_class: Local Push
ha_codeowners:
  - '@bendavid'
---

Receive signals from a keyboard and use it as a remote control.

This integration allows you to use one or more keyboards as remote controls. It will fire `keyboard_remote_command_received` events which can then be used in automation rules.

The `evdev` package is used to interface with the keyboard and thus this is Linux only. It also means you can't use your normal keyboard for this because `evdev` will block it.

```yaml
# Example configuration.yaml entry
keyboard_remote:
  type: 'key_up'
```

{% configuration %}
type:
  description: Possible values are `key_up`, `key_down`, and `key_hold`. Be careful, `key_hold` will fire a lot of events.  This can be a list of types.
  required: true
  type: string
emulate_key_hold:
  description: Emulate key hold events when key is held down.  (Some input devices do not send these otherwise.)
  required: false
  type: boolean
  default: false
emulate_key_hold_delay:
  description:  Number of milliseconds to wait before sending first emulated key hold event
  required: false
  type: float
  default: 0.250
emulate_key_hold_repeat:
  description:  Number of milliseconds to wait before sending subsequent emulated key hold event
  required: false
  type: float
  default: 0.033
device_descriptor:
  description: Path to the local event input device file that corresponds to the keyboard.
  required: false
  type: string
device_name:
  description: Name of the keyboard device.
  required: false
  type: string
{% endconfiguration %}

Either `device_name` or `device_descriptor` must be present in the configuration entry. Indicating a device name is useful in case of repeating disconnections and re-connections of the device (for example, a bluetooth keyboard): the local input device file might change, thus breaking the configuration, while the name remains the same.
In case of presence of multiple devices of the same model, `device_descriptor` must be used.

A list of possible device descriptors and names is reported in the debug log at startup when the device indicated in the configuration entry could not be found.

A full configuration for two Keyboard Remotes could look like the one below:

```yaml
keyboard_remote:
- device_descriptor: '/dev/input/by-id/bluetooth-keyboard'
  type: 'key_down'
  emulate_key_hold: true
  emulate_key_hold_delay: 250
  emulate_key_hold_repeat: 33
- device_descriptor: '/dev/input/event0'
  type:
    - 'key_up'
    - 'key_down'
```

Or like the following for one keyboard:

```yaml
keyboard_remote:
  device_name: 'Bluetooth Keyboard'
  type: 'key_down'
```

And an automation rule to breathe life into it:

```yaml
automation:
  alias: Keyboard all lights on
  trigger:
    platform: event
    event_type: keyboard_remote_command_received
    event_data:
      device_descriptor: "/dev/input/event0"
      key_code: 107 # inspect log to obtain desired keycode
  action:
    service: light.turn_on
    entity_id: light.all
```

`device_descriptor` or `device_name` may be specificed in the trigger so the automation will be fired only for that keyboard. This is especially useful if you wish to use several bluetooth remotes to control different devices. Omit them to ensure the same key triggers the automation for all keyboards/remotes.

## Disconnections

This integration manages disconnections and re-connections of the keyboard, for example in the case of a Bluetooth device that turns off automatically to preserve battery.

If the keyboard disconnects, the integration will fire an event `keyboard_remote_disconnected`.
When the keyboard reconnects, an event `keyboard_remote_connected` will be fired.

Here's an automation example that plays a sound through a media player whenever the keyboard connects/disconnects:

```yaml
automation:
  - alias: Keyboard Connected
    trigger:
      platform: event
      event_type: keyboard_remote_connected
    action:
      - service: media_player.play_media
        data:
          entity_id: media_player.speaker
          media_content_id: keyboard_connected.wav
          media_content_type: music

  - alias: Bluetooth Keyboard Disconnected
    trigger:
      platform: event
      event_type: keyboard_remote_disconnected
      event_data:
        device_name: "00:58:56:4C:C0:91"
    action:
      - service: media_player.play_media
        data:
          entity_id: media_player.speaker
          media_content_id: keyboard_disconnected.wav
          media_content_type: music
```

## Permissions

There might be permissions problems with the event input device file. If this is the case, the user that Home Assistant runs as must be allowed read and write permissions with:

```bash
sudo setfacl -m u:HASS_USER:rw /dev/input/event*
```

Where `HASS_USER` is the user who runs Home Assistant.

If you want to make this permanent, you can use a udev rule that sets it for all event input devices. Add a file `/etc/udev/rules.d/99-userdev-input.rules` containing:

```bash
KERNEL=="event*", SUBSYSTEM=="input", RUN+="/usr/bin/setfacl -m u:HASS_USER:rw $env{DEVNAME}"
```

You can check ACLs permissions with:

```bash
getfacl /dev/input/event*
```
