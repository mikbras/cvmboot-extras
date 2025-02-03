SCSI=/dev/sdb

#TRACE=--trace
ETRACE=--etrace
EVENTS=--events=events
USER=--user=mbrasher:password:id_rsa.pub
HOSTNAME=--hostname=bluefish
EXPAND=--expand-root-partition
#RESOLV=--skip-resolv-conf
#VERIFY=--verify
#NO_STRIP=--no-strip

PREP_OPTS=$(TRACE) $(EVENTS) $(USER) $(HOSTNAME) $(VERIFY) $(RESOLV) $(VERIFY) $(EXTRA)

PROT_OPTS=$(TRACE) $(VERIFY) $(NO_STRIP) $(EXTRA)

INIT_OPTS=$(TRACE) $(EVENTS) $(USER) $(HOSTNAME) $(VERIFY) $(RESOLV) $(VERIFY) $(NO_STRIP) $(EXTRA)

all:

prepare:
	sudo cvmdisk prepare clean.vhd image.vhd $(PREP_OPTS) $(ETRACE)

protect:
	sudo cvmdisk protect image.vhd cvmsign $(PROT_OPTS) $(ETRACE)

init:
	sudo cvmdisk init clean.vhd image.vhd cvmsign $(INIT_OPTS) $(ETRACE)

shell:
	sudo cvmdisk shell image.vhd

shell-ro:
	sudo cvmdisk shell image.vhd --ro

losetup:
	sudo cvmdisk __losetup image.vhd

pull:
	rm -f clean.vhd
	sudo cvmdisk copy $(SCSI) clean.vhd
	sudo cvmvhd append clean.vhd

push:
	cp --sparse=auto image.vhd image
	truncate --size=-512 image
	sudo dd if=image of=$(SCSI) bs=4096 status=progress

clean:
	sudo rm -rf image.vhd image.vhd.tmp image
