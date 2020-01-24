---
title: "Securing"
description: "Instructions on how to secure your Home Assistant installation."
redirect_from: /getting-started/securing/
---

One major advantage of Home Assistant is that it's not dependent on cloud services. Even if you're only using Home Assistant on a local network, you should take steps to secure your instance.

## Checklist

Here's the summary of what you *must* do to secure your Home Assistant system:

- Configure [secrets](/docs/configuration/secrets/) (but do remember to back them up)
- Regularly keep the system up to date

## Remote Access

If you want secure remote access, the easiest option is to use [Home Assistant cloud](/cloud/). Other options are to use [TLS/SSL](/docs/ecosystem/certificates/lets_encrypt/) to expose your instance to the internet, use a [VPN](https://pivpn.dev/), [Tor](/docs/ecosystem/tor/) or an [SSH tunnel](/blog/2017/11/02/secure-shell-tunnel/).

### Extras for manual installations

As well as the above we advise that you consider the following to improve security:

- For systems that use SSH set `PermitRootLogin no` in your sshd config (usually `/etc/ssh/sshd_config`) and to use SSH keys for authentication instead of passwords. This is particularly important if you enable remote access to your SSH services.
- Lock down the host following good practice guidance, for example:
  * [Securing Debian Manual](https://www.debian.org/doc/manuals/securing-debian-howto/index.en.html) (this also applies to Raspbian)
  * [Red Hat Enterprise Linux 7 Security Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/pdf/Security_Guide/Red_Hat_Enterprise_Linux-7-Security_Guide-en-US.pdf), [CIS Red Hat Enterprise Linux 7 Benchmark](https://benchmarks.cisecurity.org/tools2/linux/CIS_Red_Hat_Enterprise_Linux_7_Benchmark_v1.0.0.pdf)
