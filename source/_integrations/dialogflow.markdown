---
title: Dialogflow
description: Instructions on how integrate Dialogflow with Home Assistant.
logo: dialogflow.png
ha_category:
  - Voice
ha_release: 0.56
ha_config_flow: true
---

The `dialogflow` integration is designed to be used with the [webhook](https://dialogflow.com/docs/fulfillment#webhook) integration of [Dialogflow](https://dialogflow.com/). When a conversation ends with a user, Dialogflow sends an action and parameters to the webhook.

To be able to receive messages from DialogFlow, your Home Assistant instance needs to be accessible from the web ([Hass.io instructions](/addons/duckdns/)) and you need to have the `base_url` configured for the HTTP integration ([docs](/integrations/http/#base_url)). Dialogflow will return fallback answers if your server does not answer or takes too long (more than 5 seconds).

Dialogflow could be [integrated](https://dialogflow.com/docs/integrations/) with many popular messaging, virtual assistant and IoT platforms.

Using Dialogflow will be easy to create conversations like:

_User: What is the temperature at home?_

_Bot: The temperature is 34 degrees_

_User: Turn on the light_

_Bot: In which room?_

_User: In the kitchen_

_Bot: Turning on kitchen light_

To use this integration, you should define a conversation (intent) in Dialogflow, configure Home Assistant with the speech to return and, optionally, the action to execute.

### Configuring your Dialogflow account

To get the webhook URL, go to the integrations page in the configuration screen and find "Dialogflow". Click on "configure". Follow the instructions on the screen.

- [Login](https://console.dialogflow.com/) with your Google account
- Click on "Create Agent"
- Select name, language (if you are planning to use Google Actions check their [supported languages](https://support.google.com/assistant/answer/7108196?hl=en)) and time zone
- Click "Save"
- Now go to "Fulfillment" (in the left menu)
- Enable Webhook and set your Dialogflow webhook url as the endpoint, e.g., `https://myhome.duckdns.org/api/webhook/800b4cb4d27d078a8871656a90854a292651b20635685f8ea23ddb7a09e8b417`
- Click "Save"
- Create a new intent
- Below "User says" write one phrase that you, the user, will tell Dialogflow, e.g., `What is the temperature at home?`
- In "Action" set some key (this will be the bind with Home Assistant configuration), e.g.,: GetTemperature
- In "Response" set "Cannot connect to Home Assistant or it is taking to long" (fall back response)
- At the end of the page, click on "Fulfillment" and check "Use webhook"
- Click "Save"
- On the top right, where is written "Try it now...", write, or say, the phrase you have previously defined and hit enter
- Dialogflow has send a request to your Home Assistant server

<div class='note warning'>

  The V1 api will be deprecated on October 23, 2019. If you are still using the V1 API, it is recommended to change your settings in Dialogflow to use the V2 API. No changes to your intents yaml configuration need to take place after upgrading to the V2 API. Change to the V2 API by clicking on the cog button [here](https://console.dialogflow.com/) and then select the V2 API.

</div>

Take a look to "Integrations", in the left menu, to configure third parties.

### Configuring Home Assistant

When activated, the [`alexa` integration](/integrations/alexa/) will have Home Assistant's native intent support handle the incoming intents. If you want to run actions based on intents, use the [`intent_script`](/integrations/intent_script) integration.

## Examples

Download [this zip](https://github.com/home-assistant/home-assistant.io/blob/next/source/assets/HomeAssistant_APIAI.zip) and load it in your Dialogflow agent (**Settings** -> **Export and Import**) for examples intents to use with this configuration:

{% raw %}
```yaml
# Example configuration.yaml entry
dialogflow:

intent_script:
  Temperature:
    speech:
      text: The temperature at home is {{ states('sensor.home_temp') }} degrees
  LocateIntent:
    speech:
      text: >
        {%- for state in states.device_tracker -%}
          {%- if state.name.lower() == User.lower() -%}
            {{ state.name }} is at {{ state.state }}
          {%- elif loop.last -%}
            I am sorry, I do not know where {{ User }} is.
          {%- endif -%}
        {%- else -%}
          Sorry, I don't have any trackers registered.
        {%- endfor -%}
  WhereAreWeIntent:
    speech:
      text: >
        {%- if is_state('device_tracker.adri', 'home') and
               is_state('device_tracker.bea', 'home') -%}
          You are both home, you silly
        {%- else -%}
          Bea is at {{ states("device_tracker.bea") }}
          and Adri is at {{ states("device_tracker.adri") }}
        {% endif %}
  TurnLights:
    speech:
      text: Turning {{ Room }} lights {{ OnOff }}
    action:
      - service: notify.pushbullet
        data_template:
          message: Someone asked via apiai to turn {{ Room }} lights {{ OnOff }}
      - service_template: >
          {%- if OnOff == "on" -%}
            switch.turn_on
          {%- else -%}
            switch.turn_off
          {%- endif -%}
        data_template:
          entity_id: "switch.light_{{ Room | replace(' ', '_') }}"
```
{% endraw %}
