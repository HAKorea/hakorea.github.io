---
title: Discovery
description: Instructions on how to setup Home Assistant to discover new devices.
logo: home-assistant.png
ha_category:
  - Other
ha_release: 0.7
ha_quality_scale: internal
---

Home Assistant can discover and automatically configure [zeroconf](https://en.wikipedia.org/wiki/Zero-configuration_networking)/[mDNS](https://en.wikipedia.org/wiki/Multicast_DNS) and [uPnP](https://en.wikipedia.org/wiki/Universal_Plug_and_Play) devices on your network. Currently the `discovery` integration can detect:

 * [Apple TV](/integrations/apple_tv/)
 * [Belkin WeMo switches](/integrations/wemo/)
 * [Bluesound speakers](/integrations/bluesound)
 * [Bose Soundtouch speakers](/integrations/soundtouch)
 * [Denon network receivers](/integrations/denonavr/)
 * [DirecTV receivers](/integrations/directv)
 * [DLNA DMR enabled devices](/integrations/dlna_dmr)
 * [Enigma2 media player](/integrations/enigma2)
 * [Frontier Silicon internet radios](/integrations/frontier_silicon)
 * [Google Cast](/integrations/cast)
 * [Linn / Openhome](/integrations/openhome)
 * [Logitech Harmony Hub](/integrations/harmony)
 * [Logitech media server (Squeezebox)](/integrations/squeezebox)
 * [Netgear routers](/integrations/netgear)
 * [Panasonic Viera](/integrations/panasonic_viera)
 * [Philips Hue](/integrations/hue)
 * [Plex media server](/integrations/plex#media-player)
 * [Roku media player](/integrations/roku#media-player)
 * [SABnzbd downloader](/integrations/sabnzbd)
 * [Samsung SyncThru Printer](/integrations/syncthru)
 * [Samsung TVs](/integrations/samsungtv)
 * [Sonos speakers](/integrations/sonos)
 * [Telldus Live](/integrations/tellduslive/)
 * [Wink](/integrations/wink/)
 * [Yamaha media player](/integrations/yamaha)
 * [Yeelight Sunflower bulb](/integrations/yeelightsunflower/)
 * [Xiaomi Gateway (Aqara)](/integrations/xiaomi_aqara/)

It will be able to add Google Chromecasts and Belkin WeMo switches automatically,
for Philips Hue it will require some configuration from the user.

<div class='note'>

Zeroconf discoverable integrations [Axis](/integrations/axis/)/[ESPHome](/integrations/esphome/)/[HomeKit](/integrations/homekit_controller/)/[Tradfri](/integrations/tradfri/) have been migrated to use [zeroconf](/integrations/zeroconf) integration to initiate discovery.

</div>

To load this integration, add the following lines to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
discovery:
  ignore:
    - sonos
    - samsung_tv
  enable:
    - homekit
```

{% configuration discovery %}
ignore:
  description: A list of platforms that never will be automatically configured by `discovery`.
  required: false
  type: list
enable:
  description: A list of platforms not enabled by default that `discovery` should discover.
  required: false
  type: list
{% endconfiguration %}

Valid values for ignore are:

 * `apple_tv`: Apple TV
 * `belkin_wemo`: Belkin WeMo switches
 * `bluesound`: Bluesound speakers
 * `bose_soundtouch`: Bose Soundtouch speakers
 * `denonavr`: Denon network receivers
 * `directv`: DirecTV receivers
 * `enigma2`: Enigma2 media players
 * `frontier_silicon`: Frontier Silicon internet radios
 * `google_cast`: Google Cast
 * `harmony`: Logitech Harmony Hub
 * `igd`: Internet Gateway Device
 * `logitech_mediaserver`: Logitech media server (Squeezebox)
 * `netgear_router`: Netgear routers
 * `octoprint`: Octoprint
 * `openhome`: Linn / Openhome
 * `panasonic_viera`: Panasonic Viera
 * `philips_hue`: Philips Hue
 * `plex_mediaserver`: Plex media server
 * `roku`: Roku media player
 * `sabnzbd`: SABnzbd downloader
 * `samsung_printer`: Samsung SyncThru Printer
 * `samsung_tv`: Samsung TVs
 * `sonos`: Sonos speakers
 * `songpal` : Songpal
 * `tellstick`: Telldus Live
 * `wink`: Wink Hub
 * `yamaha`: Yamaha media player
 * `yeelight`: Yeelight lamps and bulbs (not only Yeelight Sunflower bulb)
 * `xiaomi_gw`: Xiaomi Aqara gateway

Valid values for enable are:

 * `dlna_dmr`: DLNA DMR enabled devices

## Troubleshooting

### UPnP

Home Assistant must be on the same network as the devices for uPnP discovery to work.
If running Home Assistant in a [Docker container](/docs/installation/docker/) use switch `--net=host` to put it on the host's network.

### Windows

#### 64-bit Python
There is currently a <a href='https://bitbucket.org/al45tair/netifaces/issues/17/dll-fails-to-load-windows-81-64bit'>known issue</a> with running this integration on a 64-bit version of Python and Windows.

### could not install dependency netdisco

If you see `Not initializing discovery because could not install dependency netdisco==0.6.1` in the logs, you will need to install the `python3-dev` or `python3-devel` package on your system manually (eg. `sudo apt-get install python3-dev` or `sudo dnf -y install python3-devel`). On the next restart of Home Assistant, the discovery should work. If you still get an error, check if you have a compiler (`gcc`) available on your system.

### DSM and Synology

For DSM/Synology, install via debian-chroot [see this forum post](https://community.home-assistant.io/t/error-starting-home-assistant-on-synology-for-first-time/917/15).
