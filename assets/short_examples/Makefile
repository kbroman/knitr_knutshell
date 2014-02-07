all: bunch_o_figs.html bunch_o_figs_2.html kable.html xtable.html asciitab.html

R_OPTS=--no-save --no-restore --no-init-file --no-site-file # vanilla, but with --environ

%.html: %.Rmd
	R ${R_OPTS} -e 'library(knitr);knit2html("$^")'

asciitab.html: asciitab.asciidoc
	R ${R_OPTS} -e 'library(knitr);knit("asciitab.asciidoc", "asciitab.txt")'
	asciidoc asciitab.txt
