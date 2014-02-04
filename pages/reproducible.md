---
layout: page
title: Comments on reproducibility
---

A key motivation for [knitr](http://yihui.name/knitr/) is
[reproducible research](http://en.wikipedia.org/wiki/Reproducibility#Reproducible_research):
that our results are accompanied by the data and code needed to
produce them.

With that in mind, here are a few comments on how to write portable knitr
documents: if you give the document and data to someone else, you want
them to be able to run it and get the same output.

- Avoid absolute paths

- keep all of the data and code within one
  branch of your file system directory and its subdirectory

- If you must use absolute paths and have data sets in different
  places, define the various directories with variables at the
  top of your knitr document, rather than sprinkling them throughout
  the document. This way, if someone else wants to run the document,
  they don't have to create an exact duplicate of the mess you created.

- Use `R --vanilla`. That is, avoid loading `.RData` or `.Rprofile` so
  that your code doesn't have undocumented dependencies (e.g., on
  particular options that you use all the time, or on particular
  packages that you load all the time).

- Value of GNU make (documents how to construct the final product)

- Include session info in your document, perhaps at the bottom.

- If you do any sort of simulation in the document, consider adding a
  call to `set.seed` in the first code chunk.  I ususally open R and
  type `runif(1, 0, 10^8)` and then paste the resulting large number
  into `set.seed()` in the first code chunk.  If you do this, then the
  random aspects of your analysis should be repeated the same way
  exactly whenever it is "knit."
  

I learned these things by experience. That is, I failed to do each of
these things at various times and was later frustrated by the
sloppiness of my prior self.

I'm not the only one who fails to meet the reproducible ideal.
[Reproducible Research with R and RStudio](http://www.amazon.com/exec/obidos/ASIN/1466572841/7210-20)
is quite a good book on the principles and tools for reproducible
research, but its
[github repository](https://github.com/christophergandrud/Rep-Res-Book)
has a few problems. I wanted to suggest a couple of minor corrections
and attempted to construct the book from its source, but I ran into
difficulties regarding installation of the necessary packages (it uses
_a lot_), and more importantly with the use of absolute paths (e.g.,
[here](https://github.com/christophergandrud/Rep-Res-Book/blob/master/Source/Children/FrontMatter/Preface.Rnw#L2)).

Also, if the source data and code are not readily available, then the
work isn't really reproducible.  For example, the
[supplement](http://rspb.royalsocietypublishing.org/content/281/1778/20132570/suppl/DC1)
to
[Earn et al. (2014) Proc Roy Soc B 281(1778):20132570](http://rspb.royalsocietypublishing.org/content/281/1778/20132570.abstract)
is a really beautiful document, but it says (on page 2):

> ...all details are visible in the source code (`feversupp.Rnw`), which
> is available upon request from \[author's email\].

And the author didn't respond to my email requesting the source.


**More to come.**
