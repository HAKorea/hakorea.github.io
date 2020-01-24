---
title: FreeDNS
description: Keep your DNS record up to date with FreeDNS.
logo: afraid_freedns.png
ha_category:
  - Network
ha_release: 0.67
---

With the `freedns` integration you can keep your [FreeDNS](https://freedns.afraid.org) record up to date.

## Setup

You need to determine your update URL or your access token.

1. Head over to the [FreeDNS](https://freedns.afraid.org) website and login to your account.
2. Select the menu "Dynamic DNS"
3. You should now see your update candiates in a table at the bottom of the page.
4. Copy the link target of the "Direct URL".
5. The access token is the part at the end of the link: `https://freedns.afraid.org/dynamic/update.php?YOUR_UPDATE_TOKEN`
6. Either put the token as `access_token` _or_ the whole URL into the `url` attribute.

## Configuration

To use the integration in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
freedns:
  access_token: YOUR_TOKEN
```

{% configuration %}
  access_token:
    description: Your access token. This is exclusive to `url`.
    required: false
    type: string
  url:
    description: The full update URL. This is exclusive to `access_token`.
    required: false
    type: string
  scan_interval:
    description: How often to call the update service.
    required: false
    type: time
    default: 10 minutes
{% endconfiguration %}
