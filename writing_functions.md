Writing functions
================

## Do something simple

Calculate Z scores

``` r
x_vec = rnorm(30, mean = 5, sd = 5)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.507010327  1.057378830  0.405518391  1.280731680  0.687813855
    ##  [6] -1.592700331 -0.920639360 -0.431243797  0.317232322 -0.310865944
    ## [11] -1.090174383 -0.513850104  0.362721078  0.505291477 -1.212291729
    ## [16]  0.331433289 -1.076609181 -0.839182032 -0.844936831  0.006503289
    ## [21] -0.527212988  2.874268927 -0.636699765 -0.218632146 -0.269744679
    ## [26]  1.927233662 -0.280631271  0.407901011  1.177285828 -1.082909426

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

    ##  [1]  0.507010327  1.057378830  0.405518391  1.280731680  0.687813855
    ##  [6] -1.592700331 -0.920639360 -0.431243797  0.317232322 -0.310865944
    ## [11] -1.090174383 -0.513850104  0.362721078  0.505291477 -1.212291729
    ## [16]  0.331433289 -1.076609181 -0.839182032 -0.844936831  0.006503289
    ## [21] -0.527212988  2.874268927 -0.636699765 -0.218632146 -0.269744679
    ## [26]  1.927233662 -0.280631271  0.407901011  1.177285828 -1.082909426

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
    ## 1 0.0506 0.988

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
    ## 1  4.07  3.09

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
    ## 1  6.25  2.81

``` r
sim_mean_sd(sigma = 3, mu = 6, samp_size = 100)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.08  3.01

``` r
sim_mean_sd(samp_size = 100)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.87  3.76

## Let’s review Napoleon Dynamite

Used CSS selector for html tags

Let’s turn that code into a function

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = read_html(url)

review_titles = 
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars = 
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>%
  as.numeric()

review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

What about the next page of reviews?

Let’s turn that code into a function

``` r
read_page_reviews = function(url) {
  
  html = read_html(url)

  review_titles = 
   html %>%
   html_nodes(".a-text-bold span") %>%
    html_text()

  review_stars = 
    html %>%
    html_nodes("#cm_cr-review_list .review-rating") %>%
    html_text() %>%
    str_extract("^\\d") %>%
    as.numeric()

  review_text = 
    html %>%
    html_nodes(".review-text-content span") %>%
    html_text() %>% 
    str_replace_all("\n", "") %>% 
    str_trim()

  reviews = tibble(
    title = review_titles,
    stars = review_stars,
    text = review_text
  )

  reviews
  
}
```

Let me try my function.

``` r
dynamite_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=2"

read_page_reviews(dynamite_url)
```

    ## # A tibble: 10 x 3
    ##    title                  stars text                                            
    ##    <chr>                  <dbl> <chr>                                           
    ##  1 "LOVE it"                  5 cult classic. So ugly it's fun. Music , acting,~
    ##  2 "Perfect"                  5 Exactly what I asked for                        
    ##  3 "Love this movie!"         5 Great movie and sent in a nice package! No dama~
    ##  4 "Love it"                  5 Love this movie. However the teenagers of today~
    ##  5 "As described"             3 Book is as described                            
    ##  6 "GOSH!!!"                  5 Just watch the movie GOSH!!!!                   
    ##  7 "Watch it right now"       5 You need to watch this movie today. It’s hilari~
    ##  8 "At this point it’s a~     5 I watch this movie way too much. Have a friend ~
    ##  9 "\U0001f495"               5 Hands down, one of my favorite movies.          
    ## 10 "Good dumb movie"          5 I really wanted to show my spouse a very dumb m~

Let’s read a few pages of reviews

``` r
dynamite_url_base = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="

dynamite_urls = str_c(dynamite_url_base, 1:5)

all_reviews = 
  bind_rows(
    read_page_reviews(dynamite_urls[1]),
    read_page_reviews(dynamite_urls[2]),
    read_page_reviews(dynamite_urls[3]),
    read_page_reviews(dynamite_urls[4]),
    read_page_reviews(dynamite_urls[5])
  )
```

## Mean scoping example

``` r
f = function(x) {
  z = x + y
  z
}

x = 1
y = 2

f(x = y)
```

    ## [1] 4

## Functions as arguments

``` r
my_summary = function(x, summ_func) {
  
  summ_func(x)
  
}

x_vec = rnorm(100, 3, 7)

mean(x_vec)
```

    ## [1] 3.099731

``` r
median(x_vec)
```

    ## [1] 4.067143

``` r
my_summary(x_vec, mean)
```

    ## [1] 3.099731
