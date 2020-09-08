Vignette
================

  - [Required Packages](#required-packages)
  - [Functions](#functions)
      - [NHL records API](#nhl-records-api)
      - [NHL stats API](#nhl-stats-api)
  - [A wrapper function for all the functions
    above](#a-wrapper-function-for-all-the-functions-above)
  - [Exploratory Data Analysis](#exploratory-data-analysis)

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
  return(fran)
}
getFran() %>% head()
```

    ## No encoding supplied: defaulting to UTF-8.

    ## $data
    ##    id firstSeasonId lastSeasonId mostRecentTeamId
    ## 1   1      19171918           NA                8
    ## 2   2      19171918     19171918               41
    ## 3   3      19171918     19341935               45
    ## 4   4      19191920     19241925               37
    ## 5   5      19171918           NA               10
    ## 6   6      19241925           NA                6
    ## 7   7      19241925     19371938               43
    ## 8   8      19251926     19411942               51
    ## 9   9      19251926     19301931               39
    ## 10 10      19261927           NA                3
    ## 11 11      19261927           NA               16
    ## 12 12      19261927           NA               17
    ## 13 13      19671968     19771978               49
    ## 14 14      19671968           NA               26
    ## 15 15      19671968           NA               25
    ## 16 16      19671968           NA                4
    ## 17 17      19671968           NA                5
    ## 18 18      19671968           NA               19
    ## 19 19      19701971           NA                7
    ## 20 20      19701971           NA               23
    ## 21 21      19721973           NA               20
    ## 22 22      19721973           NA                2
    ## 23 23      19741975           NA                1
    ## 24 24      19741975           NA               15
    ## 25 25      19791980           NA               22
    ## 26 26      19791980           NA               12
    ## 27 27      19791980           NA               21
    ## 28 28      19791980           NA               53
    ## 29 29      19911992           NA               28
    ## 30 30      19921993           NA                9
    ## 31 31      19921993           NA               14
    ## 32 32      19931994           NA               24
    ## 33 33      19931994           NA               13
    ## 34 34      19981999           NA               18
    ## 35 35      19992000           NA               52
    ## 36 36      20002001           NA               29
    ## 37 37      20002001           NA               30
    ## 38 38      20172018           NA               54
    ##    teamCommonName teamPlaceName
    ## 1       Canadiens      Montréal
    ## 2       Wanderers      Montreal
    ## 3          Eagles     St. Louis
    ## 4          Tigers      Hamilton
    ## 5     Maple Leafs       Toronto
    ## 6          Bruins        Boston
    ## 7         Maroons      Montreal
    ## 8       Americans      Brooklyn
    ## 9         Quakers  Philadelphia
    ## 10        Rangers      New York
    ## 11     Blackhawks       Chicago
    ## 12      Red Wings       Detroit
    ## 13         Barons     Cleveland
    ## 14          Kings   Los Angeles
    ## 15          Stars        Dallas
    ## 16         Flyers  Philadelphia
    ## 17       Penguins    Pittsburgh
    ## 18          Blues     St. Louis
    ## 19         Sabres       Buffalo
    ## 20        Canucks     Vancouver
    ## 21         Flames       Calgary
    ## 22      Islanders      New York
    ## 23         Devils    New Jersey
    ## 24       Capitals    Washington
    ## 25         Oilers      Edmonton
    ## 26     Hurricanes      Carolina
    ## 27      Avalanche      Colorado
    ## 28        Coyotes       Arizona
    ## 29         Sharks      San Jose
    ## 30       Senators        Ottawa
    ## 31      Lightning     Tampa Bay
    ## 32          Ducks       Anaheim
    ## 33       Panthers       Florida
    ## 34      Predators     Nashville
    ## 35           Jets      Winnipeg
    ## 36   Blue Jackets      Columbus
    ## 37           Wild     Minnesota
    ## 38 Golden Knights         Vegas
    ## 
    ## $total
    ## [1] 38

This is a function to retrieve stats about all teams.  
/franchise-team-totals (Returns Total stats for every franchise (ex
roadTies, roadWins, etc))

``` r
getFranTeamTot <- function() {
  fullurl <- paste0(baseurl_records, "/", "franchise-team-totals")
  franTeamTot <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(franTeamTot)
}
getFranTeamTot() %>% head()
```

    ## No encoding supplied: defaulting to UTF-8.

    ## $data
    ##    id activeFranchise firstSeasonId franchiseId gameTypeId
    ## 1   1               1      19821983          23          2
    ## 2   2               1      19821983          23          3
    ## 3   3               1      19721973          22          2
    ## 4   4               1      19721973          22          3
    ## 5   5               1      19261927          10          2
    ## 6   6               1      19261927          10          3
    ## 7   7               1      19671968          16          3
    ## 8   8               1      19671968          16          2
    ## 9   9               1      19671968          17          2
    ## 10 10               1      19671968          17          3
    ## 11 11               1      19241925           6          2
    ## 12 12               1      19241925           6          3
    ## 13 13               1      19701971          19          2
    ## 14 14               1      19701971          19          3
    ## 15 15               1      19171918           1          3
    ## 16 16               1      19171918           1          2
    ## 17 17               1      19921993          30          2
    ## 18 18               1      19921993          30          3
    ## 19 19               1      19271928           5          2
    ## 20 20               1      19271928           5          3
    ## 21 21               1      19992000          35          2
    ## 22 22               1      19992000          35          3
    ## 23 23               1      19971998          26          3
    ## 24 24               1      19971998          26          2
    ## 25 25               1      19931994          33          2
    ## 26 26               1      19931994          33          3
    ## 27 27               1      19921993          31          2
    ## 28 28               1      19921993          31          3
    ## 29 29               1      19741975          24          2
    ## 30 30               1      19741975          24          3
    ## 31 31               1      19261927          11          3
    ## 32 32               1      19261927          11          2
    ## 33 33               1      19321933          12          2
    ##    gamesPlayed goalsAgainst goalsFor homeLosses
    ## 1         2937         8708     8647        507
    ## 2          257          634      697         53
    ## 3         3732        11779    11889        674
    ## 4          289          845      925         48
    ## 5         6504        19863    19864       1132
    ## 6          518         1447     1404        104
    ## 7          449         1332     1335         97
    ## 8         4115        12054    13527        572
    ## 9         4115        13893    13678        679
    ## 10         385         1110     1174         83
    ## 11        6570        19001    20944        953
    ## 12         664         1875     1923        149
    ## 13        3889        11767    12333        623
    ## 14         256          765      763         54
    ## 15         759         1927     2271        131
    ## 16        6731        18092    21632        870
    ## 17        2139         6390     6093        403
    ## 18         151          372      357         35
    ## 19        6460        19805    19793       1075
    ## 20         538         1477     1380        117
    ## 21         902         3014     2465        204
    ## 22           4           17        6          2
    ## 23         101          252      241         21
    ## 24        1756         5004     4735        320
    ## 25        2053         5969     5476        385
    ## 26          48          128      115         13
    ## 27        2138         6499     6035        407
    ## 28         151          402      410         37
    ## 29        3577        11390    11325        612
    ## 30         290          821      826         75
    ## 31         548         1669     1566        104
    ## 32        6504        19501    19376       1117
    ## 33        6237        18710    19423        929
    ##    homeOvertimeLosses homeTies homeWins lastSeasonId losses
    ## 1                  82       96      783           NA   1181
    ## 2                   0       NA       74           NA    120
    ## 3                  81      170      942           NA   1570
    ## 4                   1       NA       89           NA    130
    ## 5                  73      448     1600           NA   2693
    ## 6                   0        1      137           NA    266
    ## 7                   0       NA      135           NA    218
    ## 8                  89      193     1204           NA   1429
    ## 9                  58      205     1116           NA   1718
    ## 10                  0       NA      112           NA    178
    ## 11                 89      376     1867           NA   2387
    ## 12                  2        3      191           NA    332
    ## 13                 80      197     1045           NA   1530
    ## 14                  0       NA       73           NA    132
    ## 15                  0        3      254           NA    317
    ## 16                 91      381     2025           NA   2281
    ## 17                 89       60      519           NA    912
    ## 18                  0       NA       37           NA     79
    ## 19                 82      388     1684           NA   2682
    ## 20                  0        2      148           NA    279
    ## 21                 38       26      183     20102011    437
    ## 22                  0       NA        0     20102011      4
    ## 23                  0       NA       29           NA     48
    ## 24                 72       52      433           NA    713
    ## 25                112       65      465           NA    856
    ## 26                  0       NA       12           NA     29
    ## 27                 67       56      538           NA    930
    ## 28                  0       NA       42           NA     67
    ## 29                 80      153      942           NA   1452
    ## 30                  1       NA       74           NA    152
    ## 31                  0        1      166           NA    275
    ## 32                 82      410     1642           NA   2736
    ## 33                 94      368     1729           NA   2419
    ##    overtimeLosses penaltyMinutes pointPctg points
    ## 1             162          44397    0.5330   3131
    ## 2               0           4266    0.0039      2
    ## 3             159          57422    0.5115   3818
    ## 4               0           5498    0.0138      8
    ## 5             147          85564    0.5125   6667
    ## 6               0           8181    0.0000      0
    ## 7               0           9104    0.0045      4
    ## 8             175          75761    0.5759   4740
    ## 9             148          65826    0.5180   4263
    ## 10              0           6056    0.0156     12
    ## 11            184          88037    0.5625   7391
    ## 12              0          10505    0.0301     40
    ## 13            160          60329    0.5334   4149
    ## 14              0           4682    0.0000      0
    ## 15              0          12047    0.0000      0
    ## 16            164          87019    0.5868   7899
    ## 17            164          29175    0.5084   2175
    ## 18              0           2102    0.0000      0
    ## 19            167          91941    0.5121   6616
    ## 20              0           8491    0.0112     12
    ## 21             78          13727    0.4473    807
    ## 22              0            115    0.0000      0
    ## 23              0           1198    0.0792     16
    ## 24            166          19015    0.5222   1834
    ## 25            203          28603    0.4990   2049
    ## 26              0            669    0.0000      0
    ## 27            147          30489    0.5044   2157
    ## 28              0           2110    0.0728     22
    ## 29            158          56928    0.5296   3789
    ## 30              1           5100    0.0655     38
    ## 31              0           8855    0.0000      0
    ## 32            166          91917    0.5040   6556
    ## 33            173          83995    0.5363   6690
    ##    roadLosses roadOvertimeLosses roadTies roadWins
    ## 1         674                 80      123      592
    ## 2          67                  0       NA       63
    ## 3         896                 78      177      714
    ## 4          82                  0       NA       70
    ## 5        1561                 74      360     1256
    ## 6         162                  0        7      107
    ## 7         121                  0       NA       96
    ## 8         857                 86      264      850
    ## 9        1039                 90      178      750
    ## 10         95                  1       NA       95
    ## 11       1434                 95      415     1341
    ## 12        183                  0        3      135
    ## 13        907                 80      212      745
    ## 14         78                  0       NA       51
    ## 15        186                  0        5      180
    ## 16       1411                 73      456     1424
    ## 17        509                 75       55      429
    ## 18         44                  0       NA       35
    ## 19       1607                 85      385     1154
    ## 20        162                  0        1      108
    ## 21        233                 40       19      159
    ## 22          2                  0       NA        0
    ## 23         27                  1       NA       24
    ## 24        393                 94       34      358
    ## 25        471                 91       77      387
    ## 26         16                  0       NA        7
    ## 27        523                 80       56      411
    ## 28         30                  0       NA       42
    ## 29        840                 78      150      722
    ## 30         77                  1       NA       63
    ## 31        171                  0        4      102
    ## 32       1619                 84      404     1146
    ## 33       1490                 79      405     1143
    ##    shootoutLosses shootoutWins shutouts teamId
    ## 1              79           78      193      1
    ## 2               0            0       25      1
    ## 3              67           82      167      2
    ## 4               0            0       12      2
    ## 5              66           78      403      3
    ## 6               0            0       44      3
    ## 7               0            0       33      4
    ## 8              88           50      245      4
    ## 9              53           80      184      5
    ## 10              0            0       30      5
    ## 11             80           64      500      6
    ## 12              0            0       49      6
    ## 13             71           77      194      7
    ## 14              0            0       18      7
    ## 15              0            0       67      8
    ## 16             63           68      542      8
    ## 17             78           56      135      9
    ## 18              0            0       12      9
    ## 19             77           58      419     10
    ## 20              0            0       49     10
    ## 21             29           37       41     11
    ## 22              0            0        0     11
    ## 23              0            0       10     12
    ## 24             59           46       93     12
    ## 25             95           70      112     13
    ## 26              0            0        3     13
    ## 27             57           67      118     14
    ## 28              0            1       11     14
    ## 29             69           65      174     15
    ## 30              1            0       19     15
    ## 31              0            0       32     16
    ## 32             68           73      435     16
    ## 33             73           69      421     17
    ##               teamName ties triCode wins
    ## 1    New Jersey Devils  219     NJD 1375
    ## 2    New Jersey Devils   NA     NJD  137
    ## 3   New York Islanders  347     NYI 1656
    ## 4   New York Islanders   NA     NYI  159
    ## 5     New York Rangers  808     NYR 2856
    ## 6     New York Rangers    8     NYR  244
    ## 7  Philadelphia Flyers   NA     PHI  231
    ## 8  Philadelphia Flyers  457     PHI 2054
    ## 9  Pittsburgh Penguins  383     PIT 1866
    ## 10 Pittsburgh Penguins   NA     PIT  207
    ## 11       Boston Bruins  791     BOS 3208
    ## 12       Boston Bruins    6     BOS  326
    ## 13      Buffalo Sabres  409     BUF 1790
    ## 14      Buffalo Sabres   NA     BUF  124
    ## 15  Montréal Canadiens    8     MTL  434
    ## 16  Montréal Canadiens  837     MTL 3449
    ## 17     Ottawa Senators  115     OTT  948
    ## 18     Ottawa Senators   NA     OTT   72
    ## 19 Toronto Maple Leafs  773     TOR 2838
    ## 20 Toronto Maple Leafs    3     TOR  256
    ## 21   Atlanta Thrashers   45     ATL  342
    ## 22   Atlanta Thrashers   NA     ATL    0
    ## 23 Carolina Hurricanes   NA     CAR   53
    ## 24 Carolina Hurricanes   86     CAR  791
    ## 25    Florida Panthers  142     FLA  852
    ## 26    Florida Panthers   NA     FLA   19
    ## 27 Tampa Bay Lightning  112     TBL  949
    ## 28 Tampa Bay Lightning   NA     TBL   84
    ## 29 Washington Capitals  303     WSH 1664
    ## 30 Washington Capitals   NA     WSH  137
    ## 31  Chicago Blackhawks    5     CHI  268
    ## 32  Chicago Blackhawks  814     CHI 2788
    ## 33   Detroit Red Wings  773     DET 2872
    ##  [ reached 'max' / getOption("max.print") -- omitted 72 rows ]
    ## 
    ## $total
    ## [1] 105

This function retrieves season records from one specific team, and
therefore you need to provide the `franchiseId` as an argument. The ID
can be found using `getFran` or `getFranTeamTot`.  
/site/api/franchise-season-records?cayenneExp=franchiseId=ID (Drill-down
into season records for a specific franchise)

``` r
getFranSeaRec <- function(franID) {
  fullurl <- paste0(baseurl_records, "/", "franchise-season-records?cayenneExp=franchiseId=", franID)
  franSeaRec <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(franSeaRec)
}
getFranSeaRec("20") %>% head()
```

    ## No encoding supplied: defaulting to UTF-8.

    ## $data
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
    ## 
    ## $total
    ## [1] 1

This function retrieves goalie records, and again a `franchiseId` is
required.  
/franchise-goalie-records?cayenneExp=franchiseId=ID (Goalie records for
the specified franchise)

``` r
getFranGoaRec <- function(franID) {
  fullurl <- paste0(baseurl_records, "/", "franchise-goalie-records?cayenneExp=franchiseId=", franID)
  franGoaRec <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(franGoaRec)
}
getFranGoaRec("20") %>% head()
```

    ## No encoding supplied: defaulting to UTF-8.

    ## $data
    ##      id activePlayer firstName franchiseId
    ## 1   243        FALSE      Kirk          20
    ## 2   297        FALSE   Roberto          20
    ## 3   304        FALSE   Richard          20
    ## 4   364        FALSE      Gary          20
    ## 5   367        FALSE      Sean          20
    ## 6   373        FALSE   Jacques          20
    ## 7   406        FALSE       Bob          20
    ## 8   423        FALSE      Troy          20
    ## 9   424        FALSE      John          20
    ## 10  500        FALSE       Bob          20
    ## 11  588        FALSE    George          20
    ## 12  604        FALSE   Charlie          20
    ## 13  622        FALSE    Cesare          20
    ## 14  707        FALSE      Gary          20
    ## 15  756        FALSE     Steve          20
    ## 16  761        FALSE       Kay          20
    ## 17  787        FALSE    Arturs          20
    ## 18  798        FALSE     Felix          20
    ## 19  805        FALSE     Corey          20
    ## 20  809        FALSE     Garth          20
    ## 21  854        FALSE    Martin          20
    ## 22  867        FALSE     Tyler          20
    ## 23  884        FALSE       Dan          20
    ## 24  899        FALSE     Johan          20
    ## 25  941        FALSE     Peter          20
    ## 26  952        FALSE      Mika          20
    ## 27  961        FALSE     Jason          20
    ## 28  965        FALSE      Dany          20
    ## 29  968        FALSE    Andrew          20
    ## 30  989         TRUE      Ryan          20
    ## 31  995        FALSE    Curtis          20
    ## 32 1117         TRUE   Richard          20
    ## 33 1129         TRUE     Jacob          20
    ## 34 1142         TRUE    Anders          20
    ##        franchiseName gameTypeId gamesPlayed  lastName
    ## 1  Vancouver Canucks          2         516    McLean
    ## 2  Vancouver Canucks          2         448    Luongo
    ## 3  Vancouver Canucks          2         377   Brodeur
    ## 4  Vancouver Canucks          2          73   Bromley
    ## 5  Vancouver Canucks          2          16     Burke
    ## 6  Vancouver Canucks          2          10     Caron
    ## 7  Vancouver Canucks          2          39   Essensa
    ## 8  Vancouver Canucks          2          72    Gamble
    ## 9  Vancouver Canucks          2          56   Garrett
    ## 10 Vancouver Canucks          2           6     Mason
    ## 11 Vancouver Canucks          2          42   Gardner
    ## 12 Vancouver Canucks          2          35     Hodge
    ## 13 Vancouver Canucks          2          93   Maniago
    ## 14 Vancouver Canucks          2         188     Smith
    ## 15 Vancouver Canucks          2          66     Weeks
    ## 16 Vancouver Canucks          2          74  Whitmore
    ## 17 Vancouver Canucks          2          41      Irbe
    ## 18 Vancouver Canucks          2          69    Potvin
    ## 19 Vancouver Canucks          2           6    Schwab
    ## 20 Vancouver Canucks          2         109      Snow
    ## 21 Vancouver Canucks          2           6    Brochu
    ## 22 Vancouver Canucks          2           1      Moss
    ## 23 Vancouver Canucks          2         208  Cloutier
    ## 24 Vancouver Canucks          2          21   Hedberg
    ## 25 Vancouver Canucks          2          46    Skudra
    ## 26 Vancouver Canucks          2           4   Noronen
    ## 27 Vancouver Canucks          2           9 LaBarbera
    ## 28 Vancouver Canucks          2           9  Sabourin
    ## 29 Vancouver Canucks          2          21  Raycroft
    ## 30 Vancouver Canucks          2         150    Miller
    ## 31 Vancouver Canucks          2          35   Sanford
    ## 32 Vancouver Canucks          2           7   Bachman
    ## 33 Vancouver Canucks          2         229 Markstrom
    ## 34 Vancouver Canucks          2          39   Nilsson
    ##    losses
    ## 1     228
    ## 2     137
    ## 3     173
    ## 4      27
    ## 5       9
    ## 6       5
    ## 7      12
    ## 8      29
    ## 9      21
    ## 10      4
    ## 11     22
    ## 12     13
    ## 13     45
    ## 14     81
    ## 15     34
    ## 16     28
    ## 17     11
    ## 18     30
    ## 19      1
    ## 20     52
    ## 21      3
    ## 22      0
    ## 23     68
    ## 24      6
    ## 25     13
    ## 26      1
    ## 27      2
    ## 28      4
    ## 29      5
    ## 30     68
    ## 31     11
    ## 32      4
    ## 33     93
    ## 34     22
    ##                                         mostGoalsAgainstDates
    ## 1                                                  1996-10-19
    ## 2                                      2013-02-24, 2010-04-01
    ## 3                                                  1981-10-17
    ## 4                                      1981-02-20, 1979-03-08
    ## 5                          1998-01-28, 1998-01-21, 1998-01-15
    ## 6                                                  1973-12-20
    ## 7                                      2001-02-17, 2000-12-29
    ## 8                                                  1991-02-02
    ## 9                                      1984-11-29, 1984-03-11
    ## 10                                                 1991-03-03
    ## 11                                                 1972-02-10
    ## 12                                                 1971-02-27
    ## 13                         1978-03-28, 1978-03-08, 1976-12-29
    ## 14 1976-03-27, 1976-01-08, 1975-03-21, 1974-11-24, 1973-12-16
    ## 15                                                 1990-02-28
    ## 16             1995-01-21, 1994-02-10, 1993-12-15, 1993-03-20
    ## 17             1998-03-26, 1997-12-29, 1997-12-20, 1997-11-03
    ## 18 2001-01-10, 2000-12-21, 2000-11-11, 2000-10-12, 2000-10-05
    ## 19                                                 1999-11-07
    ## 20                                                 1999-02-04
    ## 21                                     2001-11-03, 2001-10-09
    ## 22                                                 2002-12-23
    ## 23                         2005-10-12, 2001-12-20, 2001-10-18
    ## 24                         2004-03-21, 2004-03-08, 2004-02-03
    ## 25                                     2002-01-09, 2001-12-08
    ## 26                                                 2006-03-14
    ## 27                                                 2009-01-09
    ## 28                                                 2006-11-23
    ## 29 2010-04-08, 2010-04-02, 2009-12-05, 2009-11-10, 2009-10-30
    ## 30                                                 2016-04-07
    ## 31                                                 2008-12-04
    ## 32                                                 2018-11-15
    ## 33                                                 2016-11-15
    ## 34                                                 2017-12-13
    ##    mostGoalsAgainstOneGame
    ## 1                        9
    ## 2                        8
    ## 3                       10
    ## 4                        9
    ## 5                        6
    ## 6                        9
    ## 7                        5
    ## 8                        9
    ## 9                       12
    ## 10                       8
    ## 11                       9
    ## 12                       8
    ## 13                       8
    ## 14                       7
    ## 15                       7
    ## 16                       7
    ## 17                       5
    ## 18                       5
    ## 19                       5
    ## 20                       8
    ## 21                       5
    ## 22                       1
    ## 23                       6
    ## 24                       5
    ## 25                       5
    ## 26                       5
    ## 27                       6
    ## 28                       5
    ## 29                       4
    ## 30                       7
    ## 31                       6
    ## 32                       6
    ## 33                       7
    ## 34                       7
    ##                        mostSavesDates mostSavesOneGame
    ## 1              1997-04-05, 1987-12-17               48
    ## 2                          2010-03-20               50
    ## 3                          1985-02-10               51
    ## 4                          1979-03-15               41
    ## 5                          1998-01-26               33
    ## 6                          1973-11-27               33
    ## 7                          2001-03-13               37
    ## 8                          1990-10-27               40
    ## 9                          1983-02-15               43
    ## 10                         1991-02-27               35
    ## 11             1971-10-20, 1970-12-23               50
    ## 12                         1971-03-13               40
    ## 13                         1978-01-07               51
    ## 14 1974-12-15, 1974-04-03, 1974-01-15               41
    ## 15                         1989-10-27               46
    ## 16                         1995-01-25               38
    ## 17                         1998-04-01               42
    ## 18                         2000-02-29               40
    ## 19                         1999-11-09               24
    ## 20                         2000-01-13               48
    ## 21                         2001-11-03               30
    ## 22                         2002-12-23               13
    ## 23                         2004-01-09               44
    ## 24                         2004-03-06               32
    ## 25                         2002-01-09               43
    ## 26                         2006-04-15               31
    ## 27             2009-01-04, 2009-01-02               34
    ## 28                         2007-02-20               38
    ## 29                         2010-02-12               32
    ## 30             2016-03-19, 2016-01-17               47
    ## 31                         2007-10-21               35
    ## 32                         2017-03-05               43
    ## 33             2020-02-12, 2019-12-28               49
    ## 34                         2018-02-17               44
    ##                 mostShotsAgainstDates
    ## 1                          1988-11-17
    ## 2                          2010-03-20
    ## 3                          1985-02-10
    ## 4                          1979-03-15
    ## 5              1998-01-28, 1998-01-26
    ## 6                          1973-12-20
    ## 7                          2001-03-13
    ## 8              1991-02-08, 1990-10-27
    ## 9                          1983-02-15
    ## 10                         1991-02-27
    ## 11             1971-10-20, 1970-12-23
    ## 12                         1971-03-13
    ## 13                         1978-01-07
    ## 14                         1974-11-24
    ## 15                         1989-10-27
    ## 16                         1995-01-25
    ## 17                         1998-04-01
    ## 18                         2000-02-29
    ## 19                         1999-11-09
    ## 20                         2000-01-13
    ## 21                         2001-11-03
    ## 22                         2002-12-23
    ## 23                         2004-01-09
    ## 24 2004-02-14, 2004-01-29, 2003-11-24
    ## 25                         2002-01-09
    ## 26                         2006-04-15
    ## 27                         2009-01-02
    ## 28                         2007-02-20
    ## 29                         2010-02-12
    ## 30             2016-03-19, 2016-01-19
    ## 31                         2008-01-11
    ## 32             2017-04-09, 2017-03-05
    ## 33                         2019-12-28
    ## 34                         2017-12-13
    ##    mostShotsAgainstOneGame mostShutoutsOneSeason
    ## 1                       52                     5
    ## 2                       54                     9
    ## 3                       54                     2
    ## 4                       45                     2
    ## 5                       36                     0
    ## 6                       39                     0
    ## 7                       39                     1
    ## 8                       42                     1
    ## 9                       49                     1
    ## 10                      38                     0
    ## 11                      57                     0
    ## 12                      46                     0
    ## 13                      57                     1
    ## 14                      46                     6
    ## 15                      51                     0
    ## 16                      44                     1
    ## 17                      44                     2
    ## 18                      41                     1
    ## 19                      28                     0
    ## 20                      51                     6
    ## 21                      35                     0
    ## 22                      14                     0
    ## 23                      46                     7
    ## 24                      33                     3
    ## 25                      48                     1
    ## 26                      34                     0
    ## 27                      37                     0
    ## 28                      40                     0
    ## 29                      35                     1
    ## 30                      49                     6
    ## 31                      38                     1
    ## 32                      44                     0
    ## 33                      51                     2
    ## 34                      48                     2
    ##                     mostShutoutsSeasonIds mostWinsOneSeason
    ## 1                                19911992                38
    ## 2                                20082009                47
    ## 3                      19811982, 19851986                21
    ## 4                                19781979                11
    ## 5                                19971998                 2
    ## 6                                19731974                 2
    ## 7                                20002001                18
    ## 8                                19901991                16
    ## 9                                19821983                14
    ## 10                               19901991                 2
    ## 11                     19701971, 19711972                 6
    ## 12                               19701971                15
    ## 13                     19761977, 19771978                17
    ## 14                               19741975                32
    ## 15 19871988, 19881989, 19891990, 19901991                11
    ## 16                               19921993                18
    ## 17                               19971998                14
    ## 18                               20002001                14
    ## 19                               19992000                 2
    ## 20                               19981999                20
    ## 21                               20012002                 0
    ## 22                               20022003                 0
    ## 23                               20012002                33
    ## 24                               20032004                 8
    ## 25                     20012002, 20022003                10
    ## 26                               20052006                 1
    ## 27                               20082009                 3
    ## 28                               20062007                 2
    ## 29                               20092010                 9
    ## 30                               20142015                29
    ## 31                               20082009                 7
    ## 32           20152016, 20162017, 20182019                 2
    ## 33                     20172018, 20192020                28
    ## 34                               20172018                 7
    ##     mostWinsSeasonIds overtimeLosses playerId positionCode
    ## 1            19911992             NA  8449474            G
    ## 2            20062007             50  8466141            G
    ## 3            19821983             NA  8445694            G
    ## 4            19781979             NA  8445695            G
    ## 5            19971998             NA  8445769            G
    ## 6            19731974             NA  8445966            G
    ## 7            20002001             NA  8446719            G
    ## 8            19901991             NA  8447029            G
    ## 9            19831984             NA  8447066            G
    ## 10           19901991             NA  8449286            G
    ## 11           19701971             NA  8449967            G
    ## 12           19701971             NA  8449995            G
    ## 13           19761977             NA  8450020            G
    ## 14           19741975             NA  8451528            G
    ## 15           19881989             NA  8452355            G
    ## 16 19921993, 19931994             NA  8452440            G
    ## 17           19971998             NA  8456692            G
    ## 18           20002001             NA  8457714            G
    ## 19           19992000             NA  8457969            G
    ## 20           19981999             NA  8458075            G
    ## 21           20012002             NA  8459289            G
    ## 22           20022003             NA  8459451            G
    ## 23 20022003, 20032004              1  8460516            G
    ## 24           20032004             NA  8460704            G
    ## 25           20012002             NA  8464715            G
    ## 26           20052006              0  8466156            G
    ## 27           20082009              2  8467391            G
    ## 28           20062007              1  8467430            G
    ## 29           20092010              1  8467453            G
    ## 30           20142015             16  8468011            G
    ## 31           20082009              1  8468166            G
    ## 32           20162017              0  8473614            G
    ## 33           20182019             27  8474593            G
    ## 34           20172018              5  8475195            G
    ##    rookieGamesPlayed rookieShutouts rookieWins seasons
    ## 1                 41              1         11      11
    ## 2                 NA             NA         NA       8
    ## 3                 NA             NA         NA       8
    ## 4                 NA             NA         NA       3
    ## 5                 NA             NA         NA       1
    ## 6                 NA             NA         NA       1
    ## 7                 NA             NA         NA       1
    ## 8                 47              1         16       4
    ## 9                 NA             NA         NA       3
    ## 10                NA             NA         NA       1
    ## 11                NA             NA         NA       2
    ## 12                NA             NA         NA       1
    ## 13                NA             NA         NA       2
    ## 14                NA             NA         NA       3
    ## 15                NA             NA         NA       4
    ## 16                NA             NA         NA       3
    ## 17                NA             NA         NA       1
    ## 18                NA             NA         NA       2
    ## 19                NA             NA         NA       1
    ## 20                NA             NA         NA       3
    ## 21                NA             NA         NA       1
    ## 22                NA             NA         NA       1
    ## 23                NA             NA         NA       5
    ## 24                NA             NA         NA       1
    ## 25                NA             NA         NA       2
    ## 26                NA             NA         NA       1
    ## 27                NA             NA         NA       1
    ## 28                NA             NA         NA       1
    ## 29                NA             NA         NA       1
    ## 30                NA             NA         NA       3
    ## 31                NA             NA         NA       2
    ## 32                NA             NA         NA       3
    ## 33                NA             NA         NA       7
    ## 34                NA             NA         NA       2
    ##    shutouts ties wins
    ## 1        20   62  211
    ## 2        38   NA  252
    ## 3         6   62  126
    ## 4         3   14   25
    ## 5         0    4    2
    ## 6         0    1    2
    ## 7         1    3   18
    ## 8         1    9   22
    ## 9         1    5   22
    ## 10        0    0    2
    ## 11        0    4    9
    ## 12        0    5   15
    ## 13        2   17   27
    ## 14       11   23   72
    ## 15        0   11   19
    ## 16        1    6   36
    ## 17        2    6   14
    ## 18        1   10   26
    ## 19        0    1    2
    ## 20        6   11   33
    ## 21        0    0    0
    ## 22        0    0    0
    ## 23       14   23  109
    ## 24        3    2    8
    ## 25        2    8   19
    ## 26        0   NA    1
    ## 27        0   NA    3
    ## 28        0   NA    2
    ## 29        1   NA    9
    ## 30       10   NA   64
    ## 31        1   NA   11
    ## 32        0    0    3
    ## 33        5    0   99
    ## 34        2    0   10
    ##  [ reached 'max' / getOption("max.print") -- omitted 5 rows ]
    ## 
    ## $total
    ## [1] 39

This function retrieves information about skater records, and a
`franchiseId` is required.  
/franchise-skater-records?cayenneExp=franchiseId=ID (Skater records,
same interaction as goalie endpoint)

``` r
getFranSkaRec <- function(franID) {
  fullurl <- paste0(baseurl_records, "/", "franchise-skater-records?cayenneExp=franchiseId=", franID)
  franSkaRec <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(franSkaRec)
}
getFranSkaRec("20") %>% head()
```

    ## No encoding supplied: defaulting to UTF-8.

    ## $data
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
    ## 
    ## $total
    ## [1] 561

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

<!-- end list -->

``` r
baseurl_stats <- "https://statsapi.web.nhl.com/api/v1/teams"
getStats <- function(expand = "", teamID = "", stats = ""){
  if (expand != ""){
    fullurl <- paste0(baseurl_stats, "?expand=", expand)
  } else if (teamID != ""){
    fullurl <- paste0(baseurl_stats, "?teamID=", teamID)
  } else if (stats != ""){
    fullurl <- paste0(baseurl_stats, "?stats=", stats)
  }
  stats <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(stats)
}
getStats(expand = "person.names") %>% head()
```

    ## $copyright
    ## [1] "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2020. All Rights Reserved."
    ## 
    ## $teams
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
getStats(stats = "statsSingleSeasonPlayoffs") %>% head()
```

    ## $copyright
    ## [1] "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2020. All Rights Reserved."
    ## 
    ## $teams
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
getStats(expand = "team.roster")
```

    ## $copyright
    ## [1] "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2020. All Rights Reserved."
    ## 
    ## $teams
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
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         roster.roster
    ## 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               19, 35, 76, 21, 33, 28, 5, 97, 8, 15, 44, 25, 32, 37, 29, 16, 14, 63, 41, 13, 86, 8471233, 8471239, 8474056, 8475151, 8476368, 8476923, 8476941, 8477038, 8477355, 8477401, 8477425, 8477509, 8477541, 8478401, 8478406, 8479291, 8479315, 8479407, 8479415, 8480002, 8481559, Travis Zajac, Cory Schneider, P.K. Subban, Kyle Palmieri, Fredrik Claesson, Damon Severson, Connor Carrick, Nikita Gusev, Will Butcher, John Hayden, Miles Wood, Mirco Mueller, Dakota Mermis, Pavel Zacha, Mackenzie Blackwood, Kevin Rooney, Joey Anderson, Jesper Bratt, Michael McLeod, Nico Hischier, Jack Hughes, /api/v1/people/8471233, /api/v1/people/8471239, /api/v1/people/8474056, /api/v1/people/8475151, /api/v1/people/8476368, /api/v1/people/8476923, /api/v1/people/8476941, /api/v1/people/8477038, /api/v1/people/8477355, /api/v1/people/8477401, /api/v1/people/8477425, /api/v1/people/8477509, /api/v1/people/8477541, /api/v1/people/8478401, /api/v1/people/8478406, /api/v1/people/8479291, /api/v1/people/8479315, /api/v1/people/8479407, /api/v1/people/8479415, /api/v1/people/8480002, /api/v1/people/8481559, C, G, D, R, D, D, D, L, D, C, L, D, D, C, G, C, R, L, C, C, C, Center, Goalie, Defenseman, Right Wing, Defenseman, Defenseman, Defenseman, Left Wing, Defenseman, Center, Left Wing, Defenseman, Defenseman, Center, Goalie, Center, Right Wing, Left Wing, Center, Center, Center, Forward, Goalie, Defenseman, Forward, Defenseman, Defenseman, Defenseman, Forward, Defenseman, Forward, Forward, Defenseman, Defenseman, Forward, Goalie, Forward, Forward, Forward, Forward, Forward, Forward, C, G, D, RW, D, D, D, LW, D, C, LW, D, D, C, G, C, RW, LW, C, C, C
    ## 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                        55, 16, 1, 4, 47, 15, 10, 40, 34, 12, 7, 17, 2, 53, 27, 29, 14, 44, 24, 33, 3, 6, 32, 28, 25, 13, 18, 21, 38, 8, 8470187, 8471217, 8471306, 8472382, 8473463, 8473504, 8473544, 8473575, 8474066, 8474573, 8474586, 8474709, 8475181, 8475231, 8475314, 8475754, 8475832, 8476419, 8476429, 8476444, 8476917, 8477506, 8477527, 8477936, 8478038, 8478445, 8478463, 8479526, 8480222, 8480865, Johnny Boychuk, Andrew Ladd, Thomas Greiss, Andy Greene, Leo Komarov, Cal Clutterbuck, Derick Brassard, Semyon Varlamov, Thomas Hickey, Josh Bailey, Jordan Eberle, Matt Martin, Nick Leddy, Casey Cizikas, Anders Lee, Brock Nelson, Tom Kuhnhackl, Jean-Gabriel Pageau, Scott Mayfield, Christopher Gibson, Adam Pelech, Ryan Pulock, Ross Johnston, Michael Dal Colle, Devon Toews, Mathew Barzal, Anthony Beauvillier, Otto Koivula, Sebastian Aho, Noah Dobson, /api/v1/people/8470187, /api/v1/people/8471217, /api/v1/people/8471306, /api/v1/people/8472382, /api/v1/people/8473463, /api/v1/people/8473504, /api/v1/people/8473544, /api/v1/people/8473575, /api/v1/people/8474066, /api/v1/people/8474573, /api/v1/people/8474586, /api/v1/people/8474709, /api/v1/people/8475181, /api/v1/people/8475231, /api/v1/people/8475314, /api/v1/people/8475754, /api/v1/people/8475832, /api/v1/people/8476419, /api/v1/people/8476429, /api/v1/people/8476444, /api/v1/people/8476917, /api/v1/people/8477506, /api/v1/people/8477527, /api/v1/people/8477936, /api/v1/people/8478038, /api/v1/people/8478445, /api/v1/people/8478463, /api/v1/people/8479526, /api/v1/people/8480222, /api/v1/people/8480865, D, L, G, D, R, R, C, G, D, R, R, L, D, C, L, C, R, C, D, G, D, D, L, L, D, C, L, L, D, D, Defenseman, Left Wing, Goalie, Defenseman, Right Wing, Right Wing, Center, Goalie, Defenseman, Right Wing, Right Wing, Left Wing, Defenseman, Center, Left Wing, Center, Right Wing, Center, Defenseman, Goalie, Defenseman, Defenseman, Left Wing, Left Wing, Defenseman, Center, Left Wing, Left Wing, Defenseman, Defenseman, Defenseman, Forward, Goalie, Defenseman, Forward, Forward, Forward, Goalie, Defenseman, Forward, Forward, Forward, Defenseman, Forward, Forward, Forward, Forward, Forward, Defenseman, Goalie, Defenseman, Defenseman, Forward, Forward, Defenseman, Forward, Forward, Forward, Defenseman, Defenseman, D, LW, G, D, RW, RW, C, G, D, RW, RW, LW, D, C, LW, C, RW, C, D, G, D, D, LW, LW, D, C, LW, LW, D, D
    ## 3                                                                                                                                                                                                                                                                                                                                                                                         38, 74, 30, 18, 42, 20, 14, 17, 29, 16, 93, 33, 8, 65, 89, 77, 48, 31, 44, 10, 46, 23, 55, 12, 25, 21, 26, 95, 72, 40, 24, 8474230, 8480833, 8468685, 8471686, 8474090, 8475184, 8475735, 8475855, 8476396, 8476458, 8476459, 8476858, 8476885, 8476982, 8477402, 8477950, 8477962, 8478048, 8478178, 8478550, 8479027, 8479323, 8479324, 8479328, 8479333, 8479353, 8479364, 8479968, 8480078, 8480382, 8481554, Micheal Haley, Vitali Kravtsov, Henrik Lundqvist, Marc Staal, Brendan Smith, Chris Kreider, Greg McKegg, Jesper Fast, Steven Fogarty, Ryan Strome, Mika Zibanejad, Phillip Di Giuseppe, Jacob Trouba, Danny O'Regan, Pavel Buchnevich, Tony DeAngelo, Brendan Lemieux, Igor Shesterkin, Darren Raddysh, Artemi Panarin, Brandon Crawley, Adam Fox, Ryan Lindgren, Julien Gauthier, Libor Hajek, Brett Howden, Tim Gettinger, Vinni Lettieri, Filip Chytil, Alexandar Georgiev, Kaapo Kakko, /api/v1/people/8474230, /api/v1/people/8480833, /api/v1/people/8468685, /api/v1/people/8471686, /api/v1/people/8474090, /api/v1/people/8475184, /api/v1/people/8475735, /api/v1/people/8475855, /api/v1/people/8476396, /api/v1/people/8476458, /api/v1/people/8476459, /api/v1/people/8476858, /api/v1/people/8476885, /api/v1/people/8476982, /api/v1/people/8477402, /api/v1/people/8477950, /api/v1/people/8477962, /api/v1/people/8478048, /api/v1/people/8478178, /api/v1/people/8478550, /api/v1/people/8479027, /api/v1/people/8479323, /api/v1/people/8479324, /api/v1/people/8479328, /api/v1/people/8479333, /api/v1/people/8479353, /api/v1/people/8479364, /api/v1/people/8479968, /api/v1/people/8480078, /api/v1/people/8480382, /api/v1/people/8481554, C, R, G, D, D, L, C, R, C, C, C, L, D, C, R, D, L, G, D, L, D, D, D, R, D, C, L, R, C, G, R, Center, Right Wing, Goalie, Defenseman, Defenseman, Left Wing, Center, Right Wing, Center, Center, Center, Left Wing, Defenseman, Center, Right Wing, Defenseman, Left Wing, Goalie, Defenseman, Left Wing, Defenseman, Defenseman, Defenseman, Right Wing, Defenseman, Center, Left Wing, Right Wing, Center, Goalie, Right Wing, Forward, Forward, Goalie, Defenseman, Defenseman, Forward, Forward, Forward, Forward, Forward, Forward, Forward, Defenseman, Forward, Forward, Defenseman, Forward, Goalie, Defenseman, Forward, Defenseman, Defenseman, Defenseman, Forward, Defenseman, Forward, Forward, Forward, Forward, Goalie, Forward, C, RW, G, D, D, LW, C, RW, C, C, C, LW, D, C, RW, D, LW, G, D, LW, D, D, D, RW, D, C, LW, RW, C, G, RW
    ## 4                                                                                                                                                                                                                                          55, 44, 44, 37, 15, 28, 61, 25, 93, 38, 18, 13, 10, 3, 14, 21, 53, 12, 8, 6, 62, 59, 23, 11, 9, 5, 34, 82, 79, 48, 67, 49, 54, 8477502, 8473485, 8470775, 8470880, 8471702, 8473512, 8474027, 8474037, 8474161, 8474683, 8475752, 8475763, 8476404, 8476407, 8476461, 8476872, 8476906, 8477290, 8477462, 8477948, 8477979, 8478017, 8478067, 8478439, 8478500, 8479026, 8479312, 8479382, 8479394, 8480028, 8480279, 8480797, 8481178, Samuel Morin, Chris Stewart, Nate Thompson, Brian Elliott, Matt Niskanen, Claude Giroux, Justin Braun, James van Riemsdyk, Jakub Voracek, Derek Grant, Tyler Pitlick, Kevin Hayes, Andy Andreoff, Andy Welinski, Sean Couturier, Scott Laughton, Shayne Gostisbehere, Michael Raffl, Robert Hagg, Travis Sanheim, Nicolas Aube-Kubel, Mark Friedman, Oskar Lindblom, Travis Konecny, Ivan Provorov, Philippe Myers, Alex Lyon, Connor Bunnaman, Carter Hart, Morgan Frost, Kirill Ustimenko, Joel Farabee, Egor Zamula, /api/v1/people/8477502, /api/v1/people/8473485, /api/v1/people/8470775, /api/v1/people/8470880, /api/v1/people/8471702, /api/v1/people/8473512, /api/v1/people/8474027, /api/v1/people/8474037, /api/v1/people/8474161, /api/v1/people/8474683, /api/v1/people/8475752, /api/v1/people/8475763, /api/v1/people/8476404, /api/v1/people/8476407, /api/v1/people/8476461, /api/v1/people/8476872, /api/v1/people/8476906, /api/v1/people/8477290, /api/v1/people/8477462, /api/v1/people/8477948, /api/v1/people/8477979, /api/v1/people/8478017, /api/v1/people/8478067, /api/v1/people/8478439, /api/v1/people/8478500, /api/v1/people/8479026, /api/v1/people/8479312, /api/v1/people/8479382, /api/v1/people/8479394, /api/v1/people/8480028, /api/v1/people/8480279, /api/v1/people/8480797, /api/v1/people/8481178, D, R, C, G, D, C, D, L, R, C, C, C, C, D, C, C, D, L, D, D, R, D, L, R, D, D, G, C, G, C, G, L, D, Defenseman, Right Wing, Center, Goalie, Defenseman, Center, Defenseman, Left Wing, Right Wing, Center, Center, Center, Center, Defenseman, Center, Center, Defenseman, Left Wing, Defenseman, Defenseman, Right Wing, Defenseman, Left Wing, Right Wing, Defenseman, Defenseman, Goalie, Center, Goalie, Center, Goalie, Left Wing, Defenseman, Defenseman, Forward, Forward, Goalie, Defenseman, Forward, Defenseman, Forward, Forward, Forward, Forward, Forward, Forward, Defenseman, Forward, Forward, Defenseman, Forward, Defenseman, Defenseman, Forward, Defenseman, Forward, Forward, Defenseman, Defenseman, Goalie, Forward, Goalie, Forward, Goalie, Forward, Defenseman, D, RW, C, G, D, C, D, LW, RW, C, C, C, C, D, C, C, D, LW, D, D, RW, D, LW, RW, D, D, G, C, G, C, G, LW, D
    ## 5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         27, 18, 12, 71, 87, 3, 58, 72, 4, 8, 16, 17, 30, 53, 2, 59, 35, 43, 42, 19, 28, 37, 57, 6, 13, 46, 50, 8475760, 8478866, 8466139, 8471215, 8471675, 8471677, 8471724, 8471887, 8474602, 8475208, 8475722, 8475810, 8476899, 8476927, 8477244, 8477404, 8477465, 8477839, 8477953, 8477955, 8477969, 8478043, 8478074, 8478507, 8479293, 8479944, 8480945, Nick Bjugstad, Dominik Simon, Patrick Marleau, Evgeni Malkin, Sidney Crosby, Jack Johnson, Kris Letang, Patric Hornqvist, Justin Schultz, Brian Dumoulin, Jason Zucker, Bryan Rust, Matt Murray, Teddy Blueger, Chad Ruhwedel, Jake Guentzel, Tristan Jarry, Conor Sheary, Kasperi Kapanen, Jared McCann, Marcus Pettersson, Sam Lafferty, Anthony Angello, John Marino, Brandon Tanev, Zach Aston-Reese, Juuso Riikola, /api/v1/people/8475760, /api/v1/people/8478866, /api/v1/people/8466139, /api/v1/people/8471215, /api/v1/people/8471675, /api/v1/people/8471677, /api/v1/people/8471724, /api/v1/people/8471887, /api/v1/people/8474602, /api/v1/people/8475208, /api/v1/people/8475722, /api/v1/people/8475810, /api/v1/people/8476899, /api/v1/people/8476927, /api/v1/people/8477244, /api/v1/people/8477404, /api/v1/people/8477465, /api/v1/people/8477839, /api/v1/people/8477953, /api/v1/people/8477955, /api/v1/people/8477969, /api/v1/people/8478043, /api/v1/people/8478074, /api/v1/people/8478507, /api/v1/people/8479293, /api/v1/people/8479944, /api/v1/people/8480945, C, C, C, C, C, D, D, R, D, D, L, R, G, C, D, L, G, L, R, C, D, C, C, D, L, C, D, Center, Center, Center, Center, Center, Defenseman, Defenseman, Right Wing, Defenseman, Defenseman, Left Wing, Right Wing, Goalie, Center, Defenseman, Left Wing, Goalie, Left Wing, Right Wing, Center, Defenseman, Center, Center, Defenseman, Left Wing, Center, Defenseman, Forward, Forward, Forward, Forward, Forward, Defenseman, Defenseman, Forward, Defenseman, Defenseman, Forward, Forward, Goalie, Forward, Defenseman, Forward, Goalie, Forward, Forward, Forward, Defenseman, Forward, Forward, Defenseman, Forward, Forward, Defenseman, C, C, C, C, C, D, D, RW, D, D, LW, RW, G, C, D, LW, G, LW, RW, C, D, C, C, D, LW, C, D
    ## 6                                                                                                                                                                                                                                                                                                                                  86, 33, 37, 41, 46, 40, 63, 27, 13, 14, 20, 52, 35, 47, 48, 75, 21, 88, 10, 28, 67, 80, 25, 79, 19, 74, 73, 82, 58, 68, 83, 26, 8476191, 8465009, 8470638, 8470860, 8471276, 8471695, 8473419, 8475186, 8475745, 8475780, 8475807, 8476374, 8476509, 8476792, 8476891, 8477365, 8477941, 8477956, 8478075, 8478131, 8478415, 8478435, 8478443, 8478468, 8478485, 8478498, 8479325, 8479365, 8480001, 8480021, 8480901, 8480944, Kevan Miller, Zdeno Chara, Patrice Bergeron, Jaroslav Halak, David Krejci, Tuukka Rask, Brad Marchand, John Moore, Charlie Coyle, Chris Wagner, Joakim Nordstrom, Sean Kuraly, Maxime Lagace, Torey Krug, Matt Grzelcyk, Connor Clifton, Nick Ritchie, David Pastrnak, Anders Bjork, Ondrej Kase, Jakub Zboril, Dan Vladar, Brandon Carlo, Jeremy Lauzon, Zach Senyshyn, Jake DeBrusk, Charlie McAvoy, Trent Frederic, Urho Vaakanainen, Jack Studnicka, Karson Kuhlman, Par Lindholm, /api/v1/people/8476191, /api/v1/people/8465009, /api/v1/people/8470638, /api/v1/people/8470860, /api/v1/people/8471276, /api/v1/people/8471695, /api/v1/people/8473419, /api/v1/people/8475186, /api/v1/people/8475745, /api/v1/people/8475780, /api/v1/people/8475807, /api/v1/people/8476374, /api/v1/people/8476509, /api/v1/people/8476792, /api/v1/people/8476891, /api/v1/people/8477365, /api/v1/people/8477941, /api/v1/people/8477956, /api/v1/people/8478075, /api/v1/people/8478131, /api/v1/people/8478415, /api/v1/people/8478435, /api/v1/people/8478443, /api/v1/people/8478468, /api/v1/people/8478485, /api/v1/people/8478498, /api/v1/people/8479325, /api/v1/people/8479365, /api/v1/people/8480001, /api/v1/people/8480021, /api/v1/people/8480901, /api/v1/people/8480944, D, D, C, G, C, G, L, D, C, R, C, C, G, D, D, D, L, R, L, R, D, G, D, D, R, L, D, C, D, C, C, C, Defenseman, Defenseman, Center, Goalie, Center, Goalie, Left Wing, Defenseman, Center, Right Wing, Center, Center, Goalie, Defenseman, Defenseman, Defenseman, Left Wing, Right Wing, Left Wing, Right Wing, Defenseman, Goalie, Defenseman, Defenseman, Right Wing, Left Wing, Defenseman, Center, Defenseman, Center, Center, Center, Defenseman, Defenseman, Forward, Goalie, Forward, Goalie, Forward, Defenseman, Forward, Forward, Forward, Forward, Goalie, Defenseman, Defenseman, Defenseman, Forward, Forward, Forward, Forward, Defenseman, Goalie, Defenseman, Defenseman, Forward, Forward, Defenseman, Forward, Defenseman, Forward, Forward, Forward, D, D, C, G, C, G, LW, D, C, RW, C, C, G, D, D, D, LW, RW, LW, RW, D, G, D, D, RW, LW, D, C, D, C, C, C
    ## 7                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               17, 72, 48, 21, 67, 17, 90, 40, 22, 53, 33, 28, 13, 19, 35, 55, 27, 23, 62, 68, 9, 10, 26, 24, 95, 8471743, 8479420, 8471436, 8473449, 8473564, 8474190, 8475149, 8475622, 8475728, 8475784, 8476525, 8476878, 8476918, 8476931, 8476999, 8477499, 8477508, 8477933, 8477986, 8478109, 8478403, 8480035, 8480839, 8480935, 8480946, Vladimir Sobotka, Tage Thompson, Matt Hunwick, Kyle Okposo, Michael Frolik, Wayne Simmonds, Marcus Johansson, Carter Hutton, Johan Larsson, Jeff Skinner, Colin Miller, Zemgus Girgensons, Jimmy Vesey, Jake McCabe, Linus Ullmark, Rasmus Ristolainen, Curtis Lazar, Sam Reinhart, Brandon Montour, Victor Olofsson, Jack Eichel, Henri Jokiharju, Rasmus Dahlin, Lawrence Pilut, Dominik Kahun, /api/v1/people/8471743, /api/v1/people/8479420, /api/v1/people/8471436, /api/v1/people/8473449, /api/v1/people/8473564, /api/v1/people/8474190, /api/v1/people/8475149, /api/v1/people/8475622, /api/v1/people/8475728, /api/v1/people/8475784, /api/v1/people/8476525, /api/v1/people/8476878, /api/v1/people/8476918, /api/v1/people/8476931, /api/v1/people/8476999, /api/v1/people/8477499, /api/v1/people/8477508, /api/v1/people/8477933, /api/v1/people/8477986, /api/v1/people/8478109, /api/v1/people/8478403, /api/v1/people/8480035, /api/v1/people/8480839, /api/v1/people/8480935, /api/v1/people/8480946, C, R, D, R, R, R, L, G, C, L, D, C, L, D, G, D, C, C, D, L, C, D, D, D, C, Center, Right Wing, Defenseman, Right Wing, Right Wing, Right Wing, Left Wing, Goalie, Center, Left Wing, Defenseman, Center, Left Wing, Defenseman, Goalie, Defenseman, Center, Center, Defenseman, Left Wing, Center, Defenseman, Defenseman, Defenseman, Center, Forward, Forward, Defenseman, Forward, Forward, Forward, Forward, Goalie, Forward, Forward, Defenseman, Forward, Forward, Defenseman, Goalie, Defenseman, Forward, Forward, Defenseman, Forward, Forward, Defenseman, Defenseman, Defenseman, Forward, C, RW, D, RW, RW, RW, LW, G, C, LW, D, C, LW, D, G, D, C, C, D, LW, C, D, D, D, C
    ## 8                                                                                                                                                                                                                                                               45, 46, 6, 31, 26, 41, 34, 22, 90, 8, 43, 11, 60, 61, 40, 24, 54, 77, 51, 62, 92, 13, 32, 71, 58, 70, 39, 53, 20, 14, 30, 25, 15, 8477460, 8479990, 8470642, 8471679, 8473507, 8474038, 8474596, 8474668, 8475193, 8475279, 8475738, 8475848, 8475968, 8476443, 8476469, 8476479, 8476948, 8476967, 8477467, 8477476, 8477494, 8477503, 8477850, 8478133, 8478454, 8478933, 8479292, 8479376, 8479985, 8480018, 8480051, 8480068, 8480829, Laurent Dauphin, Josh Brook, Shea Weber, Carey Price, Jeff Petry, Paul Byron, Jake Allen, Dale Weise, Tomas Tatar, Ben Chiarot, Jordan Weal, Brendan Gallagher, Alex Belzile, Xavier Ouellet, Joel Armia, Phillip Danault, Charles Hudon, Brett Kulak, Gustav Olofsson, Artturi Lehkonen, Jonathan Drouin, Max Domi, Christian Folin, Jake Evans, Noah Juulsen, Michael McNiven, Charlie Lindgren, Victor Mete, Cale Fleury, Nick Suzuki, Cayden Primeau, Ryan Poehling, Jesperi Kotkaniemi, /api/v1/people/8477460, /api/v1/people/8479990, /api/v1/people/8470642, /api/v1/people/8471679, /api/v1/people/8473507, /api/v1/people/8474038, /api/v1/people/8474596, /api/v1/people/8474668, /api/v1/people/8475193, /api/v1/people/8475279, /api/v1/people/8475738, /api/v1/people/8475848, /api/v1/people/8475968, /api/v1/people/8476443, /api/v1/people/8476469, /api/v1/people/8476479, /api/v1/people/8476948, /api/v1/people/8476967, /api/v1/people/8477467, /api/v1/people/8477476, /api/v1/people/8477494, /api/v1/people/8477503, /api/v1/people/8477850, /api/v1/people/8478133, /api/v1/people/8478454, /api/v1/people/8478933, /api/v1/people/8479292, /api/v1/people/8479376, /api/v1/people/8479985, /api/v1/people/8480018, /api/v1/people/8480051, /api/v1/people/8480068, /api/v1/people/8480829, C, D, D, G, D, L, G, R, L, D, C, R, R, D, R, C, L, D, D, L, L, C, D, C, D, G, G, D, D, C, G, C, C, Center, Defenseman, Defenseman, Goalie, Defenseman, Left Wing, Goalie, Right Wing, Left Wing, Defenseman, Center, Right Wing, Right Wing, Defenseman, Right Wing, Center, Left Wing, Defenseman, Defenseman, Left Wing, Left Wing, Center, Defenseman, Center, Defenseman, Goalie, Goalie, Defenseman, Defenseman, Center, Goalie, Center, Center, Forward, Defenseman, Defenseman, Goalie, Defenseman, Forward, Goalie, Forward, Forward, Defenseman, Forward, Forward, Forward, Defenseman, Forward, Forward, Forward, Defenseman, Defenseman, Forward, Forward, Forward, Defenseman, Forward, Defenseman, Goalie, Goalie, Defenseman, Defenseman, Forward, Goalie, Forward, Forward, C, D, D, G, D, LW, G, RW, LW, D, C, RW, RW, D, RW, C, LW, D, D, LW, LW, C, D, C, D, G, G, D, D, C, G, C, C
    ## 9                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 31, 41, 81, 9, 51, 89, 74, 53, 5, 71, 28, 49, 35, 10, 13, 79, 39, 36, 72, 86, 38, 22, 7, 8475195, 8467950, 8468493, 8471676, 8473573, 8474571, 8474697, 8476285, 8476422, 8476919, 8477015, 8477149, 8477405, 8477407, 8477426, 8477963, 8477971, 8478400, 8478469, 8478846, 8478870, 8479458, 8480801, Anders Nilsson, Craig Anderson, Ron Hainsey, Bobby Ryan, Artem Anisimov, Mikkel Boedker, Mark Borowiecki, Matthew Peca, Mike Reilly, Chris Tierney, Connor Brown, Scott Sabourin, Marcus Hogberg, Anthony Duclair, Nick Paul, Jayce Hawryluk, Andreas Englund, Colin White, Thomas Chabot, Christian Wolanin, Rudolfs Balcers, Nikita Zaitsev, Brady Tkachuk, /api/v1/people/8475195, /api/v1/people/8467950, /api/v1/people/8468493, /api/v1/people/8471676, /api/v1/people/8473573, /api/v1/people/8474571, /api/v1/people/8474697, /api/v1/people/8476285, /api/v1/people/8476422, /api/v1/people/8476919, /api/v1/people/8477015, /api/v1/people/8477149, /api/v1/people/8477405, /api/v1/people/8477407, /api/v1/people/8477426, /api/v1/people/8477963, /api/v1/people/8477971, /api/v1/people/8478400, /api/v1/people/8478469, /api/v1/people/8478846, /api/v1/people/8478870, /api/v1/people/8479458, /api/v1/people/8480801, G, G, D, R, C, L, D, C, D, C, R, R, G, L, L, R, D, C, D, D, L, D, L, Goalie, Goalie, Defenseman, Right Wing, Center, Left Wing, Defenseman, Center, Defenseman, Center, Right Wing, Right Wing, Goalie, Left Wing, Left Wing, Right Wing, Defenseman, Center, Defenseman, Defenseman, Left Wing, Defenseman, Left Wing, Goalie, Goalie, Defenseman, Forward, Forward, Forward, Defenseman, Forward, Defenseman, Forward, Forward, Forward, Goalie, Forward, Forward, Forward, Defenseman, Forward, Defenseman, Defenseman, Forward, Defenseman, Forward, G, G, D, RW, C, LW, D, C, D, C, RW, RW, G, LW, LW, RW, D, C, D, D, LW, D, LW
    ## 10                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               19, 8, 73, 91, 94, 52, 3, 11, 36, 31, 44, 83, 15, 18, 61, 33, 88, 47, 23, 16, 9, 62, 50, 34, 60, 48, 38, 89, 65, 8469455, 8474162, 8475160, 8475166, 8475197, 8475716, 8475718, 8475786, 8475789, 8475883, 8476853, 8476879, 8477021, 8477341, 8477464, 8477512, 8477939, 8478115, 8478408, 8478483, 8478542, 8478843, 8479288, 8479318, 8479361, 8480157, 8480873, 8481582, 8481624, Jason Spezza, Jake Muzzin, Kyle Clifford, John Tavares, Tyson Barrie, Martin Marincin, Justin Holl, Zach Hyman, Jack Campbell, Frederik Andersen, Morgan Rielly, Cody Ceci, Alexander Kerfoot, Andreas Johnsson, Nic Petan, Frederik Gauthier, William Nylander, Pierre Engvall, Travis Dermott, Mitchell Marner, Evan Rodrigues, Denis Malgin, Kasimir Kaskisuo, Auston Matthews, Joseph Woll, Calle Rosen, Rasmus Sandin, Nicholas Robertson, Ilya Mikheyev, /api/v1/people/8469455, /api/v1/people/8474162, /api/v1/people/8475160, /api/v1/people/8475166, /api/v1/people/8475197, /api/v1/people/8475716, /api/v1/people/8475718, /api/v1/people/8475786, /api/v1/people/8475789, /api/v1/people/8475883, /api/v1/people/8476853, /api/v1/people/8476879, /api/v1/people/8477021, /api/v1/people/8477341, /api/v1/people/8477464, /api/v1/people/8477512, /api/v1/people/8477939, /api/v1/people/8478115, /api/v1/people/8478408, /api/v1/people/8478483, /api/v1/people/8478542, /api/v1/people/8478843, /api/v1/people/8479288, /api/v1/people/8479318, /api/v1/people/8479361, /api/v1/people/8480157, /api/v1/people/8480873, /api/v1/people/8481582, /api/v1/people/8481624, C, D, L, C, D, D, D, L, G, G, D, D, C, L, C, C, R, L, D, R, C, C, G, C, G, D, D, L, R, Center, Defenseman, Left Wing, Center, Defenseman, Defenseman, Defenseman, Left Wing, Goalie, Goalie, Defenseman, Defenseman, Center, Left Wing, Center, Center, Right Wing, Left Wing, Defenseman, Right Wing, Center, Center, Goalie, Center, Goalie, Defenseman, Defenseman, Left Wing, Right Wing, Forward, Defenseman, Forward, Forward, Defenseman, Defenseman, Defenseman, Forward, Goalie, Goalie, Defenseman, Defenseman, Forward, Forward, Forward, Forward, Forward, Forward, Defenseman, Forward, Forward, Forward, Goalie, Forward, Goalie, Defenseman, Defenseman, Forward, Forward, C, D, LW, C, D, D, D, LW, G, G, D, D, C, LW, C, C, RW, LW, D, RW, C, C, G, C, G, D, D, LW, RW
    ## 11                                                                                                                                                                                                                                                                                                                                                                                     22, 14, 47, 11, 51, 45, 21, 34, 18, 28, 31, 16, 6, 19, 76, 86, 48, 23, 74, 57, 4, 39, 55, 13, 64, 20, 78, 24, 43, 88, 37, 8477488, 8468508, 8473503, 8473533, 8474581, 8475222, 8475799, 8475852, 8476288, 8476323, 8476341, 8476389, 8476441, 8476462, 8476869, 8476882, 8476921, 8476934, 8476958, 8477845, 8477938, 8477968, 8477981, 8477998, 8478056, 8478427, 8478904, 8479402, 8479987, 8480039, 8480830, Brett Pesce, Justin Williams, James Reimer, Jordan Staal, Jake Gardiner, Sami Vatanen, Nino Niederreiter, Petr Mrazek, Ryan Dzingel, Max McCormick, Anton Forsberg, Vincent Trocheck, Joel Edmundson, Dougie Hamilton, Brady Skjei, Teuvo Teravainen, Jordan Martinook, Brock McGinn, Jaccob Slavin, Trevor van Riemsdyk, Haydn Fleury, Alex Nedeljkovic, Roland McKeown, Warren Foegele, Clark Bishop, Sebastian Aho, Steven Lorentz, Jake Bean, Morgan Geekie, Martin Necas, Andrei Svechnikov, /api/v1/people/8477488, /api/v1/people/8468508, /api/v1/people/8473503, /api/v1/people/8473533, /api/v1/people/8474581, /api/v1/people/8475222, /api/v1/people/8475799, /api/v1/people/8475852, /api/v1/people/8476288, /api/v1/people/8476323, /api/v1/people/8476341, /api/v1/people/8476389, /api/v1/people/8476441, /api/v1/people/8476462, /api/v1/people/8476869, /api/v1/people/8476882, /api/v1/people/8476921, /api/v1/people/8476934, /api/v1/people/8476958, /api/v1/people/8477845, /api/v1/people/8477938, /api/v1/people/8477968, /api/v1/people/8477981, /api/v1/people/8477998, /api/v1/people/8478056, /api/v1/people/8478427, /api/v1/people/8478904, /api/v1/people/8479402, /api/v1/people/8479987, /api/v1/people/8480039, /api/v1/people/8480830, D, R, G, C, D, D, R, G, C, L, G, C, D, D, D, L, L, L, D, D, D, G, D, L, C, C, C, D, C, C, R, Defenseman, Right Wing, Goalie, Center, Defenseman, Defenseman, Right Wing, Goalie, Center, Left Wing, Goalie, Center, Defenseman, Defenseman, Defenseman, Left Wing, Left Wing, Left Wing, Defenseman, Defenseman, Defenseman, Goalie, Defenseman, Left Wing, Center, Center, Center, Defenseman, Center, Center, Right Wing, Defenseman, Forward, Goalie, Forward, Defenseman, Defenseman, Forward, Goalie, Forward, Forward, Goalie, Forward, Defenseman, Defenseman, Defenseman, Forward, Forward, Forward, Defenseman, Defenseman, Defenseman, Goalie, Defenseman, Forward, Forward, Forward, Forward, Defenseman, Forward, Forward, Forward, D, RW, G, C, D, D, RW, G, C, LW, G, C, D, D, D, LW, LW, LW, D, D, D, G, D, LW, C, C, C, D, C, C, RW
    ## 12                                                                                                                                                                                                                                                                                                                                                                                                                                                                           27, 9, 3, 6, 7, 63, 68, 56, 72, 10, 13, 11, 19, 60, 14, 52, 2, 30, 16, 5, 71, 73, 77, 33, 55, 28, 61, 22, 74, 25, 8480185, 8470619, 8471735, 8471873, 8474098, 8474149, 8474884, 8475287, 8475683, 8475792, 8475796, 8476456, 8476875, 8476904, 8476952, 8477346, 8477384, 8477475, 8477493, 8477932, 8478027, 8478211, 8478366, 8478470, 8478569, 8478839, 8479388, 8479597, 8480015, 8481442, Eetu Luostarinen, Brian Boyle, Keith Yandle, Anton Stralman, Colton Sceviour, Evgenii Dadonov, Mike Hoffman, Erik Haula, Sergei Bobrovsky, Brett Connolly, Mark Pysyk, Jonathan Huberdeau, Mike Matheson, Chris Driedger, Dominic Toninato, MacKenzie Weegar, Josh Brown, Philippe Desrosiers, Aleksander Barkov, Aaron Ekblad, Lucas Wallmark, Dryden Hunt, Frank Vatrano, Sam Montembeault, Noel Acciari, Aleksi Saarela, Riley Stillman, Chase Priskie, Owen Tippett, Brady Keeper, /api/v1/people/8480185, /api/v1/people/8470619, /api/v1/people/8471735, /api/v1/people/8471873, /api/v1/people/8474098, /api/v1/people/8474149, /api/v1/people/8474884, /api/v1/people/8475287, /api/v1/people/8475683, /api/v1/people/8475792, /api/v1/people/8475796, /api/v1/people/8476456, /api/v1/people/8476875, /api/v1/people/8476904, /api/v1/people/8476952, /api/v1/people/8477346, /api/v1/people/8477384, /api/v1/people/8477475, /api/v1/people/8477493, /api/v1/people/8477932, /api/v1/people/8478027, /api/v1/people/8478211, /api/v1/people/8478366, /api/v1/people/8478470, /api/v1/people/8478569, /api/v1/people/8478839, /api/v1/people/8479388, /api/v1/people/8479597, /api/v1/people/8480015, /api/v1/people/8481442, C, C, D, D, C, R, L, L, G, R, D, L, D, G, C, D, D, G, C, D, C, L, C, G, C, C, D, D, R, D, Center, Center, Defenseman, Defenseman, Center, Right Wing, Left Wing, Left Wing, Goalie, Right Wing, Defenseman, Left Wing, Defenseman, Goalie, Center, Defenseman, Defenseman, Goalie, Center, Defenseman, Center, Left Wing, Center, Goalie, Center, Center, Defenseman, Defenseman, Right Wing, Defenseman, Forward, Forward, Defenseman, Defenseman, Forward, Forward, Forward, Forward, Goalie, Forward, Defenseman, Forward, Defenseman, Goalie, Forward, Defenseman, Defenseman, Goalie, Forward, Defenseman, Forward, Forward, Forward, Goalie, Forward, Forward, Defenseman, Defenseman, Forward, Defenseman, C, C, D, D, C, RW, LW, LW, G, RW, D, LW, D, G, C, D, D, G, C, D, C, LW, C, G, C, C, D, D, RW, D
    ## 13                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      35, 55, 17, 22, 14, 27, 91, 24, 2, 9, 77, 29, 18, 20, 86, 19, 37, 88, 13, 23, 21, 81, 7, 67, 71, 98, 44, 8470147, 8470601, 8473986, 8474031, 8474034, 8474151, 8474564, 8474567, 8474568, 8474870, 8475167, 8475809, 8476292, 8476399, 8476453, 8476624, 8476826, 8476883, 8476975, 8477409, 8478010, 8478416, 8478472, 8478477, 8478519, 8479410, 8480172, Curtis McElhinney, Braydon Coburn, Alex Killorn, Kevin Shattenkirk, Pat Maroon, Ryan McDonagh, Steven Stamkos, Zach Bogosian, Luke Schenn, Tyler Johnson, Victor Hedman, Scott Wedgewood, Ondrej Palat, Blake Coleman, Nikita Kucherov, Barclay Goodrow, Yanni Gourde, Andrei Vasilevskiy, Cedric Paquette, Carter Verhaeghe, Brayden Point, Erik Cernak, Mathieu Joseph, Mitchell Stephens, Anthony Cirelli, Mikhail Sergachev, Jan Rutta, /api/v1/people/8470147, /api/v1/people/8470601, /api/v1/people/8473986, /api/v1/people/8474031, /api/v1/people/8474034, /api/v1/people/8474151, /api/v1/people/8474564, /api/v1/people/8474567, /api/v1/people/8474568, /api/v1/people/8474870, /api/v1/people/8475167, /api/v1/people/8475809, /api/v1/people/8476292, /api/v1/people/8476399, /api/v1/people/8476453, /api/v1/people/8476624, /api/v1/people/8476826, /api/v1/people/8476883, /api/v1/people/8476975, /api/v1/people/8477409, /api/v1/people/8478010, /api/v1/people/8478416, /api/v1/people/8478472, /api/v1/people/8478477, /api/v1/people/8478519, /api/v1/people/8479410, /api/v1/people/8480172, G, D, L, D, L, D, C, D, D, C, D, G, L, C, R, C, C, G, C, C, C, D, R, C, C, D, D, Goalie, Defenseman, Left Wing, Defenseman, Left Wing, Defenseman, Center, Defenseman, Defenseman, Center, Defenseman, Goalie, Left Wing, Center, Right Wing, Center, Center, Goalie, Center, Center, Center, Defenseman, Right Wing, Center, Center, Defenseman, Defenseman, Goalie, Defenseman, Forward, Defenseman, Forward, Defenseman, Forward, Defenseman, Defenseman, Forward, Defenseman, Goalie, Forward, Forward, Forward, Forward, Forward, Goalie, Forward, Forward, Forward, Defenseman, Forward, Forward, Forward, Defenseman, Defenseman, G, D, LW, D, LW, D, C, D, D, C, D, G, LW, C, RW, C, C, G, C, C, C, D, RW, C, C, D, D
    ## 14                                                                                                                                                           63, 30, 40, 27, 17, 8, 77, 19, 62, 20, 74, 70, 9, 14, 3, 26, 4, 33, 92, 72, 43, 64, 78, 16, 1, 21, 13, 41, 34, 10, 47, 6, 42, 24, 8478063, 8478492, 8479516, 8480823, 8469454, 8471214, 8471698, 8473563, 8474176, 8474189, 8474590, 8474651, 8475200, 8475209, 8475324, 8475343, 8475455, 8475462, 8475744, 8476329, 8476880, 8477314, 8477343, 8477544, 8477831, 8477903, 8477944, 8477970, 8478399, 8478466, 8479359, 8479482, 8480796, 8481580, Shane Gersich, Ilya Samsonov, Garrett Pilon, Alexander Alexeyev, Ilya Kovalchuk, Alex Ovechkin, T.J. Oshie, Nicklas Backstrom, Carl Hagelin, Lars Eller, John Carlson, Braden Holtby, Dmitry Orlov, Richard Panik, Nick Jensen, Nic Dowd, Brenden Dillon, Radko Gudas, Evgeny Kuznetsov, Travis Boyd, Tom Wilson, Brian Pinho, Tyler Lewington, Philippe Maillet, Pheonix Copley, Garnet Hathaway, Jakub Vrana, Vitek Vanecek, Jonas Siegenthaler, Daniel Sprong, Beck Malenstyn, Michal Kempny, Martin Fehervary, Connor McMichael, /api/v1/people/8478063, /api/v1/people/8478492, /api/v1/people/8479516, /api/v1/people/8480823, /api/v1/people/8469454, /api/v1/people/8471214, /api/v1/people/8471698, /api/v1/people/8473563, /api/v1/people/8474176, /api/v1/people/8474189, /api/v1/people/8474590, /api/v1/people/8474651, /api/v1/people/8475200, /api/v1/people/8475209, /api/v1/people/8475324, /api/v1/people/8475343, /api/v1/people/8475455, /api/v1/people/8475462, /api/v1/people/8475744, /api/v1/people/8476329, /api/v1/people/8476880, /api/v1/people/8477314, /api/v1/people/8477343, /api/v1/people/8477544, /api/v1/people/8477831, /api/v1/people/8477903, /api/v1/people/8477944, /api/v1/people/8477970, /api/v1/people/8478399, /api/v1/people/8478466, /api/v1/people/8479359, /api/v1/people/8479482, /api/v1/people/8480796, /api/v1/people/8481580, L, G, C, D, L, L, R, C, L, C, D, G, D, R, D, C, D, D, C, C, R, C, D, C, G, R, L, G, D, R, L, D, D, C, Left Wing, Goalie, Center, Defenseman, Left Wing, Left Wing, Right Wing, Center, Left Wing, Center, Defenseman, Goalie, Defenseman, Right Wing, Defenseman, Center, Defenseman, Defenseman, Center, Center, Right Wing, Center, Defenseman, Center, Goalie, Right Wing, Left Wing, Goalie, Defenseman, Right Wing, Left Wing, Defenseman, Defenseman, Center, Forward, Goalie, Forward, Defenseman, Forward, Forward, Forward, Forward, Forward, Forward, Defenseman, Goalie, Defenseman, Forward, Defenseman, Forward, Defenseman, Defenseman, Forward, Forward, Forward, Forward, Defenseman, Forward, Goalie, Forward, Forward, Goalie, Defenseman, Forward, Forward, Defenseman, Defenseman, Forward, LW, G, C, D, LW, LW, RW, C, LW, C, D, G, D, RW, D, C, D, D, C, C, RW, C, D, C, G, RW, LW, G, D, RW, LW, D, D, C
    ## 15 7, 65, 90, 43, 71, 52, 2, 50, 19, 88, 44, 55, 20, 5, 6, 30, 68, 8, 22, 47, 95, 36, 17, 12, 92, 91, 46, 38, 58, 64, 60, 74, 75, 27, 34, 77, 8470607, 8476381, 8477035, 8479342, 8480798, 8481147, 8470281, 8470645, 8473604, 8474141, 8475177, 8476372, 8476438, 8476473, 8476874, 8476876, 8476886, 8477330, 8477846, 8477961, 8478106, 8478146, 8478440, 8479337, 8479423, 8479465, 8479523, 8479542, 8480025, 8480144, 8480420, 8480814, 8480831, 8480871, 8480947, 8481523, Brent Seabrook, Andrew Shaw, Matt Tomkins, Chad Krys, Philipp Kurashev, Reese Johnson, Duncan Keith, Corey Crawford, Jonathan Toews, Patrick Kane, Calvin de Haan, Nick Seeler, Brandon Saad, Connor Murphy, Olli Maatta, Malcolm Subban, Slater Koekkoek, Dominik Kubalik, Ryan Carpenter, John Quenneville, Dylan Sikura, Matthew Highmore, Dylan Strome, Alex DeBrincat, Alex Nylander, Drake Caggiula, Lucas Carlsson, Brandon Hagel, MacKenzie Entwistle, David Kampf, Collin Delia, Nicolas Beaudin, Alec Regula, Adam Boqvist, Kevin Lankinen, Kirby Dach, /api/v1/people/8470607, /api/v1/people/8476381, /api/v1/people/8477035, /api/v1/people/8479342, /api/v1/people/8480798, /api/v1/people/8481147, /api/v1/people/8470281, /api/v1/people/8470645, /api/v1/people/8473604, /api/v1/people/8474141, /api/v1/people/8475177, /api/v1/people/8476372, /api/v1/people/8476438, /api/v1/people/8476473, /api/v1/people/8476874, /api/v1/people/8476876, /api/v1/people/8476886, /api/v1/people/8477330, /api/v1/people/8477846, /api/v1/people/8477961, /api/v1/people/8478106, /api/v1/people/8478146, /api/v1/people/8478440, /api/v1/people/8479337, /api/v1/people/8479423, /api/v1/people/8479465, /api/v1/people/8479523, /api/v1/people/8479542, /api/v1/people/8480025, /api/v1/people/8480144, /api/v1/people/8480420, /api/v1/people/8480814, /api/v1/people/8480831, /api/v1/people/8480871, /api/v1/people/8480947, /api/v1/people/8481523, D, R, G, D, C, C, D, G, C, R, D, D, L, D, D, G, D, L, C, C, R, C, C, L, L, C, D, L, R, C, G, D, D, D, G, C, Defenseman, Right Wing, Goalie, Defenseman, Center, Center, Defenseman, Goalie, Center, Right Wing, Defenseman, Defenseman, Left Wing, Defenseman, Defenseman, Goalie, Defenseman, Left Wing, Center, Center, Right Wing, Center, Center, Left Wing, Left Wing, Center, Defenseman, Left Wing, Right Wing, Center, Goalie, Defenseman, Defenseman, Defenseman, Goalie, Center, Defenseman, Forward, Goalie, Defenseman, Forward, Forward, Defenseman, Goalie, Forward, Forward, Defenseman, Defenseman, Forward, Defenseman, Defenseman, Goalie, Defenseman, Forward, Forward, Forward, Forward, Forward, Forward, Forward, Forward, Forward, Defenseman, Forward, Forward, Forward, Goalie, Defenseman, Defenseman, Defenseman, Goalie, Forward, D, RW, G, D, C, C, D, G, C, RW, D, D, LW, D, D, G, D, LW, C, C, RW, C, C, LW, LW, C, D, LW, RW, C, G, D, D, D, G, C
    ## 16                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         65, 21, 17, 28, 51, 83, 81, 52, 35, 8, 43, 3, 45, 89, 26, 22, 41, 73, 74, 59, 39, 29, 71, 14, 70, 15, 8477215, 8479395, 8479425, 8480184, 8470047, 8470110, 8470144, 8470318, 8470657, 8471716, 8471794, 8473415, 8473541, 8474040, 8474597, 8475747, 8476822, 8477454, 8477474, 8477479, 8477511, 8477943, 8477946, 8477952, 8478036, 8478857, Danny DeKeyser, Dennis Cholowski, Filip Hronek, Gustav Lindstrom, Valtteri Filppula, Trevor Daley, Frans Nielsen, Jonathan Ericsson, Jimmy Howard, Justin Abdelkader, Darren Helm, Alex Biega, Jonathan Bernier, Sam Gagner, Cody Goloubef, Patrik Nemeth, Luke Glendening, Adam Erne, Madison Bowey, Tyler Bertuzzi, Anthony Mantha, Brendan Perlini, Dylan Larkin, Robby Fabbri, Christoffer Ehn, Dmytro Timashov, /api/v1/people/8477215, /api/v1/people/8479395, /api/v1/people/8479425, /api/v1/people/8480184, /api/v1/people/8470047, /api/v1/people/8470110, /api/v1/people/8470144, /api/v1/people/8470318, /api/v1/people/8470657, /api/v1/people/8471716, /api/v1/people/8471794, /api/v1/people/8473415, /api/v1/people/8473541, /api/v1/people/8474040, /api/v1/people/8474597, /api/v1/people/8475747, /api/v1/people/8476822, /api/v1/people/8477454, /api/v1/people/8477474, /api/v1/people/8477479, /api/v1/people/8477511, /api/v1/people/8477943, /api/v1/people/8477946, /api/v1/people/8477952, /api/v1/people/8478036, /api/v1/people/8478857, D, D, D, D, C, D, C, D, G, L, L, D, G, C, D, D, C, L, D, L, R, L, C, C, C, L, Defenseman, Defenseman, Defenseman, Defenseman, Center, Defenseman, Center, Defenseman, Goalie, Left Wing, Left Wing, Defenseman, Goalie, Center, Defenseman, Defenseman, Center, Left Wing, Defenseman, Left Wing, Right Wing, Left Wing, Center, Center, Center, Left Wing, Defenseman, Defenseman, Defenseman, Defenseman, Forward, Defenseman, Forward, Defenseman, Goalie, Forward, Forward, Defenseman, Goalie, Forward, Defenseman, Defenseman, Forward, Forward, Defenseman, Forward, Forward, Forward, Forward, Forward, Forward, Forward, D, D, D, D, C, D, C, D, G, LW, LW, D, G, C, D, D, C, LW, D, LW, RW, LW, C, C, C, LW
    ## 17                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                28, 5, 35, 22, 13, 8, 7, 59, 95, 4, 14, 15, 19, 51, 92, 24, 64, 42, 23, 9, 10, 1, 74, 47, 26, 33, 32, 45, 39, 57, 8480009, 8469465, 8471469, 8473560, 8474009, 8474068, 8474134, 8474600, 8475168, 8475176, 8475218, 8475225, 8475714, 8475766, 8475793, 8475797, 8475798, 8476278, 8476428, 8476887, 8476925, 8477234, 8477424, 8477446, 8477901, 8478042, 8478508, 8478851, 8478971, 8479371, Eeli Tolvanen, Dan Hamhuis, Pekka Rinne, Korbinian Holzer, Nick Bonino, Kyle Turris, Yannick Weber, Roman Josi, Matt Duchene, Ryan Ellis, Mattias Ekholm, Craig Smith, Calle Jarnkrok, Austin Watson, Ryan Johansen, Jarred Tinordi, Mikael Granlund, Colin Blackwell, Rocco Grimaldi, Filip Forsberg, Colton Sissons, Troy Grosenick, Juuse Saros, Michael McCarron, Daniel Carr, Viktor Arvidsson, Yakov Trenin, Alexandre Carrier, Connor Ingram, Dante Fabbro, /api/v1/people/8480009, /api/v1/people/8469465, /api/v1/people/8471469, /api/v1/people/8473560, /api/v1/people/8474009, /api/v1/people/8474068, /api/v1/people/8474134, /api/v1/people/8474600, /api/v1/people/8475168, /api/v1/people/8475176, /api/v1/people/8475218, /api/v1/people/8475225, /api/v1/people/8475714, /api/v1/people/8475766, /api/v1/people/8475793, /api/v1/people/8475797, /api/v1/people/8475798, /api/v1/people/8476278, /api/v1/people/8476428, /api/v1/people/8476887, /api/v1/people/8476925, /api/v1/people/8477234, /api/v1/people/8477424, /api/v1/people/8477446, /api/v1/people/8477901, /api/v1/people/8478042, /api/v1/people/8478508, /api/v1/people/8478851, /api/v1/people/8478971, /api/v1/people/8479371, R, D, G, D, C, C, D, D, C, D, D, R, C, L, C, D, C, C, R, L, C, G, G, R, L, R, C, D, G, D, Right Wing, Defenseman, Goalie, Defenseman, Center, Center, Defenseman, Defenseman, Center, Defenseman, Defenseman, Right Wing, Center, Left Wing, Center, Defenseman, Center, Center, Right Wing, Left Wing, Center, Goalie, Goalie, Right Wing, Left Wing, Right Wing, Center, Defenseman, Goalie, Defenseman, Forward, Defenseman, Goalie, Defenseman, Forward, Forward, Defenseman, Defenseman, Forward, Defenseman, Defenseman, Forward, Forward, Forward, Forward, Defenseman, Forward, Forward, Forward, Forward, Forward, Goalie, Goalie, Forward, Forward, Forward, Forward, Defenseman, Goalie, Defenseman, RW, D, G, D, C, C, D, D, C, D, D, RW, C, LW, C, D, C, C, RW, LW, C, G, G, RW, LW, RW, C, D, G, D
    ## 18                                                                                                                                                                                                                                                                                                                                                                      19, 20, 36, 57, 4, 41, 27, 6, 21, 90, 10, 72, 91, 17, 50, 51, 55, 70, 28, 61, 12, 49, 46, 35, 53, 9, 29, 77, 33, 37, 18, 8470151, 8470257, 8471426, 8474102, 8474125, 8474145, 8474565, 8474618, 8475098, 8475158, 8475170, 8475753, 8475765, 8475768, 8476412, 8476884, 8476892, 8476897, 8476907, 8477455, 8477482, 8477964, 8478013, 8478024, 8478040, 8478104, 8478407, 8478859, 8479385, 8480011, 8480023, Jay Bouwmeester, Alexander Steen, Troy Brouwer, David Perron, Carl Gunnarsson, Robert Bortuzzo, Alex Pietrangelo, Marco Scandella, Tyler Bozak, Ryan O'Reilly, Brayden Schenn, Justin Faulk, Vladimir Tarasenko, Jaden Schwartz, Jordan Binnington, Derrick Pouliot, Colton Parayko, Oskar Sundqvist, Mackenzie MacEachern, Jacob de la Rose, Zach Sanford, Ivan Barbashev, Jake Walman, Ville Husso, Austin Poganski, Sammy Blais, Vince Dunn, Niko Mikkola, Jordan Kyrou, Klim Kostin, Robert Thomas, /api/v1/people/8470151, /api/v1/people/8470257, /api/v1/people/8471426, /api/v1/people/8474102, /api/v1/people/8474125, /api/v1/people/8474145, /api/v1/people/8474565, /api/v1/people/8474618, /api/v1/people/8475098, /api/v1/people/8475158, /api/v1/people/8475170, /api/v1/people/8475753, /api/v1/people/8475765, /api/v1/people/8475768, /api/v1/people/8476412, /api/v1/people/8476884, /api/v1/people/8476892, /api/v1/people/8476897, /api/v1/people/8476907, /api/v1/people/8477455, /api/v1/people/8477482, /api/v1/people/8477964, /api/v1/people/8478013, /api/v1/people/8478024, /api/v1/people/8478040, /api/v1/people/8478104, /api/v1/people/8478407, /api/v1/people/8478859, /api/v1/people/8479385, /api/v1/people/8480011, /api/v1/people/8480023, D, L, R, L, D, D, D, D, C, C, C, D, R, L, G, D, D, C, L, L, L, C, D, G, R, L, D, D, C, C, C, Defenseman, Left Wing, Right Wing, Left Wing, Defenseman, Defenseman, Defenseman, Defenseman, Center, Center, Center, Defenseman, Right Wing, Left Wing, Goalie, Defenseman, Defenseman, Center, Left Wing, Left Wing, Left Wing, Center, Defenseman, Goalie, Right Wing, Left Wing, Defenseman, Defenseman, Center, Center, Center, Defenseman, Forward, Forward, Forward, Defenseman, Defenseman, Defenseman, Defenseman, Forward, Forward, Forward, Defenseman, Forward, Forward, Goalie, Defenseman, Defenseman, Forward, Forward, Forward, Forward, Forward, Defenseman, Goalie, Forward, Forward, Defenseman, Defenseman, Forward, Forward, Forward, D, LW, RW, LW, D, D, D, D, C, C, C, D, RW, LW, G, D, D, C, LW, LW, LW, C, D, G, RW, LW, D, D, C, C, C
    ## 19                                                                                                                                                                                                                                                                                                                                      24, 8, 5, 17, 11, 26, 7, 36, 38, 39, 20, 13, 16, 89, 77, 32, 56, 53, 28, 23, 93, 88, 55, 4, 58, 27, 10, 19, 29, 33, 50, 45, 8474612, 8479976, 8470966, 8473473, 8474150, 8474628, 8474673, 8474736, 8475278, 8475660, 8475762, 8476346, 8476356, 8476409, 8476873, 8476903, 8476979, 8477210, 8477496, 8477497, 8477935, 8478233, 8478396, 8478397, 8478430, 8478512, 8478585, 8479314, 8479346, 8479496, 8481501, 8481630, Travis Hamonic, Juuso Valimaki, Mark Giordano, Milan Lucic, Mikael Backlund, Michael Stone, TJ Brodie, Zac Rinaldo, Byron Froese, Cam Talbot, Derek Forbort, Johnny Gaudreau, Tobias Rieder, Alan Quine, Mark Jankowski, Jon Gillies, Erik Gustafsson, Buddy Robinson, Elias Lindholm, Sean Monahan, Sam Bennett, Andrew Mangiapane, Noah Hanifin, Rasmus Andersson, Oliver Kylington, Austin Czarnik, Derek Ryan, Matthew Tkachuk, Dillon Dube, David Rittich, Artyom Zagidulin, Alexander Yelesin, /api/v1/people/8474612, /api/v1/people/8479976, /api/v1/people/8470966, /api/v1/people/8473473, /api/v1/people/8474150, /api/v1/people/8474628, /api/v1/people/8474673, /api/v1/people/8474736, /api/v1/people/8475278, /api/v1/people/8475660, /api/v1/people/8475762, /api/v1/people/8476346, /api/v1/people/8476356, /api/v1/people/8476409, /api/v1/people/8476873, /api/v1/people/8476903, /api/v1/people/8476979, /api/v1/people/8477210, /api/v1/people/8477496, /api/v1/people/8477497, /api/v1/people/8477935, /api/v1/people/8478233, /api/v1/people/8478396, /api/v1/people/8478397, /api/v1/people/8478430, /api/v1/people/8478512, /api/v1/people/8478585, /api/v1/people/8479314, /api/v1/people/8479346, /api/v1/people/8479496, /api/v1/people/8481501, /api/v1/people/8481630, D, D, D, L, C, D, D, C, C, G, D, L, C, C, C, G, D, R, C, C, C, L, D, D, D, C, C, L, C, G, G, D, Defenseman, Defenseman, Defenseman, Left Wing, Center, Defenseman, Defenseman, Center, Center, Goalie, Defenseman, Left Wing, Center, Center, Center, Goalie, Defenseman, Right Wing, Center, Center, Center, Left Wing, Defenseman, Defenseman, Defenseman, Center, Center, Left Wing, Center, Goalie, Goalie, Defenseman, Defenseman, Defenseman, Defenseman, Forward, Forward, Defenseman, Defenseman, Forward, Forward, Goalie, Defenseman, Forward, Forward, Forward, Forward, Goalie, Defenseman, Forward, Forward, Forward, Forward, Forward, Defenseman, Defenseman, Defenseman, Forward, Forward, Forward, Forward, Goalie, Goalie, Defenseman, D, D, D, LW, C, D, D, C, C, G, D, LW, C, C, C, G, D, RW, C, C, C, LW, D, D, D, C, C, LW, C, G, G, D
    ## 20                                                                                                                                                                                                                        22, 6, 28, 35, 11, 44, 91, 7, 72, 31, 36, 83, 92, 90, 27, 95, 37, 29, 13, 16, 41, 54, 96, 17, 49, 20, 14, 8, 32, 15, 39, 25, 45, 8474569, 8473446, 8474013, 8474636, 8474685, 8474717, 8475172, 8475246, 8475820, 8475831, 8476391, 8476442, 8476455, 8476480, 8477435, 8477444, 8477456, 8477492, 8477501, 8477507, 8477930, 8478073, 8478420, 8479370, 8479398, 8479982, 8480032, 8480069, 8480112, 8480326, 8480925, 8481186, 8481524, Colin Wilson, Erik Johnson, Ian Cole, Michael Hutchinson, Matt Calvert, Mark Barberio, Nazem Kadri, Kevin Connauton, Joonas Donskoi, Philipp Grubauer, T.J. Tynan, Matt Nieto, Gabriel Landeskog, Vladislav Namestnikov, Ryan Graves, Andre Burakovsky, J.T. Compher, Nathan MacKinnon, Valeri Nichushkin, Nikita Zadorov, Pierre-Edouard Bellemare, Anton Lindholm, Mikko Rantanen, Tyson Jost, Samuel Girard, Conor Timmins, Shane Bowers, Cale Makar, Hunter Miska, Sheldon Dries, Pavel Francouz, Logan O'Connor, Bowen Byram, /api/v1/people/8474569, /api/v1/people/8473446, /api/v1/people/8474013, /api/v1/people/8474636, /api/v1/people/8474685, /api/v1/people/8474717, /api/v1/people/8475172, /api/v1/people/8475246, /api/v1/people/8475820, /api/v1/people/8475831, /api/v1/people/8476391, /api/v1/people/8476442, /api/v1/people/8476455, /api/v1/people/8476480, /api/v1/people/8477435, /api/v1/people/8477444, /api/v1/people/8477456, /api/v1/people/8477492, /api/v1/people/8477501, /api/v1/people/8477507, /api/v1/people/8477930, /api/v1/people/8478073, /api/v1/people/8478420, /api/v1/people/8479370, /api/v1/people/8479398, /api/v1/people/8479982, /api/v1/people/8480032, /api/v1/people/8480069, /api/v1/people/8480112, /api/v1/people/8480326, /api/v1/people/8480925, /api/v1/people/8481186, /api/v1/people/8481524, C, D, D, G, L, D, C, D, R, G, C, L, L, C, D, L, L, C, R, D, C, D, R, C, D, D, C, D, G, C, G, R, D, Center, Defenseman, Defenseman, Goalie, Left Wing, Defenseman, Center, Defenseman, Right Wing, Goalie, Center, Left Wing, Left Wing, Center, Defenseman, Left Wing, Left Wing, Center, Right Wing, Defenseman, Center, Defenseman, Right Wing, Center, Defenseman, Defenseman, Center, Defenseman, Goalie, Center, Goalie, Right Wing, Defenseman, Forward, Defenseman, Defenseman, Goalie, Forward, Defenseman, Forward, Defenseman, Forward, Goalie, Forward, Forward, Forward, Forward, Defenseman, Forward, Forward, Forward, Forward, Defenseman, Forward, Defenseman, Forward, Forward, Defenseman, Defenseman, Forward, Defenseman, Goalie, Forward, Goalie, Forward, Defenseman, C, D, D, G, LW, D, C, D, RW, G, C, LW, LW, C, D, LW, LW, C, RW, D, C, D, RW, C, D, D, C, D, G, C, G, RW, D
    ## 21                                                                                                                                                                                                                                                                                                                                                                                                70, 86, 10, 41, 18, 4, 63, 19, 39, 44, 23, 15, 93, 6, 77, 16, 28, 83, 25, 29, 84, 97, 65, 74, 82, 49, 52, 50, 56, 75, 91, 8480802, 8481598, 8481638, 8469608, 8471707, 8471729, 8474589, 8475156, 8475163, 8475178, 8475772, 8476326, 8476454, 8476457, 8476472, 8476915, 8476960, 8476988, 8477498, 8477934, 8478021, 8478402, 8478442, 8478451, 8478452, 8479347, 8479466, 8479973, 8479977, 8480803, 8481813, Ryan McLeod, Philip Broberg, Joakim Nygard, Mike Smith, James Neal, Kris Russell, Tyler Ennis, Mikko Koskinen, Alex Chiasson, Zack Kassian, Riley Sheahan, Josh Archibald, Ryan Nugent-Hopkins, Adam Larsson, Oscar Klefbom, Jujhar Khaira, Andreas Athanasiou, Matt Benning, Darnell Nurse, Leon Draisaitl, William Lagesson, Connor McDavid, Cooper Marody, Ethan Bear, Caleb Jones, Tyler Benson, Patrick Russell, Stuart Skinner, Kailer Yamamoto, Evan Bouchard, Gaetan Haas, /api/v1/people/8480802, /api/v1/people/8481598, /api/v1/people/8481638, /api/v1/people/8469608, /api/v1/people/8471707, /api/v1/people/8471729, /api/v1/people/8474589, /api/v1/people/8475156, /api/v1/people/8475163, /api/v1/people/8475178, /api/v1/people/8475772, /api/v1/people/8476326, /api/v1/people/8476454, /api/v1/people/8476457, /api/v1/people/8476472, /api/v1/people/8476915, /api/v1/people/8476960, /api/v1/people/8476988, /api/v1/people/8477498, /api/v1/people/8477934, /api/v1/people/8478021, /api/v1/people/8478402, /api/v1/people/8478442, /api/v1/people/8478451, /api/v1/people/8478452, /api/v1/people/8479347, /api/v1/people/8479466, /api/v1/people/8479973, /api/v1/people/8479977, /api/v1/people/8480803, /api/v1/people/8481813, C, D, L, G, L, D, C, G, R, R, C, R, C, D, D, L, L, D, D, C, D, C, C, D, D, L, R, G, R, D, C, Center, Defenseman, Left Wing, Goalie, Left Wing, Defenseman, Center, Goalie, Right Wing, Right Wing, Center, Right Wing, Center, Defenseman, Defenseman, Left Wing, Left Wing, Defenseman, Defenseman, Center, Defenseman, Center, Center, Defenseman, Defenseman, Left Wing, Right Wing, Goalie, Right Wing, Defenseman, Center, Forward, Defenseman, Forward, Goalie, Forward, Defenseman, Forward, Goalie, Forward, Forward, Forward, Forward, Forward, Defenseman, Defenseman, Forward, Forward, Defenseman, Defenseman, Forward, Defenseman, Forward, Forward, Defenseman, Defenseman, Forward, Forward, Goalie, Forward, Defenseman, Forward, C, D, LW, G, LW, D, C, G, RW, RW, C, RW, C, D, D, LW, LW, D, D, C, D, C, C, D, D, LW, RW, G, RW, D, C
    ## 22                                                                                                                                                                                                                                                                                                                                                                                                            21, 23, 20, 83, 57, 25, 4, 26, 8, 73, 30, 79, 44, 9, 70, 64, 38, 53, 18, 35, 6, 88, 63, 48, 51, 71, 40, 5, 43, 30, 3, 8470626, 8471303, 8474091, 8474291, 8474574, 8474593, 8474818, 8474849, 8475690, 8475726, 8475839, 8475907, 8476344, 8476468, 8476871, 8477353, 8477473, 8477500, 8477937, 8477967, 8478444, 8478874, 8478970, 8479355, 8479442, 8479772, 8480012, 8480147, 8480800, 8481478, 8481479, Loui Eriksson, Alexander Edler, Brandon Sutter, Jay Beagle, Tyler Myers, Jacob Markstrom, Jordie Benn, Antoine Roussel, Christopher Tanev, Tyler Toffoli, Louis Domingue, Micheal Ferland, Tyler Graovac, J.T. Miller, Tanner Pearson, Tyler Motte, Justin Bailey, Bo Horvat, Jake Virtanen, Thatcher Demko, Brock Boeser, Adam Gaudette, Jalen Chatfield, Olli Juolevi, Troy Stecher, Zack MacEwen, Elias Pettersson, Oscar Fantenberg, Quinn Hughes, Jake Kielly, Brogan Rafferty, /api/v1/people/8470626, /api/v1/people/8471303, /api/v1/people/8474091, /api/v1/people/8474291, /api/v1/people/8474574, /api/v1/people/8474593, /api/v1/people/8474818, /api/v1/people/8474849, /api/v1/people/8475690, /api/v1/people/8475726, /api/v1/people/8475839, /api/v1/people/8475907, /api/v1/people/8476344, /api/v1/people/8476468, /api/v1/people/8476871, /api/v1/people/8477353, /api/v1/people/8477473, /api/v1/people/8477500, /api/v1/people/8477937, /api/v1/people/8477967, /api/v1/people/8478444, /api/v1/people/8478874, /api/v1/people/8478970, /api/v1/people/8479355, /api/v1/people/8479442, /api/v1/people/8479772, /api/v1/people/8480012, /api/v1/people/8480147, /api/v1/people/8480800, /api/v1/people/8481478, /api/v1/people/8481479, L, D, C, C, D, G, D, L, D, R, G, L, C, C, L, C, R, C, R, G, R, C, D, D, D, C, C, D, D, G, D, Left Wing, Defenseman, Center, Center, Defenseman, Goalie, Defenseman, Left Wing, Defenseman, Right Wing, Goalie, Left Wing, Center, Center, Left Wing, Center, Right Wing, Center, Right Wing, Goalie, Right Wing, Center, Defenseman, Defenseman, Defenseman, Center, Center, Defenseman, Defenseman, Goalie, Defenseman, Forward, Defenseman, Forward, Forward, Defenseman, Goalie, Defenseman, Forward, Defenseman, Forward, Goalie, Forward, Forward, Forward, Forward, Forward, Forward, Forward, Forward, Goalie, Forward, Forward, Defenseman, Defenseman, Defenseman, Forward, Forward, Defenseman, Defenseman, Goalie, Defenseman, LW, D, C, C, D, G, D, LW, D, RW, G, LW, C, C, LW, C, RW, C, RW, G, RW, C, D, D, D, C, C, D, D, G, D
    ## 23                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             30, 15, 21, 44, 14, 33, 20, 26, 52, 4, 6, 42, 36, 67, 47, 29, 24, 22, 43, 32, 34, 49, 8468011, 8470612, 8470655, 8474584, 8474641, 8475164, 8475235, 8475461, 8475625, 8475764, 8475790, 8476312, 8476434, 8476483, 8476854, 8477043, 8477240, 8477947, 8478046, 8478491, 8479351, 8479368, Ryan Miller, Ryan Getzlaf, David Backes, Michael Del Zotto, Adam Henrique, Jakob Silfverberg, Nicolas Deslauriers, Andrew Agozzino, Matt Irwin, Cam Fowler, Erik Gudbranson, Josh Manson, John Gibson, Rickard Rakell, Hampus Lindholm, Christian Djoos, Carter Rowney, Sonny Milano, Danton Heinen, Jacob Larsson, Sam Steel, Max Jones, /api/v1/people/8468011, /api/v1/people/8470612, /api/v1/people/8470655, /api/v1/people/8474584, /api/v1/people/8474641, /api/v1/people/8475164, /api/v1/people/8475235, /api/v1/people/8475461, /api/v1/people/8475625, /api/v1/people/8475764, /api/v1/people/8475790, /api/v1/people/8476312, /api/v1/people/8476434, /api/v1/people/8476483, /api/v1/people/8476854, /api/v1/people/8477043, /api/v1/people/8477240, /api/v1/people/8477947, /api/v1/people/8478046, /api/v1/people/8478491, /api/v1/people/8479351, /api/v1/people/8479368, G, C, R, D, C, R, L, L, D, D, D, D, G, L, D, D, C, L, L, D, C, L, Goalie, Center, Right Wing, Defenseman, Center, Right Wing, Left Wing, Left Wing, Defenseman, Defenseman, Defenseman, Defenseman, Goalie, Left Wing, Defenseman, Defenseman, Center, Left Wing, Left Wing, Defenseman, Center, Left Wing, Goalie, Forward, Forward, Defenseman, Forward, Forward, Forward, Forward, Defenseman, Defenseman, Defenseman, Defenseman, Goalie, Forward, Defenseman, Defenseman, Forward, Forward, Forward, Defenseman, Forward, Forward, G, C, RW, D, C, RW, LW, LW, D, D, D, D, G, LW, D, D, C, LW, LW, D, C, LW
    ## 24                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            40, 45, 10, 16, 47, 15, 5, 35, 11, 30, 14, 37, 28, 91, 3, 42, 2, 12, 23, 13, 18, 24, 34, 29, 4, 60, 55, 25, 8471691, 8471392, 8470621, 8470794, 8471228, 8471260, 8471284, 8471418, 8471699, 8471750, 8473994, 8475413, 8475730, 8475794, 8475906, 8476166, 8476467, 8476889, 8476902, 8477406, 8477450, 8478449, 8478495, 8479979, 8480036, 8480848, 8481581, 8481641, Martin Hanzal, Roman Polak, Corey Perry, Joe Pavelski, Alexander Radulov, Blake Comeau, Andrej Sekera, Anton Khudobin, Andrew Cogliano, Ben Bishop, Jamie Benn, Justin Dowling, Stephen Johns, Tyler Seguin, John Klingberg, Taylor Fedun, Jamie Oleksiak, Radek Faksa, Esa Lindell, Mattias Janmark, Jason Dickinson, Roope Hintz, Denis Gurianov, Jake Oettinger, Miro Heiskanen, Ty Dellandrea, Thomas Harley, Joel Kiviranta, /api/v1/people/8471691, /api/v1/people/8471392, /api/v1/people/8470621, /api/v1/people/8470794, /api/v1/people/8471228, /api/v1/people/8471260, /api/v1/people/8471284, /api/v1/people/8471418, /api/v1/people/8471699, /api/v1/people/8471750, /api/v1/people/8473994, /api/v1/people/8475413, /api/v1/people/8475730, /api/v1/people/8475794, /api/v1/people/8475906, /api/v1/people/8476166, /api/v1/people/8476467, /api/v1/people/8476889, /api/v1/people/8476902, /api/v1/people/8477406, /api/v1/people/8477450, /api/v1/people/8478449, /api/v1/people/8478495, /api/v1/people/8479979, /api/v1/people/8480036, /api/v1/people/8480848, /api/v1/people/8481581, /api/v1/people/8481641, C, D, R, C, R, L, D, G, C, G, L, C, D, C, D, D, D, C, D, C, C, L, R, G, D, C, D, L, Center, Defenseman, Right Wing, Center, Right Wing, Left Wing, Defenseman, Goalie, Center, Goalie, Left Wing, Center, Defenseman, Center, Defenseman, Defenseman, Defenseman, Center, Defenseman, Center, Center, Left Wing, Right Wing, Goalie, Defenseman, Center, Defenseman, Left Wing, Forward, Defenseman, Forward, Forward, Forward, Forward, Defenseman, Goalie, Forward, Goalie, Forward, Forward, Defenseman, Forward, Defenseman, Defenseman, Defenseman, Forward, Defenseman, Forward, Forward, Forward, Forward, Goalie, Defenseman, Forward, Defenseman, Forward, C, D, RW, C, RW, LW, D, G, C, G, LW, C, D, C, D, D, D, C, D, C, C, LW, RW, G, D, C, D, LW
    ## 25                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           77, 23, 11, 32, 22, 8, 29, 74, 15, 6, 56, 40, 9, 10, 51, 3, 12, 44, 42, 19, 26, 46, 8470604, 8470606, 8471685, 8471734, 8473453, 8474563, 8476924, 8476947, 8477018, 8477046, 8477073, 8477361, 8477960, 8478020, 8478455, 8478911, 8479675, 8479998, 8480014, 8480113, 8480336, 8481481, Jeff Carter, Dustin Brown, Anze Kopitar, Jonathan Quick, Trevor Lewis, Drew Doughty, Martin Frk, Nikolai Prokhorkin, Ben Hutton, Joakim Ryan, Kurtis MacDermid, Calvin Petersen, Adrian Kempe, Michael Amadio, Austin Wagner, Matt Roy, Trevor Moore, Mikey Anderson, Gabriel Vilardi, Alex Iafallo, Sean Walker, Blake Lizotte, /api/v1/people/8470604, /api/v1/people/8470606, /api/v1/people/8471685, /api/v1/people/8471734, /api/v1/people/8473453, /api/v1/people/8474563, /api/v1/people/8476924, /api/v1/people/8476947, /api/v1/people/8477018, /api/v1/people/8477046, /api/v1/people/8477073, /api/v1/people/8477361, /api/v1/people/8477960, /api/v1/people/8478020, /api/v1/people/8478455, /api/v1/people/8478911, /api/v1/people/8479675, /api/v1/people/8479998, /api/v1/people/8480014, /api/v1/people/8480113, /api/v1/people/8480336, /api/v1/people/8481481, C, R, C, G, C, D, R, L, D, D, D, G, C, C, L, D, C, D, C, L, D, C, Center, Right Wing, Center, Goalie, Center, Defenseman, Right Wing, Left Wing, Defenseman, Defenseman, Defenseman, Goalie, Center, Center, Left Wing, Defenseman, Center, Defenseman, Center, Left Wing, Defenseman, Center, Forward, Forward, Forward, Goalie, Forward, Defenseman, Forward, Forward, Defenseman, Defenseman, Defenseman, Goalie, Forward, Forward, Forward, Defenseman, Forward, Defenseman, Forward, Forward, Defenseman, Forward, C, RW, C, G, C, D, RW, LW, D, D, D, G, C, C, LW, D, C, D, C, LW, D, C
    ## 26                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              65, 5, 48, 19, 88, 44, 39, 31, 9, 20, 72, 21, 11, 30, 68, 62, 67, 28, 73, 7, 38, 51, 70, 40, 46, 45, 8474578, 8474774, 8476881, 8466138, 8470613, 8471709, 8474053, 8474889, 8475169, 8475834, 8475841, 8475869, 8476474, 8477180, 8477922, 8478099, 8478136, 8478414, 8479393, 8479580, 8479983, 8480160, 8480384, 8480965, 8481516, 8481640, Erik Karlsson, Dalton Prout, Tomas Hertl, Joe Thornton, Brent Burns, Marc-Edouard Vlasic, Logan Couture, Martin Jones, Evander Kane, Marcus Sorensen, Tim Heed, Brandon Davidson, Stefan Noesen, Aaron Dell, Melker Karlsson, Kevin Labanc, Jacob Middleton, Timo Meier, Noah Gregor, Dylan Gambrell, Mario Ferraro, Radim Simek, Alexander True, Antti Suomela, Joel Kellman, Lean Bergmann, /api/v1/people/8474578, /api/v1/people/8474774, /api/v1/people/8476881, /api/v1/people/8466138, /api/v1/people/8470613, /api/v1/people/8471709, /api/v1/people/8474053, /api/v1/people/8474889, /api/v1/people/8475169, /api/v1/people/8475834, /api/v1/people/8475841, /api/v1/people/8475869, /api/v1/people/8476474, /api/v1/people/8477180, /api/v1/people/8477922, /api/v1/people/8478099, /api/v1/people/8478136, /api/v1/people/8478414, /api/v1/people/8479393, /api/v1/people/8479580, /api/v1/people/8479983, /api/v1/people/8480160, /api/v1/people/8480384, /api/v1/people/8480965, /api/v1/people/8481516, /api/v1/people/8481640, D, D, C, C, D, D, C, G, L, L, D, D, R, G, C, R, D, R, C, C, D, D, C, C, C, C, Defenseman, Defenseman, Center, Center, Defenseman, Defenseman, Center, Goalie, Left Wing, Left Wing, Defenseman, Defenseman, Right Wing, Goalie, Center, Right Wing, Defenseman, Right Wing, Center, Center, Defenseman, Defenseman, Center, Center, Center, Center, Defenseman, Defenseman, Forward, Forward, Defenseman, Defenseman, Forward, Goalie, Forward, Forward, Defenseman, Defenseman, Forward, Goalie, Forward, Forward, Defenseman, Forward, Forward, Forward, Defenseman, Defenseman, Forward, Forward, Forward, Forward, D, D, C, C, D, D, C, G, LW, LW, D, D, RW, G, C, RW, D, RW, C, C, D, D, C, C, C, C
    ## 27                                                                                                                                                                                                                                                                                                                    17, 77, 15, 24, 71, 20, 14, 13, 58, 38, 4, 27, 23, 74, 70, 28, 3, 10, 90, 8, 53, 46, 11, 44, 65, 2, 18, 42, 80, 52, 50, 19, 8471273, 8476981, 8481650, 8471804, 8473422, 8474062, 8474679, 8474715, 8475233, 8476432, 8476449, 8476850, 8476870, 8476913, 8476914, 8477416, 8477495, 8477505, 8478007, 8478460, 8478506, 8478567, 8478831, 8478882, 8478906, 8479369, 8479400, 8480074, 8480162, 8480205, 8480762, 8480853, Brandon Dubinsky, Josh Anderson, Jakob Lilja, Nathan Gerbe, Nick Foligno, Riley Nash, Gustav Nyquist, Cam Atkinson, David Savard, Boone Jenner, Scott Harrington, Ryan Murray, Stefan Matteau, Devin Shore, Joonas Korpisalo, Oliver Bjorkstrand, Seth Jones, Alexander Wennberg, Elvis Merzlikins, Zach Werenski, Gabriel Carlsson, Dean Kukan, Kevin Stenlund, Vladislav Gavrikov, Markus Nutivaara, Andrew Peeke, Pierre-Luc Dubois, Alexandre Texier, Matiss Kivlenieks, Emil Bemstrom, Eric Robinson, Liam Foudy, /api/v1/people/8471273, /api/v1/people/8476981, /api/v1/people/8481650, /api/v1/people/8471804, /api/v1/people/8473422, /api/v1/people/8474062, /api/v1/people/8474679, /api/v1/people/8474715, /api/v1/people/8475233, /api/v1/people/8476432, /api/v1/people/8476449, /api/v1/people/8476850, /api/v1/people/8476870, /api/v1/people/8476913, /api/v1/people/8476914, /api/v1/people/8477416, /api/v1/people/8477495, /api/v1/people/8477505, /api/v1/people/8478007, /api/v1/people/8478460, /api/v1/people/8478506, /api/v1/people/8478567, /api/v1/people/8478831, /api/v1/people/8478882, /api/v1/people/8478906, /api/v1/people/8479369, /api/v1/people/8479400, /api/v1/people/8480074, /api/v1/people/8480162, /api/v1/people/8480205, /api/v1/people/8480762, /api/v1/people/8480853, C, R, L, C, L, C, C, R, D, C, D, D, C, C, G, R, D, C, G, D, D, D, C, D, D, D, C, C, G, C, L, C, Center, Right Wing, Left Wing, Center, Left Wing, Center, Center, Right Wing, Defenseman, Center, Defenseman, Defenseman, Center, Center, Goalie, Right Wing, Defenseman, Center, Goalie, Defenseman, Defenseman, Defenseman, Center, Defenseman, Defenseman, Defenseman, Center, Center, Goalie, Center, Left Wing, Center, Forward, Forward, Forward, Forward, Forward, Forward, Forward, Forward, Defenseman, Forward, Defenseman, Defenseman, Forward, Forward, Goalie, Forward, Defenseman, Forward, Goalie, Defenseman, Defenseman, Defenseman, Forward, Defenseman, Defenseman, Defenseman, Forward, Forward, Goalie, Forward, Forward, Forward, C, RW, LW, C, LW, C, C, RW, D, C, D, D, C, C, G, RW, D, C, G, D, D, D, C, D, D, D, C, C, G, C, LW, C
    ## 28                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    9, 12, 20, 11, 40, 32, 46, 44, 17, 36, 37, 49, 25, 77, 27, 24, 41, 21, 38, 22, 6, 47, 31, 18, 14, 19, 61, 26, 42, 7, 8469459, 8470595, 8470600, 8470610, 8471227, 8471774, 8474716, 8474749, 8475220, 8475692, 8476415, 8476437, 8476463, 8476779, 8476851, 8476856, 8477366, 8477369, 8477451, 8477942, 8477987, 8478011, 8478039, 8478413, 8478493, 8479316, 8479734, 8479933, 8481443, 8481477, Mikko Koivu, Eric Staal, Ryan Suter, Zach Parise, Devan Dubnyk, Alex Stalock, Jared Spurgeon, Matt Bartkowski, Marcus Foligno, Mats Zuccarello, Kyle Rau, Victor Rask, Jonas Brodin, Brad Hunt, Alex Galchenyuk, Matt Dumba, Luke Johnson, Carson Soucy, Ryan Hartman, Kevin Fiala, Ryan Donato, Louie Belpedio, Kaapo Kahkonen, Jordan Greenway, Joel Eriksson Ek, Luke Kunin, Brennan Menell, Gerald Mayhew, Mat Robson, Nico Sturm, /api/v1/people/8469459, /api/v1/people/8470595, /api/v1/people/8470600, /api/v1/people/8470610, /api/v1/people/8471227, /api/v1/people/8471774, /api/v1/people/8474716, /api/v1/people/8474749, /api/v1/people/8475220, /api/v1/people/8475692, /api/v1/people/8476415, /api/v1/people/8476437, /api/v1/people/8476463, /api/v1/people/8476779, /api/v1/people/8476851, /api/v1/people/8476856, /api/v1/people/8477366, /api/v1/people/8477369, /api/v1/people/8477451, /api/v1/people/8477942, /api/v1/people/8477987, /api/v1/people/8478011, /api/v1/people/8478039, /api/v1/people/8478413, /api/v1/people/8478493, /api/v1/people/8479316, /api/v1/people/8479734, /api/v1/people/8479933, /api/v1/people/8481443, /api/v1/people/8481477, C, C, D, L, G, G, D, D, L, R, C, C, D, D, C, D, C, D, R, L, C, D, G, L, C, C, D, C, G, C, Center, Center, Defenseman, Left Wing, Goalie, Goalie, Defenseman, Defenseman, Left Wing, Right Wing, Center, Center, Defenseman, Defenseman, Center, Defenseman, Center, Defenseman, Right Wing, Left Wing, Center, Defenseman, Goalie, Left Wing, Center, Center, Defenseman, Center, Goalie, Center, Forward, Forward, Defenseman, Forward, Goalie, Goalie, Defenseman, Defenseman, Forward, Forward, Forward, Forward, Defenseman, Defenseman, Forward, Defenseman, Forward, Defenseman, Forward, Forward, Forward, Defenseman, Goalie, Forward, Forward, Forward, Defenseman, Forward, Goalie, Forward, C, C, D, LW, G, G, D, D, LW, RW, C, C, D, D, C, D, C, D, RW, LW, C, D, G, LW, C, C, D, C, G, C
    ## 29                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                18, 26, 85, 22, 5, 7, 20, 57, 2, 30, 12, 17, 38, 21, 55, 88, 37, 3, 9, 23, 44, 27, 81, 58, 28, 82, 8, 29, 60, 4, 8473412, 8471218, 8473618, 8473914, 8474579, 8475179, 8475236, 8475268, 8475868, 8476316, 8476331, 8476392, 8476400, 8476406, 8476460, 8476470, 8476945, 8477359, 8477429, 8477472, 8477504, 8477940, 8478398, 8478424, 8478458, 8478891, 8478915, 8479339, 8479574, 8480145, Bryan Little, Blake Wheeler, Mathieu Perreault, Mark Letestu, Luca Sbisa, Dmitry Kulikov, Cody Eakin, Gabriel Bourque, Anthony Bitetto, Laurent Brossoit, Dylan DeMelo, Adam Lowry, Logan Shaw, Nicholas Shore, Mark Scheifele, Nathan Beaulieu, Connor Hellebuyck, Tucker Poolman, Andrew Copp, Carl Dahlstrom, Josh Morrissey, Nikolaj Ehlers, Kyle Connor, Jansen Harkins, Jack Roslovic, Mason Appleton, Sami Niku, Patrik Laine, Mikhail Berdin, Neal Pionk, /api/v1/people/8473412, /api/v1/people/8471218, /api/v1/people/8473618, /api/v1/people/8473914, /api/v1/people/8474579, /api/v1/people/8475179, /api/v1/people/8475236, /api/v1/people/8475268, /api/v1/people/8475868, /api/v1/people/8476316, /api/v1/people/8476331, /api/v1/people/8476392, /api/v1/people/8476400, /api/v1/people/8476406, /api/v1/people/8476460, /api/v1/people/8476470, /api/v1/people/8476945, /api/v1/people/8477359, /api/v1/people/8477429, /api/v1/people/8477472, /api/v1/people/8477504, /api/v1/people/8477940, /api/v1/people/8478398, /api/v1/people/8478424, /api/v1/people/8478458, /api/v1/people/8478891, /api/v1/people/8478915, /api/v1/people/8479339, /api/v1/people/8479574, /api/v1/people/8480145, C, R, L, C, D, D, C, L, D, G, D, C, R, C, C, D, G, D, C, D, D, L, L, C, C, C, D, R, G, D, Center, Right Wing, Left Wing, Center, Defenseman, Defenseman, Center, Left Wing, Defenseman, Goalie, Defenseman, Center, Right Wing, Center, Center, Defenseman, Goalie, Defenseman, Center, Defenseman, Defenseman, Left Wing, Left Wing, Center, Center, Center, Defenseman, Right Wing, Goalie, Defenseman, Forward, Forward, Forward, Forward, Defenseman, Defenseman, Forward, Forward, Defenseman, Goalie, Defenseman, Forward, Forward, Forward, Forward, Defenseman, Goalie, Defenseman, Forward, Defenseman, Defenseman, Forward, Forward, Forward, Forward, Forward, Defenseman, Forward, Goalie, Defenseman, C, RW, LW, C, D, D, C, LW, D, G, D, C, RW, C, C, D, G, D, C, D, D, LW, LW, C, C, C, D, RW, G, D
    ## 30                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           15, 34, 33, 4, 40, 81, 55, 21, 23, 35, 91, 13, 32, 82, 8, 18, 36, 67, 83, 9, 6, 29, 46, 8470755, 8471262, 8471274, 8471769, 8473546, 8473548, 8474218, 8474613, 8475171, 8475311, 8475791, 8476994, 8477293, 8477851, 8477951, 8477989, 8478432, 8478474, 8478856, 8479343, 8479345, 8480849, 8480950, Brad Richardson, Carl Soderberg, Alex Goligoski, Niklas Hjalmarsson, Michael Grabner, Phil Kessel, Jason Demers, Derek Stepan, Oliver Ekman-Larsson, Darcy Kuemper, Taylor Hall, Vinnie Hinostroza, Antti Raanta, Jordan Oesterle, Nick Schmaltz, Christian Dvorak, Christian Fischer, Lawson Crouse, Conor Garland, Clayton Keller, Jakob Chychrun, Barrett Hayton, Ilya Lyubushkin, /api/v1/people/8470755, /api/v1/people/8471262, /api/v1/people/8471274, /api/v1/people/8471769, /api/v1/people/8473546, /api/v1/people/8473548, /api/v1/people/8474218, /api/v1/people/8474613, /api/v1/people/8475171, /api/v1/people/8475311, /api/v1/people/8475791, /api/v1/people/8476994, /api/v1/people/8477293, /api/v1/people/8477851, /api/v1/people/8477951, /api/v1/people/8477989, /api/v1/people/8478432, /api/v1/people/8478474, /api/v1/people/8478856, /api/v1/people/8479343, /api/v1/people/8479345, /api/v1/people/8480849, /api/v1/people/8480950, C, C, D, D, L, R, D, C, D, G, L, R, G, D, C, C, R, L, R, R, D, C, D, Center, Center, Defenseman, Defenseman, Left Wing, Right Wing, Defenseman, Center, Defenseman, Goalie, Left Wing, Right Wing, Goalie, Defenseman, Center, Center, Right Wing, Left Wing, Right Wing, Right Wing, Defenseman, Center, Defenseman, Forward, Forward, Defenseman, Defenseman, Forward, Forward, Defenseman, Forward, Defenseman, Goalie, Forward, Forward, Goalie, Defenseman, Forward, Forward, Forward, Forward, Forward, Forward, Defenseman, Forward, Defenseman, C, C, D, D, LW, RW, D, C, D, G, LW, RW, G, D, C, C, RW, LW, RW, RW, D, C, D
    ## 31                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        73, 5, 29, 26, 75, 67, 23, 22, 3, 19, 90, 15, 61, 21, 71, 81, 35, 20, 88, 27, 28, 92, 89, 37, 10, 52, 2, 18, 8475204, 8468674, 8470594, 8471669, 8471817, 8474157, 8474166, 8474207, 8475188, 8475191, 8475215, 8475750, 8475913, 8476393, 8476448, 8476539, 8476861, 8476905, 8477220, 8477447, 8477478, 8477931, 8477949, 8478097, 8478462, 8479639, 8480727, 8481522, Brandon Pirri, Deryk Engelland, Marc-Andre Fleury, Paul Stastny, Ryan Reaves, Max Pacioretty, Alec Martinez, Nick Holden, Brayden McNabb, Reilly Smith, Robin Lehner, Jon Merrill, Mark Stone, Nick Cousins, William Karlsson, Jonathan Marchessault, Oscar Dansk, Chandler Stephenson, Nate Schmidt, Shea Theodore, William Carrier, Tomas Nosek, Alex Tuch, Reid Duke, Nicolas Roy, Dylan Coghlan, Zach Whitecloud, Peyton Krebs, /api/v1/people/8475204, /api/v1/people/8468674, /api/v1/people/8470594, /api/v1/people/8471669, /api/v1/people/8471817, /api/v1/people/8474157, /api/v1/people/8474166, /api/v1/people/8474207, /api/v1/people/8475188, /api/v1/people/8475191, /api/v1/people/8475215, /api/v1/people/8475750, /api/v1/people/8475913, /api/v1/people/8476393, /api/v1/people/8476448, /api/v1/people/8476539, /api/v1/people/8476861, /api/v1/people/8476905, /api/v1/people/8477220, /api/v1/people/8477447, /api/v1/people/8477478, /api/v1/people/8477931, /api/v1/people/8477949, /api/v1/people/8478097, /api/v1/people/8478462, /api/v1/people/8479639, /api/v1/people/8480727, /api/v1/people/8481522, C, D, G, C, R, L, D, D, D, R, G, D, R, C, C, C, G, C, D, D, L, L, R, C, C, D, D, C, Center, Defenseman, Goalie, Center, Right Wing, Left Wing, Defenseman, Defenseman, Defenseman, Right Wing, Goalie, Defenseman, Right Wing, Center, Center, Center, Goalie, Center, Defenseman, Defenseman, Left Wing, Left Wing, Right Wing, Center, Center, Defenseman, Defenseman, Center, Forward, Defenseman, Goalie, Forward, Forward, Forward, Defenseman, Defenseman, Defenseman, Forward, Goalie, Defenseman, Forward, Forward, Forward, Forward, Goalie, Forward, Defenseman, Defenseman, Forward, Forward, Forward, Forward, Forward, Defenseman, Defenseman, Forward, C, D, G, C, RW, LW, D, D, D, RW, G, D, RW, C, C, C, G, C, D, D, LW, LW, RW, C, C, D, D, C
    ##                roster.link
    ## 1   /api/v1/teams/1/roster
    ## 2   /api/v1/teams/2/roster
    ## 3   /api/v1/teams/3/roster
    ## 4   /api/v1/teams/4/roster
    ## 5   /api/v1/teams/5/roster
    ## 6   /api/v1/teams/6/roster
    ## 7   /api/v1/teams/7/roster
    ## 8   /api/v1/teams/8/roster
    ## 9   /api/v1/teams/9/roster
    ## 10 /api/v1/teams/10/roster
    ## 11 /api/v1/teams/12/roster
    ## 12 /api/v1/teams/13/roster
    ## 13 /api/v1/teams/14/roster
    ## 14 /api/v1/teams/15/roster
    ## 15 /api/v1/teams/16/roster
    ## 16 /api/v1/teams/17/roster
    ## 17 /api/v1/teams/18/roster
    ## 18 /api/v1/teams/19/roster
    ## 19 /api/v1/teams/20/roster
    ## 20 /api/v1/teams/21/roster
    ## 21 /api/v1/teams/22/roster
    ## 22 /api/v1/teams/23/roster
    ## 23 /api/v1/teams/24/roster
    ## 24 /api/v1/teams/25/roster
    ## 25 /api/v1/teams/26/roster
    ## 26 /api/v1/teams/28/roster
    ## 27 /api/v1/teams/29/roster
    ## 28 /api/v1/teams/30/roster
    ## 29 /api/v1/teams/52/roster
    ## 30 /api/v1/teams/53/roster
    ## 31 /api/v1/teams/54/roster

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

<!-- end list -->

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
nhlFun(endpoints = "team total")
```

    ## No encoding supplied: defaulting to UTF-8.

    ## $data
    ##    id activeFranchise firstSeasonId franchiseId gameTypeId
    ## 1   1               1      19821983          23          2
    ## 2   2               1      19821983          23          3
    ## 3   3               1      19721973          22          2
    ## 4   4               1      19721973          22          3
    ## 5   5               1      19261927          10          2
    ## 6   6               1      19261927          10          3
    ## 7   7               1      19671968          16          3
    ## 8   8               1      19671968          16          2
    ## 9   9               1      19671968          17          2
    ## 10 10               1      19671968          17          3
    ## 11 11               1      19241925           6          2
    ## 12 12               1      19241925           6          3
    ## 13 13               1      19701971          19          2
    ## 14 14               1      19701971          19          3
    ## 15 15               1      19171918           1          3
    ## 16 16               1      19171918           1          2
    ## 17 17               1      19921993          30          2
    ## 18 18               1      19921993          30          3
    ## 19 19               1      19271928           5          2
    ## 20 20               1      19271928           5          3
    ## 21 21               1      19992000          35          2
    ## 22 22               1      19992000          35          3
    ## 23 23               1      19971998          26          3
    ## 24 24               1      19971998          26          2
    ## 25 25               1      19931994          33          2
    ## 26 26               1      19931994          33          3
    ## 27 27               1      19921993          31          2
    ## 28 28               1      19921993          31          3
    ## 29 29               1      19741975          24          2
    ## 30 30               1      19741975          24          3
    ## 31 31               1      19261927          11          3
    ## 32 32               1      19261927          11          2
    ## 33 33               1      19321933          12          2
    ##    gamesPlayed goalsAgainst goalsFor homeLosses
    ## 1         2937         8708     8647        507
    ## 2          257          634      697         53
    ## 3         3732        11779    11889        674
    ## 4          289          845      925         48
    ## 5         6504        19863    19864       1132
    ## 6          518         1447     1404        104
    ## 7          449         1332     1335         97
    ## 8         4115        12054    13527        572
    ## 9         4115        13893    13678        679
    ## 10         385         1110     1174         83
    ## 11        6570        19001    20944        953
    ## 12         664         1875     1923        149
    ## 13        3889        11767    12333        623
    ## 14         256          765      763         54
    ## 15         759         1927     2271        131
    ## 16        6731        18092    21632        870
    ## 17        2139         6390     6093        403
    ## 18         151          372      357         35
    ## 19        6460        19805    19793       1075
    ## 20         538         1477     1380        117
    ## 21         902         3014     2465        204
    ## 22           4           17        6          2
    ## 23         101          252      241         21
    ## 24        1756         5004     4735        320
    ## 25        2053         5969     5476        385
    ## 26          48          128      115         13
    ## 27        2138         6499     6035        407
    ## 28         151          402      410         37
    ## 29        3577        11390    11325        612
    ## 30         290          821      826         75
    ## 31         548         1669     1566        104
    ## 32        6504        19501    19376       1117
    ## 33        6237        18710    19423        929
    ##    homeOvertimeLosses homeTies homeWins lastSeasonId losses
    ## 1                  82       96      783           NA   1181
    ## 2                   0       NA       74           NA    120
    ## 3                  81      170      942           NA   1570
    ## 4                   1       NA       89           NA    130
    ## 5                  73      448     1600           NA   2693
    ## 6                   0        1      137           NA    266
    ## 7                   0       NA      135           NA    218
    ## 8                  89      193     1204           NA   1429
    ## 9                  58      205     1116           NA   1718
    ## 10                  0       NA      112           NA    178
    ## 11                 89      376     1867           NA   2387
    ## 12                  2        3      191           NA    332
    ## 13                 80      197     1045           NA   1530
    ## 14                  0       NA       73           NA    132
    ## 15                  0        3      254           NA    317
    ## 16                 91      381     2025           NA   2281
    ## 17                 89       60      519           NA    912
    ## 18                  0       NA       37           NA     79
    ## 19                 82      388     1684           NA   2682
    ## 20                  0        2      148           NA    279
    ## 21                 38       26      183     20102011    437
    ## 22                  0       NA        0     20102011      4
    ## 23                  0       NA       29           NA     48
    ## 24                 72       52      433           NA    713
    ## 25                112       65      465           NA    856
    ## 26                  0       NA       12           NA     29
    ## 27                 67       56      538           NA    930
    ## 28                  0       NA       42           NA     67
    ## 29                 80      153      942           NA   1452
    ## 30                  1       NA       74           NA    152
    ## 31                  0        1      166           NA    275
    ## 32                 82      410     1642           NA   2736
    ## 33                 94      368     1729           NA   2419
    ##    overtimeLosses penaltyMinutes pointPctg points
    ## 1             162          44397    0.5330   3131
    ## 2               0           4266    0.0039      2
    ## 3             159          57422    0.5115   3818
    ## 4               0           5498    0.0138      8
    ## 5             147          85564    0.5125   6667
    ## 6               0           8181    0.0000      0
    ## 7               0           9104    0.0045      4
    ## 8             175          75761    0.5759   4740
    ## 9             148          65826    0.5180   4263
    ## 10              0           6056    0.0156     12
    ## 11            184          88037    0.5625   7391
    ## 12              0          10505    0.0301     40
    ## 13            160          60329    0.5334   4149
    ## 14              0           4682    0.0000      0
    ## 15              0          12047    0.0000      0
    ## 16            164          87019    0.5868   7899
    ## 17            164          29175    0.5084   2175
    ## 18              0           2102    0.0000      0
    ## 19            167          91941    0.5121   6616
    ## 20              0           8491    0.0112     12
    ## 21             78          13727    0.4473    807
    ## 22              0            115    0.0000      0
    ## 23              0           1198    0.0792     16
    ## 24            166          19015    0.5222   1834
    ## 25            203          28603    0.4990   2049
    ## 26              0            669    0.0000      0
    ## 27            147          30489    0.5044   2157
    ## 28              0           2110    0.0728     22
    ## 29            158          56928    0.5296   3789
    ## 30              1           5100    0.0655     38
    ## 31              0           8855    0.0000      0
    ## 32            166          91917    0.5040   6556
    ## 33            173          83995    0.5363   6690
    ##    roadLosses roadOvertimeLosses roadTies roadWins
    ## 1         674                 80      123      592
    ## 2          67                  0       NA       63
    ## 3         896                 78      177      714
    ## 4          82                  0       NA       70
    ## 5        1561                 74      360     1256
    ## 6         162                  0        7      107
    ## 7         121                  0       NA       96
    ## 8         857                 86      264      850
    ## 9        1039                 90      178      750
    ## 10         95                  1       NA       95
    ## 11       1434                 95      415     1341
    ## 12        183                  0        3      135
    ## 13        907                 80      212      745
    ## 14         78                  0       NA       51
    ## 15        186                  0        5      180
    ## 16       1411                 73      456     1424
    ## 17        509                 75       55      429
    ## 18         44                  0       NA       35
    ## 19       1607                 85      385     1154
    ## 20        162                  0        1      108
    ## 21        233                 40       19      159
    ## 22          2                  0       NA        0
    ## 23         27                  1       NA       24
    ## 24        393                 94       34      358
    ## 25        471                 91       77      387
    ## 26         16                  0       NA        7
    ## 27        523                 80       56      411
    ## 28         30                  0       NA       42
    ## 29        840                 78      150      722
    ## 30         77                  1       NA       63
    ## 31        171                  0        4      102
    ## 32       1619                 84      404     1146
    ## 33       1490                 79      405     1143
    ##    shootoutLosses shootoutWins shutouts teamId
    ## 1              79           78      193      1
    ## 2               0            0       25      1
    ## 3              67           82      167      2
    ## 4               0            0       12      2
    ## 5              66           78      403      3
    ## 6               0            0       44      3
    ## 7               0            0       33      4
    ## 8              88           50      245      4
    ## 9              53           80      184      5
    ## 10              0            0       30      5
    ## 11             80           64      500      6
    ## 12              0            0       49      6
    ## 13             71           77      194      7
    ## 14              0            0       18      7
    ## 15              0            0       67      8
    ## 16             63           68      542      8
    ## 17             78           56      135      9
    ## 18              0            0       12      9
    ## 19             77           58      419     10
    ## 20              0            0       49     10
    ## 21             29           37       41     11
    ## 22              0            0        0     11
    ## 23              0            0       10     12
    ## 24             59           46       93     12
    ## 25             95           70      112     13
    ## 26              0            0        3     13
    ## 27             57           67      118     14
    ## 28              0            1       11     14
    ## 29             69           65      174     15
    ## 30              1            0       19     15
    ## 31              0            0       32     16
    ## 32             68           73      435     16
    ## 33             73           69      421     17
    ##               teamName ties triCode wins
    ## 1    New Jersey Devils  219     NJD 1375
    ## 2    New Jersey Devils   NA     NJD  137
    ## 3   New York Islanders  347     NYI 1656
    ## 4   New York Islanders   NA     NYI  159
    ## 5     New York Rangers  808     NYR 2856
    ## 6     New York Rangers    8     NYR  244
    ## 7  Philadelphia Flyers   NA     PHI  231
    ## 8  Philadelphia Flyers  457     PHI 2054
    ## 9  Pittsburgh Penguins  383     PIT 1866
    ## 10 Pittsburgh Penguins   NA     PIT  207
    ## 11       Boston Bruins  791     BOS 3208
    ## 12       Boston Bruins    6     BOS  326
    ## 13      Buffalo Sabres  409     BUF 1790
    ## 14      Buffalo Sabres   NA     BUF  124
    ## 15  Montréal Canadiens    8     MTL  434
    ## 16  Montréal Canadiens  837     MTL 3449
    ## 17     Ottawa Senators  115     OTT  948
    ## 18     Ottawa Senators   NA     OTT   72
    ## 19 Toronto Maple Leafs  773     TOR 2838
    ## 20 Toronto Maple Leafs    3     TOR  256
    ## 21   Atlanta Thrashers   45     ATL  342
    ## 22   Atlanta Thrashers   NA     ATL    0
    ## 23 Carolina Hurricanes   NA     CAR   53
    ## 24 Carolina Hurricanes   86     CAR  791
    ## 25    Florida Panthers  142     FLA  852
    ## 26    Florida Panthers   NA     FLA   19
    ## 27 Tampa Bay Lightning  112     TBL  949
    ## 28 Tampa Bay Lightning   NA     TBL   84
    ## 29 Washington Capitals  303     WSH 1664
    ## 30 Washington Capitals   NA     WSH  137
    ## 31  Chicago Blackhawks    5     CHI  268
    ## 32  Chicago Blackhawks  814     CHI 2788
    ## 33   Detroit Red Wings  773     DET 2872
    ##  [ reached 'max' / getOption("max.print") -- omitted 72 rows ]
    ## 
    ## $total
    ## [1] 105

``` r
nhlFun(endpoints = "skater record", "20")
```

    ## No encoding supplied: defaulting to UTF-8.

    ## $data
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
    ## 
    ## $total
    ## [1] 561

``` r
nhlFun(endpoints = "stats", expand = "person.team")
```

    ## $copyright
    ## [1] "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2020. All Rights Reserved."
    ## 
    ## $teams
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

## Exploratory Data Analysis

``` r
# perhaps use the wrapper function here
franTot <- getFranTeamTot()
```

    ## No encoding supplied: defaulting to UTF-8.

``` r
franTot <- franTot$data %>% select(-c("id", "activeFranchise", "firstSeasonId", "gameTypeId", "lastSeasonId"))
franStats <- getStats(expand = "person.names") 
franStats <- franStats$teams %>% select(c("locationName", "firstYearOfPlay", "franchiseId", "venue.city", "venue.timeZone.id", "venue.timeZone.tz", "division.name", "conference.name"))
combined <- full_join(franTot, franStats, by = "franchiseId") 
head(combined)
```

    ##   franchiseId gamesPlayed goalsAgainst goalsFor homeLosses
    ## 1          23        2937         8708     8647        507
    ## 2          23         257          634      697         53
    ## 3          22        3732        11779    11889        674
    ## 4          22         289          845      925         48
    ## 5          10        6504        19863    19864       1132
    ## 6          10         518         1447     1404        104
    ##   homeOvertimeLosses homeTies homeWins losses
    ## 1                 82       96      783   1181
    ## 2                  0       NA       74    120
    ## 3                 81      170      942   1570
    ## 4                  1       NA       89    130
    ## 5                 73      448     1600   2693
    ## 6                  0        1      137    266
    ##   overtimeLosses penaltyMinutes pointPctg points roadLosses
    ## 1            162          44397    0.5330   3131        674
    ## 2              0           4266    0.0039      2         67
    ## 3            159          57422    0.5115   3818        896
    ## 4              0           5498    0.0138      8         82
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
    ## 4     NYI  159     New York            1972   Brooklyn
    ## 5     NYR 2856     New York            1926   New York
    ## 6     NYR  244     New York            1926   New York
    ##   venue.timeZone.id venue.timeZone.tz division.name
    ## 1  America/New_York               EDT  Metropolitan
    ## 2  America/New_York               EDT  Metropolitan
    ## 3  America/New_York               EDT  Metropolitan
    ## 4  America/New_York               EDT  Metropolitan
    ## 5  America/New_York               EDT  Metropolitan
    ## 6  America/New_York               EDT  Metropolitan
    ##   conference.name
    ## 1         Eastern
    ## 2         Eastern
    ## 3         Eastern
    ## 4         Eastern
    ## 5         Eastern
    ## 6         Eastern

``` r
ggplot(combined, aes(x = homeWins, y = homeLosses)) + geom_point(aes(color = division.name))
```

![](README_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->
