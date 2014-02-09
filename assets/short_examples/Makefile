all: bunch_o_figs.html bunch_o_figs_svg.html bunch_o_figs_pointsize.html kable.html xtable.html asciitab.html

R_OPTS=--no-save --no-restore --no-init-file --no-site-file # vanilla, but with --environ

%.html: %.Rmd
	R ${R_OPTS} -e 'library(knitr);knit2html("$^")'

asciitab.html: asciitab.Rasciidoc
	R ${R_OPTS} -e 'library(knitr);knit("asciitab.Rasciidoc")'
	asciidoc asciitab.txt
