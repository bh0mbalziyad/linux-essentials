# Linux Day 5
---  
## Part 1 - `telnet`  
---  
Used for remote connection  
Similar to `ssh`  
Packages : `telnet`, `telnet-server`, `xinetd`  
Service : `telnet.socket`  

1. Install packages  
2. Go to config file `/etc/xinetd.conf` and make `disabled = no` on line 14  
3. Enable and restart `xinetd` and `telnet.socket`
4. Now connect from client machine using `telnet <ipaddr>`

---
## Part 2 - `squid`  
---  
Proxy server  
Used for filtering traffic  
Blocking sites within the network   
Config file : /etc/squid/squid.conf  
Packages : `squid`  
Port: 3128  
Log file : /var/log/squid/access.log  

1. Install packages on the server machine
2. Edit config file
   ```squid.conf
   acl localnet src <yoursubnet>
   http_access allow localhost manager localnet
   ```
3. Restart `squid` service
4. Configure client to use proxy as `<server-ip>:3128`
5. To disallow a website edit the config file as follows
   ```conf
   acl badsites url_regex "/etc/squid/badsites"
   http_access deny badsites
   ```
6. Now create the `/etc/squid/badsites` file
   ```
    facebook.com
    google.com
    ziyad-bhombal.web.app
   ```
7. Now restart the squid server and voila your mentioned sites are now blocked

---
## Part 3 - `DNS/ BIND`
---  
Packages : `bind`, `bind-utils`  
Port : 53/tcp, 53/udp  
Service : `named`  
Config : `/etc/named.conf`  


1. Assign IP addresses to client and server and make sure they can ping
2. Assign a domain based hostname to server
3. Disable `squid` server and add port 53/tcp to firewall or turn the firewall off
4. Open config file
   - make changes on line 11 and add your own IP  
   - on line 17 add any to allow querying  
5. Create two file in /var/named/ called `forward.e-corp.com` and `reverse.e-corp.com`
6. Add entries in /etc/named.conf
   ```
   zone "e-corp.com" IN {
       type master;
       file "forward.e-corp.com";
       allow-update {none;};
   }

   zone "10.168.192.in-addr-arpa" IN {
       type master;
       file "reverse.e-corp.com";
       allow-update {none;};
   }
   ```
7. cd into `/var/named` and look for `named.localhost`
8. Use that file to create `forward.e-corp.com` using `cp named.localhost forward.e-copr.com`  
   Contents of `forward.e-corp.com` will be  
   ![forward](https://imgur.com/zUXGQTS.png)
9. create copy of `forward.e-corp.com` and name it `reverse.e-corp.com`  
    it's contents will be  
    ![](https://imgur.com/sNK1Q3X.png)  
10. Change ownership to `root:named` for both the files
11. Now restart `named` service and set machines to use your server's IP as DNS
12. Set contents of `/etc/hosts` to default
13. Now you have a working DNS, to verify use `dig <domain-name>`  

---
Part 4 - SMTP  
---
Packages : `postfix`, `dovecot`, `squirrel`
Ports : 25/tcp  
Config : /etc/postfix/main.cf  

For an SMTP setup you need three things  
1. A Mail Transfer Agent - MTA  
2. A Mail User Agent - MUA  
3. A Mail Delivery Agent - MDA  

Begin setup -
1. On the mail server, you'll need DNS to be setup.
2. Go to the forward lookup zone and an entry
   ```
   svr   IN MX 10 svr.e-corp.com.
   ```
3. Now restart `named`
4. Install `postfix`
5. Go to the configuration file
   ```
   line75 (uncomment & write this)à myhostname = svr.alpha.corp

   line83 (uncomment & write this)à mydomain = alpha.corp

   line99 uncomment

   line113 uncomment

   line116 comment it.

   line164 (appendat last) mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain

   line264 mynetworks = 192.168.10.0/24, 127.0.0.0/8

   line419 (uncomment) home_mailbox = Maildir/
   ```
6. Start service `postfix`
7. Connect to smtp over telnet
8. Type 
   ```
   ehlo localhost
   mail from:<name>
   rcpt to:<name>
   data
   <msgdata>
   .
   quit
   ```
9. Now that postfix is working we'll install dovecot
