
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
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.2     ✔ tibble    3.3.0
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

“I counted 89 total deaths — some unlucky Avengers (Like Mar-Vell and
Hawkeye) are basically Meat Loaf with an E-ZPass — and on 57 occasions
the individual made a comeback.”

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

``` r
total_deaths <- deaths |> 
  filter(Death == "YES") |> 
  summarise(count_deaths = n())

total_returns <- returns |> 
  filter(Return == "YES") |> 
  summarise(count_returns = n())

total_deaths
```

    ## # A tibble: 1 × 1
    ##   count_deaths
    ##          <int>
    ## 1           89

``` r
total_returns
```

    ## # A tibble: 1 × 1
    ##   count_returns
    ##           <int>
    ## 1            57

Alexander’s Code:

``` r
years = max(av$Years.since.joining)

filter(av, Years.since.joining == 115)
```

    ##                                                     URL      Name.Alias
    ## 1    http://marvel.wikia.com/Elvin_Haliday_(Earth-616)#   Elvin Haliday
    ## 2    http://marvel.wikia.com/William_Baker_(Earth-616)#   William Baker
    ## 3    http://marvel.wikia.com/James_Santini_(Earth-616)#   James Santini
    ## 4     http://marvel.wikia.com/Emery_Schaub_(Earth-616)#    Emery Schaub
    ## 5  http://marvel.wikia.com/Fiona_(Inhuman)_(Earth-616)#           Fiona
    ## 6           http://marvel.wikia.com/Hollow_(Earth-616)#          Yvette
    ## 7      http://marvel.wikia.com/Julie_Power_(Earth-616)#     Julie Power
    ## 8       http://marvel.wikia.com/Alani_Ryan_(Earth-616)#      Alani Ryan
    ## 9  http://marvel.wikia.com/Johnathon_Gallo_(Earth-616)#    Johnny Gallo
    ## 10           http://marvel.wikia.com/Lyra_(Earth-8009)#            Lyra
    ## 11    http://marvel.wikia.com/Anya_Corazon_(Earth-616)#    Anya Corazon
    ## 12 http://marvel.wikia.com/Kevin_Masterson_(Earth-616)# Kevin Masterson
    ## 13 http://marvel.wikia.com/Michiko_Musashi_(Earth-616)# Michiko Musashi
    ## 14 http://marvel.wikia.com/Takashi_Matsuya_(Earth-616)#    Taki Matsuya
    ##    Appearances Current. Gender Probationary.Introl Full.Reserve.Avengers.Intro
    ## 1          158       NO   MALE              Feb-91                            
    ## 2          355       NO   MALE              Feb-91                            
    ## 3           40      YES   MALE                                                
    ## 4           26      YES   MALE                                                
    ## 5            2      YES FEMALE                                                
    ## 6           22      YES FEMALE                                                
    ## 7          153      YES FEMALE                                                
    ## 8           73      YES FEMALE                                                
    ## 9           43      YES   MALE                                                
    ## 10          55      YES FEMALE                                                
    ## 11         108      YES FEMALE                                                
    ## 12          62      YES   MALE                                                
    ## 13          94      YES FEMALE                                                
    ## 14          18      YES   MALE                                                
    ##    Year Years.since.joining     Honorary Death1 Return1 Death2 Return2 Death3
    ## 1  1900                 115 Probationary     NO                              
    ## 2  1900                 115 Probationary    YES     YES     NO               
    ## 3  1900                 115      Academy     NO                              
    ## 4  1900                 115      Academy     NO                              
    ## 5  1900                 115      Academy     NO                              
    ## 6  1900                 115      Academy     NO                              
    ## 7  1900                 115      Academy     NO                              
    ## 8  1900                 115      Academy     NO                              
    ## 9  1900                 115      Academy     NO                              
    ## 10 1900                 115      Academy     NO                              
    ## 11 1900                 115      Academy     NO                              
    ## 12 1900                 115      Academy     NO                              
    ## 13 1900                 115      Academy     NO                              
    ## 14 1900                 115      Academy     NO                              
    ##    Return3 Death4 Return4 Death5 Return5
    ## 1                                       
    ## 2                                       
    ## 3                                       
    ## 4                                       
    ## 5                                       
    ## 6                                       
    ## 7                                       
    ## 8                                       
    ## 9                                       
    ## 10                                      
    ## 11                                      
    ## 12                                      
    ## 13                                      
    ## 14                                      
    ##                                                                                                Notes
    ## 1                                                                                               <NA>
    ## 2  Died in Identity_Disc_Vol_1_1. Later was revealed to be alive and working along with the Vulture.
    ## 3                                                                                               <NA>
    ## 4                                                                                               <NA>
    ## 5                                                                                               <NA>
    ## 6                                                                                               <NA>
    ## 7                                                                                               <NA>
    ## 8                                                                                               <NA>
    ## 9                                                                                               <NA>
    ## 10                                                                                              <NA>
    ## 11                                                                                              <NA>
    ## 12                                                                                              <NA>
    ## 13                                                                                              <NA>
    ## 14                                                                                              <NA>

