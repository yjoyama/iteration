Iteration and listcols
================
Yuki Joyama
2023-10-31

### List

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

``` r
set.seed(1)

vec_numeric = 1:4
vec_char = c("My", "name", "is", "Jeff")
vec_logical = c(TRUE, TRUE, TRUE, FALSE)

tibble(
  num = vec_numeric,
  char = vec_char
)
```

    ## # A tibble: 4 × 2
    ##     num char 
    ##   <int> <chr>
    ## 1     1 My   
    ## 2     2 name 
    ## 3     3 is   
    ## 4     4 Jeff

Dufferent stuff with different lengths

``` r
l = list(
  vec_numeric = 1:5,
  vec_char = LETTERS,
  matrix = matrix(1:10, nrow = 5, ncol = 2),
  summary = summary(rnorm(100))
)
```

Accessing lists

``` r
l$vec_numeric
```

    ## [1] 1 2 3 4 5

``` r
l[[2]]
```

    ##  [1] "A" "B" "C" "D" "E" "F" "G" "H" "I" "J" "K" "L" "M" "N" "O" "P" "Q" "R" "S"
    ## [20] "T" "U" "V" "W" "X" "Y" "Z"

``` r
l[["summary"]]
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## -2.2147 -0.4942  0.1139  0.1089  0.6915  2.4016

### loops

``` r
list_norm_samples = list(
  a = rnorm(20, 1, 5), 
  b = rnorm(20, 0, 7),
  c = rnorm(20, 20, 1),
  d = rnorm(20, -45, 13)
)
```

``` r
# mean_and_sd function
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

``` r
mean_and_sd(list_norm_samples$a)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.57  4.07

``` r
mean_and_sd(list_norm_samples$b)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -1.52  4.68

``` r
mean_and_sd(list_norm_samples$c)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  19.6  1.18

``` r
mean_and_sd(list_norm_samples$d)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -38.7  12.7

``` r
output = vector("list", length = 4)

for(i in 1:4) {
  output[[i]] = mean_and_sd(list_norm_samples[[i]])
}
```

### use `map`

``` r
output = map(list_norm_samples, mean_and_sd)

map(list_norm_samples, median)
```

    ## $a
    ## [1] 0.03822194
    ## 
    ## $b
    ## [1] -1.379476
    ## 
    ## $c
    ## [1] 19.31396
    ## 
    ## $d
    ## [1] -40.88785

``` r
map(list_norm_samples, summary)
```

    ## $a
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -3.55461 -1.50663  0.03822  1.56914  3.74909  9.83644 
    ## 
    ## $b
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -10.7551  -3.8040  -1.3795  -1.5209  -0.0923   9.4013 
    ## 
    ## $c
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   18.09   18.69   19.31   19.60   20.13   22.09 
    ## 
    ## $d
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##  -63.52  -47.18  -40.89  -38.74  -31.54  -15.00
