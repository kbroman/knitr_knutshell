---
layout: page
title: Knitr with asciidoc
---

[AsciiDoc](http://www.methods.co.nz/asciidoc/) is similar to
[Markdown](http://daringfireball.net/projects/markdown/): a simple,
readable text markup that can be converted to [html](http://en.wikipedia.org/wiki/HTML).

For simple reports, I'm satisfied with
[R Markdown](http://www.rstudio.com/ide/docs/r_markdown), but for a
long report with a lot of sections, it's nice to have a sidebar with a
floating table of contents. (See
[this example](../assets/knitr_example_asciidoc.html)
and its
[AsciiDoc source](../assets/knitr_example.Rasciidoc).)

An important part of the appeal of Markdown is the limited set of
simple marks. But sometimes you want just a little bit extra, like
subscripts (e.g., in describing an F<sub>1</sub> hybrid). You can
insert a bit of html code, but it's even better to use a similar
system with a more rich syntax, like
[AsciiDoc](http://www.methods.co.nz/asciidoc/).

The syntax for AsciiDoc is similar in style to that of Markdown, but a bit
different; I'm always getting confused between the two, and look at
[my example](../assets/knitr_example_asciidoc.html) and the
[AsciiDoc cheatsheet](http://powerman.name/doc/asciidoc).

### Code chunk delimiters

KnitR-wise, the main difference when you use asciidoc is that the code
chunks are delimited differently. Here's an example:

    We see that this is an intercross with +r nind(sug)+ individuals.
    There are +r nphe(sug)+ phenotypes, and genotype data at 
    +r totmar(sug)+ markers across the +r nchr(sug)+ autosomes.  The genotype
    data is quite complete.

    Use +plot()+ to get a summary plot of the data.

    //begin.rcode summary_plot, fig.height=8
    plot(sug)
    //end.rcode

The larger code chunks are delimited with `//begin.rcode` and
`//end.rcode`.

The in-line code chunks are indicated with `+r` and `+`, as AsciiDoc
uses `+` to mark code, to be shown in a monospace font.

Otherwise, everything about the code chunks and chunk options is the
same as with [R Markdown](Rmarkdown.html).

### Floating table of contents and other stuff

The top of
[my AsciiDoc example](../assets/knitr_example_asciidoc.html) is the
following:

    An example Knitr/Asciidoc document
    ==================================
    link:http://www.biostat.wisc.edu/~kbroman[Karl W Broman]
    :toc2:
    :numbered:
    :data-uri:

`:toc2:` indicates to include a table of contents, floating in the
left margin.  `:numbered:` indicates to number the sections. 
`:data-uri:` indicates to embed the images within the html file.


### Tables

You can't use `kable` or `xtable` with AsciiDoc. Instead, use the
`ascii` function in the
[ascii package](http://cran.r-project.org/web/packages/ascii/index.html),
which has a shocking number of arguments.

Here's a simple example:

    //begin.rcode table, results="asis", warning=FALSE
    x <- rnorm(100, 10, 5)
    y <- 2*x + rnorm(100, 0, 2)
    out <- lm(y ~ x)
    coef_tab <- summary(out)$coef
    library(ascii)
    ascii(coef_tab, include.rownames=TRUE, include.colnames=TRUE,
          header=TRUE)
    //end.rcode

Note the use of `results="asis"`. I used `warning=FALSE` to suppress a
warning message.

Here's
[what that chunk would produce](../assets/short_examples/asciitab.html),
plus an
[AsciiDoc file with just that chunk](../assets/short_examples/asciitab.Rasciidoc).


### Installing AsciiDoc

To use AsciiDoc, you'll need to _install_ AsciiDoc; see
[this installation page](http://www.methods.co.nz/asciidoc/INSTALL.html).

On Mac OSX, I recommend using [Homebrew](http://brew.sh/); then you
just type `brew install asciidoc`.

A key point: you'll need Python 2 and _not Python 3_.
On my computer, `python` is Python 3; I have to switch to Python 2
before running AsciiDoc. If I forget to switch, I get the following
error message:

    File "/usr/local/bin/asciidoc", line 101
      except KeyError, k: raise AttributeError, k
                     ^
    SyntaxError: invalid syntax

I'd also recommend installing the [ascii package]((http://cran.r-project.org/web/packages/ascii/index.html)) for [R](http://www.r-project.org). 


### Converting AsciiDoc to html

[RStudio](http://www.rstudio.org) has no facilities for AsciiDoc;
you'll need to use command-line tools.

You first use `knit` in the knitr package to process the asciidoc/KnitR
document:

    R -e 'library(knitr);knit("knitr_example.Rasciidoc", "knitr_example.asciidoc")'

You then use `asciidoc` to convert this to an html file:

    asciidoc knitr_example.asciidoc
