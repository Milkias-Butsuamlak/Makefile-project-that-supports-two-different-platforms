#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <a makefile that is designed to operate on a host platform or the msp432 platform>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <Put a description of the supported targets here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
include sources.mk

# Platform Overrides
PLATFORM ?= 

# Architectures Specific Flags
LINKER_FILE =/home/ecee/week_2_assignment/msp432p401r.lds 
CPU = cortex-m4
ARCV=armv7e-m
ARCH = thumb
FLOAT= hard
FPU=fpv4-sp-d16
SPECS = nosys.specs
#Compiler flags and Defines

CC = 
LD = 
LDFLAGS = -Wl,-Map=$(TARGET).map
CFLAGS = -Wall -Werror -g -O0 -std=c99
CPPFLAGS =
TARGET= c1m2

ifeq ($(PLATFORM),HOST)
	CC=gcc
	CFLAGS+= -DHOST

else ifeq ($(PLATFORM),MSP432)
	CC=arm-none-eabi-gcc
	CFLAGS+= -mcpu=$(CPU) -m$(ARCH) -march=$(ARCV) --specs=$(SPECS)\
		 -mfloat-abi=$(FLOAT) -mfpu=$(FPU) -DMSP432
	LDFLAGS+= -T $(LINKER_FILE)
endif

OBJS= $(SOURCES:.c=.o)


%.o : %.c
	$(CC) -c $< $(CFLAGS) $(INCLUDES) -o $@

%.i: %.c
	gcc -E $< $(INCLUDES) -o $@

%.asm: %.c
	$(CC) -S $< $(INCLUDES) -o $@

%.asm.final:$(TARGET).out
	objdump -d $< > $@


.PHONY: build
build: all


.PHONY: all
all: $(TARGET).out

$(TARGET).out: $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) $(LDFLAGS) -o $@

.PHONY: clean
clean:
	rm -f $(OBJS) $(TARGET).out $(TARGET).map *.o *.i *.asm *.asm.final
