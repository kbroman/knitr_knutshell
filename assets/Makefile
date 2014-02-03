all: knitr_example.html knitr_example_asciidoc.html

knitr_example.html: knitr_example.Rmd
	Rscript -e 'library(knitr);knit2html("knitr_example.Rmd")'

knitr_example_asciidoc.html: knitr_example.asciidoc
	Rscript -e 'library(knitr);knit("knitr_example.asciidoc", "knitr_example_asciidoc.txt")'
	asciidoc -a data-uri -a toc -a toclevels=4 -a num knitr_example_asciidoc.txt
