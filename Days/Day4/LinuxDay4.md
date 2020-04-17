# Linux Day 4  
---
## Part 1 - The `/etc/hosts` file  
---
1. modify `/etc/hosts` file to manually bind IP addresses to names
2. ```bash
   192.168.10.5 svr.e-copr.com svr
   192.168.10.100 cli.e-corp.com cli
   ```
3. Save the file contents and voila, you have temporary DNS in place

---
## Part 2 - `ssh`
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
4. Now copy the public key onto the remote server you want to manage  
   `ssh-copy-id <username>@<ipaddr>`  
   ![Copy ID](https://imgur.com/jJIn56Z.png)
5. Now try logging in again using `ssh <username>@<ipaddr>` and it won't ask you for a password this time  

---  
## Part 3 - `scp`  
---  
`scp <source> <destination>`  
Both source and destination can be remote/local
1. Copy from remote to local
   ![Case1](https://imgur.com/sfetsa3.png)
2. Copy from local to remote
   ![Case2](https://imgur.com/w25FBxo.png)

---  
## Part 4 - `rsync`  
---  
Packages : `rsync`  
- remote sync  
- copy files to-from local & remote machines  
- faster than scp  
- uses less bandwidth  
- performs incremental copying for prexisting files  
Syntax : `rsync -[args] source destination`  
![rsync](https://imgur.com/LDlXH8b.png)  
`rsync -zvh txt{1..5}.txt root@svr:/root`  
  - z - compress  
  - v - verbose  
  - h - human readable  

To preserve file perms use the `-a` flag  
To use a particular method/service with rsync use `-e SSH` for example  
![rsync with ssh](https://imgur.com/HdnCY1H.png)  

---  
# Part 5 - NFS  
---  
File : `/etc/exports`  
Port : 2049/tcp  
Packages : `nfs-utils`, `rpcbind`  
### Setup for NFS server  
1. Install required packages
2. Create directory to share `mkdir /nfs; cd /nfs; touch txt{1..5}.txt` & change modifications `chmod -R 777 /nfs`  
3. Edit the config file `vim /etc/exports`
   ```conf
   [what-to-share]  [with-whom-to-share(share-permissions)]
   /nfs   *(no_root_squash,rw,sync)
   ```
   - no_root_squash - give root perms to users in the share
   - rw - give read and write access
   - sync - verbatim
4. Add ports `2049/tcp, 2049/udp, 111/tcp` to firewall
5. Add service `nfs, rpc-bind, mountd` to firewall
6. Run `rpcbind` service
7. Run `nfs-server.service`

### Setup for NFS client  
1. Install required packages
2. Create a mount point for NFS share `mkdir -p /mnt/NFS`
3. Use `showmount -e <ipaddr>` to see available exports
   ![Showmount](https://imgur.com/MIm0jqJ.png)
4. Mount using `mount <ipaddr>:/nfs /mnt/NFS`
5. Check mount using `lsblk`  

---  
### Part 6 - Samba Server  
---  
Packages : `samba-client`, `samba`, `cifs-utils`  
Ports : 445, 139
Config file : `/etc/samba/smb.conf`  

1. Install packages
2. Add service to firewall `samba` 
3. Create a group and add users to it which will be allowed to access the share
   ```bash
   group add grp1
   useradd -G grp1 u1
   useradd -G grp1 u2
   useradd -G grp1 u3
   ```
4. Provide passwords to the users using `passwd <username>`
5. Create a directory to be shared `mkdir -p /samba`
6. Change modifications `chown -R root:grp1 /samba`
7. Open config file with `vim /etc/samba/smb.conf`
   ```conf
   [mysamba]
   comment=this is samba shared directory
   browseable=yes
   path=/samba
   valid user=@grp1
   writeable=yes
   ```
8. Verify whether config file works with `testparm`
9. Add users to the sambalist
    ```
    smbpasswd -a u1
    smbpasswd -a u2
    smbpasswd -a u3
    ```
10. Start `smb` service
11. Verify for u1 from server
    `smbclient -L localhost -U u1`
12. To check on Linux client install `samba`, `samba-client`, `cifs-utils`
13. To mount use `mount //<ipaddr>/<share-name> /<mountpoint> -o user=<username>`  

---  
## Part 7 - Apache Web Server  
---  
Web server in Linux  
Used for hosting websites on Intranet or Internet  
Packages : `httpd`  
Service : `httpd`  
Port : 80/tcp  

1. Install the packages
2. Installs a service account called `apache`
3. Start the `httpd` service
4. To test whether it is working or not use `curl localhost`
   ![](https://imgur.com/Xx7eL12.png)