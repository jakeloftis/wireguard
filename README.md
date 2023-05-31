# wireguard <br /> 
Wireguard installation scripts and notes <br /> 

After installing Ubuntu, run the following 3 lines of code.
```
curl -O https://raw.githubusercontent.com/jakeloftis/wireguard/master/wireguard-install-open.sh
```
```
chmod +x wireguard-install-open.sh
```
```
./wireguard-install-open.sh
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
# example server config located at /etc/wireguard/wg0.con
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

### Client Name
[Peer]
PublicKey = CLIENT_PUBLIC_KEY_HERE
PresharedKey = PdS7xPReW5zidJAphG/TvLsfRPp+ChEvmUV5bYIojWI=
AllowedIPs = 10.66.66.2/32,fd42:42:42::2/128

```
