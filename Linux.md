# Linux

## Format a USB Drive

1. Show all partitions:

```console
sudo lsblk -o UUID,NAME,FSTYPE,SIZE,MOUNTPOINT,LABEL,MODEL
```

2. Identify the name of the disk partition that points to your storage device (eg. `sda`),
and start `fdisk`:

```console
sudo fdisk /dev/sda
```

3. Create a new partition using fdisk

```
    Command (m for help): g (creates a new GPT partition table)
    Command (m for help): n (creates a new partition)
    Partition number (1-128, default 1): 1 
    First sector (2048-3907024862, default 2048): 2048
    Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-3907024862, default 3907024862): 3907024862
    
    Created a new partition 1 of type 'Linux filesystem' and of size 1.8 TiB.
   
    Confirm with Y to remove the signature
    Command (m for help): w (write to disk and exit fdisk)
```

4. Add exFAT driver

```console
sudo apt update
sudo apt install exfat-fuse
```

5. Format disk

Command: `sudo mkfs.FILETYPE DEVICE`

```console
# ExFAT example
sudo mkfs.exfat -n MyDrive /dev/sda
```

6. Check partition

```console
sudo lsblk -o UUID,NAME,FSTYPE,SIZE,MOUNTPOINT,LABEL,MODEL
```

7. Mount drive

Create a new folder in `/MyDrive`:

```console
sudo mkdir /media/MyDrive
```

Mount the drive to this place with this simple command:

```
sudo mount /dev/sda /media/MyDrive -o umask=000
```

The `umask` option allows the “pi” user to write on the device.
