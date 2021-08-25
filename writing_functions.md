Writing functions
================

## Do something simple

Calculate Z scores

``` r
x_vec = rnorm(30, mean = 5, sd = 5)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.75587802  0.54592992  0.11313894  1.47426598  1.09021612  0.08941400
    ##  [7] -1.86563307 -0.77349224  0.49146307 -1.56361137 -1.32599234  0.18343320
    ## [13]  0.41849297  1.78727616  0.46557422  1.56574195  0.19816466  0.74663170
    ## [19]  0.70232083  0.99702843  0.21253487 -0.31281372 -1.11052806  0.38721419
    ## [25] -0.08738109 -0.35126111 -2.18449190 -0.59270108 -1.02739401  0.48233681

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

    ##  [1] -0.75587802  0.54592992  0.11313894  1.47426598  1.09021612  0.08941400
    ##  [7] -1.86563307 -0.77349224  0.49146307 -1.56361137 -1.32599234  0.18343320
    ## [13]  0.41849297  1.78727616  0.46557422  1.56574195  0.19816466  0.74663170
    ## [19]  0.70232083  0.99702843  0.21253487 -0.31281372 -1.11052806  0.38721419
    ## [25] -0.08738109 -0.35126111 -2.18449190 -0.59270108 -1.02739401  0.48233681

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
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 -0.0172 0.996

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
    ## 1  4.33  3.14

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
    ## 1  5.69  2.96

``` r
sim_mean_sd(sigma = 3, mu = 6, samp_size = 100)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.97  2.79

``` r
sim_mean_sd(samp_size = 100)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.74  4.54

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
