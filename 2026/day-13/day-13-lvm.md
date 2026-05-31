# Day 13 – Linux Volume Management (LVM)

## Introduction

Today I learned one of the most important Linux storage management concepts:

> Logical Volume Management (LVM)

Traditional disk partitions are fixed in size and difficult to resize after creation. LVM solves this problem by providing flexible storage management.

I also learned how AWS EBS volumes work with Linux instances and how additional storage can be attached, mounted, and managed dynamically.

This is a critical skill for Linux administrators, cloud engineers, and DevOps professionals because production systems frequently require storage expansion without downtime.

---

# Understanding Linux Storage

Before working with LVM, I reviewed some basic storage commands.

## List Block Devices

```bash
lsblk
```

Purpose:

* Displays all block devices attached to the system.
* Shows disks, partitions, and mount points.

---

## Check Disk Usage

```bash
df -h
```

Purpose:

* Displays available and used storage.
* Shows mounted file systems.

These commands help understand the current storage layout before making any modifications.

---

# AWS Elastic Block Storage (EBS)

I learned how additional EBS volumes can be attached to an EC2 instance.

### Steps

1. Open AWS Console
2. Navigate to Elastic Block Store → Volumes
3. Create a new volume
4. Select size and availability zone
5. Attach the volume to the EC2 instance

Example device names:

```text
/dev/sdf
/dev/sdg
/dev/sdh
```

Inside the Linux instance, these may appear as:

```text
/dev/xvdf
/dev/xvdg
/dev/xvdh
```

One important observation:

> Attaching a volume only makes it visible to Linux. It does not make the storage usable until it is formatted and mounted.

---

# Understanding LVM Architecture

LVM works in three layers:

```text
Physical Volume (PV)
        ↓
Volume Group (VG)
        ↓
Logical Volume (LV)
        ↓
Filesystem
        ↓
Mount Point
```

### Physical Volume (PV)

A physical disk or block device.

Examples:

```text
/dev/xvdf
/dev/xvdg
/dev/xvdh
```

---

### Volume Group (VG)

A storage pool created by combining one or more physical volumes.

Example:

```text
tws_vg
```

---

### Logical Volume (LV)

A virtual partition created from the Volume Group.

Example:

```text
tws_lv
```

Logical Volumes can be resized later without repartitioning the disk.

---

# Creating Physical Volumes

Command:

```bash
pvcreate /dev/xvdf /dev/xvdg /dev/xvdh
```

Purpose:

* Converts block devices into Physical Volumes.

Verify:

```bash
pvs
```

---

# Creating a Volume Group

Command:

```bash
vgcreate tws_vg /dev/xvdf /dev/xvdg
```

Purpose:

* Combines physical volumes into a storage pool.

Verify:

```bash
vgs
```

---

# Creating a Logical Volume

Command:

```bash
lvcreate -L 10G -n tws_lv tws_vg
```

Purpose:

* Creates a 10 GB logical volume.

Verify:

```bash
lvs
```

---

# Display Commands

To view detailed information:

```bash
pvdisplay
```

Displays physical volume details.

```bash
vgdisplay
```

Displays volume group details.

```bash
lvdisplay
```

Displays logical volume details.

---

# Mounting Volumes in Linux

After creating a Logical Volume, it must be formatted and mounted.

### Create Mount Point

```bash
mkdir /mnt/tws_lv_mount
```

---

### Format the Volume

```bash
mkfs.ext4 /dev/tws_vg/tws_lv
```

Purpose:

* Creates an ext4 filesystem.

---

### Mount the Volume

```bash
mount /dev/tws_vg/tws_lv /mnt/tws_lv_mount
```

Now the storage becomes usable.

---

# Understanding Attach vs Mount

One concept that became very clear today:

### Attach

AWS attaches the block storage to the VM.

Example:

```text
/dev/xvdf
```

Storage is connected but not usable.

---

### Mount

Linux mounts the storage to a directory.

Example:

```text
/mnt/tws_lv_mount
```

Storage becomes accessible and usable.

---

# Unmounting a Volume

Command:

```bash
umount /mnt/tws_lv_mount
```

Purpose:

* Safely disconnects the filesystem.

---

# Managing AWS EBS Without LVM

I also learned that EBS volumes can be mounted directly without creating Volume Groups and Logical Volumes.

### Create Mount Directory

```bash
mkdir /mnt/tws_disk_mount
```

### Format Disk

```bash
mkfs -t ext4 /dev/xvdh
```

### Mount Disk

```bash
mount /dev/xvdh /mnt/tws_disk_mount
```

This directly uses the EBS volume.

However, LVM provides greater flexibility for future storage expansion.

---

# Dynamic Storage Expansion Using LVM

One of the biggest advantages of LVM is dynamic volume resizing.

Example:

Current Logical Volume:

```text
10 GB
```

Additional storage added:

```text
+5 GB
```

Extend Logical Volume:

```bash
lvextend -L +5G /dev/tws_vg/tws_lv
```

This increases the volume size without recreating partitions.

This is extremely useful in production environments where storage requirements change over time.

---

# What I Learned

### 1. LVM Provides Flexible Storage Management

Storage can be resized dynamically without repartitioning.

---

### 2. Storage Becomes Usable Only After Mounting

Attaching a disk is not enough.

The filesystem must be created and mounted.

---

### 3. LVM Is Widely Used in Production

Most enterprise Linux systems use:

* Physical Volumes
* Volume Groups
* Logical Volumes

because storage requirements constantly change.

---

# Key Commands Practiced

```bash
lsblk
df -h

pvcreate
pvs
pvdisplay

vgcreate
vgs
vgdisplay

lvcreate
lvs
lvdisplay

mkfs.ext4
mount
umount

lvextend
```

---

# Final Thoughts

Today was my first deep dive into Linux storage management.

I learned how AWS EBS volumes interact with Linux systems, how LVM organizes storage into Physical Volumes, Volume Groups, and Logical Volumes, and how storage can be expanded dynamically without downtime.

Understanding LVM is an important step toward becoming comfortable with real-world Linux administration and cloud infrastructure management.

Day 13 completed.

Still learning. Still improving. Still building.
