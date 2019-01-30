
<!-- README.md is generated from README.Rmd. Please edit that file -->

# splice

<!-- badges: start -->

<!-- badges: end -->

`splice` is an experimental package that brings some capabilities of
`dplyr::summarise_(all,if,at)` to `dplyr::summarise()` directly.

## Installation

You can install from github:

``` r
# install.packages("remotes")
devtools::install_github("romainfrancois/splice")
```

## Example

The motivating example was “how do I get the mean of all variables AND
the number of observations”.

There are many ways to get just that information, but I wanted to use
`dplyr::summarise_all()` which would get me the means, but not the
number of observations.

`summarise(!!!all_(...))` is the same as `summarise_all(...)` but having
`!!!all_()` inside `summarise()` lets you add other things as well:

``` r
library(dplyr)
#> 
#> Attaching package: 'dplyr'
#> The following objects are masked from 'package:stats':
#> 
#>     filter, lag
#> The following objects are masked from 'package:base':
#> 
#>     intersect, setdiff, setequal, union
library(splice)

iris %>% 
  group_by(Species) %>% 
  summarise(n = n(), !!!all_(mean))
#> # A tibble: 3 x 6
#>   Species        n Sepal.Length Sepal.Width Petal.Length Petal.Width
#>   <fct>      <int>        <dbl>       <dbl>        <dbl>       <dbl>
#> 1 setosa        50         5.01        3.43         1.46       0.246
#> 2 versicolor    50         5.94        2.77         4.26       1.33 
#> 3 virginica     50         6.59        2.97         5.55       2.03
```

Similarly, `if_()` and `at_()`:

``` r
iris %>%
  summarise(n = n(), !!!if_(is.numeric, mean))
#>     n Sepal.Length Sepal.Width Petal.Length Petal.Width
#> 1 150     5.843333    3.057333        3.758    1.199333

iris %>%
  summarise(n = n(), !!!at_(vars(starts_with("Sepal")), mean))
#>     n Sepal.Length Sepal.Width
#> 1 150     5.843333    3.057333
```
