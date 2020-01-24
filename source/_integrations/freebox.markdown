---
title: Freebox
description: Instructions on how to integrate Freebox routers into Home Assistant.
logo: freebox.svg
ha_category:
  - Network
  - Presence Detection
  - Sensor
  - Switch
ha_release: 0.85
ha_iot_class: Local Polling
ha_codeowners:
  - '@snoof85'
---

The `freebox` integration allows you to observe and control [Freebox router](https://www.free.fr/).

There is currently support for the following device types within Home Assistant:

* [Sensor](#sensor) with traffic metrics
* [Device tracker](#presence-detection) for connected devices
* [Switch](#switch) to control WiFi

## Configuration

If you have enabled the [discovery component](/integrations/discovery/),
your Freebox should be detected automatically. Otherwise, you can set it
up manually in your `configuration.yaml` file:

```yaml
freebox:
  host: foobar.fbxos.fr
  port: 1234
```

{% configuration %}
host:
  description: The url of the Freebox.
  required: true
  type: string
port:
  description: The https port the Freebox is listening on.
  required: true
  type: string
{% endconfiguration %}

You can find out your Freebox host and port by opening the address <http://mafreebox.freebox.fr/api_version> in your browser. The
returned json should contain an api_domain (`host`) and a https_port (`port`).
Please consult the [api documentation](https://dev.freebox.fr/sdk/os/) for more information.

<div class='note warning'>
  
If you change your Freebox router for a new one, you need to delete the `freebox.conf` file located in your Home Assistant configuration directory to make the association again.

</div>

### Initial setup

<div class='note warning'>
You must have set a password for your Freebox router web administration page. Enable the option "Permettre les nouvelles demandes d'associations" and check that the option "Accès à distance sécurisé à Freebox OS" is active in "Gestion des ports" > "Connexions entrantes".
</div>

The first time Home Assistant will connect to your Freebox, you will need to
authorize it by pressing the right arrow on the facade of the Freebox when
prompted to do so.

To make the WiFi switch and the reboot service working you will have to add "Modification des réglages de la Freebox
" permission to Home Assistant application in "Paramètres de la Freebox" > "Gestion des accès" > "Applications".

### Supported routers

Only the routers with Freebox OS are supported:

* Freebox V7 also known as Freebox Delta
* Freebox V6 also known as Freebox Revolution
* Freebox mini 4k

## Presence Detection

This platform offers presence detection by keeping track of the
devices connected to a [Freebox](https://www.free.fr/) router.

### Notes

Note that the Freebox waits for some time before marking a device as
inactive, meaning that there will be a small delay (1 or 2 minutes)
between the time you disconnect a device and the time it will appear
as "away" in Home Assistant. You should take this into account when specifying
the `consider_home` parameter.
On the contrary, the Freebox immediately reports devices newly connected, so
they should appear as "home" almost instantly, as soon as Home Assistant
refreshes the devices states.

## Sensor

This platform offers you sensors to monitor a Freebox router. The monitored conditions are
instant upload and download rates in KB/s.

## Service

### Service `freebox.reboot`

This service will reboot your Freebox router. It does not take any parameter. Be aware there is no confirmation.

## Switch

This platform offers you a switch to toggle the Wifi on or off. This will toggle all WiFi interfaces of the router (all SSID and all bands).
