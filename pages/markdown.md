---
layout: page
title: Markdown
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

#### Via RStudio

If you use [RStudio](http://www.rstudio.com), the simplest way to
convert a Markdown document to html is to open the document within
RStudio. (And really, you probably want to _create_ the document in
RStudio.) When you open a Markdown document in RStudio, you'll see a
"Preview HTML" button right above the document. Press this, and
another window will open, with a preview of the result. You can then
click "Save As" or even "Publish". The latter will publish the
document to the web (where it will be viewable by _anyone_).

Another a nice feature in RStudio: when you open a Markdown document,
you'll see a little "MD" button. Press that, and you'll get see a
convenient "Markdown Quick Reference" document: a cheat-sheet on the
Markdown syntax. Like
[@StrictlyStat](https://twitter.com/StrictlyStat/status/423178160968970240),
I seem to visit the
[Markdown](http://daringfireball.net/projects/markdown) almost every
time I'm writing a Markdown document. If I used RStudio, I'd have
easier access to this information.


#### Via the command line

[Markdown](http://daringfireball.net/projects/markdown) is a
formatting syntax, but it's also a
[software tool](http://daringfireball.net/projects/downloads/Markdown_1.0.1.zip);
in particular, it's a [Perl](http://www.perl.org/) script.
So one approach to converting a Markdown document to html is to use
download and use that perl script.

But I prefer to use the
[markdown package](http://cran.r-project.org/web/packages/markdown/index.html)
for [R](http://www.r-project.org), which is what RStudio is using.

Within R, you can install the package with
`install.packages("markdown")`. Then load it with
`library(markdown)`. And then convert a Markdown document to html with

    markdownToHTML("markdown_example.md", "markdown_example.html")

In practice, I do this on the command line, as so:

    R -e 'library(markdown);markdownToHTML("markdown_example.md", "markdown_example.html")'

And _really_, I do this within a
[GNU make](http://www.gnu.org/software/make) file, like
[this one](https://github.com/kbroman/knitr_knutshell/blob/gh-pages/assets/Makefile).
(Also see my [minimal make](http://kbroman.github.io/minimal_make/)
tutorial.)
