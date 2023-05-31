# Vultr VPS Server Deployment  
Create an account at https://my.vultr.com/
```
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
Download PuTTY at https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html  
OR  
I recommend Direct Download Link for exe/portable version: https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe  
```
Launch PuTTY
Log in to Vultr: https://my.vultr.com/
Select Server
Copy/Paste IP Address from Vultr to "Host Name (or IP address)" text box in PuTTY
Click "Open"
Username: root
Password: Copy/Paste password from Vultr server page (*Hint: Use right click to paste into PuTTY)
```

# wireguard installation on Vultr server 

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
```
Press enter through each prompt that pops up until you reach the client name
Enter a client NAME you will remember
```

# client config you will need for Windows Wireguard desktop app
```
Run the following command and replace NAME_FROM_SETUP_WIZARD with the name you entered in the setup wizard
cat /root/wg0-client-NAME_FROM_SETUP_WIZARD.conf
Select all text and Copy/Paste to a Notepad file
In Notepad, click File > Save
Change the "File name:" to wgclient.conf
Change the "Save as type:" to all files
Save
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

# download and install Windows Wireguard desktop app  
https://www.wireguard.com/install/ > Download Windows Installer
```
Install the application
Launch
Click "Add Tunnel"
Navigate to your wgclient.conf you saved in Notepad
Click Open
Click "Activate"
```
