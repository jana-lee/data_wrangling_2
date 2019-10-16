data\_wrangling\_2
================
Jana Lee
10/10/2019

# Lecture: Reading Data from the Web

**2 ways to get data:** 1) Data included as content on a webpage, what
you want to “scrape” 2) Dedicated server holding data in a relatively
usable form (data.gov) Styling = CSS, makes stuff pretty

**Application Programming Interfaces (APIs)** - someone has set up a way
to let you access data. Don’t need to extract HTML

**Real talk about web data** - - data from web is messy - it will
frequently take a lot of work to figure out

## Extracting Tables - Reading in NSDH Data

Example is from the National Survey on Drug Use and Health. Includes
tables relating to drug use in the past year or month, separately for
specific kinds of drug
use.

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"
drug_use_xml = 
  read_html(url) 

drug_use_xml %>% 
  html_nodes(css = "table")
```

    ## {xml_nodeset (15)}
    ##  [1] <table class="rti" border="1" cellspacing="0" cellpadding="1" width ...
    ##  [2] <table class="rti" border="1" cellspacing="0" cellpadding="1" width ...
    ##  [3] <table class="rti" border="1" cellspacing="0" cellpadding="1" width ...
    ##  [4] <table class="rti" border="1" cellspacing="0" cellpadding="1" width ...
    ##  [5] <table class="rti" border="1" cellspacing="0" cellpadding="1" width ...
    ##  [6] <table class="rti" border="1" cellspacing="0" cellpadding="1" width ...
    ##  [7] <table class="rti" border="1" cellspacing="0" cellpadding="1" width ...
    ##  [8] <table class="rti" border="1" cellspacing="0" cellpadding="1" width ...
    ##  [9] <table class="rti" border="1" cellspacing="0" cellpadding="1" width ...
    ## [10] <table class="rti" border="1" cellspacing="0" cellpadding="1" width ...
    ## [11] <table class="rti" border="1" cellspacing="0" cellpadding="1" width ...
    ## [12] <table class="rti" border="1" cellspacing="0" cellpadding="1" width ...
    ## [13] <table class="rti" border="1" cellspacing="0" cellpadding="1" width ...
    ## [14] <table class="rti" border="1" cellspacing="0" cellpadding="1" width ...
    ## [15] <table class="rti" border="1" cellspacing="0" cellpadding="1" width ...

``` r
# We can see that this is not too helpful, so we need to pull out HTML nodes. When using `html_nodes`, we see that there are 15 tables produced.

# Use [[]] double brackets to 

table_list = (drug_use_xml %>%  html_nodes(css = "table"))
table_list [[1]] %>% 
  html_table() %>% 
  slice(-1)
