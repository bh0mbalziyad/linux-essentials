# Linux Day 3  
---
## Part 1 - Networking
---  
To configure network interfaces we can use the following utilities   
`iconfig`  
`nmcli`  
`nmtui`  

Or find the file called `ifcfg-<interfacename>` under `/etc/sysconfig/network-scripts`  

For changes to reflect, restart the `network` service. In some instances, restarting the system might be required  

---
## Part 2 - Services  
---  
Actions peformed on services are -  
- enable
- disable
- start
- stop
- restart  

Command to deal with services in RHEL 7+ is `systemctl <action> <service>`  

You can also use `service <service> action` which is a legacy utility. It executes as a wrapper around `systemctl`.  

---
## Part 3 - Firewall
---  
Utilities for managing firewall -  
`iptables` - IPv4  
`ip6tables`  -  IPv6 traffice  
`netfilter` - IPv4  
`firewall-cmd` - RHEL7

### `iptables`  

To list rules use `iptables -L`  
To flush rules use `iptables -F`, you need to be root to execute  

To accept traffic `iptables -I -s <source-ip> -j <action>`
- I - insert
- s - source
- j - jump
- action - ACCEPT, REJECT, DROP

To block SMTP protocol  
`iptables -I INPUT -s 192.168.11.0/24 -p tcp --destination-port 25 -j DROP`  ![firewall](https://imgur.com/0WQrXUL.png)  
- p - protocol `tcp/udp`
- destination-port -- which port the packet is headed to  

To save rules use `iptables-save > /etc/iptables/rules.v4` (RHEL6)  

### `firewall-cmd`  

`firewall-cmd --get-zones` - list all available zones  
`firewall-cmd --list-all` - list default zone and its settings  
![listing](https://imgur.com/O70AyBK.png)  
`firewall-cmd --zone=<zone-name> --list-all` - list details of a particular zone  
`firewall-cmd --set-default-zone=<zone-name>` - change the default zone. You might want to flush existing rules if you face an error during this.  
`firewall-cmd --reload` - reload changes made  
`firewall-cmd --add-service=<service-name>  --permanent` - to add a service to the rules  
`firewall-cmd --add-port=<portnumber>/<protocol-tcp/udp> --permanent` - to add a specific port number  
`firwall-cmd --add-source=<range-ip-address> --permanent` - to allow IP range through firewall  

---
## Part 4 - NTP
---  

Required package - `ntp`  
Config file - `/etc/ntp.conf`  
1. Get time servers for your region from [here.](https://ntppool.org)  
2. Add them to the config file with `vim /etc/ntp.conf` and comment out previous entries  
![Init file](https://imgur.com/A7g0SlE.png)  
![Changed file](https://imgur.com/hgwhMxj.png)  
3. Add your IP range to allow systems on your network to act as time providers    
![IP Range](https://imgur.com/jSxThcV.png)  
4. Add `logfile /var/ntp/ntp.log` to capture ntp logs  
5. Add service to firewall `firewall-cmd --add-service=ntp --permanent && firewall-cmd --reload`  
6. Enable ntpd using `systemctl enable ntpd`  
7. Start ntpd using `systemctl start ntpd`  
8. To list servers which are providing you time sync use `ntpq -p`  
9. To manually sync with a server use `ntpdate -u <server>`  

---
## Part 5 - DHCP  
---
Port no : 67/udp, 68/udp  
Packages : `dhcp`  
![Setup](https://imgur.com/XNn9k5c.png)  
1. Install `dhcp` with `yum install dhcp -y` 
1. Get default config file from `/usr/share/docs/dhcp-*/dhcpd.sample~` to `/etc/dhcp/dhcpd.conf`  
2. Make the following changes  
   ![Server name](https://imgur.com/CZySSQr.png)  
   ![Scope](https://imgur.com/3naebUR.png)  
3. Save changes and enable, restart and check status for `dhcpd`
   ```bash
   systemctl enable dhcpd  
   systemctl start dhcpd  
   systemctl status dhcpd  
   ```
4. Configure firewall to allow udp ports 67, 68 and dhcp service  
   ```bash
   firewall-cmd --add-port={67..68}/udp  
   firewall-cmd --add-service=dhcp
   firewall-cmd --reload
   firewall-cmd --list-all
   ```  
5. Now head over to your client machine on the same subnet and change its settings so that it can use `dhcp`  
   ![Settings](https://imgur.com/rpzNNxF.png)  
   ![Result](https://imgur.com/6H5alwt.png)  
6. To check lease information, use `cat /var/lib/dhcpd/dhcpd.leases`  
   ![Leases](https://imgur.com/C6TlmwT.png)  


---
## Part 6 - FTP
---  
### Insecure  
Port no : 21/tcp, 20/tcp  
Packages : `ftp`, `vsftpd`  

1. `yum install -y ftp vsftpd`  
2. Enable, start and check status for `vsftpd`  
3. Make ftp service and port entry in firewall
   ```bash
   firewall-cmd --add-port={20..21}/tcp --permanent
   firewall-cmd --add-service=ftp --permanent
   firewall-cmd --reload
   firewall-cmd --list-all
   ```  
4. Place files in the `/var/ftp` dir to setup files to serve  
5. Now open firefox `firefox &` and visit `ftp://192.168.10.5` and voila! 


### Secure  
1. Make sure `vsftpd` is installed and running  
2. Edit `/etc/vsftpd/vsftpd.conf`
   ```conf
   anonymous_allow=NO
   userlist_file=/etc/userlist
   userlist_deny=NO
   userlist_enable=YES
   ```
3. Only users in `/etc/userlist` will now be able to access

### FTP commands  
`get <filename>` - download file from remote server to local machine  
`put <filename>` - send file from local machine to remote server  
`ls` - list files in server  
`bye` - exit ftp server  

---
## Part 7 - SSH  
---