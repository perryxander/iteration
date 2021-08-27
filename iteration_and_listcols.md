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
    ## -3.16878 -0.77898 -0.02487 -0.06434  0.65595  2.11738

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
    ##  [1] 3.4201197 1.7564744 1.5712267 3.4206393 4.0179810 2.9053101 0.9124786
    ##  [8] 2.8963880 0.9367699 4.8846365 3.1355199 3.7913791 3.0724918 2.7399858
    ## [15] 2.8712800 3.1096841 1.9806633 2.7511148 1.9431437 3.6256445
    ## 
    ## $b
    ##  [1]  -0.31376673   0.90910449  -1.55011920   1.64988837  -9.67760164
    ##  [6]  -5.27529066   0.96263112   4.22222824  -4.01936094  10.08062554
    ## [11]   1.99956595   2.38044986  -9.53527709  -5.64991048   0.01568217
    ## [16]  -2.62502291  -5.29419800 -10.02109761   3.83500226  -4.56863341
    ## [21]  -6.31016405  -0.26925260  -7.69801924   0.21567482   4.71928590
    ## [26]   4.60334624  -0.68986648   4.91062596   3.34596911  -3.88517639
    ## 
    ## $c
    ##  [1] 10.415433  9.863520 10.154097 10.037717 10.042970  9.905743  9.947735
    ##  [8]  9.965461  9.989803 10.341590 10.000536  9.751820 10.299556 10.201109
    ## [15]  9.581564 10.125547  9.931132  9.801377 10.239098 10.152701 10.013756
    ## [22]  9.895345 10.059940  9.801479 10.347160  9.832917  9.727708  9.988981
    ## [29] 10.029025 10.029748  9.764506  9.816098  9.878818  9.654348 10.133796
    ## [36]  9.889112  9.874311  9.880825  9.787103  9.566734
    ## 
    ## $d
    ##  [1] -2.660862 -3.785405 -3.774106 -4.106210 -1.326980 -3.292345 -3.935394
    ##  [8] -3.192184 -3.248948 -4.029785 -3.650264 -1.521036 -2.827662 -4.299728
    ## [15] -1.730771 -2.262739 -3.798459 -3.807753 -3.340522 -2.916349

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
    ## 1  2.79  1.02

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -1.12  4.99

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.97 0.203

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.18 0.881

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
    ##  [1] 3.4201197 1.7564744 1.5712267 3.4206393 4.0179810 2.9053101 0.9124786
    ##  [8] 2.8963880 0.9367699 4.8846365 3.1355199 3.7913791 3.0724918 2.7399858
    ## [15] 2.8712800 3.1096841 1.9806633 2.7511148 1.9431437 3.6256445
    ## 
    ## $b
    ##  [1]  -0.31376673   0.90910449  -1.55011920   1.64988837  -9.67760164
    ##  [6]  -5.27529066   0.96263112   4.22222824  -4.01936094  10.08062554
    ## [11]   1.99956595   2.38044986  -9.53527709  -5.64991048   0.01568217
    ## [16]  -2.62502291  -5.29419800 -10.02109761   3.83500226  -4.56863341
    ## [21]  -6.31016405  -0.26925260  -7.69801924   0.21567482   4.71928590
    ## [26]   4.60334624  -0.68986648   4.91062596   3.34596911  -3.88517639
    ## 
    ## $c
    ##  [1] 10.415433  9.863520 10.154097 10.037717 10.042970  9.905743  9.947735
    ##  [8]  9.965461  9.989803 10.341590 10.000536  9.751820 10.299556 10.201109
    ## [15]  9.581564 10.125547  9.931132  9.801377 10.239098 10.152701 10.013756
    ## [22]  9.895345 10.059940  9.801479 10.347160  9.832917  9.727708  9.988981
    ## [29] 10.029025 10.029748  9.764506  9.816098  9.878818  9.654348 10.133796
    ## [36]  9.889112  9.874311  9.880825  9.787103  9.566734
    ## 
    ## $d
    ##  [1] -2.660862 -3.785405 -3.774106 -4.106210 -1.326980 -3.292345 -3.935394
    ##  [8] -3.192184 -3.248948 -4.029785 -3.650264 -1.521036 -2.827662 -4.299728
    ## [15] -1.730771 -2.262739 -3.798459 -3.807753 -3.340522 -2.916349

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
    ## 1  2.79  1.02

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.79  1.02