```

    ##                   State 12+(2013-2014) 12+(2014-2015) 12+(P Value)
    ## 1            Total U.S.         12.90a          13.36        0.002
    ## 2             Northeast         13.88a          14.66        0.005
    ## 3               Midwest         12.40b          12.76        0.082
    ## 4                 South         11.24a          11.64        0.029
    ## 5                  West          15.27          15.62        0.262
    ## 6               Alabama           9.98           9.60        0.426
    ## 7                Alaska         19.60a          21.92        0.010
    ## 8               Arizona          13.69          13.12        0.364
    ## 9              Arkansas          11.37          11.59        0.678
    ## 10           California          14.49          15.25        0.103
    ## 11             Colorado         20.74a          23.09        0.018
    ## 12          Connecticut         14.00a          15.67        0.011
    ## 13             Delaware          13.98          13.06        0.130
    ## 14 District of Columbia         21.70a          23.51        0.043
    ## 15              Florida         11.87b          12.59        0.082
    ## 16              Georgia          11.75          12.67        0.136
    ## 17               Hawaii          12.58          12.72        0.845
    ## 18                Idaho          11.58          11.40        0.750
    ## 19             Illinois          12.16          12.47        0.471
    ## 20              Indiana         12.86b          13.88        0.086
    ## 21                 Iowa           9.74           9.05        0.134
    ## 22               Kansas         11.01a          12.38        0.011
    ## 23             Kentucky         10.93a          12.28        0.015
    ## 24            Louisiana          11.23          11.22        0.989
    ## 25                Maine          19.55          19.69        0.878
    ## 26             Maryland         13.48a          15.13        0.009
    ## 27        Massachusetts          17.23          18.26        0.163
    ## 28             Michigan          15.60          15.10        0.309
    ## 29            Minnesota          12.22          12.69        0.418
    ## 30          Mississippi           9.40           8.67        0.110
    ## 31             Missouri          12.73          13.53        0.179
    ## 32              Montana         14.07b          15.38        0.085
    ## 33             Nebraska          10.35          10.75        0.435
    ## 34               Nevada          13.01          12.95        0.935
    ## 35        New Hampshire          16.95          17.35        0.595
    ## 36           New Jersey          11.25          11.86        0.270
    ## 37           New Mexico          15.61          14.72        0.267
    ## 38             New York         14.24b          15.04        0.086
    ## 39       North Carolina          12.07          11.79        0.585
    ## 40         North Dakota          10.25           9.90        0.462
    ## 41                 Ohio          11.57          12.13        0.172
    ## 42             Oklahoma          10.75          11.28        0.353
    ## 43               Oregon          19.39          19.42        0.977
    ## 44         Pennsylvania          11.70          12.35        0.105
    ## 45         Rhode Island          18.95          18.81        0.867
    ## 46       South Carolina         11.55b          12.56        0.056
    ## 47         South Dakota          8.97a          10.77        0.001
    ## 48            Tennessee          10.29          11.05        0.151
    ## 49                Texas          9.52b          10.10        0.074
    ## 50                 Utah          9.84b           9.07        0.092
    ## 51              Vermont          19.97          20.50        0.551
    ## 52             Virginia         13.04a          11.54        0.004
    ## 53           Washington         18.92b          17.49        0.077
    ## 54        West Virginia          10.93          11.07        0.805
    ## 55            Wisconsin          11.86          12.05        0.718
    ## 56              Wyoming          10.72          10.87        0.773
    ##    12-17(2013-2014) 12-17(2014-2015) 12-17(P Value) 18-25(2013-2014)
    ## 1            13.28b            12.86          0.063            31.78
    ## 2             13.98            13.51          0.266           34.66a
    ## 3             12.45            12.33          0.726            32.13
    ## 4             12.02            11.88          0.666            28.93
    ## 5            15.53a            14.43          0.018            33.72
    ## 6              9.90             9.71          0.829            26.99
    ## 7             17.30            18.44          0.392           36.47a
    ## 8             15.12            13.45          0.131            31.53
    ## 9             12.79            12.14          0.538            26.53
    ## 10            15.03            14.11          0.190            33.69
    ## 11           20.81b            18.35          0.092            43.95
    ## 12            15.65            15.63          0.989           38.82b
    ## 13           15.14b            13.04          0.079            38.56
    ## 14           19.41a            16.55          0.036            41.07
    ## 15            14.09            13.49          0.392            33.64
    ## 16            11.19            11.68          0.588            27.78
    ## 17            13.90            13.77          0.909            27.32
    ## 18            14.14            13.31          0.456            26.98
    ## 19            12.07            11.53          0.462            31.12
    ## 20            12.56            13.83          0.266            35.62
    ## 21            10.06             9.46          0.551            29.18
    ## 22            11.71            12.45          0.451            28.46
    ## 23            10.34            11.68          0.190            28.05
    ## 24            11.64            10.04          0.108            27.64
    ## 25            17.11            17.51          0.761            42.03
    ## 26            14.96            14.45          0.673            36.43
    ## 27            15.56            15.91          0.770            42.72
    ## 28            15.10            14.04          0.203            35.05
    ## 29            11.64            11.43          0.828            32.91
    ## 30            10.56             9.54          0.277           25.30a
    ## 31            12.48            12.94          0.687            33.32
    ## 32            13.38            14.62          0.307           30.03a
    ## 33            11.40            10.24          0.231            29.18
    ## 34            14.42            13.50          0.444            31.16
    ## 35            17.99            16.41          0.206            44.38
    ## 36            12.86            12.41          0.641           30.10b
    ## 37            15.92            15.15          0.555            31.64
    ## 38            13.94            13.17          0.318           32.52a
    ## 39            12.72            11.44          0.217            29.14
    ## 40            10.25            11.79          0.175            26.76
    ## 41            11.39            11.35          0.957            30.86
    ## 42            11.55            11.74          0.851            28.17
    ## 43            18.32            17.56          0.571            38.05
    ## 44            12.37            11.88          0.508            32.18
    ## 45            16.93            16.28          0.654            44.13
    ## 46            12.50            12.02          0.643           28.56b
    ## 47             9.95            10.86          0.350           24.21a
    ## 48            11.57            11.21          0.730            25.54
    ## 49            10.79            11.70          0.168            25.25
    ## 50            10.12             9.00          0.211            22.65
    ## 51            18.27            17.04          0.342            46.28
    ## 52            12.64            11.42          0.192            32.31
    ## 53            17.53            15.61          0.135            36.50
    ## 54            10.87            12.15          0.239            28.49
    ## 55            14.00            14.04          0.968            32.27
    ## 56            12.03            13.30          0.252            27.50
    ##    18-25(2014-2015) 18-25(P Value) 26+(2013-2014) 26+(2014-2015)
    ## 1             32.07          0.369          9.63a          10.25
    ## 2             36.45          0.008         10.43a          11.22
    ## 3             32.20          0.900          9.03a           9.51
    ## 4             29.20          0.581          8.14a           8.67
    ## 5             33.19          0.460         11.97b          12.71
    ## 6             26.13          0.569           7.10           6.81
    ## 7             40.69          0.015         16.70b          18.76
    ## 8             31.15          0.826          10.40           9.97
    ## 9             27.06          0.730           8.62           8.93
    ## 10            32.72          0.357         10.91a          12.25
    ## 11            45.24          0.521         16.80a          19.91
    ## 12            42.10          0.087          9.83a          11.41
    ## 13            37.32          0.463           9.77           9.17
    ## 14            43.78          0.176         17.69b          19.71
    ## 15            32.88          0.452          8.42a           9.57
    ## 16            29.38          0.275           8.99           9.87
    ## 17            27.21          0.950          10.13          10.37
    ## 18            25.66          0.369           8.55           8.68
    ## 19            31.96          0.476           8.99           9.33
    ## 20            33.55          0.233          8.85a          10.40
    ## 21            27.32          0.252           6.25           5.73
    ## 22            29.27          0.600          7.74a           9.27
    ## 23            28.68          0.693          8.15a           9.62
    ## 24            27.56          0.959           8.24           8.50
    ## 25            41.80          0.904          16.68          16.91
    ## 26            39.01          0.157          9.58a          11.41
    ## 27            42.21          0.791          12.95          14.33
    ## 28            33.41          0.140          12.29          12.08
    ## 29            35.56          0.137           8.93           9.17
    ## 30            21.69          0.014           6.33           6.19
    ## 31            32.67          0.691           9.28          10.39
    ## 32            33.33          0.050          11.51          12.49
    ## 33            30.47          0.424           6.86           7.28
    ## 34            30.46          0.674           9.98          10.19
    ## 35            41.98          0.209          12.55          13.57
    ## 36            32.75          0.083           8.22           8.66
    ## 37            31.08          0.733          12.78          11.87
    ## 38            35.95          0.003          11.11          11.68
    ## 39            31.25          0.168           9.15           8.61
    ## 40            26.65          0.945           6.55           5.90
    ## 41            32.04          0.316           8.43           9.00
    ## 42            27.59          0.705           7.54           8.32
    ## 43            39.83          0.318          16.60          16.47
    ## 44            32.36          0.872          8.28b           9.21
    ## 45            43.84          0.885          14.40          14.44
    ## 46            31.90          0.052           8.57           9.43
    ## 47            27.84          0.017          6.17a           7.76
    ## 48            26.95          0.348           7.62           8.41
    ## 49            25.21          0.967          6.40b           7.07
    ## 50            21.44          0.386           6.85           6.25
    ## 51            48.30          0.332          15.57          16.00
    ## 52            30.07          0.159          9.83a           8.46
    ## 53            35.03          0.404          16.23          14.90
    ## 54            30.27          0.293           8.27           8.09
    ## 55            32.24          0.984           8.22           8.47
    ## 56            27.41          0.957           7.67           7.77
    ##    26+(P Value) 18+(2013-2014) 18+(2014-2015) 18+(P Value)
    ## 1         0.000         12.87a          13.41        0.001
    ## 2         0.016         13.87a          14.77        0.003
    ## 3         0.044         12.39b          12.80        0.070
    ## 4         0.011         11.16a          11.62        0.022
    ## 5         0.052          15.24          15.75        0.143
    ## 6         0.615           9.99           9.59        0.445
    ## 7         0.071         19.86a          22.31        0.014
    ## 8         0.583          13.53          13.08        0.514
    ## 9         0.641          11.22          11.54        0.588
    ## 10        0.017         14.44b          15.36        0.066
    ## 11        0.012         20.74a          23.57        0.009
    ## 12        0.050         13.83a          15.68        0.009
    ## 13        0.412          13.87          13.06        0.216
    ## 14        0.070         21.83a          23.90        0.029
    ## 15        0.021         11.67b          12.51        0.061
    ## 16        0.255          11.82          12.79        0.151
    ## 17        0.770          12.46          12.62        0.828
    ## 18        0.856          11.27          11.17        0.869
    ## 19        0.486          12.17          12.56        0.393
    ## 20        0.032          12.90          13.89        0.125
    ## 21        0.336           9.71           9.01        0.156
    ## 22        0.024         10.93a          12.37        0.014
    ## 23        0.031         10.99a          12.34        0.024
    ## 24        0.670          11.19          11.35        0.773
    ## 25        0.841          19.76          19.88        0.907
    ## 26        0.016         13.33a          15.20        0.006
    ## 27        0.138          17.38          18.47        0.176
    ## 28        0.719          15.65          15.21        0.408
    ## 29        0.738          12.28          12.82        0.397
    ## 30        0.798           9.27           8.57        0.160
    ## 31        0.133          12.75          13.59        0.197
    ## 32        0.292          14.13          15.45        0.109
    ## 33        0.495          10.23          10.81        0.309
    ## 34        0.810          12.86          12.89        0.964
    ## 35        0.274          16.85          17.44        0.469
    ## 36        0.501          11.08          11.81        0.228
    ## 37        0.356          15.57          14.67        0.300
    ## 38        0.309         14.26b          15.21        0.063
    ## 39        0.393          12.01          11.82        0.750
    ## 40        0.210          10.25           9.72        0.301
    ## 41        0.238          11.59          12.21        0.161
    ## 42        0.277          10.67          11.23        0.365
    ## 43        0.908          19.50          19.59        0.919
    ## 44        0.054         11.63b          12.40        0.082
    ## 45        0.966          19.13          19.04        0.917
    ## 46        0.182         11.45a          12.61        0.044
    ## 47        0.019          8.87a          10.76        0.001
    ## 48        0.221          10.16          11.03        0.130
    ## 49        0.079           9.37           9.90        0.126
    ## 50        0.278           9.81           9.09        0.153
    ## 51        0.694          20.11          20.80        0.481
    ## 52        0.030         13.08a          11.55        0.007
    ## 53        0.175          19.06          17.68        0.118
    ## 54        0.796          10.93          10.97        0.951
    ## 55        0.695          11.64          11.85        0.717
    ## 56        0.884          10.58          10.62        0.946

``` r
# Table structure ALMOST works, but the first element is this note. The footnote is being extracted as a single data element. Need to take this piece out by slide function

