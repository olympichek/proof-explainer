#
# Simple Makefile to build PDF version.
# Uses `latexmk` (https://ctan.org/pkg/latexmk?lang=en)
#

# PDF viewer. Another alternatives are `xpdf` for Lunux
# On MacOS you can just use `open` to invoke default
# system viewer.
PDFVIEWER=evince

DOCNAME=whitepaper


SRC=    $(DOCNAME).tex \
	$(DOCNAME).bib \
        Makefile

SSRC= 	$(DOCNAME).bib \
        Makefile

# 3-rd party files I am unlikely to modify
LIBS=

all: $(DOCNAME).pdf

TECTONIC_PATH := $(shell which tectonic)
USE_LATEX ?= no

LATEX=latexmk
LATEXFLAGS=-halt-on-error -g --pdf -f -shell-escape 
LATEXCLEAN=latexmk -C

ifneq ($(TECTONIC_PATH),)
    # tectonic is present, use it unnless overriden by USE_LATEX=yes
    ifneq ($(USE_LATEX),yes)
	    LATEX=tectonic
	    LATEXFLAGS=--outfmt=pdf --chatter=minimal
	    LATEXCLEAN=rm -f *.aux *.blg *.fls  *.out *.fdb_latexmk *.log  *.toc
    endif
endif

clean:
	$(LATEXCLEAN)
	rm -f $(DOCNAME).pdf
	rm -f *.bbl *.brf
	rm -f *.nav *.snm *.vrb
	rm -f fig/*-converted-to.pdf

$(DOCNAME).pdf: $(SRC) $(LIBS)
	$(LATEX) $(LATEXFLAGS) $(DOCNAME).tex

# PS: no -f

install: $(DOCNAME).pdf 
	cp $(DOCNAME).pdf built_doc/

run: $(DOCNAME).pdf
	$(PDFVIEWER) $(DOCNAME).pdf

# Monitors TeX files and rebuilds PDF if they change.
# Requires:
# 1. https://github.com/inotify-tools/inotify-tools/wiki
# 2. https://github.com/quitesimpleorg/adhocify
watch: $(DOCNAME).pdf
	adhocify $(patsubst %,-w %,$(SRC) $(SSRC) $(LIBS)) --no-env ./rebuild.sh $(DOCNAME).pdf



