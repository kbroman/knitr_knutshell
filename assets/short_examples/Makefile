all: bunch-o-figs.html bunch-o-figs-svg.html bunch-o-figs-pointsize.html kable.html xtable.html asciitab.html pander.html

R_OPTS=--no-save --no-restore --no-init-file --no-site-file # vanilla, but with --environ

%.html: %.Rmd
	R ${R_OPTS} -e 'rmarkdown::render("$^")'

asciitab.html: asciitab.Rasciidoc
	R ${R_OPTS} -e 'library(knitr);knit("asciitab.Rasciidoc")'
	asciidoc asciitab.txt
