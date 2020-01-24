---
title: "Google Calendar Event"
description: "Instructions on how to use Google Calendars in Home Assistant."
logo: google_calendar.png
ha_category:
  - Calendar
ha_iot_class: Cloud Polling
ha_release: 0.33
---

The `google` calendar platform allows you to connect to your
[Google Calendars](https://calendar.google.com) and generate binary sensors.
The sensors created can trigger based on any event on the calendar or only for
matching events. When you first setup this integration it will generate a new
configuration file `google_calendars.yaml` in your config directory that will contain information about
all of the calendars you can see.
It also exposes a service to add an event to one of your Google Calendars.

## Prerequisites

Generate a Client ID and Client Secret on
[Google Developers Console](https://console.developers.google.com/start/api?id=calendar).

1. Follow the wizard using the following information.
1. When it gets to the point of asking _Which API are you using?_ just click cancel.
1. Under APIs & Services (left sidebar) > Credentials, click on the menu item, 'OAuth consent screen'.
1. Set the 'Application Name' (the name of the application asking for consent) to anything you want. We suggest "Home-Assistant".
1. Save this page. You don't have to fill out anything else here.
1. Click on the menu item, Credentials, then click 'Create credentials' > OAuth client ID.
1. Set the Application type to 'Other' and give this credential set a name (like "Home Assistant Credentials") then click 'Create'.
1. Copy the client ID and client secret from the page that follows into a text editor temporarily as you will need to put these in your `configuration.yaml` file.
1. Click on the menu item, Library, then search for "Google Calendar API" and enable it (if it isn't already enabled automatically through this process).

If you will later be adding more scopes than just the "Google Calendar API" to the OAuth for this application, you will need to delete your token file under your Home Assistant Profile. You will lose your refresh token due to the re-authenticating to add more API access. It's recommended to use different authorizations for different pieces of Google.

## Configuration

To integrate Google Calendar in Home Assistant,
add the following section to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
google:
  client_id: YOUR_CLIENT_ID
  client_secret: YOUR_CLIENT_SECRET
```

{% configuration %}
client_id:
  description: Use the client ID you generated in the Prerequisites stage.
  required: true
  type: string
client_secret:
  description: Use the client secret you generated in the Prerequisites stage.
  required: true
  type: string
track_new_calendar:
  description: >
    Will automatically generate a binary sensor when a new calendar
    is detected. The system scans for new calendars only on startup.
  required: false
  type: boolean
  default: true
{% endconfiguration %}

The next time you run or restart Home Assistant, you should find a new notification (the little bell icon in the lower left corner). Click on that notification it will give you a link and an authentication code. Click on that link to open a Google website where you should enter the code found in the notification. This will grant your Home Assistant service read-only access to all the Google Calendars that the account you
authenticate with can read.

## Calendar Configuration

Editing the `google_calendars.yaml` file.

A basic entry for a single calendar looks like:

```yaml
- cal_id: "*****@group.calendar.google.com"
  entities:
  - device_id: test_everything
    name: Give me everything
    track: true
- cal_id: "*****@group.calendar.google.com"
  entities:
  - device_id: test_important
    name: Important Stuff
    track: true
    search: "#Important"
    offset: "!!"
  - device_id: test_unimportant
    name: UnImportant Stuff
    track: true
    search: "#UnImportant"
```

{% configuration %}
cal_id:
  description: The Google *generated* unique id for this calendar.
  required: true
  type: string
  default: "**DO NOT CHANGE THE DEFAULT VALUE**"
entities:
  description: Yes, you can have multiple sensors for a calendar!
  required: true
  type: list
  keys:
    device_id:
      description: >
        The name that all your automations/scripts
        will use to reference this device.
      required: true
      type: string
    name:
      description: What is the name of your sensor that you'll see in the frontend.
      required: true
      type: string
    track:
      description: "Should we create a sensor `true` or ignore it `false`?"
      required: true
      type: boolean
      default: true
    search:
      description: If set will only trigger for matched events.
      required: false
      type: string
    offset:
      description: >
        A set of characters that precede a number in the event title
        for designating a pre-trigger state change on the sensor.
        This should be in the format of HH:MM or MM.
      required: false
      type: string
      default: "!!"
    ignore_availability:
      description: "Should we respect `free`/`busy` flags?"
      required: false
      type: boolean
      default: true
    max_results:
      description: "Max number of entries to retrieve"
      required: false
      type: integer
      default: 5
{% endconfiguration %}

From this we will end up with the binary sensors `calendar.test_unimportant` and
`calendar.test_important` which will toggle themselves on/off based on events on
the same calendar that match the search value set for each.
You'll also have a sensor `calendar.test_everything` that will
not filter events out and always show the next event available.

But what if you only wanted it to toggle based on all events?
Just leave out the *search* parameter.

<div class='note warning'>

If you use a `#` sign for `search` then wrap the whole search term in quotes.
Otherwise everything following the hash sign would be considered a YAML comment.

</div>

### Sensor attributes

 - **offset_reached**: If set in the event title and parsed out will be `on`/`off` once the offset in the title in minutes is reached. So the title `Very important meeting #Important !!-10` would trigger this attribute to be `on` 10 minutes before the event starts.
 - **all_day**: `true`/`false` if this is an all day event. Will be `false` if there is no event found.
 - **message**: The event title with the `search` and `offset` values extracted. So in the above example for **offset_reached** the **message** would be set to `Very important meeting`
 - **description**: The event description.
 - **location**: The event Location.
 - **start_time**: Start time of event.
 - **end_time**: End time of event.

### Service `google.add_event`

You can use the service `google.add_event` to create a new calendar event in a calendar. Calendar id's can be found in the file `google_calendars.yaml`. All dates and times are in your local time, the integration gets your time zone from your `configuration.yaml` file.

| Service data attribute | Optional | Description | Example |
| ---------------------- | -------- | ----------- | --------|
| `calendar_id` | no | The id of the calendar you want. | *****@group.calendar.google.com
| `summary` | no | Acts as the title of the event. | Bowling
| `description` | yes | The description of the event. | Birthday bowling
| `start_date_time` | yes | The date and time the event should start. | 2019-03-10 20:00:00
| `end_date_time` | yes | The date and time the event should end. | 2019-03-10 23:00:00
| `start_date` | yes | The date the whole day event should start. | 2019-03-10
| `end_date` | yes | The date the whole day event should end. | 2019-03-11
| `in` | yes | Days or weeks that you want to create the event in. | "days": 2

<div class='note'>

You either use `start_date_time` and `end_date_time`, or `start_date` and `end_date`, or `in`.

</div>

## Using calendar in automations

A calendar can be used as an external scheduler for special events or reoccurring events instead of hardcoding them in automations.

Trigger as soon as an event starts:

```yaml
    trigger:
      platform: state
      entity_id: calendar.calendar_name
      to: 'on'
```

By using specific text in the event title, you can set conditions to initiate particular automation flows on designated events while other events will be ignored.

For example, the actions following this condition will only be executed for events named 'vacation':

{% raw %}
```yaml
    condition:
        condition: template
        value_template: "{{is_state_attr('calendar.calendar_name', 'message', 'vacation') }}"
```
{% endraw %}
