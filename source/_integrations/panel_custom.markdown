---
title: Custom Panel
description: Instructions on how to add customized panels to the frontend of Home Assistant.
logo: home-assistant.png
ha_category:
  - Front End
ha_release: 0.26
ha_quality_scale: internal
ha_codeowners:
  - '@home-assistant/frontend'
---

The `panel_custom` support allows you to add additional panels to your Home Assistant frontend. The panels are listed in the sidebar if wished and can be highly customized. See the developer documentation on [instructions how to build your own panels](https://developers.home-assistant.io/docs/en/frontend_creating_custom_panels.html).

To enable customized panels in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
panel_custom:
  - name: my-panel
    sidebar_title: TodoMVC
    sidebar_icon: mdi:work
    url_path: my-todomvc
    js_url: /local/my-panel.js
    config:
      who: world
```

<div class='note'>

Store your custom panels in `<config>/www` to make them available in the frontend at the path `/local`.

</div>

{% configuration %}
name:
  description: Name of the web integration that renders your panel.
  required: true
  type: string
sidebar_title:
  description: Friendly title for the panel in the sidebar. Omitting it means no sidebar entry (but still accessible through the URL).
  required: false
  type: string
sidebar_icon:
  description: Icon for entry. Pick an icon that you can find on [materialdesignicons.com](https://materialdesignicons.com/) to use for your input and prefix the name with `mdi:`. For example `mdi:car`, `mdi:ambulance`, or  `mdi:motorbike`.
  required: false
  default: "mdi:bookmark"
  type: icon
url_path:
  description: The URL your panel will be available on in the frontend. If omitted will default to the panel name.
  required: false
  type: string
js_url:
  description: The URL that contains the JavaScript of your panel. This is exclusive to `module_url` and `webcomponent_path`.
  required: exclusive
  type: string
module_url:
  description: The URL that contains the JavaScript module of your panel. Loaded as a JavaScript module instead of a script. This is exclusive to `js_url` and `webcomponent_path`.
  required: exclusive
  type: string
config:
  description: Configuration to be passed into your web component when being instantiated.
  required: false
  type: list
require_admin:
  description: If admin access is required to see this panel.
  required: false
  type: boolean
  default: false
embed_iframe:
  description: Set to `true` to embed panel in iframe. This is necessary if the panel is using the React framework or if it contains conflicting web components.
  required: false
  default: false
  type: boolean
trust_external_script:
  description: By default the user has to confirm before loading a script from an external source. Setting this to `true` will omit this confirmation.
  required: false
  default: false
  type: boolean
webcomponent_path:
  description: "*DEPRECATED* The HTML path to your component. If omitted will default to `<config dir>/panels/<component name>.html` This is exclusive to `js_url` and `module_url`."
  required: exclusive
  type: string
{% endconfiguration %}