Can I just … map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.79  1.02
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -1.12  4.99
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.97 0.203
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.18 0.881

So…. can I add a list column ??

``` r
listcol_df =
  listcol_df %>%
  mutate(summary = map(samp, mean_and_sd),
         medians = map_dbl(samp, median))
```

## Weather Data

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: C:\Users\perry\AppData\Local/Cache/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2021-07-06 10:06:19 (7.601)

    ## file min/max dates: 1869-01-01 / 2021-07-31

    ## using cached file: C:\Users\perry\AppData\Local/Cache/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2021-07-06 10:06:28 (1.703)

    ## file min/max dates: 1965-01-01 / 2020-03-31

    ## using cached file: C:\Users\perry\AppData\Local/Cache/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2021-07-06 10:06:32 (0.914)

    ## file min/max dates: 1999-09-01 / 2021-07-31

Get out list columns ..

``` r
weather_nest =
  weather_df %>%
  nest(data = date:tmin)
```

``` r
weather_nest %>% pull(name)
```

    ## [1] "CentralPark_NY" "Waikiki_HA"     "Waterhole_WA"

``` r
weather_nest %>% pull(data)
```

    ## [[1]]
    ## # A tibble: 365 x 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # ... with 355 more rows
    ## 
    ## [[2]]
    ## # A tibble: 365 x 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0  26.7  16.7
    ##  2 2017-01-02     0  27.2  16.7
    ##  3 2017-01-03     0  27.8  17.2
    ##  4 2017-01-04     0  27.2  16.7
    ##  5 2017-01-05     0  27.8  16.7
    ##  6 2017-01-06     0  27.2  16.7
    ##  7 2017-01-07     0  27.2  16.7
    ##  8 2017-01-08     0  25.6  15  
    ##  9 2017-01-09     0  27.2  15.6
    ## 10 2017-01-10     0  28.3  17.2
    ## # ... with 355 more rows
    ## 
    ## [[3]]
    ## # A tibble: 365 x 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01   432  -6.8 -10.7
    ##  2 2017-01-02    25 -10.5 -12.4
    ##  3 2017-01-03     0  -8.9 -15.9
    ##  4 2017-01-04     0  -9.9 -15.5
    ##  5 2017-01-05     0  -5.9 -14.2
    ##  6 2017-01-06     0  -4.4 -11.3
    ##  7 2017-01-07    51   0.6 -11.5
    ##  8 2017-01-08    76   2.3  -1.2
    ##  9 2017-01-09    51  -1.2  -7  
    ## 10 2017-01-10     0  -5   -14.2
    ## # ... with 355 more rows

``` r
weather_nest$data[[1]] #tibble for central park
```

    ## # A tibble: 365 x 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # ... with 355 more rows

Suppose I want to regress ‘tmax’ on ‘timin’ for each station.

This works….

``` r
lm(tmax ~ tmin, data = weather_nest$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest$data[[1]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

``` r
lm(tmax ~ tmin, data = weather_nest$data[[2]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest$data[[2]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509

Let’s write a function.

``` r
weather_lm = function(df) {
  
  lm(tmax ~ tmin, data = df)
  
}


output = vector("list",3)

for (i in 1:3) {
  
  output[[i]] = weather_lm(weather_nest$data[[i]])
  
}

#same as below

weather_lm(weather_nest$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

``` r
weather_lm(weather_nest$data[[2]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509

``` r
weather_lm(weather_nest$data[[3]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

What about a map ….

``` r
map(weather_nest$data, weather_lm)
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

What about a map in a list column !!!!??

``` r
weather_nest =
  weather_nest %>%
    mutate(models = map(data, weather_lm))

weather_nest$models
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221
