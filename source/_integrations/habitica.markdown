---
title: Habitica
description: Instructions on enabling Habitica support for your Home Assistant
logo: habitica.png
ha_category:
  - Hub
  - Sensor
ha_release: 0.78
ha_iot_class: Cloud Polling
---

This integration allows you to monitor and manage your Habitica profile. This integration exposes the [Habitica's API](https://habitica.com/apidoc/) as a Home Assistant service. It supports multiple users and allows you to automate checking out your habits and daily tasks or casting magics using Home Assistant.

There is currently support for the following device types within Home Assistant:

- Sensor - Allows you to view and monitor your player data from [Habitica](https://habitica.com/) in Home Assistant.

The sensors will automatically appear, after setup the Habitica component.

To use the integration you should use this example configuration:

```yaml
# Minimum viable configuration.yaml entry
habitica:
  - api_user: YOUR_USER_ID
    api_key: YOUR_API_KEY
```

You can specify several users, providing `api_user` and `api_key` for each.
At runtime you will be able to use API for each respective user by their Habitica's username.
You can override this by passing `name` key, this value will be used instead of the username.
If you are hosting your own instance of Habitica, you can specify a URL to it in `url` key.

{% configuration %}
api_user:
  description: "Habitica's API user ID. This value can be grabbed from [account setting](https://habitica.com/user/settings/api)"
  required: true
  type: string
api_key:
  description: "Habitica's API password (token). This value can be grabbed from [account setting](https://habitica.com/user/settings/api) by pressing 'Show API token'"
  required: true
  type: string
name:
  description: "Override for Habitica's username. Will be used for service calls"
  required: false
  type: string
  default: Deduced at startup
url:
  description: "URL to your Habitica instance, if you are hosting your own"
  required: false
  type: string
  default: https://habitica.com
sensors:
  description: List of sensors to generate for this user. If you don't specify this entry then the default (all sensors) will be generated. If you specify this entry empty then no sensors will be generated.
  required: false
  type: list
  default: all (`name`, `hp`, `maxHealth`, `mp`, `maxMP`, `exp`, `toNextLevel`, `lvl`, `gp`, `class`)
{% endconfiguration %}

### API Service Parameters

The API is exposed to Home Assistant as a service called `habitica.api_call`. To call it you should specify this keys in service data:

| Service data attribute | Required | Type     |    Description  |
|----------------------|--------|--------|----------------|
|  `name`                |  yes     | string   |  Habitica's username as per `configuration.yaml` entry. |
| `path` | yes | [string] | Items from API URL in form of an array with method attached at the end. See the example below. |
| `args` | no | map | Any additional json or url parameter arguments. See the example below and [apidoc](https://habitica.com/apidoc/). |

A successful call to this service will fire an event `habitica_api_call_success`.

| Event data attribute |  Type     |    Description  |
|----------------------|--------|----------------|
|  `name`                |   string   |  Copied from service data attribute. |
| `path` | [string] | Copied from service data attribute. |
| `data` | map | Deserialized `data` field of JSON object Habitica's server returned in response to api call. For more info see [docs](https://habitica.com/apidoc/). |

#### Let's consider some examples on how to call the service.

For example, let's say that there is a configured `habitica` platform for user `xxxNotAValidNickxxx` with their respective `api_user` and `api_key`.
Let's create a new task (a todo) for this user via Home Assistant. There is an [API call](https://habitica.com/apidoc/#api-Task-CreateUserTasks) for this purpose.
To create a new task one should hit `https://habitica.com/api/v3/tasks/user` endpoint with `POST` request with a json object with task properties.
So let's call the API on `habitica.api_call`.

* The `name` key becomes `xxxNotAValidNickxxx`.
* The `path` key is trickier.
  * Remove `https://habitica.com/api/v3/` at the beginning of the endpoint URL.
  * Split the remaining on slashes (/) and **append the lowercase method** at the end.
  * You should get `["tasks", "user", "post"]`. To get a better idea of the API you are recommended to try all of the API calls in IPython console [using this package](https://github.com/ASMfreaK/habitipy/blob/master/README.md).
* The `args` key is more or less described in the [docs](https://habitica.com/apidoc/).

Combining all together:
call `habitica.api_call` with data

```json
{
  "name": "xxxNotAValidNickxxx",
  "path": ["tasks", "user", "post"],
  "args": {"text": "Use API from Home Assistant", "type": "todo"}
}
```

This call will create a new todo on `xxxNotAValidNickxxx`'s account with text `Use API from Home Assistant` like this:

![example task created](/images/screenshots/habitica_new_task.png)

Also an event `habitica_api_call_success` will be fired with the following data:

```json
{
  "name": "xxxNotAValidNickxxx",
  "path": ["tasks", "user", "post"],
  "data": {
    "challenge": {},
    "group": {"approval": {"required": false,
     "approved": false,
     "requested": false},
    "assignedUsers": [],
    "sharedCompletion": "recurringCompletion"},
    "completed": false,
    "collapseChecklist": false,
    "type": "todo",
    "notes": "",
    "tags": [],
    "value": 0,
    "priority": 1,
    "attribute": "str",
    "text": "Use API from Home Assistant",
    "checklist": [],
    "reminders": [],
    "_id": "NEW_TASK_UUID",
    "createdAt": "2018-08-09T18:03:27.759Z",
    "updatedAt": "2018-08-09T18:03:27.759Z",
    "userId": "xxxNotAValidNickxxx's ID",
    "id": "NEW_TASK_UUID"}
}
```
