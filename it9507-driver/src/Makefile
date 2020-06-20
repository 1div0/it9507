module=usb-it950x
EXTRA_CFLAGS = -DEXPORT_SYMTAB
CURRENT = $(shell uname -r)
KDIR = /lib/modules/$(CURRENT)/build
PWD = $(shell pwd)
MACHINE = $(shell uname -m)
KDRIVERS := /lib/modules/$(CURRENT)/kernel/drivers/media
DEST = /lib/modules/$(CURRENT)/kernel/$(MDIR)
FIRMWARE := /lib/firmware

usb-it950x-objs := it950x-core.o it950x.o

obj-m += usb-it950x.o 

default:
	make -s -C $(KDIR) SUBDIRS=$(PWD) modules
EXTRA_CFLAGS += -Wno-unused-value -Wno-unused-variable -Wno-unused-parameter \
		-Wno-switch 

ifeq "$(MACHINE)" "x86_64"
EXTRA_CFLAGS += -Wno-enum-compare
endif

install:
	@install -d $(KDRIVERS) 
	@for i in usb-it950x.ko;do  if [ -e "$$i" ]; then echo -n "$$i "; install -m 644 -c $$i $(KDRIVERS); fi; done; echo;
	@/sbin/depmod -a ${KERNELRELEASE}
	@make clean

remove: 
	echo -e "\nRemoving old $(KDRIVERS)/ files:"
	@files='usb-it950x.ko'; for i in $$files;do if [ -e $(KDRIVERS)/$$i ]; then echo -n "$$i "; rm $(KDRIVERS)/$$i; fi; done; for i in $$files;do if [ -e $(KDRIVERS)/$$i.gz ]; then echo -n "$$i.gz "; rm $(KDRIVERS)/$$i.gz; fi; done; echo;
	@modprobe -r usb_it950x 

clean:
	@-rm -f *~ *.o *.ko .*.o.cmd .*.ko.cmd *.mod.c \
	demux.h dmxdev.h dvb-pll.h dvb-usb-ids.h dvb-usb.h dvb_demux.h dvb_frontend.h \
	dvb_net.h dvb_ringbuffer.h dvbdev.h modules.order Module.markers Module.symvers

-include $(KDIR)/Rules.make
