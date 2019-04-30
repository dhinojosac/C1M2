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
# Makefile used to generates files to C1M2 coursera assignment.
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      	<FILE>.o  - Builds <FILE>.o object file
#       <FILE>.i
#	<FILE>.asm
#	build	  - Builds and links all source files
#	compile-all       - Same as build but no linked 
#
# Platform Overrides:
#      	CPU
#	ARCH
#	SPECS
#
#------------------------------------------------------------------------------
include sources.mk

# Target
TARGET = c1m2

OBJS = $(SOURCES:.c=.o)
DEPS = $(SOURCES:.c=.d)
LINKS = $(SOURCES:.c=.i)
ASMS = $(SOURCES:.c=.asm)

# Architectures Specific Flags to MSP432
LINKER_FILE = msp432p401r.lds
CPU =cortex-m4
ARCH =thumb
SPECS = nosys.specs

# Compiler Flags and Defines
ifeq ($(PLATFORM),HOST)
	CC = gcc
	LD = ld
	LDFLAGS = -Wl,-Map=$(TARGET).map
	CFLAGS = -Wall -Werror -g -O0 -std=c99
	DEFINES = -DHOST
	PRINT_SIZE = size
else #MSP432
	CC = arm-none-eabi-gcc
	LD = arm-none-eabi-ld
	LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)
	CFLAGS = -Wall -Werror -g -O0 -std=c99 -mcpu=$(CPU) -m$(ARCH) -march=armv7e-m -mfloat-abi=hard -mfpu=fpv4-sp-d16 --specs=$(SPECS)
	DEFINES = -DMSP430
	PRINT_SIZE = arm-none-eabi-size
endif


#Generate preprocessed output .i files, using -E flag
%.i: %.c
	$(CC) -E $< $(CFLAGS) -o $@ $(INCLUDE_PATH) $(DEFINES)

#Generate assembly .asm files using -S flag
%.asm: %.c
	$(CC) -S $< $(CFLAGS) -o $@ $(INCLUDE_PATH) $(DEFINES)

#Generate object .o file using -c flag
%.o: %.c
	$(CC) -c $< $(CFLAGS) -o $@ $(INCLUDE_PATH) $(DEFINES)
#Generate dependencies .d file using -M flag
%.d: %.c
	$(CC) -M $< $(CFLAGS) -o $@ $(INCLUDE_PATH) $(DEFINES)
#Compile-all .PHONY, but don't link (ex. make compile-all)
.PHONY: compile-all
compile-all:
	$(CC) -c $(CFLAGS) $(INCLUDE_PATH) $(DEFINES) $(SOURCES)

#Compile all object .PHONY (ex. make build)
.PHONY: build
build: $(TARGET).out

$(TARGET).out: $(OBJS) $(DEPS)
	$(CC) $(OBJS) $(CFLAGS) $(LDFLAGS) -o $@ $(DEFINES) $(INCLUDE_PATH)
	$(PRINT_SIZE) $(TARGET).out

#Clean
.PHONY: clean
clean:
	rm -rf *.o *.i *.asm *.map *.out *.d

