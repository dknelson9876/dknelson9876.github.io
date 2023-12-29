Title: Partitioning and Formatting New Drives in Linux
Date: 2023-12-29
Tags: linux, storage
Summary: I had a new drive to install in my server, so I learned how to use `fdisk`


-------

## Step 1: Create a Partition

Before the drive can be used, you need a partition on the disk. This is done using `fdisk`. On my system, I started by using `lsblk` to find the device for my new drive. It was easy to see which drive it was because it was the only disk without any partitions, which for me happened to be `/dev/sdc`. So, we start `fdisk` with the command
```bash
sudo fdisk /dev/sdc
```
`fdisk` uses single letters for each command, and does a lot more than we need right now. We type 
```
n
```
To start making a new partition. We then type
```
p
```
To indicate that this is a primary partition. We can then choose the partition number and the size of the partition. Since this is the only partition I want on this disk, I just hit enter 3 times to get the output:

```
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1):
First sector (2048-3907029167, default 2048):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-3907029167, default 3907029167):

Created a new partition 1 of type 'Linux' and of size 1.8 TiB.
```

If you were doing something different, you could use the `t` command here to change the partition type, but for my purposes the default type of 'Linux' is fine. We finish using `fdisk` by writing our new partition data with
```
w
```

Now if I use `lsblk` again I can see a new partition `/dev/sdc1` that takes up all of `/dev/sdc`.

## Step 2: Formatting the New Partition

Before we can use the new partition, we have to create the filesystem on it, using `mkfs`. For my purposes, ext4 will do. (From what I can tell, the only other common one you might want to use is NTFS, if you want Windows to also be able to read this drive)

```
sudo mkfs.ext4 /dev/sdc1
```

## Step 3: Mounting the New Partition

The last thing we have to do is actually mount our new filesystem somewhere we can use it. A common place for this is to mount under `/mnt`, which is also what I have already used on my system. I'm going to call my new drive `/mnt/black`, after what kind of drive it is. Thus the command is
```bash
sudo mount /dev/sdc1 /mnt/black
```
However, using `mount` is not permanent and would need to be run again after rebooting. To mount it automatically on boot, we need to list it in the special file `/etc/fstab`, or the file system table. To reliably identify the partition even after hardware changes, we can use the UUID, which we can retrieve using
```
lsblk -f
```
Whose output included the line for this partition as:
```
sdc
└─sdc1
     ext4   1.0         598cb0f9-e0dc-4094-b30e-657c5112d6c7      1.7T     0% /mnt/black
```

Therefore, to the end of `/etc/fstab` we add the line:
```
UUID=598cb0f9-e0dc-4094-b30e-657c5112d6c7   /mnt/black  ext4    defaults    0 1
```