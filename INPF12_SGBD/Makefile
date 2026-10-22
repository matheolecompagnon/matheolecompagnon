# Makefile du projet de INPF12

BIN=test

.PHONY: all run clean

all: $(BIN)

$(BIN): projet.cmo
	ocamlc -g projet.cmo -o $@

%.cmo : %.ml 
	ocamlc -g -c $<

%.cmi : %.mli
	ocamlc -g -c $<

run: $(BIN)
	./$(BIN)

clean : 
	rm -f projet.cmo projet.cmi $(BIN)
