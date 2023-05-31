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

# port forwarding examples <br />
To forward a single port
```
iptables -t nat -A PREROUTING -i enp1s0 -p tcp --dport 3074 -j DNAT --to-destination 10.66.66.2:3074
```
To forward a range of ports
```
iptables -t nat -A PREROUTING -i enp1s0 -p tcp --dport 3074:3079 -j DNAT --to-destination 10.66.66.2:3074:3079
```
