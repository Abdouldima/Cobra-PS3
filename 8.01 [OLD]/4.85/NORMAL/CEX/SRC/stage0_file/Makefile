CC				:= ppu-gcc
CXX				:= ppu-g++
LD				:= ppu-ld 
OBJCOPY			:= ppu-objcopy
AR 				:= ppu-ar
STRIP			:= ppu-strip
LV2GEN 			= ../lv2gen/lv2gen
LV2_KERNEL		= ../lv2gen/lv2_kernel.self
KERNEL_TARGET	= lv2_kernel.self
STAGE0 			= stage0.bin
STAGE0_ADDR_CEX	= 0x26C8DC
INCLUDE			=  ../lv2/include ../lv1/include ../debug/include

INCLUDE			+= $(PS3DEV)/ppu/ppu/include
LIBSDIR			=
LIBS 			=
TARGET			= stage0


CFLAGS = -ffunction-sections -fdata-sections -Os -m64 -fno-builtin -fno-exceptions \
			   -Os -Wall $(foreach dir,$(INCLUDE),-I$(dir)) \
			   -DLV2 -DFIRMWARE_4_85 -ffreestanding 
CFLAGS += --std=gnu99
LDFLAGS=-nostartfiles -nostdlib -mbig-endian
LDFLAGS= -T $(TARGET).ld -nostartfiles -nostdlib -nodefaultlibs -Wl,-static -Wl,-s -L. $(foreach dir,$(LIBSDIR),-L$(dir)) $(LIBS) -Wl,--gc-sections -Wl

OBJS = start.o main.o ../lv2/src/libc.o ../lv2/src/memory.o  ../lv2/src/io.o ../lv2/src/thread.o ../lv1/src/hvcall.o

all: $(TARGET).bin

%.bin: %.elf
	$(OBJCOPY) -O binary $< $@	
	$(LV2GEN) $(LV2_KERNEL) $(STAGE0) $(STAGE0_ADDR_CEX) $(KERNEL_TARGET)
	@rm -f *.o *.elf ../lv1/src/*.o
	@cp $(KERNEL_TARGET) ../../BIN

$(TARGET).elf: $(OBJS) $(TARGET).ld
	$(CC) $(LDFLAGS) -o $@ $(OBJS)

%.o: %.S
	$(CC) $(CFLAGS) -c -o $@ $<

%.o: %.c
	$(CC) $(CFLAGS) -c -o $@ $<

clean:
	rm -f *.o *.elf *.self *.bin *.484c *.map ../lv1/src/*.o ../debug/src/*.o ../lv2/src/*.o
