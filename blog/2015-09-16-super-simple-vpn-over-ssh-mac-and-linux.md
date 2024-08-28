---
slug: super-simple-vpn-over-ssh-mac-and-linux
title: 'Super simple VPN over SSH (Mac and Linux)'
authors: mifi
tags:
  - tip
  - mac
---

**Note: [Moved here](/docs/apps/sshuttle).**

There is a cute litte program called sshuttle for shuttling IP subnet traffic over and SSH connection, like a VPN but doesn't require any setup on the remote end.

As far as i understand, it will add a firewall rule to the client saying which destinations to tunnel (the specified netmask), and will forward these packets using a python script through an ssh pipe to a python script in the other end that will spit out the packets into the target network.

Turns out in Yosemite Apple removed ipfw and moved over to pf, so in order to get it to work you need the newest version of sshuttle, from here (which supports `pf`):

[https://github.com/sshuttle/sshuttle](https://github.com/sshuttle/sshuttle)

...or install via `homebrew`

Sources:
[https://ariejan.net/2012/07/11/vpn-too-complicated-use-a-ip-over-ssh-tunnel-instead/](https://ariejan.net/2012/07/11/vpn-too-complicated-use-a-ip-over-ssh-tunnel-instead/)
