---
title: Sentry
description: Record errors to Sentry.
logo: sentry.svg
ha_category:
  - System Monitor
ha_iot_class: Cloud Polling
ha_release: 0.104
ha_config_flow: true
ha_codeowners:
  - '@dcramer'
---

<div class='note warning'>
  
  The free Sentry account allows 5000 events per month. Depending on the amount of events sent to Sentry you will either have to upgrade your Sentry account or have a period without data flowing from Home Assistant to Sentry.
  
</div>

The `sentry` integration integrates with [Sentry](https://sentry.io/) to capture both logged errors as well as unhandled exceptions in Home Assistant.

## Configuration

To use the `sentry` integration in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sentry:
  dsn: SENTRY_DSN
```

{% configuration %}
dsn:
  description: The DSN provided to you by Sentry.
  required: true
  type: string
environment:
  description: An environment name to associate with events.
  required: false
  type: string
{% endconfiguration %}

### Getting the DSN

Follow these steps to get the DSN:
- Go to **Projects**.
- Click **Create project**.
- Fill out **Give your project a name** and **choose Assign a Team** fields and click Create project button.
- Click **Get your DSN** link in top of the page.
- Your DSN is now visable and looks like https://sdasdasdasdsadsadas@sentry.io/sdsdfsdf
