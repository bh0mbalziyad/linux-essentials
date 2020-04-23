# Linux Day 6
---
## Part 1 - `fsck`! & `xfs_repair`
---

- `fsck` - stands for file-system consistency check  
- used to recover corrupted disks and their partitions  
- replaced and phased out by `xfs_repair`  
- Needs partitions to be unmounted  
- To add a new block device without having to reboot -
  1. `ls -l /sys/class/scsi_host/`
  2. `grep mpt /sys/class/scsi_host/host??/proc_name`
  3. The host number which you get, let's call it `$number` 
  4. `echo '---' > /sys/class/scsi_host/host$number/scan`
  5. Now run lsblk again and you should see your new disk  

- To enable `fsck/xfs_repair` at boot time, set the sixth bit in the `/etc/fstab` file to eiter 1 or 2. It should be 2 for any device which doesn't mount at `/`

---
## Part 2 -- Redirection
---

1. Standard input - 0
2. Standard output - 1
3. Stanard error - 2
   
---
## Part 3 -- Recovering passwords
---

1. Stop at the GRUB screen and press 'e' to edit boot options
2. Read through and find the line starting with `linux` and that line's end add `rd.break` and then press ctrl+x to continue
3. This will halt the normal boot process and give you a terminal to interact with
4. Now use `mount -o remount,rw /sysroot`
5. Now chroot into `chroot /sysroot` which allows us to virtually change the root of our filsystem into `/sysroot`
6. Now you can use `passwd root` to change the password
7. Now create an empty file called `.autorelable` and `exit`
8. `exit` again to resume normal boot process