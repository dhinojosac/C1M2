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

# Platform Overrides
#PLATFORM = 

# Architectures Specific Flags
LINKER_FILE = msp432p401r.lds
CPU =cortex-m4
ARCH =thumb
SPECS = nosys.specs

# Compiler Flags and Defines
ifeq ($(PLATFORM),HOST)
	CC = gcc
else
	CC = arm-none-eabi-gcc
endif

CFLAGS = -Wall -Werror -g -O0 -std=c99
LD = arm-none.eabi-ld
LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)
CPPFLAGs = -mcpu=$(CPU) -m$(ARCH) --specs=$(SPECS) -Wall 


#Generate preprocessed output .i files, using -E flag
%.i: %.c $(DEPS)
	$(CC) $(CFLAGS) -E

#Generate assembly .asm files using -S flag
%.asm: %.c $(DEPS)
	$(CC) $(CFLAGS) -S

#Generate object .o file using -c flag
%.o: %c
	$(CC) -c $< $(CFLAGS) -o $@ -I$(INCLUDES)

#Compile-all .PHONY, but don't link (ex. make compile-all)
.PHONY: compile-all
compile-all:
	$(CC) $(CFLAGS)

#Compile all object .PHONY (ex. make build)
.PHONY: build
build:
	$(CC) $(CFLAGS)

#Clean
.PHONY: clean
clean:
	rm -rf *.o *.i *.asm
