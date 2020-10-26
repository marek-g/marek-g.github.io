---
layout: page
title: HTTPS Sniffing
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
