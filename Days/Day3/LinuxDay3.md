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


