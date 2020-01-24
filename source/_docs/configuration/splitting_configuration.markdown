---
title: "Splitting up the configuration"
description: "Splitting the configuration.yaml into several files."
redirect_from: /topics/splitting_configuration/
---

So you've been using Home Assistant for a while now and your configuration.yaml file brings people to tears or you simply want to start off with the distributed approach, here's how to "split the configuration.yaml" into more manageable (read: humanly readable) pieces.

First off, several community members have sanitized (read: without api keys/passwords etc) versions of their configurations available for viewing, you can see a list of them [here](/cookbook/#example-configurationyaml).

As commenting code doesn't always happen, please read on for the details.

Now despite the logical assumption that the `configuration.yaml` will be replaced by this process it will in fact remain, albeit in a much less cluttered form.

In this lighter version we will still need what could be called the core snippet:

```yaml
homeassistant:
  # Name of the location where Home Assistant is running
  name: My Home Assistant Instance
  # Location required to calculate the time the sun rises and sets
  latitude: 37
  longitude: -121
  # 'metric' for Metric, 'imperial' for Imperial
  unit_system: imperial
  # Pick yours from here: http://en.wikipedia.org/wiki/List_of_tz_database_time_zones
  time_zone: America/Los_Angeles
  customize: !include customize.yaml
```

Note that each line after `homeassistant:` is indented two (2) spaces. Since the configuration files in Home Assistant are based on the YAML language, indentation and spacing are important. Also note that seemingly strange entry under `customize:`.

`!include filename.yaml` is the statement that tells Home Assistant to insert the contents of `filename.yaml` at that point. This is how we are going to break a monolithic and hard to read file (when it gets big) into more manageable chunks.

Now before we start splitting out the different components, let's look at the other integrations (in our example) that will stay in the base file:

```yaml
history:
frontend:
logbook:
http:
  api_password: ImNotTelling!

ifttt:
  key: [nope]

wink:
  access_token: [wouldn't you]
  refresh_token: [like to know]

zwave:
  usb_path: /dev/ttyUSB0
  config_path: /usr/local/share/python-openzwave/config
  polling_interval: 10000

mqtt:
  broker: 127.0.0.1
```
As with the core snippet, indentation makes a difference. The integration headers (`mqtt:`) should be fully left aligned (aka no indent), and the parameters (`broker:`) should be indented two (2) spaces.

While some of these integrations can technically be moved to a separate file they are so small or "one off's" where splitting them off is superfluous. Also, you'll notice the # symbol (hash/pound). This represents a "comment" as far as the commands are interpreted. Put another way, any line prefixed with a `#` will be ignored. This makes breaking up files for human readability really convenient, not to mention turning off features while leaving the entry intact.

Now, lets assume that a blank file has been created in the Home Assistant configuration directory for each of the following:

```text
automation.yaml
zone.yaml
sensor.yaml
switch.yaml
device_tracker.yaml
customize.yaml
```

`automation.yaml` will hold all the automation integration details. `zones.yaml` will hold the zone integration details and so forth. These files can be called anything but giving them names that match their function will make things easier to keep track of.

Inside the base configuration file add the following entries:

```yaml
automation: !include automation.yaml
zone: !include zone.yaml
sensor: !include sensor.yaml
switch: !include switch.yaml
device_tracker: !include device_tracker.yaml
```

Note that there can only be one `!include:` for each integration so chaining them isn't going to work. If that sounds like Greek, don't worry about it.

Alright, so we've got the single integrations and the include statements in the base file, what goes in those extra files?

Let's look at the `device_tracker.yaml` file from our example:

```yaml
- platform: owntracks
- platform: nmap_tracker
  hosts: 192.168.2.0/24
  home_interval: 3

  track_new_devices: true
  interval_seconds: 40
  consider_home: 120
```

This small example illustrates how the "split" files work. In this case, we start with two (2) device tracker entries (`owntracks` and `nmap`). These files follow ["style 1"](/getting-started/devices/#style-2-list-each-device-separately) that is to say a fully left aligned leading entry (`- platform: owntracks`) followed by the parameter entries indented two (2) spaces.

This (large) sensor configuration gives us another example:

```yaml
### sensor.yaml
### METEOBRIDGE #############################################
- platform: tcp
  name: 'Outdoor Temp (Meteobridge)'
  host: 192.168.2.82
  timeout: 6
  payload: "Content-type: text/xml; charset=UTF-8\n\n"
  value_template: "{% raw %}{{value.split (' ')[2]}}{% endraw %}"
  unit: C
- platform: tcp
  name: 'Outdoor Humidity (Meteobridge)'
  host: 192.168.2.82
  port: 5556
  timeout: 6
  payload: "Content-type: text/xml; charset=UTF-8\n\n"
  value_template: "{% raw %}{{value.split (' ')[3]}}{% endraw %}"
  unit: Percent

#### STEAM FRIENDS ##################################
- platform: steam_online
  api_key: [not telling]
  accounts:
      - 76561198012067051

#### TIME/DATE ##################################
- platform: time_date
  display_options:
      - 'time'
      - 'date'
- platform: worldclock
  time_zone: Etc/UTC
  name: 'UTC'
- platform: worldclock
  time_zone: America/New_York
  name: 'Ann Arbor'
```

You'll notice that this example includes a secondary parameter section (under the steam section) as well as a better example of the way comments can be used to break down files into sections.

That about wraps it up.

If you have issues checkout `home-assistant.log` in the configuration directory as well as your indentations. If all else fails, head over to our [Discord chat server][discord] and ask away.

### Debugging multiple configuration files

If you have many configuration files, the `check_config` script allows you to see how Home Assistant interprets them:
- Listing all loaded files: `hass --script check_config --files`
- Viewing a component's config: `hass --script check_config --info light`
- Or all components' config:  `hass --script check_config --info all`

You can get help from the command line using: `hass --script check_config --help`

### Advanced Usage

We offer four advanced options to include whole directories at once. Please note that your files must have the `.yaml` file extension; `.yml` is not supported.
- `!include_dir_list` will return the content of a directory as a list with each file content being an entry in the list. The list entries are ordered based on the alphanumeric ordering of the names of the files.
- `!include_dir_named` will return the content of a directory as a dictionary which maps filename => content of file.
- `!include_dir_merge_list` will return the content of a directory as a list by merging all files (which should contain a list) into 1 big list.
- `!include_dir_merge_named` will return the content of a directory as a dictionary by loading each file and merging it into 1 big dictionary.

These work recursively. As an example using `!include_dir_* automation`, will include all 6 files shown below:

```bash
.
└── .homeassistant
    ├── automation
    │   ├── lights
    │   │   ├── turn_light_off_bedroom.yaml
    │   │   ├── turn_light_off_lounge.yaml
    │   │   ├── turn_light_on_bedroom.yaml
    │   │   └── turn_light_on_lounge.yaml
    │   ├── say_hello.yaml
    │   └── sensors
    │       └── react.yaml
    └── configuration.yaml (not included)
```

#### Example: `!include_dir_list`

`configuration.yaml`

```yaml
automation:
  - alias: Automation 1
    trigger:
      platform: state
      entity_id: device_tracker.iphone
      to: 'home'
    action:
      service: light.turn_on
      entity_id: light.entryway
  - alias: Automation 2
    trigger:
      platform: state
      entity_id: device_tracker.iphone
      from: 'home'
    action:
      service: light.turn_off
      entity_id: light.entryway
```

can be turned into:

`configuration.yaml`

```yaml
automation: !include_dir_list automation/presence/
```

`automation/presence/automation1.yaml`

```yaml
alias: Automation 1
trigger:
  platform: state
  entity_id: device_tracker.iphone
  to: 'home'
action:
  service: light.turn_on
  entity_id: light.entryway
```

`automation/presence/automation2.yaml`

```yaml
alias: Automation 2
trigger:
  platform: state
  entity_id: device_tracker.iphone
  from: 'home'
action:
  service: light.turn_off
  entity_id: light.entryway
```

It is important to note that each file must contain only **one** entry when using `!include_dir_list`.
It is also important to note that if you are splitting a file after adding -id: to support the automation UI,
the -id: line must be removed from each of the split files.

#### Example: `!include_dir_named`

`configuration.yaml`

```yaml
{% raw %}
alexa:
  intents:
    LocateIntent:
      action:
        service: notify.pushover
        data:
          message: Your location has been queried via Alexa.
      speech:
        type: plaintext
        text: >
          {%- for state in states.device_tracker -%}
            {%- if state.name.lower() == User.lower() -%}
              {{ state.name }} is at {{ state.state }}
            {%- endif -%}
          {%- else -%}
            I am sorry. Pootie! I do not know where {{User}} is.
          {%- endfor -%}
    WhereAreWeIntent:
      speech:
        type: plaintext
        text: >
          {%- if is_state('device_tracker.iphone', 'home') -%}
            iPhone is home.
          {%- else -%}
            iPhone is not home.
          {% endif %}{% endraw %}
```

can be turned into:

`configuration.yaml`

```yaml
alexa:
  intents: !include_dir_named alexa/
```

`alexa/LocateIntent.yaml`

```yaml
{% raw %}
action:
  service: notify.pushover
  data:
    message: Your location has been queried via Alexa.
speech:
  type: plaintext
  text: >
    {%- for state in states.device_tracker -%}
      {%- if state.name.lower() == User.lower() -%}
        {{ state.name }} is at {{ state.state }}
      {%- endif -%}
    {%- else -%}
      I am sorry. Pootie! I do not know where {{User}} is.
    {%- endfor -%}{% endraw %}
```

`alexa/WhereAreWeIntent.yaml`

```yaml
{% raw %}
speech:
  type: plaintext
  text: >
    {%- if is_state('device_tracker.iphone', 'home') -%}
      iPhone is home.
    {%- else -%}
      iPhone is not home.
    {% endif %}{% endraw %}
```

#### Example: `!include_dir_merge_list`

`configuration.yaml`

```yaml
automation:
  - alias: Automation 1
    trigger:
      platform: state
      entity_id: device_tracker.iphone
      to: 'home'
    action:
      service: light.turn_on
      entity_id: light.entryway
  - alias: Automation 2
    trigger:
      platform: state
      entity_id: device_tracker.iphone
      from: 'home'
    action:
      service: light.turn_off
      entity_id: light.entryway
```

can be turned into:

`configuration.yaml`

```yaml
automation: !include_dir_merge_list automation/
```

`automation/presence.yaml`

```yaml
- alias: Automation 1
  trigger:
    platform: state
    entity_id: device_tracker.iphone
    to: 'home'
  action:
    service: light.turn_on
    entity_id: light.entryway
- alias: Automation 2
  trigger:
    platform: state
    entity_id: device_tracker.iphone
    from: 'home'
  action:
    service: light.turn_off
    entity_id: light.entryway
```

It is important to note that when using `!include_dir_merge_list`, you must include a list in each file (each list item is denoted with a hyphen [-]). Each file may contain one or more entries.

#### Example: `!include_dir_merge_named`

`configuration.yaml`

```yaml
group:
  bedroom:
    name: Bedroom
    entities:
      - light.bedroom_lamp
      - light.bedroom_overhead
  hallway:
    name: Hallway
    entities:
      - light.hallway
      - thermostat.home
  front_yard:
    name: Front Yard
    entities:
      - light.front_porch
      - light.security
      - light.pathway
      - sensor.mailbox
      - camera.front_porch
```

can be turned into:

`configuration.yaml`

```yaml
group: !include_dir_merge_named group/
```

`group/interior.yaml`

```yaml
bedroom:
  name: Bedroom
  entities:
    - light.bedroom_lamp
    - light.bedroom_overhead
hallway:
  name: Hallway
  entities:
    - light.hallway
    - thermostat.home
```

`group/exterior.yaml`

```yaml
front_yard:
  name: Front Yard
  entities:
    - light.front_porch
    - light.security
    - light.pathway
    - sensor.mailbox
    - camera.front_porch
```

[discord]: https://discord.gg/c5DvZ4e
