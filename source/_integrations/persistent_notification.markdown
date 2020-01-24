---
title: Persistent Notification
description: Instructions on how to integrate persistent notifications into Home Assistant.
logo: home-assistant.png
ha_category:
  - Other
ha_release: 0.23
ha_quality_scale: internal
ha_codeowners:
  - '@home-assistant/core'
---

The `persistent_notification` integration can be used to show a notification on the frontend that has to be dismissed by the user.

<p class='img'>
  <img src='/images/screenshots/persistent-notification.png' />
</p>

### Service

The service `persistent_notification.create` takes in `message`, `title`, and `notification_id`.

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `message`              |       no | Body of the notification.
| `title`                |      yes | Title of the notification.
| `notification_id`      |      yes | If `notification_id` is given, it will overwrite the notification if there already was a notification with that ID.

The `persistent_notification` integration supports specifying [templates](/topics/templating/) for both the `message` and the `title`. This will allow you to use the current state of Home Assistant in your notifications.

In an [action](/getting-started/automation-action/) of your [automation setup](/getting-started/automation/) it could look like this with a customized subject.

```yaml
action:
  service: persistent_notification.create
  data:
    message: "Your message goes here"
    title: "Custom subject"
```

The service `persistent_notification.dismiss` requires a `notification_id`.

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `notification_id`      |      no  | the `notification_id` is required to identify the notification that should be removed.

This service allows you to remove a notifications by script or automation.

```yaml
action:
  service: persistent_notification.dismiss
  data:
    notification_id: "1234"
```

This automation example shows a notification when the Z-Wave network is starting and removes it when the network is ready.

```yaml
- alias: 'Z-Wave network is starting'
  trigger:
    - platform: event
      event_type: zwave.network_start
  action:
    - service: persistent_notification.create
      data:
        title: "Z-Wave"
        message: "Z-Wave network is starting..."
        notification_id: zwave

- alias: 'Z-Wave network is ready'
  trigger:
    - platform: event
      event_type: zwave.network_ready
  action:
    - service: persistent_notification.dismiss
      data:
        notification_id: zwave
```

### Markdown support

The message attribute supports the [Markdown formatting syntax](https://daringfireball.net/projects/markdown/syntax). Some examples are:

| Type | Message |
| ---- | ------- |
| Headline 1 | `# Headline` |
| Headline 2 | `## Headline` |
| Newline | `\n` |
| Bold | `**My bold text**` |
| Cursive | `*My cursive text*` |
| Link | `[Link](https://home-assistant.io/)` |
| Image | `![image](/local/my_image.jpg)` |

<div class="note">

  `/local/` in this context refers to the `.homeassistant/www/` folder.

</div>

### Create a persistent notification

Choose the **Services** tab from the **Developer Tools** sidebar item, then select the `persistent_notification.create` service from the "Service" dropdown. Enter something like the sample below into the **Service Data** field and press the **CALL SERVICE** button.

```json
{
  "notification_id": "1234",
  "title": "Sample notification",
  "message": "This is a sample text"
}
```
This will create the notification entry shown above.
