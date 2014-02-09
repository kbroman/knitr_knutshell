---
layout: page
title: Knitr overview
---

### Writing reports

I'll start with a bit of motivating blather. Skip to the next
section when you get bored.

Statisticians write a lot of reports. You do a bunch of analyses,
create a bunch of figures and tables, and you want to describe what
you've done to a collaborator.

When I was first starting out, I'd create a bunch of figures and
tables and email them to my collaborator with a description of the
findings in the body of the email. That was cumbersome for me and for
the collaborator. ("Which figure are we talking about, again?")

I moved towards writing formal reports in
[LaTeX](http://www.latex-project.org) and sending my collaborator a
PDF. But that was a lot of work, and if I later wanted to re-run
things (e.g., if additional data were added), it was a real hassle.

[Sweave](http://leisch.userweb.mwn.de/Sweave/) was a big help.
Your LaTeX document could contain chunks of R code, and when processed
through Sweave, the R code would be replaced by the results of the
analysis or by the figures generated. Then, if new data were added or
analyses modified, it was easy to re-compile the document and get an
updated PDF.

But getting LaTeX-based documents to look nice can be a lot of work,
particularly if you are as compulsive as I am. The biggest
problem, in my experience, is dealing with page breaks and the
positioning of figures. Sweave had a number of annoyances, too. The
biggest one, for me, was figuring out how to reference that critical
`Sweave.sty` file; I ended up just putting a local copy in every
project directory. Yeah, I can be an idiot.

[Knitr](http://yihui.name/knitr/) is Sweave
reborn. [Yihui Xie](http://yihui.name/) took the Sweave idea and
started over, removing the annoyances and making something even
better. With knitr, you can mix basically any kind of text with
basically any kind of code. This is a really big deal, as lots of
people who should be writing these sorts of
[literate programming](http://en.wikipedia.org/wiki/Literate_programming)
documents (e.g., many statistics graduate students)
are completely turned off by LaTeX and just skip the whole business.

Now, I deliver my informal reports to collaborators as
[html](http://en.wikipedia.org/wiki/HTML) documents that can be viewed
in a browser. A big advantage to this is that I don't have to worry
about page breaks. For example, I can have very tall figures, with say
30 panels. That makes it easy to show the results in detail, and you
don't have to worry about how to get figures to fit nicely into a
page.

But I'm not writing _html_ for this. I use
[Markdown](http://daringfireball.net/projects/markdown/) or
[AsciiDoc](http://www.methods.co.nz/asciidoc/). These are two systems
for writing simple, readable text, with the sort of marks that you'd
use in an email message (for example, `**bold**` for **bold** or
`_italics_` for _italics_), that can be easily converted to html. And
both Markdown and Asciidoc allow the figures to be embedded within the
html document, so you only need to email the one file to your
collaborator.

Technically, I'm not using 
[Markdown](http://daringfireball.net/projects/markdown/) but rather
[R Markdown](http://www.rstudio.com/ide/docs/r_markdown), a variant of
Markdown developed by the folks at [RStudio](http://www.rstudio.com).

Enough blather; now how does this work?

### Code chunks

The basic idea in knitr (and sweave before that, and 
[literate programming](http://en.wikipedia.org/wiki/Literate_programming)
more generally) is that your regular text document will be interrupted
by chunks of code delimited in a special way.

Here's an example with [R Markdown](http://www.rstudio.com/ide/docs/r_markdown):

    We see that this is an intercross with `r nind(sug)` individuals.
    There are `r nphe(sug)` phenotypes, and genotype data at 
    `r totmar(sug)` markers across the `r nchr(sug)` autosomes.  The genotype
    data is quite complete.

    Use `plot()` to get a summary plot of the data.

    ```{r summary_plot, fig.height=8}
    plot(sug)
    ```
    
The backticks (`` ` ``) indicate code. The bits like `` `r nind(sug)` ``
are indicating R code. When processed by knitr, they'll be evaluated
and replaced by the result. So the first paragraph would end up
as something like this:

> We see that this is an intercross with 163 individuals.
> There are 6 phenotypes, and genotype data at 
> 93 markers across the 19 autosomes.  The genotype
> data is quite complete.

In the second paragraph, `` `plot()` `` would just appear as `plot()`
(that is, rendered like code, in a monospace font).

The most useful bit is the last "paragraph". When the document is run
through knitr, `plot(sug)` will be evaluated, producing a figure,
which will then be inserted at that point in the final document.

In R Markdown, code chunks start with a line like

    ```{r chunk_name, options}

The chunk name (here, `chunk_name`) is optional; if included it needs
to be distinct from that for another chunk. Then there are a bunch of
chunk options, here `fig.height=8` indicates the height of the figure.

A code chunk ends with a line that is just three backticks:

    ```

In knitr, different types of text (e.g., R Markdown, AsciiDoc, LaTeX)
have different ways of delimiting the code chunks (as well as the
in-line bits of code). This is because
knitr is basically doing a search-and-replace for these chunks 
and depending on the type of text, different patterns will be easier
to find.

In [AsciiDoc](http://www.methods.co.nz/asciidoc/), the above would be written as follows:

    We see that this is an intercross with +r nind(sug)+ individuals.
    There are +r nphe(sug)+ phenotypes, and genotype data at 
    +r totmar(sug)+ markers across the +r nchr(sug)+ autosomes.  The genotype
    data is quite complete.

    Use +plot()+ to get a summary plot of the data.

    //begin.rcode summary_plot, fig.height=8
    plot(sug)
    //end.rcode

In [LaTeX](http://www.latex-project.org), it would be:

    We see that this is an intercross with \Sexpr{nind(sug)} individuals.
    There are \Sexpr{nphe(sug)} phenotypes, and genotype data at 
    \Sexpr{totmar(sug)} markers across the \Sexpr{nchr(sug)} autosomes.  The genotype
    data is quite complete.

    Use {\tt plot()} to get a summary plot of the data.

    <<summary_plot, fig.height=8>>=
    plot(sug)
    @

[Yihui](http://yihui.name/) would probably yell at me for the `{\tt }`
bit, but that seemed easiest for me.

A knitr document will have often have _many_ code chunks. They are
evaluated in order, in a single R session, and the state of the
various variables in one code chunk are preserved in future chunks.

The examples above are taken from longer examples that you can find
[here](https://github.com/kbroman/knitr_knutshell/tree/gh-pages/assets), including an
[R Markdown example](../assets/knitr_example.Rmd) (and
[its html product](../assets/knitr_example.html)), an
[AsciiDoc example](../assets/knitr_example.Rasciidoc) (and
[its html product](../assets/knitr_example_asciidoc.html)), and a
[LaTeX example](../assets/knitr_example.Rnw) (and
[its pdf product](../assets/knitr_example.pdf)). 
All of these examples require installation of my [R/qtl](http://www.rqtl.org)
package (sorry!). In R, type `install.packages("qtl")`.

### Compiling the document

Once you've created a knitr document (e.g., R Markdown with chunks of
R code), how do you use knitr to process it, to create the final document?

If you're creating an 
[R Markdown](http://www.rstudio.com/ide/docs/r_markdown) document
in [RStudio](http://www.rstudio.com), it's dead easy: there's a
button. And it's a particularly cute little button, with a ball of
yarn and a knitting needle.

More generally, you'd call R, load the knitr package, and use `knit()`
or one of its variants, like `knit2html()`. I prefer to create a 
[GNU make](http://www.gnu.org/software/make) file, like 
[this one](../assets/Makefile), for the examples I'd mentioned above.
(See also my [minimal make](http://kbroman.github.io/minimal_make) tutorial.)

### What next?

That's knitr in a knutshell: chunks of R code inserted within a text
document. When processed by knitr, the R code chunks are executed and
results and/or figures inserted.

Now go to my pages about [Markdown](markdown.html) and [Knitr with R Markdown](Rmarkdown.html). 

Even if you're mostly interested in [Asciidoc](asciidoc.html) or
[LaTeX](latex.html), start with the [Markdown](markdown.html) and [R Markdown page](Rmarkdown.html), as
I'll give the full details about knitr there and will only explain the extra stuff
in the other two pages. Plus, I think you'll find Knitr with R
Markdown useful, at least for short, informal reports.

If you're an experienced 
[Sweave](http://leisch.userweb.mwn.de/Sweave/) user, you might look at
my [Knitr from Sweave](sweave.html) page, or [Yihui](http://yihui.name/)'s page, 
[Transition from Sweave to knitr](http://yihui.name/knitr/demo/sweave/).
