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

In this vignette, we want to show how to access APIs to retrieve data.
We use two NHL repositories as examples: [NHL
records](https://gitlab.com/dword4/nhlapi/-/tree/master) and [NHL
stats](https://gitlab.com/dword4/nhlapi/-/blob/master/stats-api.md).

## Required Packages

To be able to access data from APIs, you should install and load the
`httr`, `jsonlite`, and `tidyverse` packages.

    ```r
    library(httr)
    library(jsonlite)
    library(tidyverse)
    ```

## Functions

### NHL records API

``` r
baseurl_records <- "https://records.nhl.com/site/api"
```

Here is a function to get basic information about all teams.  
/franchise (Returns id, ﬁrstSeasonId and lastSeasonId and name of every
team in the history of the NHL)

``` r
getFran <- function(){
  fullurl <- paste0(baseurl_records, "/", "franchise")
  fran <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(fran$data)
}
getFran() %>% head()
```

    ## No encoding supplied: defaulting to UTF-8.

    ##   id firstSeasonId lastSeasonId mostRecentTeamId
    ## 1  1      19171918           NA                8
    ## 2  2      19171918     19171918               41
    ## 3  3      19171918     19341935               45
    ## 4  4      19191920     19241925               37
    ## 5  5      19171918           NA               10
    ## 6  6      19241925           NA                6
    ##   teamCommonName teamPlaceName
    ## 1      Canadiens      Montréal
    ## 2      Wanderers      Montreal
    ## 3         Eagles     St. Louis
    ## 4         Tigers      Hamilton
    ## 5    Maple Leafs       Toronto
    ## 6         Bruins        Boston

This is a function to retrieve stats about all teams.  
/franchise-team-totals (Returns Total stats for every franchise (ex
roadTies, roadWins, etc))

``` r
getFranTeamTot <- function() {
  fullurl <- paste0(baseurl_records, "/", "franchise-team-totals")
  franTeamTot <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(franTeamTot$data)
}
getFranTeamTot() %>% head()
```

    ## No encoding supplied: defaulting to UTF-8.

    ##   id activeFranchise firstSeasonId franchiseId gameTypeId
    ## 1  1               1      19821983          23          2
    ## 2  2               1      19821983          23          3
    ## 3  3               1      19721973          22          2
    ## 4  4               1      19721973          22          3
    ## 5  5               1      19261927          10          2
    ## 6  6               1      19261927          10          3
    ##   gamesPlayed goalsAgainst goalsFor homeLosses
    ## 1        2937         8708     8647        507
    ## 2         257          634      697         53
    ## 3        3732        11779    11889        674
    ## 4         291          850      931         48
    ## 5        6504        19863    19864       1132
    ## 6         518         1447     1404        104
    ##   homeOvertimeLosses homeTies homeWins lastSeasonId losses
    ## 1                 82       96      783           NA   1181
    ## 2                  0       NA       74           NA    120
    ## 3                 81      170      942           NA   1570
    ## 4                  1       NA       90           NA    131
    ## 5                 73      448     1600           NA   2693
    ## 6                  0        1      137           NA    266
    ##   overtimeLosses penaltyMinutes pointPctg points roadLosses
    ## 1            162          44397    0.5330   3131        674
    ## 2              0           4266    0.0039      2         67
    ## 3            159          57422    0.5115   3818        896
    ## 4              0           5540    0.0137      8         83
    ## 5            147          85564    0.5125   6667       1561
    ## 6              0           8181    0.0000      0        162
    ##   roadOvertimeLosses roadTies roadWins shootoutLosses
    ## 1                 80      123      592             79
    ## 2                  0       NA       63              0
    ## 3                 78      177      714             67
    ## 4                  0       NA       70              0
    ## 5                 74      360     1256             66
    ## 6                  0        7      107              0
    ##   shootoutWins shutouts teamId           teamName ties
    ## 1           78      193      1  New Jersey Devils  219
    ## 2            0       25      1  New Jersey Devils   NA
    ## 3           82      167      2 New York Islanders  347
    ## 4            0       12      2 New York Islanders   NA
    ## 5           78      403      3   New York Rangers  808
    ## 6            0       44      3   New York Rangers    8
    ##   triCode wins
    ## 1     NJD 1375
    ## 2     NJD  137
    ## 3     NYI 1656
    ## 4     NYI  160
    ## 5     NYR 2856
    ## 6     NYR  244

To allow for convenient access of team information in the following
function, we first construct a subset of data, so users can use team
names or franchise ID to look up information.

``` r
index <- getFranTeamTot() %>% select(c("franchiseId", "teamName")) %>% unique()
```

    ## No encoding supplied: defaulting to UTF-8.

This function retrieves season records from one specific team, and
therefore you need to provide the `franchiseId` or `teamName` as an
argument. The ID can be found using `getFran` or `getFranTeamTot`.  
/site/api/franchise-season-records?cayenneExp=franchiseId=ID (Drill-down
into season records for a specific franchise)

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
getFranSeaRec(20) %>% head()
```

    ## No encoding supplied: defaulting to UTF-8.

    ##   id fewestGoals fewestGoalsAgainst
    ## 1 23         182                185
    ##   fewestGoalsAgainstSeasons fewestGoalsSeasons fewestLosses
    ## 1              2010-11 (82)       2016-17 (82)           19
    ##   fewestLossesSeasons fewestPoints fewestPointsSeasons
    ## 1        2010-11 (82)           48        1971-72 (78)
    ##   fewestTies fewestTiesSeasons fewestWins
    ## 1          3      1993-94 (84)         20
    ##            fewestWinsSeasons franchiseId     franchiseName
    ## 1 1971-72 (78), 1977-78 (80)          20 Vancouver Canucks
    ##   homeLossStreak       homeLossStreakDates homePointStreak
    ## 1              7 Mar 11 2017 - Apr 08 2017              18
    ##        homePointStreakDates homeWinStreak
    ## 1 Nov 04 1992 - Jan 16 1993            11
    ##          homeWinStreakDates homeWinlessStreak
    ## 1 Feb 03 2009 - Mar 19 2009                12
    ##      homeWinlessStreakDates lossStreak
    ## 1 Feb 19 2017 - Apr 08 2017         10
    ##             lossStreakDates mostGameGoals
    ## 1 Oct 23 1997 - Nov 11 1997            11
    ##                                                                         mostGameGoalsDates
    ## 1 Mar 28 1971 - CGS 5 @ VAN 11, Nov 25 1986 - LAK 5 @ VAN 11, Mar 01 1992 - CGY 0 @ VAN 11
    ##   mostGoals mostGoalsAgainst mostGoalsAgainstSeasons
    ## 1       346              401            1984-85 (80)
    ##   mostGoalsSeasons mostLosses mostLossesSeasons
    ## 1     1992-93 (84)         50      1971-72 (78)
    ##   mostPenaltyMinutes mostPenaltyMinutesSeasons mostPoints
    ## 1               2326              1992-93 (84)        117
    ##   mostPointsSeasons mostShutouts mostShutoutsSeasons
    ## 1      2010-11 (82)           10        2008-09 (82)
    ##   mostTies mostTiesSeasons mostWins mostWinsSeasons
    ## 1       20    1980-81 (80)       54    2010-11 (82)
    ##   pointStreak          pointStreakDates roadLossStreak
    ## 1          17 Dec 08 2010 - Jan 11 2011             12
    ##         roadLossStreakDates roadPointStreak
    ## 1 Nov 28 1981 - Feb 06 1982              10
    ##        roadPointStreakDates roadWinStreak
    ## 1 Dec 01 2010 - Jan 11 2011             9
    ##          roadWinStreakDates roadWinlessStreak
    ## 1 Mar 05 2011 - Mar 29 2011                20
    ##      roadWinlessStreakDates winStreak
    ## 1 Jan 02 1986 - Apr 02 1986        10
    ##              winStreakDates winlessStreak
    ## 1 Nov 09 2002 - Nov 30 2002            NA
    ##   winlessStreakDates
    ## 1                 NA

``` r
getFranSeaRec("Vancouver Canucks") %>% head()
```

    ## No encoding supplied: defaulting to UTF-8.

    ##   id fewestGoals fewestGoalsAgainst
    ## 1 23         182                185
    ##   fewestGoalsAgainstSeasons fewestGoalsSeasons fewestLosses
    ## 1              2010-11 (82)       2016-17 (82)           19
    ##   fewestLossesSeasons fewestPoints fewestPointsSeasons
    ## 1        2010-11 (82)           48        1971-72 (78)
    ##   fewestTies fewestTiesSeasons fewestWins
    ## 1          3      1993-94 (84)         20
    ##            fewestWinsSeasons franchiseId     franchiseName
    ## 1 1971-72 (78), 1977-78 (80)          20 Vancouver Canucks
    ##   homeLossStreak       homeLossStreakDates homePointStreak
    ## 1              7 Mar 11 2017 - Apr 08 2017              18
    ##        homePointStreakDates homeWinStreak
    ## 1 Nov 04 1992 - Jan 16 1993            11
    ##          homeWinStreakDates homeWinlessStreak
    ## 1 Feb 03 2009 - Mar 19 2009                12
    ##      homeWinlessStreakDates lossStreak
    ## 1 Feb 19 2017 - Apr 08 2017         10
    ##             lossStreakDates mostGameGoals
    ## 1 Oct 23 1997 - Nov 11 1997            11
    ##                                                                         mostGameGoalsDates
    ## 1 Mar 28 1971 - CGS 5 @ VAN 11, Nov 25 1986 - LAK 5 @ VAN 11, Mar 01 1992 - CGY 0 @ VAN 11
    ##   mostGoals mostGoalsAgainst mostGoalsAgainstSeasons
    ## 1       346              401            1984-85 (80)
    ##   mostGoalsSeasons mostLosses mostLossesSeasons
    ## 1     1992-93 (84)         50      1971-72 (78)
    ##   mostPenaltyMinutes mostPenaltyMinutesSeasons mostPoints
    ## 1               2326              1992-93 (84)        117
    ##   mostPointsSeasons mostShutouts mostShutoutsSeasons
    ## 1      2010-11 (82)           10        2008-09 (82)
    ##   mostTies mostTiesSeasons mostWins mostWinsSeasons
    ## 1       20    1980-81 (80)       54    2010-11 (82)
    ##   pointStreak          pointStreakDates roadLossStreak
    ## 1          17 Dec 08 2010 - Jan 11 2011             12
    ##         roadLossStreakDates roadPointStreak
    ## 1 Nov 28 1981 - Feb 06 1982              10
    ##        roadPointStreakDates roadWinStreak
    ## 1 Dec 01 2010 - Jan 11 2011             9
    ##          roadWinStreakDates roadWinlessStreak
    ## 1 Mar 05 2011 - Mar 29 2011                20
    ##      roadWinlessStreakDates winStreak
    ## 1 Jan 02 1986 - Apr 02 1986        10
    ##              winStreakDates winlessStreak
    ## 1 Nov 09 2002 - Nov 30 2002            NA
    ##   winlessStreakDates
    ## 1                 NA

This function retrieves goalie records, and again a `franchiseId` or
`teamName` is required.  
/franchise-goalie-records?cayenneExp=franchiseId=ID (Goalie records for
the specified franchise)

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
getFranGoaRec(20) %>% head()
```

    ## No encoding supplied: defaulting to UTF-8.

    ##    id activePlayer firstName franchiseId     franchiseName
    ## 1 243        FALSE      Kirk          20 Vancouver Canucks
    ## 2 297        FALSE   Roberto          20 Vancouver Canucks
    ## 3 304        FALSE   Richard          20 Vancouver Canucks
    ## 4 364        FALSE      Gary          20 Vancouver Canucks
    ## 5 367        FALSE      Sean          20 Vancouver Canucks
    ## 6 373        FALSE   Jacques          20 Vancouver Canucks
    ##   gameTypeId gamesPlayed lastName losses
    ## 1          2         516   McLean    228
    ## 2          2         448   Luongo    137
    ## 3          2         377  Brodeur    173
    ## 4          2          73  Bromley     27
    ## 5          2          16    Burke      9
    ## 6          2          10    Caron      5
    ##                mostGoalsAgainstDates
    ## 1                         1996-10-19
    ## 2             2013-02-24, 2010-04-01
    ## 3                         1981-10-17
    ## 4             1981-02-20, 1979-03-08
    ## 5 1998-01-28, 1998-01-21, 1998-01-15
    ## 6                         1973-12-20
    ##   mostGoalsAgainstOneGame         mostSavesDates
    ## 1                       9 1997-04-05, 1987-12-17
    ## 2                       8             2010-03-20
    ## 3                      10             1985-02-10
    ## 4                       9             1979-03-15
    ## 5                       6             1998-01-26
    ## 6                       9             1973-11-27
    ##   mostSavesOneGame  mostShotsAgainstDates
    ## 1               48             1988-11-17
    ## 2               50             2010-03-20
    ## 3               51             1985-02-10
    ## 4               41             1979-03-15
    ## 5               33 1998-01-28, 1998-01-26
    ## 6               33             1973-12-20
    ##   mostShotsAgainstOneGame mostShutoutsOneSeason
    ## 1                      52                     5
    ## 2                      54                     9
    ## 3                      54                     2
    ## 4                      45                     2
    ## 5                      36                     0
    ## 6                      39                     0
    ##   mostShutoutsSeasonIds mostWinsOneSeason mostWinsSeasonIds
    ## 1              19911992                38          19911992
    ## 2              20082009                47          20062007
    ## 3    19811982, 19851986                21          19821983
    ## 4              19781979                11          19781979
    ## 5              19971998                 2          19971998
    ## 6              19731974                 2          19731974
    ##   overtimeLosses playerId positionCode rookieGamesPlayed
    ## 1             NA  8449474            G                41
    ## 2             50  8466141            G                NA
    ## 3             NA  8445694            G                NA
    ## 4             NA  8445695            G                NA
    ## 5             NA  8445769            G                NA
    ## 6             NA  8445966            G                NA
    ##   rookieShutouts rookieWins seasons shutouts ties wins
    ## 1              1         11      11       20   62  211
    ## 2             NA         NA       8       38   NA  252
    ## 3             NA         NA       8        6   62  126
    ## 4             NA         NA       3        3   14   25
    ## 5             NA         NA       1        0    4    2
    ## 6             NA         NA       1        0    1    2

``` r
getFranGoaRec("Vancouver Canucks") %>% head()
```

    ## No encoding supplied: defaulting to UTF-8.

    ##    id activePlayer firstName franchiseId     franchiseName
    ## 1 243        FALSE      Kirk          20 Vancouver Canucks
    ## 2 297        FALSE   Roberto          20 Vancouver Canucks
    ## 3 304        FALSE   Richard          20 Vancouver Canucks
    ## 4 364        FALSE      Gary          20 Vancouver Canucks
    ## 5 367        FALSE      Sean          20 Vancouver Canucks
    ## 6 373        FALSE   Jacques          20 Vancouver Canucks
    ##   gameTypeId gamesPlayed lastName losses
    ## 1          2         516   McLean    228
    ## 2          2         448   Luongo    137
    ## 3          2         377  Brodeur    173
    ## 4          2          73  Bromley     27
    ## 5          2          16    Burke      9
    ## 6          2          10    Caron      5
    ##                mostGoalsAgainstDates
    ## 1                         1996-10-19
    ## 2             2013-02-24, 2010-04-01
    ## 3                         1981-10-17
    ## 4             1981-02-20, 1979-03-08
    ## 5 1998-01-28, 1998-01-21, 1998-01-15
    ## 6                         1973-12-20
    ##   mostGoalsAgainstOneGame         mostSavesDates
    ## 1                       9 1997-04-05, 1987-12-17
    ## 2                       8             2010-03-20
    ## 3                      10             1985-02-10
    ## 4                       9             1979-03-15
    ## 5                       6             1998-01-26
    ## 6                       9             1973-11-27
    ##   mostSavesOneGame  mostShotsAgainstDates
    ## 1               48             1988-11-17
    ## 2               50             2010-03-20
    ## 3               51             1985-02-10
    ## 4               41             1979-03-15
    ## 5               33 1998-01-28, 1998-01-26
    ## 6               33             1973-12-20
    ##   mostShotsAgainstOneGame mostShutoutsOneSeason
    ## 1                      52                     5
    ## 2                      54                     9
    ## 3                      54                     2
    ## 4                      45                     2
    ## 5                      36                     0
    ## 6                      39                     0
    ##   mostShutoutsSeasonIds mostWinsOneSeason mostWinsSeasonIds
    ## 1              19911992                38          19911992
    ## 2              20082009                47          20062007
    ## 3    19811982, 19851986                21          19821983
    ## 4              19781979                11          19781979
    ## 5              19971998                 2          19971998
    ## 6              19731974                 2          19731974
    ##   overtimeLosses playerId positionCode rookieGamesPlayed
    ## 1             NA  8449474            G                41
    ## 2             50  8466141            G                NA
    ## 3             NA  8445694            G                NA
    ## 4             NA  8445695            G                NA
    ## 5             NA  8445769            G                NA
    ## 6             NA  8445966            G                NA
    ##   rookieShutouts rookieWins seasons shutouts ties wins
    ## 1              1         11      11       20   62  211
    ## 2             NA         NA       8       38   NA  252
    ## 3             NA         NA       8        6   62  126
    ## 4             NA         NA       3        3   14   25
    ## 5             NA         NA       1        0    4    2
    ## 6             NA         NA       1        0    1    2

This function retrieves information about skater records, and a
`franchiseId` or `teamName` is required.  
/franchise-skater-records?cayenneExp=franchiseId=ID (Skater records,
same interaction as goalie endpoint)

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
getFranSkaRec(20) %>% head()
```

    ## No encoding supplied: defaulting to UTF-8.

    ##      id activePlayer assists firstName franchiseId
    ## 1 16941        FALSE     648    Daniel          20
    ## 2 16942        FALSE     830    Henrik          20
    ## 3 17026        FALSE      52      Gino          20
    ## 4 17057        FALSE     224     Pavel          20
    ## 5 17115        FALSE      53    Donald          20
    ## 6 17141        FALSE     410    Markus          20
    ##       franchiseName gameTypeId gamesPlayed goals lastName
    ## 1 Vancouver Canucks          2        1306   393    Sedin
    ## 2 Vancouver Canucks          2        1330   240    Sedin
    ## 3 Vancouver Canucks          2         444    46   Odjick
    ## 4 Vancouver Canucks          2         428   254     Bure
    ## 5 Vancouver Canucks          2         388    50 Brashear
    ## 6 Vancouver Canucks          2         884   346  Naslund
    ##                                                                                                                                                                                                     mostAssistsGameDates
    ## 1 2006-02-03, 2006-10-25, 2007-01-13, 2008-03-06, 2008-10-09, 2009-10-07, 2010-01-05, 2010-03-03, 2011-03-08, 2011-12-19, 2012-02-18, 2013-04-06, 2014-10-11, 2014-11-19, 2015-01-03, 2015-11-10, 2016-02-21, 2017-12-15
    ## 2                                                                                                                                                             2007-02-06, 2010-04-10, 2012-02-18, 2015-11-21, 2016-02-21
    ## 3                                                                                                                                                                                     1992-10-16, 1992-12-29, 1993-10-23
    ## 4                                                                                                                                                                                     1992-12-18, 1995-04-22, 1997-02-06
    ## 5                                                                                                                                                                                                             2000-12-08
    ## 6                                                                                                                                                                                                             2003-02-25
    ##   mostAssistsOneGame mostAssistsOneSeason
    ## 1                  3                   63
    ## 2                  4                   83
    ## 3                  2                   13
    ## 4                  4                   50
    ## 5                  3                   19
    ## 6                  5                   56
    ##   mostAssistsSeasonIds                 mostGoalsGameDates
    ## 1             20102011                         2004-02-24
    ## 2             20092010                         2009-11-14
    ## 3   19921993, 19931994 1992-11-23, 1993-10-11, 1993-11-16
    ## 4             19921993                         1992-10-12
    ## 5             20002001                         1997-04-12
    ## 6             20022003             2002-12-14, 2003-12-09
    ##   mostGoalsOneGame mostGoalsOneSeason mostGoalsSeasonIds
    ## 1                4                 41           20102011
    ## 2                3                 29           20092010
    ## 3                2                 16           19931994
    ## 4                4                 60 19921993, 19931994
    ## 5                2                 11           19992000
    ## 6                4                 48           20022003
    ##   mostPenaltyMinutesOneSeason mostPenaltyMinutesSeasonIds
    ## 1                          50                    20072008
    ## 2                          66                    20062007
    ## 3                         371                    19961997
    ## 4                          86                    19931994
    ## 5                         372                    19971998
    ## 6                          74                    19981999
    ##      mostPointsGameDates mostPointsOneGame
    ## 1             2007-02-06                 5
    ## 2             2015-11-21                 5
    ## 3 1992-11-23, 1993-10-23                 3
    ## 4 1992-10-12, 1996-12-15                 5
    ## 5             2000-12-08                 3
    ## 6             2003-02-25                 6
    ##   mostPointsOneSeason mostPointsSeasonIds penaltyMinutes
    ## 1                 104            20102011            546
    ## 2                 112            20092010            680
    ## 3                  29            19931994           2127
    ## 4                 110            19921993            328
    ## 5                  28            20002001           1159
    ## 6                 104            20022003            614
    ##   playerId points positionCode rookiePoints seasons
    ## 1  8467875   1041            L           34      17
    ## 2  8467876   1070            C           29      17
    ## 3  8449961     98            L            8       8
    ## 4  8455738    478            R           60       7
    ## 5  8459246    103            L           NA       6
    ## 6  8458530    756            L           NA      12

``` r
getFranSkaRec("Vancouver Canucks") %>% head()
```

    ## No encoding supplied: defaulting to UTF-8.

    ##      id activePlayer assists firstName franchiseId
    ## 1 16941        FALSE     648    Daniel          20
    ## 2 16942        FALSE     830    Henrik          20
    ## 3 17026        FALSE      52      Gino          20
    ## 4 17057        FALSE     224     Pavel          20
    ## 5 17115        FALSE      53    Donald          20
    ## 6 17141        FALSE     410    Markus          20
    ##       franchiseName gameTypeId gamesPlayed goals lastName
    ## 1 Vancouver Canucks          2        1306   393    Sedin
    ## 2 Vancouver Canucks          2        1330   240    Sedin
    ## 3 Vancouver Canucks          2         444    46   Odjick
    ## 4 Vancouver Canucks          2         428   254     Bure
    ## 5 Vancouver Canucks          2         388    50 Brashear
    ## 6 Vancouver Canucks          2         884   346  Naslund
    ##                                                                                                                                                                                                     mostAssistsGameDates
    ## 1 2006-02-03, 2006-10-25, 2007-01-13, 2008-03-06, 2008-10-09, 2009-10-07, 2010-01-05, 2010-03-03, 2011-03-08, 2011-12-19, 2012-02-18, 2013-04-06, 2014-10-11, 2014-11-19, 2015-01-03, 2015-11-10, 2016-02-21, 2017-12-15
    ## 2                                                                                                                                                             2007-02-06, 2010-04-10, 2012-02-18, 2015-11-21, 2016-02-21
    ## 3                                                                                                                                                                                     1992-10-16, 1992-12-29, 1993-10-23
    ## 4                                                                                                                                                                                     1992-12-18, 1995-04-22, 1997-02-06
    ## 5                                                                                                                                                                                                             2000-12-08
    ## 6                                                                                                                                                                                                             2003-02-25
    ##   mostAssistsOneGame mostAssistsOneSeason
    ## 1                  3                   63
    ## 2                  4                   83
    ## 3                  2                   13
    ## 4                  4                   50
    ## 5                  3                   19
    ## 6                  5                   56
    ##   mostAssistsSeasonIds                 mostGoalsGameDates
    ## 1             20102011                         2004-02-24
    ## 2             20092010                         2009-11-14
    ## 3   19921993, 19931994 1992-11-23, 1993-10-11, 1993-11-16
    ## 4             19921993                         1992-10-12
    ## 5             20002001                         1997-04-12
    ## 6             20022003             2002-12-14, 2003-12-09
    ##   mostGoalsOneGame mostGoalsOneSeason mostGoalsSeasonIds
    ## 1                4                 41           20102011
    ## 2                3                 29           20092010
    ## 3                2                 16           19931994
    ## 4                4                 60 19921993, 19931994
    ## 5                2                 11           19992000
    ## 6                4                 48           20022003
    ##   mostPenaltyMinutesOneSeason mostPenaltyMinutesSeasonIds
    ## 1                          50                    20072008
    ## 2                          66                    20062007
    ## 3                         371                    19961997
    ## 4                          86                    19931994
    ## 5                         372                    19971998
    ## 6                          74                    19981999
    ##      mostPointsGameDates mostPointsOneGame
    ## 1             2007-02-06                 5
    ## 2             2015-11-21                 5
    ## 3 1992-11-23, 1993-10-23                 3
    ## 4 1992-10-12, 1996-12-15                 5
    ## 5             2000-12-08                 3
    ## 6             2003-02-25                 6
    ##   mostPointsOneSeason mostPointsSeasonIds penaltyMinutes
    ## 1                 104            20102011            546
    ## 2                 112            20092010            680
    ## 3                  29            19931994           2127
    ## 4                 110            19921993            328
    ## 5                  28            20002001           1159
    ## 6                 104            20022003            614
    ##   playerId points positionCode rookiePoints seasons
    ## 1  8467875   1041            L           34      17
    ## 2  8467876   1070            C           29      17
    ## 3  8449961     98            L            8       8
    ## 4  8455738    478            R           60       7
    ## 5  8459246    103            L           NA       6
    ## 6  8458530    756            L           NA      12

### NHL stats API

For this function, eight modifiers can be chosen, and thus an argument
(`expand`, `teamID`, or `stats`) has to be provided. Below are the eight
modifiers:

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

  - `expand = "person.names"`  
  - `teamId = "4, 5, 29"`  
  - `stats = "statsSingleSeasonPlayoffs"`

For more information about these modifiers, see [the
documentation](https://gitlab.com/dword4/nhlapi/-/blob/master/stats-api.md).

``` r
baseurl_stats <- "https://statsapi.web.nhl.com/api/v1/teams"
getStats <- function(expand = "", teamID = "", stats = ""){
  if (teamID!=""){
    if (length(teamID)==1){
      baseurl_stats <- paste0("https://statsapi.web.nhl.com/api/v1/teams/", teamID)
    }
    else{
      fullurl <- paste0("https://statsapi.web.nhl.com/api/v1/teams?teamID=", teamID)
    }
  }
  if (expand != ""){
    fullurl <- paste0(baseurl_stats, "?expand=", expand)
  } else if (stats != ""){
    fullurl <- paste0(baseurl_stats, "?stats=", stats)
  }
  stats <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  if (expand == "team.roster"){
    stats <- stats$teams$roster.roster
  } else if (expand == "team.schedule.next"){
    stats <- stats$teams$nextGameSchedule.dates
  } else if (expand == "team.schedule.previous"){
    stats <- stats$teams$previousGameSchedule.dates
  } else if (expand == "team.stats"){
    stats <- stats$teams$teamStats[[1]]$splits[[1]]
  } else {
    stats <- as.data.frame(stats$teams)
  }
  if (is.null(stats)) {
    stop("No information is available")
  } 
  return(stats)
}
# getStats(teamID = 20, expand = "team.stats")
# getStats(teamID = 20, expand = "person.names")
# getStats(teamID = 20, expand = "team.schedule.next")
# getStats(expand = "team.roster&season=20142015") %>% head()
getStats(teamID = 53, expand = "team.roster")
```

    ## [[1]]
    ##    jerseyNumber person.id      person.fullName
    ## 1            15   8470755      Brad Richardson
    ## 2            34   8471262       Carl Soderberg
    ## 3            33   8471274       Alex Goligoski
    ## 4             4   8471769   Niklas Hjalmarsson
    ## 5            40   8473546      Michael Grabner
    ## 6            81   8473548          Phil Kessel
    ## 7            55   8474218         Jason Demers
    ## 8            21   8474613         Derek Stepan
    ## 9            23   8475171 Oliver Ekman-Larsson
    ## 10           35   8475311        Darcy Kuemper
    ## 11           91   8475791          Taylor Hall
    ## 12           13   8476994    Vinnie Hinostroza
    ## 13           32   8477293         Antti Raanta
    ## 14           82   8477851      Jordan Oesterle
    ## 15            8   8477951        Nick Schmaltz
    ## 16           18   8477989     Christian Dvorak
    ## 17           36   8478432    Christian Fischer
    ## 18           67   8478474        Lawson Crouse
    ## 19           83   8478856        Conor Garland
    ## 20            9   8479343       Clayton Keller
    ## 21            6   8479345       Jakob Chychrun
    ## 22           29   8480849       Barrett Hayton
    ## 23           46   8480950      Ilya Lyubushkin
    ##               person.link position.code position.name
    ## 1  /api/v1/people/8470755             C        Center
    ## 2  /api/v1/people/8471262             C        Center
    ## 3  /api/v1/people/8471274             D    Defenseman
    ## 4  /api/v1/people/8471769             D    Defenseman
    ## 5  /api/v1/people/8473546             L     Left Wing
    ## 6  /api/v1/people/8473548             R    Right Wing
    ## 7  /api/v1/people/8474218             D    Defenseman
    ## 8  /api/v1/people/8474613             C        Center
    ## 9  /api/v1/people/8475171             D    Defenseman
    ## 10 /api/v1/people/8475311             G        Goalie
    ## 11 /api/v1/people/8475791             L     Left Wing
    ## 12 /api/v1/people/8476994             R    Right Wing
    ## 13 /api/v1/people/8477293             G        Goalie
    ## 14 /api/v1/people/8477851             D    Defenseman
    ## 15 /api/v1/people/8477951             C        Center
    ## 16 /api/v1/people/8477989             C        Center
    ## 17 /api/v1/people/8478432             R    Right Wing
    ## 18 /api/v1/people/8478474             L     Left Wing
    ## 19 /api/v1/people/8478856             R    Right Wing
    ## 20 /api/v1/people/8479343             R    Right Wing
    ## 21 /api/v1/people/8479345             D    Defenseman
    ## 22 /api/v1/people/8480849             C        Center
    ## 23 /api/v1/people/8480950             D    Defenseman
    ##    position.type position.abbreviation
    ## 1        Forward                     C
    ## 2        Forward                     C
    ## 3     Defenseman                     D
    ## 4     Defenseman                     D
    ## 5        Forward                    LW
    ## 6        Forward                    RW
    ## 7     Defenseman                     D
    ## 8        Forward                     C
    ## 9     Defenseman                     D
    ## 10        Goalie                     G
    ## 11       Forward                    LW
    ## 12       Forward                    RW
    ## 13        Goalie                     G
    ## 14    Defenseman                     D
    ## 15       Forward                     C
    ## 16       Forward                     C
    ## 17       Forward                    RW
    ## 18       Forward                    LW
    ## 19       Forward                    RW
    ## 20       Forward                    RW
    ## 21    Defenseman                     D
    ## 22       Forward                     C
    ## 23    Defenseman                     D

``` r
# getStats(stats = "statsSingleSeasonPlayoffs") %>% head()
```

## A wrapper function for all the functions above

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

``` r
# add a 
nhlFun <- function(endpoints, ...){
  if (endpoints == "franchise") {
    getFran()
  } else if (endpoints == "team total") {
    getFranTeamTot()
  } else if (endpoints == "season record"){
    getFranSeaRec(...)
  } else if (endpoints == "goalie record"){
    getFranGoaRec(...)
  } else if (endpoints == "skater record"){
    getFranSkaRec(...)
  } else if (endpoints == "stats"){
    getStats(...)
  } else {
    stop("Please enter a valid endpoint")
  }
}
nhlFun(endpoints = "skater record", 20)
```

    ## No encoding supplied: defaulting to UTF-8.

    ##       id activePlayer assists firstName franchiseId
    ## 1  16941        FALSE     648    Daniel          20
    ## 2  16942        FALSE     830    Henrik          20
    ## 3  17026        FALSE      52      Gino          20
    ## 4  17057        FALSE     224     Pavel          20
    ## 5  17115        FALSE      53    Donald          20
    ## 6  17141        FALSE     410    Markus          20
    ## 7  17178        FALSE     242      Doug          20
    ## 8  17222        FALSE      18    Claire          20
    ## 9  17238        FALSE       1       Jim          20
    ## 10 17241        FALSE     190      Greg          20
    ## 11 17254        FALSE       2      Greg          20
    ## 12 17262        FALSE       1     Bruce          20
    ## 13 17338        FALSE       0      John          20
    ## 14 17413        FALSE      21      Dave          20
    ## 15 17421        FALSE       2     Shawn          20
    ## 16 17556        FALSE       0       Ken          20
    ## 17 17562        FALSE      44     Gregg          20
    ## 18 17579        FALSE       0     Larry          20
    ## 19 17589        FALSE      25     Brent          20
    ## 20 17621        FALSE     267     Andre          20
    ## 21 17722        FALSE     131      Dave          20
    ## 22 17736        FALSE       0     Peter          20
    ## 23 17775        FALSE      34    Murray          20
    ## 24 17794        FALSE       1     Robin          20
    ## 25 17892        FALSE       0     Robin          20
    ## 26 17962        FALSE       3     Drake          20
    ## 27 17969        FALSE      31      Neil          20
    ## 28 17971        FALSE      55       Jim          20
    ## 29 18006        FALSE       2      Marc          20
    ## 30 18036        FALSE       4       Ken          20
    ## 31 18124        FALSE      20     Wayne          20
    ## 32 18136        FALSE       0       Bob          20
    ## 33 18163        FALSE     125      Rick          20
    ##        franchiseName gameTypeId gamesPlayed goals
    ## 1  Vancouver Canucks          2        1306   393
    ## 2  Vancouver Canucks          2        1330   240
    ## 3  Vancouver Canucks          2         444    46
    ## 4  Vancouver Canucks          2         428   254
    ## 5  Vancouver Canucks          2         388    50
    ## 6  Vancouver Canucks          2         884   346
    ## 7  Vancouver Canucks          2         666    65
    ## 8  Vancouver Canucks          2          32     8
    ## 9  Vancouver Canucks          2          65     0
    ## 10 Vancouver Canucks          2         489   179
    ## 11 Vancouver Canucks          2          12     4
    ## 12 Vancouver Canucks          2           5     0
    ## 13 Vancouver Canucks          2          13     0
    ## 14 Vancouver Canucks          2         116    22
    ## 15 Vancouver Canucks          2          70     1
    ## 16 Vancouver Canucks          2           1     0
    ## 17 Vancouver Canucks          2         273    23
    ## 18 Vancouver Canucks          2          15     0
    ## 19 Vancouver Canucks          2         124    23
    ## 20 Vancouver Canucks          2         458   121
    ## 21 Vancouver Canucks          2         409    23
    ## 22 Vancouver Canucks          2          10     2
    ## 23 Vancouver Canucks          2         383    10
    ## 24 Vancouver Canucks          2          40     0
    ## 25 Vancouver Canucks          2           2     0
    ## 26 Vancouver Canucks          2          39     2
    ## 27 Vancouver Canucks          2         106    13
    ## 28 Vancouver Canucks          2         241    15
    ## 29 Vancouver Canucks          2           9     0
    ## 30 Vancouver Canucks          2          27     4
    ## 31 Vancouver Canucks          2          53    14
    ## 32 Vancouver Canucks          2           2     0
    ## 33 Vancouver Canucks          2         324    96
    ##      lastName
    ## 1       Sedin
    ## 2       Sedin
    ## 3      Odjick
    ## 4        Bure
    ## 5    Brashear
    ## 6     Naslund
    ## 7     Lidster
    ## 8   Alexander
    ## 9       Agnew
    ## 10      Adams
    ## 11      Adams
    ## 12    Affleck
    ## 13     Arbour
    ## 14      Balon
    ## 15    Antoski
    ## 16      Block
    ## 17      Boddy
    ## 18  Bolonchuk
    ## 19     Ashton
    ## 20   Boudrias
    ## 21     Babych
    ## 22    Bakovic
    ## 23      Baron
    ## 24     Bartel
    ## 25       Bawa
    ## 26 Berehowsky
    ## 27    Belland
    ## 28    Benning
    ## 29   Bergevin
    ## 30      Berry
    ## 31   Connelly
    ## 32       Cook
    ## 33     Blight
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      mostAssistsGameDates
    ## 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  2006-02-03, 2006-10-25, 2007-01-13, 2008-03-06, 2008-10-09, 2009-10-07, 2010-01-05, 2010-03-03, 2011-03-08, 2011-12-19, 2012-02-18, 2013-04-06, 2014-10-11, 2014-11-19, 2015-01-03, 2015-11-10, 2016-02-21, 2017-12-15
    ## 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              2007-02-06, 2010-04-10, 2012-02-18, 2015-11-21, 2016-02-21
    ## 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      1992-10-16, 1992-12-29, 1993-10-23
    ## 4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      1992-12-18, 1995-04-22, 1997-02-06
    ## 5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              2000-12-08
    ## 6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              2003-02-25
    ## 7                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1986-01-31, 1986-12-23, 1991-03-16, 1992-11-26
    ## 8                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1978-03-15
    ## 9                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1987-12-15
    ## 10                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1988-02-14, 1992-01-21, 1992-11-10
    ## 11                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1989-03-10, 1989-04-01
    ## 12                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1979-12-19
    ## 13                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1970-11-04, 1970-11-05, 1970-11-07, 1970-11-10, 1970-11-11, 1970-11-14, 1970-11-15, 1970-11-17, 1970-11-20, 1970-11-21, 1970-11-24, 1970-11-26, 1970-11-28, 1970-11-29, 1970-12-01
    ## 14                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1972-01-19
    ## 15                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1993-11-03, 1994-01-09
    ## 16 1970-10-09, 1970-10-11, 1970-10-12, 1970-10-14, 1970-10-15, 1970-10-18, 1970-10-20, 1970-10-23, 1970-10-25, 1970-10-27, 1970-10-31, 1970-11-04, 1970-11-05, 1970-11-07, 1970-11-10, 1970-11-11, 1970-11-14, 1970-11-15, 1970-11-17, 1970-11-20, 1970-11-21, 1970-11-24, 1970-11-26, 1970-11-28, 1970-11-29, 1970-12-01, 1970-12-05, 1970-12-06, 1970-12-08, 1970-12-09, 1970-12-12, 1970-12-15, 1970-12-18, 1970-12-20, 1970-12-23, 1970-12-26, 1970-12-30, 1971-01-02, 1971-01-06, 1971-01-07, 1971-01-09, 1971-01-12, 1971-01-16, 1971-01-17, 1971-01-20, 1971-01-23, 1971-01-24, 1971-01-26, 1971-01-29, 1971-01-31, 1971-02-02, 1971-02-06, 1971-02-09, 1971-02-12, 1971-02-14, 1971-02-16, 1971-02-19, 1971-02-22, 1971-02-25, 1971-02-27, 1971-02-28, 1971-03-03, 1971-03-06, 1971-03-07, 1971-03-09, 1971-03-11, 1971-03-13, 1971-03-16, 1971-03-19, 1971-03-21, 1971-03-23, 1971-03-25, 1971-03-26, 1971-03-28, 1971-03-30, 1971-03-31, 1971-04-02, 1971-04-04
    ## 17                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1975-12-03
    ## 18 1972-10-07, 1972-10-11, 1972-10-12, 1972-10-14, 1972-10-17, 1972-10-19, 1972-10-21, 1972-10-22, 1972-10-24, 1972-10-28, 1972-10-31, 1972-11-03, 1972-11-05, 1972-11-08, 1972-11-11, 1972-11-12, 1972-11-14, 1972-11-17, 1972-11-19, 1972-11-21, 1972-11-22, 1972-11-24, 1972-11-26, 1972-11-28, 1972-12-01, 1972-12-05, 1972-12-07, 1972-12-09, 1972-12-10, 1972-12-12, 1972-12-15, 1972-12-16, 1972-12-20, 1972-12-21, 1972-12-23, 1972-12-26, 1972-12-29, 1972-12-30, 1973-01-01, 1973-01-03, 1973-01-06, 1973-01-07, 1973-01-09, 1973-01-12, 1973-01-14, 1973-01-16, 1973-01-19, 1973-01-20, 1973-01-24, 1973-01-26, 1973-01-27, 1973-02-01, 1973-02-03, 1973-02-04, 1973-02-06, 1973-02-09, 1973-02-11, 1973-02-13, 1973-02-14, 1973-02-16, 1973-02-17, 1973-02-20, 1973-02-22, 1973-02-24, 1973-02-28, 1973-03-03, 1973-03-04, 1973-03-09, 1973-03-10, 1973-03-14, 1973-03-16, 1973-03-17, 1973-03-21, 1973-03-22, 1973-03-25, 1973-03-27, 1973-03-30, 1973-03-31
    ## 19                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1980-03-10
    ## 20                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1973-02-09, 1973-10-10, 1973-10-26, 1973-11-16, 1974-03-31, 1974-10-15, 1974-11-16, 1974-12-28, 1975-02-01, 1975-02-14, 1975-02-15
    ## 21                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1991-10-21, 1992-11-28, 1993-02-03, 1993-03-09, 1993-10-06, 1993-11-05, 1994-01-16, 1994-04-01, 1995-02-18, 1995-03-11, 1995-10-15, 1996-02-10, 1996-12-15, 1997-02-15, 1997-03-26, 1997-04-09, 1997-12-15
    ## 22                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1988-03-08, 1988-03-10, 1988-03-12, 1988-03-13, 1988-03-16, 1988-03-18, 1988-03-22, 1988-03-25, 1988-03-26, 1988-03-29, 1988-04-01
    ## 23                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             2000-02-19
    ## 24                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1986-11-25
    ## 25                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1992-03-18, 1992-03-20, 1992-03-22, 1992-03-24, 1992-03-26, 1992-03-28, 1992-03-29, 1992-04-12, 1992-04-14, 1992-04-16
    ## 26                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     2001-03-16, 2001-12-08, 2001-12-13
    ## 27                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1982-02-28
    ## 28                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1988-02-21, 1990-02-28
    ## 29                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 2004-03-29, 2004-04-03
    ## 30                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1988-03-01
    ## 31                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1972-02-22, 1972-04-02
    ## 32 1970-10-09, 1970-10-11, 1970-10-12, 1970-10-14, 1970-10-15, 1970-10-18, 1970-10-20, 1970-10-23, 1970-10-25, 1970-10-27, 1970-10-31, 1970-11-04, 1970-11-05, 1970-11-07, 1970-11-10, 1970-11-11, 1970-11-14, 1970-11-15, 1970-11-17, 1970-11-20, 1970-11-21, 1970-11-24, 1970-11-26, 1970-11-28, 1970-11-29, 1970-12-01, 1970-12-05, 1970-12-06, 1970-12-08, 1970-12-09, 1970-12-12, 1970-12-15, 1970-12-18, 1970-12-20, 1970-12-23, 1970-12-26, 1970-12-30, 1971-01-02, 1971-01-06, 1971-01-07, 1971-01-09, 1971-01-12, 1971-01-16, 1971-01-17, 1971-01-20, 1971-01-23, 1971-01-24, 1971-01-26, 1971-01-29, 1971-01-31, 1971-02-02, 1971-02-06, 1971-02-09, 1971-02-12, 1971-02-14, 1971-02-16, 1971-02-19, 1971-02-22, 1971-02-25, 1971-02-27, 1971-02-28, 1971-03-03, 1971-03-06, 1971-03-07, 1971-03-09, 1971-03-11, 1971-03-13, 1971-03-16, 1971-03-19, 1971-03-21, 1971-03-23, 1971-03-25, 1971-03-26, 1971-03-28, 1971-03-30, 1971-03-31, 1971-04-02, 1971-04-04
    ## 33                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1976-03-23, 1977-12-23, 1978-04-07, 1979-01-05
    ##    mostAssistsOneGame mostAssistsOneSeason
    ## 1                   3                   63
    ## 2                   4                   83
    ## 3                   2                   13
    ## 4                   4                   50
    ## 5                   3                   19
    ## 6                   5                   56
    ## 7                   3                   51
    ## 8                   4                   18
    ## 9                   1                    1
    ## 10                  3                   40
    ## 11                  1                    2
    ## 12                  1                    1
    ## 13                  0                    0
    ## 14                  2                   19
    ## 15                  1                    2
    ## 16                  0                    0
    ## 17                  2                   12
    ## 18                  0                    0
    ## 19                  4                   14
    ## 20                  3                   62
    ## 21                  2                   28
    ## 22                  0                    0
    ## 23                  2                   10
    ## 24                  1                    1
    ## 25                  0                    0
    ## 26                  1                    2
    ## 27                  3                   13
    ## 28                  3                   26
    ## 29                  1                    2
    ## 30                  2                    3
    ## 31                  2                   20
    ## 32                  0                    0
    ## 33                  3                   40
    ##    mostAssistsSeasonIds
    ## 1              20102011
    ## 2              20092010
    ## 3    19921993, 19931994
    ## 4              19921993
    ## 5              20002001
    ## 6              20022003
    ## 7              19861987
    ## 8              19771978
    ## 9              19871988
    ## 10             19871988
    ## 11             19881989
    ## 12             19791980
    ## 13             19701971
    ## 14             19711972
    ## 15             19931994
    ## 16             19701971
    ## 17             19741975
    ## 18             19721973
    ## 19             19791980
    ## 20             19741975
    ## 21             19931994
    ## 22             19871988
    ## 23             19992000
    ## 24             19861987
    ## 25             19911992
    ## 26             20012002
    ## 27             19831984
    ## 28             19871988
    ## 29             20032004
    ## 30             19871988
    ## 31             19711972
    ## 32             19701971
    ## 33             19761977
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    mostGoalsGameDates
    ## 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          2004-02-24
    ## 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          2009-11-14
    ## 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1992-11-23, 1993-10-11, 1993-11-16
    ## 4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1992-10-12
    ## 5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1997-04-12
    ## 6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              2002-12-14, 2003-12-09
    ## 7                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1985-11-09, 1990-01-07
    ## 8                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1978-01-28, 1978-02-18
    ## 9  1986-10-11, 1986-10-13, 1986-10-15, 1986-10-16, 1986-10-18, 1986-10-22, 1986-10-24, 1986-10-26, 1986-10-28, 1986-10-31, 1986-11-02, 1986-11-04, 1986-11-05, 1986-11-07, 1986-11-08, 1986-11-11, 1986-11-12, 1986-11-14, 1986-11-18, 1986-11-21, 1986-11-22, 1986-11-25, 1986-11-26, 1986-11-29, 1986-12-02, 1986-12-05, 1986-12-07, 1986-12-09, 1986-12-11, 1986-12-13, 1986-12-14, 1986-12-17, 1986-12-19, 1986-12-20, 1986-12-23, 1986-12-27, 1986-12-30, 1987-01-02, 1987-01-04, 1987-01-06, 1987-01-07, 1987-01-10, 1987-01-11, 1987-01-14, 1987-01-16, 1987-01-17, 1987-01-19, 1987-01-21, 1987-01-23, 1987-01-27, 1987-01-28, 1987-01-30, 1987-02-01, 1987-02-03, 1987-02-04, 1987-02-06, 1987-02-08, 1987-02-14, 1987-02-17, 1987-02-18, 1987-02-20, 1987-02-22, 1987-02-24, 1987-02-26, 1987-02-28, 1987-03-01, 1987-03-04, 1987-03-06, 1987-03-08, 1987-03-10, 1987-03-13, 1987-03-17, 1987-03-20, 1987-03-22, 1987-03-26, 1987-03-28, 1987-03-29, 1987-04-01, 1987-04-03, 1987-04-05, 1987-12-06, 1987-12-08, 1987-12-11, 1987-12-12, 1987-12-15, 1987-12-17, 1987-12-20, 1987-12-23, 1987-12-26, 1987-12-28, 1987-12-29, 1987-12-31, 1988-01-02, 1988-01-04, 1988-01-06, 1988-01-07, 1988-01-09, 1988-01-12, 1988-01-13, 1988-01-15, 1988-01-17, 1988-01-19, 1988-01-22, 1988-01-24, 1988-01-26, 1988-01-29, 1988-01-30, 1988-02-02, 1988-02-03, 1988-02-05, 1988-02-11, 1988-02-13, 1988-02-14, 1988-02-17, 1988-02-19, 1988-02-21, 1988-02-23, 1988-02-24, 1988-02-26, 1988-02-28, 1988-03-01, 1988-03-03, 1988-03-06, 1988-03-08, 1988-03-10, 1988-03-12, 1988-03-13, 1988-03-16, 1988-03-18, 1988-03-22, 1988-03-25, 1988-03-26, 1988-03-29, 1988-04-01, 1990-03-04, 1990-03-09, 1990-03-11, 1990-03-13, 1990-03-15, 1990-03-17, 1990-03-18, 1990-03-20, 1990-03-23, 1990-03-25, 1990-03-27, 1990-03-31, 1990-10-04, 1990-10-06, 1990-10-09, 1990-10-12, 1990-10-14, 1990-10-17, 1990-10-19, 1990-10-21, 1990-10-23, 1990-10-25, 1990-10-27, 1990-10-30, 1990-11-01, 1990-11-03, 1990-11-06, 1990-11-08, 1990-11-09, 1990-11-11, 1990-11-14, 1990-11-16, 1990-11-19, 1990-11-21, 1990-11-23, 1990-11-24, 1990-11-27, 1990-11-29, 1990-12-02, 1990-12-04, 1990-12-05, 1990-12-07, 1990-12-10, 1990-12-12, 1990-12-14, 1990-12-16, 1990-12-18, 1990-12-20, 1990-12-22, 1990-12-23, 1990-12-27, 1990-12-28, 1990-12-31, 1991-01-02, 1991-01-03, 1991-01-05, 1991-01-08, 1991-01-10, 1991-01-12, 1991-01-16, 1991-01-23, 1991-01-25, 1991-01-26, 1991-01-28, 1991-01-30, 1991-01-31, 1991-02-02, 1991-02-05, 1991-02-07, 1991-02-08, 1991-02-10, 1991-02-14, 1991-02-16, 1991-02-18, 1991-02-20, 1991-02-21, 1991-02-23, 1991-02-25, 1991-02-27, 1991-03-01, 1991-03-03, 1991-03-05, 1991-03-07, 1991-03-09, 1991-03-10, 1991-03-13, 1991-03-16, 1991-03-17, 1991-03-20, 1991-03-22, 1991-03-26, 1991-03-28, 1991-10-27, 1991-10-29, 1991-11-01, 1991-11-03, 1991-11-05, 1991-11-07, 1991-11-10, 1991-11-12, 1991-11-14, 1991-11-16, 1991-11-19, 1991-11-21, 1991-11-22, 1991-11-26, 1991-11-29, 1991-12-01, 1991-12-03, 1991-12-04, 1991-12-07, 1991-12-10, 1991-12-12, 1991-12-14, 1991-12-17, 1991-12-19, 1991-12-22, 1991-12-27, 1991-12-28, 1991-12-31, 1992-01-03, 1992-01-04, 1992-01-07, 1992-01-12, 1992-01-14, 1992-01-15, 1992-01-21, 1992-01-23, 1992-01-25, 1992-01-28, 1992-01-30, 1992-02-01, 1992-02-04, 1992-02-06, 1992-02-10, 1992-02-12, 1992-02-13, 1992-02-15, 1992-02-17, 1992-02-19, 1992-02-21, 1992-02-23, 1992-02-25, 1992-02-28, 1992-03-01, 1992-03-02, 1992-03-05, 1992-03-07, 1992-03-08, 1992-03-12, 1992-03-14, 1992-03-18, 1992-03-20, 1992-03-22, 1992-03-24, 1992-03-26, 1992-03-28, 1992-03-29, 1992-04-12, 1992-04-14, 1992-04-16
    ## 10                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1987-10-08
    ## 11                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1989-03-22
    ## 12                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1979-10-09, 1979-10-12, 1979-10-14, 1979-10-17, 1979-10-18, 1979-10-20, 1979-10-21, 1979-10-24, 1979-10-27, 1979-10-31, 1979-11-02, 1979-11-04, 1979-11-06, 1979-11-10, 1979-11-11, 1979-11-13, 1979-11-14, 1979-11-16, 1979-11-18, 1979-11-20, 1979-11-23, 1979-11-24, 1979-11-28, 1979-11-30, 1979-12-02, 1979-12-04, 1979-12-05, 1979-12-08, 1979-12-09, 1979-12-11, 1979-12-14, 1979-12-15, 1979-12-19, 1979-12-21, 1979-12-22, 1979-12-28, 1979-12-29, 1980-01-03, 1980-01-04, 1980-01-06, 1980-01-08, 1980-01-09, 1980-01-11, 1980-01-12, 1980-01-16, 1980-01-18, 1980-01-22, 1980-01-23, 1980-01-27, 1980-01-29, 1980-02-02, 1980-02-03, 1980-02-07, 1980-02-09, 1980-02-12, 1980-02-16, 1980-02-17, 1980-02-19, 1980-02-22, 1980-02-23, 1980-02-26, 1980-02-29, 1980-03-01, 1980-03-04, 1980-03-05, 1980-03-07, 1980-03-09, 1980-03-10, 1980-03-13, 1980-03-15, 1980-03-16, 1980-03-19, 1980-03-21, 1980-03-23, 1980-03-25, 1980-03-28, 1980-03-30, 1980-04-01, 1980-04-03, 1980-04-05
    ## 13                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1970-11-04, 1970-11-05, 1970-11-07, 1970-11-10, 1970-11-11, 1970-11-14, 1970-11-15, 1970-11-17, 1970-11-20, 1970-11-21, 1970-11-24, 1970-11-26, 1970-11-28, 1970-11-29, 1970-12-01
    ## 14                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1972-01-05
    ## 15                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1994-01-05
    ## 16                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1970-10-09, 1970-10-11, 1970-10-12, 1970-10-14, 1970-10-15, 1970-10-18, 1970-10-20, 1970-10-23, 1970-10-25, 1970-10-27, 1970-10-31, 1970-11-04, 1970-11-05, 1970-11-07, 1970-11-10, 1970-11-11, 1970-11-14, 1970-11-15, 1970-11-17, 1970-11-20, 1970-11-21, 1970-11-24, 1970-11-26, 1970-11-28, 1970-11-29, 1970-12-01, 1970-12-05, 1970-12-06, 1970-12-08, 1970-12-09, 1970-12-12, 1970-12-15, 1970-12-18, 1970-12-20, 1970-12-23, 1970-12-26, 1970-12-30, 1971-01-02, 1971-01-06, 1971-01-07, 1971-01-09, 1971-01-12, 1971-01-16, 1971-01-17, 1971-01-20, 1971-01-23, 1971-01-24, 1971-01-26, 1971-01-29, 1971-01-31, 1971-02-02, 1971-02-06, 1971-02-09, 1971-02-12, 1971-02-14, 1971-02-16, 1971-02-19, 1971-02-22, 1971-02-25, 1971-02-27, 1971-02-28, 1971-03-03, 1971-03-06, 1971-03-07, 1971-03-09, 1971-03-11, 1971-03-13, 1971-03-16, 1971-03-19, 1971-03-21, 1971-03-23, 1971-03-25, 1971-03-26, 1971-03-28, 1971-03-30, 1971-03-31, 1971-04-02, 1971-04-04
    ## 17                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1974-12-03
    ## 18                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1972-10-07, 1972-10-11, 1972-10-12, 1972-10-14, 1972-10-17, 1972-10-19, 1972-10-21, 1972-10-22, 1972-10-24, 1972-10-28, 1972-10-31, 1972-11-03, 1972-11-05, 1972-11-08, 1972-11-11, 1972-11-12, 1972-11-14, 1972-11-17, 1972-11-19, 1972-11-21, 1972-11-22, 1972-11-24, 1972-11-26, 1972-11-28, 1972-12-01, 1972-12-05, 1972-12-07, 1972-12-09, 1972-12-10, 1972-12-12, 1972-12-15, 1972-12-16, 1972-12-20, 1972-12-21, 1972-12-23, 1972-12-26, 1972-12-29, 1972-12-30, 1973-01-01, 1973-01-03, 1973-01-06, 1973-01-07, 1973-01-09, 1973-01-12, 1973-01-14, 1973-01-16, 1973-01-19, 1973-01-20, 1973-01-24, 1973-01-26, 1973-01-27, 1973-02-01, 1973-02-03, 1973-02-04, 1973-02-06, 1973-02-09, 1973-02-11, 1973-02-13, 1973-02-14, 1973-02-16, 1973-02-17, 1973-02-20, 1973-02-22, 1973-02-24, 1973-02-28, 1973-03-03, 1973-03-04, 1973-03-09, 1973-03-10, 1973-03-14, 1973-03-16, 1973-03-17, 1973-03-21, 1973-03-22, 1973-03-25, 1973-03-27, 1973-03-30, 1973-03-31
    ## 19                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1979-12-05, 1980-12-19, 1981-01-13
    ## 20                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1972-01-22, 1973-01-16
    ## 21                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1991-11-22
    ## 22                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1988-04-01
    ## 23                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1999-04-07, 1999-04-14, 1999-12-06, 2000-03-22, 2000-10-18, 2001-01-18, 2001-02-18, 2002-03-10, 2002-11-29, 2003-01-17
    ## 24                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1986-10-11, 1986-10-13, 1986-10-15, 1986-10-16, 1986-10-18, 1986-10-22, 1986-10-24, 1986-10-26, 1986-10-28, 1986-10-31, 1986-11-02, 1986-11-04, 1986-11-05, 1986-11-07, 1986-11-08, 1986-11-11, 1986-11-12, 1986-11-14, 1986-11-18, 1986-11-21, 1986-11-22, 1986-11-25, 1986-11-26, 1986-11-29, 1986-12-02, 1986-12-05, 1986-12-07, 1986-12-09, 1986-12-11, 1986-12-13, 1986-12-14, 1986-12-17, 1986-12-19, 1986-12-20, 1986-12-23, 1986-12-27, 1986-12-30, 1987-01-02, 1987-01-04, 1987-01-06, 1987-01-07, 1987-01-10, 1987-01-11, 1987-01-14, 1987-01-16, 1987-01-17, 1987-01-19, 1987-01-21, 1987-01-23, 1987-01-27, 1987-01-28, 1987-01-30, 1987-02-01, 1987-02-03, 1987-02-04, 1987-02-06, 1987-02-08, 1987-02-14, 1987-02-17, 1987-02-18, 1987-02-20, 1987-02-22, 1987-02-24, 1987-02-26, 1987-02-28, 1987-03-01, 1987-03-04, 1987-03-06, 1987-03-08, 1987-03-10, 1987-03-13, 1987-03-17, 1987-03-20, 1987-03-22, 1987-03-26, 1987-03-28, 1987-03-29, 1987-04-01, 1987-04-03, 1987-04-05
    ## 25                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1992-03-18, 1992-03-20, 1992-03-22, 1992-03-24, 1992-03-26, 1992-03-28, 1992-03-29, 1992-04-12, 1992-04-14, 1992-04-16
    ## 26                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             2001-04-07, 2001-11-11
    ## 27                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1983-12-13
    ## 28                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1986-12-27, 1987-01-16, 1987-11-07, 1987-11-27, 1987-12-01, 1988-02-13, 1988-02-26, 1988-03-18, 1988-03-25, 1989-02-10, 1989-04-01, 1989-04-02, 1989-11-05, 1989-11-26, 1990-01-18
    ## 29                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         2004-03-10, 2004-03-12, 2004-03-13, 2004-03-16, 2004-03-18, 2004-03-19, 2004-03-21, 2004-03-24, 2004-03-27, 2004-03-29, 2004-03-31, 2004-04-02, 2004-04-03
    ## 30                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1988-03-03, 1988-03-10, 1989-03-24, 1989-03-29
    ## 31                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1972-01-29
    ## 32                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1970-10-09, 1970-10-11, 1970-10-12, 1970-10-14, 1970-10-15, 1970-10-18, 1970-10-20, 1970-10-23, 1970-10-25, 1970-10-27, 1970-10-31, 1970-11-04, 1970-11-05, 1970-11-07, 1970-11-10, 1970-11-11, 1970-11-14, 1970-11-15, 1970-11-17, 1970-11-20, 1970-11-21, 1970-11-24, 1970-11-26, 1970-11-28, 1970-11-29, 1970-12-01, 1970-12-05, 1970-12-06, 1970-12-08, 1970-12-09, 1970-12-12, 1970-12-15, 1970-12-18, 1970-12-20, 1970-12-23, 1970-12-26, 1970-12-30, 1971-01-02, 1971-01-06, 1971-01-07, 1971-01-09, 1971-01-12, 1971-01-16, 1971-01-17, 1971-01-20, 1971-01-23, 1971-01-24, 1971-01-26, 1971-01-29, 1971-01-31, 1971-02-02, 1971-02-06, 1971-02-09, 1971-02-12, 1971-02-14, 1971-02-16, 1971-02-19, 1971-02-22, 1971-02-25, 1971-02-27, 1971-02-28, 1971-03-03, 1971-03-06, 1971-03-07, 1971-03-09, 1971-03-11, 1971-03-13, 1971-03-16, 1971-03-19, 1971-03-21, 1971-03-23, 1971-03-25, 1971-03-26, 1971-03-28, 1971-03-30, 1971-03-31, 1971-04-02, 1971-04-04
    ## 33                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1976-10-06
    ##    mostGoalsOneGame mostGoalsOneSeason
    ## 1                 4                 41
    ## 2                 3                 29
    ## 3                 2                 16
    ## 4                 4                 60
    ## 5                 2                 11
    ## 6                 4                 48
    ## 7                 2                 12
    ## 8                 2                  8
    ## 9                 0                  0
    ## 10                4                 36
    ## 11                2                  4
    ## 12                0                  0
    ## 13                0                  0
    ## 14                2                 19
    ## 15                1                  1
    ## 16                0                  0
    ## 17                2                 11
    ## 18                0                  0
    ## 19                2                 18
    ## 20                3                 30
    ## 21                3                  5
    ## 22                2                  2
    ## 23                1                  3
    ## 24                0                  0
    ## 25                0                  0
    ## 26                1                  1
    ## 27                2                  7
    ## 28                1                  7
    ## 29                0                  0
    ## 30                1                  2
    ## 31                2                 14
    ## 32                0                  0
    ## 33                4                 28
    ##                                  mostGoalsSeasonIds
    ## 1                                          20102011
    ## 2                                          20092010
    ## 3                                          19931994
    ## 4                                19921993, 19931994
    ## 5                                          19992000
    ## 6                                          20022003
    ## 7                                19851986, 19861987
    ## 8                                          19771978
    ## 9  19861987, 19871988, 19891990, 19901991, 19911992
    ## 10                                         19871988
    ## 11                                         19881989
    ## 12                                         19791980
    ## 13                                         19701971
    ## 14                                         19711972
    ## 15                                         19931994
    ## 16                                         19701971
    ## 17                                         19741975
    ## 18                                         19721973
    ## 19                                         19801981
    ## 20                                         19721973
    ## 21                               19911992, 19961997
    ## 22                                         19871988
    ## 23                                         20002001
    ## 24                                         19861987
    ## 25                                         19911992
    ## 26                               20002001, 20012002
    ## 27                                         19831984
    ## 28                                         19871988
    ## 29                                         20032004
    ## 30                               19871988, 19881989
    ## 31                                         19711972
    ## 32                                         19701971
    ## 33                                         19761977
    ##    mostPenaltyMinutesOneSeason mostPenaltyMinutesSeasonIds
    ## 1                           50                    20072008
    ## 2                           66                    20062007
    ## 3                          371                    19961997
    ## 4                           86                    19931994
    ## 5                          372                    19971998
    ## 6                           74                    19981999
    ## 7                          105                    19871988
    ## 8                            6                    19771978
    ## 9                           81                    19901991
    ## 10                          30                    19871988
    ## 11                          35                    19881989
    ## 12                           0                    19791980
    ## 13                          12                    19701971
    ## 14                          22                    19721973
    ## 15                         190                    19931994
    ## 16                           0                    19701971
    ## 17                          70                    19721973
    ## 18                           6                    19721973
    ## 19                          57                    19801981
    ## 20                          46                    19741975
    ## 21                          63                    19911992
    ## 22                          48                    19871988
    ## 23                         115                    19981999
    ## 24                          14                    19861987
    ## 25                           0                    19911992
    ## 26                          21                    20002001
    ## 27                          24                    19831984
    ## 28                          58                    19871988
    ## 29                           2                    20032004
    ## 30                           6                    19871988
    ## 31                          12                    19711972
    ## 32                           0                    19701971
    ## 33                          54                    19791980
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       mostPointsGameDates
    ## 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              2007-02-06
    ## 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              2015-11-21
    ## 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1992-11-23, 1993-10-23
    ## 4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1992-10-12, 1996-12-15
    ## 5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              2000-12-08
    ## 6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              2003-02-25
    ## 7                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1986-01-31
    ## 8                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1978-01-28, 1978-03-15
    ## 9                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1987-12-15
    ## 10                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1987-10-08, 1987-11-27, 1988-02-14, 1988-10-12, 1992-11-16
    ## 11                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1989-03-22, 1989-04-01
    ## 12                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1979-12-19
    ## 13                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1970-11-04, 1970-11-05, 1970-11-07, 1970-11-10, 1970-11-11, 1970-11-14, 1970-11-15, 1970-11-17, 1970-11-20, 1970-11-21, 1970-11-24, 1970-11-26, 1970-11-28, 1970-11-29, 1970-12-01
    ## 14                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1972-01-19
    ## 15                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1993-11-03, 1994-01-05, 1994-01-09
    ## 16 1970-10-09, 1970-10-11, 1970-10-12, 1970-10-14, 1970-10-15, 1970-10-18, 1970-10-20, 1970-10-23, 1970-10-25, 1970-10-27, 1970-10-31, 1970-11-04, 1970-11-05, 1970-11-07, 1970-11-10, 1970-11-11, 1970-11-14, 1970-11-15, 1970-11-17, 1970-11-20, 1970-11-21, 1970-11-24, 1970-11-26, 1970-11-28, 1970-11-29, 1970-12-01, 1970-12-05, 1970-12-06, 1970-12-08, 1970-12-09, 1970-12-12, 1970-12-15, 1970-12-18, 1970-12-20, 1970-12-23, 1970-12-26, 1970-12-30, 1971-01-02, 1971-01-06, 1971-01-07, 1971-01-09, 1971-01-12, 1971-01-16, 1971-01-17, 1971-01-20, 1971-01-23, 1971-01-24, 1971-01-26, 1971-01-29, 1971-01-31, 1971-02-02, 1971-02-06, 1971-02-09, 1971-02-12, 1971-02-14, 1971-02-16, 1971-02-19, 1971-02-22, 1971-02-25, 1971-02-27, 1971-02-28, 1971-03-03, 1971-03-06, 1971-03-07, 1971-03-09, 1971-03-11, 1971-03-13, 1971-03-16, 1971-03-19, 1971-03-21, 1971-03-23, 1971-03-25, 1971-03-26, 1971-03-28, 1971-03-30, 1971-03-31, 1971-04-02, 1971-04-04
    ## 17                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1973-12-08, 1974-11-19, 1974-11-26, 1974-11-30, 1974-12-03, 1975-03-08, 1975-03-14, 1975-12-03
    ## 18 1972-10-07, 1972-10-11, 1972-10-12, 1972-10-14, 1972-10-17, 1972-10-19, 1972-10-21, 1972-10-22, 1972-10-24, 1972-10-28, 1972-10-31, 1972-11-03, 1972-11-05, 1972-11-08, 1972-11-11, 1972-11-12, 1972-11-14, 1972-11-17, 1972-11-19, 1972-11-21, 1972-11-22, 1972-11-24, 1972-11-26, 1972-11-28, 1972-12-01, 1972-12-05, 1972-12-07, 1972-12-09, 1972-12-10, 1972-12-12, 1972-12-15, 1972-12-16, 1972-12-20, 1972-12-21, 1972-12-23, 1972-12-26, 1972-12-29, 1972-12-30, 1973-01-01, 1973-01-03, 1973-01-06, 1973-01-07, 1973-01-09, 1973-01-12, 1973-01-14, 1973-01-16, 1973-01-19, 1973-01-20, 1973-01-24, 1973-01-26, 1973-01-27, 1973-02-01, 1973-02-03, 1973-02-04, 1973-02-06, 1973-02-09, 1973-02-11, 1973-02-13, 1973-02-14, 1973-02-16, 1973-02-17, 1973-02-20, 1973-02-22, 1973-02-24, 1973-02-28, 1973-03-03, 1973-03-04, 1973-03-09, 1973-03-10, 1973-03-14, 1973-03-16, 1973-03-17, 1973-03-21, 1973-03-22, 1973-03-25, 1973-03-27, 1973-03-30, 1973-03-31
    ## 19                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1980-03-10
    ## 20                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1973-02-09, 1973-10-10, 1975-02-14
    ## 21                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1991-11-22
    ## 22                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1988-04-01
    ## 23                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             2000-02-19
    ## 24                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1986-11-25
    ## 25                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1992-03-18, 1992-03-20, 1992-03-22, 1992-03-24, 1992-03-26, 1992-03-28, 1992-03-29, 1992-04-12, 1992-04-14, 1992-04-16
    ## 26                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             2001-03-16, 2001-04-07, 2001-11-11, 2001-12-08, 2001-12-13
    ## 27                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1982-02-28
    ## 28                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1988-02-21, 1990-02-28
    ## 29                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 2004-03-29, 2004-04-03
    ## 30                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1988-03-01
    ## 31                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1972-01-29
    ## 32 1970-10-09, 1970-10-11, 1970-10-12, 1970-10-14, 1970-10-15, 1970-10-18, 1970-10-20, 1970-10-23, 1970-10-25, 1970-10-27, 1970-10-31, 1970-11-04, 1970-11-05, 1970-11-07, 1970-11-10, 1970-11-11, 1970-11-14, 1970-11-15, 1970-11-17, 1970-11-20, 1970-11-21, 1970-11-24, 1970-11-26, 1970-11-28, 1970-11-29, 1970-12-01, 1970-12-05, 1970-12-06, 1970-12-08, 1970-12-09, 1970-12-12, 1970-12-15, 1970-12-18, 1970-12-20, 1970-12-23, 1970-12-26, 1970-12-30, 1971-01-02, 1971-01-06, 1971-01-07, 1971-01-09, 1971-01-12, 1971-01-16, 1971-01-17, 1971-01-20, 1971-01-23, 1971-01-24, 1971-01-26, 1971-01-29, 1971-01-31, 1971-02-02, 1971-02-06, 1971-02-09, 1971-02-12, 1971-02-14, 1971-02-16, 1971-02-19, 1971-02-22, 1971-02-25, 1971-02-27, 1971-02-28, 1971-03-03, 1971-03-06, 1971-03-07, 1971-03-09, 1971-03-11, 1971-03-13, 1971-03-16, 1971-03-19, 1971-03-21, 1971-03-23, 1971-03-25, 1971-03-26, 1971-03-28, 1971-03-30, 1971-03-31, 1971-04-02, 1971-04-04
    ## 33                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1976-10-06, 1977-12-23
    ##    mostPointsOneGame mostPointsOneSeason
    ## 1                  5                 104
    ## 2                  5                 112
    ## 3                  3                  29
    ## 4                  5                 110
    ## 5                  3                  28
    ## 6                  6                 104
    ## 7                  4                  63
    ## 8                  4                  26
    ## 9                  1                   1
    ## 10                 4                  76
    ## 11                 2                   6
    ## 12                 1                   1
    ## 13                 0                   0
    ## 14                 3                  38
    ## 15                 1                   3
    ## 16                 0                   0
    ## 17                 2                  23
    ## 18                 0                   0
    ## 19                 4                  29
    ## 20                 4                  78
    ## 21                 3                  32
    ## 22                 2                   2
    ## 23                 2                  12
    ## 24                 1                   1
    ## 25                 0                   0
    ## 26                 1                   3
    ## 27                 3                  20
    ## 28                 3                  33
    ## 29                 1                   2
    ## 30                 2                   5
    ## 31                 3                  34
    ## 32                 0                   0
    ## 33                 4                  68
    ##    mostPointsSeasonIds penaltyMinutes playerId points
    ## 1             20102011            546  8467875   1041
    ## 2             20092010            680  8467876   1070
    ## 3             19931994           2127  8449961     98
    ## 4             19921993            328  8455738    478
    ## 5             20002001           1159  8459246    103
    ## 6             20022003            614  8458530    756
    ## 7             19861987            526  8448839    307
    ## 8             19771978              6  8444869     26
    ## 9             19871988            189  8444893      1
    ## 10            19871988            154  8444894    369
    ## 11            19881989             35  8444898      6
    ## 12            19791980              0  8444905      1
    ## 13            19701971             12  8444966      0
    ## 14            19711972             43  8445007     43
    ## 15            19931994            265  8445017      3
    ## 16            19701971              0  8445100      0
    ## 17            19741975            263  8445104     67
    ## 18            19721973              6  8445114      0
    ## 19            19801981             68  8445118     48
    ## 20            19741975            140  8445137    388
    ## 21            19931994            290  8445208    154
    ## 22            19871988             48  8445232      2
    ## 23            19992000            375  8445266     44
    ## 24            19861987             14  8445277      1
    ## 25            19911992              0  8445335      0
    ## 26            20012002             39  8445413      5
    ## 27            19831984             54  8445415     44
    ## 28            19871988            172  8445416     70
    ## 29            20032004              2  8445428      2
    ## 30            19871988             11  8445455      8
    ## 31            19711972             12  8445521     34
    ## 32            19701971              0  8445530      0
    ## 33            19761977            168  8445558    221
    ##    positionCode rookiePoints seasons
    ## 1             L           34      17
    ## 2             C           29      17
    ## 3             L            8       8
    ## 4             R           60       7
    ## 5             L           NA       6
    ## 6             L           NA      12
    ## 7             D           30      10
    ## 8             D           NA       1
    ## 9             D            1       5
    ## 10            L           NA       8
    ## 11            L           NA       1
    ## 12            D           NA       1
    ## 13            D            0       1
    ## 14            L           NA       2
    ## 15            L            3       5
    ## 16            D            0       1
    ## 17            D            7       5
    ## 18            D            0       1
    ## 19            L           19       2
    ## 20            L           NA       6
    ## 21            D           NA       7
    ## 22            R            2       1
    ## 23            D           NA       5
    ## 24            D            1       1
    ## 25            C            0       1
    ## 26            D           NA       2
    ## 27            D            9       5
    ## 28            D           NA       4
    ## 29            D           NA       1
    ## 30            L           NA       2
    ## 31            C           NA       1
    ## 32            R            0       1
    ## 33            R           56       6
    ##  [ reached 'max' / getOption("max.print") -- omitted 528 rows ]

``` r
nhlFun(endpoints = "stats", expand = "person.team")
```

    ##    id                  name             link abbreviation
    ## 1   1     New Jersey Devils  /api/v1/teams/1          NJD
    ## 2   2    New York Islanders  /api/v1/teams/2          NYI
    ## 3   3      New York Rangers  /api/v1/teams/3          NYR
    ## 4   4   Philadelphia Flyers  /api/v1/teams/4          PHI
    ## 5   5   Pittsburgh Penguins  /api/v1/teams/5          PIT
    ## 6   6         Boston Bruins  /api/v1/teams/6          BOS
    ## 7   7        Buffalo Sabres  /api/v1/teams/7          BUF
    ## 8   8    Montréal Canadiens  /api/v1/teams/8          MTL
    ## 9   9       Ottawa Senators  /api/v1/teams/9          OTT
    ## 10 10   Toronto Maple Leafs /api/v1/teams/10          TOR
    ## 11 12   Carolina Hurricanes /api/v1/teams/12          CAR
    ## 12 13      Florida Panthers /api/v1/teams/13          FLA
    ## 13 14   Tampa Bay Lightning /api/v1/teams/14          TBL
    ## 14 15   Washington Capitals /api/v1/teams/15          WSH
    ## 15 16    Chicago Blackhawks /api/v1/teams/16          CHI
    ## 16 17     Detroit Red Wings /api/v1/teams/17          DET
    ## 17 18   Nashville Predators /api/v1/teams/18          NSH
    ## 18 19       St. Louis Blues /api/v1/teams/19          STL
    ## 19 20        Calgary Flames /api/v1/teams/20          CGY
    ## 20 21    Colorado Avalanche /api/v1/teams/21          COL
    ## 21 22       Edmonton Oilers /api/v1/teams/22          EDM
    ## 22 23     Vancouver Canucks /api/v1/teams/23          VAN
    ## 23 24         Anaheim Ducks /api/v1/teams/24          ANA
    ## 24 25          Dallas Stars /api/v1/teams/25          DAL
    ## 25 26     Los Angeles Kings /api/v1/teams/26          LAK
    ## 26 28       San Jose Sharks /api/v1/teams/28          SJS
    ## 27 29 Columbus Blue Jackets /api/v1/teams/29          CBJ
    ## 28 30        Minnesota Wild /api/v1/teams/30          MIN
    ## 29 52         Winnipeg Jets /api/v1/teams/52          WPG
    ## 30 53       Arizona Coyotes /api/v1/teams/53          ARI
    ## 31 54  Vegas Golden Knights /api/v1/teams/54          VGK
    ##          teamName locationName firstYearOfPlay    shortName
    ## 1          Devils   New Jersey            1982   New Jersey
    ## 2       Islanders     New York            1972 NY Islanders
    ## 3         Rangers     New York            1926   NY Rangers
    ## 4          Flyers Philadelphia            1967 Philadelphia
    ## 5        Penguins   Pittsburgh            1967   Pittsburgh
    ## 6          Bruins       Boston            1924       Boston
    ## 7          Sabres      Buffalo            1970      Buffalo
    ## 8       Canadiens     Montréal            1909     Montréal
    ## 9        Senators       Ottawa            1990       Ottawa
    ## 10    Maple Leafs      Toronto            1917      Toronto
    ## 11     Hurricanes     Carolina            1979     Carolina
    ## 12       Panthers      Florida            1993      Florida
    ## 13      Lightning    Tampa Bay            1991    Tampa Bay
    ## 14       Capitals   Washington            1974   Washington
    ## 15     Blackhawks      Chicago            1926      Chicago
    ## 16      Red Wings      Detroit            1926      Detroit
    ## 17      Predators    Nashville            1997    Nashville
    ## 18          Blues    St. Louis            1967     St Louis
    ## 19         Flames      Calgary            1980      Calgary
    ## 20      Avalanche     Colorado            1979     Colorado
    ## 21         Oilers     Edmonton            1979     Edmonton
    ## 22        Canucks    Vancouver            1970    Vancouver
    ## 23          Ducks      Anaheim            1993      Anaheim
    ## 24          Stars       Dallas            1967       Dallas
    ## 25          Kings  Los Angeles            1967  Los Angeles
    ## 26         Sharks     San Jose            1990     San Jose
    ## 27   Blue Jackets     Columbus            1997     Columbus
    ## 28           Wild    Minnesota            1997    Minnesota
    ## 29           Jets     Winnipeg            2011     Winnipeg
    ## 30        Coyotes      Arizona            1979      Arizona
    ## 31 Golden Knights        Vegas            2016        Vegas
    ##                       officialSiteUrl franchiseId active
    ## 1     http://www.newjerseydevils.com/          23   TRUE
    ## 2    http://www.newyorkislanders.com/          22   TRUE
    ## 3      http://www.newyorkrangers.com/          10   TRUE
    ## 4  http://www.philadelphiaflyers.com/          16   TRUE
    ## 5      http://pittsburghpenguins.com/          17   TRUE
    ## 6        http://www.bostonbruins.com/           6   TRUE
    ## 7              http://www.sabres.com/          19   TRUE
    ## 8           http://www.canadiens.com/           1   TRUE
    ## 9      http://www.ottawasenators.com/          30   TRUE
    ## 10         http://www.mapleleafs.com/           5   TRUE
    ## 11 http://www.carolinahurricanes.com/          26   TRUE
    ## 12    http://www.floridapanthers.com/          33   TRUE
    ## 13  http://www.tampabaylightning.com/          31   TRUE
    ## 14 http://www.washingtoncapitals.com/          24   TRUE
    ## 15  http://www.chicagoblackhawks.com/          11   TRUE
    ## 16    http://www.detroitredwings.com/          12   TRUE
    ## 17 http://www.nashvillepredators.com/          34   TRUE
    ## 18       http://www.stlouisblues.com/          18   TRUE
    ## 19      http://www.calgaryflames.com/          21   TRUE
    ## 20  http://www.coloradoavalanche.com/          27   TRUE
    ## 21     http://www.edmontonoilers.com/          25   TRUE
    ## 22            http://www.canucks.com/          20   TRUE
    ## 23       http://www.anaheimducks.com/          32   TRUE
    ## 24        http://www.dallasstars.com/          15   TRUE
    ## 25            http://www.lakings.com/          14   TRUE
    ## 26           http://www.sjsharks.com/          29   TRUE
    ## 27        http://www.bluejackets.com/          36   TRUE
    ## 28               http://www.wild.com/          37   TRUE
    ## 29           http://winnipegjets.com/          35   TRUE
    ## 30     http://www.arizonacoyotes.com/          28   TRUE
    ## 31 http://www.vegasgoldenknights.com/          38   TRUE
    ##                  venue.name          venue.link
    ## 1         Prudential Center /api/v1/venues/null
    ## 2           Barclays Center /api/v1/venues/5026
    ## 3     Madison Square Garden /api/v1/venues/5054
    ## 4        Wells Fargo Center /api/v1/venues/5096
    ## 5          PPG Paints Arena /api/v1/venues/5034
    ## 6                 TD Garden /api/v1/venues/5085
    ## 7            KeyBank Center /api/v1/venues/5039
    ## 8               Bell Centre /api/v1/venues/5028
    ## 9      Canadian Tire Centre /api/v1/venues/5031
    ## 10         Scotiabank Arena /api/v1/venues/null
    ## 11                PNC Arena /api/v1/venues/5066
    ## 12              BB&T Center /api/v1/venues/5027
    ## 13             AMALIE Arena /api/v1/venues/null
    ## 14        Capital One Arena /api/v1/venues/5094
    ## 15            United Center /api/v1/venues/5092
    ## 16     Little Caesars Arena /api/v1/venues/5145
    ## 17        Bridgestone Arena /api/v1/venues/5030
    ## 18        Enterprise Center /api/v1/venues/5076
    ## 19    Scotiabank Saddledome /api/v1/venues/5075
    ## 20             Pepsi Center /api/v1/venues/5064
    ## 21             Rogers Place /api/v1/venues/5100
    ## 22             Rogers Arena /api/v1/venues/5073
    ## 23             Honda Center /api/v1/venues/5046
    ## 24 American Airlines Center /api/v1/venues/5019
    ## 25           STAPLES Center /api/v1/venues/5081
    ## 26   SAP Center at San Jose /api/v1/venues/null
    ## 27         Nationwide Arena /api/v1/venues/5059
    ## 28       Xcel Energy Center /api/v1/venues/5098
    ## 29           Bell MTS Place /api/v1/venues/5058
    ## 30         Gila River Arena /api/v1/venues/5043
    ## 31           T-Mobile Arena /api/v1/venues/5178
    ##      venue.city venue.id   venue.timeZone.id
    ## 1        Newark       NA    America/New_York
    ## 2      Brooklyn     5026    America/New_York
    ## 3      New York     5054    America/New_York
    ## 4  Philadelphia     5096    America/New_York
    ## 5    Pittsburgh     5034    America/New_York
    ## 6        Boston     5085    America/New_York
    ## 7       Buffalo     5039    America/New_York
    ## 8      Montréal     5028    America/Montreal
    ## 9        Ottawa     5031    America/New_York
    ## 10      Toronto       NA     America/Toronto
    ## 11      Raleigh     5066    America/New_York
    ## 12      Sunrise     5027    America/New_York
    ## 13        Tampa       NA    America/New_York
    ## 14   Washington     5094    America/New_York
    ## 15      Chicago     5092     America/Chicago
    ## 16      Detroit     5145     America/Detroit
    ## 17    Nashville     5030     America/Chicago
    ## 18    St. Louis     5076     America/Chicago
    ## 19      Calgary     5075      America/Denver
    ## 20       Denver     5064      America/Denver
    ## 21     Edmonton     5100    America/Edmonton
    ## 22    Vancouver     5073   America/Vancouver
    ## 23      Anaheim     5046 America/Los_Angeles
    ## 24       Dallas     5019     America/Chicago
    ## 25  Los Angeles     5081 America/Los_Angeles
    ## 26     San Jose       NA America/Los_Angeles
    ## 27     Columbus     5059    America/New_York
    ## 28     St. Paul     5098     America/Chicago
    ## 29     Winnipeg     5058    America/Winnipeg
    ## 30     Glendale     5043     America/Phoenix
    ## 31    Las Vegas     5178 America/Los_Angeles
    ##    venue.timeZone.offset venue.timeZone.tz division.id
    ## 1                     -4               EDT          18
    ## 2                     -4               EDT          18
    ## 3                     -4               EDT          18
    ## 4                     -4               EDT          18
    ## 5                     -4               EDT          18
    ## 6                     -4               EDT          17
    ## 7                     -4               EDT          17
    ## 8                     -4               EDT          17
    ## 9                     -4               EDT          17
    ## 10                    -4               EDT          17
    ## 11                    -4               EDT          18
    ## 12                    -4               EDT          17
    ## 13                    -4               EDT          17
    ## 14                    -4               EDT          18
    ## 15                    -5               CDT          16
    ## 16                    -4               EDT          17
    ## 17                    -5               CDT          16
    ## 18                    -5               CDT          16
    ## 19                    -6               MDT          15
    ## 20                    -6               MDT          16
    ## 21                    -6               MDT          15
    ## 22                    -7               PDT          15
    ## 23                    -7               PDT          15
    ## 24                    -5               CDT          16
    ## 25                    -7               PDT          15
    ## 26                    -7               PDT          15
    ## 27                    -4               EDT          18
    ## 28                    -5               CDT          16
    ## 29                    -5               CDT          16
    ## 30                    -7               MST          15
    ## 31                    -7               PDT          15
    ##    division.name division.nameShort        division.link
    ## 1   Metropolitan              Metro /api/v1/divisions/18
    ## 2   Metropolitan              Metro /api/v1/divisions/18
    ## 3   Metropolitan              Metro /api/v1/divisions/18
    ## 4   Metropolitan              Metro /api/v1/divisions/18
    ## 5   Metropolitan              Metro /api/v1/divisions/18
    ## 6       Atlantic                ATL /api/v1/divisions/17
    ## 7       Atlantic                ATL /api/v1/divisions/17
    ## 8       Atlantic                ATL /api/v1/divisions/17
    ## 9       Atlantic                ATL /api/v1/divisions/17
    ## 10      Atlantic                ATL /api/v1/divisions/17
    ## 11  Metropolitan              Metro /api/v1/divisions/18
    ## 12      Atlantic                ATL /api/v1/divisions/17
    ## 13      Atlantic                ATL /api/v1/divisions/17
    ## 14  Metropolitan              Metro /api/v1/divisions/18
    ## 15       Central                CEN /api/v1/divisions/16
    ## 16      Atlantic                ATL /api/v1/divisions/17
    ## 17       Central                CEN /api/v1/divisions/16
    ## 18       Central                CEN /api/v1/divisions/16
    ## 19       Pacific                PAC /api/v1/divisions/15
    ## 20       Central                CEN /api/v1/divisions/16
    ## 21       Pacific                PAC /api/v1/divisions/15
    ## 22       Pacific                PAC /api/v1/divisions/15
    ## 23       Pacific                PAC /api/v1/divisions/15
    ## 24       Central                CEN /api/v1/divisions/16
    ## 25       Pacific                PAC /api/v1/divisions/15
    ## 26       Pacific                PAC /api/v1/divisions/15
    ## 27  Metropolitan              Metro /api/v1/divisions/18
    ## 28       Central                CEN /api/v1/divisions/16
    ## 29       Central                CEN /api/v1/divisions/16
    ## 30       Pacific                PAC /api/v1/divisions/15
    ## 31       Pacific                PAC /api/v1/divisions/15
    ##    division.abbreviation conference.id conference.name
    ## 1                      M             6         Eastern
    ## 2                      M             6         Eastern
    ## 3                      M             6         Eastern
    ## 4                      M             6         Eastern
    ## 5                      M             6         Eastern
    ## 6                      A             6         Eastern
    ## 7                      A             6         Eastern
    ## 8                      A             6         Eastern
    ## 9                      A             6         Eastern
    ## 10                     A             6         Eastern
    ## 11                     M             6         Eastern
    ## 12                     A             6         Eastern
    ## 13                     A             6         Eastern
    ## 14                     M             6         Eastern
    ## 15                     C             5         Western
    ## 16                     A             6         Eastern
    ## 17                     C             5         Western
    ## 18                     C             5         Western
    ## 19                     P             5         Western
    ## 20                     C             5         Western
    ## 21                     P             5         Western
    ## 22                     P             5         Western
    ## 23                     P             5         Western
    ## 24                     C             5         Western
    ## 25                     P             5         Western
    ## 26                     P             5         Western
    ## 27                     M             6         Eastern
    ## 28                     C             5         Western
    ## 29                     C             5         Western
    ## 30                     P             5         Western
    ## 31                     P             5         Western
    ##          conference.link franchise.franchiseId
    ## 1  /api/v1/conferences/6                    23
    ## 2  /api/v1/conferences/6                    22
    ## 3  /api/v1/conferences/6                    10
    ## 4  /api/v1/conferences/6                    16
    ## 5  /api/v1/conferences/6                    17
    ## 6  /api/v1/conferences/6                     6
    ## 7  /api/v1/conferences/6                    19
    ## 8  /api/v1/conferences/6                     1
    ## 9  /api/v1/conferences/6                    30
    ## 10 /api/v1/conferences/6                     5
    ## 11 /api/v1/conferences/6                    26
    ## 12 /api/v1/conferences/6                    33
    ## 13 /api/v1/conferences/6                    31
    ## 14 /api/v1/conferences/6                    24
    ## 15 /api/v1/conferences/5                    11
    ## 16 /api/v1/conferences/6                    12
    ## 17 /api/v1/conferences/5                    34
    ## 18 /api/v1/conferences/5                    18
    ## 19 /api/v1/conferences/5                    21
    ## 20 /api/v1/conferences/5                    27
    ## 21 /api/v1/conferences/5                    25
    ## 22 /api/v1/conferences/5                    20
    ## 23 /api/v1/conferences/5                    32
    ## 24 /api/v1/conferences/5                    15
    ## 25 /api/v1/conferences/5                    14
    ## 26 /api/v1/conferences/5                    29
    ## 27 /api/v1/conferences/6                    36
    ## 28 /api/v1/conferences/5                    37
    ## 29 /api/v1/conferences/5                    35
    ## 30 /api/v1/conferences/5                    28
    ## 31 /api/v1/conferences/5                    38
    ##    franchise.teamName        franchise.link
    ## 1              Devils /api/v1/franchises/23
    ## 2           Islanders /api/v1/franchises/22
    ## 3             Rangers /api/v1/franchises/10
    ## 4              Flyers /api/v1/franchises/16
    ## 5            Penguins /api/v1/franchises/17
    ## 6              Bruins  /api/v1/franchises/6
    ## 7              Sabres /api/v1/franchises/19
    ## 8           Canadiens  /api/v1/franchises/1
    ## 9            Senators /api/v1/franchises/30
    ## 10        Maple Leafs  /api/v1/franchises/5
    ## 11         Hurricanes /api/v1/franchises/26
    ## 12           Panthers /api/v1/franchises/33
    ## 13          Lightning /api/v1/franchises/31
    ## 14           Capitals /api/v1/franchises/24
    ## 15         Blackhawks /api/v1/franchises/11
    ## 16          Red Wings /api/v1/franchises/12
    ## 17          Predators /api/v1/franchises/34
    ## 18              Blues /api/v1/franchises/18
    ## 19             Flames /api/v1/franchises/21
    ## 20          Avalanche /api/v1/franchises/27
    ## 21             Oilers /api/v1/franchises/25
    ## 22            Canucks /api/v1/franchises/20
    ## 23              Ducks /api/v1/franchises/32
    ## 24              Stars /api/v1/franchises/15
    ## 25              Kings /api/v1/franchises/14
    ## 26             Sharks /api/v1/franchises/29
    ## 27       Blue Jackets /api/v1/franchises/36
    ## 28               Wild /api/v1/franchises/37
    ## 29               Jets /api/v1/franchises/35
    ## 30            Coyotes /api/v1/franchises/28
    ## 31     Golden Knights /api/v1/franchises/38

``` r
# nhlFun(endpoints = "team total")
```

## Exploratory Data Analysis

### Retrieve Information

Now we demonstrate how to use functions above to do exploratory data
analysis. We will first retrieve data from two endpoints: **team total**
and **person.names**, remove some columns we don’t need, and combine the
two objects. Then we’ll add two new variables: `winPercent`: the
proportion of wins among all games played and `homeWinPercent`: the
proportion of wins of home games among all wins.

``` r
# perhaps use the wrapper function here
franTot <- nhlFun(endpoint = "team total")
```

    ## No encoding supplied: defaulting to UTF-8.

``` r
franTot <- franTot %>% select(-c("id", "activeFranchise", "firstSeasonId", "gameTypeId", "lastSeasonId"))
franStats <- nhlFun(endpoint = "stats", expand = "person.names") 
franStats <- franStats %>% select(c("locationName", "firstYearOfPlay", "franchiseId", "venue.city", "venue.timeZone.id", "venue.timeZone.tz", "division.name", "conference.name"))
# create two variables: winPercent and homeWinPercent 
combined <- full_join(franTot, franStats, by = "franchiseId") %>% mutate(winPercent = wins / gamesPlayed, homeWinPercent = homeWins / wins)
head(combined)
```

    ##   franchiseId gamesPlayed goalsAgainst goalsFor homeLosses
    ## 1          23        2937         8708     8647        507
    ## 2          23         257          634      697         53
    ## 3          22        3732        11779    11889        674
    ## 4          22         291          850      931         48
    ## 5          10        6504        19863    19864       1132
    ## 6          10         518         1447     1404        104
    ##   homeOvertimeLosses homeTies homeWins losses
    ## 1                 82       96      783   1181
    ## 2                  0       NA       74    120
    ## 3                 81      170      942   1570
    ## 4                  1       NA       90    131
    ## 5                 73      448     1600   2693
    ## 6                  0        1      137    266
    ##   overtimeLosses penaltyMinutes pointPctg points roadLosses
    ## 1            162          44397    0.5330   3131        674
    ## 2              0           4266    0.0039      2         67
    ## 3            159          57422    0.5115   3818        896
    ## 4              0           5540    0.0137      8         83
    ## 5            147          85564    0.5125   6667       1561
    ## 6              0           8181    0.0000      0        162
    ##   roadOvertimeLosses roadTies roadWins shootoutLosses
    ## 1                 80      123      592             79
    ## 2                  0       NA       63              0
    ## 3                 78      177      714             67
    ## 4                  0       NA       70              0
    ## 5                 74      360     1256             66
    ## 6                  0        7      107              0
    ##   shootoutWins shutouts teamId           teamName ties
    ## 1           78      193      1  New Jersey Devils  219
    ## 2            0       25      1  New Jersey Devils   NA
    ## 3           82      167      2 New York Islanders  347
    ## 4            0       12      2 New York Islanders   NA
    ## 5           78      403      3   New York Rangers  808
    ## 6            0       44      3   New York Rangers    8
    ##   triCode wins locationName firstYearOfPlay venue.city
    ## 1     NJD 1375   New Jersey            1982     Newark
    ## 2     NJD  137   New Jersey            1982     Newark
    ## 3     NYI 1656     New York            1972   Brooklyn
    ## 4     NYI  160     New York            1972   Brooklyn
    ## 5     NYR 2856     New York            1926   New York
    ## 6     NYR  244     New York            1926   New York
    ##   venue.timeZone.id venue.timeZone.tz division.name
    ## 1  America/New_York               EDT  Metropolitan
    ## 2  America/New_York               EDT  Metropolitan
    ## 3  America/New_York               EDT  Metropolitan
    ## 4  America/New_York               EDT  Metropolitan
    ## 5  America/New_York               EDT  Metropolitan
    ## 6  America/New_York               EDT  Metropolitan
    ##   conference.name winPercent homeWinPercent
    ## 1         Eastern  0.4681648      0.5694545
    ## 2         Eastern  0.5330739      0.5401460
    ## 3         Eastern  0.4437299      0.5688406
    ## 4         Eastern  0.5498282      0.5625000
    ## 5         Eastern  0.4391144      0.5602241
    ## 6         Eastern  0.4710425      0.5614754

``` r
# create a subset for numbers of games lost or won
subset <- combined %>% select(starts_with("home"), starts_with("road"), "division.name", -"homeWinPercent") %>% gather(-"division.name", key = "type", value = "game") %>% group_by(division.name, type) %>% summarise(sum = sum(game, na.rm = TRUE))
```

    ## `summarise()` regrouping output by 'division.name' (override with `.groups` argument)

### Summaries

``` r
# summaries
apply(combined[,c(2, 5:11, 13:17)], FUN = summary, MARGIN = 2)
```

    ## $gamesPlayed
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##       2      77     290    1188    1675    6731 
    ## 
    ## $homeLosses
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     0.0    17.0    68.0   205.1   297.0  1132.0 
    ## 
    ## $homeOvertimeLosses
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##    0.00    0.00    7.00   35.70   73.75  112.00      39 
    ## 
    ## $homeTies
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##     0.0     3.0    45.0    84.6   103.2   448.0      37 
    ## 
    ## $homeWins
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     0.0    16.0    78.0   311.8   433.0  2025.0 
    ## 
    ## $losses
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     1.0    43.0   144.0   493.2   709.0  2736.0 
    ## 
    ## $overtimeLosses
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##    0.00    0.00   11.50   73.18  158.00  203.00      39 
    ## 
    ## $penaltyMinutes
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##      12    1042    5118   17576   27013   91941 
    ## 
    ## $points
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##       0       0      39    1162    1838    7899       1 
    ## 
    ## $roadLosses
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     1.0    27.0    83.0   288.1   392.0  1619.0 
    ## 
    ## $roadOvertimeLosses
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##    0.00    0.00    6.50   37.74   78.75   95.00      39 
    ## 
    ## $roadTies
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##    0.00    3.00   33.50   84.62  126.25  456.00      37 
    ## 
    ## $roadWins
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     0.0    12.0    63.0   227.5   316.0  1424.0

``` r
# contingency table
table(combined$division.name, combined$firstYearOfPlay)
```

    ##               
    ##                1909 1917 1924 1926 1967 1970 1972 1974 1979
    ##   Atlantic        2    6    2    6    0    2    0    0    0
    ##   Central         0    0    0    2    6    0    0    0    4
    ##   Metropolitan    0    0    0    2    4    0    2    2    4
    ##   Pacific         0    0    0    0    2    2    0    0    8
    ##               
    ##                1980 1982 1990 1991 1993 1997 2011 2016
    ##   Atlantic        0    0    2    2    2    0    0    0
    ##   Central         0    0    0    0    0    4    4    0
    ##   Metropolitan    0    5    0    0    0    2    0    0
    ##   Pacific         4    0    2    0    2    0    0    2

``` r
table(combined$conference.name, combined$firstYearOfPlay)
```

    ##          
    ##           1909 1917 1924 1926 1967 1970 1972 1974 1979 1980
    ##   Eastern    2    6    2    8    4    2    2    2    4    0
    ##   Western    0    0    0    2    8    2    0    0   12    4
    ##          
    ##           1982 1990 1991 1993 1997 2011 2016
    ##   Eastern    5    2    2    2    2    0    0
    ##   Western    0    2    0    2    4    4    2

### Visualize Data

Now we have the data, we can make some plots to visualize the data.

``` r
# scatter plot of homeWins and roadWins
ggplot(combined, aes(x = homeWins, y = roadWins)) + geom_point(aes(color = division.name))
```

![](README_files/figure-gfm/unnamed-chunk-29-1.png)<!-- -->

``` r
# histogram of winPercent
ggplot(combined, aes(x = winPercent)) + geom_histogram(aes(y = ..density..))
```

    ## `stat_bin()` using `bins = 30`. Pick better value with
    ## `binwidth`.

![](README_files/figure-gfm/unnamed-chunk-29-2.png)<!-- -->

``` r
# boxplots of gamesPlayed by division
ggplot(combined, aes(x = division.name, y = gamesPlayed)) + geom_boxplot() + geom_jitter(aes(color = venue.timeZone.tz))
```

![](README_files/figure-gfm/unnamed-chunk-29-3.png)<!-- -->

``` r
# barplot of gamePlayed
# ggplot(combined, aes(x = gamesPlayed)) + geom_bar(aes(x = division.name))
ggplot(subset, aes(y = sum, fill = type)) + geom_bar(position = "stack", stat = "identity", aes(x = division.name))
```

![](README_files/figure-gfm/unnamed-chunk-29-4.png)<!-- -->

``` r
ggplot(combined, aes(x = penaltyMinutes)) + geom_freqpoly()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with
    ## `binwidth`.

![](README_files/figure-gfm/unnamed-chunk-29-5.png)<!-- -->

``` r
# roster <- nhlFun(endpoint = "stats", teamID = 20, expand = "team.roster")
# teamStats <- nhlFun(endpoint = "stats", teamID = 20, expand = "team.stats")
```
