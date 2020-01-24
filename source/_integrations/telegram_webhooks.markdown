---
title: "Telegram webhooks"
description: "Telegram webhooks support"
logo: telegram.png
ha_category:
  - Notifications
ha_release: 0.42
---

Telegram chatbot webhooks implementation as described in the Telegram [documentation](https://core.telegram.org/bots/webhooks).

Using Telegrams `setWebhook` method your bot's webhook URL should be set to `https://<public_url>:<port>/api/telegram_webhooks`.

This is one of two bot implementations supported by Telegram. Described by Telegram as the preferred implementation but requires your Home Assistant instance to be exposed to the internet.

## Configuration

To integrate this into Home Assistant, add the following section to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
http:
  base_url: <public_url> # the Home Assistant https url which is exposed to the internet.

telegram_bot:
  - platform: webhooks
    api_key: YOUR_API_KEY
    parse_mode: html
    allowed_chat_ids:
      - 12345
      - 67890
```

{% configuration %}
allowed_chat_ids:
  description: A list of ids representing the users and group chats that are authorized to interact with the webhook.
  required: true
  type: list
api_key:
  description: The API token of your bot.
  required: true
  type: string
parse_mode:
  description: Default parser for messages if not explicit in message data, either `html` or `markdown`.
  required: false
  default: markdown
  type: string
proxy_url:
  description: Proxy url if working behind one (`socks5://proxy_ip:proxy_port`).
  required: false
  type: string
proxy_params:
  description: Proxy configuration parameters, as dict, if working behind a proxy (`username`, `password`, etc.).
  required: false
  type: string
url:
  description: Allow to overwrite the `base_url` from the [`http`](/integrations/http/) integration for different configurations (`https://<public_url>:<port>`).
  required: false
  type: string
trusted_networks:
  description: Telegram server access ACL as list.
  required: false
  type: string
  default: 149.154.160.0/20, 91.108.4.0/22
{% endconfiguration %}

To get your `chat_id` and `api_key` follow the instructions [here](/integrations/telegram). As well as authorizing the chat, if you have added your bot to a group you will also need to authorize any user that will be interacting with the webhook. When an unauthorized user tries to interact with the webhook Home Assistant will raise an error ("Incoming message is not allowed"), you can easily obtain the users id by looking in the "from" section of this error message.

## Full configuration example

The configuration sample below shows how an entry can look like:

```yaml
# Example configuration.yaml entry
http:
  base_url: <public_url>

telegram_bot:
  - platform: webhooks
    api_key: YOUR_API_KEY
    trusted_networks:
      - 149.154.160.0/20
      - 91.108.4.0/22
    allowed_chat_ids:
      - 12345
      - 67890
```
