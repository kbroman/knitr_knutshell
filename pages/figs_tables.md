---
layout: page
title: Figures and tables
---

Figures are really easy in KnitR. Tables are not quite so easy, but
the ability to produce fully reproducible tables is really
important. You should never be copy-pasting or retyping data summaries
into a table.

### Figures

Figures are super easy in KnitR. For the most part, you don't need to
do anything. If a code chunk produces a figure, it will automatically
be produced and inserted into the final document. If a code chunk
produces a bunch of figures, then a bunch of image files will be
produced and inserted into the final document.

Consider this example:

    ```{r bunch_o_figs, fig.height=4, fig.width=8}
    n <- 100
    x <- rnorm(n)
    par(mfrow=c(1,2), las=1)
    for(i in 1:8) {
      y <- i*x + rnorm(n)
      plot(x, y, main=i)
    }
    ```

The `par(mfrow=c(1,2))` makes multi-panel figures that have one row
and two columns. But the `for` loop makes 8 figures, so this will
produce four image files, each with a pair of panels,
side-by-side. All four will appear, one on top of the other, in the
final document.

For R Markdown, the default graphics device is `png`. You can choose
a differnt device using the chunk option `dev`.

You can pass arguments to the graphics device using the chunk option
`dev.args`, which takes a list.  For example:

    ```{r bunch_o_figs, fig.height=4, fig.width=8, dev.args=list(pointsize=18)}
    n <- 100
    x <- rnorm(n)
    par(mfrow=c(1,2), las=1)
    par(cex.lab=1.5, cex.axis=1.5)
    for(i in 1:8) {
      y <- i*x + rnorm(n)
      plot(x, y, main=i)
    }
    ```

The chunk option `dev.args=list(pointsize=18)` increases the size of
the points, but it seems that to increase the size of the axis labels,
you also need to use `par(cex.lab=1.5, cex.axis=1.5)`.

For more information on graphics with KnitR, see the
[Knitr graphics manual](http://yihui.name/knitr/demo/graphics/).


### Tables

I try to avoid tables; figures are almost always better. And for
informal reports, I'll often just print out a matrix or data frame,
rather than create a formal table.

#### kable

If you want to make a somewhat nicer table, the simplest approach is
to use the `kable` function in the `knitr` package. It doesn't have
many options, but in many cases it's sufficient. Here's an example.

    ```{r kable, results="asis"}
    n <- 100
    x <- rnorm(n)
    y <- 2*x + rnorm(n)
    out <- lm(y ~ x)
    kable(summary(out)$coef, format="html",
          digits=2)
    ```

You need to use the chunk option `results="asis"`, as otherwise the
html code will be printed as if it were R output.

#### xtable

If you want more precise control of the appearance of the table, use
the `xtable` function in the
[xtable package](http://cran.r-project.org/web/packages/xtable/index.html).

Here's the above example, using xtable:

    ```{r xtable, results="asis"}
    n <- 100
    x <- rnorm(n)
    y <- 2*x + rnorm(n)
    out <- lm(y ~ x)
    library(xtable)
    tab <- xtable(summary(out)$coef, digits=c(0, 2, 2, 1, 2))
    print(tab, type="html")
    ```

### Up next

Read my [comments on reproducibility](reproducible.html), and
perhaps about [Knitr with AsciiDoc](asciidoc.html) or
[Knitr with LaTeX](latex.html).
