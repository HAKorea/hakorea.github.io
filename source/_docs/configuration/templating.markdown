---
title: "Templating"
description: "Instructions on how to use the templating feature of Home Assistant."
redirect_from: /topics/templating/
---

This is an advanced feature of Home Assistant. You'll need a basic understanding of:

- [Home Assistant architecture](/developers/architecture/), especially states.
- The [State object](/topics/state_object/).

Templating is a powerful feature that allows you to control information going into and out of the system. It is used for:

- Formatting outgoing messages in, for example, the [notify](/integrations/notify/) platforms and [alexa](/integrations/alexa/) component.
- Process incoming data from sources that provide raw data, like [MQTT](/integrations/mqtt/), [`rest` sensor](/integrations/rest/) or the [`command_line` sensor](/integrations/sensor.command_line/).
- [Automation Templating](/docs/automation/templating/).

## Building templates

Templating in Home Assistant is powered by the [Jinja2](https://palletsprojects.com/p/jinja) templating engine. This means that we are using their syntax and make some custom Home Assistant variables available to templates during rendering. Jinja2 supports a wide variety of operations:

- [Mathematical operation](https://jinja.palletsprojects.com/en/master/templates/#math)
- [Comparisons](https://jinja.palletsprojects.com/en/master/templates/#comparisons)
- [Logic](https://jinja.palletsprojects.com/en/master/templates/#logic)

We will not go over the basics of the syntax, as Jinja2 does a great job of this in their [templates documentation](https://jinja.palletsprojects.com/en/master/templates/).

The frontend has a template editor tool to help develop and debug templates. Click on the <img src='/images/screenshots/developer-tool-templates-icon.png' alt='template developer tool icon' class="no-shadow" height="38" /> icon, create your template in the _Template editor_ and check the results on the right.

Templates can get big pretty fast. To keep a clear overview, consider using YAML multiline strings to define your templates:

{% raw %}
```yaml
script:
  msg_who_is_home:
    sequence:
      - service: notify.notify
        data_template:
          message: >
            {% if is_state('device_tracker.paulus', 'home') %}
              Ha, Paulus is home!
            {% else %}
              Paulus is at {{ states('device_tracker.paulus') }}.
            {% endif %}
```
{% endraw %}

## Home Assistant template extensions

Extensions allow templates to access all of the Home Assistant specific states and adds other convenience functions and filters.

### States

- Iterating `states` will yield each state sorted alphabetically by entity ID.
- Iterating `states.domain` will yield each state of that domain sorted alphabetically by entity ID.
- `states.sensor.temperature` returns the state object for `sensor.temperature` (avoid when possible, see note below).
- `states('device_tracker.paulus')` will return the state string (not the object) of the given entity or `unknown` if it doesn't exist.
- `is_state('device_tracker.paulus', 'home')` will test if the given entity is the specified state.
- `state_attr('device_tracker.paulus', 'battery')` will return the value of the attribute or None if it doesn't exist.
- `is_state_attr('device_tracker.paulus', 'battery', 40)` will test if the given entity attribute is the specified state (in this case, a numeric value).

<div class='note warning'>

  Avoid using `states.sensor.temperature.state`, instead use `states('sensor.temperature')`. It is strongly advised to use the `states()`, `is_state()`, `state_attr()` and `is_state_attr()` as much as possible, to avoid errors and error message when the entity isn't ready yet (e.g., during Home Assistant startup).

</div>

Besides the normal [state object methods and properties](/topics/state_object/), `states.sensor.temperature.state_with_unit` will print the state of the entity and, if available, the unit.

#### States examples

The next two statements result in the same value if the state exists. The second one will result in an error if the state does not exist.

{% raw %}
```text
{{ states('device_tracker.paulus') }}
{{ states.device_tracker.paulus.state }}
```
{% endraw %}

Print out a list of all the sensor states:

{% raw %}
```text
{% for state in states.sensor %}
  {{ state.entity_id }}={{ state.state }},
{% endfor %}
```
{% endraw %}

Other state examples:
{% raw %}

```text
{% if is_state('device_tracker.paulus', 'home') %}
  Ha, Paulus is home!
{% else %}
  Paulus is at {{ states('device_tracker.paulus') }}.
{% endif %}

{{ states('sensor.temperature') | float + 1 }}

{{ (states('sensor.temperature') | float * 10) | round(2) }}

{% if states('sensor.temperature') | float > 20 %}
  It is warm!
{% endif %}

{{ as_timestamp(states.binary_sensor.garage_door.last_changed) }}

{{ as_timestamp(now()) - as_timestamp(states.binary_sensor.garage_door.last_changed) }}
```
{% endraw %}

### Attributes

You can print an attribute with `state_attr` if state is defined.

#### Attributes examples

{% raw %}
```text
{% if states.device_tracker.paulus %}
  {{ state_attr('device_tracker.paulus', 'battery') }}
{% else %}
  ??
{% endif %}
```
{% endraw %}

With strings:

{% raw %}
```text
{% set tracker_name = "paulus"%}

{% if states("device_tracker." + tracker_name) != "unknown" %}
  {{ state_attr("device_tracker." + tracker_name, "battery")}}
{% else %}
  ??
{% endif %}
```
{% endraw %}

### Working with Groups

The `expand` function and filter can be used to sort entities and expand groups. It outputs a sorted array of entities with no duplicates.

#### Expand examples

{% raw %}
```text
{% for tracker in expand('device_tracker.paulus', 'group.child_trackers') %}
  {{ state_attr(tracker, 'battery') }}
  {%- if not loop.last %}, {% endif -%}
{% endfor %}
```
{% endraw %}

The same thing can also be expressed as a filter:

{% raw %}
```text
{{ ['device_tracker.paulus', 'group.child_trackers'] | expand 
  | selectattr("attributes.battery", 'defined')
  | join(', ', attribute="attributes.battery") }}
```
{% endraw %}

### Time

- `now()` will be rendered as the current time in your time zone.
  - For specific values: `now().second`, `now().minute`, `now().hour`, `now().day`, `now().month`, `now().year`, `now().weekday()` and `now().isoweekday()`
- `utcnow()` will be rendered as UTC time.
  - For specific values: `utcnow().second`, `utcnow().minute`, `utcnow().hour`, `utcnow().day`, `utcnow().month`, `utcnow().year`, `utcnow().weekday()` and `utcnow().isoweekday()`.
- `as_timestamp()` will convert datetime object or string to UNIX timestamp. This function also be used as a filter.
- `strptime(string, format)` will parse a string to a datetime based on a [format](https://docs.python.org/3.6/library/datetime.html#strftime-and-strptime-behavior).
- `relative_time` will convert datetime object to its human-friendly "age" string. The age can be in second, minute, hour, day, month or year (but only the biggest unit is considered, e.g. if it's 2 days and 3 hours, "2 days" will be returned). Note that it only works for dates _in the past_.
- Filter `timestamp_local`  will convert an UNIX timestamp to local time/data.
- Filter `timestamp_utc` will convert a UNIX timestamp to UTC time/data.
- Filter `timestamp_custom(format_string, local_boolean)` will convert a UNIX timestamp to a custom format, the use of a local timestamp is default. Supports the standard [Python time formatting options](https://docs.python.org/3/library/time.html#time.strftime).

### To/From JSON

The `to_json` filter serializes an object to a JSON string. In some cases, it may be necessary to format a JSON string for use with a webhook, as a parameter for command line utilities or any number of other applications. This can be complicated in a template, especially when dealing with escaping special characters. Using the `to_json` filter, this is handled automatically.

The `from_json` filter operates similarly, but in the other direction, de-serializing a JSON string back into an object.

### To/From JSON examples

In this example, the special character '°' will be automatically escaped in order to produce valid JSON. The difference between the stringified object and the actual JSON is evident.

*Template*

{% raw %}
```text
{% set temp = {'temperature': 25, 'unit': '°C'} %}
stringified object: {{ temp }}
object|to_json: {{ temp|to_json }}
```
{% endraw %}

*Output*

{% raw %}
```text
stringified object: {'temperature': 25, 'unit': '°C'}
object|to_json: {"temperature": 25, "unit": "\u00b0C"}
```
{% endraw %}

Conversely, `from_json` can be used to de-serialize a JSON string back into an object to make it possible to easily extract usable data.

*Template*

{% raw %}
```text
{% set temp = '{"temperature": 25, "unit": "\u00b0C"}'|from_json %}
The temperature is {{ temp.temperature }}{{ temp.unit }}
```
{% endraw %}

*Output*

{% raw %}
```text
The temperature is 25°C
```
{% endraw %}

### Distance

- `distance()` will measure the distance in kilometers between home, entity, coordinates.
- `closest()` will find the closest entity.

#### Distance examples

If only one location is passed in, Home Assistant will measure the distance from home.

{% raw %}
```text
Using Lat Lng coordinates: {{ distance(123.45, 123.45) }}

Using State: {{ distance(states.device_tracker.paulus) }}

These can also be combined in any combination:
{{ distance(123.45, 123.45, 'device_tracker.paulus') }}
{{ distance('device_tracker.anne_therese', 'device_tracker.paulus') }}
```
{% endraw %}

#### Closest examples

The closest function and filter will find the closest entity to the Home Assisant location:

{% raw %}
```text
Query all entities: {{ closest(states) }}
Query all entities of a specific domain: {{ closest(states.device_tracker) }}
Query all entities in group.children: {{ closest('group.children') }}
Query all entities in group.children: {{ closest(states.group.children) }}
```
{% endraw %}

Find entities closest to a coordinate or another entity. All previous arguments still apply for second argument.

{% raw %}
```text
Closest to a coordinate: {{ closest(23.456, 23.456, 'group.children') }}
Closest to an entity: {{ closest('zone.school', 'group.children') }}
Closest to an entity: {{ closest(states.zone.school, 'group.children') }}
```
{% endraw %}

Since closest returns a state, we can combine it with distance too.

{% raw %}
```text
{{ closest(states).name }} is {{ distance(closest(states)) }} kilometers away.
```
{% endraw %}

The last argument of the closest function has an implicit `expand`, and can take any iterable sequence of states or entity IDs, and will expand groups:

{% raw %}
```text
Closest out of given entities: 
    {{ closest(['group.children', states.device_tracker]) }}
Closest to a coordinate:  
    {{ closest(23.456, 23.456, ['group.children', states.device_tracker]) }}
Closest to some entity: 
    {{ closest(states.zone.school, ['group.children', states.device_tracker]) }}
```

It will also work as a filter over an iterable group of entities or groups:

```text
Closest out of given entities: 
    {{ ['group.children', states.device_tracker] | closest }}
Closest to a coordinate:  
    {{ ['group.children', states.device_tracker] | closest(23.456, 23.456) }}
Closest to some entity: 
    {{ ['group.children', states.device_tracker] | closest(states.zone.school) }}
```

{% endraw %}

### Formatting

- `float` will format the output as float.

### Numeric functions and filters

Some of these functions can also be used in a [filter](https://jinja.palletsprojects.com/en/master/templates/#id11). This means they can act as a normal function like this `sqrt(2)`, or as part of a filter like this `2|sqrt`.

- `log(value, base)` will take the logarithm of the input. When the base is omitted, it defaults to `e` - the natural logarithm. Can also be used as a filter.
- `sin(value)` will return the sine of the input. Can be used as a filter.
- `cos(value)` will return the cosine of the input. Can be used as a filter.
- `tan(value)` will return the tangent of the input. Can be used as a filter.
- `asin(value)` will return the arcus sine of the input. Can be used as a filter.
- `acos(value)` will return the arcus cosine of the input. Can be used as a filter.
- `atan(value)` will return the arcus tangent of the input. Can be used as a filter.
- `atan2(y, x)` will return the four quadrant arcus tangent of y / x. Can be used as a filter.
- `sqrt(value)` will return the square root of the input. Can be used as a filter.
- `e` mathematical constant, approximately 2.71828.
- `pi` mathematical constant, approximately 3.14159.
- `tau` mathematical constant, approximately 6.28318.
- Filter `round(x)` will convert the input to a number and round it to `x` decimals. Round has four modes and the default mode (with no mode specified) will [round-to-even](https://en.wikipedia.org/wiki/Rounding#Roundhalfto_even).
  - `round(x, "floor")` will always round down to `x` decimals
  - `round(x, "ceil")` will always round up to `x` decimals
  - `round(1, "half")` will always round to the nearest .5 value. `x` should be 1 for this mode
- Filter `max` will obtain the largest item in a sequence.
- Filter `min` will obtain the smallest item in a sequence.
- Filter `value_one|bitwise_and(value_two)` perform a bitwise and(&) operation with two values.
- Filter `value_one|bitwise_or(value_two)` perform a bitwise or(\|) operation with two values.
- Filter `ord` will return for a string of length one an integer representing the Unicode code point of the character when the argument is a Unicode object, or the value of the byte when the argument is an 8-bit string.

### Regular expressions

- Filter `string|regex_match(find, ignorecase=FALSE)` will match the find expression at the beginning of the string using regex.
- Filter `string|regex_search(find, ignorecase=FALSE)` will match the find expression anywhere in the string using regex.
- Filter `string|regex_replace(find='', replace='', ignorecase=False)` will replace the find expression with the replace string using regex.
- Filter `string|regex_findall_index(find='', index=0, ignorecase=False)` will find all regex matches of find in string and return the match at index (findall returns an array of matches).

## Processing incoming data

The other part of templating is processing incoming data. It allows you to modify incoming data and extract only the data you care about. This will only work for platforms and integrations that mention support for this in their documentation.

It depends per integration or platform, but it is common to be able to define a template using the `value_template` configuration key. When a new value arrives, your template will be rendered while having access to the following values on top of the usual Home Assistant extensions:

| Variable     | Description                        |
|--------------|------------------------------------|
| `value`      | The incoming value.                |
| `value_json` | The incoming value parsed as JSON. |

This means that if the incoming values looks like the sample below:

```json
{
  "on": "true",
  "temp": 21
}
```

The template for `on` would be:

{% raw %}
```yaml
'{{value_json.on}}'
```
{% endraw %}

Nested JSON in a response is supported as well:

```json
{
  "sensor": {
    "type": "air",
    "id": "12345"
  },
  "values": {
    "temp": 26.09,
    "hum": 56.73
  }
}
```

Just use the "Square bracket notation" to get the value.

{% raw %}
```yaml
'{{ value_json['values']['temp'] }}'
```
{% endraw %}

The following overview contains a couple of options to get the needed values:

```text
# Incoming value:
{"primes": [2, 3, 5, 7, 11, 13]}

# Extract third prime number
{% raw %}{{ value_json.primes[2] }}{% endraw %}

# Format output
{% raw %}{{ "%+.1f" | value_json }}{% endraw %}

# Math
{% raw %}{{ value_json | float * 1024 }}{% endraw %}
{% raw %}{{ float(value_json) * (2**10) }}{% endraw %}
{% raw %}{{ value_json | log }}{% endraw %}
{% raw %}{{ log(1000, 10) }}{% endraw %}
{% raw %}{{ sin(pi / 2) }}{% endraw %}
{% raw %}{{ cos(tau) }}{% endraw %}
{% raw %}{{ tan(pi) }}{% endraw %}
{% raw %}{{ sqrt(e) }}{% endraw %}

# Timestamps
{% raw %}{{ value_json.tst | timestamp_local }}{% endraw %}
{% raw %}{{ value_json.tst | timestamp_utc }}{% endraw %}
{% raw %}{{ value_json.tst | timestamp_custom('%Y' True) }}{% endraw %}
```

To evaluate a response, go to the <img src='/images/screenshots/developer-tool-templates-icon.png' alt='template developer tool icon' class="no-shadow" height="38" /> template developer tools, create your output in "Template", and check the result.

{% raw %}
```yaml
{% set value_json=
    {"name":"Outside",
	 "device":"weather-ha",
     "data":
	    {"temp":"24C",
		 "hum":"35%"
		 }	}%}

{{value_json.data.hum[:-1]}}
```
{% endraw %}

## Some more things to keep in mind

### `entity_id` that begins with a number

If your template uses an `entity_id` that begins with a number (example: `states.device_tracker.2008_gmc`) you must use a bracket syntax to avoid errors caused by rendering the `entity_id` improperly. In the example given, the correct syntax for the device tracker would be: `states.device_tracker['2008_gmc']`

### Templates without entities using `now()`

Note that templates that depend on time (`now()`) and do not use any entities will not be updated as it only happens on entity state changes. For more information and examples refer to [`template` sensor documentation](/integrations/template/#working-without-entities)

### Priority of operators

The default priority of operators is that the filter (`|`) has priority over everything except brackets. This means that:

{% raw %}
```yaml
{{ states('sensor.temperature') | float / 10 | round(2) }}
```
{% endraw %}

Would round `10` to 2 decimal places, then divide `states('sensor.temperature')` by that.
