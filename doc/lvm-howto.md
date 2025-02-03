LVM How-to
==========

# Overview

This document breifly describes how to use the Logical Volume Manager (needed
for creating thinly-provisioned logical partitions. To just see the key
commands, see the "Cheat sheet" section.

The first step is to create a physical partition on your disk. Use fdisk to
create a partition of the desired size whose type is "Linux filesystem" (the
default for Linux). For the sake of discussion, suppose the physical partition
is called /dev/sd1.

The second step is to format the physical partition (/dev/sdb2) with the
pvcreate tool as follows.

```
# pvcreate /dev/sdb1
# pvdisplay
```

Next create a virtual group that will contain our volumes.

```
# vgcreate myvg /dev/sdb1
# pvdisplay
```

Optionally, add another physical partition (/dev/sdb2) to the new virtual group.

```
# vgextend myvg /dev/sdb2
# pvdisplay
```

Now add a two logical volumes to the virtual group (myvg), where the size is 400
and 500 megabytes respectively.

```
# lvcreate -L 400 -n vol1 myvg
# lvcreate -L 500 -n vol2 myvg
```

Next format one of the logical partitions as an EXT4 file system.

```
# mkfs.ext4 -m 0 /dev/myvg/vol1
```

Finally, mount the EXT4 file system.

```
# mount /dev/myvg/vol1 /mnt
```

# Thin provisioning

Create the thin-provisioning pool.

```
# lvcreate --thin -L 15G myvg/thinpool
```

Create the thin-provisioning volume.

```
# lvcreate --virtualsize 500G --thin myvg/thinpool -n thinvol
```

# Grub booting

```
# /etc/default/grub
GRUB_PRELOAD_MODULES="... lvm"
```

```
# /boot/efi/eFI/ubuntu/grub.cfg (old):
search.fs_uuid 6c36fdf3-4112-4b8e-bd13-f998881c88ca root hd0,gpt2
set prefix=($root)'/boot/grub'
configfile $prefix/grub.cfg
```

```
# /boot/efi/eFI/ubuntu/grub.cfg (new):
set root=lvm/lvm_group_name-lvm_logical_boot_partition_name
set prefix=($root)/boot/grub
configfile $prefix/grub.cfg
```

# Cheat sheet

```
# pvcreate /dev/sdb1
# pvdisplay
# vgextend myvg /dev/sdb2
# lvcreate -L 400 -n vol1 myvg
# lvcreate -L 500 -n vol2 myvg
# mkfs.ext4 -m 0 /dev/myvg/vol1
# mount /dev/myvg/vol1 /mnt
# lvcreate --thin -L 15G myvg/thinpool
# lvcreate --virtualsize 500G --thin myvg/thinpool -n thinvol
```

# Activiating an LVM

```
# vgchange -ay ubuntu-vg
# mount /dev/ubuntu-vg/ubuntu-lv /mnt
```
