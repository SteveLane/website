---
date: 2014-11-12
title: "Asking why in data analytics/science"
comments: true
tags: ["data analytics", "data science"]
slug: "asking-why-data-analytics"
---

Working as a biostatistician/researcher, my first reaction to a
[recent article](http://www.wired.com/2014/11/enough-with-feel-good-data-science/)
on Wired was 'Well, duh!'. The author, Christopher Gooley, who we are told is
the founder of a customer insights company called
[Preact](http://www.preact.io), finishes with the following:

> So if you’re succeeding, know why. If you're failing, know why and do
> something about it.

Data science seems to be a bit of a buzz-phrase at the moment, along with the
job description 'data scientist'. IBM even
[defines](http://www-01.ibm.com/software/data/infosphere/data-scientist/) what a
data scientist is. Quoting Anjul Bhambri from that page:

>A data scientist is somebody who is inquisitive, who can stare at data and spot
>trends. It's almost like a Renaissance individual who really wants to learn and
>bring change to an organization.

So the first sentence to me is a statistician. At least, that's how I see (part
of) my role as a statistician. The second sentence in that quote though is
really where we start to see the difference: '&hellip; bring change to an
organization'. This to me, is perhaps what separates a 'statistician' from a
'data scientist' and perhaps vice versa.

The job role 'statistician' is probably misconstrued by the public. If someone
even knows what a statistician is (may be), and that's not a given, then they
often assume I count things from surveys, as in what the
[ABS](http://www.abs.gov.au) and [US Census Bureau](http://www.census.gov/) do
(among other things).

Now looking at the use of data science in Google books ngram viewer, shows that
data science is mentioned at a much greater frequency than data analytics from
the late 80s to the 2000s. So perhaps my initial characterisation of data
science being a buzz-phrase is incorrect! Have a look at data analytics
though. This is a relatively recent term, gaining popularity in the early
2000's:

<iframe name="ngram_chart" src="https://books.google.com/ngrams/interactive_chart?content=data+science%2Cdata+analytics&case_insensitive=on&year_start=1985&year_end=2014&corpus=15&smoothing=3&share=&direct_url=t4%3B%2Cdata%20science%3B%2Cc0%3B%2Cs0%3B%3BData%20Science%3B%2Cc0%3B%3Bdata%20science%3B%2Cc0%3B%3Bdata%20Science%3B%2Cc0%3B.t4%3B%2Cdata%20analytics%3B%2Cc0%3B%2Cs0%3B%3Bdata%20analytics%3B%2Cc0%3B%3BData%20Analytics%3B%2Cc0" width=800 height=500 marginwidth=0 marginheight=0 hspace=0 vspace=0 frameborder=0 scrolling=no></iframe>

Data analytics almost caught up to data science by 2006, but they seem to be
increasing in frequency at the same rate by 2008. It will be interesting to see
how that continues.

Anyway, back to my original astonishment. Perhaps there are companies who just
mash data together as Gooley suggests, but that seems an extremely naive thing
to do - surely not! Gooley later suggests that "&hellip; drive-by data science -
occasionally running large-scale data science projects to uncover correlations"
is performed a lot and shouldn't be done. But that's almost what is happening in
the experiment he's chosen later: take some random controls, your highest
frequency 'log-ins' and an algorithmically selected group, and then compare
who'll upgrade the most after a month. The result? No surprise, he says it's the
algorithmically selected group!

But how was that algorithm trained in the first place? Surely some 'drive-by
data science' was performed, and parameters estimated on a training dataset, and
then validated on a hold-out dataset?

Perhaps I'm reading too much into the piece. Maybe I should just take the more
cynical route and say that perhaps the suggestion is that you should use a
commercial product to predict who will buy your product, rather than some
'drive-by analytics'.

Anyway, asking why in data science is a good thing. It is a necessary
thing. Calling the vast majority 'feel good' data science does a disservice to
analysts/statisticians/data scientists/whatever the heck you want to call them -
it's not data science.
