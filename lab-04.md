Lab 04 - La Quinta is Spanish for next to Denny’s, Pt. 1
================
Lindsey Wilson
2/5/23

### Load packages and data

``` r
library(tidyverse) 
library(dsbox) 
```

``` r
states <- read.csv("data/states.csv")
dennys <- dennys
laquinta <- laquinta
```

### Exercise 1

``` r
message("The Denny's dataset contains ", nrow(dennys), " rows and ", ncol(dennys), " columns")
```

    ## The Denny's dataset contains 1643 rows and 6 columns

### Exercise 2

``` r
message("The La Quinta dataset contains ", nrow(laquinta), " rows and ", ncol(laquinta), " columns")
```

    ## The La Quinta dataset contains 909 rows and 6 columns

### Exercise 3

Based on the websites given in the lab, it doesn’t look like there are
any Denny’s locations outside the US. However, it does look like there
are a few La Quinta’s outside the US: two in Canada, six in Mexico, two
in China, one in New Zealand, three in Turkey, one in. the UAB, one in.
Chile, and one in Colombia.

### Exercise 4

I can think of a few ways to figure out from the data whether or not
there are Denny’s/La Quinta locations in the US.

One quick way might be to filter out results that don’t have a zip code
(or something weird like ‘00000’)

Another suggestion is to look up the highest/lowest longitude and
latitude in the continental US and then filter out results that fall
outside of the box defined by those coordinates. You could take a
similar approach for Hawaii and Alaska to cover them as well.

### Exercise 5

The code below calculates the number Denny’s outside the US by filtering
out Denny’s locations with a state code that isn’t in the `states`
dataset and then counting the number of rows left over:

``` r
dennys_outside_US_count <- dennys %>%
  filter(!(state %in% states$abbreviation)) %>%
  nrow()

  message("There are ", dennys_outside_US_count, " Denny's locations outside the US")
```

    ## There are 0 Denny's locations outside the US

### Exercise 6

Since we know there aren’t any Denny’s outside the US, we can go ahead
and add a country variable to the Denny’s dataset and set the value for
all rows as “United States”

``` r
dennys <- dennys %>%
  mutate(country = "United States")
```

### Exercise 7

We can take the same approach to find how many La Quinta locations are
outside the US as well:

``` r
laquinta_outside_US_count <- laquinta %>%
  filter(!(state %in% states$abbreviation)) %>%
  nrow()

  message("There are ", laquinta_outside_US_count, " La Quinta locations outside the US")
```

    ## There are 14 La Quinta locations outside the US

And a little extra code lets us figure out where those locations are

``` r
laquinta_outside_US <- laquinta %>%
   filter(!(state %in% states$abbreviation))

laquinta_outside_US
```

    ## # A tibble: 14 × 6
    ##    address                                     city  state zip   longi…¹ latit…²
    ##    <chr>                                       <chr> <chr> <chr>   <dbl>   <dbl>
    ##  1 Carretera Panamericana Sur KM 12            "\nA… AG    20345  -102.    21.8 
    ##  2 Av. Tulum Mza. 14 S.M. 4 Lote 2             "\nC… QR    77500   -86.8   21.2 
    ##  3 Ejercito Nacional 8211                      "Col… CH    32528  -106.    31.7 
    ##  4 Blvd. Aeropuerto 4001                       "Par… NL    66600  -100.    25.8 
    ##  5 Carrera 38 # 26-13 Avenida las Palmas con … "\nM… ANT   0500…   -75.6    6.22
    ##  6 AV. PINO SUAREZ No. 1001                    "Col… NL    64000  -100.    25.7 
    ##  7 Av. Fidel Velazquez #3000 Col. Central      "\nM… NL    64190  -100.    25.7 
    ##  8 63 King Street East                         "\nO… ON    L1H1…   -78.9   43.9 
    ##  9 Calle Las Torres-1 Colonia Reforma          "\nP… VE    93210   -97.4   20.6 
    ## 10 Blvd. Audi N. 3 Ciudad Modelo               "\nS… PU    75010   -97.8   19.2 
    ## 11 Ave. Zeta del Cochero No 407                "Col… PU    72810   -98.2   19.0 
    ## 12 Av. Benito Juarez 1230 B (Carretera 57) Co… "\nS… SL    78399  -101.    22.1 
    ## 13 Blvd. Fuerza Armadas                        "con… FM    11101   -87.2   14.1 
    ## 14 8640 Alexandra Rd                           "\nR… BC    V6X1…  -123.    49.2 
    ## # … with abbreviated variable names ¹​longitude, ²​latitude

### Exercise 8

After some Googling, I’m able to figure out which countries these
locations are in based on their country codes: Codes AG, QR, CH, NL, VE,
PU, and SL are in Mexico Code ANT is in Colombia Codes ON and BC are in
Canada Code FM in is Honduras

We can use this knowledge to add a country code to the La Quinta data as
well:

``` r
laquinta <- laquinta %>%
  mutate(country = case_when(
    state %in% state.abb     ~ "United States",
    state %in% c("AG", "QR", "CH",  "NL", "VE", "PU", "SL")  ~"Mexico",
    state %in% c("ON", "BC") ~ "Canada",
    state == "ANT"           ~ "Colombia",
    state == "FM"            ~ "Honduras" 
    ))

laquinta <- laquinta %>%
  filter(country == "United States")
```
