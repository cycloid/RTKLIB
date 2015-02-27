# makefile for rnx2rtkp

BINDIR  = /usr/local/bin
SRC     = ../../../src

OPTS    = -DTRACE -DENAGLO -DENAQZS -DENAGAL -DNFREQ=3

MKLDIR  = /opt/intel/mkl
LINALG  = -DLAPACK   # Use: -DMKL for MKL; blank for builtin linear algebra routines
CFLAGS  = -Wall -O3 -ansi -pedantic -I$(SRC) $(OPTS) $(LINALG)

# Choose the correct LDLIBS given our OS and math library 
OS := $(shell uname -s)
ifeq (Darwin, $(findstring Darwin,$(OS)))
  ifeq (-DLAPACK, $(strip $(LINALG)))   # MacOS X with Accelerate.framework LAPACK and BLAS
    CC     = clang
    LDLIBS = -framework Accelerate
  else  # MacOS X with RTKLIB linear algebra routines
    LDLIBS =
  endif
else ifeq (Linux, $(findstring Linux,$(OS)))
  CFLAGS += -Wno-unused-but-set-variable
  ifeq (-DLAPACK, $(strip $(LINALG)))  # Linux with LAPACK
    LDLIBS = -lm -lrt -lpthread -llapack -lblas
  else ifeq (-DMKL, $(strip $(LINALG)))
    # Linux with MKL
    LDLIBS = -L$(MKLDIR)/lib/intel64 -lm -lrt -lmkl_core -lmkl_intel_lp64 -lmkl_gnu_thread -liomp5 -lpthread
  else
    LDLIBS = -lm -lrt    # Linux with RTKLIB linear algebra routines
  endif
endif

# General rules for finding RTKLIB sources

%.o : ../%.c $(SRC)/rtklib.h
	$(CC) -c $(CFLAGS) $<

%.o : $(SRC)/%.c $(SRC)/rtklib.h
	$(CC) -c $(CFLAGS) $<

%.o : $(SRC)/rcv/%.c $(SRC)/rtklib.h
	$(CC) -c $(CFLAGS) $<

all        : rnx2rtkp

rnx2rtkp   : rnx2rtkp.o rtkcmn.o rinex.o rtkpos.o postpos.o solution.o
rnx2rtkp   : lambda.o geoid.o sbas.o preceph.o pntpos.o ephemeris.o options.o
rnx2rtkp   : ppp.o ppp_ar.o rtcm.o rtcm2.o rtcm3.o rtcm3e.o ionex.o qzslex.o

CMD1    = ./rnx2rtkp
INPUT11 = ../../../test/data/rinex/07590920.05o ../../../test/data/rinex/30400920.05n
INPUT12 = ../../../test/data/rinex/30400920.05o
OPTS1   = -r -3978241.958 3382840.234 3649900.853

test : test1 test2 test3 test4 test5 test6 test7 test8 test9 test10
test : test11 test12 test13 test14 test15 test16 test17 test18 test19 test20
test : test21 test22 test23 test24

test1 :
	$(CMD1) $(INPUT11) -x 5 -o test1.pos
test2 :
	$(CMD1) -t -e $(OPTS1) $(INPUT11) > test2.pos
test3 :
	$(CMD1) -t -p 1 -e $(OPTS1) $(INPUT11) $(INPUT12) > test3.pos
test4 :
	$(CMD1) -t -p 3 -e $(OPTS1) $(INPUT11) $(INPUT12) > test4.pos
test5 :
	$(CMD1) -t -m 15 -e $(OPTS1) $(INPUT11) $(INPUT12) > test5.pos
test6 :
	$(CMD1) -t -f 1 -e $(OPTS1) $(INPUT11) $(INPUT12) > test6.pos
test7 :
	$(CMD1) -t -v 5 -e $(OPTS1) $(INPUT11) $(INPUT12) > test7.pos
test8 :
	$(CMD1) -t -i -e $(OPTS1) $(INPUT11) $(INPUT12) > test8.pos
test9 :
	$(CMD1) -t -p 0 $(OPTS1) $(INPUT11) > test9.pos
test10 :
	$(CMD1) -t -p 0 $(OPTS1) $(INPUT11) -o test10.pos
test11 :
	$(CMD1) -t -p 0 -n $(OPTS1) $(INPUT11) > test11.pos
test12 :
	$(CMD1) -t -p 0 -g $(OPTS1) $(INPUT11) > test12.pos
test13 :
	$(CMD1) -t -p 0 $(OPTS1) $(INPUT11) > test13.pos
test14 :
	$(CMD1) -t -p 0 -u $(OPTS1) $(INPUT11) > test14.pos
test15 :
	$(CMD1) -t -p 0 -d 9 $(OPTS1) $(INPUT11) > test15.pos
test16 :
	$(CMD1) -t -p 0 -s , $(OPTS1) $(INPUT11) > test16.pos
test17 :
	$(CMD1) -t -b -e $(OPTS1) $(INPUT11) $(INPUT12) > test17.pos
test18 :
	$(CMD1) -t -c -e $(OPTS1) $(INPUT11) $(INPUT12) > test18.pos
test19 :
	$(CMD1) -t -h -e $(OPTS1) $(INPUT11) $(INPUT12) > test19.pos
test20 :
	$(CMD1) -t -p 4 -a $(OPTS1) $(INPUT11) $(INPUT12) > test20.pos
test21 :
	$(CMD1) $(INPUT11) $(INPUT12) > test21.pos
test22 :
	$(CMD1) -k opts1.conf $(INPUT11) $(INPUT12) > test22.pos
test23 :
	$(CMD1) -k opts2.conf $(INPUT11) $(INPUT12) > test23.pos
test24 :
	$(CMD1) -k opts3.conf $(INPUT11) $(INPUT12) -y 2 -o test24.pos
test25 :
	$(CMD1) -k opts4.conf $(INPUT11) $(INPUT12) -y 2 -o test25.pos

clean :
	rm -f rnx2rtkp rnx2rtkp.exe *.o *.pos *.trace

prof :
	gprof rnx2rtkp > prof.txt

install :
	cp rnx2rtkp $(BINDIR)

