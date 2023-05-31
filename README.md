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
Password: Copy/Paste password from Vultr server page into PuTTY password prompt (*Hint: Use right click to paste into PuTTY)  

# Wireguard installation on Vultr server 

After logging in to your Vultr server, run the 3 commands below by doing a copy/paste/enter sequence   
(Hint: Use right click to paste each line into PuTTY)  
```
curl -O https://raw.githubusercontent.com/jakeloftis/wireguard/master/wireguard-install-open.sh
```
```
chmod +x wireguard-install-open.sh
```
```
./wireguard-install-open.sh
```
Press enter through each prompt that pops up until you're prompted for a client name  
Enter a client NAME you will remember  
If you encounter a (pink/purple) popup screen about out of date daemons or restarting daemons,  
Use the arrow keys to navigate up and down, then press space bar to put a * next to each service  
Press tab to switch to okay and press enter

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
Port: In the example below, port is 3074  
IP Address and Port of client: In the example below, my Windows Wireguard Client and Port are 10.66.66.2:3074  
```
iptables -t nat -A PREROUTING -i enp1s0 -p tcp --dport 3074 -j DNAT --to-destination 10.66.66.2:3074
```
To forward a range of ports
```
iptables -t nat -A PREROUTING -i enp1s0 -p tcp --dport 3074:3079 -j DNAT --to-destination 10.66.66.2:3074-3079
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
Example Server Config: https://github.com/jakeloftis/wireguard/blob/main/example-server-config.md  
  
Example Client Config: https://github.com/jakeloftis/wireguard/blob/main/example-client-config.md  
  
If you want to add, list, revoke, or uninstall Wireguard, you can access the menu by entering the following commands:  
```
curl -O https://raw.githubusercontent.com/jakeloftis/wireguard/master/wireguard-install-open.sh
```
```
chmod +x wireguard-install-open.sh
```
```
./wireguard-install-open.sh
```
