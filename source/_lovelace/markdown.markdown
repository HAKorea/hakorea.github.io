---
title: "Markdown Card"
sidebar_label: Markdown
description: "Markdown card is used to render markdown"
---

Markdown card is used to render [Markdown](https://commonmark.org/help/).

The renderer uses [Marked.js](https://marked.js.org), which supports [several specifications of Markdown](https://marked.js.org/#/README.md#specifications), including CommonMark, GitHub Flavored Markdown (GFM) and `markdown.pl`.
 	 

<p class='img'>
<img src='/images/lovelace/lovelace_markdown.png' alt='Screenshot of the markdown card'>
Screenshot of the markdown card.
</p>

{% configuration %}
type:
  required: true
  description: markdown
  type: string
content:
  required: true
  description: "Content to render as [Markdown](https://commonmark.org/help/). May contain [templates](/docs/configuration/templating/)."
  type: string
title:
  required: false
  description: The card title.
  type: string
  default: none
card_size:
  required: false
  type: integer
  default: none
  description: The algorithm for placing cards aesthetically in lovelace may have problems with the markdown card if it contains templates. You can use this value to help it estimate the height of the card in units of 50 pixels (approximately 3 lines of text in default size). (e.g., `4`)
entity_id:
  required: false
  type: [string, list]
  default: none
  description: "A list of entity IDs so a template in `content:` only reacts to the state changes of these entities. This can be used if the automatic analysis fails to find all relevant entities."
theme:
  required: false
  description: "Set to any theme within `themes.yaml`"
  type: string
{% endconfiguration %}

## Example

```yaml
type: markdown
content: >
  ## Lovelace

  Starting with Home Assistant 0.72, we're experimenting with a new way of defining your interface. We're calling it the **Lovelace UI**.
```

## Template variables

A special template variable - `config` is set up for the `content` of the card. It contains the configuration of the card.

E.g.
{% raw %}
```yaml
type: entity-filter
entities:
  - light.bed_light
  - light.ceiling_lights
  - light.kitchen_lights
state_filter:
  - 'on'
card:
  type: markdown
  content: |
    The lights that are on are:
    {% for l in config.entities %}
      - {{ l.entity }}
    {%- endfor %}

    And the door is {% if is_state('binary_sensor.door', 'on') %} open {% else %} closed {% endif %}.
```
{% endraw %}
