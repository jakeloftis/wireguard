# Port forwarding behind a Carrier Grade Nat (CGNAT)

# Vultr VPS Server Deployment  
Create an account at https://my.vultr.com/
```
Deploy New Server
Choose Server: Cloud Compute
CPU & Storage Technology: Intel Regular Performance
Server Location: Choose Server Location
Server Image: Operating System > Ubuntu > 22.04  LTS  x64
Server Size: 25 GB SSD $5/month
Add Auto Backups: Optional
Deploy Now
```

# Vultr VPS Login with PuTTY  
Download and install PuTTY at https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html  
OR  
I recommend the direct download Link for exe/portable version: https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe  
  
Launch PuTTY  
Log in to Vultr: https://my.vultr.com/  
Select your server (it may be called "Cloud Instance" by default)  
Copy/Paste IP Address from Vultr to the "Host Name (or IP address)" text box in PuTTY  
Click "Open"  
Username: root  
Password: Copy/Paste password from Vultr server page into PuTTY password prompt  
(*Hint: Use right click to paste into PuTTY command line)  

# Wireguard installation on Vultr server 

After logging in to your Vultr server, run the 3 commands below by doing a copy/paste/enter sequence   
(*Hint: Use right click to paste each line into PuTTY command line)  
```
curl -O https://raw.githubusercontent.com/jakeloftis/wireguard/master/wireguard-install-open.sh
```
```
chmod +x wireguard-install-open.sh
```
```
./wireguard-install-open.sh
```


Press enter through each of the following prompts
```
IPv4 or IPv6 public address: VULTR-SERVER-IP-ADDRESS-WILL-BE-HERE
Public interface: enp1s0
WireGuard interface name: wg0
Server WireGuard IPv4: 10.66.66.1
Server WireGuard IPv6: fd42:42:42::1
Server WireGuard port [1-65535]: 51820
First DNS resolver to use for the clients: 1.1.1.1
Second DNS resolver to use for the clients (optional): 1.0.0.1

WireGuard uses a parameter called AllowedIPs to determine what is routed over the VPN.
Allowed IPs list for generated clients (leave default to route everything): 0.0.0.0/0,::/0
```

