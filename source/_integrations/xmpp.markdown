---
title: Jabber (XMPP)
description: Instructions on how to add Jabber (XMPP) notifications to Home Assistant.
logo: xmpp.png
ha_category:
  - Notifications
ha_release: pre 0.7
ha_codeowners:
  - '@fabaff'
  - '@flowolf'
---

The `xmpp` notification platform allows you to deliver notifications from Home Assistant to a [Jabber (XMPP)](https://xmpp.org/) account.

## Configuration

To enable Jabber notifications in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
notify:
  - name: NOTIFIER_NAME  # e.g. jabber
    platform: xmpp
    sender: YOUR_JID
    password: YOUR_JABBER_ACCOUNT_PASSWORD
    recipient: YOUR_RECIPIENT
```

{% configuration %}
name:
  description: "Setting the optional parameter `name` allows multiple notifiers to be created. The default value is `notify`. The notifier will bind to the service `notify.NOTIFIER_NAME`."
  required: false
  type: string
  default: notify
sender:
  description: "The Jabber ID (JID) that will act as origin of the messages. Add your JID including the domain, e.g. your_name@jabber.org."
  required: true
  type: string
resource:
  description: "Resource part of JID, e.g., your_name@jabber.org/`HA-cabin`."
  required: false
  type: string
  default: home-assistant
password:
  description: The password for your given Jabber account.
  required: true
  type: string
recipient:
  description: The Jabber ID (JID) that will receive the messages.
  required: true
  type: string
tls:
  description: Force TLS.
  required: false
  type: boolean
  default: true
verify:
  description: Allow disabling SSL certificate validity check, e.g., self-signed certificate.
  required: false
  type: boolean
  default: true
room:
  description: "Room's name (e.g., example@conference.jabber.org). If set, send a message to chatroom instead of the recipient."
  required: false
  type: string
{% endconfiguration %}

<div class='note'>

  Pre Home Assistant 0.81 `sleekxmpp` was used to connect to XMPP servers. `sleekxmpp` as of version 1.3.2, does not support > TLS v1. If you are running your own XMPP server (e.g., Prosody, ejabberd) make sure to allow using TLS v1.

  Home Assistant after 0.81 uses `slixmpp`, which also supports TLS v1.1 and TLS v1.2.

</div>

All Jabber IDs (JID) must include the domain. Make sure that the password matches the account provided as sender.

You can send text messages and images as well as other files through Jabber.

### Jabber Text Message

Here are some examples on how to set up a script, that can be run from an automation.

Number 1 shows a classical, text-only message. The Title is optional, although if omitted,
`Home-Assistant` will be set. To keep it empty set it to `""`.

```yaml
# Example script.yaml entry
1_send_jabber_message:
  alias: "Text only Jabber message"
  sequence:
    - service: notify.jabber  # from notify.NOTIFIER_NAME
      data:
        title: "Optional Title"
        message: "My funny or witty message"
```

### Jabber Image Message

You can send images or files from locally stored files or remote web locations via Jabber's HTTP Upload feature.
To send files and images, your jabber server must support [XEP_0363](https://xmpp.org/extensions/xep-0363.html).

<div class='note'>

Be aware that images are uploaded onto the Jabber server of your provider. They reside there un-encrypted and could be accessed by the server admins. Usually images are deleted after a few days.<br>
<br>
Home-Assistant supports TLS encryption to ensure transport encryption. TLS is enforced by default. You can disable it  with the [`tls`](#tls) flag -- which is not recommended.

</div>

Number 2 sends only an image, retrieved from the URL. The TLS connection to get the image is also not verified (use with caution).

```yaml
# Example script.yaml entry
2_send_jabber_message_with_image_url:
  alias: "Send Image via Jabber from website"
  sequence:
    - service: notify.jabber
      data:
        title: ""
        message: ""
        data:
          url: "https://www.graz.at:8443/webcam_neu/getimg.php"
          verify: false
```

Number 3 sends an image from a local path.

```yaml
# Example script.yaml entry
3_send_jabber_message_with_local_image_path:
  alias: "Send Image via Jabber from local file"
  sequence:
    - service: notify.jabber
      data:
        title: ""
        message: ""
        data:
          path: "/home/homeassistant/super_view.jpg"
```

### Jabber File Message


Number 4 sends a text-file, retrieved from Github, renamed to `Hass_Cheatsheet.txt` to be viewable on a mobile Android device, as most don't offer any application to view `.md` files. Optionally you can add a timeout for the HTTP upload in seconds.

```yaml      
# Example script.yaml entry
4_send_jabber_message_with_file:
  alias: "Send text file via Jabber"
  sequence:
    - service: notify.jabber
      data:
        title: ""
        message: ""
        data:
          url: "https://raw.githubusercontent.com/arsaboo/homeassistant-config/master/HASS%20Cheatsheet.md"
          path: "Hass_Cheatsheet.txt"
          timeout: 10
```

### Templating

Number 5 sends an image retrieved from a URL, and an additional text message with `title` and `message`.

```yaml
# Example script.yaml entry
5_send_jabber_message_with_image_and_text:
  alias: "Send Image and Text via Jabber"
  sequence:
    - service: notify.jabber
      data:
        title: "The Time is now"
        message: "{% raw %} {{ {% endraw %}now(){% raw %} }} {% endraw %}, templating works as well..."
        data:
          url: "https://github.com/home-assistant/home-assistant.io/raw/next/source/images/favicon-192x192.png"
```

Number 6 sends an image from a templated URL.

```yaml
# Example script.yaml entry
6_send_jabber_message_with_image_from_url_template:
  alias: "Send Image from template URL via Jabber"
  sequence:
    - service: notify.jabber
      data:
        title: ""
        message: ""
        data:
          url_template: "https://www.foto-webcam.eu/webcam/dornbirn/{% raw %}{{ now().year }}/{{ '%02d' % now().month }}/{{ '%02d' % now().day }}/{{ '%02d' % now().hour }}{{ (now().minute + 58) % 60 // 10}}{% endraw %}0_hd.jpg"
```

The possible source of a file is prioritized and only one will be picked up. `url_template` has the hightest priority; next is `url` then `path_template` and finally if none of them are defined `path` would be used. `path` will be used to eliminate file extension guessing for unknown URL downloads. Only the file extension will be left, as Home Assistant changes the filename to a random string for added privacy.

To find out more about notifications, please see the [getting started with automation page](/getting-started/automation/).
