---
title: Cisco IOS
description: Instructions on how to integrate Cisco IOS routers into Home Assistant.
logo: cisco.png
ha_category:
  - Presence Detection
ha_release: 0.33
ha_codeowners:
  - '@fbradyirl'
---

This is a presence detection scanner for [Cisco](https://www.cisco.com/) IOS devices.

<div class='note warning'>
This device tracker needs SSH to be enabled on the router.
</div>

Before using this scanner it is recommended that you lower the ARP cache timeout on your router, as Cisco IOS normally comes with a 4 hour default ARP cache timeout.

For example, the following commands will lower the timeout to 2 minutes on Vlan1:

```bash
# 1. use this command to see what Vlan your devices are on
show ip arp

# 2. Go into configure mode
conf t

# 3. Use the Vlan name as you see it from step 1 above
interface Vlan1

# 4. Set a new arp cache timeout
arp timeout 120

# 5. Exit
# Press <ctrl+c> to exit configure mode

# 6. Don't forget to save the new config, so that it will survive a reboot
copy running-config startup-config
```

<div class='note warning'>

If you have a very large number of devices on your VLan (+1000), then you may want to adjust the ARP cache timeout to suit your needs. See [this discussion](https://supportforums.cisco.com/discussion/10169296/arp-timeout) to learn more.

</div>

To use this device tracker in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
device_tracker:
  - platform: cisco_ios
    host: ROUTER_IP_ADDRESS
    username: YOUR_ADMIN_USERNAME
    password: YOUR_ADMIN_PASSWORD
```

{% configuration %}
host:
  description: The IP address of your router, e.g., 192.168.1.1.
  required: true
  type: string
username:
  description: The username of an user with administrative privileges.
  required: true
  type: string
password:
  description: The password for your given admin account.
  required: true
  type: string
{% endconfiguration %}

See the [device tracker integration page](/integrations/device_tracker/) for instructions how to configure the people to be tracked.
