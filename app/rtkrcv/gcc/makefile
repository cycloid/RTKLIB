# makefile for rtkrcv

BINDIR = /usr/local/bin
SRC    = ../../../src

# for beagleboard
#CTARGET= -mfpu=neon -mfloat-abi=softfp -ffast-math

CTARGET= -DENAGLO -DENAGAL -DENAQZS -DNFREQ=3

CFLAGS = -Wall -O3 -ansi -pedantic -I$(SRC) -DTRACE $(CTARGET) -g

OS := $(shell uname -s)
ifeq (Darwin, $(findstring Darwin,$(OS)))
  LDLIBS = 
else ifeq (Linux, $(findstring Linux,$(OS)))
  CFLAGS += -Wno-unused-but-set-variable
  LDLIBS  = -lm -lrt -lpthread
endif

# General rules for finding RTKLIB sources

%.o : ../%.c $(SRC)/rtklib.h
	$(CC) -c $(CFLAGS) $<

%.o : $(SRC)/%.c $(SRC)/rtklib.h
	$(CC) -c $(CFLAGS) $<

%.o : $(SRC)/rcv/%.c $(SRC)/rtklib.h
	$(CC) -c $(CFLAGS) $<

all        : rtkrcv
rtkrcv     : rtkrcv.o rtkcmn.o rtksvr.o rtkpos.o geoid.o solution.o lambda.o
rtkrcv     : sbas.o stream.o rcvraw.o rtcm.o preceph.o options.o pntpos.o ppp.o ppp_ar.o
rtkrcv     : novatel.o ublox.o ss2.o crescent.o skytraq.o gw10.o javad.o nvs.o binex.o
rtkrcv     : rt17.o ephemeris.o rinex.o ionex.o rtcm2.o rtcm3.o rtcm3e.o qzslex.o

install:
	cp rtkrcv $(BINDIR)

test1:
	./rtkrcv -t 4 -m 52001 -t 4

test2:
	./rtkrcv -p 2105 -m 52001

test3:
	./rtkrcv -o ../rtk_pb.conf

clean:
	rm -f rtkrcv rtkrcv.exe rtkrcv.nav *.o *.out *.trace

