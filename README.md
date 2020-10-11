John W Pickering
9 October 2020

<!-- README.md is generated from README.Rmd. Please edit that file -->

# rap

<!-- badges: start -->

<!-- badges: end -->

The rap package contains functions for generating statistical metrics
and visual means to assess the improvement in risk prediction of one
risk model over another. It includes the Risk Assessment Plot (hence
rap).

## Installation

You can install the development version or rap from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("JohnPickering/rap")
```

## History and versions

rap began as Matlab code in 2012 after I wrote a paper \[1\] for the
Nephrology community on assessing the added value of one biomarker to a
clinical prediction model. I worked with Professor Zoltan Endre on that
paper. Dr David Cairns kindly provided some R code for the Risk
Assessment Plot. This formed the basis of versions 0.1 to 0.4.
Importantly, for those versions and the current version all errors are
mine (sorry) and not those of Professor Endre or Dr Cairns.

Version 1.02 is the current version and represents a major change and
update. First, I have used piping in the code where possible - hopefully
to speed things up. Second, I have dropped some statistical metrics that
I believe are poor or wrongly applied. In particularly, I dropped
providing the total NRI (Net Reclassification Improvement) and total IDI
(Integrated Discrimination Improvement) metrics. These should never be
presented because they inappropriately add together two fractions with
differing denominators (NRI) or two means (IDI). Instead, these the NRIs
and IDIs for those with and without the event of interest should be
provided. Third, I have provided the change in AUCs rather than a
p-value because the change is much more meaningful.

This version allows as input logistic regression models from glm()
(stats) and lrm (rms) as well as risk predictions calculated elsewhere.

This version provides as outputs in addtion to the Risk Assessment Plot,
a form of calibration plot and decision curve.

Finally, the output from the main functions CI.raplot, and Ci.classNRI
are lists that include the metrics for each bootstrap sample as well as
the summary metrics. Bootstrapping is used to determine confidence
intervals.

## Example 1

This is a basic example for assessing the difference between two
logistic regression models:

``` r
library(rap)
## basic example code

baseline_risk <- data_risk$baseline # or the glm model itself
new_risk <- data_risk$new # or the glm model itself
outcome <- data_risk$outcome

assessment <- CI.raplot(x1 = baseline_risk, x2 = new_risk, y = outcome,
                        n.boot = 20, dp = 2) # Note the default is 2000 bootstraps (n.boot = 2000).  This can take quite some time to run, so when testing I use a smaller number of bootstraps.  
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Setting levels: control = 0, case = 1
#> Setting direction: controls < cases
#> Warning: The `x` argument of `as_tibble.matrix()` must have unique column names if `.name_repair` is omitted as of tibble 2.0.0.
#> Using compatibility `.name_repair`.
#> This warning is displayed once every 8 hours.
#> Call `lifecycle::last_warnings()` to see where this warning was generated.

# View results  
## meta data  
(assessment$meta_data)
#>   Thresholds Confidence.interval Number.of.bootstraps Input.data.type
#> 1   baseline                  95                   20   User supplied
#>   X..decimal.places
#> 1                 2

## exact point estimates  
(assessment$Metrics)
#> $n
#> [1] 433
#> 
#> $n_event
#> [1] 86
#> 
#> $n_non_event
#> [1] 347
#> 
#> $Prevalence
#> [1] 0.1986143
#> 
#> $NRI_up_event
#> [1] 20
#> 
#> $NRI_up_nonevent
#> [1] 19
#> 
#> $NRI_down_event
#> [1] 13
#> 
#> $NRI_down_nonevent
#> [1] 79
#> 
#> $NRI_event
#> [1] 0.08139535
#> 
#> $NRI_nonevent
#> [1] 0.1729107
#> 
#> $IDI_event
#> [1] 0.1363479
#> 
#> $IDI_nonevent
#> [1] 0.03397117
#> 
#> $IP_baseline
#> [1] 0.1849132
#> 
#> $IS_baseline
#> [1] 0.2516693
#> 
#> $IP_new
#> [1] 0.1504058
#> 
#> $IS_new
#> [1] 0.388494
#> 
#> $Brier_baseline
#> [1] 0.1500246
#> 
#> $Brier_new
#> [1] 0.123228
#> 
#> $Brier_skill
#> [1] 17.86145
#> 
#> $AUC_baseline
#> [1] 0.6823772
#> 
#> $AUC_new
#> [1] 0.8227331
#> 
#> $AUC_difference
#> [1] 0.1403559

## bootstrap derived metrics with confidence intervals  
(assessment$Summary_metrics)
#> # A tibble: 22 x 2
#>    metric            statistics                
#>    <chr>             <chr>                     
#>  1 n                 434 (CI: 427.43 to 437.57)
#>  2 n_event           85 (CI: 66.28 to 96.67)   
#>  3 n_non_event       349 (CI: 334.43 to 368.67)
#>  4 Prevalence        0.2 (CI: 0.15 to 0.22)    
#>  5 NRI_up_event      19.5 (CI: 9.38 to 30.72)  
#>  6 NRI_up_nonevent   20.5 (CI: 15.95 to 27.05) 
#>  7 NRI_down_event    10.5 (CI: 5 to 18)        
#>  8 NRI_down_nonevent 83 (CI: 49.75 to 120.62)  
#>  9 NRI_event         0.09 (CI: 0.02 to 0.23)   
#> 10 NRI_nonevent      0.17 (CI: 0.08 to 0.26)   
#> # … with 12 more rows
```

## Graphical assessments

### The Risk Assessment Plot

``` r
ggrap(x1 = baseline_risk, x2 = new_risk, y = outcome)
```

<img src="man/figures/README-ggrap-1.png" width="100%" />

### The calibration curve

``` r
ggcalibrate(x1 = baseline_risk, x2 = new_risk, y = outcome)
#> $g
```

<img src="man/figures/README-ggcalibrate-1.png" width="100%" />

### The decision curve

``` r
ggdecision(x1 = baseline_risk, x2 = new_risk, y = outcome)
#> `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="man/figures/README-ggdecision-1.png" width="100%" />

## Example 2

This is a basic example for assessing the difference in the results of
reclassification:

``` r
## basic example code

baseline_class <- data_class$base_class
new_class <- data_class$new_class
outcome_class <- data_class$Outcome

class_assessment <- CI.classNRI(c1 = baseline_class, c2 = new_class, y = outcome_class, n.boot = 20, dp = 2) # Note the default is 2000 bootstraps (n.boot = 2000).  This can take quite some time to run, so when testing I use a smaller number of bootstraps.  

# View results  
## meta data  
#(class_assessment$meta_data)

## exact point estimates and confusion matrices
#(class_assessment$Metrics)

## bootstrap derived metrics with confidence intervals  
#(class_assessment$Summary_metrics)
```
