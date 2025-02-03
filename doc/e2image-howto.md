
```
# Sparse copy of EXT4 file system
sudo e2image -rap /dev/sdb2 outfile
```

```
# Discard unused space
e2fsck -E discard src_fs
```
