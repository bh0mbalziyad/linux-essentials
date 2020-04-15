# Linux Day 1
---
## Part 1 - *The presentation*
---
- ### `chvt` - Change Virtual Terminal
- ### Why to **choose** Linux
  ![Why to choose linux](https://imgur.com/rXGe1xr.png)
- ### **SELinux** - **S**ecurity **E**nhanced **Linux**
- ### Portability - Being able to use **Live versions** of Distros with **persistence on USB**
- ### TUX - **T**orwalds **U**ni**x**, the mascot of GNU/Linux
- ### Linux Environments
  ![Linux Environments](https://imgur.com/5Xwwjeo.png)
- ### Linux Distributions
  ![Linux Distributions](https://imgur.com/kyQjx6w.png)
- ### Linux principles
  1. Everything is a file
  2. KISS - **K**eep **I**t **S**imple **S**tupid
  3. Chaining programs together to perform complex tasks
  4. Avoid captive user interfaces -  Commands expect arguments instead of asking users for what they want to do
  5. Configuration is stored text files
- ### GPT 
  - 128 Primary paritions
  - 18 EiB partition size
- ### GRUB - GRand Unified Bootloader
  - RHEL 6 - GRUB 0.9
  - RHEL 7 - GRUB 2.0
- ### init vs. systemd
  RHEL 6 uses init and RHEL 7 uses [systemd](https://www.linode.com/docs/quick-answers/linux-essentials/what-is-systemd/).  
  ![init vs systemd](https://imgur.com/LSJrIWb.png)
- ### HFS - Heirarchial File System 
  - `/bin` - binaries for normal users
  - `/sbin` - binaries for sudoers
  - `/etc` - config files
  - `/boot` - mount point for efi parition
  - `/kernel` - ???
  - `/mnt` - temporary mount points
  - `/dev` - device files
  - `/run` - similar to `/mnt` in other distros
  - `/tmp` - temporary files
  - `/var` - primarily contains log files
  - `/lib or /libx64` -  library files
  - `/home` - home directory for users other than root
  - `/root` - home directory for root users
  - `/proc` - vfs for process related and kernel related information
  - `/usr` - holds user installed packages
- ### RHEL 6 vs. RHEL 7  
  ![RHEL 6 vs 7](https://imgur.com/33VAyZy.png)  
  User ID allocation  
  ![UID allocation](https://imgur.com/v9KbzoK.png)
---
## Part 2 - *The installation*
---
1. Screen 1  
   ![Screen 1](https://imgur.com/iIPLFaa.png)
2. **Anaconda** the installer for RHEL will then start. **Donot** press enter, it will begin automatically
3. Contains 18 Indian languages
4. Changes time zone based off selected language
5. Choose installation source and choose auto-detect
6. Choose software installtion type
   - Minimal install - Pure command line
   - Infrastructure Server - 
   - File and Print server
   - Basic Web Server
   - Virtualization host
   - Server with GUI - install everything
7. Select Server with GUI and choose to install all the packages
8. Choose a computer name in the localhost text entry  
   ![Imgur](https://i.imgur.com/SiERfR9.png)  
9. Setup networking details by manually assigning IP
10. Select installation medium  
    ![Installation Medium](https://imgur.com/LSFIqc8.png)
11. Select manual partioning  
    ![partioning scheme](https://imgur.com/PItYwHz.png)
12. Select standard partioning and setup mount points
    - / - 35 GB
    - /boot - 500 MB 
    - /swap (2 x RAM) - 4GB
13. Begin installation
14. Setup root password
15. Create your own user  
    ![User account](https://imgur.com/JZEt4ap.png)
16. Wait for installation to complete  
    ![Wait...](https://imgur.com/CvxIVDv.png)
---
## Part 3 - *Post installation*
---
1. GRUB Menu  
   ![GRUB](https://imgur.com/uuKvSMI.png)
2. Accept the License Agreement
3. KDump - Useful for root cause analysis incase of kernel panic/failure. Reserves portion of hard disk space for storing logs related to this event
   ![KDump](https://imgur.com/AaOsVZL.png)
4. Under subscription managment, choose register later and proceed
5. Login with root user
6. Installation of OS is now complete  
   ![GNOME](https://imgur.com/Q4EDoyv.png)
---
## Part 4 - Finding your feet
---
- Explore the GUI
- Launch the terminal
  - `whoami` - print current user's name
  - `hostname` - print hostname
  - `pwd` - print working directory
  - `su - username` -  switch to specified user's Home directory
  - `tty` -  prints current terminal info  
    ![TTY](https://imgur.com/0cl9k0S.png)
  - `who` - lists users who are currently logged in  ![lists](https://imgur.com/UVCbgGD.png)   
  - `hostnamectl` - changing hostname/computer name. You'll have to reload the terminal for changes to reflect![](https://imgur.com/8gl7gjd.png)
  - `ifconfig` - lists interface config ![ifconfig](https://imgur.com/iGYhrzK.png)
---
## Part 5 - File Management
---  
![Tasks](https://imgur.com/SCodUj0.png) 

### Text Editors in Linux
- `touch` - Creates empty files, updates time stamp. Creates empty file if file doesn't exist, if it exists then update its timestamp.
- `cat` - create, view, append, overwrite files.
  - create - `cat > file.txt` Press **Ctrl+D** to exit write mode
  - view - `cat file.txt`
  - append - `cat >> file.txt`
  - overwrite, if file exists - `cat > file.txt`  
- `vi`/`vim` - text editor as complex as an OS within itself
  - Learn vi commands
  - Press i to go in insert mode
  - Update the file
  - Press colon and then `wq!` to exit while saving contents
- `gedit` - GUI based text editor
- `nano` - minimal CLI based text editor

### Deleting files
`rm <filename>`
`rm -f *.<extension>` delete files with a certain extension. `-f` for forefully
### Copying files
`cp <source> <destination>`
> `cp originalfile filecopy`
### Path
- Relative path - path which begins from your current working directory
- Absolute path - path which begins from the `/`
### Moving files
`mv <source> <destination>`
> `mv /root/Desktop/t1 /home/<username>/Desktop/`
### Renaming files
`mv oldname newname`
> `mv /root/Desktop/fileoldname /root/Desktop/filenewname`
### Hide files
`mv <originalfile> .<originalfile>`
> `mv config .config`
### Unhide files
`mv .<originalfile> <originalfile>`
> `mv .config config`
### . and ..
**.** is current directory
**..** is directory above the current directory in the heirarchy
### Creating directories
`mkdir <dirname>`
### Deleting directories
- For **empty** directories
`rmdir <dirname>`
- For **non-empty**
`rm -rf <dirname>`
---
## File Permissions
---
For file/directory  
- r - read (4)  
- w - write (2)  
- x - execute (1)
- all perms (7)

To check permissions
>`ls -l`

![Information](https://imgur.com/Y0gHsoG.png)
- Column 1
  > 10 bits
  1st bit is file type
  next 3 bits are permissions for file owner
  next 3 bits are permissions for owner's group
  last 3 bits are permissions for every user
- Column 2
  > Nuber of links occupied in storage
- Column 3
  > owner of the file/directory
  For **changing** user
  `chown <username> <file/dir>`
- Column 4
  > group owner of the file/directory
  For **changing** group
  `chgrp <groupname> <file/dir>`
  `chown <username>:<groupname> <file/dir>`
  `chown :<groupname> <file/dir>`
- Column 5
  > File/dir size in bytes
- Column 6
  > Timestamp of file/dir
- Column 7
  > Name of file/dir
  
To change permissions
> `chmod <perms> -[arguments] file/dir`
> The permissions can be either 3 octal numbers or alphabet based
> u - user
> g - group
> o - others
> r - read, w - write, x - execute
> 4 - read, 2 - write, 1 - execute
> `chmod 666 <file>` - r,w for u,g and o
> `chmod 700 <file>` - r,w for u and - for g and o
> `chmod g+r,w <file>` - gave r,w to g
> = - assigns new values
> \+ - appends to existing permissions

---
# Part 6 - User management
---
Important Files
- `/etc/passwd` 7 columns-  
  >1. username
  >2. referenced password
  >3. userID
  >4. groupID
  >5. User GECOS - can be changed with `chfn`
  >6. Default Home Dir for user
  >7. Default Shell of the user
- `/etc/shadow` 9 Columns
  > 1. Login name
  > 2. Encrypted password
  > Encrypted with (MD5 or SHA512)
  > \$6 - SHA512
  > \$1 - MD5
  > To change algo use `authconfig --passalgo=[md5s|ha512] --update`
  > To check algo use `authconfig --test | grep hashing`
  > 3. Last password change in number of days since 1st Jan 1970
  > 4. Minimum password age in days ( if 1 then password can't be changed for 1 day)
  > 5. Maximum password age in days (default is 99999 days ~ 273 years)
  > 6. Warning days from which you start getting a prompt to change your password
  > 7. Inactive (if kept 1 then you can still log in for 1 day after your password has expired)
  > 8. Account Expiry - ???
  > 9. Not used till date - ???
  
+ Creating a user
  +  `useradd <options> <username>` 
  +  Creates entries in four files
     - `/etc/passwd`
     - `/etc/shadow`
     - `/home/<username>`
     - `/var/mail`  
+ Modify a user
  + `usermod <options> <username>`
+ Delete a user
  + Partial Deletion
    - `userdel <username>`
    - Only removes entries from `/etc/shadow` & `/etc/passwd`
  + Full Deletion
    - `userdel -r <username>`
    - Removes entries from all the places 
+ Creating a group
  +  `groupadd <options> <groupname>`  
+ Modify a group  
  + `groupmod <options> <groupname>`  
+ Delete a group  
  + Partial Deletion  
    - `groupdel <groupname>`  