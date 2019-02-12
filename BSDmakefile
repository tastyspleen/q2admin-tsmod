# q2admin.so FreeBSD makefile

# -I/usr/i486-linuxlibc1/include

#this nice line comes from the linux kernel makefile
ARCH != uname -m | sed -e s/i.86/i386/ -e s/amd64/x86_64/ -e s/sun4u/sparc64/ -e s/arm.*/arm/ -e s/sa110/arm/ -e s/alpha/axp/

CC      = gcc
CFLAGS  = -O2 -fPIC -DARCH="$(ARCH)"
CFLAGS += -ffast-math -w -DGAME_INCLUDE -DLINUX 
CFLAGS += -DSTDC_HEADERS -I/usr/include
LDFLAGS = -shared -lm

OUTFILES = g_main.o zb_spawn.o zb_vote.o zb_ban.o zb_cmd.o \
	zb_flood.o zb_init.o zb_log.o zb_lrcon.o zb_msgqueue.o \
	zb_util.o zb_zbot.o zb_zbotcheck.o zb_disable.o \
	zb_checkvar.o

game$(ARCH).so: $(OUTFILES)
	$(CC) $(OUTFILES) $(CFLAGS) $(LDFLAGS) -o $@

zip: game$(ARCH).so
	strip game$(ARCH).so
	zip -9 q2admin-gamei386.zip game$(ARCH).so

clean:
	rm -f $(OUTFILES) game$(ARCH).so dependencies

depends:
	$(CC) $(CFLAGS) -MM *.c > dependencies

all:
	make clean
	make depends
	make

-include dependencies
