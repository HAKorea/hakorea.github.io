---
title: "출현 감지 설정"
description: "Instructions on how to setup presence detection within Home Assistant."
---

출현 감지는 자동화의 가장 중요한 출발점인 재실 여부를 확인 하는 기능입니다. 누군가의 현재 위치를 알 수 있다면 이런 자동화를 실행할 수 있습니다:

- 아이가 학교에 도착하면 알람을 받는다
- 퇴근후 집으로 출발하면서 에어콘을 켠다


<p class='img'>
<img src='/images/screenshots/map.png' />
두 사람의 위치가 학교, 직장 또는 집 여부를 나타내는 홈어시스턴트 화면.
</p>

### Adding presence detection

There are different ways of setting up presence detection. Usually the easiest way to detect presence is by checking which devices are connected to the network. You can do that if you have one of our [supported routers][routers]. By leveraging what your router already knows, you can easily detect if people are at home.

It's also possible to run an app on your phone to provide detailed location information to your Home Assistant instance. For iOS and Android, we suggest using the [Home Assistant Companion app][companion].

During the setup of Home Assistant Companion on your mobile device, the app will ask for permission to allow the device's location to be provided to Home Assistant. Allowing this will create a `device_tracker` entity for that device which can be used in automations and conditions.


### Zones

<img src='/images/screenshots/badges-zone.png' style='float: right; margin-left: 8px; height: 100px;'>

Zones allow you to name areas on a map. These areas can then be used to name the location a tracked user is, or use entering/leaving a zone as an automation [trigger] or [condition]. Zones can be set up from the integration page in the configurations screen.

<div class='note'>
The map view will hide all devices that are home.
</div>

[routers]: /integrations/#presence-detection
[nmap]: /integrations/nmap_tracker
[ha-bluetooth]: /integrations/bluetooth_tracker
[ha-bluetooth-le]: /integrations/bluetooth_le_tracker
[ha-locative]: /integrations/locative
[ha-gpslogger]: /integrations/gpslogger
[ha-presence]: /integrations/#presence-detection
[mqtt-self]: /integrations/mqtt/#run-your-own
[mqtt-cloud]: /integrations/mqtt/#cloudmqtt
[zone]: /integrations/zone/
[trigger]: /getting-started/automation-trigger/#zone-trigger
[condition]: /getting-started/automation-condition/#zone-condition
[ha-map]: /integrations/map/
[companion]: https://companion.home-assistant.io/

### [Next step: Join the Community &raquo;](/getting-started/join-the-community/)
