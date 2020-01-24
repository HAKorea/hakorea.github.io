---
title: Home Assistant Device Tracker to support FortiOS
description: Instructions on how to use Fortinet FortiOS to track devices in Home Assistant.
logo: fortinet.jpg
ha_category:
  - Presence Detection
ha_release: 0.97
ha_iot_class: Local Polling
ha_codeowners:
  - '@kimfrellsen'
---

This integration enables Home Assistant to do device tracking of devices with a MAC address connected to a FortiGate from [Fortinet](https://www.fortinet.com).

The integration relies on the [fortiosapi](https://pypi.org/project/fortiosapi/).
The integration has been tested both on FortiGate appliance and FortiGate VM running SW FortiOS v. 6.0.x and 6.2.0.

All devices with a MAC address identified by FortiGate would be tracked, this covers both Ethernet and WiFi devices, including devices detected by LLDP.

The integration is based on the Home Assistant `device_tracker` platform.

```yaml
# Example configuration.yaml entry
device_tracker:
  - platform: fortios
    host: YOUR_HOST
    token: YOUR_API_USER_KEY
```

{% configuration %}
host:
    description: Hostname or IP address of the FortiGate.
    required: true
    type: string
token:
    description: "See [Fortinet Developer Network](https://fndn.fortinet.net) for how to create an API token. Remember this integration only needs read access to a FortiGate, so configure the API user to only to have limited and read-only access."
    required: true
    type: string
verify_ssl:
    description: If the SSL certificate should be verified. In most home cases users do not have a verified certificate.
    required: false
    type: boolean
    default: false
{% endconfiguration %}
