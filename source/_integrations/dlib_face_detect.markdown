---
title: Dlib Face Detect
description: Instructions on how to integrate Dlib Face Detect into Home Assistant.
logo: dlib.png
ha_category:
  - Image Processing
ha_release: 0.44
---

The `dlib_face_detect` image processing platform allows you to use the [Dlib](http://www.dlib.net/) through Home Assistant. This platform enables face detection from cameras, and can fire events with attributes.

This can be used to trigger an automation rule. Further info is on the [integration](/integrations/image_processing/) page.

### Configuration Home Assistant

```yaml
# Example configuration.yaml entry
image_processing:
  - platform: dlib_face_detect
    source:
      - entity_id: camera.door
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
{% endconfiguration %}
