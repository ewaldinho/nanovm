#
# Makefile include for NanoVM core files
#

VERSION = 1.4

NVM_DIR = $(ROOT_DIR)/vm/src
# CFLAGS += -std=c99 -Wall -Werror -pedantic-errors -Wundef -Wno-long-long -I$(NVM_DIR)
CFLAGS += -Wall -Werror -Wundef -Wno-long-long -I$(NVM_DIR)
NVM_OBJS  = NanoVM.o nvmfile.o vm.o heap.o native_impl.o array.o \
	error.o loader.o native_avr.o native_stdio.o stack.o \
	native_asuro.o uart.o debug.o native_lcd.o nvmcomm1.o nvmcomm2.o
OBJS += $(NVM_OBJS)

./nvmfile.o: ./nvmdefault.h Makefile
./nvmfile.d: ./nvmdefault.h Makefile

nvmdefault.h: $(ROOT_DIR)/java/examples/$(DEFAULT_FILE).java
	javac -classpath $(ROOT_DIR)/java $(ROOT_DIR)/java/examples/$(DEFAULT_FILE).java
	java -jar $(ROOT_DIR)/tool/NanoVMTool.jar -c -f $@ $(ROOT_DIR)/tool/config/$(CONFIG) $(ROOT_DIR)/java/examples $(DEFAULT_FILE)

# convert and upload a class file
upload-%: $(ROOT_DIR)/java/examples/%.java
	javac -classpath $(ROOT_DIR)/java $(ROOT_DIR)/java/examples/$*.java
	java -jar $(ROOT_DIR)/tool/NanoVMTool.jar $(ROOT_DIR)/tool/config/$(CONFIG) $(ROOT_DIR)/java/examples $*

%.o:$(NVM_DIR)/%.c Makefile
	$(CC) $(CFLAGS) -c $< -o $@

%.d:$(NVM_DIR)/%.c
	set -e; $(CC) -MM $(CFLAGS) $< \
	| sed 's/\($*\)\.o[ :]*/\1.o $@ : /g' > $@ ; \
	[ -s $@ ] || rm -f $@
