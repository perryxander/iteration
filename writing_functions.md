Writing functions
================

## Do something simple

Calculate Z scores

``` r
x_vec = rnorm(30, mean = 5, sd = 5)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.53656307  0.35539990 -0.01625656  0.17634834 -0.17928099 -1.59327669
    ##  [7]  0.08966969  0.66973133 -0.02764918 -0.21359011 -0.98689317  1.03490609
    ## [13] -1.59367361 -1.08748570  0.54663877  0.63825818  0.29920110  0.67052454
    ## [19]  0.23683822 -0.41505704  2.05992763 -1.06937133  2.09822559 -0.81283509
    ## [25]  1.93823389 -0.93107536  0.95914720 -0.35309679 -1.29978603 -0.65715978

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

    ##  [1] -0.53656307  0.35539990 -0.01625656  0.17634834 -0.17928099 -1.59327669
    ##  [7]  0.08966969  0.66973133 -0.02764918 -0.21359011 -0.98689317  1.03490609
    ## [13] -1.59367361 -1.08748570  0.54663877  0.63825818  0.29920110  0.67052454
    ## [19]  0.23683822 -0.41505704  2.05992763 -1.06937133  2.09822559 -0.81283509
    ## [25]  1.93823389 -0.93107536  0.95914720 -0.35309679 -1.29978603 -0.65715978

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

## Multiple outputs

``` r
mean_and_sd = function(x) {
  
  if(!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if (length(x) < 3) {
    stop("Input must have at least three numbers")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)
  
  tibble(
    mean = mean_x,
    sd = sd_x
  )
  
  
}
```

Check that the function works.

``` r
x_vec = rnorm(1000)
mean_and_sd(x_vec)
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0183 0.975

## Multiple inputs

I’d like to do this with a function.

``` r
  sim_data = 
    tibble(
      x = rnorm(n = 100, mean = 4, sd = 3)
    )

sim_data %>%
  summarize(
    mean = mean(x),
    sd = sd(x)
  )
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.93  2.96

Function for above.

Can set default value that runs if user doesn’t provide one as shown
below.

``` r
sim_mean_sd = function(samp_size, mu = 3, sigma = 4) {
  
  sim_data = 
    tibble(
      x = rnorm(n = samp_size, mean = mu, sd = sigma)
    )

  sim_data %>%
    summarize(
      mean = mean(x),
      sd = sd(x)
    )
  
}

sim_mean_sd(samp_size = 100, mu= 6, sigma = 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.75  2.67

``` r
sim_mean_sd(sigma = 3, mu = 6, samp_size = 100)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.18  3.03

``` r
sim_mean_sd(samp_size = 100)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.07  4.48
