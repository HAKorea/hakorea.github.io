---
title: Graphite
description: Instructions on how to record Home Assistant history in Graphite.
logo: graphite.png
ha_category:
  - History
ha_release: 0.13
---

The `graphite` integration records all events and state changes and feeds the data to a [graphite](http://graphite.wikidot.com/) instance.

To enable this component, add the following lines to your `configuration.yaml`:

```yaml
# Example configuration.yaml entry
graphite:
```

{% configuration %}
host:
  description: IP address of your graphite host, e.g., 192.168.1.10.
  required: false
  type: string
  default: localhost
port:
  description: This is a description of what this key is for.
  required: false
  type: integer
  default: 2003
prefix:
  description: Prefix is the metric prefix in graphite.
  required: false
  type: string
  default: ha
{% endconfiguration %}
