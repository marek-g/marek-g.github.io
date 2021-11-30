---
layout: page
title: HTTPS Sniffing
parent: Tips & Tricks
---

# Install mitmproxy

1. Install `mitmproxy` with your package manager

# Generate self signed certificate

1. Run `mitmproxy` & exit (creates ~/.mitmproxy with certificate valid for 10 years)

# Install certificate on Manjaro Linux

1. `openssl x509 -in ~/.mitmproxy/mitmproxy-ca.pem -inform PEM -out ca.crt`

2. `sudo trust anchor ca.crt`

3. `rm ca.crt`

Remove certificate after usage:

1. `trust list`

2. `sudo trust anchor --remove "pkcs11:id=%AA%BB%CC%DD%EE;type=cert"`

# HTTPS Sniffing with mitmproxy in reverse mode

1. `mitmproxy --mode reverse:https://login.nestbank.pl/ -p 4433`

2. Connect to "https://localhost:4433"

Source:

https://unix.stackexchange.com/questions/103037/what-tool-can-i-use-to-sniff-http-https-traffic

https://gist.github.com/franciscocpg/a4f52afcc00d472a9d7c407db16a92ee

# Sniff traffic from Android phone

## Install certificate on the phone

1. Copy ~/.mitmproxy/mitmproxy-ca-cert.cer to the phone

2. Go to Settings, Security / ... / “Install from device storage”

3. Select certificate file and click “OK”

4. Set name & keep used for: "VPN and apps"

5. Now click on “Trusted credentials” and select the “User” tab. The certificate should now appear in the list.

Starting from Android 24 (7.0) the certificate must be installed in the system trust store. To do it:

1. Move certificate file found in `/data/misc/user/0/cacerts-added/` folder to `/system/etc/security/cacerts`

2. Reboot the phone

Firefox does not use system certificates and the certificate must be imported to Firefox.

## Option 1 - regular proxy

Regular proxy works with most apps, but not with not all of them (TCP stream and/or Mono apps).
But it's easy to enable.

On the phone:

1. Go to the wifi settings

2. Go to the network's settings

3. Put computer's IP and port (8080)

Run mitmproxy as regular proxy:

1. `mitmproxy`

## Option 2 - transparent proxy

On the phone:

1. Got to the wifi settings, static IP

2. Put your PC's IP address as the default gateway

On PC - these settings will not be persistent after reset:

1. Enable IP forwarding and disable ICMP redirects:

`sysctl -w net.ipv4.ip_forward=1`
`sysctl -w net.ipv6.conf.all.forwarding=1`
`sysctl -w net.ipv4.conf.all.send_redirects=0`

2. Redirect the desired traffic to mitmproxy:

`iptables -t nat -A PREROUTING -i eno1 -p tcp --dport 1:65535 -j REDIRECT --to-port 8080`
`ip6tables -t nat -A PREROUTING -i eno1 -p tcp --dport 1:65535 -j REDIRECT --to-port 8080`

where `eno1` is the interface name (may be different for you, verify with `ifconfig`)
and `1:65535` is port range (you may alternatively provide single port like `443` and `80`)

3. Run mitmproxy with transparent mode:

`mitmproxy --mode transparent --showhost`
