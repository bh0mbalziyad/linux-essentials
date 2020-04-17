# Linux Day 4  
---
## Part 1 - The *hosts* file  
---
1. modify `/etc/hosts` file to manually bind IP addresses to names
2. ```bash
   192.168.10.5 svr.e-copr.com svr
   192.168.10.100 cli.e-corp.com cli
   ```
3. Save the file contents and you have temporary DNS in place

---
## Part 2 - SSH
---
Port : 22/tcp  
Packages : `openssh*`  

### Password based

1. Install the required packages `yum install -y openssh*`  
2. Enable, start and check `sshd`  
   ```bash
   systemctl enable sshd
   systemctl start sshd
   systemctl status ssh
   ```
3. Perform steps 1 & 2 on the client machine as well  
4. Now connect to the remote host using `ssh <username>@<ipaddr>` and you can now destroy everything  

### Password-less

1. Generate a new public-private key pair  
   `ssh-keygen -t rsa -b 4096`
2. Enter filename to save the key in 
3. Enter passphrase if you want for added security  
   This is what this process looks like  
   ![ssh-keygen](https://imgur.com/rJy6bFF.png)  
4. Now copy the public key onto the server you want to remotely manage
   `ssh-copy-id <username>@<ipaddr>`  
   ![Copy ID](https://imgur.com/jJIn56Z.png)
5. Now try logging in again using `ssh <username>@<ipaddr>` and it won't ask you for a password this time  