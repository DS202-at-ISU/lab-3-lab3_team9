
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
library(readr)
# Extract the numeric part from the column names for deaths
death_columns <- colnames(av)[grepl("^Death\\d+$", colnames(av))]
time_values <- parse_number(death_columns)

# Create the deaths dataset
deaths <- data.frame(Time = time_values, Death = character(length(time_values)), stringsAsFactors = FALSE)

# Populate the Death column based on the original dataset
for (i in 1:nrow(deaths)) {
  deaths$Death[i] <- ifelse(av[i, death_columns[i]] == "YES", "yes", "no")
}
```

Similarly, deal with the returns of characters.

``` r
# Extract the numeric part from the column names for returns
return_columns <- colnames(av)[grepl("^Return\\d+$", colnames(av))]
time_values_return <- parse_number(return_columns)

# Create the returns dataset
returns <- data.frame(Time = time_values_return, Return = character(length(time_values_return)), stringsAsFactors = FALSE)

# Populate the Return column based on the original dataset
for (i in 1:nrow(returns)) {
  returns$Return[i] <- ifelse(av[i, return_columns[i]] == "YES", "yes", "no")
}
```

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
# Calculate the average number of deaths
average_deaths <- mean(deaths$Time)
average_deaths
```

    ## [1] 3

**On average, an Avenger suffers approximately 3 deaths**

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Emily:  

Fact: The MVP of the Earth-616 Marvel Universe Avengers has to be
Jocasta — an android based on Janet van Dyne and built by Ultron — who
has been destroyed five times and then recovered five times.  

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
# Filter the data for Jocasta
jocasta <- av %>%
  filter(Name.Alias == "Jocasta")

# Calculate the number of times Jocasta has been destroyed and recovered
num_deaths <- sum(jocasta$Death1 == "YES", jocasta$Death2 == "YES", jocasta$Death3 == "YES", jocasta$Death4 == "YES", jocasta$Death5 == "YES")
num_returns <- sum(jocasta$Return1 == "YES", jocasta$Return2 == "YES", jocasta$Return3 == "YES", jocasta$Return4 == "YES", jocasta$Return5 == "YES")

num_deaths
```

    ## [1] 5

``` r
num_returns
```

    ## [1] 5

The dataset showed that Jocasta indeed was destroyed 5 times and then
recovered 5 times.  

Ryan:  

Fact: There’s a 2-in-3 chance that a member of the Avengers returned
from their first stint in the afterlife, but only a 50 percent chance
they recovered from a second or third death.  

``` r
# Calculating ratio of those who return from their first death
firstdeath <- sum(av$Death1 == "YES")
firstreturn <- sum(av$Return1 == "YES")
firstreturn / firstdeath
```

    ## [1] 0.6666667

``` r
# Calculating ratio of those who return from second/third death
stdeath <- sum(av$Death2 == "YES",
               av$Death3 == "YES")
streturn <- sum(av$Return2 == "YES",
                av$Return3 == "YES")
streturn / stdeath
```

    ## [1] 0.5

## Individually - Mason

### FiveThirtyEight Statement

> Out of 173 listed Avengers, my analysis found that 69 had died at
> least one time after they joined the team. That’s about 40 percent of
> all people who have ever signed on to the team.

### Include the code

``` r
# Fact-checking the statement about the number of Avengers who died at least once
avengers_with_deaths <- av %>% 
  filter(Death1 == "YES")

number_of_avengers_with_deaths <- nrow(avengers_with_deaths)
number_of_avengers <- nrow(av)

number_of_avengers_with_deaths
```

    ## [1] 69

``` r
number_of_avengers_with_deaths/number_of_avengers *100
```

    ## [1] 39.88439

### Include your answer

The number of avengers with at least 1 death was 69, which is accurate.
Out of the total of 173 the percentage of deaths were 39.88%, but for
this case you can’t have a decimal percentage so it is rounded up to
40%, which makes this statement accurate.
