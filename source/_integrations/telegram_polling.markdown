---
title: "Telegram polling"
description: "Telegram polling support"
logo: telegram.png
ha_category:
  - Notifications
ha_release: 0.42
---

Telegram chatbot polling implementation.

One of two bot implementations supported by Telegram. Your Home Assistant does not have to be exposed to the internet.

## Configuration

To integrate this into Home Assistant, add the following section to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
telegram_bot:
  - platform: polling
    api_key: YOUR_API_KEY
    allowed_chat_ids:
      - 12345
      - 67890
```

{% configuration %}
allowed_chat_ids:
  description: A list of users in the `user_id` Telegram format that are authorized to interact with the webhook.
  required: true
  type: list
api_key:
  description: The API token of your bot.
  required: true
  type: string
parse_mode:
  description: Default parser for messages if not explicit in message data, either `html` or `markdown`.
  required: false
  type: string
  default: "`markdown`"
proxy_url:
  description: Proxy url if working behind one (`socks5://proxy_ip:proxy_port`).
  required: false
  type: string
proxy_params:
  description: Proxy configuration parameters, as dict, if working behind a proxy (`username`, `password`, etc.).
  required: false
  type: string
{% endconfiguration %}

To get your `chat_id` and `api_key` follow the instructions [here](/integrations/telegram).
