---
title: hddtemp
description: Instructions on how to integrate hard drive temperature information into Home Assistant.
ha_category:
  - System Monitor
ha_release: 0.32
ha_iot_class: Local Polling
---

The `hddtemp` sensor platform is using the data provided by [HDDTemp](https://savannah.nongnu.org/projects/hddtemp).

It required that `hddtemp` is started or running in daemon mode on a local or remote system.

```bash
$ hddtemp -dF
```

To setup a HDDTemp to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: hddtemp
    disks:
      - /dev/sda1
```

{% configuration %}
name:
  description: Friendly name to use for the frontend.
  required: false
  default: HD Temperature
  type: string
host:
  description: Host where `hddtemp` is running.
  required: false
  default: localhost
  type: string
port:
  description: Port that is used by `hddtemp`.
  required: false
  default: 7634
  type: integer
disks:
  description: "Disk to be monitored. Example: `/dev/sda1`."
  required: false
  type: list
{% endconfiguration %}
