# Vultr VPS Server Deployment
```
https://my.vultr.com/
Deploy New Server
Choose Server: Cloud Compute
CPU & Storage Technology: Intel Regular Performance
Server Location: Choose Server Location
Server Image: Operating System > Ubuntu > 22.04  LTS  x64
Server Size: 25 GB SSD $5/month
Add AUto Backups: Optional
Deploy Now
```

# Vultr VPS Login with PuTTY
```
Download PuTTY at https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
OR
Direct Download Link for exe/portable version: https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe
Launch PuTTY
Log in to Vultr: https://my.vultr.com/
Select Server
Copy/Paste IP Address from Vultr to "Host Name (or IP address)" text box in PuTTY
Click "Open"
Username: root
Password: Copy/Paste password from Vultr server page (*Hint: Use right click to paste into PuTTY)
```

# wireguard <br /> 
Wireguard installation scripts and notes <br /> 

After installing Ubuntu 20.04, run the following 3 lines of code.
```
curl -O https://raw.githubusercontent.com/jakeloftis/wireguard/master/wireguard-install-open.sh
```
```
chmod +x wireguard-install-open.sh
```
```
./wireguard-install-open.sh
```

# example server config located at "cat /etc/wireguard/wg0.conf"
```
[Interface]
Address = 10.66.66.1/24,fd42:42:42::1/64
ListenPort = 51820
PrivateKey = SERVER_PRIVATE_KEY_HERE
PostUp = iptables -I INPUT -p udp --dport 51820 -j ACCEPT
PostUp = iptables -I FORWARD -i enp1s0 -o wg0 -j ACCEPT
PostUp = iptables -I FORWARD -i wg0 -j ACCEPT
PostUp = iptables -t nat -A POSTROUTING -o enp1s0 -j MASQUERADE
PostUp = ip6tables -I FORWARD -i wg0 -j ACCEPT
PostUp = ip6tables -t nat -A POSTROUTING -o enp1s0 -j MASQUERADE
PostDown = iptables -D INPUT -p udp --dport 51820 -j ACCEPT
PostDown = iptables -D FORWARD -i enp1s0 -o wg0 -j ACCEPT
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT
PostDown = iptables -t nat -D POSTROUTING -o enp1s0 -j MASQUERADE
PostDown = ip6tables -D FORWARD -i wg0 -j ACCEPT
PostDown = ip6tables -t nat -D POSTROUTING -o enp1s0 -j MASQUERADE

### Client NAME_FROM_SETUP_WIZARD
[Peer]
PublicKey = CLIENT_PUBLIC_KEY_HERE
PresharedKey = PRESHARED_KEY_HERE
AllowedIPs = 10.66.66.2/32,fd42:42:42::2/128
```

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
# port forwarding examples <br />
To forward a single port
```
iptables -t nat -A PREROUTING -i enp1s0 -p tcp --dport 3074 -j DNAT --to-destination 10.66.66.2:3074
```
To forward a range of ports
```
iptables -t nat -A PREROUTING -i enp1s0 -p tcp --dport 3074:3079 -j DNAT --to-destination 10.66.66.2:3074:3079
```
