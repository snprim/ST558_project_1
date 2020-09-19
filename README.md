Vignette
================

  - [Required Packages](#required-packages)
  - [Functions](#functions)
      - [NHL records API](#nhl-records-api)
      - [NHL stats API](#nhl-stats-api)
      - [A wrapper function for all the functions
        above](#a-wrapper-function-for-all-the-functions-above)
  - [Exploratory Data Analysis](#exploratory-data-analysis)
      - [Retrieve Information](#retrieve-information)
      - [Summaries](#summaries)
      - [Visualize Data](#visualize-data)

This vignette demonstrates how to access APIs to retrieve data. We use
two NHL repositories as examples: [NHL
records](https://gitlab.com/dword4/nhlapi/-/tree/master) and [NHL
stats](https://gitlab.com/dword4/nhlapi/-/blob/master/stats-api.md).

## Required Packages

To be able to access data from APIs, you should install and load the
`httr`, `jsonlite`, and `tidyverse` packages.

``` r
library(httr)
library(jsonlite)
library(tidyverse)
```

## Functions

### NHL records API

``` r
baseurl_records <- "https://records.nhl.com/site/api"
```

The function `getFran` retrieves basic information (id, ﬁrstSeasonId and
lastSeasonId and name of every team in the history of the NHL) about all
teams. No arguments are needed.

``` r
getFran <- function(){
  fullurl <- paste0(baseurl_records, "/", "franchise")
  fran <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(fran$data)
}
getFran() %>% tbl_df()
```

The function `getFranTeamTot` retrieves stats about all teams (Total
stats for every franchise (ex roadTies, roadWins, etc)). No arguments
are needed.

``` r
getFranTeamTot <- function() {
  fullurl <- paste0(baseurl_records, "/", "franchise-team-totals")
  franTeamTot <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(franTeamTot$data)
}
getFranTeamTot() %>% tbl_df()
```

To allow for convenient access of team information in the following
functions, we first construct a subset of data, so users can use team
names or franchise ID (or team names or team ID) to look up information.

``` r
index <- getFranTeamTot() %>% select(c("franchiseId", "teamName", "teamId")) %>% unique()
```

The function `getFranSeaRec` retrieves season records for a specific
franchise, and therefore the argument `franchiseId` or `teamName` is
needed. (`franchiseId` can be found using `getFran` or
`getFranTeamTot`.)

``` r
getFranSeaRec <- function(team) {
  if (is.character(team)) {
    id <- index$franchiseId[index$teamName == team]
  } else if (is.numeric(team)){
    id <- team
  }
  fullurl <- paste0(baseurl_records, "/", "franchise-season-records?cayenneExp=franchiseId=", id)
  franSeaRec <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(franSeaRec$data)
}
getFranSeaRec(20) %>% tbl_df()
getFranSeaRec("Vancouver Canucks") %>% tbl_df()
```

The function `getFranGoaRec` retrieves goalie records for a specific
franchise, and again a `franchiseId` or `teamName` is required.

``` r
getFranGoaRec <- function(team) {
  if (is.character(team)) {
    id <- index$franchiseId[index$teamName == team]
  } else if (is.numeric(team)){
    id <- team
  }
  fullurl <- paste0(baseurl_records, "/", "franchise-goalie-records?cayenneExp=franchiseId=", id)
  franGoaRec <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(franGoaRec$data)
}
getFranGoaRec(20) %>% tbl_df()
getFranGoaRec("Vancouver Canucks") %>% tbl_df()
```

The function `getFranSkaRec` retrieves information about skater records
for a specific franchise, and an argument about `franchiseId` or
`teamName` is required.

``` r
getFranSkaRec <- function(team) {
  if (is.character(team)) {
    id <- index$franchiseId[index$teamName == team]
  } else if (is.numeric(team)){
    id <- team
  }
  fullurl <- paste0(baseurl_records, "/", "franchise-skater-records?cayenneExp=franchiseId=", id)
  franSkaRec <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(franSkaRec$data)
}
getFranSkaRec(20) %>% tbl_df()
getFranSkaRec("Vancouver Canucks") %>% tbl_df()
```

### NHL stats API

For this function, eight modifiers can be chosen, and thus one or more
arguments (`ID`, `expand`, `teamID`, `stats`, and/or `season`) has to be
provided. Below are the eight modifiers:

  - ?expand=team.roster Shows roster of active players for the specified
    team  
  - ?expand=person.names Same as above, but gives less info.  
  - ?expand=team.schedule.next Returns details of the upcoming game for
    a team  
  - ?expand=team.schedule.previous Same as above but for the last game
    played  
  - ?expand=team.stats Returns the teams stats for the season  
  - ?expand=team.roster\&season=20142015 Adding the season identifier
    shows the roster for that season  
  - ?teamId=4,5,29 Can string team id together to get multiple teams  
  - ?stats=statsSingleSeasonPlayoffs Specify which stats to get. Not
    fully sure all of the values

Examples of arguments:

  - `ID = 20`
  - `ID = "Calgary Flames"`  
  - `expand = "person.names"`  
  - `teamId = "4, 5, 29"`  
  - `stats = "statsSingleSeasonPlayoffs"`  
  - ’expand = “team.stats”, season = “20142015”\`

Note: If you would like information for more than one team, enter
`teamId`, but, in this case, no more specific information can be chosen.
If you want specific information, such as roster, enter only one `ID` or
`teamName`.

For more information about these modifiers, see [the
documentation](https://gitlab.com/dword4/nhlapi/-/blob/master/stats-api.md).

``` r
baseurl_stats <- "https://statsapi.web.nhl.com/api/v1/teams"
getStats <- function(ID = "", expand = "", teamID = "", stats = "", season = ""){
  if (ID != ""){
      if (is.character(ID)) {
        ID <- index$teamId[index$teamName == ID]
      }
    baseurl_stats <- paste0("https://statsapi.web.nhl.com/api/v1/teams/", ID)
  }
  if (teamID != ""){
    fullurl <- paste0("https://statsapi.web.nhl.com/api/v1/teams?teamId=", teamID)
  } else if (expand != ""){
    if (expand == "team.roster" || expand == "team.stats"){
       if (season != ""){
         fullurl <- paste0(baseurl_stats, "?expand=", expand, "&season=", season)
       } 
    } else {
    fullurl <- paste0(baseurl_stats, "?expand=", expand)
    } 
  } else if (stats != ""){
    fullurl <- paste0(baseurl_stats, "?stats=", stats)
  }
  stats <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  if (str_detect(expand, "team.roster")){
    stats_new <- stats$teams$roster.roster[[1]]
    stats <- cbind(stats$teams[,c(1,2,10)], stats_new)
  } else if (expand == "team.schedule.next"){
    if (is.null(stats$teams$nextGameSchedule.dates)){
      stop("No information is available")
    } else {
    stats_new <- stats$teams$nextGameSchedule.dates
    stats <- cbind(stats$teams[1,c(1,2,10)], stats_new)
    }
  } else if (expand == "team.schedule.previous"){
    if (is.null(stats$teams$previousGameSchedule.dates)){
      stop("No information is available")
    } else {
    stats_new <- stats$teams$previousGameSchedule.dates
    stats <- cbind(stats$teams[,c(1,2,10)], stats_new)
    }
  } else if (expand == "team.stats"){
    stats_new <- stats$teams$teamStats[[1]]$splits[[1]]
    stats <- cbind(stats$teams[1,c(1,2,11)], stats_new)
  } else {
    stats <- stats$teams
  }
  if (is.null(stats)) {
    stop("No information is available")
  } 
  return(as.data.frame(stats))
}
# getStats(ID = 20, expand = "team.stats")
# getStats(ID = "Calgary Flames", expand = "person.names")
getStats(ID = 14, expand = "team.schedule.next") %>% tbl_df()
getStats(ID = 20, expand = "team.stats", season= "20102011") %>% tbl_df()
```

    ## Warning in data.frame(..., check.names = FALSE): row names were found from a short
    ## variable and have been discarded

``` r
# getStats(ID = 53, expand = "team.roster") 
getStats(teamID = "4,5,29") %>% tbl_df()
# getStats(ID = 54, stats = "statsSingleSeasonPlayoffs") %>% tbl_df()
```

### A wrapper function for all the functions above

Endpoints:

  - franchise (getFran)  
  - team total (getFranTeamTot)  
  - season record (getFranSeaRec)  
  - goalie record (getFranGoaRec)  
  - skater record (getFranSkaRec)  
  - stats (getStats)
      - expand = “team.roster”  
      - expand = “person.names”  
      - expand = “team.schedule.next”  
      - expand = “team.schedule.previous”  
      - expand = “team.stats”  
      - expand = “team.roster”, season = “20142015”  
      - teamId = “4, 5, 29”  
      - stats = “statsSingleSeasonPlayoffs”

See more details in the documentation of [NHL
records](https://gitlab.com/dword4/nhlapi/-/blob/master/records-api.md)
and [NHL
stats](https://gitlab.com/dword4/nhlapi/-/blob/master/stats-api.md).

The function `nhlFun` can retrieve any of the datapoints covered above,
and one or more arguments listed above are required.

Examples of arguments:

  - `endpoint = "stats", ID = "Calgary Flames", expand =
    "team.roster"`  
  - `endpoint = "franchise"`

<!-- end list -->

``` r
# add a 
nhlFun <- function(endpoint, ...){
  if (endpoint == "franchise") {
    getFran()
  } else if (endpoint == "team total") {
    getFranTeamTot()
  } else if (endpoint == "season record"){
    getFranSeaRec(...)
  } else if (endpoint == "goalie record"){
    getFranGoaRec(...)
  } else if (endpoint == "skater record"){
    getFranSkaRec(...)
  } else if (endpoint == "stats"){
    getStats(...)
  } else {
    stop("Please enter a valid endpoint")
  }
}
# nhlFun(endpoint = "skater record", 20) %>% tbl_df()
nhlFun(endpoint = "stats", ID = "Calgary Flames", expand = "person.team") %>% tbl_df()
nhlFun(endpoint = "stats", ID = 14, expand = "team.schedule.next") %>% tbl_df()
```

## Exploratory Data Analysis

### Retrieve Information

Now we use functions above to do exploratory data analysis. We will
first retrieve data from two endpoints: **team total** and
**person.names**, remove some columns we don’t need, and combine the two
data frames. Then we’ll add two new variables: `winPercent` (the
proportion of wins among all games played) and `homeWinPercent` (the
proportion of wins of home games among all home games). We also create a
subset to include fewer columns and transform the data from wide to long
to show the numbers of wins, losses, and ties in various situations for
each division.

``` r
# perhaps use the wrapper function here
franTot <- nhlFun(endpoint = "team total")
franTot <- franTot %>% select(-c("id", "activeFranchise", "firstSeasonId", "lastSeasonId"))
franStats <- nhlFun(endpoint = "stats", expand = "person.names") 
franStats <- franStats %>% select(c("locationName", "firstYearOfPlay", "franchiseId", "venue.city", "venue.timeZone.id", "venue.timeZone.tz", "division.name", "conference.name"))
# create two variables: winPercent and homeWinPercent 
combined <- inner_join(franTot, franStats, by = "franchiseId") %>% mutate(winPercent = wins / gamesPlayed, homeWinPercent = homeWins / (homeWins + homeLosses + homeOvertimeLosses + homeTies))
head(combined)

# create a subset for numbers of games lost or won
subset <- combined %>% select(starts_with("home"), starts_with("road"), "division.name", -"homeWinPercent") %>% gather(-"division.name", key = "type", value = "game") %>% group_by(division.name, type) %>% summarise(sum = sum(game, na.rm = TRUE))
```

### Summaries

Now we have the data, we can see some summary statistics for continuous
variables. It is interesting to see that, for all the variables listed,
the medians are much smaller than the means, suggesting that the
distributions are right skewed. Perhaps a few teams have larger than
average values, pulling the means upwards. A plot on penalty minutes
shows the right-skewedness.

Two contingency tables show the counts of two categorical variables. The
first contingency table shows that 1926, 1967, and 1979 added the most
number of teams. The second contingency table show that the same numbers
of teams play in the regular seasons and playoffs in the Atlantic,
Central, and Pacific seasons. Somehow one team in the Metropolitan
division did not show up in the playoffs. Perhaps the data is missing or
the team really did not make it into the playoffs.

``` r
# summaries
apply(combined[,c(3, 6:12, 14:18)], FUN = summary, MARGIN = 2)
```

    ## $gamesPlayed
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     2.0   153.5   480.0  1395.0  2054.0  6731.0 
    ## 
    ## $homeLosses
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     0.0    33.0   104.0   239.7   363.0  1132.0 
    ## 
    ## $homeOvertimeLosses
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##    0.00    0.00    7.50   35.86   73.75  112.00      21 
    ## 
    ## $homeTies
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##     0.0    11.0    58.0   107.5   166.5   448.0      36 
    ## 
    ## $homeWins
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     0.0    33.5   112.0   367.8   520.0  2025.0 
    ## 
    ## $losses
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     1.0    71.0   236.0   575.5   830.0  2736.0 
    ## 
    ## $overtimeLosses
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##    0.00    0.00   11.50   73.18  158.00  203.00      21 
    ## 
    ## $penaltyMinutes
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##      12    1722    7217   20812   29416   91941 
    ## 
    ## $points
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##     0.0     0.0    58.5  1372.6  2170.5  7899.0       1 
    ## 
    ## $roadLosses
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     1.0    38.0   128.0   335.8   467.0  1619.0 
    ## 
    ## $roadOvertimeLosses
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##    0.00    0.00    7.00   37.92   78.75   95.00      21 
    ## 
    ## $roadTies
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##     0.0     7.0    55.0   108.8   174.0   456.0      36 
    ## 
    ## $roadWins
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     0.0    26.5    95.0   269.4   416.5  1424.0

``` r
# contingency table
table(combined$division.name, combined$firstYearOfPlay)
```

    ##               
    ##                1909 1917 1924 1926 1967 1970 1972 1974 1979 1980 1982 1990 1991 1993
    ##   Atlantic        2    6    2    6    0    2    0    0    0    0    0    2    2    2
    ##   Central         0    0    0    2    6    0    0    0    4    0    0    0    0    0
    ##   Metropolitan    0    0    0    2    4    0    2    2    4    0    5    0    0    0
    ##   Pacific         0    0    0    0    2    2    0    0    8    4    0    2    0    2
    ##               
    ##                1997 2011 2016
    ##   Atlantic        0    0    0
    ##   Central         4    4    0
    ##   Metropolitan    2    0    0
    ##   Pacific         0    0    2

``` r
table(combined$division.name, combined$gameTypeId)
```

    ##               
    ##                 2  3
    ##   Atlantic     12 12
    ##   Central      10 10
    ##   Metropolitan 11 10
    ##   Pacific      11 11

### Visualize Data

Now we have the data and summaries, we can make plots to visualize the
data.

In the first scatter plot, we can see that as a lot of teams play low
number of games, and a few teams play a lot more number of games. The
winning percentage increases slightly as the number of games played
increases, but the slope is small and the difference might not be
significant. The colors of dots represent different divisions, and it
does not appear that there are different patterns for different
divisions. The second plot looks at each division separately; again the
patterns for different divisions do not seem to differ. The second plot
does show that no teams in the Pacific division play more than 4000
games, but all the other divisions have a few teams that play more than
4000 games.

The third plot is similar, but the y-axis shows the winning percentage
of home games, and the colors represent different types of games
(regular season vs. playoffs). It does not appear that the number of
games played and the winning percentage of home games are correlated, as
the slope is close to zero.

``` r
# scatter plot of homeWins and roadWins
ggplot(combined, aes(x = gamesPlayed, y = winPercent)) + geom_point(aes(color = division.name), position = "jitter") + geom_smooth(method = lm, color = "blue") + scale_color_discrete()
```

![](README_files/figure-gfm/unnamed-chunk-29-1.png)<!-- -->

``` r
ggplot(combined, aes(x = gamesPlayed, y = winPercent)) + geom_point(position = "jitter") + geom_smooth(method = lm, color = "blue") + facet_wrap(~ division.name)
```

![](README_files/figure-gfm/unnamed-chunk-29-2.png)<!-- -->

``` r
ggplot(combined, aes(x = gamesPlayed, y = homeWinPercent)) + geom_point(aes(color = as.factor(gameTypeId)), position = "jitter") + geom_smooth(method = lm, color = "blue") + scale_color_discrete(name = "Game Type", labels = c("regular season", "playoffs"))
```

    ## Warning: Removed 51 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 51 rows containing missing values (geom_point).

![](README_files/figure-gfm/unnamed-chunk-29-3.png)<!-- -->

The histogram shows that the center of winning percentages of all teams
is around 0.5, and the distribution is left-skewed. Therefore most of
the teams have winning percentages between 0.3 and 0.6, but some teams
have much lower winning percentages.

``` r
# histogram of winPercent
ggplot(combined, aes(x = winPercent)) + geom_histogram(bins = 30, aes(y = ..density..)) + geom_density(kernel = "gaussian", color = "red", alpha = 0.5, fill = "green")
```

![](README_files/figure-gfm/unnamed-chunk-30-1.png)<!-- -->

Next we look at the total numbers of games played for each division. The
boxplots show that, on average, the Central division play the most
games, and the Pacific division play the fewest. However, the variation
is large, and therefore the differences might not be significant.

``` r
# boxplots of gamesPlayed by division
ggplot(combined, aes(x = division.name, y = gamesPlayed)) + geom_boxplot() + geom_jitter(aes(color = venue.timeZone.tz))
```

![](README_files/figure-gfm/unnamed-chunk-31-1.png)<!-- -->

Now we look at two barplots. The first one shows wins, losses, and ties
of each division. We see that the Atlantic division play the highest
number of games and the Pacific division play the lowest number of
games. Overall, most results are from wins and losses, and ties and
overtime occupy a small proportion for all divisions. The second
barplots show that all divisions have higher winning percentages in
playoffs than in the regular season. The differences among divisions
might not be significant.

``` r
# barplot of gamePlayed
ggplot(subset, aes(y = sum, fill = type)) + geom_bar(position = "stack", stat = "identity", aes(x = division.name))
```

![](README_files/figure-gfm/unnamed-chunk-32-1.png)<!-- -->

``` r
ggplot(combined, aes(x = division.name, y = winPercent, fill = as.factor(gameTypeId))) + geom_bar(position = "dodge", stat = "identity") + scale_fill_discrete(name = "Game Type", labels = c("regular season", "playoffs"))
```

![](README_files/figure-gfm/unnamed-chunk-32-2.png)<!-- -->

Lastly, we look at a frequency plot of penalty minutes. The plot clearly
shows that the distribution is right-skewed. 20 teams have no penalty
minutes, and a few teams have penalty minutes of more than 50,000.

``` r
# frequency plot for penalty minutes
ggplot(combined, aes(x = penaltyMinutes)) + geom_freqpoly()
```

![](README_files/figure-gfm/unnamed-chunk-33-1.png)<!-- -->
