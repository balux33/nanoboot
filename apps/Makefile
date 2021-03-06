CC=mipsel-linux-gnu-gcc
CFLAGS=-Wall -Werror -std=gnu99 -g -Os -mips32 -mno-shared -mno-gpopt \
	-G0 -fpic -fomit-frame-pointer -mno-interlink-mips16 -mno-abicalls \
	-mabi=32 -mgp32 -mno-check-zero-division -I..
AS=mipsel-linux-gnu-as
ASFLAGS=--fatal-warnings -mips32 -mabi=eabi -mgp32 -mno-shared -G0
LD=mipsel-linux-gnu-ld
LDFLAGS=-static -G0
CPP=mipsel-linux-gnu-cpp
OC=mipsel-linux-gnu-objcopy
NM=mipsel-linux-gnu-nm
MKIMAGE=mkimage
TARGETS=ramcheck.ecw ramcheck.uImage mmioscan.ecw mmioscan.uImage \
	cp0regs.ecw cp0regs.uImage freqs.ecw freqs.uImage \
	reset.ecw reset.uImage poweroff.ecw poweroff.uImage

all: $(TARGETS)

clean:
	rm -f *.o *.elf *.bp *.s *~ $(TARGETS)

# apps

%.elf: %.o appstart.o
	$(LD) $(LDFLAGS) -o $@ -T app.x -nostdlib $^

%.ecw: %.elf
	$(OC) -j .text -O binary $< $@

%.uImage: %.ecw
	$(MKIMAGE) -A mips -O linux -T kernel -C none -n "$*" \
		-a 0xA00FFE00 -e 0xA0100000 -d $*.ecw $@

# ...and rules for C and assembly files

%.o: %.S
	$(CPP) -o $*.s $^
	$(AS) $(ASFLAGS) -o $@ $*.s

%.o: %.c
	$(CC) $(CFLAGS) -o $@ -c $^

.PHONY: all clean
