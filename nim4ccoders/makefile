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
	echo "        run   -- produce test binary and uin it"
	echo
	echo "        clean -- clean up"
	echo "        touch -- touch example source"

bin: example-release-strip
run: example-run

touch:
	touch $(EXAMPLE_SRC)

example-run: $(EXAMPLE_SRC)
	@rm -f $@ $(CACHEDD_DIR)/*
	nim c -r -o:$@ $(EXAMPLE_SRC)
	gcc -E -I$(NIM_HEADERS) $(CACHEDD_DIR)/@m$(EXAMPLE_SRC).c | indent -o $@.c
	@echo "*** cache:"
	@ls -l $(CACHEDD_DIR)/* | sed 's/^/    /'

example-release-strip: $(EXAMPLE_SRC)
	@rm -f $@ $(CACHEDR_DIR)/*
	nim c -d:release -o:$@ $(EXAMPLE_SRC)
	strip $@
	gcc -E -I$(NIM_HEADERS) $(CACHEDR_DIR)/@m$(EXAMPLE_SRC).c | indent -o $@.c
	@echo "*** cache:"
	@ls -l $(CACHEDR_DIR)/* | sed 's/^/    /'

clean:
	rm -f example-run
	rm -f example-run.c
	rm -f example-release-strip
	rm -f example-release-strip.c