table_marj =
  table_list [[1]] %>% 
  html_table() %>% 
  slice(-1) %>% 
  as_tibble()
```

## Learning Assessment

## CSS Selectors

Getting HP data

``` r
hpsaga_html = 
  read_html("https://www.imdb.com/list/ls000630791/")
```

Need to get HP data nodes:

``` r
hp_movie_name = 
  hpsaga_html %>% 
  html_nodes(".lister-item-header a") %>% 
  html_text

hp_runtime = 
  hpsaga_html %>% 
  html_nodes(".runtime") %>% 
  html_text
# Get "lister" from the Selector Gadget!! It knows!!!
```

## Learning Assessment: Getting Napolean Dynamite Review Data

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
  html_text()

review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text()

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
# Tibble creates a DF.
```

## Using an API w/ Water Consumption in NYC

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.csv") %>% 
  content("parsed")
```

    ## Parsed with column specification:
    ## cols(
    ##   year = col_double(),
    ##   new_york_city_population = col_double(),
    ##   nyc_consumption_million_gallons_per_day = col_double(),
    ##   per_capita_gallons_per_person_per_day = col_double()
    ## )

``` r
# Get = helps us get a URL. Can be JSON or CSV (CSV might be easier?)
# Content = Extract content from a request
```

More examples for API Extraction

``` r
poke =
  GET("http://pokeapi.co/api/v2/pokemon/1") %>%
  content()
