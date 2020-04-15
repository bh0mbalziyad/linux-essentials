# Linux Day 2
---
## Part 1 - *Commands*
---
Finding text in files `grep <regexp> <path-to-file>`  
Finding files/directory `find <dir-to-search-onwards> [args]`
- For files `find <dir-to-search-onwards> -name <filename>`
- For dirs `find <dir-to-search-onwards> -t d -name <dirname>`
---  
## Part 2 - sudo users 
---  
They are normal users + **limited** admin rights
`/etc/sudoers` - contains list of sudo users  

To edit this file use (requires elevated permissions) -
- `vim /etc/sudoers`
- `visudo`  
- `EDITOR = vim sudoers`  

Understanding entries in `/etc/sudoers`  
![sudoers](https://imgur.com/YMmEIt1.png)  
- Col 1 - *username*
- Col 2 - *machine=terminal* (from *which terminal* on *which machine* can this be allowed)
- Col 3 - commands given to the user

To add a new sudo user, edit the `/etc/sudoers` file and an entry for the new user below line 99 in the above explained format  

When a user tries be to a sudo user without the proper permissions a mail will be sent to the root user in `/var/mail/root` which you can read with `mail <maildir>`  

To list the commands allowed to execute for a sudo user, use `sudo -l`  

To specify which commands a user can execute use an alias from within the file like `NETWORKING`  or `SOFTWARE` and separate them with commas  

An example would be `u5    ALL=(ALL)    NETWORKING,SOFTWARE` which gives u5 perms to execute networking and software related commands on **all terminals** on **all machines**  

You can also have custom aliases by add a line in the `/etc/sudoers` file above the username entry in the following format  

`Cmnd_Alias <ALIASNAME>=[command1], [command2], ....`  

---
## Part 3 - Package Management
---  
## Source of installation
- Local repo
- Internet based repo

Redhat DVD (Local, free)  
Redhat Satellite Server (Online, paid)
CentOS DVD (Local, free)
CentOS Mirror Server (Online, free)  

## Package Manager - install, remove, update packages  
- RPM 
  - Redhat Package Manager
  - extensions : .rpm
- YUM
  - Yellowdog Update Manager
  - A server that **needs to be installed**
  - extensions : .rpm
- APT 
  - Advanced Packaging Tool
  - extensions : .rpm, .deb, .dpkg
- DPKG
  - Debian package manager
  - extensions : .deb, .dpkg

## Naming convention of packages
![zip](https://imgur.com/lThiqBI.png)  
**zip** - name of package  
**3.0** - major release/version  
**10.el7** - minor version/release  
**x86_64** - architecture of the package  
**.rpm** - extension  

## Architecture of packages  
32 bit - i686
64 bit - x86_64
noarch - supports both 32 and 64 bit

## Installtion using RPM
`rpm -ivh <package-name>`  
- i - install
- v - verbose
- h - hashing
  > shows package installation completion bar
  #######
## Listing installed packages
`rpm -q <package-name>`
- q - query  

`rpm -qa`  
- a - all  

## Updating packages
`rpm -Uvh <packagename>`  
- U - update  

## Removing packages
`rpm -ev <packagename>`
- e - erase