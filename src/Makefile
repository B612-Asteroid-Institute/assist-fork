ifndef REB_DIR
	
ifneq ($(wildcard ../../rebound/.*),) # Check for REBOUND in default location
REB_DIR=../../rebound
endif

ifneq ($(wildcard ../../../rebound/.*),) # Check for ASSIST being inside the REBOUND directory
REB_DIR=../..
endif

endif

ifndef REB_DIR # REBOUND is not in default location and REB_DIR is not set
    $(error ASSIST is not in the same directory as REBOUND.  To use a custom location, you must set the REB_DIR environment variable for the path to your rebound directory, e.g., export REB_DIR=/Users/dtamayo/rebound.)
endif

include $(REB_DIR)/src/Makefile.defs
OPT+= -fPIC -DLIBASSIST

ifndef ASSISTGITHASH
	ASSISTGITHASH = $(shell git rev-parse HEAD || echo '0000000000gitnotfound0000000000000000000')
	PREDEF+= -DASSISTGITHASH=$(ASSISTGITHASH)
endif

SOURCES=assist.c spk.c forces.c
OBJECTS=$(SOURCES:.c=.o)
HEADERS=$(SOURCES:.c=.h)

all: $(SOURCES) librebound.so libassist.so
	
%.o: %.c $(HEADERS)
	@echo "Compiling source file $< ..."
	$(CC) -c $(OPT) $(PREDEF) -I$(REB_DIR)/src -o $@ $<

librebound.so:
	@echo "Compiling shared library librebound.so ..."
	$(MAKE) -C $(REB_DIR)/src/
	@echo "Creating link for shared library librebound.so ..."
	@-rm -f librebound.so
	@ln -s $(REB_DIR)/src/librebound.so .

libassist.so: $(OBJECTS)
	@echo ""        
	@echo "Linking shared library $@ ..."
	$(CC) $(OPT) -shared $(OBJECTS) $(LIB) -lrebound -L$(REB_DIR)/src -o $@ 
	@echo ""        
	@echo "The shared library $@ has been created successfully."
	
clean:
	@echo "Cleaning up shared library librebound.so ..."
	@-rm -f librebound.so
	$(MAKE) -C $(REB_DIR)/src/ clean
	@echo "Cleaning up shared library libassist.so ..."
	@-rm -f libassist.so
	@-rm -f *.o
	
