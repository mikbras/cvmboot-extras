```
# pvcreate /dev/sdb1
# pvdisplay
# vgcreate myvg /dev/sdb1
# vgextend myvg /dev/sdb2
# lvcreate -L 400 -n vol1 myvg
# lvcreate -L 500 -n vol2 myvg
# mkfs.ext4 -m 0 /dev/myvg/vol1
# mount /dev/myvg/vol1 /mnt
# lvcreate --thin --size 15G myvg/thinpool
# lvcreate --virtualsize 500G --thin myvg/thinpool -name thinvol
# lvchange -an myvg/thinpool
# lvcreate -L 30G --virtualsize 500G --thin cvmboot-vg/cvmboot-lv
```

```
$ sudo pvcreate /dev/sdc1
$ sudo vgcreate cvmboot-vg /dev/sdc1
$ sudo lvcreate --thin --size 30G cvmboot-vg/cvmboot-lv
$ sudo lvcreate --virtualsize 500G --thin cvmboot-vg/cvmboot-lv --name cvmboot-thin
$ sudo mkfs.ext4 /dev/cvmboot-vg/cvmboot-thin
$ sudo lvremove -f cvmboot-vg/cvmboot-lv
$ sudo vgremove -f cvmboot-vg
```

sudo lvcreate --type thin -V 500G -L 30G --thinpool cvmboot-tp

```
$ sudo pvcreate /dev/sdc1
$ sudo vgcreate cvmboot-vg /dev/sdc1
$ sudo lvcreate --type thin -V 500G -L 30G --thinpool cvmboot-tp cvmboot-vg --name cvmboot-tv
$ sudo mkfs.ext4 /dev/cvmboot-vg/cvmboot-tv
$ sudo lvchange --setautoactivation n cvmboot-vg/cvmboot-tp
$ sudo vgchange --setautoactivation n vmboot-vg
$ sudo lvchange -ay cvmboot-vg/cvmboot-tp # activate
$ sudo vgchange -ay cvmboot-vg # activate
$ sudo lvchange -an cvmboot-vg/cvmboot-tp # deactivate
$ sudo vgchange -an cvmboot-vg # deactivate
$ sudo lvremove -f cvmboot-vg/cvmboot-tp
$ sudo vgremove -f cvmboot-vg
``
