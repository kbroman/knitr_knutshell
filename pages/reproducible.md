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

1. Avoid absolute paths. I mean, don't ever use something like
   `/Users/kbroman/Data/blah.csv` or even `~/Data/blah.csv`. When
   someone else tries to reproduce things, they won't have a
   `/Users/kbroman` directory, and they may not want to put the data
   in `~/Data`.

2. Keep all of the data and code within one branch of your file system:
   some directory and its subdirectories. By that I mean, encapsulate the
   full project into one directory, and use relative paths (like
   `Data/blah.csv`) in the code.

3. If you must use absolute paths and have data sets in different
   places, define the various directories with variables at the
   top of your knitr document (or
   [GNU make](http://www.gnu.org/software/make) file), rather than
   sprinkling them throughout
   the document. This way, if someone else wants to run the document,
   they don't have to create an exact duplicate of the mess you
   created. They can just go in and change those variables to point
   elsewhere.

4. Use `R --vanilla`. That is, avoid loading `.RData` or `~/.Rprofile` so
   that your code doesn't have undocumented dependencies (e.g., on
   particular options that you use all the time, or on particular
   packages that you load all the time). I often use functions in my
   [R/broman](http://github.com/kbroman/broman) package, and so I load
   it automatically in my `~/.Rprofile` file. But then I forget to
   include `library(broman)` at the top of knitr documents where I
   really need to.

   Actually, `R --vanilla` doesn't work for me, since I put R packages
   in `~/Rlibs/` and so need to define `R_LIBS=/Users/kbroman/Rlibs`
   in my `~/.Renviron` file. So it seems that I have to use

       R --no-save --no-restore --no-init-file --no-site-file

   (That is, `R --vanilla` but without the `--no-environ`.)

5. Use a [GNU make](http://www.gnu.org/software/make) file to define how to
   construct the final product. You might be tempted to define some
   shell shortcut for knitr, but the person trying to reproduce your
   results won't know about that. What's great about GNU make is that
   it both _automates_ your workflow and _documents_ it. See my
   [minimal make](http://kbroman.github.io/minimal_make/) tutorial,
   and also read [Mike Bostock](http://bost.ocks.org/mike/)'s
   "[Why Use Make](http://bost.ocks.org/mike/make/)".

6. Include "session info" in your document, preferably at the bottom:
   this lists the version of R that you're using plus all of the packages
   you've loaded. In R Markdown, include a code chunk like the
   following; I include the options so that we're absolutely sure that
   it will be shown.
   
       ```{r sessionInfo, include=TRUE, echo=TRUE, results='markup'}
       sessionInfo()
       ```

7. If you do any sort of simulation in the document, consider adding a
   call to `set.seed` in the first code chunk.  I usually open R and
   type `runif(1, 0, 10^8)` and then paste the resulting large number
   into `set.seed()` in the first code chunk.  If you do this, then
   the random aspects of your analysis should be repeated the same way
   exactly whenever it is "knit."

I learned these things by experience. That is, I failed to do each of
these things at various times and was later frustrated by the
sloppiness of my prior self.

I'm not the only one who fails to meet the reproducible ideal.
[Reproducible Research with R and RStudio](http://www.amazon.com/exec/obidos/ASIN/1466572841/7210-20)
is quite a good book on the principles and tools for reproducible
research, but its
[github repository](https://github.com/christophergandrud/Rep-Res-Book)
has a few problems. I attempted to construct the book from its source but ran into
difficulties regarding installation of the necessary packages (it uses
_a lot_), and more importantly with the use of absolute paths (e.g.,
[here](https://github.com/christophergandrud/Rep-Res-Book/blob/master/Source/Children/FrontMatter/Preface.Rnw#L2)).

Also, if the data and source code are not readily available, then the
work isn't really reproducible.  For example, the
[supplement](http://rspb.royalsocietypublishing.org/content/281/1778/20132570/suppl/DC1)
to
[Earn et al. (2014) Proc Roy Soc B 281(1778):20132570](http://rspb.royalsocietypublishing.org/content/281/1778/20132570.abstract)
is a really beautiful document, but it says (on page 2):

> ...all details are visible in the source code (`feversupp.Rnw`), which
> is available upon request from \[author's email\].

It'd be better to have it on [GitHub](http://github.com),
[Figshare](http://figshare.com), or even a personal web page. 
