Writing functions
================

## Do something simple

Calculate Z scores

``` r
x_vec = rnorm(30, mean = 5, sd = 5)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -1.79766634  0.05440553  1.36908340 -1.44966841 -0.57103329  0.53095421
    ##  [7]  0.73857976  1.18289568  1.60961161  1.22720346 -1.10307149 -0.01637798
    ## [13]  1.08498096 -0.32794313 -0.58534144 -1.62244971  0.16957332  0.48907068
    ## [19]  1.25829491 -0.75957278 -0.71980391 -0.23268343  0.64411131  0.71011452
    ## [25] -0.68478706  0.62412676  1.15905111 -1.61149982 -0.74028014 -0.62987829

I want a function to compute z-scores

``` r
z_scores = function(x) {
  
  if(!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if (length(x) < 3) {
    stop("Input must have at least three numbers")
  }
  
  z = (x - mean(x)) / sd(x)
  
  return(z)
}

z_scores(x_vec)
```

    ##  [1] -1.79766634  0.05440553  1.36908340 -1.44966841 -0.57103329  0.53095421
    ##  [7]  0.73857976  1.18289568  1.60961161  1.22720346 -1.10307149 -0.01637798
    ## [13]  1.08498096 -0.32794313 -0.58534144 -1.62244971  0.16957332  0.48907068
    ## [19]  1.25829491 -0.75957278 -0.71980391 -0.23268343  0.64411131  0.71011452
    ## [25] -0.68478706  0.62412676  1.15905111 -1.61149982 -0.74028014 -0.62987829

Try my function on some other things. These should give errors

``` r
z_scores(3)
```

    ## Error in z_scores(3): Input must have at least three numbers

``` r
z_scores("my name is jeff")
```

    ## Error in z_scores("my name is jeff"): Input must be numeric

``` r
z_scores(mtcars)
```

    ## Error in z_scores(mtcars): Input must be numeric

``` r
z_scores(c(TRUE, TRUE, FALSE, TRUE))
```

    ## Error in z_scores(c(TRUE, TRUE, FALSE, TRUE)): Input must be numeric