``` r
filter(av, Years.since.joining < 115)
```

    ##                                                                     URL
    ## 1                         http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2                    http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3                     http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4               http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5                      http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6                     http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ## 7                     http://marvel.wikia.com/Steven_Rogers_(Earth-616)
    ## 8                      http://marvel.wikia.com/Clint_Barton_(Earth-616)
    ## 9                   http://marvel.wikia.com/Pietro_Maximoff_(Earth-616)
    ## 10                   http://marvel.wikia.com/Wanda_Maximoff_(Earth-616)
    ## 11                 http://marvel.wikia.com/Jacques_Duquesne_(Earth-616)
    ## 12                         http://marvel.wikia.com/Hercules_(Earth-616)
    ## 13                       http://marvel.wikia.com/T%27Challa_(Earth-616)
    ## 14                           http://marvel.wikia.com/Vision_(Earth-616)
    ## 15                                 http://marvel.wikia.com/Dane_Whitman
    ## 16                http://marvel.wikia.com/Natalia_Romanova_(Earth-616)#
    ## 17                          http://marvel.wikia.com/Mantis_(Earth-616)#
    ## 18                     http://marvel.wikia.com/Henry_McCoy_(Earth-616)#
    ## 19                 http://marvel.wikia.com/Heather_Douglas_(Earth-616)#
    ## 20                 http://marvel.wikia.com/Patricia_Walker_(Earth-616)#
    ## 21                    http://marvel.wikia.com/Matthew_Hawk_(Earth-616)#
    ## 22                    http://marvel.wikia.com/Robert_Frank_(Earth-616)#
    ## 23                  http://marvel.wikia.com/Simon_Williams_(Earth-616)#
    ## 24                    http://marvel.wikia.com/Yondu_Udonta_(Earth-691)#
    ## 25               http://marvel.wikia.com/Martinex_T%27Naga_(Earth-691)#
    ## 26                      http://marvel.wikia.com/Charlie-27_(Earth-691)#
    ## 27                 http://marvel.wikia.com/Nicholette_Gold_(Earth-691)#
    ## 28                    http://marvel.wikia.com/Stakar_Ogord_(Earth-691)#
    ## 29                     http://marvel.wikia.com/Vance_Astro_(Earth-691)#
    ## 30                        http://marvel.wikia.com/Mar-Vell_(Earth-616)#
    ## 31                   http://marvel.wikia.com/Carol_Danvers_(Earth-616)#
    ## 32                   http://marvel.wikia.com/Samuel_Wilson_(Earth-616)#
    ## 33                         http://marvel.wikia.com/Jocasta_(Earth-616)#
    ## 34                    http://marvel.wikia.com/Greer_Nelson_(Earth-616)#
    ## 35                http://marvel.wikia.com/Jennifer_Walters_(Earth-616)#
    ## 36                  http://marvel.wikia.com/Monica_Rambeau_(Earth-616)#
    ## 37                            http://marvel.wikia.com/Eros_(Earth-616)#
    ## 38                    http://marvel.wikia.com/James_Rhodes_(Earth-616)#
    ## 39                   http://marvel.wikia.com/Barbara_Morse_(Earth-616)#
    ## 40                   http://marvel.wikia.com/Moira_Brandon_(Earth-616)#
    ## 41                  http://marvel.wikia.com/Benjamin_Grimm_(Earth-616)#
    ## 42                   http://marvel.wikia.com/Bonita_Juarez_(Earth-616)#
    ## 43                    http://marvel.wikia.com/Marc_Spector_(Earth-616)#
    ## 44                     http://marvel.wikia.com/John_Walker_(Earth-616)#
    ## 45           http://marvel.wikia.com/Human_Torch_(Android)_(Earth-616)#
    ## 46                   http://marvel.wikia.com/Miguel_Santos_(Earth-616)#
    ## 47                 http://marvel.wikia.com/Julia_Carpenter_(Earth-616)#
    ## 48                    http://marvel.wikia.com/2ZP45-9-X-51_(Earth-616)#
    ## 49                    http://marvel.wikia.com/Chris_Powell_(Earth-616)#
    ## 50                  http://marvel.wikia.com/Namor_McKenzie_(Earth-616)#
    ## 51                      http://marvel.wikia.com/Scott_Lang_(Earth-616)#
    ## 52                   http://marvel.wikia.com/Anthony_Druid_(Earth-616)#
    ## 53               http://marvel.wikia.com/Marrina_Smallwood_(Earth-616)#
    ## 54              http://marvel.wikia.com/Ravonna_Renslayer_(Earth-6311)#
    ## 55                     http://marvel.wikia.com/Rita_DeMara_(Earth-616)#
    ## 56                   http://marvel.wikia.com/Dennis_Dunphy_(Earth-616)#
    ## 57                       http://marvel.wikia.com/Gilgamesh_(Earth-616)#
    ## 58                   http://marvel.wikia.com/Reed_Richards_(Earth-616)#
    ## 59                     http://marvel.wikia.com/Susan_Storm_(Earth-616)#
    ## 60                  http://marvel.wikia.com/Wendell_Vaughn_(Earth-616)#
    ## 61                 http://marvel.wikia.com/Ashley_Crawford_(Earth-616)#
    ## 62                      http://marvel.wikia.com/Dinah_Soar_(Earth-616)#
    ## 63                    http://marvel.wikia.com/DeMarr_Davis_(Earth-616)#
    ## 64                     http://marvel.wikia.com/Val_Ventura_(Earth-616)#
    ## 65                    http://marvel.wikia.com/Craig_Hollis_(Earth-616)#
    ## 66                    http://marvel.wikia.com/Gene_Lorrene_(Earth-616)#
    ## 67                    http://marvel.wikia.com/Doreen_Green_(Earth-616)#
    ## 68                      http://marvel.wikia.com/Monkey_Joe_(Earth-616)#
    ## 69                    http://marvel.wikia.com/Doug_Taggert_(Earth-616)#
    ## 70                       http://marvel.wikia.com/Tippy-Toe_(Earth-616)#
    ## 71                     http://marvel.wikia.com/Wade_Wilson_(Earth-616)#
    ## 72                     http://marvel.wikia.com/Greg_Willis_(Earth-616)#
    ## 73                           http://marvel.wikia.com/Sersi_(Earth-616)#
    ## 74                    http://marvel.wikia.com/Peter_Parker_(Earth-616)#
    ## 75                   http://marvel.wikia.com/Walter_Newell_(Earth-616)#
    ## 76             http://marvel.wikia.com/Crystalia_Amaquelin_(Earth-616)#
    ## 77                  http://marvel.wikia.com/Eric_Masterson_(Earth-616)#
    ## 78                   http://marvel.wikia.com/Philip_Javert_(Earth-921)#
    ## 79                 http://marvel.wikia.com/Melissa_Darrow_(Earth-9201)#
    ## 80                        http://marvel.wikia.com/Deathcry_(Earth-616)#
    ## 81                 http://marvel.wikia.com/Anthony_Stark_(Earth-96020)#
    ## 82       http://marvel.wikia.com/Giuletta_Nefaria_(Masque)_(Earth-616)#
    ## 83                  http://marvel.wikia.com/Vance_Astrovik_(Earth-616)#
    ## 84                  http://marvel.wikia.com/Angelica_Jones_(Earth-616)#
    ## 85              http://marvel.wikia.com/Delroy_Garrett_Jr._(Earth-616)#
    ## 86     http://marvel.wikia.com/Maria_de_Guadalupe_Santiago_(Earth-616)#
    ## 87                   http://marvel.wikia.com/Jonathan_Hart_(Earth-616)#
    ## 88                    http://marvel.wikia.com/Kelsey_Leigh_(Earth-616)#
    ## 89                       http://marvel.wikia.com/Luke_Cage_(Earth-616)#
    ## 90                         http://marvel.wikia.com/Veranke_(Earth-616)#
    ## 91                   http://marvel.wikia.com/James_Howlett_(Earth-616)#
    ## 92                 http://marvel.wikia.com/Robert_Reynolds_(Earth-616)#
    ## 93                      http://marvel.wikia.com/Maya_Lopez_(Earth-616)#
    ## 94  http://marvel.wikia.com/Nathaniel_Richards_(Iron_Lad)_(Earth-6311)#
    ## 95                  http://marvel.wikia.com/Elijah_Bradley_(Earth-616)#
    ## 96                  http://marvel.wikia.com/William_Kaplan_(Earth-616)#
    ## 97                     http://marvel.wikia.com/Dorrek_VIII_(Earth-616)#
    ## 98                  http://marvel.wikia.com/Cassandra_Lang_(Earth-616)#
    ## 99                http://marvel.wikia.com/Katherine_Bishop_(Earth-616)#
    ## 100                 http://marvel.wikia.com/Vision_(Jonas)_(Earth-616)#
    ## 101                http://marvel.wikia.com/Thomas_Shepherd_(Earth-616)#
    ## 102                    http://marvel.wikia.com/Daniel_Rand_(Earth-616)#
    ## 103                http://marvel.wikia.com/Stephen_Strange_(Earth-616)#
    ## 104                           http://marvel.wikia.com/Ares_(Earth-616)#
    ## 105          http://marvel.wikia.com/James_Buchanan_Barnes_(Earth-616)#
    ## 106                   http://marvel.wikia.com/Jessica_Drew_(Earth-616)#
    ## 107                  http://marvel.wikia.com/Jessica_Jones_(Earth-616)#
    ## 108                    http://marvel.wikia.com/Amadeus_Cho_(Earth-616)#
    ## 109                     http://marvel.wikia.com/Maria_Hill_(Earth-616)#
    ## 110                 http://marvel.wikia.com/Robert_Baldwin_(Earth-616)#
    ## 111                  http://marvel.wikia.com/Sharon_Carter_(Earth-616)#
    ## 112                 http://marvel.wikia.com/Eric_O%27Grady_(Earth-616)#
    ## 113                     http://marvel.wikia.com/Brunnhilde_(Earth-616)#
    ## 114                  http://marvel.wikia.com/Richard_Rider_(Earth-616)#
    ## 115                 http://marvel.wikia.com/Brian_Braddock_(Earth-616)#
    ## 116                   http://marvel.wikia.com/Dennis_Sykes_(Earth-616)#
    ## 117                       http://marvel.wikia.com/Noh-Varr_(Earth-616)#
    ## 118                  http://marvel.wikia.com/Thaddeus_Ross_(Earth-616)#
    ## 119                      http://marvel.wikia.com/John_Aman_(Earth-616)#
    ## 120                      http://marvel.wikia.com/Shang-Chi_(Earth-616)#
    ## 121                http://marvel.wikia.com/Jeanne_Foucault_(Earth-616)#
    ## 122                http://marvel.wikia.com/Jennifer_Takeda_(Earth-616)#
    ## 123                       http://marvel.wikia.com/Ken_Mack_(Earth-616)#
    ## 124                 http://marvel.wikia.com/Humberto_Lopez_(Earth-616)#
    ## 125                 http://marvel.wikia.com/Brandon_Sharpe_(Earth-616)#
    ## 126                  http://marvel.wikia.com/Daisy_Johnson_(Earth-616)#
    ## 127                   http://marvel.wikia.com/Ororo_Munroe_(Earth-616)#
    ## 128                http://marvel.wikia.com/Matthew_Murdock_(Earth-616)#
    ## 129                http://marvel.wikia.com/Eugene_Thompson_(Earth-616)#
    ## 130                  http://marvel.wikia.com/Otto_Octavius_(Earth-616)#
    ## 131              http://marvel.wikia.com/Alexander_Summers_(Earth-616)#
    ## 132                 http://marvel.wikia.com/Samuel_Guthrie_(Earth-616)#
    ## 133               http://marvel.wikia.com/Roberto_da_Costa_(Earth-616)#
    ## 134                      http://marvel.wikia.com/Eden_Fesi_(Earth-616)#
    ## 135               http://marvel.wikia.com/Captain_Universe_(Earth-616)#
    ## 136                    http://marvel.wikia.com/Isabel_Kane_(Earth-616)#
    ## 137                http://marvel.wikia.com/Marcus_Milton_(Earth-13034)#
    ## 138             http://marvel.wikia.com/Rogue_(Anna_Marie)_(Earth-616)#
    ## 139                  http://marvel.wikia.com/Shiro_Yoshida_(Earth-616)#
    ## 140                      http://marvel.wikia.com/Ex_Nihilo_(Earth-616)#
    ## 141          http://marvel.wikia.com/Abyss_(Ex_Nihilo%27s)_(Earth-616)#
    ## 142                      http://marvel.wikia.com/Nightmask_(Earth-616)#
    ## 143                   http://marvel.wikia.com/Kevin_Connor_(Earth-616)#
    ## 144                  http://marvel.wikia.com/Sam_Alexander_(Earth-616)#
    ## 145                 http://marvel.wikia.com/America_Chavez_(Earth-616)#
    ## 146          http://marvel.wikia.com/Loki_Laufeyson_(Ikol)_(Earth-616)#
    ## 147                  http://marvel.wikia.com/David_Alleyne_(Earth-616)#
    ## 148             http://marvel.wikia.com/Nicholas_Fury,_Jr._(Earth-616)#
    ## 149                http://marvel.wikia.com/Phillip_Coulson_(Earth-616)#
    ## 150                   http://marvel.wikia.com/Tony_Masters_(Earth-616)#
    ## 151                  http://marvel.wikia.com/Victor_Mancha_(Earth-616)#
    ## 152                   http://marvel.wikia.com/Monica_Chang_(Earth-616)#
    ## 153              http://marvel.wikia.com/Doombot_(Avenger)_(Earth-616)#
    ## 154                         http://marvel.wikia.com/Alexis_(Earth-616)#
    ## 155                    http://marvel.wikia.com/Eric_Brooks_(Earth-616)#
    ## 156                  http://marvel.wikia.com/Adam_Brashear_(Earth-616)#
    ## 157                 http://marvel.wikia.com/Victor_Alvarez_(Earth-616)#
    ## 158                      http://marvel.wikia.com/Ava_Ayala_(Earth-616)#
    ## 159                          http://marvel.wikia.com/Kaluu_(Earth-616)#
    ##                              Name.Alias Appearances Current. Gender
    ## 1             Henry Jonathan "Hank" Pym        1269      YES   MALE
    ## 2                        Janet van Dyne        1165      YES FEMALE
    ## 3           Anthony Edward "Tony" Stark        3068      YES   MALE
    ## 4                   Robert Bruce Banner        2089      YES   MALE
    ## 5                          Thor Odinson        2402      YES   MALE
    ## 6                Richard Milhouse Jones         612      YES   MALE
    ## 7                         Steven Rogers        3458      YES   MALE
    ## 8                Clinton Francis Barton        1456      YES   MALE
    ## 9                       Pietro Maximoff         769      YES   MALE
    ## 10                       Wanda Maximoff        1214      YES FEMALE
    ## 11                     Jacques Duquesne         115       NO   MALE
    ## 12                             Heracles         741      YES   MALE
    ## 13                             T'Challa         780       NO   MALE
    ## 14                 Victor Shade (alias)        1036      YES   MALE
    ## 15                         Dane Whitman         482       NO   MALE
    ## 16           Natalia Alianovna Romanova        1112      YES FEMALE
    ## 17                               Brandt         160       NO FEMALE
    ## 18                       Henry P. McCoy        1886       NO   MALE
    ## 19                      Heather Douglas         332       NO FEMALE
    ## 20                         Patsy Walker         557       NO FEMALE
    ## 21       Matthew Liebowitz (birth name)         197       NO   MALE
    ## 22                  Robert L. Frank Sr.         106       NO   MALE
    ## 23                       Simon Williams         692      YES   MALE
    ## 24                         Yondu Udonta         109       NO   MALE
    ## 25                      Martinex T'Naga         100       NO   MALE
    ## 26                           Charlie-27         132       NO   MALE
    ## 27                      Nicholette Gold         108       NO FEMALE
    ## 28                               Stakar         100       NO   MALE
    ## 29                       Vance Astrovik         156       NO   MALE
    ## 30                             Mar-Vell         254       NO   MALE
    ## 31             Carol Susan Jane Danvers         935      YES FEMALE
    ## 32                 Samuel Thomas Wilson         576      YES   MALE
    ## 33                              Jocasta         141      YES FEMALE
    ## 34                   Greer Grant Nelson         355      YES FEMALE
    ## 35                     Jennifer Walters         933      YES FEMALE
    ## 36                       Monica Rambeau         348      YES FEMALE
    ## 37                                 Eros         206       NO   MALE
    ## 38                      James R. Rhodes         533       NO   MALE
    ## 39           Barbara Barton (nee Morse)         374       NO FEMALE
    ## 40                        Moira Brandon           2       NO FEMALE
    ## 41                 Benjamin Jacob Grimm        2305       NO   MALE
    ## 42                        Bonita Juarez          83       NO FEMALE
    ## 43                         Marc Spector         402       NO   MALE
    ## 44                       John F. Walker         352       NO   MALE
    ## 45                  Jim Hammond (alias)         565       NO   MALE
    ## 46                        Miguel Santos         112       NO   MALE
    ## 47                      Julia Carpenter         218       NO FEMALE
    ## 48                                 X-51         149       NO   MALE
    ## 49                   Christopher Powell         168       NO   MALE
    ## 50                       Namor McKenzie        1561       NO   MALE
    ## 51             Scott Edward Harris Lang         217       NO   MALE
    ## 52                Anthony Ludgate Druid         158       NO   MALE
    ## 53                    Marrina Smallwood          86       NO FEMALE
    ## 54              Ravonna Lexus Renslayer          41       NO FEMALE
    ## 55                          Rita DeMara          68       NO FEMALE
    ## 56                        Dennis Dunphy          70       NO   MALE
    ## 57                                               61       NO   MALE
    ## 58                        Reed Richards        2125      YES   MALE
    ## 59           Susan Richards (nee Storm)        1761       NO   MALE
    ## 60                 Wendell Elvis Vaughn         293       NO   MALE
    ## 61                      Ashley Crawford          36      YES FEMALE
    ## 62                                               22       NO FEMALE
    ## 63                         DeMarr Davis          31      YES   MALE
    ## 64                          Val Ventura          34      YES   MALE
    ## 65                         Craig Hollis          33      YES   MALE
    ## 66                         Gene Lorrene           4       NO   MALE
    ## 67                        Dorreen Green          47       NO FEMALE
    ## 68                                                7       NO   MALE
    ## 69                         Doug Taggert           3       NO   MALE
    ## 70                                               16       NO FEMALE
    ## 71                          Wade Wilson         575       NO   MALE
    ## 72                          Greg Willis          58       NO   MALE
    ## 73                                Circe         237       NO FEMALE
    ## 74                Peter Benjamin Parker        4333      YES   MALE
    ## 75                        Walter Newell         126       NO   MALE
    ## 76           Crystal Amaquelin Maximoff         517       NO FEMALE
    ## 77                 Eric Kevin Masterson         202       NO   MALE
    ## 78                       Phillip Javert          31       NO   MALE
    ## 79                                               28       NO FEMALE
    ## 80                                               50       NO FEMALE
    ## 81                 Anthony Edward Stark          27       NO   MALE
    ## 82                  "Giulietta Nefaria"          18       NO FEMALE
    ## 83                       Vance Astrovik         302       NO   MALE
    ## 84                       Angelica Jones         330       NO FEMALE
    ## 85                   Delroy Garrett Jr.         101       NO   MALE
    ## 86          Maria de Guadalupe Santiago          43       NO FEMALE
    ## 87                        Jonathan Hart         126       NO   MALE
    ## 88                   Kelsey Leigh Shorr          24       NO FEMALE
    ## 89                           Carl Lucas         886      YES   MALE
    ## 90                              Veranke         159       NO FEMALE
    ## 91                James "Logan" Howlett        3130      YES   MALE
    ## 92                      Robert Reynolds         241       NO   MALE
    ## 93                           Maya Lopez          67       NO FEMALE
    ## 94                   Nathaniel Richards          23       NO   MALE
    ## 95                       Elijah Bradley         103       NO   MALE
    ## 96               William "Billy" Kaplan         123      YES   MALE
    ## 97  Dorrek VIII/Theodore "Teddy" Altman         110      YES   MALE
    ## 98                          Cassie Lang         160       NO FEMALE
    ## 99              Katherine "Kate" Bishop         132      YES FEMALE
    ## 100                        Alias: Jonas         121       NO   MALE
    ## 101             Thomas "Tommy" Shepherd          59      YES   MALE
    ## 102             Daniel Thomas Rand K'ai         629       NO   MALE
    ## 103      Doctor Stephen Vincent Strange        1324       NO   MALE
    ## 104                                Ares         236       NO   MALE
    ## 105               James Buchanan Barnes         663       NO   MALE
    ## 106                 Jessica Miriam Drew         525      YES FEMALE
    ## 107                       Jessica Jones         205      YES FEMALE
    ## 108                         Amadeus Cho         108       NO   MALE
    ## 109                          Maria Hill         359      YES FEMALE
    ## 110                      Robbie Baldwin         299       NO   MALE
    ## 111                       Sharon Carter         333       NO FEMALE
    ## 112                        Eric O'Grady          88       NO   MALE
    ## 113                          Brunnhilde         369       NO FEMALE
    ## 114                       Richard Rider         380       NO   MALE
    ## 115                      Brian Braddock         545       NO   MALE
    ## 116                        Dennis Sykes           6       NO   MALE
    ## 117                            Noh-Varr         126      YES   MALE
    ## 118                       Thaddeus Ross         417       NO   MALE
    ## 119                           John Aman          31      YES   MALE
    ## 120                           Shang-Chi         310      YES   MALE
    ## 121                     Jeanne Foucault          63      YES FEMALE
    ## 122                     Jennifer Takeda          73      YES FEMALE
    ## 123                            Ken Mack          59       NO   MALE
    ## 124                      Humberto Lopez          66      YES   MALE
    ## 125                      Brandon Sharpe          64      YES   MALE
    ## 126                       Daisy Johnson          81       NO FEMALE
    ## 127                        Ororo Munroe        1598       NO FEMALE
    ## 128                        Matt Murdock        1375       NO   MALE
    ## 129                      Flash Thompson         746      YES   MALE
    ## 130                       Otto Octavius         561       NO   MALE
    ## 131                        Alex Summers         592      YES   MALE
    ## 132                      Samuel Guthrie         679      YES   MALE
    ## 133                    Roberto da Costa         491      YES   MALE
    ## 134                           Eden Fesi          65      YES   MALE
    ## 135                                              55      YES   MALE
    ## 136                           Izzy Kane          44      YES FEMALE
    ## 137                       Marcus Milton          65      YES   MALE
    ## 138                          Anna Marie         877      YES FEMALE
    ## 139                       Shiro Yoshida         176      YES   MALE
    ## 140                                              24      YES   MALE
    ## 141                                              25      YES FEMALE
    ## 142                                Adam          35      YES   MALE
    ## 143                   Kevin Kale Connor          44      YES   MALE
    ## 144                       Sam Alexander          78      YES   MALE
    ## 145                      America Chavez          22      YES FEMALE
    ## 146                      Loki Laufeyson          77       NO   MALE
    ## 147                       David Alleyne         115      YES   MALE
    ## 148  Nicholas Fury, Jr., Marcus Johnson          77      YES   MALE
    ## 149                     Phillip Coulson          69      YES   MALE
    ## 150                        Tony Masters         173       NO   MALE
    ## 151                       Victor Mancha          75      YES   MALE
    ## 152                        Monica Chang          12      YES FEMALE
    ## 153                                              14      YES   MALE
    ## 154                              Alexis          13      YES FEMALE
    ## 155                         Eric Brooks         198      YES   MALE
    ## 156                       Adam Brashear          29      YES   MALE
    ## 157                      Victor Alvarez          45      YES   MALE
    ## 158                           Ava Ayala          49      YES FEMALE
    ## 159                               Kaluu          35      YES   MALE
    ##     Probationary.Introl Full.Reserve.Avengers.Intro Year Years.since.joining
    ## 1                                            Sep-63 1963                  52
    ## 2                                            Sep-63 1963                  52
    ## 3                                            Sep-63 1963                  52
    ## 4                                            Sep-63 1963                  52
    ## 5                                            Sep-63 1963                  52
    ## 6                                            Sep-63 1963                  52
    ## 7                                            Mar-64 1964                  51
    ## 8                                            May-65 1965                  50
    ## 9                                            May-65 1965                  50
    ## 10                                           May-65 1965                  50
    ## 11                                           Sep-65 1965                  50
    ## 12                                           Oct-67 1967                  48
    ## 13                                           May-68 1968                  47
    ## 14                                           Nov-68 1968                  47
    ## 15                                           Dec-69 1969                  46
    ## 16                                           May-73 1973                  42
    ## 17                                           Aug-73 1973                  42
    ## 18               Jul-75                      Sep-76 1976                  39
    ## 19               Jul-75                      Sep-76 1976                  39
    ## 20               Nov-75                      Sep-76 1976                  39
    ## 21                                           Aug-75 1975                  40
    ## 22               Apr-77                      Jul-78 1978                  37
    ## 23               Apr-77                      Mar-79 1979                  36
    ## 24                                           Feb-78 1978                  37
    ## 25                                           Feb-78 1978                  37
    ## 26                                           Feb-78 1978                  37
    ## 27                                           Feb-78 1978                  37
    ## 28                                           Feb-78 1978                  37
    ## 29                                           Feb-78 1978                  37
    ## 30                                           Jul-78 1978                  37
    ## 31                                           Apr-79 1979                  36
    ## 32                                           Jun-79 1979                  36
    ## 33               Jul-80                      Nov-88 1988                  27
    ## 34                                           Sep-81 1981                  34
    ## 35                                           Jul-82 1982                  33
    ## 36               Jan-83                      May-83 1983                  32
    ## 37               Jun-83                      May-84 1984                  31
    ## 38               May-84                      Sep-84 1984                  31
    ## 39               Jun-84                      Sep-84 1984                  31
    ## 40                                           Nov-93 1993                  22
    ## 41                                           Jun-86 1986                  29
    ## 42                                           Sep-87 1987                  28
    ## 43               Sep-87                      Jun-88 1988                  27
    ## 44                                           May-89 1989                  26
    ## 45                                           Nov-89 1989                  26
    ## 46                                           Apr-91 1991                  24
    ## 47                                           Sep-92 1992                  23
    ## 48                                           Jun-92 1992                  23
    ## 49                                           May-93 1993                  22
    ## 50                                           Dec-85 1985                  30
    ## 51               Jan-87                       3-Feb 2003                  12
    ## 52                                           Apr-87 1987                  28
    ## 53                                           Dec-87 1987                  28
    ## 54                                           May-88 1988                  27
    ## 55                                           Nov-88 1988                  27
    ## 56                                           Jan-88 1988                  27
    ## 57                                           Feb-89 1989                  26
    ## 58                                           Feb-89 1989                  26
    ## 59                                           Feb-89 1989                  26
    ## 60                                           Oct-89 1989                  26
    ## 61                                           Jul-89 1989                  26
    ## 62                                           Jul-89 1989                  26
    ## 63                                           Jul-89 1989                  26
    ## 64                                           Jul-89 1989                  26
    ## 65                                           Jul-89 1989                  26
    ## 66                                            5-Apr 2005                  10
    ## 67                                            5-May 2005                  10
    ## 68                                            5-May 2005                  10
    ## 69                                            5-May 2005                  10
    ## 70                                            5-Jul 2005                  10
    ## 71                                            7-Sep 2007                   8
    ## 72                                            9-Aug 2009                   6
    ## 73                                           Feb-90 1990                  25
    ## 74                                           Apr-90 1990                  25
    ## 75                                           Jul-90 1990                  25
    ## 76               Aug-91                      Jan-92 1992                  23
    ## 77                                           Jan-92 1992                  23
    ## 78                                           Dec-92 1992                  23
    ## 79                                           Jun-93 1993                  22
    ## 80                                           Jul-93 1993                  22
    ## 81                                           Feb-96 1996                  19
    ## 82                                           Apr-96 1996                  19
    ## 83                                           May-98 1998                  17
    ## 84                                           May-98 1998                  17
    ## 85                                           Apr-00 2000                  15
    ## 86                                           Jul-00 2000                  15
    ## 87                                            1-Aug 2001                  14
    ## 88                                            4-Jun 2004                  11
    ## 89                                            5-Mar 2005                  10
    ## 90                                            5-Mar 2005                  10
    ## 91                                            5-Jun 2005                  10
    ## 92                                            5-Oct 2005                  10
    ## 93                                            5-Nov 2005                  10
    ## 94                                            5-Apr 2005                  10
    ## 95                                            5-Apr 2005                  10
    ## 96                                            5-Apr 2005                  10
    ## 97                                            5-Apr 2005                  10
    ## 98                                            5-Jun 2005                  10
    ## 99                                            5-Jun 2005                  10
    ## 100                                           6-Feb 2006                   9
    ## 101                                           6-Jun 2006                   9
    ## 102                                           7-Feb 2007                   8
    ## 103                                           7-Feb 2007                   8
    ## 104                                           7-Mar 2007                   8
    ## 105                                           8-Dec 2008                   7
    ## 106                                           8-Dec 2008                   7
    ## 107                                          10-Aug 2010                   5
    ## 108                                           9-Dec 2009                   6
    ## 109                                          10-May 2010                   5
    ## 110                                          10-Jun 2010                   5
    ## 111                                          10-May 2010                   5
    ## 112                                          10-May 2010                   5
    ## 113                                          10-May 2010                   5
    ## 114                                          10-May 2010                   5
    ## 115                                          10-May 2010                   5
    ## 116                                          10-Sep 2010                   5
    ## 117                                          10-Jun 2010                   5
    ## 118                                          11-Jun 2011                   4
    ## 119                                          10-Dec 2010                   5
    ## 120                                          11-Apr 2011                   4
    ## 121                                          10-Aug 2010                   5
    ## 122                                          10-Aug 2010                   5
    ## 123                                          10-Aug 2010                   5
    ## 124                                          10-Aug 2010                   5
    ## 125                                          10-Aug 2010                   5
    ## 126                                          12-Jan 2012                   3
    ## 127                                          12-Jan 2012                   3
    ## 128                                          11-Nov 2011                   4
    ## 129                                          12-Apr 2012                   3
    ## 130                                          13-Jan 2013                   2
    ## 131                                          12-Dec 2012                   3
    ## 132                                          13-Feb 2013                   2
    ## 133                                          13-Feb 2013                   2
    ## 134                                          13-Feb 2013                   2
    ## 135                                          13-Feb 2013                   2
    ## 136                                          13-Feb 2013                   2
    ## 137                                          13-Feb 2013                   2
    ## 138                                          13-Apr 2013                   2
    ## 139                                          13-May 2013                   2
    ## 140                                          13-Oct 2013                   2
    ## 141                                          13-Oct 2013                   2
    ## 142                                          13-Oct 2013                   2
    ## 143                                          13-Oct 2013                   2
    ## 144                                          15-Feb 2015                   0
    ## 145                                          13-Jul 2013                   2
    ## 146                                          13-Jul 2013                   2
    ## 147                                          13-Sep 2013                   2
    ## 148                                          13-Apr 2013                   2
    ## 149                                          13-Apr 2013                   2
    ## 150                                          13-May 2013                   2
    ## 151                                          13-Sep 2013                   2
    ## 152                                          13-Sep 2013                   2
    ## 153                                          13-Sep 2013                   2
    ## 154                                          13-Nov 2013                   2
    ## 155                                          13-Nov 2013                   2
    ## 156                                          14-Jan 2014                   1
    ## 157                                          14-Jan 2014                   1
    ## 158                                          14-Jan 2014                   1
    ## 159                                          15-Jan 2015                   0
    ##     Honorary Death1 Return1 Death2 Return2 Death3 Return3 Death4 Return4 Death5
    ## 1       Full    YES      NO                                                    
    ## 2       Full    YES     YES                                                    
    ## 3       Full    YES     YES                                                    
    ## 4       Full    YES     YES                                                    
    ## 5       Full    YES     YES    YES      NO                                     
    ## 6   Honorary     NO                                                            
    ## 7       Full    YES     YES                                                    
    ## 8       Full    YES     YES    YES     YES                                     
    ## 9       Full    YES     YES                                                    
    ## 10      Full    YES     YES                                                    
    ## 11      Full    YES     YES                                                    
    ## 12      Full     NO                                                            
    ## 13      Full     NO                                                            
    ## 14      Full    YES     YES                                                    
    ## 15      Full     NO                                                            
    ## 16      Full    YES     YES                                                    
    ## 17      Full    YES     YES                                                    
    ## 18      Full     NO                                                            
    ## 19      Full    YES     YES    YES     YES                                     
    ## 20      Full    YES     YES                                                    
    ## 21      Full    YES     YES    YES      NO                                     
    ## 22      Full    YES      NO                                                    
    ## 23      Full    YES     YES                                                    
    ## 24  Honorary     NO                                                            
    ## 25  Honorary     NO                                                            
    ## 26  Honorary     NO                                                            
    ## 27  Honorary     NO                                                            
    ## 28  Honorary     NO                                                            
    ## 29  Honorary     NO                                                            
    ## 30      Full    YES     YES    YES     YES    YES      NO                      
    ## 31      Full     NO                                                            
    ## 32      Full     NO                                                            
    ## 33      Full    YES     YES    YES     YES    YES     YES    YES     YES    YES
    ## 34      Full     NO                                                            
    ## 35      Full    YES     YES                                                    
    ## 36      Full     NO                                                            
    ## 37      Full     NO                                                            
    ## 38      Full     NO                                                            
    ## 39      Full    YES     YES                                                    
    ## 40  Honorary    YES      NO                                                    
    ## 41      Full    YES     YES                                                    
    ## 42      Full     NO                                                            
    ## 43      Full     NO                                                            
    ## 44      Full     NO                                                            
    ## 45      Full    YES     YES                                                    
    ## 46      Full     NO                                                            
    ## 47      Full     NO                                                            
    ## 48      Full    YES     YES                                                    
    ## 49      Full     NO                                                            
    ## 50      Full    YES     YES                                                    
    ## 51      Full    YES     YES                                                    
    ## 52  Honorary    YES     YES    YES     YES                                     
    ## 53      Full    YES     YES    YES     YES                                     
    ## 54      Full    YES     YES    YES      NO                                     
    ## 55  Honorary    YES     YES                                                    
    ## 56      Full    YES     YES    YES      NO                                     
    ## 57      Full    YES     YES                                                    
    ## 58      Full     NO                                                            
    ## 59      Full     NO                                                            
    ## 60      Full    YES     YES                                                    
    ## 61      Full     NO                                                            
    ## 62      Full    YES      NO                                                    
    ## 63      Full    YES     YES                                                    
    ## 64      Full     NO                                                            
    ## 65      Full     NO                                                            
    ## 66      Full     NO                                                            
    ## 67      Full     NO                                                            
    ## 68      Full    YES      NO                                                    
    ## 69      Full    YES      NO                                                    
    ## 70      Full     NO                                                            
    ## 71      Full    YES      NO                                                    
    ## 72      Full     NO                                                            
    ## 73      Full     NO                                                            
    ## 74      Full    YES     YES    YES     YES                                     
    ## 75      Full     NO                                                            
    ## 76      Full     NO                                                            
    ## 77      Full    YES     YES    YES      NO                                     
    ## 78  Honorary     NO                                                            
    ## 79  Honorary     NO                                                            
    ## 80  Honorary    YES     YES    YES      NO                                     
    ## 81      Full    YES      NO                                                    
    ## 82  Honorary    YES      NO                                                    
    ## 83      Full     NO                                                            
    ## 84      Full     NO                                                            
    ## 85      Full     NO                                                            
    ## 86      Full     NO                                                            
    ## 87      Full    YES     YES    YES     YES                                     
    ## 88      Full     NO                                                            
    ## 89      Full     NO                                                            
    ## 90      Full    YES      NO                                                    
    ## 91      Full    YES      NO                                                    
    ## 92      Full    YES     YES                                                    
    ## 93      Full    YES     YES    YES      NO                                     
    ## 94      Full     NO                                                            
    ## 95      Full     NO                                                            
    ## 96      Full     NO                                                            
    ## 97      Full     NO                                                            
    ## 98      Full    YES     YES                                                    
    ## 99      Full     NO                                                            
    ## 100     Full    YES      NO                                                    
    ## 101     Full     NO                                                            
    ## 102     Full     NO                                                            
    ## 103     Full     NO                                                            
    ## 104     Full    YES     YES    YES      NO                                     
    ## 105     Full     NO                                                            
    ## 106     Full     NO                                                            
    ## 107     Full     NO                                                            
    ## 108     Full     NO                                                            
    ## 109     Full     NO                                                            
    ## 110     Full     NO                                                            
    ## 111     Full    YES     YES                                                    
    ## 112     Full    YES      NO                                                    
    ## 113     Full     NO                                                            
    ## 114     Full    YES      NO                                                    
    ## 115     Full     NO                                                            
    ## 116     Full    YES      NO                                                    
    ## 117     Full     NO                                                            
    ## 118     Full    YES     YES                                                    
    ## 119     Full     NO                                                            
    ## 120     Full     NO                                                            
    ## 121  Academy     NO                                                            
    ## 122  Academy     NO                                                            
    ## 123  Academy    YES      NO                                                    
    ## 124  Academy    YES      NO                                                    
    ## 125  Academy     NO                                                            
    ## 126     Full     NO                                                            
    ## 127     Full     NO                                                            
    ## 128     Full     NO                                                            
    ## 129 Honorary     NO                                                            
    ## 130     Full    YES      NO                                                    
    ## 131     Full     NO                                                            
    ## 132     Full     NO                                                            
    ## 133     Full     NO                                                            
    ## 134     Full     NO                                                            
    ## 135     Full     NO                                                            
    ## 136     Full     NO                                                            
    ## 137     Full    YES      NO                                                    
    ## 138     Full     NO                                                            
    ## 139     Full    YES     YES                                                    
    ## 140     Full    YES      NO                                                    
    ## 141     Full    YES      NO                                                    
    ## 142     Full    YES      NO                                                    
    ## 143     Full    YES      NO                                                    
    ## 144 Honorary     NO                                                            
    ## 145     Full     NO                                                            
    ## 146     Full     NO                                                            
    ## 147     Full     NO                                                            
    ## 148     Full     NO                                                            
    ## 149     Full     NO                                                            
    ## 150     Full     NO                                                            
    ## 151     Full    YES     YES                                                    
    ## 152     Full     NO                                                            
    ## 153     Full     NO                                                            
    ## 154     Full     NO                                                            
    ## 155     Full     NO                                                            
    ## 156     Full     NO                                                            
    ## 157     Full     NO                                                            
    ## 158     Full     NO                                                            
    ## 159     Full     NO                                                            
    ##     Return5
    ## 1          
    ## 2          
    ## 3          
    ## 4          
    ## 5          
    ## 6          
    ## 7          
    ## 8          
    ## 9          
    ## 10         
    ## 11         
    ## 12         
    ## 13         
    ## 14         
    ## 15         
    ## 16         
    ## 17         
    ## 18         
    ## 19         
    ## 20         
    ## 21         
    ## 22         
    ## 23         
    ## 24         
    ## 25         
    ## 26         
    ## 27         
    ## 28         
    ## 29         
    ## 30         
    ## 31         
    ## 32         
    ## 33      YES
    ## 34         
    ## 35         
    ## 36         
    ## 37         
    ## 38         
    ## 39         
    ## 40         
    ## 41         
    ## 42         
    ## 43         
    ## 44         
    ## 45         
    ## 46         
    ## 47         
    ## 48         
    ## 49         
    ## 50         
    ## 51         
    ## 52         
    ## 53         
    ## 54         
    ## 55         
    ## 56         
    ## 57         
    ## 58         
    ## 59         
    ## 60         
    ## 61         
    ## 62         
    ## 63         
    ## 64         
    ## 65         
    ## 66         
    ## 67         
    ## 68         
    ## 69         
    ## 70         
    ## 71         
    ## 72         
    ## 73         
    ## 74         
    ## 75         
    ## 76         
    ## 77         
    ## 78         
    ## 79         
    ## 80         
    ## 81         
    ## 82         
    ## 83         
    ## 84         
    ## 85         
    ## 86         
    ## 87         
    ## 88         
    ## 89         
    ## 90         
    ## 91         
    ## 92         
    ## 93         
    ## 94         
    ## 95         
    ## 96         
    ## 97         
    ## 98         
    ## 99         
    ## 100        
    ## 101        
    ## 102        
    ## 103        
    ## 104        
    ## 105        
    ## 106        
    ## 107        
    ## 108        
    ## 109        
    ## 110        
    ## 111        
    ## 112        
    ## 113        
    ## 114        
    ## 115        
    ## 116        
    ## 117        
    ## 118        
    ## 119        
    ## 120        
    ## 121        
    ## 122        
    ## 123        
    ## 124        
    ## 125        
    ## 126        
    ## 127        
    ## 128        
    ## 129        
    ## 130        
    ## 131        
    ## 132        
    ## 133        
    ## 134        
    ## 135        
    ## 136        
    ## 137        
    ## 138        
    ## 139        
    ## 140        
    ## 141        
    ## 142        
    ## 143        
    ## 144        
    ## 145        
    ## 146        
    ## 147        
    ## 148        
    ## 149        
    ## 150        
    ## 151        
    ## 152        
    ## 153        
    ## 154        
    ## 155        
    ## 156        
    ## 157        
    ## 158        
    ## 159        
    ##                                                                                                                                                                                                                                                                  Notes
    ## 1                                                                                                                                                                                                    Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                                                                                                      Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3                                                                                     Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                                                                                                                   Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                                                                                                          Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                                                                                                                 <NA>
    ## 7                                                                                                                                                                                                                     Dies at the end of Civil War. Later comes back. 
    ## 8                                                                                                           Dies in exploding Kree ship in Averngers Vol. 1  Issue 502. Brought back by Scarlet Witch. Dies again in House of M Vol 1 Issue 7. Is later brought back. 
    ## 9                                                                                                                                                                                                                   Dies in House of M Vol 1 Issue 7. Later comes back
    ## 10                                                                                                                                                                                                                 Dies in Uncanny_Avengers_Vol_1_14. Later comes back
    ## 11                                                                                                                                                                                                          Dies in Avengers_Vol_1_130. Brought back by the Chaos King
    ## 12                                                                                                                                                                                                                                                                <NA>
    ## 13                                                                                                                                                                                                                                                                <NA>
    ## 14                                                                                                                                                                                                                 Dies in Avengers_Vol_1_500. Is eventually rebuilt. 
    ## 15                                                                                                                                                                                                                                                                <NA>
    ## 16                                                                                                                                                                                                    Killed by The Hand. Later revived with The Stone of the Chaste. 
    ## 17                                                                                                                                                          Dies in Silver_Surfer_Vol_3_3. Actually "fragments of her essence were spread out all across the universe"
    ## 18                                                                                                                                                                                                                                                                <NA>
    ## 19                                                                                        Dies in Defenders_Vol_1_152. Later 'Obtained a new body.' Dies in Annihilation:_Conquest_Vol_1_3. Comes back as "As before" also was once named Madame MacEvil because LOL. 
    ## 20                                                                                                                                         Died during time on Defenders. Eventually ressurected by villian Grim Reaper along with Mockingbird to fight the Avengers. 
    ## 21                                                                                                        Died during Return_to_the_Old_West. Resurrected by She Hulk in negotiation with Time Variance Authority. As a time traveller he died in 1938. Did not return
    ## 22                                                                                                                                Died in Vision_and_the_Scarlet_Witch_Vol_1_2. Did not return however he was eventually cloned but the clone was killed immediately. 
    ## 23                                                                                                                                                                                                      Died in Avengers_Vol_1_9. Actually just in a death-like coma. 
    ## 24                                                                                                                                                                                                                                                                <NA>
    ## 25                                                                                                                                                                                                                                                                <NA>
    ## 26                                                                                                                                                                                                                                                                <NA>
    ## 27                                                                                                                                                                                                                                                                <NA>
    ## 28                                                                                                                                                                                                                                                                <NA>
    ## 29                                                                                                                                                                                                                                                                <NA>
    ## 30  The bad penny of the Marvel universe. First killed in Secret invasion. Revived during the Chaos war. Died again during aformentioned chaos war. Resurrected by the Kree Empire using the\xe6M'Kraan Crystal. Third death during Avengers vs. X-Men. Has not return
    ## 31                                                                                                                                                                                                                                                                <NA>
    ## 32                                                                                                                                                                                                                                                                <NA>
    ## 33     From her article: Death1: "Defeated Ultron and reversed the process leaving Jocasta a mindless husk." Return 1: "Ultron later revived Jocasta with a remote link activating the mental residue the Wasp left behind" Death 2: "Sacrificing herself to try to ki
    ## 34                                                                                                                                                                                                                                                                <NA>
    ## 35                                                                                                                                                                      Dies during Red Hulk Saga. Returns when Lyra infiltrates Intelligenica and finds her in statis
    ## 36                                                                                                                                                                                                                                                                <NA>
    ## 37                                                                                                                                                                                                                                                                <NA>
    ## 38                                                                                                                                                                                                                                                                <NA>
    ## 39                                                                                                                      Killed by Mephisto. After Secret Invasion it turns out she was actually being impersonated by a Skrull the whole time and was alive and well. 
    ## 40                                                                                                                                                                                 Died in her second appearance earns honorary Avengers status doing so. Stays dead. 
    ## 41                                                                                                                                               Once killed during a battle with Doctor Doom.' Brought back by the FF when they literally went to Heaven to get him. 
    ## 42                                                                                                                                                                                                                                                                <NA>
    ## 43                                                                                                                                                                                   NA but he he did die and return to get his powers prior to joining the Avengers. 
    ## 44                                                                                                                                                                                                                                                                <NA>
    ## 45                                                                                                    Dies during New Invaders. Was "was revived as a living weapon the Thinker intended to sell." Prior to joining the avengers he had died like 2 times in the 40s. 
    ## 46                                                                                                     NA: Has not died since joing the Avengers but prior to joining he fought the West Coast avengers and seemingly died. Later turned up under control of a villain
    ## 47                                                                                                                                                                                                                                                                <NA>
    ## 48                                                                                                                                                                                             Died in Deathlok Vol 2 #5. JK that was actually just a Life Model Decoy
    ## 49                                                                                                                                                                                                                                                                <NA>
    ## 50                                                                                                                                         Namor was presumed killed in the battle with Atlantean Barbarians. Had actually survived and turned up in the South Pacific
    ## 51                                                                                      Died in Avengers:_Disassembled. Was brought back in the Young Avengers: Children's Crusade arc because time travel but take my word for it it was actually kind of well done. 
    ## 52                                                                Shot with a Breathing gun and corpse dumped in trash. Revived by the Grim Reaper. Helped Avengers defeat the Reaper so his spirit could remain at peace. Later completely revived by the Chaos King.
    ## 53                                                                                Killed by Namor with the Ebony Blade. Returned when The Master put her in a statis tube. Death 2 was when Namor mercy killed her. Returned to life a second time by the Chaos King. 
    ## 54                                                                                                                                                                         Killed in Avengers_Vol_1_24. Revived by the Grandmaster. Died in Avengers:_Forever_Vol_1_3.
    ## 55                                                                                                                                                                                   Killed by Iron Man when he was controlled by Immortus. Revived during Chaos war. 
    ## 56                                                                                                          Died in Captain_America_348. Actually "Dunphy miraculously survived the crash and lived with an Inuit tribe." Second death when killed by Sharon Carter.  
    ## 57                                                                                                                                                                                               Killed by Neut who was working for Immortus. Reborn into a new body. 
    ## 58                                                                                                                                                                                                                                                                <NA>
    ## 59                                                                                                                                                                                                                                                                <NA>
    ## 60                                                                                                                                                                               Killed by the Cosmic Assassin. Actually turned into a being composed of pure energy. 
    ## 61                                                                                                                                                                                                                                                                <NA>
    ## 62                                                                                                                                                                                                              Died in Great_Lakes_Avengers_Vol_1_1. Has not returned
    ## 63                                                                                                                                                             Sacrificed self so that Mr. Immortal could stop the villain Maelstrom. Returned as the Angel of Death. 
    ## 64                                                                                                                                                                                                                                                                <NA>
    ## 65                                                                                    Christ; where to begin. His superpower is that he cannot die when he dies he immediately comes back. The Wikia inventories 24 deaths. These will be excluded from the analysis. 
    ## 66                                                                                                                                                                                                                                                                <NA>
    ## 67                                                                                                                                                                                                                                                                <NA>
    ## 68                                                                                                                                                                                                                                               Killed by Leather Boy
    ## 69                                                                                                                                                                                                                                          Accidently killed by Zaran
    ## 70                                                                                                                                                                                                                                                                <NA>
    ## 71                                                                                                            Died during incursion. It's not ever particulalry clear if he ever joined an Avengers team besides one issue so his inclusion here is specious at best. 
    ## 72                                                                                                                                                                                                         Died on Battleworld and came back prior to joining avengers
    ## 73                                                                                                                                                                                                                                                                <NA>
    ## 74                                                   Since joining the New Avengers: First death Killed by Morlun. Ressurected in a brand new body. Died in Amazing Spider-Man #700. Eventually mainfested in his body that Octavious stole and then took over again. 
    ## 75                                                                                                                                                                                                                                                                <NA>
    ## 76                                                                                                                                                                                                                                                                <NA>
    ## 77                                                                                   Became posessed died to purge self of corruption. Revived as undead Avenger minion of the Grim Reaper. Returned to the spirit world by the Scarlet Witch. Has since stayed dead. 
    ## 78                                                                                                                                                                                                                                                                <NA>
    ## 79                                                                                                                                                                                                                                                                <NA>
    ## 80                                                                                                                                Reduced her to a pile of bones and organsby Vargas. Brought back by Chaos King. Died second time in Chaos_War:_Dead_Avengers_Vol_1_3
    ## 81                                                                                                                                                                                                                                          Merged with 616 Tony Stark
    ## 82                                                                                                                                                                                                     Whitney killed was a clone possibly this incarnation of Masque'
    ## 83                                                                                                                                                                                                                                                                <NA>
    ## 84                                                                                                                                                                                                                                                                <NA>
    ## 85                                                                                                                                                                                                                                                                <NA>
    ## 86                                                                                                                                                                                                                                                                <NA>
    ## 87                                                                    First died in a zero energy explosion in space. Body was apparently reanimated by Scarlet Witch. Abruptly exploded killing Scott Lang as well. Second return in Marvel_Zombies_Supreme_Vol_1_2. 
    ## 88                                                                                                                                                                                                                                                                <NA>
    ## 89                                                                                                                                                                                                                                                                <NA>
    ## 90                                                                                                                                                                                                                       Killed by Norman Osbourne. Has not returned. 
    ## 91                                                                                                                                                                                                            Died in Death_of_Wolverine_Vol_1_4. Has not yet returned
    ## 92                                                                                                                                                 Died during Seige. Brought back to life by the Apocalypse twins as a horseman of death in Uncanny_Avengers_Vol_1_9.
    ## 93                                                                                      First death 'had been murdered in a fight with Elektra.' "Revived by Elektra and the Hand with their dark magic." Died in most recent run of Moon Knight has not yet returned.
    ## 94                                                                                                                                                                                                                    NA is actually Kang but let's just not go there.
    ## 95                                                                                                                                                                                                                                                                <NA>
    ## 96                                                                                                                                                                                                                                                                <NA>
    ## 97                                                                                                                                                                                                                                                                <NA>
    ## 98                                                                                                                                                           Dies in Young Avengers: The Childrens Crusade when she is killed by Doom. Eventually reborn years later. 
    ## 99                                                                                                                                                                                                                                                                <NA>
    ## 100                                                                                                                                                                                                                      Dies in Young Avengers The Children's Crusade
    ## 101                                                                                                                                                                                                                                                               <NA>
    ## 102                                                                                                                                                                                                                                                               <NA>
    ## 103                                                                                                                                                                                                                                                               <NA>
    ## 104                                                                                                                                                                   Ripped in half by the Sentry during Seige. Revived during Chaos King. Then killedby Chaos King. 
    ## 105                                                                                                                                                                                                                                                               <NA>
    ## 106                                                                                                                                                                                                                                                               <NA>
    ## 107                                                                                                                                                                                                                                                               <NA>
    ## 108                                                                                                                                                                                                                                                               <NA>
    ## 109                                                                                                                                                                                                                                                               <NA>
    ## 110                                                                                                                                                                                                                                                               <NA>
    ## 111                                                                                                                                                                       Died in Captain_America_Vol_7_10. It's not clear how she survived but yeah she totally did. 
    ## 112                                                                                                                                                                                                                                 Died in Secret_Avengers_Vol_1_22. 
    ## 113                                                                                                                                                                                                 NA but died/came back during Ragnarok prior to becoming an Avenger
    ## 114                                                                                                                                                                                                                          Died in Guardians_of_the_Galaxy_Vol_3_20.
    ## 115                                                                                                                                                                                                                                                               <NA>
    ## 116                                                                                                                                                                                                        Died in Heroic_Age:_One_Month_to_Live_Vol_1_5 stayed dead. 
    ## 117                                                                                                                                                                                                                                                               <NA>
    ## 118                                                                                                                                                                                              Died in Circle of Four arc. Made a deal with Mephisto and came back. 
    ## 119                                                                                                                                                                                                                                                               <NA>
    ## 120                                                                                                                                                                                                                                                               <NA>
    ## 121                                                                                                                                                                                                                                                               <NA>
    ## 122                                                                                                                                                                                                                                                               <NA>
    ## 123                                                                                                                                                                                                                          Died in Murderworld during Avengers Arena
    ## 124                                                                                                                                                                                                                          Died in Murderworld during Avengers Arena
    ## 125                                                                                                                                                                                                                                                               <NA>
    ## 126                                                                                                                                                                                                                                                               <NA>
    ## 127                                                                                                                                                                                                                                                               <NA>
    ## 128                                                                                                                                                                                                                                                               <NA>
    ## 129                                                                                                                                                                                                                                                               <NA>
    ## 130                                                                                                                                                                                                                             Died in Superior_Spider-Man_Vol_1_30. 
    ## 131                                                                                                                                                                                                                                                               <NA>
    ## 132                                                                                                                                                                                                                                                               <NA>
    ## 133                                                                                                                                                                                                                                                               <NA>
    ## 134                                                                                                                                                                                                                                                               <NA>
    ## 135                                                                                                                                                                                                                                                               <NA>
    ## 136                                                                                                                                                                                                                                                               <NA>
    ## 137                                                                                                                                                                                                                                    Died in New_Avengers_Vol_3_32. 
    ## 138                                                                                                                                                                                                                                                               <NA>
    ## 139                                                                                                                                 Died in Uncanny_Avengers_Vol_1_22. Returned when 'he was able to use his power to transform his body into a state of pure energy '
    ## 140                                                                                                                                                                                                                                      Died in New_Avengers_Vol_3_32
    ## 141                                                                                                                                                                                                                                      Died in New_Avengers_Vol_3_32
    ## 142                                                                                                                                                                                                                                      Died in New_Avengers_Vol_3_32
    ## 143                                                                                                                                                                                                                                      Died in New_Avengers_Vol_3_32
    ## 144                                                                                                                                                                                                                                                               <NA>
    ## 145                                                                                                                                                                                                                                                               <NA>
    ## 146                                                                                                                                                                                                                                                               <NA>
    ## 147                                                                                                                                                                                                                                                               <NA>
    ## 148                                                                                                                                                                                                                                                               <NA>
    ## 149                                                                                                                                                                                                                                                               <NA>
    ## 150                                                                                                                                                                                                                                                               <NA>
    ## 151                                                                                                                                                                                                 Died in Avengers_A.I._Vol_1_4. Returned in Avengers_A.I._Vol_1_9. 
    ## 152                                                                                                                                                                                                                                                               <NA>
    ## 153                                                                                                                                                                                                                                                               <NA>
    ## 154                                                                                                                                                                                                                                                               <NA>
    ## 155                                                                                                                                                                                                                                                               <NA>
    ## 156                                                                                                                                                                                                                                                               <NA>
    ## 157                                                                                                                                                                                                                                                               <NA>
    ## 158                                                                                                                                                                                                                                                               <NA>
    ## 159                                                                                                                                                                                                                                                               <NA>

``` r
dc <- deaths |>
  filter(Death != "")

dc2 <- dc |>
  group_by(URL) |>
  summarise(total_deaths = sum(Death == "YES"))

td <- sum(dc2$total_deaths)

mpd = (12 * years) / td

mpd53 = (12 * 53) / td

mpd53
```

    ## [1] 7.146067

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

I filtered and calculated the total deaths of the Avengers and then
calculated the total returns of the individual. It seems the total
deaths are 89 and there were 57 returns, which means upon analysis, the
quote is correct.

Alexander’s Discussion:
