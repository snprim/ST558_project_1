Vignette
================

  - [Required Packages](#required-packages)
  - [Functions](#functions)
      - [NHL records API](#nhl-records-api)
      - [NHL stats API](#nhl-stats-api)
  - [Exploratory Data Analysis](#exploratory-data-analysis)

``` r
# rmarkdown::render("vignette.Rmd", output_format = "github_document", output_file = "README.md")
```

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

/franchise (Returns id, ﬁrstSeasonId and lastSeasonId and name of every
team in the history of the NHL)

``` r
getfran <- function(){
  fullurl <- paste0(baseurl_records, "/", "franchise")
  fran <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(fran)
}
getfran()
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

/franchise-team-totals (Returns Total stats for every franchise (ex
roadTies, roadWins, etc))

``` r
getFranTeamTot <- function() {
  fullurl <- paste0(baseurl_records, "/", "franchise-team-totals")
  franTeamTot <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(franTeamTot)
}
getFranTeamTot()
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
    ## 4          288          837      923         48
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
    ## 28         150          400      402         37
    ## 29        3577        11390    11325        612
    ## 30         290          821      826         75
    ## 31         548         1669     1566        104
    ## 32        6504        19501    19376       1117
    ## 33        6237        18710    19423        929
    ##    homeOvertimeLosses homeTies homeWins lastSeasonId losses
    ## 1                  82       96      783           NA   1181
    ## 2                   0       NA       74           NA    120
    ## 3                  81      170      942           NA   1570
    ## 4                   1       NA       89           NA    129
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
    ## 28                  0       NA       41           NA     67
    ## 29                 80      153      942           NA   1452
    ## 30                  1       NA       74           NA    152
    ## 31                  0        1      166           NA    275
    ## 32                 82      410     1642           NA   2736
    ## 33                 94      368     1729           NA   2419
    ##    overtimeLosses penaltyMinutes pointPctg points
    ## 1             162          44397    0.5330   3131
    ## 2               0           4266    0.0039      2
    ## 3             159          57422    0.5115   3818
    ## 4               0           5474    0.0139      8
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
    ## 28              0           2098    0.0733     22
    ## 29            158          56928    0.5296   3789
    ## 30              1           5100    0.0655     38
    ## 31              0           8855    0.0000      0
    ## 32            166          91917    0.5040   6556
    ## 33            173          83995    0.5363   6690
    ##    roadLosses roadOvertimeLosses roadTies roadWins
    ## 1         674                 80      123      592
    ## 2          67                  0       NA       63
    ## 3         896                 78      177      714
    ## 4          81                  0       NA       70
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
    ## 28 Tampa Bay Lightning   NA     TBL   83
    ## 29 Washington Capitals  303     WSH 1664
    ## 30 Washington Capitals   NA     WSH  137
    ## 31  Chicago Blackhawks    5     CHI  268
    ## 32  Chicago Blackhawks  814     CHI 2788
    ## 33   Detroit Red Wings  773     DET 2872
    ##  [ reached 'max' / getOption("max.print") -- omitted 72 rows ]
    ## 
    ## $total
    ## [1] 105

/site/api/franchise-season-records?cayenneExp=franchiseId=ID (Drill-down
into season records for a speciﬁc franchise)

``` r
getFranSeaRec <- function(franID) {
  fullurl <- paste0(baseurl_records, "/", "franchise-season-records?cayenneExp=franchiseId=", franID)
  franSeaRec <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(franSeaRec)
}
getFranSeaRec("20")
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

/franchise-goalie-records?cayenneExp=franchiseId=ID (Goalie records for
the speciﬁed franchise)

``` r
getFranGoaRec <- function(franID) {
  fullurl <- paste0(baseurl_records, "/", "franchise-goalie-records?cayenneExp=franchiseId=", franID)
  franGoaRec <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(franGoaRec)
}
getFranGoaRec("20")
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

/franchise-skater-records?cayenneExp=franchiseId=ID (Skater records,
same interaction as goalie endpoint)

``` r
getFranSkaRec <- function(franID) {
  fullurl <- paste0(baseurl_records, "/", "franchise-skater-records?cayenneExp=franchiseId=", franID)
  franSkaRec <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(franSkaRec)
}
getFranSkaRec("20")
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
getStats(expand = "person.names")
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
getStats(stats = "statsSingleSeasonPlayoffs")
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
franTot <- getFranTeamTot()
```

    ## No encoding supplied: defaulting to UTF-8.

``` r
franTot <- franTot$data %>% select(-c("id", "activeFranchise", "firstSeasonId", "gameTypeId", "lastSeasonId"))
franStats <- getStats(expand = "person.names") 
franStats <- franStats$teams %>% select(c("locationName", "firstYearOfPlay", "franchiseId", "venue.city", "venue.timeZone.id", "venue.timeZone.tz", "division.name", "conference.name"))
combined <- full_join(franTot, franStats, by = "franchiseId") 
ggplot(combined, aes(x = homeWins, y = homeLosses)) + geom_point(aes(color = division.name))
```

![](README_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->
