---
layout: post
title:  "Moving From 'Data Science' to Software Development"
date:   2019-05-16 15:17:00 -0500
categories: jekyll update
---

I love [R](https://www.r-project.org/).

If you don't know, R is a language and software for statistical computation &
graphics. Coming hot out of graduate school with degrees in economics & finance,
the FOSS language-du-jour for working with relational data was (and still is, to
an extent) R, though the scope was usually fixed on using it for strictly
statistical or 'data science' work. And for that, R is *great*. Instead of
fudging around in MS Excel to line up your columns by hand, clicking through the
Data Analysis Toolpak, and generating a separate boxy output of your statistical
results, you can just type a few lines of R code to get the same results:

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
only grab three columns, and aggregate by one of them? Base R might look like
this:

```r
df_2 <- df_1[df_1$Region == "North America", c("Sales_District", "Client", "Sales")]
df_3 <- aggregate(df_2$Sales, by = list(Sales_District = df_2$Sales_District, Client = df_2$Client), FUN = sum)
```

Ew.

Meanwhile, `dplyr` might look like this:

```r
df_2 <- df_1 %>%
    filter(Region == "North America") %>%
    group_by(Sales_District, Client) %>%
    summarize(Sales = sum(Sales))
```

The *pipe* operator '`%>%`' comes from the `magrittr` package in R, which is
loaded along with `dplyr` and is read as: "take the output of the last thing,
and use it as the first input in the next thing" (much like a shell pipe). So,
this whole block reads *in plain English* as: "make `df_2` by taking `df_1`,
filtering it to North America, group it by Sales District and Client, and give
me the sum of the Sales for each group." The results are the same as with base
R, but it *reads* much easier.

Now, I could go on and on about `dplyr` and the rest of the `tidyverse`, but
I'll spare you; there's plenty of material you can Google on all that. This is a
story about how tools like *these*, pushed me towards what I do today.

Now, our small team had discovered the `tidyverse`, and we *ran with it*. All of
our scripts to read, transform, export, and analyze our client data was fueled
by coffee and `dplyr`. Our `dplyr` pipelines (like you saw above) got longer,
and longer, until a single pipe block might span 100 lines itself!

- 'script kiddies'

- unit tests

- data validations

- automate automate automate
