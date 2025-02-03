```
sudo fio --name=test --size=8GB --numjobs=8 --rw=randwrite --bs=4k --iodepth=8 --direct=1 --runtime=60 --overwrite=1 --thread --group_reporting --ioengine=libaio --filename=test.dat

sudo fio --name=test --size=8GB --numjobs=8 --rw=randrw --bs=4k --iodepth=8 --direct=1 --runtime=60 --overwrite=1 --thread --group_reporting --ioengine=libaio --filename=test.dat
```
