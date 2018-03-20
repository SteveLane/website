---
layout: post
title: "Astrology matters!"
subtitle: "Hmmm&hellip;"
comments: true
tags: ["correlation", "data mining"]
slug: "birth-disease-assn"
---

The standard line from scientists is *correlation does not equal causation*, but
that seems to have been forgotten by the authors of this paper: ['Birth Month
Affects Lifetime Disease Risk: A Phenome-Wide Method'](http://jamia.oxfordjournals.org/content/early/2015/06/01/jamia.ocv046.full).

I came across this article in
[The Age (paywalled)](http://www.theage.com.au/technology/sci-tech/astronomy/scientists-have-discovered-how-the-month-youre-born-matters-for-your-health-20150615-ghosn8.html),
but it has also been picked up by that respected scientific outlet
[Cosmopolitan](http://www.cosmopolitan.com/health-fitness/news/a41657/what-your-birthday-says-about-your-health/).

Apparently data mining a large electronic database and using logistic regression
on 1700 diagnosis codes (SNOMED-CT) combined with a Benjamini-Hochberg adjustment
for multiple comparisons is a new procedure that deserves a new name SeaWAS:
Season-Wide Association Study. This is a bit of a stretch, but let's run with
it.

So what does the procedure amount to? Individuals with the SNOMED-CT code were
extracted, along with 10 individuals without the SNOMED-CT code (but still from
the same database), and it appears that a logistic regression was then
fit, with birth month (as an integer) as the predictor variable. SNOMED-CT
conditions that had a p-value less than the Benjamini-Hochberg cutoff were
deemed *significant*.

Unfortunately there's not much more detail than that, but we can guess that this
procedure is likely to lead to spurious results (as I think it does, more
later): it would seem that the p-value referenced is likely the Wald p-value for
the *linear* birth month term&mdash;but so what? What makes us think that birth
month (on the logistic scale) should be linearly related to the SNOMED-CT
condition?

Anyway, that's what was used, and they found 55 (55!) *conditions dependent on
birht month*. And within these 55, 16 *completely novel* conditions were
found. *Completely novel* meaning that no-one else had apparently published an
association between birth month and these conditions. And when you have a look
at these *completely novel* conditions it's no wonder! Have a look at their
Table 2, and you'll find that the following SNOMED-CT conditions are related to
birth month:

- Nonvenomous insect bite
- Venereal disease screening
- Vomiting

What?!! Relevance? Looking at their seasonal pattern graphic in the same table,
it looks like if you were born in October then you're out of luck, you're more
likely to receive a nonvenomous insect bite&hellip;

Sorry to be the harbinger of doom.

#### References

<div style="font-size:75%">

Mary Regina Boland, Zachary Shahn, David Madigan, George Hripcsak, Nicholas
P. Tatonetti. "Birth Month Affects Lifetime Disease Risk: A Phenome-Wide Method"
*Journal of the American Medical Informatics Association* Jun 2015. DOI:
[10.1093/jamia/ocv046](http://dx.doi.org/10.1093/jamia/ocv046)
