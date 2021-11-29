---
layout: page
title: HTTPS Sniffing
parent: Tips & Tricks
---

# HTTPS Sniffing with mitmproxy

1. Install mitmproxy

2. Run mitmproxy & exit (creates ~/.mitmproxy)

3. openssl x509 -in ~/.mitmproxy/mitmproxy-ca.pem -inform PEM -out ca.crt

4. sudo trust anchor ca.crt

5. rm ca.crt

6. mitmproxy --mode reverse:https://login.nestbank.pl/ -p 4433

7. Connect to "https://localhost:4433"

Remove certificate after usage:

1. trust list

2. sudo trust anchor --remove "pkcs11:id=%AA%BB%CC%DD%EE;type=cert"

Source:

https://unix.stackexchange.com/questions/103037/what-tool-can-i-use-to-sniff-http-https-traffic

https://gist.github.com/franciscocpg/a4f52afcc00d472a9d7c407db16a92ee

## Sniff traffic from Android phone

### Install certificate on the phone

1. Copy ~/.mitmproxy/mitmproxy-ca-cert.cer to the phone

2. Go to Settings, Security / ... / “Install from device storage”

3. Select certificate file and click “OK”

4. Set name & keep used for: "VPN and apps"

5. Now click on “Trusted credentials” and select the “User” tab. The certificate should now appear in the list.

Starting from Android 24 (7.0) the certificate must be installed in the system trust store. To do it:

1. Move certificate file found in `/data/misc/user/0/cacerts-added/` folder to `/system/etc/security/cacerts`

2. Reboot the phone

Firefox does not use system certificates and the certificate must be imported to Firefox.

### Setup a proxy on the phone

1. Go to the wifi settings

2. Go to the network's settings

3. Put computer's IP and port (8080)

### Run mitmproxy as regular proxy

1. mitmproxy
