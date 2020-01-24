---
title: FFmpeg Motion
description: Instructions on how to integrate an FFmpeg-based motion binary sensor
logo: ffmpeg.png
ha_category:
  - Image Processing
ha_release: 0.27
---

The `ffmpeg` platform allows you to use any video feed with [FFmpeg](https://www.ffmpeg.org/) for motion sensors in Home Assistant.

<div class='note'>

If the `ffmpeg` process is broken, the sensor will be unavailable. To control the ffmpeg process of sensor, use the service *ffmpeg.start*, *ffmpeg.stop*, *ffmpeg.restart*.

</div>

## Motion

FFmpeg doesn't have a motion detection filter, but can use a scene filter to detect a new scene/motion. You can set how much needs to change in order to detect motion with the option 'changes', the percent value of change between frames. If you want a really small value for 'changes', you can also add a denoise filter.

## Configuration

To add FFmpeg with motion detection to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
binary_sensor:
  - platform: ffmpeg_motion
    input: FFMPEG_SUPPORTED_INPUT
```

{% configuration %}
input:
  description: An FFmpeg-compatible input file, stream, or feed.
  required: true
  type: string
name:
  description: Override the name of your camera for the frontend.
  required: false
  type: string
initial_state:
  description: Start `ffmpeg` with Home Assistant.
  required: false
  type: boolean
  default: true
changes:
  description: How much needs to change between two frames to detect it as motion, value in percentage (a lower value is more sensitive).
  required: false
  type: integer
  default: 10
reset:
  description: The time to reset the state after no new motion is detected.
  required: false
  type: integer
  default: 20
repeat:
  description: How many events need to be detected in *repeat_time* in order to trigger a motion, 0 repeats means deactivated.
  required: false
  type: integer
  default: 0
repeat_time:
  description: The span of time *repeat* events need to occur in before triggering a motion, 0 seconds means deactivated.
  required: false
  type: integer
  default: 0
extra_arguments:
  description: Extra options to pass to `ffmpeg`, e.g., video denoise filtering.
  required: false
  type: string
{% endconfiguration %}

To experiment with values (changes/100 is the scene value in `ffmpeg`):

```bash
$ ffmpeg -i YOUR_INPUT -an -filter:v select=gt(scene\,0.1) -f framemd5 -
```

If you are running into trouble with this sensor, please refer to the [troubleshooting section](/integrations/ffmpeg/#troubleshooting).

#### Tips

- Use motion only in a custom area with [crop filter](https://ffmpeg.org/ffmpeg-filters.html#crop):

```yaml
extra_arguments: -filter:v "crop=100:100:12:34"
```
