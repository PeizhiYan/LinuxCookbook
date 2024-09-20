[return to table](../README.md)

---

Mounting a new hard drive to a Linux system involves several steps. Here's a step-by-step guide:

1. **Identify the New Hard Drive:**
   - First, you need to identify the new hard drive. You can use the `lsblk` or `fdisk -l` command to list all the block devices and find your new drive.
   ```sh
   lsblk
   ```
   or
   ```sh
   sudo fdisk -l
   ```

2. **Partition the New Hard Drive:**
   - If your new hard drive is not already partitioned, you need to create a partition. You can use the `fdisk` or `parted` tool.
   ```sh
   sudo fdisk /dev/sdX
   ```
   Replace `/dev/sdX` with your actual device name (e.g., `/dev/sdb`).

   In `fdisk`:
   - Press `n` to create a new partition.
   - Press `p` for primary partition.
   - Choose the partition number and accept the default values to use the entire disk.
   - Press `w` to write the changes and exit.

3. **Format the New Partition:**
   - Once the partition is created, you need to format it with a filesystem. For example, to format it with the ext4 filesystem:
   ```sh
   sudo mkfs.ext4 /dev/sdX1
   ```
   Replace `/dev/sdX1` with your actual partition name.

4. **Create a Mount Point:**
   - You need to create a directory where you will mount the new hard drive.
   ```sh
   sudo mkdir /mnt/newdrive
   ```

5. **Mount the New Partition:**
   - Now, you can mount the new partition to the mount point you created.
   ```sh
   sudo mount /dev/sdX1 /mnt/newdrive
   ```

6. **Update /etc/fstab:**
   - To ensure the new drive mounts automatically at boot, you need to add an entry to the `/etc/fstab` file.
   - First, get the UUID of the new partition:
   ```sh
   sudo blkid /dev/sdX1
   ```
   - Open `/etc/fstab` in a text editor:
   ```sh
   sudo nano /etc/fstab
   ```
   - Add a line for the new drive:
   ```sh
   UUID=your-uuid-here /mnt/newdrive ext4 defaults 0 2
   ```
   Replace `your-uuid-here` with the UUID you got from the `blkid` command.

7. **Mount All Filesystems:**
   - Finally, test the `/etc/fstab` entry by running:
   ```sh
   sudo mount -a
   ```
   - If there are no errors, your new hard drive should be mounted and ready to use.

### Example:
Assuming your new hard drive is `/dev/sdb` and you want to mount it at `/mnt/newdrive`:
```sh
sudo fdisk /dev/sdb
# Create partition, write changes

sudo mkfs.ext4 /dev/sdb1
# Format partition

sudo mkdir /mnt/newdrive
# Create mount point

sudo mount /dev/sdb1 /mnt/newdrive
# Mount partition

sudo blkid /dev/sdb1
# Get UUID

sudo nano /etc/fstab
# Add line: UUID=your-uuid-here /mnt/newdrive ext4 defaults 0 2

sudo mount -a
# Test fstab entry
```


# Create a RAM Drive (data will be lost when power off!)

Example:
```
mount -o size=16G -t tmpfs none /mnt/ram
```



