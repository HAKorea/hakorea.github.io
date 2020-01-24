---
title: Logitech Squeezebox
description: Instructions on how to integrate a Logitech Squeezebox player into Home Assistant.
logo: squeezebox.png
ha_category:
  - Media Player
ha_release: pre 0.7
ha_iot_class: Local Polling
---

The `squeezebox` platform allows you to control a [Logitech Squeezebox](https://en.wikipedia.org/wiki/Squeezebox_%28network_music_player%29) music player from Home Assistant. This lets you control Squeezebox hardware like the Classic, Transporter, Duet, Boom, Radio and Touch and of software players like [SoftSqueeze](http://softsqueeze.sourceforge.net/), [SqueezePlayer](https://play.google.com/store/apps/details?id=de.bluegaspode.squeezeplayer) and [SqueezeSlave](https://forums.slimdevices.com/showthread.php?93607-ANNOUNCE-Squeezeslave-1-2-released).

To add your Squeezebox player to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
media_player:
  - platform: squeezebox
    host: IP_ADDRESS
```

{% configuration %}
host:
  description: The host name or address of the Logitech Media Server, e.g., 192.168.1.21.
  required: true
  type: string
port:
  description: The web interface port to Logitech Media Server.
  required: false
  default: 9000
  type: integer
username:
  description: The username, if password protection is enabled.
  required: false
  type: string
password:
  description: The password, if password protection is enabled.
  required: false
  type: string
{% endconfiguration %}

<div class='note'>
This platform now uses the web interface of the Logitech Media Server to send commands. The default port of the web interface is 9000. It is the same port that you use to access the LMS through your web browser. Originally, this platform used the telnet interface, which defaults to 9090. If you previously specified the port in your configuration file, you will likely need to update it.
</div>

The Logitech Transporter which have two digital inputs can be activated using a script. The following example turns on the Transporter and activates the toslink input interface:

```yaml
# Turn on Transporter and activate toslink interface
transporter_toslink:
  sequence:
    - service: homeassistant.turn_on
      entity_id: media_player.transporter
    - service: media_player.play_media
      data:
        entity_id: media_player.transporter
        media_content_id: "source:toslink"
        media_content_type: "music"
```

### Service `call_method`

Call a custom Squeezebox JSONRPC API.

See documentation for this interface on `http://HOST:PORT/html/docs/cli-api.html?player=` where HOST and PORT are the host name and port for your Logitech Media Server.

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `entity_id` | no | Name(s) of the Squeezebox entities where to run the API method.
| `command` | no | Command to pass to Logitech Media Server (p0 in the CLI documentation).
| `parameters` | yes | Array of additional parameters to pass to Logitech Media Server (p1, ..., pN in the CLI documentation).

This service can be used to integrate any Squeezebox action to an automation.

It can also be used to target a Squeezebox from IFTT (or DialogFlow, Alexa...).

For example, to play an album from your collection, create an IFTT applet like this:
- Trigger: Google assistant, with sentence: `I want to listen to album $`
- Action: JSON post query with such JSON body:
`{ "entity_id": "media_player.squeezebox_radio", "command": "playlist", "parameters": ["loadtracks", "album.titlesearch={{TextField}}"] }`
This can work with title search and basically any thing. The same wouldn't have worked by calling directly Squeezebox server as IFTT cannot escape the text field.
