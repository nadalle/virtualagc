# Copyright:	Public domain.
# Filename:	Makefile
# Purpose:	Makefile for Colossus Build 237.
#		(The source code for the Command Module's (CM)
#		Apollo Guidance Computer (AGC), for
# Contact:	Ron Burkey <info@sandroid.org>.
# Mod history:	2010-12-12 RSB	Adapted from the corresponding
#				Colossus 249 file.
#		2011-05-03 JL	Replaced with newer version from Artemis072.
#				Point to moved Oct2Bin.
#		2012-09-16 JL	Updated to match tools dir changes.
#		2016-07-17 RSB	I don't believe that Colossus237 is actually
#				correct yet, since I get diff errors when
#				building it, so the diff step errors are
#				ignored.
#		2016-08-14 RSB	All of the afore-mentioned diff errors were
#				SBANK discrepancies (i.e., a well-known 
#				problem in which yaYUL's assumptions about
#				superbank settings don't always agree 100%
#				with those of YUL or GAP, and thus have to
#				be fixed manually in the AGC source code).
#				They have now all been fixed in the source,
#				and therefore diff errors are no longer 
#				ignored.

BASENAME=Colossus237

SOURCE:=$(wildcard *.agc)
TARGETS=${BASENAME}.bin ${BASENAME}.lst 

default: $(TARGETS)

${BASENAME}.binsource: ${BASENAME}.binsource2.txt
	../Tools/listing2binsource ${VERBOSE} $^ $@

${BASENAME}.bin: ${BASENAME}.binsource
	../Tools/oct2bin <${BASENAME}.binsource
	mv oct2bin.bin $@

MAIN.agc.bin MAIN.agc.lst: ${SOURCE}
	../yaYUL/yaYUL --unpound-page --html MAIN.agc >MAIN.agc.lst

${BASENAME}.lst: MAIN.agc.bin ${BASENAME}.bin
	diff -s MAIN.agc.bin ${BASENAME}.bin
	mv MAIN.agc.lst ${BASENAME}.lst
	mv MAIN.agc.symtab ${BASENAME}.symtab

corediff.txt: ${BASENAME}.bin MAIN.agc.bin MAIN.agc.lst
	python ../Tools/ropediff.py -p -c -a -o $@ ${BASENAME}.bin MAIN.agc.bin

clean:
	rm -f *.lst *~ MAIN.agc.bin ${BASENAME}.bin *.symtab oct2bin.bin* *.html ${BASENAME}.binsource
