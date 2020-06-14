ZZ ?= $(shell which zz)
CWD ?= $(shell pwd)

ZZFLAGS =

default: build

release: ZZFLAGS+=--release
release: build

build:
	$(ZZ) build $(ZZFLAGS)

clean:
	$(ZZ) clean

check:
	$(ZZ) check

test:
	$(ZZ) test $(TEST)

bench:
	$(ZZ) bench
