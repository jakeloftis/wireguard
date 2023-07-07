```
apt-get update
```

```
apt-get install -y wireguard iptables resolvconf qrencode
```

![daemons](https://raw.githubusercontent.com/jakeloftis/wireguard/main/images/daemons.png)

![daemons2](https://raw.githubusercontent.com/jakeloftis/wireguard/main/images/daemons2.png)

```
ip -4 route ls | grep default | grep -Po '(?<=dev )(\S+)' | head -1
```

```
ip link add dev wg0 type wireguard
```

```

```
