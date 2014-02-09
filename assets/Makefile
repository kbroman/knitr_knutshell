# these examples rely on the R/qtl package, www.rqtl.org
# install R/qtl package, within R, by typing install.packages("qtl")

all: knitr_example.html knitr_example_asciidoc.html knitr_example.pdf markdown_example.html

R_OPTS=--no-save --no-restore --no-init-file --no-site-file # vanilla, but with --environ

knitr_example.html: knitr_example.Rmd
	R ${R_OPTS} -e 'library(knitr);knit2html("knitr_example.Rmd")'

knitr_example_asciidoc.html: knitr_example.Rasciidoc
	R ${R_OPTS} -e 'library(knitr);knit("knitr_example.Rasciidoc")'
	asciidoc -o knitr_example_asciidoc.html knitr_example.txt

knitr_example.pdf: knitr_example.Rnw
	R ${R_OPTS} -e 'library(knitr);knit("knitr_example.Rnw")'
	pdflatex knitr_example.tex

markdown_example.html: markdown_example.md
	R ${R_OPTS} -e 'library(markdown);markdownToHTML("markdown_example.md", "markdown_example.html")'

clean:
	rm knitr_example.md knitr_example_asciidoc.txt knitr_example.tex
