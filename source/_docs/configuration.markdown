---
title: "Configuring Home Assistant"
description: "Configuring Home Assistant."
---

When launched for the first time, Home Assistant will create a default configuration file enabling the web interface and device discovery. It can take up to a minute after startup for your devices to be discovered and appear in the user interface.

The web interface can be found at `http://ip.ad.dre.ss:8123/` - for example if your Home Assistant system has the IP address `192.168.0.40` then you'll find the web interface as `http://192.168.0.40:8123/`.

The location of the folder differs between operating systems:

| OS | Path |
| -- | ---- |
| macOS | `~/.homeassistant` |
| Linux | `~/.homeassistant` |
| Windows | `%APPDATA%/.homeassistant` |
| Hass.io | `/config` |
| Docker | `/config` |

If you want to use a different folder for configuration, use the config command line parameter: `hass --config path/to/config`.

Inside your configuration folder is the file `configuration.yaml`. This is the main file that contains integrations to be loaded with their configurations. Throughout the documentation you will find snippets that you can add to your configuration file to enable functionality.

If you run into trouble while configuring Home Assistant, have a look at the [configuration troubleshooting page](/getting-started/troubleshooting-configuration/) and at the [configuration.yaml examples](/cookbook/#example-configurationyaml).

<div class='note tip'>

  Test any changes to your configuration files from the command line with `hass --script check_config`. This script allows you to test changes without the need to restart Home Assistant. Remember to run this as the user you run Home Assistant as.

</div>

## Reloading changes

You will have to restart Home Assistant for most changes to `configuration.yaml` to take effect.
You can load changes to [automations](/docs/automation/), [core (customize)](/docs/configuration/customizing-devices/), [groups](/integrations/group/), and [scripts](/integrations/script/) without restarting.

<div class='note warning'>

If you've made any changes, remember to [check your configuration](/docs/configuration/troubleshooting/#problems-with-the-configuration) before trying to reload or restart.

</div>

## Migrating to a new system

If you want to migrate your configuration to a new system then you can copy the contents of your configuration folder from the current system to the new system. Be aware that some of the files you need start with `.`, which is hidden by default from both `ls` (in SSH), in Windows Explorer, and macOS Finder. You'll need to ensure that you're viewing all files before you copy them.
