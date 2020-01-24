---
title: Ecobee
description: Instructions for how to integrate ecobee thermostats and sensors within Home Assistant.
logo: ecobee.png
ha_category:
  - Sensor
  - Binary Sensor
  - Notifications
  - Climate
  - Weather
featured: true
ha_release: 0.9
ha_iot_class: Cloud Poll
ha_config_flow: true
ha_codeowners:
  - '@marthoc'
---

The `ecobee` integration lets you control and view sensor data from [ecobee](https://ecobee.com) thermostats.

## Preliminary Step

You will need to obtain an API key from ecobee's [developer site](https://www.ecobee.com/developers/) to use this integration. To get the key, your thermostat must be registered on ecobee's website (which you likely would have already done while installing your thermostat). Once you have done that, perform the following steps.

1. Click on the **Become a developer** link on the [developer site](https://www.ecobee.com/developers/).
2. Log in with your ecobee credentials.
3. Accept the SDK agreement.
4. Fill in the fields.
5. Click **save**.

Log in to the regular consumer portal, and click the overflow menu button in the upper right. You will see a new option named **Developer**. Now we can create the Application to hook up to Home Assistant.

1. Select the **Developer** option from the hamburger menu.
2. Select **Create New**.
3. Give your App a name (it must be unique across all ecobee users; try your-name-or-alias-home-assistant) and a summary (which need not be unique). Neither of these are important as they are not used anywhere in Home Assistant.
4. For authorization method select **ecobee PIN**.
5. You don't need an Application Icon or Detailed Description.
6. Click **Create**.

Under the Name and Summary Section, you will now have an API key. Copy this key and use it in your configuration section below. Click the **X** to close the Developer section.

## Configuring the Integration

To configure the ecobee integration in Home Assistant, you can either use the **Configuration** > **Integrations** menu, or add an entry to `configuration.yaml`.

### Setup via the Integrations menu

1. In the **Configuration** > **Integrations** menu, click **+** and then select `ecobee` from the pop-up menu.
2. In the pop-up box, enter the API key you obtained from ecobee.com.
3. In the next pop-up box, you will be presented with a unique four-character PIN code which you will need to authorize in the [ecobee consumer portal](https://www.ecobee.com/consumerportal/index.html). You can do this by logging in, selecting **My Apps** from the hamburger menu, clicking **Add Application** on the left, entering the PIN code from Home Assistant, and clicking **Validate** and then **Add Application** in the bottom right.
4. After authorizing the App on ecobee.com, return to Home Assistant and hit **Submit**. If the authorization was successful, a config entry will be created and your thermostats and sensors will be available in Home Assistant.

### Setup via configuration.yaml

If you prefer to initially set up this integration in [`configuration.yaml`](/docs/configuration/), you may do so by adding your API key (and optional parameters) as follows (however, you must still complete authorization via the **Integrations** menu):

```yaml
# Example configuration.yaml entry
ecobee:
  api_key: YOUR_API_KEY
```

{% configuration %}
api_key:
  description: Your ecobee API key. This is only needed for the initial setup of the integration. Once registered it can be removed. If you revoke the key in the ecobee portal, you will need to remove the existing `ecobee` configuration in the **Integrations** menu, update this, and then configure the integration again.
  required: false
  type: string
{% endconfiguration %}

<p class='img'>
  <img src='{{site_root}}/images/screenshots/ecobee-sensor-badges.png' />
  <img src='{{site_root}}/images/screenshots/ecobee-thermostat-card.png' />
</p>

[Restart Home Assistant](/docs/configuration/#reloading-changes) for the changes to take effect. In the **Configuration** > **Integrations** menu, hit **Configure** next to the discovered `ecobee` entry, and continue to authorize the App per the Integration menu instructions above.

The first time you (re)run Home Assistant with this integration it will give you a PIN code that you need to authorize in the [ecobee consumer portal](https://www.ecobee.com/consumerportal/index.html). You can do this by clicking **Add Application** in the **My Apps** section in the sidebar.

The PIN can be found in the Home Assistant portal on the Ecobee card or from the **configurator.ecobee** entity in the States developer tool.

- If you do not have an ecobee card, you may be using groups with `default_view` that don't show the card. To get around this, you can temporarily comment out the `default_view` section or add the `configurator.ecobee` integration to your `default_view` and restart Home Assistant.

Once you enter the PIN on the ecobee site, wait approximately 5 minutes, and then click on the **I have authorized the app** link at the bottom of the ecobee pop-up window. If everything worked correctly, you should now be able to restart Home Assistant again to see the full ecobee card with all of the sensors populated or see the list of sensors in the developer tools. Now you can re-enable your `default_view` (if you had to disable it) and add the ecobee sensors to a group and/or view.

## Notifications

To get your ecobee notifications working with Home Assistant, you must first have the main ecobee integration loaded and running. Once you have that configured, you can set up this integration to send messages to your ecobee device.

To use this notification platform in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
notify:
  - name: NOTIFIER_NAME
    platform: ecobee
```

{% configuration %}
name:
  description: Setting the optional parameter `name` allows multiple notifiers to be created. The notifier will bind to the service `notify.NOTIFIER_NAME`.
  required: false
  default: "`notify`"
  type: string
{% endconfiguration %}

To use notifications, please see the [getting started with automation page](/getting-started/automation/).

## Thermostat

### Concepts

The ecobee thermostat supports the following key concepts.

The _target temperature_ is the temperature that the device attempts
to achieve. The target temperature is either determined by the
currently active climate or it may be overridden by a hold. When the
thermostat is not in auto mode, there is a single target
temperature. When the thermostat is in auto HVAC mode, there is a
pair of target temperatures: the lower target temperature determines
the lowest desired temperature, while the higher target temperature
determines the highest desired temperature (the thermostat will switch
between heating and cooling to keep the temperature within these
limits).

A _climate_ is a predefined or user-defined set of presets that the
thermostat aims to achieve. The ecobee thermostat provides three predefined
climates: Home, Away, and Sleep. Ecobee refers to these as _comfort settings_. The user can define additional climates.

A _preset_ is an override of the target temperature defined in the
currently active climate. The temperature targeted in the preset mode may be
explicitly set (temperature preset), it may be derived from a reference
climate (home, away, sleep, etc.), or it may be derived from a vacation
defined by the thermostat. All holds are temporary. Temperature and
climate holds expire when the thermostat transitions to the next climate
defined in its program. A vacation hold starts at the beginning of the
defined vacation period and expires when the vacation period ends.

When in _away preset_, the target temperature is permanently overridden by
the target temperature defined for the away climate. The away preset is a
simple way to emulate a vacation mode.

The _HVAC mode_ of the device is the currently active operational
modes that the ecobee thermostat provides: heat, auxHeatOnly, cool,
auto, and off.

## Attributes

The ecobee climate entity has some extra attributes to represent the state of the thermostat.

| Name | Description |
| ---- | ----------- |
| `fan` | If the fan is currently on or off: `on` / `off`.
| `climate_mode` | This is the climate mode that is active, or would be active if no override is active.
| `equipment_running` | This is a comma-separated list of equipment that is currently running.
| `fan_min_on_time` | The minimum amount of time (in minutes) that the fan will run per hour. This is determined by the minimum fan runtime setting which can be changed in the ecobee app or on the thermostat itself.

## Services

Besides the standard services provided by the Home Assistant [Climate](/integrations/climate/) integration, the following extra services are provided by the ecobee integration:

- `ecobee.create_vacation`
- `ecobee.delete_vacation`
- `ecobee.resume_program`
- `ecobee.set_fan_min_on_time`

### Service `ecobee.create_vacation`

Creates a vacation on the selected ecobee thermostat.

| Service data attribute | Optional | Description                                                                                          |
| ---------------------- | -------- | ---------------------------------------------------------------------------------------------------- |
| `entity_id`            | no       | ecobee thermostat on which to create the vacation                                                    |
| `vacation_name`        | no       | Name of the vacation to create. Must be unique on the thermostat                                     |
| `cool_temp`            | no       | Cooling temperature during the vacation                                                              |
| `heat_temp`            | no       | Heating temperature during the vacation                                                              |
| `start_date`           | yes      | Date the vacation starts in YYYY-MM-DD format                                                        |
| `start_time`           | yes      | Time the vacation starts in the local time zone. Must be in 24-hour format (HH:MM:SS)        |
| `end_date`             | yes      | Date the vacation ends in YYYY-MM-DD format (14 days from now if not provided)                       |
| `end_time`             | yes      | Time the vacation ends in the local time zone. Must be in 24-hour format (HH:MM:SS)          |
| `fan_mode`             | yes      | Fan mode of the thermostat during the vacation (auto or on) (auto if not provided)                   |
| `fan_min_on_time`      | yes      | Minimum number of minutes to run the fan each hour (0 to 60) during the vacation (0 if not provided) |

### Service `ecobee.delete_vacation`

Delete a vacation on the selected ecobee thermostat.

| Service data attribute | Optional | Description                                       |
| ---------------------- | -------- | ------------------------------------------------- |
| `entity_id`            | no       | ecobee thermostat on which to delete the vacation |
| `vacation_name`        | no       | Name of the vacation to delete                    |

### Service `ecobee.resume_program`

Resumes the currently active schedule.

| Service data attribute | Optional | Description                                                                                            |
| ---------------------- | -------- | ------------------------------------------------------------------------------------------------------ |
| `entity_id`            | yes      | String or list of strings that point at `entity_id`'s of climate devices to control. Else targets all. |
| `resume_all`           | no       | true or false                                                                                          |

### Service `ecobee.set_fan_min_on_time`

Sets the minimum amount of time that the fan will run per hour.

| Service data attribute | Optional | Description                                                                                            |
| ---------------------- | -------- | ------------------------------------------------------------------------------------------------------ |
| `entity_id`            | yes      | String or list of strings that point at `entity_id`'s of climate devices to control. Else targets all. |
| `fan_min_on_time`      | no       | integer (e.g. 5)                                                                                       |
