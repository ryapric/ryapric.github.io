---
layout: post
title:  "Moving From 'Data Science' to Software Development"
date:   2019-05-25 23:58:00 -0500
categories: jekyll update
---

***! WARNING: disorganized wall of text braindump incoming !***

I love [R](https://www.r-project.org/). If you don't know, R is a language and
software for statistical computation & graphics. Coming hot out of graduate
school with degrees in economics & finance, the FOSS language-du-jour for
working with relational data was (and still is, to an extent) R, though the
scope was usually fixed on using it for strictly statistical or 'data science'
work. And for that, R is *great*. Instead of fudging around in MS Excel to line
up your columns by hand, clicking through the Data Analysis Toolpak, and
generating a separate boxy output of your statistical results, you can just type
a few lines of R code to get the same results:

```r
# Read your data in
df_0 <- read.csv("some_data.csv")
# Fit a linear model
fit <- lm(Sales ~ Region, data = df_0)
# Show results
summary(fit)
```

ezpz.

I used R for tasks like this for a few years at most, at my first job out of
school at a start-up in St. Louis. But in time, I would come to realize that
'sweet machine learning' stuff is *nowhere near as valuable* as having quality,
reliable, automated data pipelines that actually *feed* those processes (if
those processes don't exist). The requests from our clients to instead provide
complex ETL pipelines got louder and louder. So, I pivoted to doing more work in
a space often called 'data engineering'.

***And I loved it***. Being able to script out super complex transformation
logic according to business rules is gratifying, exciting, and of incredibly
high business value. And since *we* would do it, and not our clients, they
basically *threw* money at us to take on the task.

But, I was so used to using R, which is *statistical* software. What was I
supposed to do, drop everything I'd painstakingly learned about the (often
cumbersome) language and pick up something else? "No way!", I screamed
internally. "I'm too stubborn to learn a *new* tool! Surely R can work fine for
this, too?"

As it turns out, my stubborn self was rewarded immediately, because R is
thankfully *incredibly powerful* as a tabular-data processing software; it's
*designed* to work with tables! But, R *can be* pretty slow, all things
considered, when using the out-of-the-box libraries. And I'd often try to do
something, and my latop's RAM would fill up & brick my machine.

Around here is where I discovered the [tidyverse](https://www.tidyverse.org/) of
R packages. The bread and butter package from the tidyverse is called
[dplyr](https://dplyr.tidyverse.org/), which provides a suite of functionality
that makes data processing not only fast, but in a syntax that is both
elegant and expressive.

Take, for example, the act of subsetting a data frame (R's table structure). In
base R, it would look something like this:

```r
df_2 <- df_1[df_1$Region == "North America", ]
```

Using `dplyr`, the same operation might look like this:

```r
df_2 <- filter(df_1, Region == "North America")
```

Ok, whatever. "They look more or less the same, I guess". But base R's
subsetting syntax can be much more alien to folks coming from another tool, like
a SQL. What do those brackets even do? What's the dollar sign for? Why is there
a trailing comma and a space at the end of the bracket enclosure? With `dplyr`'s
`filter()`, the plain English that it's written in makes the intent of the code
much clearer, whether you're new to R, or new to programming at all.

But this is trivial. What if you wanted to subset the data to North America,
only grab three columns, and aggregate by two of them? Base R might look like
this:

```r
df_2 <- df_1[df_1$Region == "North America", c("Sales_District", "Client", "Sales")]
df_3 <- aggregate(df_2$Sales, by = list(Sales_District = df_2$Sales_District, Client = df_2$Client), FUN = sum)
```

To some folks: ew.

Meanwhile, `dplyr` might look like this:

```r
df_2 <- df_1 %>%
    filter(Region == "North America") %>%
    group_by(Sales_District, Client) %>%
    summarize(Sales = sum(Sales))
```

The *pipe* operator '`%>%`' comes from the `magrittr` package in R, which is
loaded along with `dplyr` and is read as: "take the output of the last thing,
and use it as the first input in the next thing" (much like a Unix shell pipe).
So, this whole block reads *in plain English* as: "make `df_2` by taking `df_1`,
filtering it to North America, group it by Sales District and Client, and give
me the sum of the Sales for each group." The results are the same as with base
R, but it *reads* much easier.

Now, I could go on and on about `dplyr` and the rest of the `tidyverse`, but
I'll spare you; there's plenty of material you can Google on all that. This is a
story about how tools like *these*, pushed me towards what I do today.

Now, our small team had discovered the `tidyverse`, and we *ran with it*. All of
our scripts to read, transform, export, and analyze our client data were fueled
by coffee and `dplyr`. Our `dplyr` pipelines (like you saw above) got longer,
and longer, until a single pipe block might span 100 lines itself! *Look at us,
we're wizards!*

But it was not to last. There wasn't (and might still not be) support for
introspection *within* a code block like that in R, so trying to debug any
errors (or worse, bad code that *didn't* throw errors) was incredibly tedious.
Worse, things might not throw errors at all, but would spit out bad data that we
wouldn't catch until it was live in a client dashboard. We'd gone *so hard* in
writing scripts under these paradigms that we didn't know how to handle the
consequences of our poor design. We were just being "script kiddies"; pasting in
bits of Stack Overflow code that mysteriously worked, and other bad practices
that made our work harder & harder to understand, least of all to ourselves.

That kind of workflow isn't sustainable. Most developers understand this very
early in their careers. But we didn't consider ourselves "developers"; we were
"data analysts", after all. Why should software design best-practices matter to
us?

As the `tidyverse` grew in popularity as a framework for R, the core team at
RStudio themselves (who develop it) began to create resources around software
development best-practices. Jenny Bryan (@jennybc), Hadley Wickham (@hadley),
Garrett Grolemund (@statgarrett), Jim Hester (@jimhester), and others describe
in detail the pitfalls of haphazardly applying R frameworks to otherwise good
data software. We eventually stumbled across these readings (there are too many
to link, but you can find the authors on Twitter & GitHub via those handles),
but didn't take immediated heed of their warnings. They'd given us the greatest
set of tools we'd ever seen, and now we're supposed to be *careful*? Surely
there must be something wrong with our *scripts*, not our *approach*.

Then we hired Thom.

Thom used to work at Google. He'd been buried in software design of one of the
largest and most successful software companies in the world. Thom swooned us
during his interviews, submitted his code sample in an R Markdown document
(instead of the Word doc we sent), and his words dripped gold. He was made an
offer without any hesitation.

Thom took one look at our codebase, and almost quit! "Why isn't any of this in
*packages*? How do you run tests, validations, send to production?"

"...", we replied.

Thom stuck around, though, and it was, without exaggeration, the turning point
in my career and FOSS-development life. He introduced four new concepts to me
that I came to love dearly:

- R packages (which would extend to code packaging in general). Discovering not
  just that I *could* do something like package up my R code and functions, but
  that Hadley Wickham had written the excellent [R
  Packages](http://r-pkgs.had.co.nz/) book was my first real introduction to
  modularity, reproducibility, and good software design. I always thought
  packages were just things I `install`ed, too arcane for lowly lifeforms like
  myself to ever dabble in. I had no interest in publishing a package to CRAN
  (the main R package repo), so why bother? But packaging up your code in *any*
  language allows for all the same benefits: everything is composable from
  *functions* (or classes/methods) you write (*not* scripts), it's inherently
  *testable* by the software's build system, you can automate much more of your
  code's behavior, and more.

- Unit & functional tests. Hadley Wickham & team have written a user-friendly R
  package called [testthat](https://testthat.r-lib.org/) for testing
  expectations in R. We'd never tested *anything* in this way; we'd push data
  from our scripts, and check things ad-hoc in a Tableau dashboard that was
  eventually published for our clients. This was not only time-consuming, but
  incredibly error-prone. Once we started to test our *code*, we also started to
  test our *business logic* in the form of logged data validations. I loved this
  idea so much that I even wrote a CRAN package for just this, called
  [loggit](https://github.com/ryapric/loggit), if you want to check that out.

- Documentation. Sure, we threw in some comments here & there when we thought we
  needed them, but I'm talking about *real* documentation: docblocks, READMEs
  via markdown, the whole gamut. R has an incredibly robust way to document its
  packages (it's also very naggy), and it's all made incredibly easy thanks to
  the package [roxygen2](https://github.com/yihui/roxygen2). Good documentation
  was one of the most refreshing things I took away from all of this change.

- Automation. We said to Thom: "well, we use Windows Task Scheduler to run our
  scripts, sure". He groaned. That's not what he meant. He meant everything I've
  outlined above. If some part of your workflow can't be automated, then you
  should take a long, hard look at whether or not it should be part of your
  workflow at all.

I had the great pleasure of adopting these tenets into my work for about 6
months at that job, before I was approached by Slalom Consulting about a job
opportunity. I'd become so taken by this newfound approach to my work & personal
projects that I'd slammed out some GitHub repos and a ton of updates to my
LinkedIn. I spent about two months interviewing with various people at Slalom,
and the common question I posed in all those conversations was something like:

"I'm on a bit of a learning journey right now in my career. Will this job
allow me to pursue all the things I don't know about software? Will the people
here help teach me these things? What does the company do to support people like
me, who have discovered the drive to never stop learning?"

I got a job offer at the end of that two month period, and it's been everything
I could have dreamed of. I work with ***the smartest*** people, of all walks of
life & fields of expertise, and they're all just a Slack message away, always
willing to help point you in the right direction, sit with you and explain
something, or just ramble about what they think is cool. Thom gave me the fire;
Slalom gave me the pine pitch to burn it hottest. I've learned *so many things*,
across so many different software fields. I've learned about virtualization,
Docker, bash, Unix tools, Python, Python's Flask framework, JavaScript, nodejs,
cloud providers, RBDMSes & what makes them different from each other,
networking. And on and on and on.

And it will *never stop*. At this job or any other, I've become ***gripped*** by
this unending hunger. I fall asleep reading documentation for GNU `awk`. I
re-read the bash manual in transit. I read blog posts of opinions on web
development bloat while on the toilet. I creep on Slack threads about the
relative merits of Microsoft buying GitHub. And lately, I've gotten to *share*
all this knowledge with others who are willing to listen, by way of Meetup
groups, conferences, and workshops.

I've become a lifelong student of the craft of software, and I couldn't be
happier.
