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
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -2.36064 -0.66306  0.14830  0.04797  0.66802  2.07741

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
    ##  [1] 3.1861271 3.2771629 2.4666993 4.6405084 3.5554544 2.4799965 1.9508638
    ##  [8] 2.8187005 0.1865435 0.3970841 4.5730394 3.3803927 2.7595174 3.7171395
    ## [15] 4.8300652 2.1839292 2.0725880 4.3841161 4.2367620 2.3633825
    ## 
    ## $b
    ##  [1]  -0.3074816   1.7744352   9.8147943   2.9463807   0.9379882   0.2646711
    ##  [7]   5.9607585   4.5564951  -0.1860114   8.0467886   3.2407783   2.8489714
    ## [13]   2.9492795   2.2697298  -4.0903583   0.2991706  -1.7634931   3.3930147
    ## [19]  -3.3850645   2.8956469  -6.8283482   1.3188546  -1.5532431   2.0539742
    ## [25] -11.5971333   5.2985475  -2.2591078  -4.6230347   6.0720130   6.0390517
    ## 
    ## $c
    ##  [1] 10.194899 10.239776 10.222178 10.206813  9.929610 10.290851 10.538515
    ##  [8]  9.831521 10.257046 10.178204 10.120625  9.626707  9.810799  9.896588
    ## [15]  9.862949 10.261629 10.059501 10.209307  9.643502  9.894978  9.988201
    ## [22] 10.107588  9.723005  9.759504  9.905726 10.030258  9.813171 10.155163
    ## [29]  9.959994  9.753327  9.644020 10.024490 10.169090 10.299441  9.955125
    ## [36] 10.212591 10.085267 10.351245  9.815229  9.906098
    ## 
    ## $d
    ##  [1] -2.176680 -2.673643 -2.518075 -1.926033 -2.937512 -1.614531 -3.978086
    ##  [8] -2.350346 -2.357479 -1.790890 -1.862333 -5.664847 -2.246129 -4.176601
    ## [15] -4.631341 -2.655369 -4.540850 -2.988216 -1.913787 -3.150854

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
    ## 1  2.97  1.29

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.21  4.50

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.220

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.91  1.12

Let’s use a for loop:

``` r
output = vector("list", length = 4)

output[[1]] = mean_and_sd(list_norm[[1]])


for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norm[[i]])
  
}
```

## Let’s try map!

``` r
output = map(list_norm, mean_and_sd)
```

What if you want a different function..?

``` r
output = map(list_norm, median)
```

``` r
output = map_dbl(list_norm, median)
```

``` r
output = map_df(list_norm, mean_and_sd, .id = "input")
```

## List columns

``` r
listcol_df =
  tibble(
    name = c("a","b","c","d"),
    samp = list_norm
  )
```

``` r
listcol_df %>% pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% pull(samp)
```

    ## $a
    ##  [1] 3.1861271 3.2771629 2.4666993 4.6405084 3.5554544 2.4799965 1.9508638
    ##  [8] 2.8187005 0.1865435 0.3970841 4.5730394 3.3803927 2.7595174 3.7171395
    ## [15] 4.8300652 2.1839292 2.0725880 4.3841161 4.2367620 2.3633825
    ## 
    ## $b
    ##  [1]  -0.3074816   1.7744352   9.8147943   2.9463807   0.9379882   0.2646711
    ##  [7]   5.9607585   4.5564951  -0.1860114   8.0467886   3.2407783   2.8489714
    ## [13]   2.9492795   2.2697298  -4.0903583   0.2991706  -1.7634931   3.3930147
    ## [19]  -3.3850645   2.8956469  -6.8283482   1.3188546  -1.5532431   2.0539742
    ## [25] -11.5971333   5.2985475  -2.2591078  -4.6230347   6.0720130   6.0390517
    ## 
    ## $c
    ##  [1] 10.194899 10.239776 10.222178 10.206813  9.929610 10.290851 10.538515
    ##  [8]  9.831521 10.257046 10.178204 10.120625  9.626707  9.810799  9.896588
    ## [15]  9.862949 10.261629 10.059501 10.209307  9.643502  9.894978  9.988201
    ## [22] 10.107588  9.723005  9.759504  9.905726 10.030258  9.813171 10.155163
    ## [29]  9.959994  9.753327  9.644020 10.024490 10.169090 10.299441  9.955125
    ## [36] 10.212591 10.085267 10.351245  9.815229  9.906098
    ## 
    ## $d
    ##  [1] -2.176680 -2.673643 -2.518075 -1.926033 -2.937512 -1.614531 -3.978086
    ##  [8] -2.350346 -2.357479 -1.790890 -1.862333 -5.664847 -2.246129 -4.176601
    ## [15] -4.631341 -2.655369 -4.540850 -2.988216 -1.913787 -3.150854

``` r
listcol_df %>%
  filter(name == "a")
```

    ## # A tibble: 1 x 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

Let’s try some operations

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.97  1.29

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.97  1.29

Can I just … map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.97  1.29
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.21  4.50
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.220
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.91  1.12

So…. can I add a list column ??

``` r
listcol_df =
  listcol_df %>%
  mutate(summary = map(samp, mean_and_sd),
         medians = map_dbl(samp, median))
```
