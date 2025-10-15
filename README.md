
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.1     ✔ stringr   1.5.1
    ## ✔ ggplot2   4.0.0     ✔ tibble    3.3.0
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.1.0     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
deaths <- av |>
  pivot_longer(
    c(Death1, Death2, Death3, Death4, Death5),
    names_to = "Time",
    values_to = "Death"
  ) |>
  mutate(
    Time = parse_number(Time),
    Death = as.factor(Death)
  )

deaths
```

    ## # A tibble: 865 × 18
    ##    URL                Name.Alias Appearances Current. Gender Probationary.Introl
    ##    <chr>              <chr>            <int> <chr>    <chr>  <chr>              
    ##  1 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  2 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  3 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  4 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  5 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  6 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  7 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  8 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  9 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ## 10 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ## # ℹ 855 more rows
    ## # ℹ 12 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Return1 <chr>, Return2 <chr>,
    ## #   Return3 <chr>, Return4 <chr>, Return5 <chr>, Notes <chr>, Time <dbl>,
    ## #   Death <fct>

Similarly, deal with the returns of characters.

``` r
returns <- av |>
  pivot_longer(
    c(Return1, Return2, Return3, Return4, Return5),
    names_to = "Time",
    values_to = "Return"
  ) |>
  mutate(
    Time = parse_number(Time),
    Return = as.factor(Return)
  )

returns
```

    ## # A tibble: 865 × 18
    ##    URL                Name.Alias Appearances Current. Gender Probationary.Introl
    ##    <chr>              <chr>            <int> <chr>    <chr>  <chr>              
    ##  1 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  2 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  3 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  4 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  5 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  6 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  7 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  8 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  9 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ## 10 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ## # ℹ 855 more rows
    ## # ℹ 12 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Death1 <chr>, Death2 <chr>,
    ## #   Death3 <chr>, Death4 <chr>, Death5 <chr>, Notes <chr>, Time <dbl>,
    ## #   Return <fct>

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
deaths_clean <- deaths |> 
  filter(Death != "")

deaths_clean |> 
  group_by(URL) |> 
  summarise(total_deaths = sum(Death == "YES")) |>
  summarise(average_deaths = mean(total_deaths))
```

    ## # A tibble: 1 × 1
    ##   average_deaths
    ##            <dbl>
    ## 1          0.514

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

Grace’s Quote:

“Out of 173 listed Avengers, my analysis found that 69 had died at least
one time after they joined the team. That’s about 40 percent of all
people who have ever signed on to the team.”

Jyotika’s Quote:

Alexander’s Quote:

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

Grace’s Code:

``` r
library(dplyr)

av %>%
  summarise(
    total_avengers = n(),
    total_died = sum(Death1 == "YES", na.rm = TRUE),
    percent_died = (total_died / total_avengers) * 100
  )
```

    ##   total_avengers total_died percent_died
    ## 1            173         69     39.88439

Jyotika’s Code:

Alexander’s Code:

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.

Grace’s Discussion:

After running the code, I found that 69 of 173 Avengers have died at
least once, giving a mortality rate of about 39.9%. This confirms
FiveThirtyEight’s statement that approximately 40 percent of Avengers
have died after joining the team. The numbers in the dataset align
closely with the article’s findings, validating its analysis.

Jyotika’s Discussion:

Alexander’s Discussion:
