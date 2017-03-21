
<!-- README.md is generated from README.Rmd. Please edit that file -->
purrr <img src="logo.png" align="right" />
==========================================

[![CRAN\_Status\_Badge](http://www.r-pkg.org/badges/version/purrr)](http://cran.r-project.org/package=purrr) [![Build Status](https://travis-ci.org/tidyverse/purrr.svg?branch=master)](https://travis-ci.org/tidyverse/purrr) [![Coverage Status](https://img.shields.io/codecov/c/github/tidyverse/purrr/master.svg)](https://codecov.io/github/tidyverse/purrr?branch=master)

Overview
--------

purrr enhances R's functional programming (FP) toolkit by providing a complete and consistent set of tools for working with functions and vectors. If you've never heard of FP before, the best place to start is the family of `map()` functions which allow you to replace many for loops with code that is both more succinct and easier to read. The best place to learn about the `map()` functions is the [iteration chapter](http://r4ds.had.co.nz/iteration.html) in R for data science.

Installation
------------

``` r
# The easiest way to get purrr is to install the whole tidyverse:
install.packages("tidyverse")

# Alternatively, install just purrr:
install.packages("purrr")

# Or the the development version from GitHub:
# install.packages("devtools")
devtools::install_github("tidyverse/purrr")
```

Usage
-----

The following example uses purrr to solve a fairly realistic problem: split a data frame into pieces, fit a model to each piece, compute the summarse, then extract the R<sup>2</sup>.

``` r
library(purrr)

mtcars %>%
  split(.$cyl) %>% # from base R
  map(~ lm(mpg ~ wt, data = .)) %>%
  map(summary) %>%
  map_dbl("r.squared")
#>         4         6         8 
#> 0.5086326 0.4645102 0.4229655
```

This example illustrates some of the advantages of purrr functions over the equivalents in base R:

-   The first argument is always the data, so purrr works naturally with the pipe.

-   All purrr functions are type-stable. They always return the advertised output type (`map()` returns lists; `map_dbl()` returns double vectors), or they throw an errror.

-   All `map()` functions either accept function, formulas (used for succinctly generating anonymous functions), a character vector (used to extract components by name), or a numeric vector (used to extract by position).

Philosophy
----------

The goal is not to try and simulate Haskell in R: purrr does not implement currying or destructuring binds or pattern matching. The goal is to give you similar expressiveness to an FP language, while allowing you to write code that looks and works like R.

-   Instead of point free style, use the pipe, `%>%`, to write code that can be read from left to right.

-   Instead of currying, we use `...` to pass in extra arguments.

-   Anonymous functions are verbose in R, so we provide two convenient shorthands. For unary functions, `~ .x + 1` is equivalent to `function(.x) .x + 1`. For chains of transformations functions, `. %>% f() %>% g()` is equivalent to `function(.) . %>% f() %>% g()`.

-   R is weakly typed, we need variants `map_int()`, `map_dbl()`, etc since we don't know what `.f` will return.

-   R has named arguments, so instead of providing different functions for minor variations (e.g. `detect()` and `detectLast()`) I use a named argument, `.right`. Type-stable functions are easy to reason about so additional arguments will never change the type of the output.

Related work
------------

-   [rlist](http://renkun.me/rlist/), another R package to support working with lists. Similar goals but somewhat different philosophy.

-   Functional programming librarys for javascript: [underscore.js](http://underscorejs.org), [lodash](https://lodash.com) and [lazy.js](http://danieltao.com/lazy.js/).

-   List operations defined in the Haskell [prelude](http://hackage.haskell.org/package/base-4.7.0.1/docs/Prelude.html#g:11)

-   Scala's [list methods](http://www.scala-lang.org/api/current/index.html#scala.collection.immutable.List).
