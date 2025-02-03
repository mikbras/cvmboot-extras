Using azcopy to create a sparse file with progress:
====================================================

```
azcopy copy <url> --from-to BlobPipe | pv | cvmdisk __cat erp.vhd
```
