---
title: Blink
description: Instructions for how to integrate Blink camera/security system within Home Assistant.
logo: blink.png
ha_category:
  - Hub
  - Alarm
  - Binary Sensor
  - Camera
  - Sensor
ha_release: '0.40'
ha_iot_class: Cloud Polling
ha_codeowners:
  - '@fronzbot'
---

The `blink` integration lets you view camera images and motion events from [Blink](https://blinkforhome.com/) camera and security systems.

## Setup

You will need your Blink login information (username, which is usually your email address, and password) to use this module.

## Configuration

To enable devices linked in your [Blink](https://blinkforhome.com) account, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
blink:
  username: YOUR_USERNAME
  password: YOUR_PASSWORD
```

{% configuration %}
username:
  description: The username for accessing your Blink account.
  required: true
  type: string
password:
  description: The password for accessing your Blink account.
  required: true
  type: string
scan_interval:
  description: How frequently to query for new data. Defaults to 300 seconds (5 minutes).
  required: false
  type: integer
binary_sensors:
  description: Binary sensor configuration options.
  required: false
  type: map
  keys:
   monitored_conditions:
     description: The conditions to create sensors from.
     required: false
     type: list
     default: all (`motion_enabled`, `motion_detected`)
sensors:
  description: Sensor configuration options.
  required: false
  type: map
  keys:
    monitored_conditions:
      description: The conditions to create sensors from.
      required: false
      type: list
      default: all (`battery`, `temperature`, `wifi_strength`)
offset:
  description: How far back in time (minutes) to look for motion. Motion is determined if a new video has been recorded between now and the last time you refreshed plus this offset.
  required: false
  type: integer
  default: 1
mode:
  description: Set to 'legacy' to enable use of old API endpoint subdomains (APIs can differ based on region, so use this if you are having issues with the integration).
  required: false
  type: string
  default: not set
{% endconfiguration %}

Once Home Assistant starts, the `blink` integration will create the following platforms:

- An `alarm_control_panel` to arm/disarm the whole blink system (note, `alarm_arm_home` is not implemented and will not actually do anything, despite it being an option in the GUI).
- A `camera` for each camera linked to your Blink sync module.
- A `sensor` per camera for every item listed in `monitored_conditions` (if no items specified in your `configuration.yaml`, all of them will be added by default).
- A `binary_sensor` for each item listed in `monitored_conditions` (if no items specified in your `configuration.yaml`, all of them will be added by default).

Since the cameras are battery operated, setting the `scan_interval` must be done with care so as to not drain the battery too quickly, or hammer Blink's servers with too many API requests.  The cameras can be manually updated via the `trigger_camera` service which will ignore the throttling caused by `scan_interval`.  As a note, all of the camera-specific sensors are only polled when a new image is requested from the camera. This means that relying on any of these sensors to provide timely and accurate data is not recommended.

Please note that each camera reports two different states: one as `sensor.blink_<camera_name>_status` and the other as `binary_sensor.blink_<camera_name>_motion_enabled`. The `motion_enabled` property reports if the `camera` is ready to detect motion *regardless if the system is actually armed**.

Below is an example showing every possible entry:

```yaml
# Example configuration.yaml entry
blink:
  username: YOUR_USERNAME
  password: YOUR_PASSWORD
  scan_interval: 300
  binary_sensors:
    monitored_conditions:
      - motion_enabled
      - motion_detected
  sensors:
    monitored_conditions:
      - battery
      - temperature
      - wifi_strength
```

## Services

Any sequential calls to services relating to blink should have a minimum of a 5 second delay in between them to prevent the calls fro being throttled and ignored.

### `blink.blink_update`

Force a refresh of the Blink system.

### `blink.trigger_camera`

Trigger a camera to take a new still image.

| Service Data Attribute | Optional | Description                            |
| ---------------------- | -------- | -------------------------------------- |
| `name`                 | no       | Name of camera to take new image with. |

### `blink.save_video`

Save the last recorded video of a camera to a local file. Note that in most cases, Home Assistant will need to know that the directory is writable via the `whitelist_external_dirs` in your `configuration.yaml` file (see example below).

| Service Data Attribute | Optional | Description                              |
| ---------------------- | -------- | ---------------------------------------- |
| `name`                 | no       | Name of camera containing video to save. |
| `filename`             | no       | Location of save file.                   |


```yaml
homeassistant:
    ...
    whitelist_external_dirs:
        - '/tmp'
        - '/path/to/whitelist'
```

### Other Services

In addition to the services mentioned above, there are generic `camera` and `alarm_control_panel` services available for use as well. The `camera.enable_motion_detection` and `camera.disable_motion_detection` services allow for individual cameras to be enabled and disabled, respectively, within the Blink system. The `alarm_control_panel.alarm_arm_away` and `alarm_control_panel.alarm_disarm` services allow for the whole system to be armed and disarmed, respectively.


## Examples

The following are some examples showing how to correctly make service calls using Blink:

### Snap Picture and Save Locally

This example script shows how to take a picture with your camera, named `My Camera` in your Blink app (this is **not necessarily** the friendly name in home-assistant).  After snapping a picture, the image will then be saved to a local directory called `/tmp/my_image.jpg`.  Note that this example makes use of services found in the [camera integration](/integrations/camera#service-snapshot)

```yaml
alias: Blink Snap Picture
sequence:
    - service: blink.trigger_camera
      data:
          name: "My Camera"
    - delay: 00:00:05  
    - service: blink.blink_update
    - service: camera.snapshot
      data:
          entity_id: camera.blink_my_camera
          filename: /tmp/my_image.jpg
```

### Arm Blink When Away

This example automation will arm your blink sync module to detect motion on any of your blink cameras that have motion detection enabled.  By default, Blink enables motion detection on all cameras so, unless you've changed anything in your app, you're all set.  If you want to manually enable motion detection for individual cameras, you can utilize the [appropriate camera service](/integrations/camera#service-enable_motion_detection) but please note that motion will only be captured if the sync module is armed.

Here, this example assumes your blink module is named `My Sync Module` and that you have [device trackers](/integrations/device_tracker) set up for presence detection.

```yaml
- id: arm_blink_when_away
  alias: Arm Blink When Away
  trigger:
      platform: state
      entity_id: all
      to: 'not_home'
  action:
      service: alarm_control_panel.alarm_arm_away
      entity_id: alarm_control_panel.blink_my_sync_module 
```

### Disarm Blink When Home

Similar to the previous example, this automation will disarm blink when arriving home.

```yaml
- id: disarm_blink_when_home
  alias: Disarm Blink When Home
  trigger:
      platform: state
      entity_id: all
      to: 'home'
  action:
      service: alarm_control_panel.alarm_disarm
      entity_id: alarm_control_panel.blink_my_sync_module 
```

### Save Video Locally When Motion Detected

When motion is detected, you can use the Blink Home-Assistant integration to save the last recorded video locally, rather than relying on Blink's servers to save your data.

Again, this example assumes your camera's name (in the blink app) is `My Camera` and your sync module name is `My Sync Module`.  The file will be saved to `/tmp/videos/blink_video_{YYYMMDD_HHmmSS}.mp4` where `{YYYYMMDD_HHmmSS}` will be a timestamp create via the use of [templating](/docs/configuration/templating/).

{% raw %}
```yaml
- id: save_blink_video_on_motion
  alias: Save Blink Video on Motion
  trigger:
      platform: state
      entity_id: binary_sensor.blink_my_camera_motion_detected
      to: 'on'
  action:
      service: blink.save_video
      data_template:
          name: "My Camera"
          filename: "/tmp/videos/blink_video_{{ now().strftime('%Y%m%d_%H%M%S') }}.mp4"
      
```
{% endraw %}
