---
title: "홈어시스턴트 설치"
description: "Getting started: How to install Home Assistant."
---

{% comment %}

Note for contributors:

The getting started guide aims at getting new users get Home Assistant up and
running as fast as possible. Nothing else. All other things should not be
written down, as it creates a spaghetti of links and the user will lose focus.

So here are guidelines:

 - Focus on the bare necessities. No remote port, no securing installation. The
   defaults are good enough to get a system up and running for the first guide.
 - Avoid or explain technical terms.
 - Do not talk about YAML if it can be partially/fully done in UI.
 - Do not tell people about stuff they can do later. This can be added to a
   2nd tier guide.
 - The first page of the guide is for installation, hence Hass.io specific.
   Other pages should not refer to it except for the page introducing the last
   page that introduces `configuration.yaml`.

{% endcomment %}

본 문서는 라즈베리파이에 홈어시스턴트를 운용하는 것에 대한 가이드입니다. 가장 쉬운 방법은 [Hass.io](/hassio/) 를 설치하는 것으로 라즈베리파이와 다른 기기들을 연결해주는 강력한 홈오토메이션 허브를 만들 수 있습니다. 

리눅스에 대한 경험이 없거나 홈어시스턴트를 빨리 시작하려면 이 문서를 참고하세요. 숙련된 유저이거나 [이 문서에서 사용한 장비][supported]가 없다면 [다른 설치 방법](/docs/installation/)을 참고하길 바랍니다. 다른 설치 방법을 완료하신 분은 바로 [다음단계][next-step]로 넘어가세요. 

[supported]: /hassio/installation/

### Suggested hardware

We will need a few things to get started with installing Home Assistant. The Raspberry Pi 4 Model B is a good, affordable starting point for your home automation journey. Links below lead to Amazon US. If you're not in the US, you should be able to find these items in web stores in your country.

- [Raspberry Pi 4 Model B (2GB)](https://amzn.to/2XULT2z) + [Power Supply](https://www.raspberrypi.org/help/faqs/#powerReqs) (at least 2.5A)
- [Micro SD Card](https://amzn.to/2X0Z2di). Ideally get one that is [Application Class 2](https://www.sdcard.org/developers/overview/application/index.html) as they handle small I/O much more consistently than cards not optimized to host applications. A 32 GB or bigger card is recommended.
- SD Card reader. This is already part of most laptops, but you can purchase a [standalone USB adapter](https://amzn.to/2WWxntY) if you don't have one. The brand doesn't matter, just pick the cheapest.
- Ethernet cable. Home Assistant can work with Wi-Fi, but an Ethernet connection would be more reliable.

### Software requirements

- Download and extract the HassOS image for [your device](/hassio/installation/)
- Download [balenaEtcher] to write the image to an SD card

[balenaEtcher]: https://www.balena.io/etcher

### Installation

1. Put the SD card in your card reader.
2. Open balenaEtcher, select the HassOS image and flash it to the SD card.
3. Unmount the SD card and remove it from your card reader.
4. Follow this step if you want to configure Wi-Fi or a static IP address (this step requires a USB stick). Otherwise, move to step 5.
   - Format a USB stick to FAT32 with the volume name `CONFIG`.
   - Create a folder named `network` in the root of the newly-formatted USB stick.
   - Within that folder, create a file named `my-network` without a file extension.
   - Copy one of [the examples] to the `my-network` file and adjust accordingly.
   - Plug the USB stick into the Raspberry Pi.

5. Insert the SD card into your Raspberry Pi. If you are going to use an Ethernet cable, connect that too.
6. Connect your power supply to the Raspberry Pi.
7. The Raspberry Pi will now boot up, connect to the Internet and download the latest version of Home Assistant. This will take about 20 minutes.
8. Home Assistant will be available at `http://hassio.local:8123`. If you are running an older Windows version or have a stricter network configuration, you might need to access Home Assistant at `http://hassio:8123`.
9. If you used a USB stick for configuring the network, you can now remove it.

[the examples]: https://github.com/home-assistant/hassos/blob/dev/Documentation/network.md

### [Next step: Onboarding &raquo;][next-step]

[next-step]: /getting-started/onboarding/

_As an Amazon Associate Home Assistant earns from qualifying purchases._
