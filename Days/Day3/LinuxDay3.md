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