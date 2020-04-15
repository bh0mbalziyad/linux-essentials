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
