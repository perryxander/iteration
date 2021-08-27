Iteration and List Columns
================

## Lists

You can put anything in a list

``` r
l = list(
  vec_numeric = 5:8,
  vec_logical = c("TRUE","TRUE","FALSE","TRUE","FALSE","FALSE"),
  mat = matrix(1:8, nrow = 2, ncol = 4),
  summary = summary(rnorm(100))
)
```

``` r
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $vec_logical
    ## [1] "TRUE"  "TRUE"  "FALSE" "TRUE"  "FALSE" "FALSE"
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $summary
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## -2.6534 -0.5953 -0.2283 -0.1351  0.3258  2.6801

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
l[["vec_numeric"]]
```

    ## [1] 5 6 7 8

``` r
mean(l[["vec_numeric"]])
```

    ## [1] 6.5

## ‘for’ loop

Create a new list.

``` r
list_norm =
  list(
    a = rnorm(20, mean = 3, sd = 1),
    b = rnorm(30, mean = 0, sd = 5),
    c = rnorm(40, mean = 10, sd = .2),
    d = rnorm(20, mean = -3, sd = 1)
  )
```

``` r
list_norm
```

    ## $a
    ##  [1] 2.3475448 2.3231826 3.9857939 3.1971987 2.2931758 2.9278346 1.1668100
    ##  [8] 5.1654032 2.4036300 4.0005689 1.3557727 3.0418950 0.7254523 3.5516227
    ## [15] 4.5331403 1.4215956 3.8836314 3.5783530 4.5000787 2.3982108
    ## 
    ## $b
    ##  [1]   2.8357212   3.6468143  -5.3103583   2.6849325   7.0663237   2.8005471
    ##  [7]   2.8828792   5.4930528   1.3201197  -2.3784164 -13.1204274  -2.3803518
    ## [13]   0.1320987   1.1379736  -1.9785071   1.4579443   0.3934187   1.8893814
    ## [19]  -2.8317745   4.3062358  -2.1556466   5.5596304   3.5064214   1.7270683
    ## [25]  -0.2598810   2.7561245   3.5734917   0.5651403  -1.5502118   2.1722451
    ## 
    ## $c
    ##  [1] 10.259389  9.902688  9.849541 10.050613 10.227244  9.914840  9.839798
    ##  [8] 10.197709 10.086456 10.185986 10.106462 10.018930  9.938745 10.328536
    ## [15]  9.994469 10.226791 10.096186 10.065972  9.912318 10.365997 10.300416
    ## [22] 10.185911  9.851656  9.798898  9.983837 10.156496 10.174076 10.134107
    ## [29]  9.899348  9.881412 10.246900  9.787336  9.744365  9.878852 10.112158
    ## [36] 10.299149 10.100288 10.218117  9.907814  9.943757
    ## 
    ## $d
    ##  [1] -1.475357 -3.703224 -2.552936 -2.808687 -2.402384 -3.725695 -2.411654
    ##  [8] -1.935514 -3.057432 -2.399152 -4.324111 -2.729675 -2.622754 -2.513849
    ## [15] -3.540090 -3.708794 -3.196842 -4.302081 -2.705710 -2.214511

Pause and get my old function

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

I can apply that function to each list element.

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.94  1.22

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.865  3.86

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.1 0.170

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.92 0.764

Let’s use a for loop:

``` r
output = vector("list", length = 4)


output[[1]] = mean_and_sd(list_norm[[1]])


for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norm[[i]])
  
}
```
