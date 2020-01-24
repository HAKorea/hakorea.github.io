---
title: HomeKit Controller
description: Instructions for how to integrate your HomeKit devices within Home Assistant.
logo: apple-homekit.png
ha_category:
  - Hub
  - Alarm
  - Climate
  - Cover
  - Light
  - Lock
  - Switch
  - Binary Sensor
  - Sensor
  - Fan
  - Health
ha_release: 0.68
ha_iot_class: Local Polling
ha_config_flow: true
ha_codeowners:
  - '@Jc2k'
---

The [HomeKit](https://developer.apple.com/homekit/) controller integration allows you to connect accessories with the "Works with HomeKit" logo to Home Assistant. This integration should not be confused with the [HomeKit](/integrations/homekit/) integration, which allows you to control Home Assistant devices via HomeKit.

The integration will automatically detect HomeKit compatible devices that are ready to pair if the [`zeroconf`](/integrations/zeroconf/) integration is enabled. This is enabled by default on new installations via the [`default_config`](/integrations/default_config/) component.

To see which devices have been discovered see the "Integrations" page in your Home Assistant dashboard. When you click on "Configure" you can enter your HomeKit PIN and the device should be added to your Home Assistant instance. If your device is currently paired with an Apple device via HomeKit, you will need to reset it in order to pair it with Home Assistant. Once Home Assistant is configured to work with the device, you can export it back to Siri and Apple Home with the [`HomeKit`](/integrations/homekit/) integration.

## Supported devices

There is currently support for the following device types within Home Assistant:

- Alarm Control Panel (HomeKit security system)
- Climate (HomeKit thermostats and air conditioners)
- Cover (HomeKit garage door openers, windows, or window coverings)
- Light (HomeKit lights)
- Lock (HomeKit lock)
- Switch (HomeKit switches)
- Binary Sensor (HomeKit motion, contact and smoke sensors)
- Sensor (HomeKit humidity, temperature, co2 and light level sensors)
- Fan
- Air Quality

HomeKit IP accessories for these device types may work with some caveats:

- If the device is WiFi based and has no physical controls or screen then you may need an Apple HomeKit device like an iPhone or iPad to get the accessory onto your WiFi network. For example, for a Koogeek LS1 you must add the accessory to HomeKit on your iOS device, then remove it from the iOS device. This leaves the LS1 in an unpaired state but still on your WiFi. Home Assistant can then find it and pair with it.
- You need to know the HomeKit PIN. There is no way to recover this if you do not have it. In this case, you will need to contact the manufacturer to see what options you have.

Home Assistant does not currently support HomeKit BLE.

<div class='note'>

  HomeKit Controller might detect some HomeKit devices on your network that are not currently supported by this integration. One such example is HomeKit displays (such as Vizio TVs which have been updated with HomeKit support). These devices might pair with Home Assistant if you go through the pairing process, but no entity will be created, and you won't be able to control these devices with Home Assistant. However, pairing the device with Home Assistant *will* remove the persistent notification generated when Home Assistant finds new devices.

</div>

## Troubleshooting

### I don't have a HomeKit PIN

When you buy a certified HomeKit-enabled device, the PIN might be in the instructions or on a sticker on the accessory itself.

Devices with screens like thermostats may not have PIN codes in the packaging at all. Every time you click on "Configure" in the Home Assistant frontend, your accessory will generate a new pairing code and show it on the display.

If your device doesn't have a display and received HomeKit support after it was released, you may not have a pairing code. Dealing with this is manufacturer specific. Some manufacturers allow you to see the pairing code in their iOS app. Others force you to use their app to configure HomeKit and don't let you have the pairing pin - right now you won't be able to use HomeKit Controller with those devices.

If you have lost your PIN code, then you may not be able to pair your accessory. You should contact the manufacturer to see if there is anything you can do.

### My accessory isn't updating straight away

This is normal - HomeKit controller is currently a local polling based integration. It polls your accessory for its latest state once per minute.

### Home Assistant cannot discover my device

For IP accessories, Home Assistant can only find devices that are already on the same network as your device. If an accessory is WiFi based and has no user interface for joining it to your Wifi network, you will need an Apple HomeKit controller device (an iPhone or iPad). You should pair it with the controller and then remove the pairing in the UI (but do not reset the accessory itself). This will leave the accessory on your WiFi network but in an unpaired state, and then Home Assistant can find it.

Home Assistant can only find accessories that aren't already paired. Even if you reset your Home Assistant configuration, the accessory will still think it is paired and you won't be able to use it with Home Assistant. You should reset the accessory according to the manufacturer's instructions. Some devices have a "Reset HomeKit" option, and some may require a full reset.

### HomeKit controller is finding devices on my network even though I don't have any Apple devices

This is completely normal. Unlike many other commercial IoT offerings, the HomeKit protocol is a local and offline protocol that does not rely on the Apple ecosystem to function. You do not need an Apple online account to use a "Works with HomeKit" device. Some WiFi devices may need an iOS device briefly to get them onto your WiFi, but other than that you do not need any Apple hardware on your network.

Many IoT devices are getting a post-launch HomeKit upgrade. This might mean your device starts showing in Home Assistant as a `homekit_controller` device even though when you bought it without HomeKit support. This might be a better choice for you than a native integration. For example, many climate devices have an online-only API and a HomeKit API. The HomeKit one might not expose all of the settings and controls you are used to, but it also won't break if your Internet connection goes down or the cloud service goes away.

### I have a warning in my logs about HomeKit controller skipping updates

You may say a log entry that looks like this:

```log
HomeKit controller update skipped as previous poll still in flight
```

In these cases it's unlikely that HomeKit controller itself is directly responsible. This is a safety feature to avoid overloading your Home Assistant instance. It means that Home Assistant tried to poll your accessory but the previous poll was still happening. This means it is taking over 1 minute to poll your accessory. This could be caused by a number of things:

- You have too many blocking synchronous integrations for your Home Assistant instance. All synchronous integrations share a thread pool, and if there are lots of tasks to run on it they will queued, which will cause delays. In the worst cases this queue can build up faster than it can be emptied. Faster hardware may help, but you may need to disable some integrations.
- Your network connection to an accessory is poor and HomeKit Controller is unable to reach the accessory reliably. This will likely require a change to your network setup to improve WiFi coverage or replace damaged cabling.
- There is a problem with the accessory itself which is causing intermittent network issues.

In these cases, HomeKit Controller will skip polling to avoid a buildup of back pressure in your instance.
