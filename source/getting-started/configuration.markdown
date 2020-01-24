---
title: "Advanced Configuration"
description: "Instructions to get Home Assistant configured."
---

The onboarding process takes care of the initial setup for Home Assistant, such as naming your home and selecting your location. After initial onboarding, these options can be changed in the user interface by clicking on Configuration in the sidebar and clicking on General, or by manually editing them in the Home Assistant configuration file called `configuration.yaml`. This section will explain how to do the latter.

<div class='note'>

The steps below only apply if you've installed Home Assistant via Hass.io. If you've used another installation method, [see here](/docs/configuration/).

</div>

We are going to help you make your first changes to `configuration.yaml`. To do this, we are going to install an add-on from the Hass.io add-on store: the Configurator. To get to the add-on store, click on the menu icon in the top left, then click on Hass.io. On the new page, open the add-on store tab.

<p class='img'>
<img src='/images/hassio/screenshots/main_panel_addon_store.png' />
From the Hass.io main panel, open the add-on store.
</p>

Under the "Core" section you will find Configurator.

 - Click on Configurator and click on INSTALL. When installation is complete, the UI will go to the add-on details page for the configurator.
 - Now start the add-on by clicking on START.
 - Open the user interface by clicking on OPEN WEB UI.

Now let's make a change using the configurator: we are going to change the name, location, unit system, and time zone of your Home Assistant installation.

 - Click the folder icon in the top left of the configurator window to open the file browser sidebar.
 - Click the `configuration.yaml` file (in the `/config/` folder) to load it into the main Configurator edit window.
 - Add the following to this file (preferably at the very top, but it ultimately doesn't matter):
 ```yaml
     homeassistant:
       name: Home
       latitude: xx.xxxx
       longitude: xx.xxxx
       unit_system: imperial
       time_zone: America/Chicago
  ```
<div class='note'>
 
  Valid options for `unit_system` are `imperial` or `metric`. See [here](https://timezonedb.com/time-zones) for a list of valid time zones. Enter the appropriate option found under the Time Zone column at that page.

</div>

 - Click the save icon in the top right to commit changes.
 - Most changes in `configuration.yaml` require Home Assistant to be restarted to see the changes. You can verify that your changes are acceptable by running a config check. Do this by clicking on Configuration in the sidebar, click on "Server Control" and click on the CHECK CONFIG button. When it's valid, it will show the text "Configuration valid!"
 - Now Restart Home Assistant using the RESTART button in the Server management section on the same page. In order for "Check Config" to be visible, you must enable "Advanced Mode" on your user profile.

<p class='img'>
<img src='/images/screenshots/configuration-validation.png' />
Screenshot of the "General" page in the configuration panel.
</p>

<div class='note'>
 
  If you have watched any videos about setting up Home Assistant using configuration.yaml (particularly ones that are old), you might notice your default configuration file is much smaller than what the videos show. Don't be concerned, you haven't done anything wrong. Many items in the default configuration files shown in those old videos are now included in the `default_config:` line that you see in your configuration file. [See here](/integrations/default_config/) for more information on what's included in that line.

</div>

### Editing config via Samba/Windows Networking

Maybe you are not a big fan of our web editor and want to use a text editor on your computer instead. This is possible by sharing the configuration over the network using the Samba add-on, which can also be installed from the Hass.io add-on store. This will make your configuration accessible via the network tab on your computer.

Go to the add-on store and look for Samba in the core section. After you have installed the add-on, click on START. Hass.io should now be available in the networking tab on your computer.

We suggest that to edit `configuration.yaml`, you use the free text editor [Visual Studio Code](https://code.visualstudio.com/) in combination with the [Home Assistant Config Helper extension](https://marketplace.visualstudio.com/items?itemName=keesschollaart.vscode-home-assistant).
