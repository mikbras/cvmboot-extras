
    mount -t tmpfs mytmpfs /mnt2/tmpfs

    mount -t overlay overlay -o rw \
        -o lowerdir=/mnt2/lower \
        -o upperdir=/mnt2/tmpfs/upper \
        -o workdir=/mnt2/tmpfs/work \
        /mnt

    mount -t proc /proc $overlay_destination/proc/
    mount --rbind /sys $overlay_destination/sys/
    mount --rbind /dev $overlay_destination/dev/
    exec chroot /mnt
    //? exec chroot /mnt /sbin/init

