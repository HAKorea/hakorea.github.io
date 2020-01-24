---
title: Dlib Face Identify
description: Instructions on how to integrate Dlib Face Identify into Home Assistant.
logo: dlib.png
ha_category:
  - Image Processing
ha_release: 0.44
---

The `dlib_face_identify` image processing platform allows you to use the [Dlib](http://www.dlib.net/) through Home Assistant. This platform allow you to identify persons on camera and fire an event with identify persons.

For using the result inside an automation rule, take a look at the [integration](/integrations/image_processing/) page.

### Configuration Home Assistant

```yaml
# Example configuration.yaml entry
image_processing:
 - platform: dlib_face_identify
   source:
    - entity_id: camera.door
   faces:
     Jon: /home/hass/jon.jpg
     Bob: /home/hass/bob.jpg
```

{% configuration %}
source:
  description: List of image sources.
  required: true
  type: list
  keys:
    entity_id:
      description: A camera entity id to get picture from.
      required: true
      type: string
    name:
      description: This parameter allows you to override the name of your `image_processing` entity.
      required: false
      type: string
faces:
  description: List of faces sources.
  required: true
  type: list
confidence:
  description: How much distance between faces to consider it a match. Using tolerance values lower than 0.6 will make the comparison more strict.
  required: false
  type: float
  default: 0.6
{% endconfiguration %}

<div class='note'>

If the platform fails to load because it could not install its requirement, install cmake: `sudo apt-get install cmake`.

</div>
