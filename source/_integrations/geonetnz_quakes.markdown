---
title: GeoNet NZ Quakes
description: Instructions on how to integrate the GeoNet New Zealand Quakes feed into Home Assistant.
logo: geonet-nz.png
ha_category:
  - Geolocation
ha_iot_class: Cloud Polling
ha_release: 0.98
ha_config_flow: true
ha_codeowners:
  - '@exxamalte'
---

The `geonetnz_quakes` integration lets you use a GeoJSON feed provided by 
New Zealand's [GeoNet](https://www.geonet.org.nz/) with information 
about quakes in the New Zealand region that happened within the last 7 days. 
It retrieves incidents from a feed and 
shows information of those incidents filtered by distance to Home Assistant's 
location.

Entities are generated, updated and removed automatically with each update 
from the feed. Each entity defines latitude and longitude and will be shown 
on the default map automatically, or on a map card by defining the source 
`geonetnz_quakes`. The distance is available as the state of each entity, and 
converted to the unit (kilometers or miles) configured in Home Assistant.

<p class='img'>
  <img src='{{site_root}}/images/screenshots/geonetnz-quakes-feed-map.png' />
</p>

The data is updated every 5 minutes.

<div class='note'>

The material used by this integration is provided under the [Creative Commons Attribution 3.0 New Zealand (CC BY 3.0 NZ) license](https://creativecommons.org/licenses/by/3.0/nz/).
It has only been modified for the purpose of presenting the material in Home Assistant.
Please refer to the [creator's disclaimer notice](https://www.geonet.org.nz/disclaimer) and [data policy](https://www.geonet.org.nz/policy) for more information.

We acknowledge the New Zealand GeoNet project and its sponsors EQC, GNS Science and LINZ, for providing data/images used in this integration.

</div>

## Configuration

To integrate the GeoNet New Zealand Quakes feed use the "Integrations" feature 
in the GUI, you find it under Configurations - Integrations, or add the 
following line to your `configuration.yaml`.

```yaml
# Example configuration.yaml entry
geonetnz_quakes:
```

{% configuration %}
mmi:
  description: Request quakes that may have caused shaking greater than or equal to the [MMI](https://www.geonet.org.nz/earthquake/mmi) value. Allowable values are -1..8 inclusive. Value -1 is used for quakes that are too small to calculate a stable MMI value for.
  required: false
  type: integer
  default: 2
minimum_magnitude:
  description: The minimum magnitude of an earthquake to be included.
  required: false
  type: float
  default: 0.0
radius:
  description: The radius around your location to monitor; defaults to 50 km or mi (depending on the unit system defined in your `configuration.yaml`).
  required: false
  type: float
  default: 50.0
latitude:
  description: Latitude of the coordinates around which quakes are considered.
  required: false
  type: float
  default: Latitude defined in your configuration.
longitude:
  description: Longitude of the coordinates around which quakes are considered.
  required: false
  type: float
  default: Longitude defined in your configuration.
{% endconfiguration %}

## State Attributes

The following state attributes are available for each entity in addition to 
the standard ones:

| Attribute   | Description |
|-------------|-------------|
| latitude    | Latitude of the quake.  |
| longitude   | Longitude of the quake. |
| source      | `geonetnz_quakes` to be used in conjunction with `geo_location` automation trigger. |
| external_id | The external ID used in the feed to identify the quake in the feed. |
| title       | Title of this entry. |
| mmi         | The calculated MMI shaking at the closest locality in the New Zealand region.
| magnitude   | The summary magnitude for the quake. |
| depth       | The depth of the quake in km. |
| time        | The origin date and time of the quake. |
| locality    | Distance and direction to the nearest locality. |
| quality     | The quality of this information: best, good, caution, deleted. |

Please note that the reported MMI may be lower than the minimum requested MMI. 
This integration is passing the requested MMI value to the feed source and 
displays all entries retrieved without further filtering by MMI.

## Sensor

This integration automatically creates a sensor that shows how many entities
are currently managed by this integration. In addition to that the sensor has
some useful attributes that indicate the currentness of the data retrieved
from the feed.

<p class='img'>
  <img src='{{site_root}}/images/screenshots/geonetnz-quakes-sensor.png' />
</p>

| Attribute              | Description |
|------------------------|-------------|
| status                 | Status of last update from the feed ("OK" or "ERROR").  |
| last update            | Timestamp of the last update from the feed.  |
| last update successful | Timestamp of the last successful update from the feed.  |
| last timestamp         | Timestamp of the latest entry from the feed.  |
| created                | Number of entities that were created during last update (optional).  |
| updated                | Number of entities that were updated during last update (optional).  |
| removed                | Number of entities that were removed during last update (optional).  |

## Full Configuration

```yaml
# Example configuration.yaml entry
geonetnz_quakes:
  radius: 100
  mmi: 4
  minimum_magnitude: 4.5
  latitude: -41.2
  longitude: 174.7
```
