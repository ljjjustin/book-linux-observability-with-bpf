.PHONY: all

all: mon-exec

CINCLUDE = -I/usr/include/aarch64-linux-gnu

LINUXSRC=/usr/src/linux-source-5.4.0/linux-source-5.4.0

BPFLOADER = $(LINUXSRC)/samples/bpf/bpf_load.c

BPFINCLUDE += -I$(LINUXSRC)/samples/bpf
BPFINCLUDE += -I$(LINUXSRC)/tools/lib
BPFINCLUDE += -I$(LINUXSRC)/tools/perf
BPFINCLUDE += -I$(LINUXSRC)/tools/include

LDFLAGS += -lelf -lbpf

# Setting -DHAVE_ATTR_TEST=0 for the kernel containing below patch:
# 06f84d1989b7 perf tools: Make usage of test_attr__* optional for perf-sys.h
CFLAGS += $(shell grep -q "define HAVE_ATTR_TEST 1" $(LINUXSRC)/tools/perf/perf-sys.h \
	                    && echo "-DHAVE_ATTR_TEST=0")
clean:
	rm -f *.o *.so mon-exec

bpf_hello:
	clang -O2 -target bpf -c hello.c $(CINCLUDE) -o bpf_hello.o

mon-exec: bpf_hello
	clang $(CFLAGS) -o mon-exec $(BPFINCLUDE) $(LDFLAGS) $(BPFLOADER) loader.c
	chmod +x mon-exec
