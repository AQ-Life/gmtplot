
<!-- README.md is generated from README.Rmd. Please edit that file -->

# gmtplot

<!-- badges: start -->
<!-- badges: end -->

The goal of gmtplot is to draw GMT (Geometric Mean Titer) plot in
immunogenicity in vaccine clinical trials. Please refer to [Generation
of Geometric Mean Titer Plot in Immunogenicity from SAS and
R](https://www.lexjansen.com/pharmasug-cn/2023/CC/Pharmasug-China-2023-CC115.pdf)
from PharmaSUG.

## Installation

You can install the development version of gmtplot from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("AQ-Life/gmtplot")
```

## Example

This is a basic example which shows you how to solve a common problem:

``` r
library(gmtplot)
#> Loading required package: tidyverse
#> ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
#> ✔ dplyr     1.1.4     ✔ readr     2.1.4
#> ✔ forcats   1.0.0     ✔ stringr   1.5.1
#> ✔ ggplot2   3.4.4     ✔ tibble    3.2.1
#> ✔ lubridate 1.9.3     ✔ tidyr     1.3.0
#> ✔ purrr     1.0.2     
#> ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
#> ✖ dplyr::filter() masks stats::filter()
#> ✖ dplyr::lag()    masks stats::lag()
#> ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
library(haven)

adis <- read_sas("adis.sas7bdat")

adis <- adis %>% 
  dplyr::filter(IPPSFL == "Y", PARAMCD == "SAR2NAB", ANL01FL == "Y") %>% 
  mutate(Gvar = if_else(COHORT == "Cohort 2",TRTAN+2, TRTAN),
         AVISITN = if_else(AVISITN == 42, 28, AVISITN))

gmtplot(datain = adis,
        GrpVar = adis$Gvar,
        GrpLabel = c("Group A", "Group B", "Group C", "Group D"),
        AvisitnVar = adis$AVISITN,
        AvisintVal = c(0, 28),
        AvisitLabel = c("D0", "D28"),
        Aval = adis$AVAL,
        Base = adis$BASE,
        YLabel = "PRNT50",
        LegendLabel = c(),
        colorSet = c("grey", "blue", "grey", "blue"),
        # LineYN = TRUE,
        # LegendYN = TRUE,
        FigureName = "gmt_plot")
#> Warning: Removed 686 rows containing missing values (`position_stack()`).
#> [1] "gmt_plot.png"
```

## Additional Requirements

The parameter “datain” from “gmtplot” function needs to generated per
ADaM IG and includes main variables that will be used are listed as
below.

| Variable | Label                     |
|----------|---------------------------|
| USUBJID  | Unique Subject Identifier |
| TRTAN    | Actual Treatment (N)      |
| AVISITN  | Analysis Visit (N)        |
| AVAL     | Analysis Value            |
| BASE     | Baseline Value            |