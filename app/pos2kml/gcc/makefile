# makefile for pos2kml

BINDIR = /usr/local/bin
SRC    = ../../../src
CFLAGS = -Wall -O3 -ansi -pedantic -I$(SRC) -DTRACE

# Set OS-specific compile and link options

OS := $(shell uname -s)
ifeq (Darwin, $(findstring Darwin,$(OS)))
  CC = clang
  LDLIBS = 
else ifeq (Linux, $(findstring Linux,$(OS)))
  CFLAGS += -Wno-unused-but-set-variable
  LDLIBS  = -lm -lrt
endif

%.o : ../%.c $(SRC)/rtklib.h
	$(CC) -c $(CFLAGS) $<

%.o : $(SRC)/%.c $(SRC)/rtklib.h
	$(CC) -c $(CFLAGS) $<

pos2kml    : pos2kml.o convkml.o solution.o geoid.o rtkcmn.o preceph.o

install:
	cp pos2kml $(BINDIR)

clean:
	rm -f pos2kml pos2kml.exe *.o
