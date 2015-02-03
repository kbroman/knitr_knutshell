---
layout: page
title: Markdown
description: Introduction to Markdown
---

My prefered way to construct an informal report describing a data
analysis project is as a web page. A great advantage is that I don't
need to worry about page breaks and the placement of figures.

Web pages are written in
[html](http://en.wikipedia.org/wiki/HTML). But html is cumbersome to
write directly, and so for analysis reports, I'll generally use either
[Markdown](http://daringfireball.net/projects/markdown/) or
[AsciiDoc](http://www.methods.co.nz/asciidoc/). These are two systems
for writing simple, readable text, with the sort of marks that you'd
use in an email message (for example, `**bold**` for **bold** or
`_italics_` for _italics_), that can be easily converted to html.

Here, I'll discuss Markdown. This is a prerequisite for [what comes
next](Rmarkdown.html):
[R Markdown](http://www.rstudio.com/ide/docs/r_markdown)
with [knitr](http://yihui.name/knitr/).

### HTML

It's helpful to know a bit of
[html](http://en.wikipedia.org/wiki/HTML), which is the markup
language that web pages are written in. html really isn't that hard;
it's just cumbersome.

An html document contains pairs of tags to indicate content, like
`<h1>` and `</h1>` to indicate that the enclosed text is a "level one
header", or `<em>` and `</em>` to indicate emphasis (generally
_italics_). A web browser will
[parse](http://en.wikipedia.org/wiki/Parsing) the html tags and render
the web page, often using a
[Cascading style sheet (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets)
to define the precise style of the different elements.

But we won't get into all of that; html is great, but the code is
cumbersome to create directly, as it looks something like this:

    <!DOCTYPE html>
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
    </head>

    <body>
    <h1>Markdown example</h1>

    <p>This is a simple example of a Markdown document.</p>

    <p>Use a blank link between paragraphs.
    You can use a bit of <strong>bold</strong> or <em>italics</em>. Use backticks to indicate
    <code>code</code> that will be rendered in monospace.</p>

    <p>Here's a list:</p>

    <ul>
    <li>an item in the list</li>
    <li>another item</li>
    <li>yet another item</li>
    </ul>

    <p>You can include blocks of code using three backticks:</p>

    <p><code>
    x &lt;- rnorm(100)
    y &lt;- 2*x + rnorm(100)
    </code></p>

    <p>Or you could indent four spaces:</p>

    <pre><code>mean(x)
    sd(x)
    </code></pre>

    <p>It'll figure out numbered lists, too:</p>

    <ol>
    <li>First item</li>
    <li>Second item</li>
    </ol>

    <p>And it's easy to create links, like to
    the <a href="http://daringfireball.net/projects/markdown/">Markdown</a>
    page.</p>
    </body>
    </html>

Pretty ugly. That's probably more than you really needed to see. But
knowing about html gives you a greater appreciation of Markdown.

Note that there are six levels of headers, with tags
`<h1>`, `<h2>`, `<h3>`, ..., `<h6>`. Think of these as the title,
section, subsection, sub-subsection, &hellip;

A key design principle for creating good html documents (as well as
Markdown, AsciiDoc, and LaTeX documents), is that you want to focus on
the semantics (ie, the _meaning_ of elements) rather than the style in
which the material is to be presented. So focus on things like
"section" or "heading" rather than "large and bold".  The reason for
this, is that you're giving the web browser more information about the
material, and also you can more easily revise, externally (with
[Cascading Style Sheets (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets)),
the style in which the material is to be presented without having to
go in and revise the html code.

### Markdown

As I mentioned above,
[Markdown](http://daringfireball.net/projects/markdown/) is a system
for writing simple, readable text that is easily converted into
html. The reason it's useful to know a bit of html is that then you
have a better idea how the final product will look. (Plus, if you want
to get fancy, you can just insert a bit of html within the Markdown
document.)

A Markdown document looks like this:

    # Markdown example

    This is a simple example of a Markdown document.

    Use a blank link between paragraphs.
    You can use a bit of **bold** or _italics_. Use backticks to indicate
    `code` that will be rendered in monospace.

    Here's a list:

    - an item in the list
    - another item
    - yet another item

    You can include blocks of code using three backticks:

    ```
    x <- rnorm(100)
    y <- 2*x + rnorm(100)
    ```

    Or you could indent four spaces:

        mean(x)
        sd(x)

    It'll figure out numbered lists, too:

    1. First item
    2. Second item

    And it's easy to create links, like to
    the [Markdown](http://daringfireball.net/projects/markdown/)
    page.

That bit of Markdown text gets converted to the html code in the
previous section. (Here is the
[source file](https://github.com/kbroman/knitr_knutshell/blob/gh-pages/assets/markdown_example.md)
and the
[derived html file](https://github.com/kbroman/knitr_knutshell/blob/gh-pages/assets/markdown_example.html).)

I hope the markup is reasonably self-explanatory. Markdown is just a
system of marks that will get searched-and-replaced to create an html
document. A big advantage of the Markdown marks is that the source
document is much like what you might write in an email, and so it's
much more human-readable.

Here's a
[more extensive Markdown example](http://www.unexpected-vortices.com/sw/gouda/quick-markdown-example.html).
Also look at the [Markdown basics](http://daringfireball.net/projects/markdown/basics) page,
and the more complete [Markdown syntax](http://daringfireball.net/projects/markdown/syntax),
or just the [Markdown cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

### Converting Markdown to html

You can skip this section and move on to
[knitr with R Markdown](Rmarkdown.html), but for completeness let me
explain how to convert a Markdown document to html.

#### Via RStudio

If you use [RStudio](http://www.rstudio.com), the simplest way to
convert a Markdown document to html is to open the document within
RStudio. You'll see a
"Preview HTML" button just above the document. Click that, and another
window will open, with a preview of the result. (The resulting `.html`
file will be placed in the same directory as your `.md` file.)  You
can click "Open in browser" to open the document in your web browser,
or "Publish" to publish the document to the web (where it will be
viewable by _anyone_).

Another a nice feature in RStudio: when you open a Markdown document,
you'll see a little button with a question mark. Click that, and then
"Markdown Quick Reference," and you'll get a cheat-sheet on the
Markdown syntax. Like
[@StrictlyStat](https://twitter.com/StrictlyStat/status/423178160968970240),
I seem to visit the
[Markdown](http://daringfireball.net/projects/markdown) site almost every
time I'm writing a Markdown document. If I used RStudio, I'd have
easier access to this information.


#### Via the command line

[Markdown](http://daringfireball.net/projects/markdown) is a
formatting syntax, but it's also a
[software tool](http://daringfireball.net/projects/downloads/Markdown_1.0.1.zip);
in particular, it's a [Perl](http://www.perl.org/) script.
So one approach to converting a Markdown document to html is to
download and use that perl script.

But I prefer to use the
[markdown package](http://cran.r-project.org/web/packages/markdown/index.html)
for [R](http://www.r-project.org).

Within R, you can install the package with
`install.packages("markdown")`. Then load it with
`library(markdown)`. And then convert a Markdown document to html with

    markdownToHTML('markdown_example.md', 'markdown_example.html')

In practice, I do this on the command line, as so:

    R -e "markdown::markdownToHTML('markdown_example.md', 'markdown_example.html')"

(Note that in Windows, it's important to use double-quotes on the
outside and single-quotes inside, rather than the other way around.)

Rather than actually type that line, I include it within a
[GNU make](http://www.gnu.org/software/make) file, like
[this one](https://github.com/kbroman/knitr_knutshell/blob/gh-pages/assets/Makefile).
(Also see my [minimal make](http://kbroman.org/minimal_make/)
tutorial.)

RStudio uses the
[rmarkdown package](https://github.com/rstudio/rmarkdown) package to
convert from Markdown to html. This uses
[pandoc](http://johnmacfarlane.net/pandoc/) for the actual
conversion. The
[RStudio Desktop software](http://www.rstudio.com/products/rstudio/#Desk)
includes pandoc, so if you install RStudio, you won't need to install
pandoc separately; you just need to include it within your `PATH`. On
a Mac, you'd use:

    export PATH=$PATH:/Applications/RStudio.app/Contents/MacOS/pandoc

In Windows, you'd include `"c:\Program Files\RStudio\bin\pandoc"` in
your `Path` system environment variable. (For example, see
[this page](http://www.howtogeek.com/118594/how-to-edit-your-system-path-for-easy-command-line-access/),
though it's a bit ad-heavy.)

To convert your Markdown document to HTML, you'd then use

    R -e "rmarkdown::render('markdown_example.md')"

(I still sort of prefer the
[markdown package](http://cran.r-project.org/web/packages/markdown/index.html)
to the use of the [rmarkdown](https://github.com/rstudio/rmarkdown)
package and [pandoc](http://johnmacfarlane.net/pandoc/); the output
file is **a lot** larger with the latter. But it's best to follow the
RStudio folks on this.


### Up next

Now go to heart of this tutorial, [knitr with R Markdown](Rmarkdown.html).
