#! make -f

EXAMPLE_SRC = example.nim
CACHEDD_DIR = ~/.cache/nim/example_d
CACHEDR_DIR = ~/.cache/nim/example_r

NIM_HEADERS = /data/local/src/nim/lib

.SILENT: help
help:
	echo Usage: $(MAKE) target
	echo
	echo "target: bin   -- produce stripped binary"
	echo "        min   -- produce minimal stripped binary"
	echo "        run   -- produce test binary and run it"
	echo
	echo "        clean -- clean up"
	echo "        touch -- touch example source"

bin: example-release-strip
min: example-minimal-strip
run: example-run-nostrip

touch:
	touch $(EXAMPLE_SRC)

example-run-nostrip: $(EXAMPLE_SRC)
	@rm -f $@ $(CACHEDD_DIR)/*
	nim c -r -o:$@ $(EXAMPLE_SRC)
	gcc -E -I$(NIM_HEADERS) $(CACHEDD_DIR)/@m$(EXAMPLE_SRC).c | indent -o $@.c
	@echo "*** cache:"
	@ls -l $(CACHEDD_DIR)/* | sed 's/^/    /'
	@echo "*** executable:"
	@ls -l $@ | sed 's/^/    /'

example-release-strip: $(EXAMPLE_SRC)
	@rm -f $@ $(CACHEDR_DIR)/*
	nim c -d:strip -d:release -o:$@ $(EXAMPLE_SRC)
	gcc -E -I$(NIM_HEADERS) $(CACHEDR_DIR)/@m$(EXAMPLE_SRC).c | indent -o $@.c
	@echo "*** cache:"
	@ls -l $(CACHEDR_DIR)/* | sed 's/^/    /'
	@echo "*** executable:"
	@ls -l $@ | sed 's/^/    /'

example-minimal-strip: $(EXAMPLE_SRC)
	@rm -f $@ $(CACHEDR_DIR)/*
	nim c -d:strip -d:danger --opt:size -o:$@ $(EXAMPLE_SRC)
	gcc -E -I$(NIM_HEADERS) $(CACHEDR_DIR)/@m$(EXAMPLE_SRC).c | indent -o $@.c
	@echo "*** cache:"
	@ls -l $(CACHEDR_DIR)/* | sed 's/^/    /'
	@echo "*** executable:"
	@ls -l $@ | sed 's/^/    /'

clean:
	rm -f example-*-*strip example-*-*strip.c
	rm -f *~
