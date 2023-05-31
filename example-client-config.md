# example client config located at "cat /root/wg0-client-NAME_FROM_SETUP_WIZARD.conf"
```
[Interface]
PrivateKey = CLIENT_PRIVATE_KEY_HERE
Address = 10.66.66.2/32,fd42:42:42::2/128
DNS = 1.1.1.1,1.0.0.1

[Peer]
PublicKey = SERVER_PUBLIC_KEY_HERE
PresharedKey = PRESHARED_KEY_HERE
Endpoint = VPS_SERVER_IP_HERE:51820
AllowedIPs = 0.0.0.0/0,::/0
```
