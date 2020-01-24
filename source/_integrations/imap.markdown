---
title: IMAP
description: Instructions on how to integrate IMAP unread email into Home Assistant.
logo: smtp.png
ha_category:
  - Mailbox
ha_release: 0.25
ha_iot_class: Cloud Push
---

The `imap` integration is observing your [IMAP server](https://en.wikipedia.org/wiki/Internet_Message_Access_Protocol) and reporting the amount of unread emails.

## Configuration

To enable this sensor, add the following lines to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: imap
    server: YOUR_IMAP_SERVER
    username: YOUR_USERNAME
    password: YOUR_PASSWORD
```

{% configuration %}
server:
  description: The IP address or hostname of the IMAP server.
  required: true
  type: string
port:
  description: The port where the server is accessible.
  required: false
  default: 993
  type: integer
name:
  description: Name of the IMAP sensor.
  required: false
  type: string
username:
  description: Username for the IMAP server.
  required: true
  type: string
password:
  description: Password for the IMAP server.
  required: true
  type: string
folder:
  description: The IMAP folder to watch.
  required: false
  default: inbox
  type: string
search:
  description: The IMAP search to perform on the watched folder.
  required: false
  default: UnSeen UnDeleted
  type: string
charset:
  description: The character set used for this connection.
  required: false
  default: utf-8
  type: string
{% endconfiguration %}

### Configuring IMAP Searches

By default, this integration will count unread emails. By configuring the search string, you can count other results, for example:

* `ALL` to count all emails in a folder
* `FROM`, `TO`, `SUBJECT` to find emails in a folder (see [IMAP RFC for all standard options](https://tools.ietf.org/html/rfc3501#section-6.4.4))
* [Gmail's IMAP extensions](https://developers.google.com/gmail/imap/imap-extensions) allow raw Gmail searches, like `X-GM-RAW "in: inbox older_than:7d"` to show emails older than one week in your inbox. Note that raw Gmail searches will ignore your folder configuration and search all emails in your account!

#### Full configuration sample with search

```yaml
# Example configuration.yaml entry for gmail
sensor:
  - platform: imap
    server: imap.gmail.com
    port: 993
    username: YOUR_USERNAME
    password: YOUR_PASSWORD
    search: FROM <sender@email.com>, SUBJECT <subject here>

# Example configuration.yaml entry for Office 365
sensor:
  - platform: imap
    server: outlook.office365.com
    port: 993
    username: email@address.com
    password: password
    search: FROM <sender@email.com>, SUBJECT <subject here>
    charset: US-ASCII
```
