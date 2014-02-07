---
layout: page
title: Knitr from Sweave
---

If you have experience using [Sweave](http://leisch.userweb.mwn.de/Sweave/) with
[LaTeX](http://www.latex-project.org), you'll find that it will be an
easy transition from Sweave to [KnitR](http://yihui.name/knitr/), and
one well worth making. A number of Sweave annoyances have been
eliminated, but I think the big feature is that for less formal
reports you can use [KnitR with R Markdown](Rmarkdown.html) or
[with AsciiDoc](asciidoc.html), which are a lot easier to write than
LaTeX, and you don't have to deal with page breaks in the resulting
web page.

[Yihui Xie](http://yihui.name/) has written a great document on the
[Transition from Sweave to knitr](http://yihui.name/knitr/demo/sweave/). Here,
I'll just give a thumbnail sketch.

Chunks are still delimited with `<<>>=` and `@`, and in-line code
still uses `\Sexpr{ }`. 

Here are some of the new things in knitr:

1. A number of chunk options have been changed, and for good
reason: In knitr, all of the chunk options are valid R code. So you
use `results="hide"` rather than `results=hide`, and `eval=TRUE`
rather than `eval=true`.

2. The chunk options `width` and `height` were changed to `fig.width`
and `fig.height`, respectively. Again, see Yihui's page on
[Transition from Sweave to knitr](http://yihui.name/knitr/demo/sweave/).

3. Chunk names/labels must be distinct. In Sweave, I'd often have the
problem of copy-pasting one chunk to create another, particularly for
figures. If (or really _when_) I forgot to change the chunk label, the
second chunk's figure would overwrite the first chunk's figure, and
you'd just get that second figure in duplicate in the two places. In
knitr, if two chunks have the same label, knitr halts with an error.
This is a _good thing_!

4. You don't have to indicate that a chunk is going to
produce a figure. If the chunk produces a figure, knitr will include
the figure.

5. A chunk can produce more than one figure. Multiple image files will
be created, and they'll be inserted into the final document one after
the other.

6. You don't have to worry about that `Sweave.sty` file. The knitr
package takes care of this sort of thing.

Because of the change in chunk options, the knitr package includes a
function, `Sweave2knitr()` for converting old Sweave-based `.Rnw`
files to the knitr syntax.
