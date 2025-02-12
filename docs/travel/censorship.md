---
tags: [vpn, censorship, gfw, china]
---

# Internet censorship

After using NordVPN for years, but not really being satisfied, I did some research to find better alternatives. Here's a little (unbiased, I don't have any affiliation with any provider or companion) collection of resources on accessing the free internet in heavily censoring states like China.

<!--truncate-->

## ✅ Hong Kong SIM

Apparently the easiest way to bypass the GFW is to get a Hong Kong SIM card with roaming enabled for China. Roaming data will go through an uncensored communication channel back to Hong Kong before hitting the internet, and because Hong Kong is (for now), relatively uncensored, this allows mostly free internet access within China. [Source](https://www.reddit.com/r/dumbclub/comments/143dswa/how_to_bypass_the_gfw_in_china_new_to_this/).

Tested by me Oct 24 2023 Hong Kong/China border - **Hong Kong SIM with China roaming worked!** Although personal hotspot did not seem to work using the SIM.

## Paid VPN services

### ❌ Mainstream VPNs

Apparently most mainstream VPNs like NordVPN, ExpressVPN, Surfshark etc. all *claim* to work in China, but in reality they don't. Because they are so popular, GFW will easily identify and block their servers and protocols very fast (cat/mouse-game). I've personally used NordVPN for a long time, but their app is horribly slow, buggy, freezes and keeps signing me out all the time. Also when I've actually *needed it* to work due to censorship in certain countries (not even China), it usually didn't work at all.

**Cheap.** Used to work in China but not anymore (I tested SurfShark march 2024 in Shanghai aiport).

### ✅ [LetsVPN](https://letsvpn.world/)

**Cheap.** Many people report it working. Possibly Chinese owned, local servers. Probably not 100% safe and they might monitor traffic. Some reports of people getting banned from visiting certain sites [[1]](https://www.reddit.com/r/vpnreviews/comments/149d55a/working_vpn_in_china/jo6xps5/)[[2]](https://www.reddit.com/r/dumbclub/comments/17uzdtv/bypass_letsvpn_ai_account_blocking/)[[3]](https://www.reddit.com/r/chinalife/comments/1cwb9tc/warning_about_letsvpn/). Not sure if it works on all sites. Some Norwegian websites seem to have blocked requests coming from Chinese LetsVPN servers, probably because of Chinese IP address. Choosing a Norwegian server (identified as Microsoft Corporation in Oslo) seems to resolve the problem (requires LetsVPN Platinum subscription).

Tested by me Oct 24 2023 Hong Kong/China border - Works! Sep 2024: Still works, but it's a bit slow.

- https://www.reddit.com/r/China/comments/13lhb9n/am_i_able_to_buy_and_use_letsvpn_while_in_china/

### ✅ [Astrill VPN](https://www.astrill.com/)

**Expensive.** Astrill VPN seems to be a go-to-service that kind of *just works* in China most of the time but people say it is slow. They have their own [proprietary obfuscated protocols](https://www.astrill.com/features/vpn-protocols).

Update: Seems in 2024 it might not be a silver bullet anymore, people have reported slowdowns and Astrill stopping working.

- https://www.reddit.com/r/chinalife/comments/13g2l4m/best_vpn_for_china_2023/
- https://www.reddit.com/r/China/comments/1017p4x/vpn_rant_2023/

### ❌ Clodflare WARP (aka. 1.1.1.1)

Reportedly this service works well in China because Cloudflare runs the VPN on the same servers of websites that Chinese rely on every day, making it harder to block. There is also a paid WARP+.

**Update Oct. 2023**: [1.1.1.1 might have been blocked in China now](https://github.com/net4people/bbs/issues/295).

Tested by me Oct 24 2023 Hong Kong/China border - CloudFlare WARP (not plus) did not work.

## DIY and exotic proxying software

### [Shadowrocket](https://apps.apple.com/ca/app/shadowrocket/id932747118)

Shadowrocket claims to be open source, but it is a **paid** iOS/iPadOS app that also works on Apple Silicon Macs (M1/M2) due to ARM. The app supports most exotic open source tunneling protocols. For Shadowrocket you need to bring your own servers, but it can also be used with WannaFlix.

#### How Shadowrocket works

Shadowrocket's documentation is not very good, and I'm struggling to find out exactly how it works, but from what I can understand, it creates a new tunnel device:

```
utun5: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 4000
	options=6463<RXCSUM,TXCSUM,TSO4,TSO6,CHANNEL_IO,PARTIAL_CSUM,ZEROINVERT_CSUM>
	inet 198.18.0.1 --> 198.18.0.1 netmask 0xffffff00 
	inet6 fe80::1e57:dcff:fe46:f143%utun5 prefixlen 64 scopeid 0x17 
	inet6 fc00:1234:ffff::10 prefixlen 64 
	nd6 options=201<PERFORMNUD,DAD>
```

Then it hijacks `/etc/resolv.conf` where it sets `nameserver 198.18.0.2`, which is probably the ShadowRocket app:

```bash
$ scutil --dns

resolver #1
  nameserver[0] : 198.18.0.2
  if_index : 23 (utun5)
  flags    : Request A records, Request AAAA records
  reach    : 0x00000002 (Reachable)

...
```

If you try to lookup any host name, it will resolve to a unique IP address under this local range, and Shadowrocket will intercept the request and pass it through the proxy, e.g.:
- `google.com` -> `198.18.0.13`
- `yahoo.com` -> `198.18.0.14`

This makes it work with any TCP connection, also from command line utilities. Not sure if it works with UDP, but I think it would. So I'm not 100% sure how it works, but I think it has its own DNS server and maps all DNS requests to unique local IPs, for which it then intercepts the requests to, and passes them through the proxy.

#### [WannaFlix](https://wannaflix.com/)

[Great documentation](https://docs.wannaflix.net/), supports many [exotic circumvention protocols](https://docs.wannaflix.net/which-protocol-to-choose). Hong Kong based, so might be risky. They endorse the Shadowrocket app.

Setup: https://docs.wannaflix.net/mac-os/v2ray-shadowsocks/shadowrocket-m1-macs-only

#### ❌ Free providers

There are many other companies like [sshOcean](https://sshocean.com/), [OpenTunnel](https://opentunnel.net/), [GreenSSH](https://greenssh.com/) that all kind of look the same. They provide free servers for many of the protocols supported by Shadowrocket. Seems to work with the Shadowrocket app. You just copy paste the (TLS) `vmess://`-URI into Chrome and it will launch the Shadowrocket app and add the configuration. They give you a temporary credential for like 7 days. Not sure who they are or how they make money or pay for their costs, but sshOcean's domain name is registered in Reykjavik, Iceland. 🤔

Tested by me Oct 24 2023 Hong Kong/China border - sshOcean did not work.

#### ❌ [VPN.fail](https://vpn.fail/)

They claim to do it as a [philanthropic](https://vpn.fail/faq) [gesture](https://vpn.fail/about). Considering that they provide a list of their servers as a [JSON file](https://vpn.fail/free-proxy/json), I would think that GFW would easily block all of these servers. Note: `vmess://` URIs are not available in the JSON API, only through the web UI.

- https://vpn.fail/free-proxy
- https://vpn.fail/free-proxy/type/v2ray

Tested by me Oct 24 2023 Hong Kong/China border - VPN.fail did not work.

#### Parsing `vmess://` URLs

With Node.js:

```js
JSON.parse(Buffer.from('vmess://uri'.replace(/^vmess:\/\//, ''), 'base64').toString())
```

Example output:
```json
{
  add: '198.2.218.26',
  aid: 64,
  host: 'www.51360818.xyz',
  id: '418048af-a293-4b99-9b0c-98ca3580dd24',
  net: 'ws',
  path: '/path/1695204567237',
  port: 443,
  ps: '🇺🇸US-198.2.218.26-0169',
  tls: 'tls',
  type: 'auto',
  security: 'auto',
  'skip-cert-verify': true,
  sni: ''
}
```

#### Hosting your own server

Drawbacks:
- Takes effort to set up
- If your IP gets flagged/banned, there's no fallback.

There are many open source protocols like Shadowsocks.

##### [`shadowsocks-libev`](https://github.com/shadowsocks/shadowsocks-libev)

On ubuntu:

```bash
sudo apt install shadowsocks-libev
```

Now edit `/etc/shadowsocks-libev/config.json`:

```conf
{
    "server":["::0", "0.0.0.0"],
    "mode":"tcp_and_udp",
    "server_port":443,
    "local_port":1080,
    "password":"create a strong password",
    "timeout":86400,
    "method":"chacha20-ietf-poly1305"
}
```

```bash
sudo systemctl restart shadowsocks-libev
```

Then in Shadowrocket add a new connection:
- Type: Shadowsocks
- Address: IP of server
- Port: 443 (some firewalls block other ports)
- Method: chacha20-ietf-poly1305
- UDP relay: On

See also:

- https://github.com/shadowsocks/shadowsocks-rust

### [xiringuito](https://github.com/ivanilves/xiringuito)

Similar to `sshuttle`, but `xiringuito` uses tun/tap and tunnels also UDP, but still has the issue with TCP-over-TCP performance.

## Protocols

### Trojan

Trojan is a newer protocol designed to act like HTTPS which is the most common traffic on the internet if you don't know. [Source](https://www.reddit.com/r/dumbclub/comments/s63uq4/difference_between_vless_vmess_and_trojan_in_the/htv2t2v/).

### VLESS+XTLS

...

### Vmess+TLS+WS

...

## `socat` on port 443

`sshuttle` with `ssh` tunnel over `socat` port 443 to mimic HTTP TLS traffic?

## Other / technical info
- https://github.com/net4people/bbs/issues/129
- https://github.com/klzgrad/naiveproxy
- https://www.v2ray.com/en/index.html

## Links
- https://en.m.wikipedia.org/wiki/Great_Firewall#Methods_for_bypassing_the_firewall
- https://www.reddit.com/r/vpnreviews/comments/149d55a/working_vpn_in_china/
