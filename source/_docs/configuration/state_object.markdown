---
title: "State Objects"
description: "Describes all there is to know about state objects in Home Assistant."
redirect_from: /topics/state_object/
---

Your devices are represented in Home Assistant as entities. The entities will write their current state to the state machine for other entities/templates/frontend to access. States are a current representation of the entity.

If you overwrite a state via the states dev tool or the API, it will not impact the actual device. If the device state is being polled, it will overwrite the state in the state machine the next polling.

All states will always have an entity id, a state and a timestamp when last updated and last changed.

Field | Description
----- | -----------
`state.state` | String representation of the current state of the entity. Example `off`.
`state.entity_id` | Entity ID. Format: `<domain>.<object_id>`. Example: `light.kitchen`.
`state.domain` | Domain of the entity. Example: `light`.
`state.object_id` | Object ID of entity. Example: `kitchen`.
`state.name` | Name of the entity. Based on `friendly_name` attribute with fall back to object ID. Example: `Kitchen Ceiling`.
`state.last_updated` | Time the state was written to the state machine in UTC time. Note that writing the exact same state including attributes will not result in this field being updated. Example: `2017-10-28 08:13:36.715874+00:00`.
`state.last_changed` | Time the state changed in the state machine in UTC time. This is not updated when there are only updated attributes. Example: `2017-10-28 08:13:36.715874+00:00`.
`state.attributes` | A dictionary with extra attributes related to the current state.

The attributes of an entity are optional. There are a few attributes that are used by Home Assistant for representing the entity in a specific way. Each integration will also have its own attributes to represent extra state data about the entity. For example, the light integration has attributes for the current brightness and color of the light. When an attribute is not available, Home Assistant will not write it to the state.

When using templates, attributes will be available by their name. For example `state.attributes.assumed_state`.

Attribute | Description
--------- | -----------
`friendly_name` | Name of the entity. Example: `Kitchen Ceiling`.
`icon` | Icon to use for the entity in the frontend. Example: `mdi:home`.
`entity_picture` | URL to a picture that should be used instead of showing the domain icon. Example: `http://example.com/picture.jpg`.
`assumed_state` | Boolean if the current state is an assumption. [More info](/blog/2016/02/12/classifying-the-internet-of-things/#classifiers) Example: `True`.
`unit_of_measurement` | The unit of measurement the state is expressed in. Used for grouping graphs or understanding the entity. Example: `°C`.
`hidden` | Boolean if the entity should not be shown in the frontend. Example: `true`. This does not apply to the Lovelace UI, and is only relevant for the old `states` UI.

When an attribute contains spaces, you can retrieve it like this: `states.sensor.livingroom.attributes["Battery numeric"]`.
