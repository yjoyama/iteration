writing functions
================
Yuki Joyama
2023-10-26

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.3     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'
    ## 
    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

Set seed for reproducibility

``` r
set.seed(1)
```

### Z score function

``` r
x_vec = rnorm(25, mean = 5, sd = 3)
```

Compute Z scores for `x_vec`

``` r
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.83687228  0.01576465 -1.05703126  1.50152998  0.16928872 -1.04107494
    ##  [7]  0.33550276  0.59957343  0.42849461 -0.49894708  1.41364561  0.23279252
    ## [13] -0.83138529 -2.50852027  1.00648110 -0.22481531 -0.19456260  0.81587675
    ## [19]  0.68682298  0.44756609  0.78971253  0.64568566 -0.09904161 -2.27133861
    ## [25]  0.47485186

Write a function to do this!

``` r
z_score = function (x) {
  z = (x - mean(x)) / sd(x)
  z
}
```

Check that this works

``` r
z_score(x_vec)
```

    ##  [1] -0.83687228  0.01576465 -1.05703126  1.50152998  0.16928872 -1.04107494
    ##  [7]  0.33550276  0.59957343  0.42849461 -0.49894708  1.41364561  0.23279252
    ## [13] -0.83138529 -2.50852027  1.00648110 -0.22481531 -0.19456260  0.81587675
    ## [19]  0.68682298  0.44756609  0.78971253  0.64568566 -0.09904161 -2.27133861
    ## [25]  0.47485186

Keep checking

``` r
z_score(3)
```

    ## [1] NA

``` r
z_score("my name is jeff")
```

    ## Warning in mean.default(x): argument is not numeric or logical: returning NA

    ## Error in x - mean(x): non-numeric argument to binary operator

``` r
z_score(c(T, T, F, T))
```

    ## [1]  0.5  0.5 -1.5  0.5

``` r
z_score(iris)
```

    ## Warning in mean.default(x): argument is not numeric or logical: returning NA

    ## Warning in Ops.factor(left, right): '-' not meaningful for factors

    ## Error in is.data.frame(x): 'list' object cannot be coerced to type 'double'

``` r
z_score = function (x) {
  
  if (!is.numeric(x)) {
    stop("Argument should be numbers")
  } else if (length(x) < 2) {
    stop("You need at least 2 numbers to get z scores")
  }
    
  z = (x - mean(x)) / sd(x)
  
  z
}


z_score(3)
```

    ## Error in z_score(3): You need at least 2 numbers to get z scores

``` r
z_score("my name is jeff")
```

    ## Error in z_score("my name is jeff"): Argument should be numbers

``` r
z_score(c(T, T, F, T))
```

    ## Error in z_score(c(T, T, F, T)): Argument should be numbers

``` r
z_score(iris)
```

    ## Error in z_score(iris): Argument should be numbers

## Multiple outputs

Write a function that returns the mean and sd from a sample of numbers

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument should be numbers")
  } else if (length(x) < 2) {
    stop("You need at least 2 numbers to get z scores")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)
  
  tibble(
    mean = mean_x,
    sd = sd_x
  )
}
```

Double check I did it right

``` r
mean_and_sd(x_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.51  2.85

### Start getiing means and sds

``` r
x_vec = rnorm(n = 30, mean = 5, sd = .5)

tibble(
  mean = mean(x_vec),
  sd = sd(x_vec)
)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.02 0.371

Let’s write a function that uses `n`, a true mean, and true SD as inputs

``` r
sim_mean_sd = function(n_obs, mu, sigma) {
  x_vec = rnorm(n = n_obs, mean = mu, sd = sigma)

  tibble(
    mean = mean(x_vec),
    sd = sd(x_vec)
  )
}

sim_mean_sd(n_obs = 30, mu = 50, sigma = 10)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  50.9  10.3

``` r
sim_mean_sd(12, 24, 4)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  25.7  3.28
