
<!-- README.md is generated from README.Rmd. Please edit that file -->

# ggconsort

<!-- badges: start -->
<!-- badges: end -->

The goal of ggconsort is to provide convenience functions for creating
CONSORT diagrams with `ggplot2`.

## Installation

You can install the released version of ggconsort from
[GitHub](https://github.com/tgerke/ggconsort) with:

``` r
devtools::install_github("tgerke/ggconsort")
```

## Example

**This paragraph needs to be modified**

In the following, we filter the `penguins` data to only those from
Biscoe island. In the same `dplyr` chain, we collect the number of
distinct species overall (i.e. before the filter to Biscoe) and the
number of distinct species on Biscoe into a list called `counts`. In the
next step, we will use `counts` to construct a basic data flow diagram.

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
library(ggconsort)
library(palmerpenguins)

penguin_cohorts <- 
  penguins %>%
  mutate(.id = row_number()) %>%
  cohort_start("Penguins observerd by Palmer Station LTER") %>%
  # Define cohorts using named expressions --------------------
  # Notice that you can use previously defined cohorts in subsequent steps
  cohort_define(
    adelie = .full %>% filter(species == "Adelie"),
    adelie_male = adelie %>% filter(sex == "male"),
    biscoe = .full %>% filter(island == "Biscoe"),
    biscoe_adelie_male = biscoe %>% semi_join(adelie_male, by = ".id")
  ) %>%
  # Provide text labels for cohorts ---------------------------
  cohort_label(
    adelie = "Adelie penguins",
    adelie_male = "Adelie male penguins",
    biscoe = "Penguins on Biscoe island",
    biscoe_adelie_male = "Male Adelie penguins on Biscoe island"
  )
```