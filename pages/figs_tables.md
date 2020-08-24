---
layout: page
title: Figures and tables
description: Figures and tables with knitr and R Markdown.
---

Figures are really easy in knitr. Tables are not quite so easy, but
the ability to produce fully reproducible tables is really
important. You should never be copy-pasting or retyping data summaries
into a table.

### Figures

Figures are super easy in knitr. For the most part, you don't need to
do anything. If a code chunk produces a figure, it will automatically
be produced and inserted into the final document. If a code chunk
produces a bunch of figures, then a bunch of image files will be
produced and inserted into the final document.

Consider this example:

    ```{r bunch-o-figs, fig.height=4, fig.width=8}
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
side-by-side. All four will appear, one by one, in the
final document.

Here's
[what that chunk would produce](../assets/short_examples/bunch-o-figs.html),
plus an
[R Markdown file with just that chunk](../assets/short_examples/bunch-o-figs.Rmd).

For R Markdown, the default graphics device is `png`. You can choose
a different device using the chunk option `dev`. For example, to use
the `svg` device (for
[Scalable Vector Graphics](https://en.wikipedia.org/wiki/Scalable_Vector_Graphics),
which may look better in a web page as they can be scaled without loss
of quality), you would use `dev='svg'`, as follows:

    ```{r bunch-o-figs-svg, fig.height=4, fig.width=8, dev='svg'}
    n <- 100
    x <- rnorm(n)
    par(mfrow=c(1,2), las=1)
    for(i in 1:8) {
      y <- i*x + rnorm(n)
      plot(x, y, main=i)
    }
    ```

Here's
[what that chunk would produce](../assets/short_examples/bunch-o-figs-svg.html),
plus an
[R Markdown file with just that chunk](../assets/short_examples/bunch-o-figs-svg.Rmd).

You can pass arguments to the graphics device using the chunk option
`dev.args`, which takes a list.  For example:

    ```{r bunch-o-figs-pointsize, fig.height=4, fig.width=8, dev.args=list(pointsize=18)}
    n <- 100
    x <- rnorm(n)
    par(mfrow=c(1,2), las=1)
    for(i in 1:8) {
      y <- i*x + rnorm(n)
      plot(x, y, main=i)
    }
    ```

The chunk option `dev.args=list(pointsize=18)` passes `pointsize=18`
to the `png()` device. This is equivalent to, in R, using
`png("myfile.png", pointsize=18)`.

Here's
[what that chunk would produce](../assets/short_examples/bunch-o-figs-pointsize.html),
plus an
[R Markdown file with just that chunk](../assets/short_examples/bunch-o-figs-pointsize.Rmd).

For more information on graphics with knitr, see the
[Knitr graphics manual](https://yihui.name/knitr/demo/graphics/).


### Tables

I try to avoid tables; figures are almost always better. And for
informal reports, I'll often just print out a matrix or data frame,
rather than create a formal table.

#### kable

If you want to make a somewhat nicer table, the simplest approach is
to use the `kable` function in the `knitr` package. It doesn't have
many options, but in many cases it's sufficient. Here's an example.

    ```{r kable}
    n <- 100
    x <- rnorm(n)
    y <- 2*x + rnorm(n)
    out <- lm(y ~ x)
    library(knitr)
    kable(summary(out)$coef, digits=2)
    ```

Here's
[what that chunk would produce](../assets/short_examples/kable.html),
plus an
[R Markdown file with just that chunk](../assets/short_examples/kable.Rmd).

#### pander

Another good option is the
[pander package](http://rapporter.github.io/pander/).
It allows more customization, and if you give it the output of `lm()`,
it will automatically produce the table of regression coefficients
that we're interested in.

    ```{r pander}
    n <- 100
    x <- rnorm(n)
    y <- 2*x + rnorm(n)
    out <- lm(y ~ x)
    library(pander)
    panderOptions("digits", 2)
    pander(out)
    ```

Here's
[what that chunk would produce](../assets/short_examples/pander.html),
plus an
[R Markdown file with just that chunk](../assets/short_examples/pander.Rmd).


#### xtable

If you want more precise control of the appearance of the table, use
the `xtable` function in the
[xtable package](https://cran.r-project.org/package=xtable).

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

Here you need to use the chunk option `results="asis"`, as otherwise the
html code will be printed as if it were R output.

Here's
[what that chunk would produce](../assets/short_examples/xtable.html),
plus an
[R Markdown file with just that chunk](../assets/short_examples/xtable.Rmd).

### Up next

Read my [comments on reproducibility](reproducible.html), and
perhaps about [Knitr with AsciiDoc](asciidoc.html) or
[Knitr with LaTeX](latex.html).
