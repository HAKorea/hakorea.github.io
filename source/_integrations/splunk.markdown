---
title: Splunk
description: Record events in Splunk.
logo: splunk.png
ha_category:
  - History
ha_release: 0.13
---

The `splunk` integration makes it possible to log all state changes to an external [Splunk](https://splunk.com/) database using Splunk's HTTP Event Collector (HEC) feature. You can either use this alone, or with the Home Assistant for Splunk [app](https://github.com/miniconfig/splunk-homeassistant). Since the HEC feature is new to Splunk, you will need to use at least version 6.3.

## Configuration

To use the `splunk` integration in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
splunk:
  token: YOUR_SPLUNK_TOKEN
```

{% configuration %}
token:
  description: The HTTP Event Collector Token already created in your Splunk instance.
  required: true
  type: string
host:
  description: "IP address or host name of your Splunk host, e.g., 192.168.1.10."
  required: false
  default: localhost
  type: string
port:
  description: Port to use.
  required: false
  default: 8080
  type: integer
ssl:
  description: Use HTTPS instead of HTTP to connect.
  required: false
  default: false
  type: boolean
verify_ssl:
  description: Allows you do disable checking of the SSL certificate.
  required: false
  default: false
  type: boolean
name:
  description: This parameter allows you to specify a friendly name to send to Splunk as the host, instead of using the name of the HEC.
  required: false
  default: HASS
  type: string
filter:
  description: Filters for entities to be included/excluded from Splunk. Default is to include all entities.
  required: false
  type: map
  keys:
    include_domains:
      description: Domains to be included.
      required: false
      type: list
    include_entities:
      description: Entities to be included.
      required: false
      type: list
    exclude_domains:
      description: Domains to be excluded.
      required: false
      type: list
    exclude_entities:
      description: Entities to be excluded.
      required: false
      type: list
{% endconfiguration %}
