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
### Source of installation
- Local repo
- Internet based repo

Redhat DVD (Local, free)  
Redhat Satellite Server (Online, paid)
CentOS DVD (Local, free)
CentOS Mirror Server (Online, free)  

### Package Manager - install, remove, update packages  
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

### Naming convention of packages
![zip](https://imgur.com/lThiqBI.png)  
**zip** - name of package  
**3.0** - major release/version  
**10.el7** - minor version/release  
**x86_64** - architecture of the package  
**.rpm** - extension  

### Architecture of packages  
32 bit - i686
64 bit - x86_64
noarch - supports both 32 and 64 bit

### Installtion using RPM
`rpm -ivh <package-name>`  
- i - install
- v - verbose
- h - hashing
  > shows package installation completion bar
  #######
### Listing installed packages
`rpm -q <package-name>`
- q - query  

`rpm -qa`  
- a - all  

### Updating packages
`rpm -Uvh <packagename>`  
- U - update  

### Removing packages  
`rpm -ev <packagename>`  
- e - erase  

### Problems with RPM
1. Full names of package is required
2. Location of package is required
3. Dependency issues - doesn't resolve dependencies automatically  

## yum
### Setting up yum  
1. Stop firewall and turn off SELinux  
   `systemctl stop firewalld`  
   `setenforce 0`  
2. Check whether follwing packages are installed  
   `rpm -q createrepo deltarpm python-deltarpm vsftpd`
3. Create a local repo folder  
   `mkdir /var/ftp/pub/yumserver`
4. Copy contents from ISO/DVD into above folder  
   `cp -var /path-to-DVD/* /var/ftp/pub/yumserver`
5. Create yum/client answer file  
   - remove files present in `/etc/yum.repos.d/`
   - then run `vim <reponame>.repo` to create a new repo file  
   - contents of the file will be in the following format
   ``` yaml  
    [reponame]  
    name='Repo Label'  
    baseurl=ftp://<your-ip-address>/pub/yumserver  
    gpgcheck=0  
    enabled=1  
   ```
6. Create indexes for packages  
   `createrepo -v /var/ftp/pub/yumserver/`  
7. Start vsftp daemon  
   ```bash  
   systemctl enable vsftpd  
   systemctl start vsftpd  
   systemctl status vsftpd  
   ```  
8. Clean, update and verify  
   ```bash  
   yum clean all  
   yum update all  
   yum repolist  
   ```  
### Installing with yum
`yum install <package-name>`  
or  
`yum install -y <package-name>`  
### Querying with yum
`yum info <package-name>`  
### Querying with yum
`yum update <package-name>` to update specific packages  
or  
`yum update` to update all the packages  
### Removing with yum  
`yum remove <package-name>`  

---
## Part 4 - Partitioning
---
### Simple Partitioning  
1. create paritions on physical disks using `fdisk /dev/sd[a-z]`  ![partitions](https://imgur.com/hH3hat0.png)  
2. Put a file system on the disk using `mkfs`  ![FS on LV1](https://imgur.com/mdDLUgS.png)  
3. Create a mount point for the partition using `mkdir <dirname>`  
4. mount the partition  
   - Permanent - editing `/etc/fstab` file  ![fstab](https://imgur.com/eW599i8.png)  
   - Temporary - using `mount` and `umount`  ![manual](https://imgur.com/AtSxRQj.png)  

### Logical Volumne Management (LVM)  
![LVM Intro](https://imgur.com/xogjVFM.png)
- Format disks with `fdisk`  
- pv - `pvcreate`, `pvdisplay`, `pvremove`  
- vg - `vgcreate`, `vgdisplay`, `vgremove`
- lv -  `lvcreate`, `lvs/lvdisplay`, `lvremove`


1. create paritions on physical disks using `fdisk /dev/sd[a-z]`  
![partitions](https://imgur.com/hH3hat0.png)  
2. `pvcreate /dev/sd[a-z][0-9] ...` to create physical volumes from partitions  ![pvcreate](https://imgur.com/pQDWn6s.png)  
3. `pvdisplay` to check creation of physical volume  ![pvdisplay](https://imgur.com/n6Al4VP.png)  
4. `vgcreate <volume-group-name> <pv-names>`  ![vgcreate](https://imgur.com/amIiexL.png)  
5. `lvcreate -n <lv-name> -L <length/size> <vg-name>`  ![LV creation](https://imgur.com/Y9hqNWJ.png)  
6. make a filesystem on the logical volumne  ![FS on LV1](https://imgur.com/mdDLUgS.png)  
7. create a mount point using `mkdir <dirname>`  
8. mount the volume  
   - Permanent - editing `/etc/fstab` file  ![fstab](https://imgur.com/eW599i8.png)  
   - Temporary - using `mount` and `umount`  ![manual](https://imgur.com/AtSxRQj.png)  

### RAID
1. `yum install -y mdadm`  
2. Format disks `/dev/sdd and /dev/sde` as shown above
3. Change the parition's system id with `fdisk --> t` to `fd` which is *Linux raid autodetect*  ![RAID autodetect](https://imgur.com/UTiDUBo.png)  
![RAID Autodetect](https://imgur.com/ZjbKRSu.png)  
4. Examine the disks using `mdadm -e /dev/sdd /dev/sde`  
5. Check existing RAID arrays using `cat /proc/mdstat`  
6. Create a **RAID 0** array called `md0` with the command  
   `mdadm --create --verbose /dev/md0 --level=0 --raid-devices=2 /dev/sdd /dev/sde`  ![Results](https://imgur.com/Slvipdw.png)
7. Ensure that the RAID array was setup properly using `cat /proc/mdstat`  ![checkup](https://imgur.com/HnztCtk.png)
8. Now create a filesystem on `/dev/md0` using `mkfs.xfs /dev/md0`  ![MKFS](https://imgur.com/GgVfoc5.png)  
9. Create a mount point for it `mkdir -p /RAID/ar1` and mount it using `mount /dev/md0 /RAID/ar1`  ![MOUNT FS](https://imgur.com/3dK9xCD.png)  
10. Check whether new space is available using `df -h`  ![DF](https://imgur.com/nRm2CHj.png)
11. Save the array layout to `/etc/mdadm/mdadm.conf` so that your array is reassembled on the next reboot. You can do so using `mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf`  
12. Update `/etc/fstab` for persistence and auto-mounting  
    `echo '/dev/md0 /RAID/ar1 xfs defaults,nofail,discard 0 0' | sudo tee -a /etc/fstab`  ![fstab](https://imgur.com/qlKpEEN.jpg)  