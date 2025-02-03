tpm2-tools: examples
====================

Installing
==========

$ sudo apt install -y tpm2-tools

Dump PCR banks
==============

```
$ sudo tpm2_pcrread
$ sudo tpm2_pcrread sha1
$ sudo tpm2_pcrread sha256
```

Extended PCR[11]
================

```
$ sudo tpm2_pcrextend 11:sha1=f1d2d2f924e986ac86fdf7b36c94bcdf32beec15 11:sha256=b5bb9d8014a0f9b1d61e21e796d78dccdf1352f23cd32812f4850b878ae4944c
```