```

# Lecture: Strings vs. Factors

Both look like character vectors, but strings are just like strings.
Factors have an underlying numeric structure. Stringr package is what we
will be using\!

Regular expressions = naming things in r, what you have named, you might
need to search for them.

Factors = few categorical variables siting on top of each other. Forcats
package. -when you use linear models, need reference categories - have
to be in control of factor variables -for categorical variables and
anagram for factors

\#\#Strings and Regex

``` r
library(p8105.datasets)

string_vec = c("my", "name", "is", "jeff")

str_detect(string_vec, "jeff")
```

    ## [1] FALSE FALSE FALSE  TRUE

``` r
#str prefix: every function that we want to use in stringr package is this. We can scroll through str to see what exists. str_detect detects whether "jeff" is in each of the strings. It doesn't exist in the first 3, but it does in the last.
```

str\_replace

``` r
str_replace(string_vec, "jeff", "Jeff")
```

    ## [1] "my"   "name" "is"   "Jeff"

New string vector

``` r
string_vec = c(
  "i think we all rule for participating",
  "i think i have been caught",
  "i think this will be quite fun actually",
  "it will be fun, i think"
  )
str_detect(string_vec, "^i think")
```

    ## [1]  TRUE  TRUE  TRUE FALSE

``` r
# ^ is start of a string variable

