---
layout: page
title: Knitr with LaTeX
description: How to use knitr with LaTeX
---

If you want precise control of a document,
[LaTeX](https://www.latex-project.org) is a great way to go. It's much
more complicated than
[Markdown](https://daringfireball.net/projects/markdown/), but if you're
writing a journal article, thesis, or book, you may want the extra
precision.

I'm assuming here that you already know how to write a LaTeX
document. If you don't, I'd suggesting focusing first on using knitr
with markdown, study LaTeX elsewhere (I like
[this book](https://www.amazon.com/gp/product/0321173856?ie=UTF8&tag=7210-20),
but it's maybe not for beginners),
and then coming back to this.

The first thing to note, with KnitR and LaTeX, is that the file that
mixes R code chunks and LaTeX should have the extension `.Rnw`. That
stands for "R noweb". [Noweb](https://www.cs.tufts.edu/~nr/noweb/) was
an early system (still used) for
[literate programming](https://en.wikipedia.org/wiki/Literate_programming)
(mixing code and text).

### Code chunk delimiters

Code chunks in `.Rnw` files are delimited with `<<>>=` at the top (you
put the chunk label and any chunk options in the middle of that), and
`@` following. In-line code chunks are indicated with `\Sexpr{}` (you
put the code inside the curly braces).  Here's a bit from
[my example](../assets/knitr_example.Rnw):

    We see that this is an intercross with \Sexpr{nind(sug)} individuals.
    There are \Sexpr{nphe(sug)} phenotypes, and genotype data at
    \Sexpr{totmar(sug)} markers across the \Sexpr{nchr(sug)} autosomes.  The genotype
    data is quite complete.

    Use {\tt plot()} to get a summary plot of the data.

    <<summary_plot, fig.height=8>>=
    plot(sug)
    @

Otherwise, everything about the code chunks and chunk options is the
same as with [R Markdown](Rmarkdown.html).

### Tables

To produce LaTeX tables, you can use `kable` and `xtable`
[much as you would do with R Markdown](figs_tables.html).  And the
results look considerably nicer than the html tables.  (You can't use
[pander package](https://rapporter.github.io/pander/) within LaTeX.)

Here's a `kable` example.

    <<kable>>=
    n <- 100
    x <- rnorm(n)
    y <- 2*x + rnorm(n)
    out <- lm(y ~ x)
    library(knitr)
    kable(summary(out)$coef, digits=2)
    @


And here's the `xtable` version. It's simpler than the html version,
where we assigned the output of `xtable` to an object and then printed
it with `type="html"`.

    <<xtable, results="asis">>=
    n <- 100
    x <- rnorm(n)
    y <- 2*x + rnorm(n)
    out <- lm(y ~ x)
    library(xtable)
    xtable(summary(out)$coef, digits=c(0, 2, 2, 1, 2))
    @

### Converting KnitR/LaTeX to PDF

#### RStudio

You can use [RStudio](https://www.rstudio.com) to convert a `.Rnw` file
to PDF and preview the result, in the same way you worked with R
Markdown.

But the default in RStudio is still to use
[Sweave](http://leisch.userweb.mwn.de/Sweave/), so you first need to
[change that default](https://www.rstudio.com/ide/docs/authoring/rnw_weave).
Go to the RStudio (on menu bar) &rarr; Preferences and select Sweave on the left. Then
change the selection for "Weave Rnw files using:" from Sweave to
knitr. You also have a choice of using pdfLaTeX or
[XeLaTeX](https://www.sharelatex.com/learn/XeLaTeX).

**Note**: There is another place in RStudio that you can select knitr
vs Sweave: Tools (on menu bar) &rarr; Project Options and select
Sweave on left, then again change the selection for "Weave Rnw files
using:" from Sweave to knitr. (From Tools on menu bar, you'll also see
"Global Options"; this seems to be the same as RStudio &rarr;
Preferences.) It seems like the selection within Project Options
overrides the selection within Global Options/Preferences.

Now, if you open a `.Rnw` file in RStudio, or if you create a new one
with File &rarr; New &rarr; R Sweave, there will be a button "Compile
PDF" that acts just like "Knit HTML" for R Markdown
files. (Unfortunately, though, it's not a cute button, like the yarn
one.) Click that button, and a preview window will open.

**Note**: If you opened the new `.Rnw` file while the options were still set to
Sweave (rather than knitr), the new file will have a line with
`\SweaveOpts{concordance=TRUE}`.
But if you're using knitr, you'll need to delete this line.

#### LyX

[Yihui Xie](https://yihui.name/) recommends [LyX](https://www.lyx.org/),
a graphical front-end for LaTeX that includes support for knitr. See
[Using knitr with LyX](https://yihui.name/knitr/demo/lyx/).


#### Command-line

To convert a `.Rnw` file to pdf from the command-line (or within a
[GNU make](https://www.gnu.org/software/make) file), you first use the
`knit` function in the knitr package to process the code chunks and
create a `.tex` file:

    R -e 'library(knitr);knit("knitr_example.Rnw")'

You then the usual latex, pdflatex, or xelatex command to convert the
`.tex` file to a PDF. If you need to submit sources to a journal, you
can send them that intermediate `.tex` file that's created.
