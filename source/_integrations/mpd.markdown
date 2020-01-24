---
title: Music Player Daemon (MPD)
description: Instructions on how to integrate Music Player Daemon into Home Assistant.
logo: mpd.png
ha_category:
  - Media Player
ha_release: pre 0.7
ha_iot_class: Local Polling
ha_codeowners:
  - '@fabaff'
---

The `mpd` platform allows you to control a [Music Player Daemon](https://www.musicpd.org/) from Home Assistant. Unfortunately you will not be able to manipulate the playlist (add or delete songs) or add transitions between the songs.

Even though no playlist manipulation is possible, it is possible to use the play_media service to load an existing saved playlist as part of an automation or scene.

To add MPD to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
media_player:
  - platform: mpd
    host: IP_ADDRESS
```

{% configuration %}
host:
  description: IP address of the Host where Music Player Daemon is running.
  required: true
  type: string
port:
  description: Port of the Music Player Daemon.
  required: false
  type: integer
  default: 6600
name:
  description: Name of your Music Player Daemon.
  required: false
  type: string
  default: MPD
password:
  description: Password for your Music Player Daemon.
  required: false
  type: string
{% endconfiguration %}

Example script to load a saved playlist called "DeckMusic" and set the volume:

```yaml
relaxdeck:
    sequence:
    - service: media_player.play_media
      data:
        entity_id: media_player.main
        media_content_type: playlist
        media_content_id: DeckMusic

    - service: media_player.volume_set
      data:
        entity_id: media_player.main
        volume_level: 0.60
```

This platform works with [Music Player Daemon](https://www.musicpd.org/) and [mopidy](https://www.mopidy.com/) with [Mopidy-MPD](https://docs.mopidy.com/en/latest/ext/mpd/) as used by [Pi MusicBox](https://www.pimusicbox.com/).
