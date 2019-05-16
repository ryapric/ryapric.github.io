---
layout: post
title:  "\"Hey Look, an Easy Blog Framework!\": Markdown FTW"
date:   2019-05-16 11:33:12 -0500
categories: jekyll update
---

I've always thought about keeping a blog; not for any vanity reasons, or to make
money, or anything like that, but to log my thoughts & experiences as I traverse
my career. I've often picked up a new tool or technology, and been eager to
share what I've learned about it with peers. The first go-to has been to just
fire up [a new GitHub repo](https://github.com/ryapric), throw in some demo
work I'd done, and then hope someone wants to spend their time exploring it.

But, that's *me*. That's how *I'd* want to explore someone's work they'd
experimented with. Not everyone is like that. Most folks, aguably, would rather
have some human-readable, distilled format to read through.

I've had a blog page before, over on [Blogger](https://www.blogger.com), where I
wrote about analyses I had done on politico-economic topics, algorithmic tax
frameworks, etc. While that's fine and well, Blogger is maintained like a
typical end-user content-management site: you log into the browser, you use
their provided editing tools, you fuddle with the provided UI, etc. That never
felt quite right to me; something about the "fuddle with the UI" part. I
wouldn't really grow to understand *what* felt off about it until I discovered
[Markdown](https://daringfireball.net/projects/markdown/).

Markdown is a plaintext-based syntax for authoring content like `README`s, code
documentation, release notes, *whatever*. Markdown is used in contexts where the
*formatting* isn't as important as the *content*; a separate formatting engine
(like [pandoc](https://pandoc.org/)) can interpret the Markdown text, and
generate outputs like HTML, PDF, etc, but Markdown text content can stand all on
its own as a readable format (unlike, for example,
[LaTeX](https://www.latex-project.org/)). Its adoption as common syntax across
software development projects of all kinds is a testament to its cleanliness and
flexibility. It's even picked up speed as a replacement for direct `LaTeX`
typesetting (used a lot in academia), because `pandoc` can convert Markdown to
PDF, by way of generating `LaTeX` intermediately (see [the R Markdown
project](https://rmarkdown.rstudio.com/)).

So, when I finally decided to read more about [GitHub
Pages](https://pages.github.com), learned that it supports the
[Jekyll](https://jekyllrb.com) blogging framework (which support Markdown as
content), and is free for public use, I feel like I've landed on the tool I want
to use the most. I'm already in my terminal or IDE enough most of the time, so
being able to pop open a post document, write plain text as content, build,
commit & push, and have the post just... work, is something I've been hoping for
for a long time.

So the point here is: stay tuned! I plan to post my thoughts and/or criticisms
of various software languages, tools, frameworks, and libraries, as well as
data-related things (most of my professional work), books I read on supporting
philosophies; whatever. I'll post as these things come across my desk, but I
also plan to post topics retrospectively that I haven't had the chance to record
before.
