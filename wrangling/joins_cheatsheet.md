Joins Cheatsheet
================
Aditya Sharma

### Cheatsheet on Joins

Inspiration: [Jenny Bryan's Cheatsheet](http://stat545.com/bit001_dplyr-cheatsheet.html).

If you are initmidated by the name of joins on data frames or tables due to the confusion between many of them, you are not alone! I am compiling a cheatsheet here on a manually created dataset of my choice using Jenny's idea. I liked it so because it is so simple to understand once you understand it using the right amount of data. We want both the data frames to be as small as possible, while still retaining the expository value.

For our cheatsheet, we are going to work with `bands` data and `labels` (record label information) data. Let us create both the required tibbles.

``` r
bands <- tribble(
    ~name,                     ~origin,            ~genre,           ~record_label,
    "Linkin Park",             "USA",              "Alt. Rock",      "Warner Bros.",
    "Disturbed",               "USA",              "Heavy Metal",    "Warner Bros.",
    "Deftones",                "USA",              "Alt. Metal",     "Warner Bros.",
    "Betraying the Martyrs",   "France",           "Prog. Metal",    "Sumerian Records",
    "Periphery",               "USA",              "Prog. Metal",    "Sumerian Records",
    "Asking Alexandria",       "England",          "Heavy Metal",    "Sumerian Records",
    "Evanescence",             "USA",              "Rock",           "Epic Records"
)


labels <- tribble(
  ~record_label,      ~year_founded,
  "Warner Bros.",     1958,
  "Sumerian Records", 2006,
  "Reprise Records",  1960
)
```

#### inner\_join(bands, labels)

> inner\_join(x, y): Return all rows from x where there are matching values in y, and all columns from x and y. If there are multiple matches between x and y, all combination of the matches are returned. This is a mutating join.

``` r
(ijbl <- inner_join(bands, labels))
```

    ## Joining, by = "record_label"

    ## # A tibble: 6 x 5
    ##   name                  origin  genre       record_label     year_founded
    ##   <chr>                 <chr>   <chr>       <chr>                   <dbl>
    ## 1 Linkin Park           USA     Alt. Rock   Warner Bros.             1958
    ## 2 Disturbed             USA     Heavy Metal Warner Bros.             1958
    ## 3 Deftones              USA     Alt. Metal  Warner Bros.             1958
    ## 4 Betraying the Martyrs France  Prog. Metal Sumerian Records         2006
    ## 5 Periphery             USA     Prog. Metal Sumerian Records         2006
    ## 6 Asking Alexandria     England Heavy Metal Sumerian Records         2006

In this join, we don't obtain the information on `Evanescence` because it has `Epic Records` as the record label and that label doesn't appear anywhere in the `y = labels` table. The join has all the variables from bands plus `year_founded` from labels.

#### semi\_join(bands, labels)

> semi\_join(x, y): Return all rows from x where there are matching values in y, keeping just columns from x. A semi join differs from an inner join because an inner join will return one row of x for each matching row of y, where a semi join will never duplicate rows of x. This is a filtering join.

``` r
(sjbl <- semi_join(bands, labels))
```

    ## Joining, by = "record_label"

    ## # A tibble: 6 x 4
    ##   name                  origin  genre       record_label    
    ##   <chr>                 <chr>   <chr>       <chr>           
    ## 1 Linkin Park           USA     Alt. Rock   Warner Bros.    
    ## 2 Disturbed             USA     Heavy Metal Warner Bros.    
    ## 3 Deftones              USA     Alt. Metal  Warner Bros.    
    ## 4 Betraying the Martyrs France  Prog. Metal Sumerian Records
    ## 5 Periphery             USA     Prog. Metal Sumerian Records
    ## 6 Asking Alexandria     England Heavy Metal Sumerian Records

This join is similar to inner join but it returns the columns that are only present in the first table (here, x = bands).

#### left\_join(bands, labels)

> left\_join(x, y): Return all rows from x, and all columns from x and y. If there are multiple matches between x and y, all combination of the matches are returned. This is a mutating join.

``` r
(left_join(bands, labels))
```

    ## Joining, by = "record_label"

    ## # A tibble: 7 x 5
    ##   name                  origin  genre       record_label     year_founded
    ##   <chr>                 <chr>   <chr>       <chr>                   <dbl>
    ## 1 Linkin Park           USA     Alt. Rock   Warner Bros.             1958
    ## 2 Disturbed             USA     Heavy Metal Warner Bros.             1958
    ## 3 Deftones              USA     Alt. Metal  Warner Bros.             1958
    ## 4 Betraying the Martyrs France  Prog. Metal Sumerian Records         2006
    ## 5 Periphery             USA     Prog. Metal Sumerian Records         2006
    ## 6 Asking Alexandria     England Heavy Metal Sumerian Records         2006
    ## 7 Evanescence           USA     Rock        Epic Records               NA

In this join, all the rows are retained from the first table `x` and the new variable `year_founded` is added from the table `y`. The rows which do not have any matching entry in the colum by which we are joining will have a `NA` in the corresponding cell for the `year_founded` variable.

#### anti\_join(superheroes, publishers)

> anti\_join(x, y): Return all rows from x where there are not matching values in y, keeping just columns from x. This is a filtering join.

``` r
(anti_join(bands, labels))
```

    ## Joining, by = "record_label"

    ## # A tibble: 1 x 4
    ##   name        origin genre record_label
    ##   <chr>       <chr>  <chr> <chr>       
    ## 1 Evanescence USA    Rock  Epic Records

This returns only a row for `Evanescence` as there is no matching entry in labels for it and the columns are only from the first table (`x = bands`).

Now we will go in the other direction and reverse the order of the tables and see their effect on the joins.

#### inner\_join(labels, bands)

> inner\_join(x, y): Return all rows from x where there are matching values in y, and all columns from x and y. If there are multiple matches between x and y, all combination of the matches are returned. This is a mutating join.

``` r
(inner_join(labels, bands))
```

    ## Joining, by = "record_label"

    ## # A tibble: 6 x 5
    ##   record_label     year_founded name                  origin  genre      
    ##   <chr>                   <dbl> <chr>                 <chr>   <chr>      
    ## 1 Warner Bros.             1958 Linkin Park           USA     Alt. Rock  
    ## 2 Warner Bros.             1958 Disturbed             USA     Heavy Metal
    ## 3 Warner Bros.             1958 Deftones              USA     Alt. Metal 
    ## 4 Sumerian Records         2006 Betraying the Martyrs France  Prog. Metal
    ## 5 Sumerian Records         2006 Periphery             USA     Prog. Metal
    ## 6 Sumerian Records         2006 Asking Alexandria     England Heavy Metal

We get the same result as in the opposite direction but the main thing to note here is that this result is from the direction of labels. This means there are multiple matches for a single entry. Although we get the same variable order, we should not rely on this in our analysis.

#### semi\_join(publishers, superheroes)

> semi\_join(x, y): Return all rows from x where there are matching values in y, keeping just columns from x. A semi join differs from an inner join because an inner join will return one row of x for each matching row of y, where a semi join will never duplicate rows of x. This is a filtering join.

``` r
(semi_join(labels, bands))
```

    ## Joining, by = "record_label"

    ## # A tibble: 2 x 2
    ##   record_label     year_founded
    ##   <chr>                   <dbl>
    ## 1 Warner Bros.             1958
    ## 2 Sumerian Records         2006

We obtain only two rows and the effect of switching the `x` and the `y` tables is more clear now. We lose the information on `Epic Records` as there is no observation in `y = bands` where `record_label = Epic Records`.

#### left\_join(publishers, superheroes)

> left\_join(x, y): Return all rows from x, and all columns from x and y. If there are multiple matches between x and y, all combination of the matches are returned. This is a mutating join.

``` r
(left_join(labels, bands))
```

    ## Joining, by = "record_label"

    ## # A tibble: 7 x 5
    ##   record_label     year_founded name                  origin  genre      
    ##   <chr>                   <dbl> <chr>                 <chr>   <chr>      
    ## 1 Warner Bros.             1958 Linkin Park           USA     Alt. Rock  
    ## 2 Warner Bros.             1958 Disturbed             USA     Heavy Metal
    ## 3 Warner Bros.             1958 Deftones              USA     Alt. Metal 
    ## 4 Sumerian Records         2006 Betraying the Martyrs France  Prog. Metal
    ## 5 Sumerian Records         2006 Periphery             USA     Prog. Metal
    ## 6 Sumerian Records         2006 Asking Alexandria     England Heavy Metal
    ## 7 Reprise Records          1960 <NA>                  <NA>    <NA>

We get the result same as the inner join but with an extra row corresponding to the entry which is present in the table `x = labels` but not in `y = bands`. So the corresponding column cells for which we don't have the values are filled with `NA`.

#### anti\_join(publishers, superheroes)

> anti\_join(x, y): Return all rows from x where there are not matching values in y, keeping just columns from x. This is a filtering join.

``` r
(anti_join(labels, bands))
```

    ## Joining, by = "record_label"

    ## # A tibble: 1 x 2
    ##   record_label    year_founded
    ##   <chr>                  <dbl>
    ## 1 Reprise Records         1960

As with the other direction, we get only one row for which the `record_label` entry is not present in the second table `y = bands`. So we only have `Reprise Records` and columns corresponding to table `x = labels`.

#### full\_join(superheroes, publishers)

> full\_join(x, y): Return all rows and all columns from both x and y. Where there are not matching values, returns NA for the one missing. This is a mutating join.

``` r
(full_join(bands, labels))
```

    ## Joining, by = "record_label"

    ## # A tibble: 8 x 5
    ##   name                  origin  genre       record_label     year_founded
    ##   <chr>                 <chr>   <chr>       <chr>                   <dbl>
    ## 1 Linkin Park           USA     Alt. Rock   Warner Bros.             1958
    ## 2 Disturbed             USA     Heavy Metal Warner Bros.             1958
    ## 3 Deftones              USA     Alt. Metal  Warner Bros.             1958
    ## 4 Betraying the Martyrs France  Prog. Metal Sumerian Records         2006
    ## 5 Periphery             USA     Prog. Metal Sumerian Records         2006
    ## 6 Asking Alexandria     England Heavy Metal Sumerian Records         2006
    ## 7 Evanescence           USA     Rock        Epic Records               NA
    ## 8 <NA>                  <NA>    <NA>        Reprise Records          1960

This is the most general form of join that contains all the information from both the tables. We have all rows of `x = bands` plus a new row from `y = labels`. We have all variables from `x = bands` and all variables from `y = labels`. The corresponding cell entries which are solely derived from one table or the other is filled with `NA`s for the variables found only in the other table.

*Open a github issue or a pull request if you want some additions to this cheatsheet. Let's make it an awesome resource to learn joins!*
