# q2admin.so linux makefile

# This builds in the native mode of the current OS by default.

# Note that you might need to install the 32-bit libc package
# if it isn't already installed on your platform.
# Examples:
# sudo apt-get install ia32-libs
# sudo apt-get install libc6-dev-i386
# On Ubuntu 16.x use sudo apt install libc6-dev-i386

#this nice line comes from the linux kernel makefile
ARCH := $(shell uname -m | sed -e s/i.86/i386/ -e s/sun4u/sparc64/ -e s/arm.*/arm/ -e s/sa110/arm/ -e s/alpha/axp/)

# On 64-bit OS use the command: setarch i386 make all
# to obtain the 32-bit binary DLL on 64-bit Linux.

CC = gcc -std=c99 -Wall

cc = gcc

CFLAGS =-O2 -fPIC -DARCH="$(ARCH)" -DLINUX -DSTDC_HEADERS -I/usr/include
LDFLAGS = -ldl -lm -shared

ifeq ($(ARCH),i386)
CFLAGS =-m32 -O2 -fPIC -DARCH="$(ARCH)" -DLINUX -DSTDC_HEADERS -I/usr/include
endif

# Msys2 on Windows for MinGW
ifeq ($(shell uname -o),Msys)
LDFLAGS += -lregex
endif

# flavors of Linux
ifeq ($(shell uname),Linux)
#SVNDEV := -D'SVN_REV="$(shell svnversion -n .)"'
#CFLAGS += $(SVNDEV)
CFLAGS += -DLINUX
LIBTOOL = ldd
endif

# OS X wants to be Linux and FreeBSD too.
ifeq ($(shell uname),Darwin)
#SVNDEV := -D'SVN_REV="$(shell svnversion -n .)"'
#CFLAGS += $(SVNDEV)
CFLAGS += -DLINUX
LIBTOOL = otool
endif

OUTFILES = g_main.o zb_spawn.o zb_vote.o zb_ban.o zb_cmd.o zb_flood.o \
	zb_init.o zb_log.o zb_lrcon.o zb_msgqueue.o zb_util.o zb_zbot.o \
	zb_zbotcheck.o zb_disable.o zb_checkvar.o

game$(ARCH).so: $(OUTFILES)
	$(CC) $(CFLAGS) $(OUTFILES) $(LDFLAGS) -o game$(ARCH).so
	$(LIBTOOL) -r $@

zip: game$(ARCH).so
	strip game$(ARCH).so
	zip -9 q2admin-game$(ARCH).zip game$(ARCH).so

clean:
	rm -f $(OUTFILES) game$(ARCH).so

depends:
	$(CC) $(CFLAGS) -MM *.c > dependencies

all:
	make clean
	make depends
	make

-include dependencies
