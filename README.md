
<!-- README.md is generated from README.Rmd. Please edit that file -->

# readrba <img src="man/figures/logo.png" align="right" height="139" />

<!-- badges: start -->

[![Lifecycle:
maturing](https://img.shields.io/badge/lifecycle-maturing-blue.svg)](https://www.tidyverse.org/lifecycle/#maturing)
[![R build
status](https://github.com/MattCowgill/readrba/workflows/R-CMD-check/badge.svg)](https://github.com/MattCowgill/readrba/actions)
[![Travis build
status](https://travis-ci.com/MattCowgill/readrba.svg?branch=master)](https://travis-ci.com/MattCowgill/readrba)
[![Codecov test
coverage](https://codecov.io/gh/MattCowgill/readrba/branch/master/graph/badge.svg)](https://codecov.io/gh/MattCowgill/readrba?branch=master)

<!-- badges: end -->

Get data from the [Reserve Bank of
Australia](https://rba.gov.au/statistics/tables/) in a
[tidy](https://tidyr.tidyverse.org/articles/tidy-data.html)
[tibble](https://tibble.tidyverse.org).

This package is still in active development. Some aspects of its
functionality are incomplete.

## Installation

The package is not yet on CRAN. Install from GitHub:

``` r
remotes::install_github("mattcowgill/readrba")
```

## Examples

### Reading RBA data

There is one main function in {readrba}: `read_rba()`.

Here’s how you fetch the current version of a single RBA statistical
table: table G1, consumer price inflation:

``` r
cpi_table <- readrba::read_rba(table_no = "g1")
```

The object returned by `read_rba()` is a tidy tibble (ie. in ‘long’
format):

``` r
head(cpi_table)
#> # A tibble: 6 x 11
#>   date       series value frequency series_type units source pub_date  
#>   <date>     <chr>  <dbl> <chr>     <chr>       <chr> <chr>  <date>    
#> 1 1922-06-01 Consu…   2.8 Quarterly Original    Inde… ABS /… 2020-10-29
#> 2 1922-09-01 Consu…   2.8 Quarterly Original    Inde… ABS /… 2020-10-29
#> 3 1922-09-01 Quart…  -0.1 Quarterly Original    Per … ABS /… 2020-10-29
#> 4 1922-12-01 Consu…   2.7 Quarterly Original    Inde… ABS /… 2020-10-29
#> 5 1922-12-01 Quart…  -1.1 Quarterly Original    Per … ABS /… 2020-10-29
#> 6 1923-03-01 Consu…   2.7 Quarterly Original    Inde… ABS /… 2020-10-29
#> # … with 3 more variables: series_id <chr>, description <chr>,
#> #   table_title <chr>
```

You can also request multiple tables. They’ll be returned together as
one tidy tibble:

``` r
rba_data <- readrba::read_rba(table_no = c("a1", "g1"))

head(rba_data)
#> # A tibble: 6 x 11
#>   date       series value frequency series_type units source pub_date  
#>   <date>     <chr>  <dbl> <chr>     <chr>       <chr> <chr>  <date>    
#> 1 1994-06-01 Austr… 13680 Weekly    Original    $ mi… RBA    2020-10-23
#> 2 1994-06-01 Capit…   633 Weekly    Original    $ mi… RBA    2020-10-23
#> 3 1994-06-01 Depos…  2605 Weekly    Original    $ mi… RBA    2020-10-23
#> 4 1994-06-01 Excha…  3241 Weekly    Original    $ mi… RBA    2020-10-23
#> 5 1994-06-01 Gold … 19179 Weekly    Original    $ mi… RBA    2020-10-23
#> 6 1994-06-01 Notes… 17231 Weekly    Original    $ mi… RBA    2020-10-23
#> # … with 3 more variables: series_id <chr>, description <chr>,
#> #   table_title <chr>

unique(rba_data$table_title)
#> [1] "A1 Reserve Bank Of Australia - Liabilities And Assets - Summary"
#> [2] "G1 Consumer Price Inflation"
```

You can also retrieve data based on the unique RBA time series
identifier(s). For example, to getch the consumer price index series
only:

``` r
cpi_series <- readrba::read_rba(series_id = "GCPIAG")
head(cpi_series)
#> # A tibble: 6 x 11
#>   date       series value frequency series_type units source pub_date  
#>   <date>     <chr>  <dbl> <chr>     <chr>       <chr> <chr>  <date>    
#> 1 1922-06-01 Consu…   2.8 Quarterly Original    Inde… ABS /… 2020-10-29
#> 2 1922-09-01 Consu…   2.8 Quarterly Original    Inde… ABS /… 2020-10-29
#> 3 1922-12-01 Consu…   2.7 Quarterly Original    Inde… ABS /… 2020-10-29
#> 4 1923-03-01 Consu…   2.7 Quarterly Original    Inde… ABS /… 2020-10-29
#> 5 1923-06-01 Consu…   2.8 Quarterly Original    Inde… ABS /… 2020-10-29
#> 6 1923-09-01 Consu…   2.9 Quarterly Original    Inde… ABS /… 2020-10-29
#> # … with 3 more variables: series_id <chr>, description <chr>,
#> #   table_title <chr>
unique(cpi_series$series_id)
#> [1] "GCPIAG"
```

The convenience function `read_rba_seriesid()` is a wrapper around
`read_rba()`. This means `read_rba_seriesid("GCPIAG")` is equivalent to
`read_rba(series_id = "GCPIAG")`.

By default, `read_rba()` fetches the current version of whatever table
you request. You can specify the historical version of a table, if it’s
available, using the `cur_hist` argument:

``` r

hist_a11 <- readrba::read_rba(table_no = "a1.1", cur_hist = "historical")

head(hist_a11)
#> # A tibble: 6 x 11
#>   date       series value frequency series_type units source pub_date  
#>   <date>     <chr>  <dbl> <chr>     <chr>       <chr> <chr>  <date>    
#> 1 1969-07-31 Austr…   865 Monthly   Original; … $ mi… RBA    2015-06-26
#> 2 1969-07-31 Capit…    60 Monthly   Original; … $ mi… RBA    2015-06-26
#> 3 1969-07-31 Excha…  1144 Monthly   Original; … $ mi… RBA    2015-06-26
#> 4 1969-07-31 Gold …  1160 Monthly   Original; … $ mi… RBA    2015-06-26
#> 5 1969-07-31 Notes…  1104 Monthly   Original; … $ mi… RBA    2015-06-26
#> 6 1969-07-31 Other…   561 Monthly   Original; … $ mi… RBA    2015-06-26
#> # … with 3 more variables: series_id <chr>, description <chr>,
#> #   table_title <chr>
```

### Browsing RBA data

Two functions are provided to help you find the table number or series
ID you need. These are `browse_rba_tables()` and `browse_rba_series()`.
Each returns a tibble with information about the available RBA data.

``` r
readrba::browse_rba_tables()
#> # A tibble: 123 x 5
#>    title                   no    url                  current_or_histo… readable
#>    <chr>                   <chr> <chr>                <chr>             <lgl>   
#>  1 Liabilities and Assets… A1    https://rba.gov.au/… current           TRUE    
#>  2 Liabilities and Assets… A1.1  https://rba.gov.au/… current           TRUE    
#>  3 Monetary Policy Changes A2    https://rba.gov.au/… current           TRUE    
#>  4 Monetary Policy Operat… A3    https://rba.gov.au/… current           TRUE    
#>  5 Holdings of Australian… A3.1  https://rba.gov.au/… current           TRUE    
#>  6 Foreign Exchange Trans… A4    https://rba.gov.au/… current           TRUE    
#>  7 Daily Foreign Exchange… A5    https://rba.gov.au/… current           FALSE   
#>  8 Banknotes on Issue by … A6    https://rba.gov.au/… current           TRUE    
#>  9 Detected Australian Co… A7    https://rba.gov.au/… current           TRUE    
#> 10 Assets of Financial In… B1    https://rba.gov.au/… current           TRUE    
#> # … with 113 more rows
```

``` r
readrba::browse_rba_series()
#> # A tibble: 4,252 x 8
#>    table_no series series_id series_type table_title cur_hist description
#>    <chr>    <chr>  <chr>     <chr>       <chr>       <chr>    <chr>      
#>  1 A1       Austr… ARBAAASTW Original    A1 Reserve… current  Australian…
#>  2 A1       Capit… ARBALCRFW Original    A1 Reserve… current  Capital an…
#>  3 A1       Depos… ARBALDEPW Original    A1 Reserve… current  Deposits (…
#>  4 A1       Excha… ARBALESBW Original    A1 Reserve… current  Exchange s…
#>  5 A1       Gold … ARBAAGFXW Original    A1 Reserve… current  Gold and f…
#>  6 A1       Notes… ARBALNOIW Original    A1 Reserve… current  Notes on i…
#>  7 A1       Other… ARBAAOAW  Original    A1 Reserve… current  Other asse…
#>  8 A1       Other… ARBALOLW  Original    A1 Reserve… current  Other liab…
#>  9 A1       Total… ARBAATAW  Original    A1 Reserve… current  Total RBA …
#> 10 A1       Total… ARBALTLW  Original    A1 Reserve… current  Total RBA …
#> # … with 4,242 more rows, and 1 more variable: frequency <chr>
```

You can specify a search string to filter the tables or series, as in:

``` r
readrba::browse_rba_tables("inflation")
#> # A tibble: 3 x 5
#>   title                 no    url                     current_or_histo… readable
#>   <chr>                 <chr> <chr>                   <chr>             <lgl>   
#> 1 Consumer Price Infla… G1    https://rba.gov.au/sta… current           TRUE    
#> 2 Consumer Price Infla… G2    https://rba.gov.au/sta… current           TRUE    
#> 3 Inflation Expectatio… G3    https://rba.gov.au/sta… current           TRUE
```

## Data availability

The `read_rba()` function is able to import most tables on the
[Statistical Tables](https://rba.gov.au/statistics/tables/) page of the
RBA website. These are the tables that are downloaded when you use
`read_rba(cur_hist = "current")`, the default.

`read_rba()` can also download many of the tables on the [Historical
Data](https://rba.gov.au/statistics/historical-data.html) page of the
RBA website. To get these, specify `cur_hist = "historical"` in
`read_rba()`.

### Historical exchange rate tables

The historical exchange rate tables do not have table numbers on the RBA
website. They can still be downloaded, using the following table
numbers:

| Table title                                                                      | table\_no          |
| :------------------------------------------------------------------------------- | :----------------- |
| Exchange Rates – Daily – 1983 to 1986                                            | ex\_daily\_8386    |
| Exchange Rates – Daily – 1987 to 1990                                            | ex\_daily\_8790    |
| Exchange Rates – Daily – 1991 to 1994                                            | ex\_daily\_9194    |
| Exchange Rates – Daily – 1995 to 1998                                            | ex\_daily\_9598    |
| Exchange Rates – Daily – 1999 to 2002                                            | ex\_daily\_9902    |
| Exchange Rates – Daily – 2003 to 2006                                            | ex\_daily\_0306    |
| Exchange Rates – Daily – 2007 to 2009                                            | ex\_daily\_0709    |
| Exchange Rates – Daily – 2010 to 2013                                            | ex\_daily\_1013    |
| Exchange Rates – Daily – 2014 to 2017                                            | ex\_daily\_1417    |
| Exchange Rates – Daily – 2018 to Current                                         | ex\_daily\_18cur   |
| Exchange Rates – Monthly – January 2010 to latest complete month of current year | ex\_monthly\_10cur |
| Exchange Rates – Monthly – July 1969 to December 2009                            | ex\_monthly\_6909  |

### Non-standard tables

`read_rba()` is currently only able to import RBA statistical tables
that are formatted in a (more or less) standard way. Some are formatted
in a non-standard way, either because they’re distributions rather than
time series, or because they’re particularly old.

Tables that are **not** able to be downloaded are:

| Table title                                                               | table\_no | current\_or\_historical |
| :------------------------------------------------------------------------ | :-------- | :---------------------- |
| Daily Foreign Exchange Market Intervention Transactions                   | A5        | current                 |
| Household Balance Sheets – Distribution                                   | E3        | current                 |
| Household Gearing – Distribution                                          | E4        | current                 |
| Household Financial Assets – Distribution                                 | E5        | current                 |
| Household Non-Financial Assets – Distribution                             | E6        | current                 |
| Household Debt – Distribution                                             | E7        | current                 |
| Open Market Operations – 2012 to 2013                                     | A3        | historical              |
| Open Market Operations – 2009 to 2011                                     | A3        | historical              |
| Open Market Operations – 2003 to 2008                                     | A3        | historical              |
| Individual Banks’ Assets – 1991–1992 to 1997–1998                         | J1        | historical              |
| Individual Banks’ Liabilities – 1991–1992 to 1997–1998                    | J2        | historical              |
| Treasury Note Tenders - 1989–2006                                         | E4        | historical              |
| Treasury Bond Tenders – 1982–2006                                         | E5        | historical              |
| Treasury Bond Tenders – Amount Allotted, by Years to Maturity – 1982–2006 | E5        | historical              |
| Treasury Bond Switch Tenders – 2008                                       | E6        | historical              |
| Treasury Capital Indexed Bonds – 1985–2006                                | E7        | historical              |
| Zero-coupon Interest Rates – Analytical Series – 1992 to 2008             | F17       | historical              |

## Issues and contributions

I welcome any feature requests or bug reports. The best way is to file a
[GitHub issue](https://github.com/MattCowgill/readrba/issues).

I would welcome contributions to the package. Please start by filing an
issue, outlining the bug you intend to fix or functionality you intend
to add or modify.

## Disclaimer

This package is not affiliated with or endorsed by the Reserve Bank of
Australia. All data is provided subject to any conditions and
restrictions set out on the RBA website.
