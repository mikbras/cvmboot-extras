
# See https://www.kernel.org/doc/html/latest/admin-guide/device-mapper/thin-provisioning.html

# Create a pool:

```
data_block_size=4096
low_water_mark=1024
dmsetup create pool \
    --table "0 20971520 thin-pool $metadata_dev $data_dev \
             $data_block_size $low_water_mark"
```

```
dmsetup message /dev/mapper/pool 0 "create_thin 0"
```

```
dmsetup create thin --table "0 2097152 thin /dev/mapper/pool 0"
```

sudo dd if=/dev/zero of=/dev/sda3 bs=4096 count=1

## Creating thin partitions:

```
#!/bin/bash
block_size=4096
low_water_mark=1024
data_dev=/dev/sda3
meta_dev=/dev/sda4
data_sectors=$(sudo blockdev --getsz "${data_dev}")
sudo dd if=/dev/zero of=${meta_dev} bs=4096 count=1
sudo dmsetup create cvmboot-thin-pool --table "0 ${data_sectors} thin-pool ${meta_dev} ${data_dev} ${block_size} ${low_water_mark}"
sudo dmsetup message /dev/mapper/cvmboot-thin-pool 0 "create_thin 0"
thin_sectors=268435456
sudo dmsetup create cvmboot-thin --table "0 ${thin_sectors} thin /dev/mapper/cvmboot-thin-pool 0"
dmsetup remove cvmboot-thin
dmsetup remove cvmboot-thin-pool
```

## Activating thin partitions:

```
#!/bin/bash
block_size=4096
low_water_mark=1024
data_dev=/dev/sda3
meta_dev=/dev/sda4
data_sectors=$(sudo blockdev --getsz "${data_dev}")
sudo dmsetup create cvmboot-thin-pool --table "0 ${data_sectors} thin-pool ${meta_dev} ${data_dev} ${block_size} ${low_water_mark}"
thin_sectors=268435456
sudo dmsetup create cvmboot-thin --table "0 ${thin_sectors} thin /dev/mapper/cvmboot-thin-pool 0"
```

## Copying from to a thin partition:

```
dd if=/dev/sda2 of=/dev/mapper/cvmboot-thin bs=4096 conv=sparse status=progress
```
