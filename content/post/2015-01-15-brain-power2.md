---
date: 2015-01-15
title: "Brain Power: Part II"
subtitle: "Statistics"
comments: true
galleryid: brain-power
customjs:
 - /public/js/jquery-1.11.0.min.js
 - /public/js/lightbox.min.js
tags: ["scientific journalism", "neuroscience", "power", "simulation"]
---

&hellip; follows on from [previous post]({%post_url 2015-01-15-brain-power%})

We saw from the previous post that there may be some selection and
generalisability issues with the
[brain glial study](http://brain.oxfordjournals.org/content/early/2015/01/08/brain.awu377).

Let's now take a look at some statistics (which I'm much more qualified to
comment on!) First of all, let's have a look at the authors' hypothesis:

> &hellip; we hypothesized that in patients with chronic LBP glial activation
> would be detected in early pain processing regions, as they are most proximal
> to the pathological sites in the back (e.g. spinal cord/nerve roots). Thus, a
> thalamus-targeted region of interest analysis was first performed to test this
> regionally-specific hypothesis. In this analysis, a matched-pairs test (sign
> test) was performed on the mean SUVRs extracted from the voxels within the
> right and left thalamus labels of the Harvard-Oxford Subcortical Structural
> Atlas

Comments:

1. Normally in human experiments where hypotheses are being tested, you would
   expect to see some discussion on sample size/power, however this is missing
   from this article. And if indeed this study was actually a preliminary study,
   then perhaps they shouldn't be testing hypotheses.
2. The hypothesis is that subjects with chronic lower back pain (CLBP), brain
   glial activity would be detected. However, the analysis that is discussed
   (and performed) splits this into the right and left thalamus. Why is the
   split being made? Is there an expectation that the glial activation will be
   different between the sides? And if so, then nine matched pairs is certainly
   not enough to delineate this difference with any reasonable power.

Ok, with those comments aside, let's turn our attention to Figure 1A, where the
aformentioned hypothesis is being tested. I first want to comment on the
layout of the figure (which is reproduced below).

<div>
{% for gallery in site.data.brain-power %}
  {% if gallery.id == page.galleryid %}
	<ul class="rig columns-3">
    {% for image in gallery.images %}
      <li>
        <a href="{{ gallery.imagefolder }}/{{ image.name }}" data-lightbox="{{ gallery.id }}" title="{{ image.title }}">
          <img src="{{ gallery.imagefolder }}/{{ image.thumb }}">
        </a>
		{{ image.text }}
      </li>
    {% endfor %}
    </ol>
  {% endif %}
{% endfor %}
</div>

Our hypothesis is a comparison between subjects with CLBP and without, yet the
figure is grouped by CLBP in the left half, and controls in the right half (with
those lovely asterisks denoting 'significance'). The figure should really
display the CLBP and control boxplots side-by-side (irrespective of the
left/right issue). Further, why is the variability much less in the right
thalamus of controls, than it is in the left? I'd put this down to the small
sample size, rather than evidence of the hypothesis that wasn't (see above).

Now, we can get some information out of these images, so let's do that and have
a look at the power of the signed rank test that they have used to test the
hypothesis. We can use a ruler (or the distance tool in common pdf readers) to
get the various points.

The test the authors pupport using is the sign test, so I'll look at the
Wilcoxon signed rank test (which is more powerful, giving them the benefit of
the doubt). Let's look at the comparison in the left thalamus, and take as given
that the small sample sizes and boxplots provide us with good information about
the underlying distributions of (mean) thalamic SUVR. We'll also assume the that
the boxplot is 'standard', i.e. the fences are to the most extreme points
within 1.5 times the IQR. There's no outliers in the figure, so let's take it
that the limits of the fences are the minimum and maximum in each sample.

So, let's assume that the underlying distribution of SUVR is normal, so we need
estimates of the mean and standard deviation (sd) to fully specify the
distributions. Let's say that the median (line within the box in the figure) is
a good estimate for the mean, so the estimate for the mean in subjects with CLBP
is 1.06, and in controls is 0.98. Let's average two estimates for the standard
deviation: the IQR covers 50% of the distribution, so in terms of standard
normal deviations, covers $2 \times 0.6745$ sd, so an estimate of sd would be
the width of the IQR divided by ($2 \times 0.6745$). The other estimate is
similar, with such a small sample, let's say that the minimum and maximum as
identified by the fences in the figure are good estimates for the 95th
percentile. Then the sd can be approximated by the maximum minus the minimum,
divided by ($2 \times 1.96$).

The width of the IQR in subjects with CLBP is 0.08, and in controls is 0.10,
giving us estimates of sd as 0.059 and 0.074 respectively. The distance by the
maximum and minimum in subjects with CLBP is 0.18, and in controls is 0.14,
giving us estimates of sd as 0.046 and 0.036 respectively. Averaging the
approximations gives an estimate of sd for subjects with CLBP as 0.053, and for
controls as 0.055.

Now, using these assumed means and standard deviations, what would be the power
of the signed rank test to reject the null hypothesis of no difference, if the
true difference was 0.08 (1.06 minus 0.98)? We can do this by simulation:
generate nine pairs of SUVR observations with the assumed mean/sd, perform the
signed rank test, and repeat over and over. An estimate of the power (at
$\alpha=0.05$) is the proportion of times the test rejects the null
hypothesis. We'll do this in R.

First, write a simple function to simulate the data, and calculate the test:


```r
simBrain <- function(m1, m2, n){
	x <- rnorm(n = n, mean = m1, sd = 0.053)
	y <- rnorm(n = n, mean = m2, sd = 0.055)
	test <- wilcox.test(x = x, y = y, paired = TRUE, exact = TRUE)
	test$p.value < 0.05
}
```

which we can then use `replicate` to call many times, and calculate the power:


```r
set.seed(42)
power <- replicate(9999, simBrain(m1 = 1.06, m2 = 0.98, n = 9))
mean(power)
```

```
## [1] 0.7193719
```

Recalling that a *standard* of human research is 80% power, the 'apparent' power
of this specific difference of 72% is underwhelming,
especially considering the level of inaccuracy in calculate the parameters at
such small sample sizes seen here.

What if we actually thought that the mean in the CLBP subjects was lower? Given
the median is not in the middle, and is tending towards the top of the box, the
mean might actually be smaller. A mean of 1.05 would definitely not be out of
the question here, so if we use that instead?


```r
set.seed(42)
power <- replicate(9999, simBrain(m1 = 1.05, m2 = 0.98, n = 9))
```

A simple *little* change such as that, and our power has now shot down to only
60%! So at such low sample sizes, small changes in
parameters produce large changes in inference! That's why I don't think that
they warrant the description of

> &hellip; a more aggressive exploration of this therapeutic route &hellip;

as reported in the
[Scientific American](http://www.scientificamerican.com/article/chronic-pain-associated-with-activation-of-brain-s-glial-cells/) report.
