---
title: "Map Card"
sidebar_label: Map
description: "A card that allows you to display entities on a map"
---

A card that allows you to display entities on a map.

<p class='img'>
<img src='/images/lovelace/lovelace_map_card.png' alt='Screenshot of the map card'>
Screenshot of the map card.
</p>

{% configuration %}
type:
  required: true
  description: map
  type: string
entities:
  required: true
  description: List of entity IDs. Either this or the `geo_location_sources` configuration option is required.
  type: list
geo_location_sources:
  required: true
  description: List of geolocation sources. All current entities with that source will be displayed on the map. See [Geolocation](/integrations/geo_location/) platform for valid sources. Set to `all` to use all available sources. Either this or the `entities` configuration option is required.
  type: list
title:
  required: false
  description: The card title.
  type: string
aspect_ratio:
  required: false
  description: "Forces the height of the image to be a ratio of the width. You may enter a value such as: `16x9`, `16:9`, `1.78`."
  type: string
default_zoom:
  required: false
  description: The default zoom level of the map.
  type: integer
  default: 14 (or whatever zoom level is required to fit all visible markers)
dark_mode:
  required: false
  description: Enable a dark theme for the map.
  type: boolean
  default: false
{% endconfiguration %}

<div class='note'>
  Only entities that have latitude and longitude attributes will be displayed on the map.
</div>

<div class="note">

  The `default_zoom` value will be ignored if it is set higher than the current zoom level
  after fitting all visible entity markers in the map window. In other words, this can only
  be used to zoom the map _out_ by default.

</div>

## Examples

```yaml
type: map
aspect_ratio: 16:9
default_zoom: 8
entities:
  - device_tracker.demo_paulus
  - zone.home
```

```yaml
type: map
geo_location_sources:
  - nsw_rural_fire_service_feed
entities:
  - zone.home
```
