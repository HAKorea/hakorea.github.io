---
title: GeoNet NZ Volcano
description: Instructions on how to integrate the GeoNet New Zealand Volcanic Alert Level feed into Home Assistant.
logo: geonet-nz.png
ha_category:
  - Sensor
ha_iot_class: Cloud Polling
ha_release: 0.103
ha_config_flow: true
ha_codeowners:
  - '@exxamalte'
---

The `geonetnz_volcano` integration lets you use a GeoJSON feed provided by 
New Zealand's [GeoNet](https://www.geonet.org.nz/) with information 
about volcanoes in the New Zealand region. 
It shows information of those volcanoes filtered by distance to Home Assistant's 
location.

Entities are generated and updated automatically with each update 
from the feed. Each entity defines the [volcanic alert level](https://www.geonet.org.nz/about/volcano/val) 
as its state.  Latitude and longitude of the volcano are included as attributes 
to show on the default map automatically.

The data is updated every 5 minutes.

<p class='img'>
  <img src='{{site_root}}/images/integrations/geonetnz_volcano/map.png' />
</p>

<p class='img'>
  <img src='{{site_root}}/images/integrations/geonetnz_volcano/sensor.png' />
</p>

<div class='note'>

The material used by this integration is provided under the [Creative Commons Attribution 3.0 New Zealand (CC BY 3.0 NZ) license](http://creativecommons.org/licenses/by/3.0/nz/).
It has only been modified for the purpose of presenting the material in Home Assistant.
Please refer to the [creator's disclaimer notice](https://www.geonet.org.nz/disclaimer) and [data policy](https://www.geonet.org.nz/policy) for more information.

We acknowledge the New Zealand GeoNet project and its sponsors EQC, GNS Science and LINZ, for providing data/images used in this integration.

</div>

## Configuration

To integrate the GeoNet New Zealand Volcanic Alert Level feed use the **Integrations** feature 
in the GUI, you find it under **Configurations** -> **Integrations**, or add the 
following line to your `configuration.yaml`.

```yaml
# Example configuration.yaml entry
geonetnz_volcano:
```

{% configuration %}
radius:
  description: The radius around your location to monitor; defaults to 50 km or mi (depending on the unit system defined in your configuration).
  required: false
  type: float
  default: 50.0
latitude:
  description: Latitude of the coordinates around which volcanoes are considered.
  required: false
  type: float
  default: Latitude defined in your configuration.
longitude:
  description: Longitude of the coordinates around which volcanoes are considered.
  required: false
  type: float
  default: Longitude defined in your configuration.
{% endconfiguration %}

## State Attributes

The following state attributes are available for each entity in addition to 
the standard ones:

| Attribute              | Description |
|------------------------|-------------|
| latitude               | Latitude of the volcano.  |
| longitude              | Longitude of the volcano. |
| external_id            | The external ID used in the feed to identify the volcano. |
| activity               | Volcanic activity. |
| hazards                | Most likely hazards. |
| distance               | The distance in km or mi to Home Assistant's location. |
| last update            | Timestamp of the last update from the feed.  |
| last update successful | Timestamp of the last successful update from the feed.  |

## Full Configuration

```yaml
# Example configuration.yaml entry
geonetnz_quakes:
  radius: 100
  latitude: -41.2
  longitude: 174.7
```
