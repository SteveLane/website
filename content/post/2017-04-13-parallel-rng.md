---
date: 2017-04-13
title: "Random number generation in parallel"
comments: true
tags: ["r", "reproducible research"]
slug: "parallel-rng"
---

I've never been entirely clear on random number generation in parallel within R. So I'm just making a quick post on this so I don't forget, and in case others find it useful.

Using the R package `parallel`, we may wish to generate random numbers on each worker, and moreover, require it to be reproducible. For example, We might be running simulations and/or generating data. When others wish to use our work, we want them to generate the same data!

The standard approach to generating reproducible random numbers is to use `set.seed()`; for example:


```r
## Set the seed
set.seed(42)
## Generate a random uniform number
runif(1)
```

```
## [1] 0.915
```

```r
## Set the seed
set.seed(42)
## Generate a random uniform number
runif(1)
```

```
## [1] 0.915
```

which you can clearly see that we get the same result in both cases.

Unfortunately this approach doesn't work when using `parallel`:


```r
## Load the parallel library
library(parallel)
## Let parallel know it can use half the available cores
options(mc.cores = parallel::detectCores()/2)
## Set the seed
set.seed(42)
## Generate 4 random numbers using mclapply
mclapply(1:4, function(i) runif(1))
```

```
## [[1]]
## [1] 0.429
## 
## [[2]]
## [1] 0.951
## 
## [[3]]
## [1] 0.693
## 
## [[4]]
## [1] 0.0254
```

```r
## Set the seed
set.seed(42)
## Generate 4 random numbers using mclapply
mclapply(1:4, function(i) runif(1))
```

```
## [[1]]
## [1] 0.763
## 
## [[2]]
## [1] 0.759
## 
## [[3]]
## [1] 0.26
## 
## [[4]]
## [1] 0.102
```

The generated numbers are clearly different! This is not too good for our reproducibility! What we need is a seed that allows us to reproduce results across all cores. The `parallel` package has that ability baked in; we just need to set the seed using the `"L'Ecuyer"` option (see the `?RNGkind` help file for details):


```r
## Set the seed
set.seed(42, "L'Ecuyer")
## Generate 4 random numbers using mclapply
mclapply(1:4, function(i) runif(1))
```

```
## [[1]]
## [1] 0.868
## 
## [[2]]
## [1] 0.417
## 
## [[3]]
## [1] 0.102
## 
## [[4]]
## [1] 0.889
```

```r
## Set the seed
set.seed(42, "L'Ecuyer")
## Generate 4 random numbers using mclapply
mclapply(1:4, function(i) runif(1))
```

```
## [[1]]
## [1] 0.868
## 
## [[2]]
## [1] 0.417
## 
## [[3]]
## [1] 0.102
## 
## [[4]]
## [1] 0.889
```

There we go! Reproducible random numbers in parallel. There's a slight caveat though: by default, `mclapply` calls `mc.reset.stream` by default after all workers/streams have finished. This means that if you run `mclapply` again without setting a new seed (or generating some other random numbers in-between), you'll get the same results:


```r
## Generate 4 random numbers using mclapply (no seed set)
mclapply(1:4, function(i) runif(1))
```

```
## [[1]]
## [1] 0.868
## 
## [[2]]
## [1] 0.417
## 
## [[3]]
## [1] 0.102
## 
## [[4]]
## [1] 0.889
```

If you need to generate some new random numbers, you'll need to set a new seed (if you want them reproducible); if you don't want them to be reproducible, reset your random number generator kind back to R's default: `RNGkind("Mersenne")`.