After the installation completes you will be promted with
```
Client configuration

The client name must consist of alphanumeric character(s). It may also include underscores or dashes and can't exceed 15 chars.
Client name: ENTER-CLIENT-NAME-OF-YOUR-CHOICE
Client WireGuard IPv4: 10.66.66.2
Client WireGuard IPv6: fd42:42:42::2

```
If you encounter a (pink/purple) popup screen about out of date daemons or restarting daemons,  
Use the arrow keys to navigate up and down, then press space bar to put a * next to each service  
Press tab to switch to okay and press enter  
![daemons2](https://raw.githubusercontent.com/jakeloftis/wireguard/main/images/daemons2.png)  

# client config you will need for Windows Wireguard desktop app
Run the command below and replace NAME_FROM_SETUP_WIZARD with the client name you entered in the setup wizard
```
cat /root/wg0-client-NAME_FROM_SETUP_WIZARD.conf
```
Select all text and Copy/Paste to a Notepad file  
In Notepad, click File > Save  
Change the "File name:" to wgclient.conf  
Change the "Save as type:" to "All files (*.*)"  
Save  

# port forwarding examples  
To forward a single port, you'll need to specify your WAN interface, incoming port, and client IP address:port  
  
WAN Interface: To check your WAN interface in PuTTY, run the command:  
```
ip route show default | cut -d " " -f 5
```
You should recieve an output with enp1s0, enp1s0, eth0, eth1, etc...  
  
WAN Interface: In the examples below, my WAN Interface is enp1s0  
Port: In the example below, the port I want to forward is 3074 (--dport 3074)  
IP Address and Port of client: In the example below, I'm forwarding to the windows Wireguard Client at 10.66.66.2 on port 3074  (--to-destination 10.66.66.2:3074)
  
```
iptables -t nat -A PREROUTING -i enp1s0 -p tcp --dport 3074 -j DNAT --to-destination 10.66.66.2:3074
```
To forward a range of ports
```
iptables -t nat -A PREROUTING -i enp1s0 -p tcp --dport 3074:3079 -j DNAT --to-destination 10.66.66.2:3074-3079
```

Forwarding all ports for gaming except Wireguard port 51820 (not necessarily secure but works for open NAT)
```
iptables -t nat -A PREROUTING -i enp1s0 -p tcp --dport 1024:51819 -j DNAT --to-destination 10.66.66.2:1024-51819
iptables -t nat -A PREROUTING -i enp1s0 -p udp --dport 1024:51819 -j DNAT --to-destination 10.66.66.2:1024-51819
iptables -t nat -A PREROUTING -i enp1s0 -p tcp --dport 51821:65535 -j DNAT --to-destination 10.66.66.2:51821-65535
iptables -t nat -A PREROUTING -i enp1s0 -p udp --dport 51821:65535 -j DNAT --to-destination 10.66.66.2:51821-65535
```

# deleting port forwards (change the -A to -D)
```
iptables -t nat -D PREROUTING -i enp1s0 -p tcp --dport 3074 -j DNAT --to-destination 10.66.66.2:3074
```
To forward a range of ports
```
iptables -t nat -D PREROUTING -i enp1s0 -p tcp --dport 3074:3079 -j DNAT --to-destination 10.66.66.2:3074-3079
```
Deleting forwarding all ports for gaming except Wireguard port 51820 (not necessarily secure but works for open NAT)
```
iptables -t nat -D PREROUTING -i enp1s0 -p tcp --dport 1024:51819 -j DNAT --to-destination 10.66.66.2:1024-51819
iptables -t nat -D PREROUTING -i enp1s0 -p udp --dport 1024:51819 -j DNAT --to-destination 10.66.66.2:1024-51819
iptables -t nat -D PREROUTING -i enp1s0 -p tcp --dport 51821:65535 -j DNAT --to-destination 10.66.66.2:51821-65535
iptables -t nat -D PREROUTING -i enp1s0 -p udp --dport 51821:65535 -j DNAT --to-destination 10.66.66.2:51821-65535
```

# view port forwarding rules in effect
```
iptables -t nat -nvL
```

# download and install Windows Wireguard desktop app  
https://www.wireguard.com/install/ > Download Windows Installer  
Install the application  
Launch  
Click "Add Tunnel"  
Navigate to the location you saved your wgclient.conf in Notepad  
Click Open  
Click "Activate"  
You're done!  
Check your IP address at: https://www.whatismyip.com/

# notes
Port forwarding rules will reset each time you reboot your Ubuntu server  

Example Server Config: https://github.com/jakeloftis/wireguard/blob/main/example-server-config.md  
  
Example Client Config: https://github.com/jakeloftis/wireguard/blob/main/example-client-config.md  
  
If you want to add users, list users, revoke users, or uninstall Wireguard, you can access the menu by entering the following commands:  
```
curl -O https://raw.githubusercontent.com/jakeloftis/wireguard/master/wireguard-install-open.sh
```
```
chmod +x wireguard-install-open.sh
```
```
./wireguard-install-open.sh
```

# to be added

"persistant" ip tables when wireguard interface is up can be created in the following manner  
```
nano /etc/wireguard/wg0.conf
```
```
PreUp = iptables -t nat -A PREROUTING -i enp1s0 -p tcp --dport 1024:51819 -j DNAT --to-destination 10.66.66.2:1024-51819
PreUp = iptables -t nat -A PREROUTING -i enp1s0 -p udp --dport 1024:51819 -j DNAT --to-destination 10.66.66.2:1024-51819
PreUp = iptables -t nat -A PREROUTING -i enp1s0 -p tcp --dport 51821:65535 -j DNAT --to-destination 10.66.66.2:51821-65535
PreUp = iptables -t nat -A PREROUTING -i enp1s0 -p udp --dport 51821:65535 -j DNAT --to-destination 10.66.66.2:51821-65535
```
```
PreDown = iptables -t nat -D PREROUTING -i enp1s0 -p tcp --dport 1024:51819 -j DNAT --to-destination 10.66.66.2:1024-51819
PreDown = iptables -t nat -D PREROUTING -i enp1s0 -p udp --dport 1024:51819 -j DNAT --to-destination 10.66.66.2:1024-51819
PreDown = iptables -t nat -D PREROUTING -i enp1s0 -p tcp --dport 51821:65535 -j DNAT --to-destination 10.66.66.2:51821-65535
PreDown = iptables -t nat -D PREROUTING -i enp1s0 -p udp --dport 51821:65535 -j DNAT --to-destination 10.66.66.2:51821-65535
```

Client/Windows Desktop App: Allowed ip's to maintain access to LAN network  

Router: 192.168.0.1
```
AllowedIPs = 0.0.0.0/1, 128.0.0.0/2, 192.0.0.0/9, 192.128.0.0/11, 192.160.0.0/13, 192.168.1.0/24, 192.168.2.0/23, 192.168.4.0/22, 192.168.8.0/21, 192.168.16.0/20, 192.168.32.0/19, 192.168.64.0/18, 192.168.128.0/17, 192.169.0.0/16, 192.170.0.0/15, 192.172.0.0/14, 192.176.0.0/12, 192.192.0.0/10, 193.0.0.0/8, 194.0.0.0/7, 196.0.0.0/6, 200.0.0.0/5, 208.0.0.0/4, 224.0.0.0/3
```  
Router: 192.168.1.1
```
AllowedIPs = 0.0.0.0/1, 128.0.0.0/2, 192.0.0.0/9, 192.128.0.0/11, 192.160.0.0/13, 192.168.0.0/24, 192.168.2.0/23, 192.168.4.0/22, 192.168.8.0/21, 192.168.16.0/20, 192.168.32.0/19, 192.168.64.0/18, 192.168.128.0/17, 192.169.0.0/16, 192.170.0.0/15, 192.172.0.0/14, 192.176.0.0/12, 192.192.0.0/10, 193.0.0.0/8, 194.0.0.0/7, 196.0.0.0/6, 200.0.0.0/5, 208.0.0.0/4, 224.0.0.0/3
```
Router: 192.168.2.1
```
AllowedIPs = 0.0.0.0/1, 128.0.0.0/2, 192.0.0.0/9, 192.128.0.0/11, 192.160.0.0/13, 192.168.0.0/23, 192.168.3.0/24, 192.168.4.0/22, 192.168.8.0/21, 192.168.16.0/20, 192.168.32.0/19, 192.168.64.0/18, 192.168.128.0/17, 192.169.0.0/16, 192.170.0.0/15, 192.172.0.0/14, 192.176.0.0/12, 192.192.0.0/10, 193.0.0.0/8, 194.0.0.0/7, 196.0.0.0/6, 200.0.0.0/5, 208.0.0.0/4, 224.0.0.0/3
```  


