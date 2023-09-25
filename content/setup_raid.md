[return to table](../README.md)

---

Suppose I have two hard drives ```/dev/sda``` and ```/dev/sdb```, I want to setup a RAID system on these two drives.

**Step 1:**
> Use the following command to format the physical drive:
> ```
> gdisk /dev/sda
> ```
> Follow the following example to proceed:
> ```
> Command (? for help): n
> Partition number (1-128, default 1):
> First sector (2048-3907029134, default = 2048) or {+-}size{KMGTP}:
> Last sector (2048-3907029134, default = 3907029134) or {+-}size{KMGTP}:
> Current type is 'Linux filesystem'
> Hex code or GUID (L to show codes, Enter = 8300): fd00
> Changed type of partition to 'Linux RAID'
>
> Command (? for help): p
> Disk /dev/sda: 3907029168 sectors, 1.8 TiB
> Model: ST2000DM001-1ER1
> Sector size (logical/physical): 512/4096 bytes
> Disk identifier (GUID): F81E265F-2D02-864D-AF62-CEA1471CFF39
> Partition table holds up to 128 entries
> Main partition table begins at sector 2 and ends at sector 33
> First usable sector is 2048, last usable sector is 3907029134
> Partitions will be aligned on 2048-sector boundaries
> Total free space is 0 sectors (0 bytes)
> 
> Number Start (sector) End (sector) Size Code Name
> 1
> 2048 3907029134 1.8 TiB FD00 Linux RAID
>
> Command (? for help): w
> 
> Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
> PARTITIONS!!
>
> Do you want to proceed? (Y/N): y
> OK; writing new GUID partition table (GPT) to /dev/sda.
> The operation has completed successfully.
> ```
>
> Repeat the above operation to format ```/dev/sdb``` as well.
>
> Once done, you can check the corresponding partitions:
> ```
> ls /dev/sd[a-b]*
> ```
> The output should include ```/dev/sda```, ```/dev/sda1```, ```/dev/sdb```, ```/dev/sdb1```.
>

**Step 2**
> Create the RAID using **mdadm** (Note: level=0 refers to RAID-0; level=1 refers to RAID-1):
> ```
> sudo mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sda1 /dev/sdb1
> ```
>

**Step 3**
> Make the filesystem:
> ```
> mkfs.ext4 -F /dev/md0
> ```

**Step 4**
> Create the mount location. For example:
> ```
> sudo mkdir /mnt/raid1_data1
> ```

**Step 5**
> Check the UUID of the new device (```/dev/md0```):
> ```
> ls -l /dev/disk/by-uuid
> ```
>
> Then, edit the ```/etc/fstab``` to add the mount for the new RAID device.
> NOTE: be sure to use the UUID rather than the device file ```/dev/md0```!!
> Example:
> ```
> UUID=7bd945b4-ded9-4ef0-a075-be4c7ea246fb /mnt/raid1_data1 ext4 auto 0 0
> ```
> , where ```/mnt/raid1_data1``` is the mount location.

**Step 6**
> Reboot the system, and check whether the RAID can be automatically mounted on reboot.
> You can use X11 application ```filelight``` or use ```df -H``` to check if the RAID drive shows up.