str_detect(string_vec, "^i think$")
```

    ## [1] FALSE FALSE FALSE FALSE

``` r
# $ends with a string variable
```

New string vector

``` r
string_vec = c(
  "Y'all remember Pres. HW Bush?",
  "I saw a green bush",
  "BBQ and Bushwalking at Molonglo Gorge",
  "BUSH -- LIVE IN CONCERT!!"
  )
str_detect(string_vec, "[Bb]ush")
```

    ## [1]  TRUE  TRUE  TRUE FALSE

``` r
# detecting any match of "bush". Either start with a capital B or lowercase b. Doesn't detect all caps!
```

New string vector

``` r
string_vec = c(
  '7th inning stretch',
  '1st half soon to begin. Texas won the toss.',
  'she is 5 feet 4 inches tall',
  '3AM - cant sleep :('
  )
str_detect(string_vec, "[0-9][a-zA-Z]")
```

    ## [1]  TRUE  TRUE FALSE  TRUE

``` r
# any number from 0-9, followed by any letter lower case a-z to upper case a-z
```

New string vector

``` r
string_vec = c(
  'Its 7:11 in the evening',
  'want to go to 7-11?',
  'my flight is AA711',
  'NetBios: scanning ip 203.167.114.66'
  )
str_detect(string_vec, "7.11")
```

    ## [1]  TRUE  TRUE FALSE  TRUE

``` r
# finds 7 number followed by 11. . matches any character at all. AA711 doesn't count bc no character in between 7 and
```
