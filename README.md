Vignette
================

  - [Required Packages](#required-packages)
  - [Functions](#functions)
      - [NHL records API](#nhl-records-api)
      - [NHL stats API](#nhl-stats-api)
  - [Exploratory Data Analysis](#exploratory-data-analysis)

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
getFranSeaRec("5")
```

    ## No encoding supplied: defaulting to UTF-8.

    ## $data
    ##   id fewestGoals fewestGoalsAgainst
    ## 1 10         147                131
    ##   fewestGoalsAgainstSeasons fewestGoalsSeasons fewestLosses
    ## 1              1953-54 (70)       1954-55 (70)           16
    ##   fewestLossesSeasons fewestPoints fewestPointsSeasons
    ## 1        1950-51 (70)           48        1984-85 (80)
    ##   fewestTies fewestTiesSeasons fewestWins
    ## 1          4      1989-90 (80)         20
    ##            fewestWinsSeasons franchiseId
    ## 1 1981-82 (80), 1984-85 (80)           5
    ##         franchiseName homeLossStreak
    ## 1 Toronto Maple Leafs              7
    ##         homeLossStreakDates homePointStreak
    ## 1 Nov 11 1984 - Dec 05 1984              18
    ##                                   homePointStreakDates
    ## 1 Nov 28 1933 - Mar 10 1934, Oct 31 1953 - Jan 23 1954
    ##   homeWinStreak        homeWinStreakDates homeWinlessStreak
    ## 1            13 Jan 31 2018 - Mar 24 2018                11
    ##                                 homeWinlessStreakDates
    ## 1 Dec 19 1987 - Jan 25 1988, Feb 11 2012 - Mar 29 2012
    ##   lossStreak           lossStreakDates mostGameGoals
    ## 1         10 Jan 15 1967 - Feb 08 1967            14
    ##             mostGameGoalsDates mostGoals mostGoalsAgainst
    ## 1 Mar 16 1957 - NYR 1 @ TOR 14       337              387
    ##   mostGoalsAgainstSeasons mostGoalsSeasons mostLosses
    ## 1            1983-84 (80)     1989-90 (80)         52
    ##   mostLossesSeasons mostPenaltyMinutes
    ## 1      1984-85 (80)               2419
    ##   mostPenaltyMinutesSeasons mostPoints mostPointsSeasons
    ## 1              1989-90 (80)        105      2017-18 (82)
    ##   mostShutouts mostShutoutsSeasons mostTies mostTiesSeasons
    ## 1           13        1953-54 (70)       22    1954-55 (70)
    ##   mostWins mostWinsSeasons pointStreak
    ## 1       49    2017-18 (82)          16
    ##            pointStreakDates roadLossStreak
    ## 1 Nov 22 2003 - Dec 26 2003             11
    ##         roadLossStreakDates roadPointStreak
    ## 1 Feb 20 1988 - Apr 01 1988              11
    ##        roadPointStreakDates roadWinStreak
    ## 1 Dec 03 2016 - Jan 25 2017             7
    ##                                                                roadWinStreakDates
    ## 1 Nov 14 1940 - Dec 15 1940, Dec 04 1960 - Jan 05 1961, Jan 29 2003 - Feb 22 2003
    ##   roadWinlessStreak    roadWinlessStreakDates winStreak
    ## 1                18 Oct 06 1982 - Jan 05 1983        10
    ##              winStreakDates winlessStreak
    ## 1 Oct 07 1993 - Oct 28 1993             6
    ##          winlessStreakDates
    ## 1 Nov 09 2019 - Nov 19 2019
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
getFranGoaRec("5")
```

    ## No encoding supplied: defaulting to UTF-8.

    ## $data
    ##     id activePlayer firstName franchiseId
    ## 1  246        FALSE      Turk           5
    ## 2  324         TRUE     James           5
    ## 3  334        FALSE       Tom           5
    ## 4  338        FALSE       Don           5
    ## 5  342        FALSE        Ed           5
    ## 6  395        FALSE       Don           5
    ## 7  408        FALSE      Doug           5
    ## 8  418        FALSE     Grant           5
    ## 9  445        FALSE      Paul           5
    ## 10 453        FALSE     Glenn           5
    ## 11 474        FALSE    Curtis           5
    ## 12 481        FALSE      Mark           5
    ## 13 484        FALSE    Michel           5
    ## 14 541        FALSE      Paul           5
    ## 15 545        FALSE    Johnny           5
    ## 16 550        FALSE     Lorne           5
    ## 17 554        FALSE        Ed           5
    ## 18 583        FALSE     Bruce           5
    ## 19 596        FALSE    George           5
    ## 20 606        FALSE       Joe           5
    ## 21 609        FALSE     Eddie           5
    ## 22 616        FALSE      Bert           5
    ## 23 618        FALSE     Harry           5
    ## 24 627        FALSE   Jacques           5
    ## 25 638        FALSE     Terry           5
    ## 26 642        FALSE       Don           5
    ## 27 651        FALSE      Dunc           5
    ## 28 657        FALSE    Bernie           5
    ## 29 666        FALSE     Daren           5
    ## 30 680        FALSE      Curt           5
    ## 31 690        FALSE       Jim           5
    ## 32 712        FALSE      Rick           5
    ## 33 735        FALSE     Wayne           5
    ## 34 752        FALSE      Rick           5
    ##          franchiseName gameTypeId gamesPlayed   lastName
    ## 1  Toronto Maple Leafs          2         629      Broda
    ## 2  Toronto Maple Leafs          2         207     Reimer
    ## 3  Toronto Maple Leafs          2           4   Barrasso
    ## 4  Toronto Maple Leafs          2          11    Beaupre
    ## 5  Toronto Maple Leafs          2         170    Belfour
    ## 6  Toronto Maple Leafs          2          38    Edwards
    ## 7  Toronto Maple Leafs          2          74     Favell
    ## 8  Toronto Maple Leafs          2          95       Fuhr
    ## 9  Toronto Maple Leafs          2          55   Harrison
    ## 10 Toronto Maple Leafs          2          65      Healy
    ## 11 Toronto Maple Leafs          2         270     Joseph
    ## 12 Toronto Maple Leafs          2          27   Laforest
    ## 13 Toronto Maple Leafs          2          74   Larocque
    ## 14 Toronto Maple Leafs          2          29   Bibeault
    ## 15 Toronto Maple Leafs          2         475      Bower
    ## 16 Toronto Maple Leafs          2         214     Chabot
    ## 17 Toronto Maple Leafs          2         180   Chadwick
    ## 18 Toronto Maple Leafs          2         210     Gamble
    ## 19 Toronto Maple Leafs          2         147 Hainsworth
    ## 20 Toronto Maple Leafs          2           1  Ironstone
    ## 21 Toronto Maple Leafs          2          26   Johnston
    ## 22 Toronto Maple Leafs          2          16    Lindsay
    ## 23 Toronto Maple Leafs          2         267     Lumley
    ## 24 Toronto Maple Leafs          2         106     Plante
    ## 25 Toronto Maple Leafs          2          91    Sawchuk
    ## 26 Toronto Maple Leafs          2          58    Simmons
    ## 27 Toronto Maple Leafs          2          49     Wilson
    ## 28 Toronto Maple Leafs          2          65     Parent
    ## 29 Toronto Maple Leafs          2           8      Puppa
    ## 30 Toronto Maple Leafs          2           6     Ridley
    ## 31 Toronto Maple Leafs          2          18 Rutherford
    ## 32 Toronto Maple Leafs          2          49  St. Croix
    ## 33 Toronto Maple Leafs          2          97     Thomas
    ## 34 Toronto Maple Leafs          2          11    Wamsley
    ##    losses
    ## 1     222
    ## 2      76
    ## 3       2
    ## 4       8
    ## 5      61
    ## 6      23
    ## 7      26
    ## 8      42
    ## 9      29
    ## 10     30
    ## 11     97
    ## 12     14
    ## 13     35
    ## 14     14
    ## 15    157
    ## 16     78
    ## 17     89
    ## 18     84
    ## 19     48
    ## 20      0
    ## 21      9
    ## 22     11
    ## 23    106
    ## 24     38
    ## 25     30
    ## 26     21
    ## 27     22
    ## 28     25
    ## 29      2
    ## 30      2
    ## 31     10
    ## 32     28
    ## 33     37
    ## 34      6
    ##                                         mostGoalsAgainstDates
    ## 1                                                  1938-01-22
    ## 2                                                  2016-02-15
    ## 3                                                  2002-03-21
    ## 4                                                  1996-03-08
    ## 5                                                  2005-12-17
    ## 6                                                  1986-02-06
    ## 7                                                  1975-01-23
    ## 8                                                  1991-12-26
    ## 9                          1980-01-19, 1979-12-26, 1978-12-03
    ## 10                                                 1999-03-24
    ## 11 2008-11-25, 2000-12-16, 2000-11-29, 2000-01-01, 1999-02-13
    ## 12                                                 1989-11-04
    ## 13                                     1982-03-13, 1981-03-19
    ## 14                                                 1944-03-05
    ## 15                                                 1960-03-09
    ## 16                                                 1929-11-19
    ## 17                                                 1957-10-17
    ## 18                                                 1971-01-17
    ## 19             1936-01-11, 1935-03-09, 1935-01-29, 1934-03-06
    ## 20                                                 1928-03-03
    ## 21                                                 1974-02-26
    ## 22                                                 1919-01-11
    ## 23                                                 1956-02-16
    ## 24                                                 1971-11-07
    ## 25                                                 1965-11-07
    ## 26                                                 1964-01-18
    ## 27                                                 1974-12-30
    ## 28                                                 1971-04-03
    ## 29                                                 1993-02-14
    ## 30                                                 1980-11-28
    ## 31                                                 1981-02-18
    ## 32                                                 1985-04-03
    ## 33                                     1977-02-20, 1975-10-12
    ## 34                                                 1992-02-18
    ##    mostGoalsAgainstOneGame
    ## 1                        9
    ## 2                        7
    ## 3                        4
    ## 4                        7
    ## 5                        8
    ## 6                        8
    ## 7                        8
    ## 8                       12
    ## 9                        7
    ## 10                       7
    ## 11                       6
    ## 12                       7
    ## 13                      10
    ## 14                       8
    ## 15                       9
    ## 16                      10
    ## 17                       9
    ## 18                       9
    ## 19                       7
    ## 20                       0
    ## 21                       7
    ## 22                      13
    ## 23                       8
    ## 24                       8
    ## 25                       9
    ## 26                      11
    ## 27                       7
    ## 28                       8
    ## 29                       5
    ## 30                       6
    ## 31                       8
    ## 32                       9
    ## 33                       8
    ## 34                       7
    ##                                    mostSavesDates
    ## 1                                            <NA>
    ## 2  2015-04-04, 2013-11-23, 2013-04-20, 2012-02-04
    ## 3                                      2002-03-21
    ## 4                                      1996-01-30
    ## 5                                      2003-01-04
    ## 6                                      1986-03-06
    ## 7                                      1974-11-27
    ## 8                                      1991-10-05
    ## 9                                      1980-01-12
    ## 10                                     1998-02-07
    ## 11                                     1999-10-09
    ## 12                                     1989-11-22
    ## 13                                     1982-04-04
    ## 14                                           <NA>
    ## 15                                     1962-03-25
    ## 16                                           <NA>
    ## 17                         1959-12-20, 1958-12-25
    ## 18                                     1968-02-29
    ## 19                                           <NA>
    ## 20                                           <NA>
    ## 21                                     1974-03-16
    ## 22                                           <NA>
    ## 23                                     1955-10-09
    ## 24                                     1970-11-21
    ## 25                                     1965-03-07
    ## 26                         1963-01-17, 1962-03-22
    ## 27                                     1974-12-30
    ## 28                                     1972-01-15
    ## 29                                     1993-04-11
    ## 30                                     1980-11-28
    ## 31                         1981-03-04, 1981-02-03
    ## 32                                     1984-12-26
    ## 33                                     1976-12-18
    ## 34                                     1992-12-06
    ##    mostSavesOneGame
    ## 1                NA
    ## 2                49
    ## 3                27
    ## 4                33
    ## 5                50
    ## 6                46
    ## 7                48
    ## 8                45
    ## 9                44
    ## 10               38
    ## 11               46
    ## 12               48
    ## 13               52
    ## 14               NA
    ## 15               50
    ## 16               NA
    ## 17               44
    ## 18               50
    ## 19               NA
    ## 20               NA
    ## 21               38
    ## 22               NA
    ## 23               48
    ## 24               46
    ## 25               43
    ## 26               36
    ## 27               40
    ## 28               42
    ## 29               35
    ## 30               37
    ## 31               42
    ## 32               41
    ## 33               51
    ## 34               42
    ##                             mostShotsAgainstDates
    ## 1                                            <NA>
    ## 2  2015-04-04, 2013-12-07, 2013-11-23, 2013-04-20
    ## 3                                      2002-03-21
    ## 4                                      1996-01-30
    ## 5                                      2005-10-24
    ## 6                                      1986-03-06
    ## 7                                      1974-11-27
    ## 8                                      1991-10-05
    ## 9                                      1980-01-12
    ## 10                                     1998-02-07
    ## 11                                     1999-10-09
    ## 12                                     1989-11-22
    ## 13                                     1982-04-04
    ## 14                                           <NA>
    ## 15                                     1962-03-25
    ## 16                                           <NA>
    ## 17                                     1959-12-20
    ## 18                         1971-01-17, 1968-02-29
    ## 19                                           <NA>
    ## 20                                           <NA>
    ## 21                                     1974-03-16
    ## 22                                           <NA>
    ## 23                                     1955-10-09
    ## 24                         1970-11-21, 1970-10-17
    ## 25                                     1965-03-07
    ## 26                                     1963-01-17
    ## 27                                     1974-12-30
    ## 28                                     1972-01-15
    ## 29                                     1993-04-11
    ## 30                                     1980-11-28
    ## 31                         1981-03-04, 1981-02-03
    ## 32                                     1984-12-26
    ## 33                                     1976-12-18
    ## 34                                     1992-12-06
    ##    mostShotsAgainstOneGame mostShutoutsOneSeason
    ## 1                       NA                     9
    ## 2                       50                     4
    ## 3                       31                     0
    ## 4                       36                     0
    ## 5                       53                    10
    ## 6                       52                     0
    ## 7                       52                     1
    ## 8                       50                     2
    ## 9                       48                     1
    ## 10                      40                     2
    ## 11                      50                     6
    ## 12                      53                     0
    ## 13                      59                     0
    ## 14                      NA                     5
    ## 15                      55                     5
    ## 16                      NA                    10
    ## 17                      51                     5
    ## 18                      54                     5
    ## 19                      NA                     8
    ## 20                      NA                     1
    ## 21                      43                     1
    ## 22                      NA                     0
    ## 23                      51                    13
    ## 24                      49                     4
    ## 25                      46                     2
    ## 26                      42                     3
    ## 27                      47                     1
    ## 28                      45                     3
    ## 29                      37                     2
    ## 30                      43                     0
    ## 31                      47                     0
    ## 32                      47                     0
    ## 33                      53                     2
    ## 34                      48                     0
    ##           mostShutoutsSeasonIds mostWinsOneSeason
    ## 1                      19491950                32
    ## 2                      20122013                20
    ## 3                      20012002                 2
    ## 4            19951996, 19961997                 0
    ## 5                      20032004                37
    ## 6                      19851986                12
    ## 7                      19741975                14
    ## 8                      19911992                25
    ## 9                      19781979                 9
    ## 10                     19992000                 9
    ## 11                     20002001                36
    ## 12                     19891990                 9
    ## 13 19801981, 19811982, 19821983                10
    ## 14                     19431944                13
    ## 15           19591960, 19631964                34
    ## 16                     19281929                24
    ## 17                     19561957                21
    ## 18           19671968, 19691970                28
    ## 19           19341935, 19351936                30
    ## 20                     19271928                 0
    ## 21                     19731974                12
    ## 22                     19181919                 5
    ## 23                     19531954                32
    ## 24                     19701971                24
    ## 25                     19661967                16
    ## 26                     19631964                15
    ## 27                     19731974                 9
    ## 28                     19711972                17
    ## 29                     19921993                 6
    ## 30           19791980, 19801981                 1
    ## 31                     19801981                 4
    ## 32 19821983, 19831984, 19841985                 5
    ## 33                     19751976                28
    ## 34           19911992, 19921993                 4
    ##     mostWinsSeasonIds overtimeLosses playerId positionCode
    ## 1            19471948             NA  8449837            G
    ## 2            20102011             23  8473503            G
    ## 3            20012002             NA  8445275            G
    ## 4  19951996, 19961997             NA  8445381            G
    ## 5            20022003              4  8445386            G
    ## 6            19851986             NA  8446597            G
    ## 7            19731974             NA  8446794            G
    ## 8            19911992             NA  8446991            G
    ## 9            19791980             NA  8447663            G
    ## 10           19992000             NA  8447709            G
    ## 11           19992000              1  8448382            G
    ## 12           19891990             NA  8448628            G
    ## 13           19811982             NA  8448700            G
    ## 14           19431944             NA  8449823            G
    ## 15           19591960             NA  8449835            G
    ## 16           19321933             NA  8449850            G
    ## 17 19561957, 19571958             NA  8449851            G
    ## 18           19681969             NA  8449959            G
    ## 19           19341935             NA  8449987            G
    ## 20           19271928             NA  8450000            G
    ## 21           19731974             NA  8450005            G
    ## 22           19181919             NA  8450014            G
    ## 23           19531954             NA  8450019            G
    ## 24           19701971             NA  8450066            G
    ## 25 19641965, 19661967             NA  8450111            G
    ## 26           19621963             NA  8450113            G
    ## 27           19731974             NA  8450148            G
    ## 28           19711972             NA  8450178            G
    ## 29           19921993             NA  8450627            G
    ## 30           19801981             NA  8450830            G
    ## 31           19801981             NA  8451076            G
    ## 32           19831984             NA  8451626            G
    ## 33           19751976             NA  8451891            G
    ## 34           19911992             NA  8452287            G
    ##    rookieGamesPlayed rookieShutouts rookieWins seasons
    ## 1                 45              3         22      14
    ## 2                 37              3         20       6
    ## 3                 NA             NA         NA       1
    ## 4                 NA             NA         NA       2
    ## 5                 NA             NA         NA       3
    ## 6                 NA             NA         NA       1
    ## 7                 NA             NA         NA       3
    ## 8                 NA             NA         NA       2
    ## 9                 NA             NA         NA       2
    ## 10                NA             NA         NA       4
    ## 11                NA             NA         NA       5
    ## 12                NA             NA         NA       1
    ## 13                NA             NA         NA       3
    ## 14                NA             NA         NA       1
    ## 15                NA             NA         NA      12
    ## 16                NA             NA         NA       5
    ## 17                70              5         21       5
    ## 18                NA             NA         NA       6
    ## 19                NA             NA         NA       4
    ## 20                NA             NA         NA       1
    ## 21                NA             NA         NA       1
    ## 22                NA             NA         NA       1
    ## 23                NA             NA         NA       4
    ## 24                NA             NA         NA       3
    ## 25                NA             NA         NA       3
    ## 26                NA             NA         NA       3
    ## 27                NA             NA         NA       2
    ## 28                NA             NA         NA       2
    ## 29                NA             NA         NA       1
    ## 30                NA             NA         NA       2
    ## 31                NA             NA         NA       1
    ## 32                NA             NA         NA       3
    ## 33                NA             NA         NA       2
    ## 34                NA             NA         NA       2
    ##    shutouts ties wins
    ## 1        61  102  304
    ## 2        11   NA   85
    ## 3         0    0    2
    ## 4         0    0    0
    ## 5        17   11   93
    ## 6         0    0   12
    ## 7         1   16   26
    ## 8         3    9   38
    ## 9         1    5   17
    ## 10        2    5   23
    ## 11       17   27  138
    ## 12        0    0    9
    ## 13        0   13   16
    ## 14        5    2   13
    ## 15       32   79  219
    ## 16       31   31  102
    ## 17       14   34   57
    ## 18       19   31   83
    ## 19       19   20   79
    ## 20        1    1    0
    ## 21        1    4   12
    ## 22        0    0    5
    ## 23       34   58  103
    ## 24        7   15   48
    ## 25        4   13   42
    ## 26        5    7   29
    ## 27        1    7   17
    ## 28        3   12   24
    ## 29        2    0    6
    ## 30        0    0    1
    ## 31        0    2    4
    ## 32        0    2   11
    ## 33        3   18   38
    ## 34        0    0    4
    ##  [ reached 'max' / getOption("max.print") -- omitted 19 rows ]
    ## 
    ## $total
    ## [1] 53

/franchise-skater-records?cayenneExp=franchiseId=ID (Skater records,
same interaction as goalie endpoint)

``` r
getFranSkaRec <- function(franID) {
  fullurl <- paste0(baseurl_records, "/", "franchise-skater-records?cayenneExp=franchiseId=", franID)
  franSkaRec <- GET(fullurl) %>% content("text") %>% fromJSON(flatten = TRUE)
  return(franSkaRec)
}
getFranSkaRec("5")
```

    ## No encoding supplied: defaulting to UTF-8.

    ## $data
    ##       id activePlayer assists firstName franchiseId
    ## 1  16888        FALSE     417    George           5
    ## 2  16994        FALSE     567      Mats           5
    ## 3  17005        FALSE     620     Borje           5
    ## 4  17019        FALSE     112       Tie           5
    ## 5  17056        FALSE     238      Rick           5
    ## 6  17070        FALSE     321      Doug           5
    ## 7  17125        FALSE      99      Dave           5
    ## 8  17152        FALSE     125      Wilf           5
    ## 9  17184        FALSE     302       Ian           5
    ## 10 17195        FALSE       0    Gordon           5
    ## 11 17207        FALSE       1      Doug           5
    ## 12 17210        FALSE      43      Jack           5
    ## 13 17212        FALSE       2      Stew           5
    ## 14 17218        FALSE      18      Gary           5
    ## 15 17221        FALSE      29    Claire           5
    ## 16 17245        FALSE       2      Russ           5
    ## 17 17269        FALSE      19      Mike           5
    ## 18 17292        FALSE       5     Lloyd           5
    ## 19 17295        FALSE     204      John           5
    ## 20 17298        FALSE      94     Glenn           5
    ## 21 17311        FALSE     231       Syl           5
    ## 22 17312        FALSE       7        Al           5
    ## 23 17318        FALSE       3      Amos           5
    ## 24 17333        FALSE       0      Jack           5
    ## 25 17343        FALSE       1    Murray           5
    ## 26 17346        FALSE       1      Norm           5
    ## 27 17347        FALSE       0      John           5
    ## 28 17380        FALSE       5      Pete           5
    ## 29 17388        FALSE      81       Ace           5
    ## 30 17396        FALSE       9       Bob           5
    ## 31 17400        FALSE       1      Doug           5
    ## 32 17403        FALSE       6      Earl           5
    ## 33 17423        FALSE       0      Andy           5
    ##          franchiseName gameTypeId gamesPlayed goals
    ## 1  Toronto Maple Leafs          2        1188   296
    ## 2  Toronto Maple Leafs          2         981   420
    ## 3  Toronto Maple Leafs          2        1099   148
    ## 4  Toronto Maple Leafs          2         777    84
    ## 5  Toronto Maple Leafs          2         534   299
    ## 6  Toronto Maple Leafs          2         393   131
    ## 7  Toronto Maple Leafs          2         223   120
    ## 8  Toronto Maple Leafs          2         187    78
    ## 9  Toronto Maple Leafs          2         580   112
    ## 10 Toronto Maple Leafs          2           3     0
    ## 11 Toronto Maple Leafs          2           2     0
    ## 12 Toronto Maple Leafs          2         133    76
    ## 13 Toronto Maple Leafs          2           8     0
    ## 14 Toronto Maple Leafs          2          86    15
    ## 15 Toronto Maple Leafs          2         123    10
    ## 16 Toronto Maple Leafs          2           8     1
    ## 17 Toronto Maple Leafs          2          86     7
    ## 18 Toronto Maple Leafs          2          54     8
    ## 19 Toronto Maple Leafs          2         534   189
    ## 20 Toronto Maple Leafs          2         221    63
    ## 21 Toronto Maple Leafs          2         423   201
    ## 22 Toronto Maple Leafs          2          66     2
    ## 23 Toronto Maple Leafs          2          21     1
    ## 24 Toronto Maple Leafs          2          10     1
    ## 25 Toronto Maple Leafs          2          12     0
    ## 26 Toronto Maple Leafs          2           7     1
    ## 27 Toronto Maple Leafs          2           3     0
    ## 28 Toronto Maple Leafs          2          36     4
    ## 29 Toronto Maple Leafs          2         316   111
    ## 30 Toronto Maple Leafs          2          86     6
    ## 31 Toronto Maple Leafs          2          15     0
    ## 32 Toronto Maple Leafs          2          80    14
    ## 33 Toronto Maple Leafs          2           1     0
    ##      lastName
    ## 1   Armstrong
    ## 2      Sundin
    ## 3     Salming
    ## 4        Domi
    ## 5       Vaive
    ## 6     Gilmour
    ## 7  Andreychuk
    ## 8    Paiement
    ## 9    Turnbull
    ## 10     Spence
    ## 11      Acomb
    ## 12      Adams
    ## 13      Adams
    ## 14    Aldcorn
    ## 15  Alexander
    ## 16       Adam
    ## 17    Allison
    ## 18    Andrews
    ## 19   Anderson
    ## 20   Anderson
    ## 21       Apps
    ## 22     Arbour
    ## 23     Arbour
    ## 24     Arbour
    ## 25  Armstrong
    ## 26  Armstrong
    ## 27    Arundel
    ## 28     Backor
    ## 29     Bailey
    ## 30     Bailey
    ## 31    Baldwin
    ## 32    Balfour
    ## 33      Barbe
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      mostAssistsGameDates
    ## 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1956-01-07, 1957-03-16, 1957-11-24, 1961-01-15, 1961-12-02, 1962-02-25, 1964-02-23, 1965-12-18, 1969-01-31
    ## 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1995-10-17, 1995-11-10, 1996-03-09, 1996-11-21, 1998-10-24, 1998-12-02, 2000-01-17, 2000-03-29, 2002-04-13, 2006-01-30, 2006-10-30, 2006-12-16, 2007-04-07, 2007-10-06
    ## 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1977-12-16, 1978-10-14
    ## 4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1996-12-28, 1997-02-22, 1998-03-09, 1998-11-12, 2002-11-05, 2003-02-20
    ## 5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1984-03-12
    ## 6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1993-02-13
    ## 7                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1994-04-14
    ## 8                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1981-12-19
    ## 9                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      1977-02-12, 1977-02-20, 1979-11-10
    ## 10                                                                                                                                                                                                                                                                                                                                                                                                                         1925-11-28, 1925-12-01, 1925-12-05, 1925-12-09, 1925-12-12, 1925-12-19, 1925-12-23, 1925-12-26, 1925-12-29, 1925-12-30, 1926-01-01, 1926-01-05, 1926-01-09, 1926-01-12, 1926-01-15, 1926-01-21, 1926-01-23, 1926-01-26, 1926-01-29, 1926-02-02, 1926-02-03, 1926-02-06, 1926-02-09, 1926-02-11, 1926-02-13, 1926-02-16, 1926-02-18, 1926-02-20, 1926-02-22, 1926-02-27, 1926-03-04, 1926-03-06, 1926-03-11, 1926-03-13, 1926-03-16, 1926-03-17
    ## 11                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1970-03-07
    ## 12                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1923-02-03, 1923-02-10, 1923-02-14, 1924-01-02, 1924-12-10, 1924-12-22, 1925-02-07
    ## 13                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1933-01-03, 1933-01-05
    ## 14                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1957-12-25
    ## 15                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1975-01-29, 1977-01-02, 1977-01-31, 1977-02-02, 1977-02-05
    ## 16                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1982-10-13, 1982-10-30
    ## 17                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1986-11-05
    ## 18                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1922-12-16, 1922-12-23, 1923-02-14, 1923-02-17, 1923-12-19
    ## 19                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1979-11-10, 1981-02-28, 1981-10-10, 1983-12-21, 1984-03-24, 1984-10-24, 1985-01-19
    ## 20                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1992-01-25, 1993-01-08, 1993-01-11
    ## 21                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1937-01-30
    ## 22                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1961-10-28
    ## 23                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1923-12-26, 1924-01-05, 1924-01-19
    ## 24                                                                                                                                                                                                                                                                                                                         1928-11-15, 1928-11-17, 1928-11-20, 1928-11-22, 1928-11-24, 1928-11-27, 1928-12-01, 1928-12-04, 1928-12-08, 1928-12-11, 1928-12-15, 1928-12-20, 1928-12-22, 1928-12-25, 1928-12-27, 1928-12-29, 1929-01-01, 1929-01-03, 1929-01-05, 1929-01-08, 1929-01-10, 1929-01-12, 1929-01-17, 1929-01-20, 1929-01-22, 1929-01-24, 1929-01-26, 1929-01-29, 1929-01-31, 1929-02-02, 1929-02-03, 1929-02-05, 1929-02-09, 1929-02-14, 1929-02-16, 1929-02-17, 1929-02-23, 1929-02-28, 1929-03-02, 1929-03-07, 1929-03-09, 1929-03-12, 1929-03-14, 1929-03-16
    ## 25                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1938-11-24
    ## 26                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1962-12-16
    ## 27 1949-10-15, 1949-10-16, 1949-10-19, 1949-10-22, 1949-10-27, 1949-10-29, 1949-10-30, 1949-11-02, 1949-11-05, 1949-11-06, 1949-11-10, 1949-11-12, 1949-11-13, 1949-11-16, 1949-11-19, 1949-11-20, 1949-11-23, 1949-11-24, 1949-11-26, 1949-11-27, 1949-12-01, 1949-12-03, 1949-12-04, 1949-12-08, 1949-12-10, 1949-12-11, 1949-12-14, 1949-12-15, 1949-12-17, 1949-12-18, 1949-12-21, 1949-12-24, 1949-12-25, 1949-12-28, 1949-12-31, 1950-01-01, 1950-01-04, 1950-01-07, 1950-01-11, 1950-01-14, 1950-01-18, 1950-01-19, 1950-01-21, 1950-01-22, 1950-01-25, 1950-01-28, 1950-01-29, 1950-02-01, 1950-02-04, 1950-02-05, 1950-02-08, 1950-02-11, 1950-02-12, 1950-02-16, 1950-02-18, 1950-02-19, 1950-02-22, 1950-02-25, 1950-03-01, 1950-03-04, 1950-03-05, 1950-03-09, 1950-03-11, 1950-03-12, 1950-03-15, 1950-03-18, 1950-03-19, 1950-03-22, 1950-03-25, 1950-03-26
    ## 28                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1944-10-29, 1944-11-02, 1944-11-04, 1944-11-18, 1945-02-06
    ## 29                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1927-01-08, 1929-11-19, 1931-02-28, 1933-01-14
    ## 30                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1953-10-24, 1954-01-30, 1954-02-06, 1954-02-13, 1954-02-14, 1954-02-28, 1954-03-07, 1954-10-23, 1955-01-05
    ## 31                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1945-11-08
    ## 32                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1954-02-28, 1955-12-07, 1955-12-11, 1955-12-24, 1955-12-31, 1956-01-14
    ## 33 1950-10-14, 1950-10-15, 1950-10-18, 1950-10-21, 1950-10-22, 1950-10-25, 1950-10-28, 1950-10-29, 1950-11-01, 1950-11-02, 1950-11-04, 1950-11-08, 1950-11-11, 1950-11-12, 1950-11-16, 1950-11-18, 1950-11-19, 1950-11-22, 1950-11-23, 1950-11-25, 1950-11-26, 1950-11-30, 1950-12-02, 1950-12-03, 1950-12-06, 1950-12-09, 1950-12-10, 1950-12-13, 1950-12-14, 1950-12-16, 1950-12-17, 1950-12-20, 1950-12-23, 1950-12-27, 1950-12-30, 1950-12-31, 1951-01-06, 1951-01-09, 1951-01-13, 1951-01-14, 1951-01-18, 1951-01-20, 1951-01-21, 1951-01-24, 1951-01-27, 1951-01-28, 1951-02-01, 1951-02-03, 1951-02-04, 1951-02-07, 1951-02-10, 1951-02-11, 1951-02-15, 1951-02-17, 1951-02-18, 1951-02-21, 1951-02-24, 1951-03-01, 1951-03-03, 1951-03-05, 1951-03-07, 1951-03-10, 1951-03-11, 1951-03-14, 1951-03-15, 1951-03-17, 1951-03-18, 1951-03-21, 1951-03-24, 1951-03-25
    ##    mostAssistsOneGame mostAssistsOneSeason
    ## 1                   3                   35
    ## 2                   3                   53
    ## 3                   5                   66
    ## 4                   2                   17
    ## 5                   4                   41
    ## 6                   6                   95
    ## 7                   4                   46
    ## 8                   4                   57
    ## 9                   4                   57
    ## 10                  0                    0
    ## 11                  1                    1
    ## 12                  2                   14
    ## 13                  1                    2
    ## 14                  2                   14
    ## 15                  2                   12
    ## 16                  1                    2
    ## 17                  3                   16
    ## 18                  1                    4
    ## 19                  3                   49
    ## 20                  3                   43
    ## 21                  5                   29
    ## 22                  2                    5
    ## 23                  1                    3
    ## 24                  0                    0
    ## 25                  1                    1
    ## 26                  1                    1
    ## 27                  0                    0
    ## 28                  1                    5
    ## 29                  3                   21
    ## 30                  1                    7
    ## 31                  1                    1
    ## 32                  1                    5
    ## 33                  0                    0
    ##    mostAssistsSeasonIds
    ## 1              19651966
    ## 2              19961997
    ## 3              19761977
    ## 4              19961997
    ## 5              19831984
    ## 6              19921993
    ## 7              19931994
    ## 8              19801981
    ## 9              19761977
    ## 10             19251926
    ## 11             19691970
    ## 12             19241925
    ## 13             19321933
    ## 14             19571958
    ## 15             19761977
    ## 16             19821983
    ## 17             19861987
    ## 18             19221923
    ## 19             19821983
    ## 20             19921993
    ## 21   19361937, 19371938
    ## 22             19611962
    ## 23             19231924
    ## 24             19281929
    ## 25             19381939
    ## 26             19621963
    ## 27             19491950
    ## 28             19441945
    ## 29             19291930
    ## 30             19531954
    ## 31             19451946
    ## 32             19551956
    ## 33             19501951
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                mostGoalsGameDates
    ## 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1959-03-15, 1961-12-16
    ## 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      2006-04-11
    ## 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      1981-10-10
    ## 4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1996-11-16, 2000-11-29, 2000-11-30, 2002-03-26
    ## 5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1982-03-22, 1983-01-02, 1983-12-21
    ## 6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      1993-10-15
    ## 7                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      1993-12-01
    ## 8                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1980-01-12, 1980-03-29, 1981-03-28
    ## 9                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      1977-02-02
    ## 10                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1925-11-28, 1925-12-01, 1925-12-05, 1925-12-09, 1925-12-12, 1925-12-19, 1925-12-23, 1925-12-26, 1925-12-29, 1925-12-30, 1926-01-01, 1926-01-05, 1926-01-09, 1926-01-12, 1926-01-15, 1926-01-21, 1926-01-23, 1926-01-26, 1926-01-29, 1926-02-02, 1926-02-03, 1926-02-06, 1926-02-09, 1926-02-11, 1926-02-13, 1926-02-16, 1926-02-18, 1926-02-20, 1926-02-22, 1926-02-27, 1926-03-04, 1926-03-06, 1926-03-11, 1926-03-13, 1926-03-16, 1926-03-17
    ## 11                                                                                                                                                                                                                                                 1969-10-11, 1969-10-15, 1969-10-18, 1969-10-19, 1969-10-22, 1969-10-25, 1969-10-29, 1969-11-01, 1969-11-02, 1969-11-04, 1969-11-05, 1969-11-08, 1969-11-09, 1969-11-12, 1969-11-15, 1969-11-19, 1969-11-22, 1969-11-23, 1969-11-26, 1969-11-29, 1969-11-30, 1969-12-03, 1969-12-06, 1969-12-07, 1969-12-10, 1969-12-11, 1969-12-13, 1969-12-14, 1969-12-20, 1969-12-21, 1969-12-24, 1969-12-26, 1969-12-27, 1969-12-31, 1970-01-03, 1970-01-04, 1970-01-07, 1970-01-10, 1970-01-14, 1970-01-15, 1970-01-17, 1970-01-22, 1970-01-23, 1970-01-25, 1970-01-28, 1970-01-31, 1970-02-01, 1970-02-04, 1970-02-05, 1970-02-07, 1970-02-11, 1970-02-12, 1970-02-14, 1970-02-15, 1970-02-18, 1970-02-21, 1970-02-22, 1970-02-25, 1970-02-28, 1970-03-01, 1970-03-03, 1970-03-05, 1970-03-07, 1970-03-11, 1970-03-14, 1970-03-15, 1970-03-18, 1970-03-21, 1970-03-22, 1970-03-25, 1970-03-28, 1970-03-29, 1970-04-01, 1970-04-02, 1970-04-04, 1970-04-05
    ## 12                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1922-12-23, 1924-12-03, 1926-01-05
    ## 13                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1932-11-10, 1932-11-12, 1932-11-17, 1932-11-20, 1932-11-24, 1932-11-26, 1932-11-27, 1932-12-03, 1932-12-08, 1932-12-10, 1932-12-13, 1932-12-15, 1932-12-17, 1932-12-20, 1932-12-22, 1932-12-24, 1932-12-27, 1932-12-29, 1933-01-01, 1933-01-03, 1933-01-05, 1933-01-07, 1933-01-10, 1933-01-14, 1933-01-17, 1933-01-19, 1933-01-24, 1933-01-26, 1933-01-28, 1933-01-31, 1933-02-04, 1933-02-07, 1933-02-11, 1933-02-14, 1933-02-16, 1933-02-18, 1933-02-23, 1933-02-25, 1933-02-28, 1933-03-02, 1933-03-04, 1933-03-05, 1933-03-07, 1933-03-11, 1933-03-16, 1933-03-18, 1933-03-21, 1933-03-23
    ## 14                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1958-01-12
    ## 15                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1975-02-25
    ## 16                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1982-11-03
    ## 17                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1987-02-18
    ## 18                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1922-12-23
    ## 19                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1983-01-12, 1985-03-24
    ## 20                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1992-03-17
    ## 21                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1939-01-12, 1946-03-02
    ## 22                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1962-03-24, 1962-12-26
    ## 23                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1923-12-22
    ## 24                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1928-12-01
    ## 25 1937-11-04, 1937-11-06, 1937-11-13, 1937-11-14, 1937-11-18, 1937-11-20, 1937-11-21, 1937-11-23, 1937-11-25, 1937-11-27, 1937-12-04, 1937-12-11, 1937-12-14, 1937-12-16, 1937-12-18, 1937-12-25, 1937-12-26, 1937-12-28, 1938-01-01, 1938-01-04, 1938-01-06, 1938-01-08, 1938-01-13, 1938-01-15, 1938-01-16, 1938-01-20, 1938-01-22, 1938-01-29, 1938-02-01, 1938-02-03, 1938-02-05, 1938-02-06, 1938-02-10, 1938-02-12, 1938-02-13, 1938-02-17, 1938-02-19, 1938-02-20, 1938-02-22, 1938-02-26, 1938-03-01, 1938-03-05, 1938-03-06, 1938-03-08, 1938-03-12, 1938-03-17, 1938-03-19, 1938-03-20, 1938-11-03, 1938-11-05, 1938-11-10, 1938-11-12, 1938-11-15, 1938-11-17, 1938-11-19, 1938-11-20, 1938-11-24, 1938-11-26, 1938-12-03, 1938-12-04, 1938-12-10, 1938-12-15, 1938-12-17, 1938-12-24, 1938-12-26, 1938-12-27, 1938-12-31, 1939-01-01, 1939-01-03, 1939-01-07, 1939-01-08, 1939-01-12, 1939-01-14, 1939-01-15, 1939-01-17, 1939-01-21, 1939-01-24, 1939-01-28, 1939-01-29, 1939-02-02, 1939-02-04, 1939-02-05, 1939-02-07, 1939-02-11, 1939-02-12, 1939-02-18, 1939-02-19, 1939-02-25, 1939-02-26, 1939-02-28, 1939-03-02, 1939-03-04, 1939-03-11, 1939-03-14, 1939-03-18, 1939-03-19
    ## 26                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1962-12-15
    ## 27                                                                                                                                                                                                                                                                                                                         1949-10-15, 1949-10-16, 1949-10-19, 1949-10-22, 1949-10-27, 1949-10-29, 1949-10-30, 1949-11-02, 1949-11-05, 1949-11-06, 1949-11-10, 1949-11-12, 1949-11-13, 1949-11-16, 1949-11-19, 1949-11-20, 1949-11-23, 1949-11-24, 1949-11-26, 1949-11-27, 1949-12-01, 1949-12-03, 1949-12-04, 1949-12-08, 1949-12-10, 1949-12-11, 1949-12-14, 1949-12-15, 1949-12-17, 1949-12-18, 1949-12-21, 1949-12-24, 1949-12-25, 1949-12-28, 1949-12-31, 1950-01-01, 1950-01-04, 1950-01-07, 1950-01-11, 1950-01-14, 1950-01-18, 1950-01-19, 1950-01-21, 1950-01-22, 1950-01-25, 1950-01-28, 1950-01-29, 1950-02-01, 1950-02-04, 1950-02-05, 1950-02-08, 1950-02-11, 1950-02-12, 1950-02-16, 1950-02-18, 1950-02-19, 1950-02-22, 1950-02-25, 1950-03-01, 1950-03-04, 1950-03-05, 1950-03-09, 1950-03-11, 1950-03-12, 1950-03-15, 1950-03-18, 1950-03-19, 1950-03-22, 1950-03-25, 1950-03-26
    ## 28                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1944-11-04, 1945-01-06, 1945-01-28, 1945-02-18
    ## 29                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1931-03-21
    ## 30                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1954-01-02, 1954-02-20, 1954-10-09, 1955-01-02, 1955-01-08, 1955-01-19
    ## 31                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1945-10-27, 1945-11-01, 1945-11-03, 1945-11-04, 1945-11-07, 1945-11-08, 1945-11-10, 1945-11-11, 1945-11-14, 1945-11-17, 1945-11-18, 1945-11-24, 1945-11-25, 1945-12-01, 1945-12-02, 1945-12-08, 1945-12-09, 1945-12-13, 1945-12-15, 1945-12-16, 1945-12-22, 1945-12-23, 1945-12-25, 1945-12-26, 1945-12-29, 1946-01-01, 1946-01-05, 1946-01-10, 1946-01-12, 1946-01-19, 1946-01-20, 1946-01-23, 1946-01-26, 1946-02-02, 1946-02-03, 1946-02-06, 1946-02-09, 1946-02-10, 1946-02-16, 1946-02-23, 1946-02-24, 1946-02-27, 1946-03-02, 1946-03-03, 1946-03-06, 1946-03-09, 1946-03-10, 1946-03-14, 1946-03-16, 1946-03-17
    ## 32                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1955-10-19, 1955-11-19, 1955-12-10, 1956-01-14
    ## 33                                                                                                                                                                                                                                                                                                                         1950-10-14, 1950-10-15, 1950-10-18, 1950-10-21, 1950-10-22, 1950-10-25, 1950-10-28, 1950-10-29, 1950-11-01, 1950-11-02, 1950-11-04, 1950-11-08, 1950-11-11, 1950-11-12, 1950-11-16, 1950-11-18, 1950-11-19, 1950-11-22, 1950-11-23, 1950-11-25, 1950-11-26, 1950-11-30, 1950-12-02, 1950-12-03, 1950-12-06, 1950-12-09, 1950-12-10, 1950-12-13, 1950-12-14, 1950-12-16, 1950-12-17, 1950-12-20, 1950-12-23, 1950-12-27, 1950-12-30, 1950-12-31, 1951-01-06, 1951-01-09, 1951-01-13, 1951-01-14, 1951-01-18, 1951-01-20, 1951-01-21, 1951-01-24, 1951-01-27, 1951-01-28, 1951-02-01, 1951-02-03, 1951-02-04, 1951-02-07, 1951-02-10, 1951-02-11, 1951-02-15, 1951-02-17, 1951-02-18, 1951-02-21, 1951-02-24, 1951-03-01, 1951-03-03, 1951-03-05, 1951-03-07, 1951-03-10, 1951-03-11, 1951-03-14, 1951-03-15, 1951-03-17, 1951-03-18, 1951-03-21, 1951-03-24, 1951-03-25
    ##    mostGoalsOneGame mostGoalsOneSeason mostGoalsSeasonIds
    ## 1                 3                 23           19591960
    ## 2                 4                 41 19961997, 20012002
    ## 3                 3                 19           19791980
    ## 4                 2                 15           20022003
    ## 5                 4                 54           19811982
    ## 6                 3                 32 19921993, 19951996
    ## 7                 3                 53           19931994
    ## 8                 3                 40           19801981
    ## 9                 5                 22           19761977
    ## 10                0                  0           19251926
    ## 11                0                  0           19691970
    ## 12                3                 21 19241925, 19251926
    ## 13                0                  0           19321933
    ## 14                2                 10           19571958
    ## 15                3                  7           19741975
    ## 16                1                  1           19821983
    ## 17                2                  7           19861987
    ## 18                2                  5           19221923
    ## 19                3                 37           19831984
    ## 20                3                 24           19911992
    ## 21                4                 26           19471948
    ## 22                1                  1 19611962, 19621963
    ## 23                1                  1           19231924
    ## 24                1                  1           19281929
    ## 25                0                  0 19371938, 19381939
    ## 26                1                  1           19621963
    ## 27                0                  0           19491950
    ## 28                1                  4           19441945
    ## 29                4                 23           19301931
    ## 30                1                  4           19541955
    ## 31                0                  0           19451946
    ## 32                2                 14           19551956
    ## 33                0                  0           19501951
    ##    mostPenaltyMinutesOneSeason mostPenaltyMinutesSeasonIds
    ## 1                           97                    19551956
    ## 2                           94                    20012002
    ## 3                          170                    19811982
    ## 4                          365                    19971998
    ## 5                          229                    19801981
    ## 6                          105                    19931994
    ## 7                           98                    19931994
    ## 8                          203                    19811982
    ## 9                          104                    19801981
    ## 10                           0                    19251926
    ## 11                           0                    19691970
    ## 12                          67                    19241925
    ## 13                           0                    19321933
    ## 14                          12                    19571958
    ## 15                          12          19741975, 19761977
    ## 16                          11                    19821983
    ## 17                          66                    19861987
    ## 18                          10                    19221923
    ## 19                          31                    19801981
    ## 20                         117                    19921993
    ## 21                          12                    19471948
    ## 22                          68                    19611962
    ## 23                           4                    19231924
    ## 24                          10                    19281929
    ## 25                           0          19371938, 19381939
    ## 26                           2                    19621963
    ## 27                           9                    19491950
    ## 28                           6                    19441945
    ## 29                          80          19261927, 19281929
    ## 30                          70                    19531954
    ## 31                           6                    19451946
    ## 32                          40                    19551956
    ## 33                           2                    19501951
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       mostPointsGameDates
    ## 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1957-03-16, 1962-02-25, 1964-12-12, 1965-03-21, 1967-11-02
    ## 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              2006-04-11
    ## 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1977-12-16, 1978-10-14
    ## 4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              2000-11-29
    ## 5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      1982-01-31, 1982-03-22, 1983-12-08
    ## 6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1993-02-13
    ## 7                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      1993-02-13, 1994-01-08, 1994-04-14
    ## 8                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              1980-01-12
    ## 9                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  1977-02-02, 1977-02-20
    ## 10                                                                                                                                                                                                                                                                                                                                                                                                                         1925-11-28, 1925-12-01, 1925-12-05, 1925-12-09, 1925-12-12, 1925-12-19, 1925-12-23, 1925-12-26, 1925-12-29, 1925-12-30, 1926-01-01, 1926-01-05, 1926-01-09, 1926-01-12, 1926-01-15, 1926-01-21, 1926-01-23, 1926-01-26, 1926-01-29, 1926-02-02, 1926-02-03, 1926-02-06, 1926-02-09, 1926-02-11, 1926-02-13, 1926-02-16, 1926-02-18, 1926-02-20, 1926-02-22, 1926-02-27, 1926-03-04, 1926-03-06, 1926-03-11, 1926-03-13, 1926-03-16, 1926-03-17
    ## 11                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1970-03-07
    ## 12                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1922-12-23, 1923-02-03, 1923-02-14, 1924-12-10
    ## 13                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1933-01-03, 1933-01-05
    ## 14                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1957-12-25, 1958-01-12, 1958-01-26, 1958-02-08, 1958-02-09
    ## 15                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1975-02-25
    ## 16                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1982-10-13, 1982-10-30, 1982-11-03
    ## 17                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1986-11-05
    ## 18                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1922-12-23
    ## 19                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1981-10-10
    ## 20                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1993-01-08
    ## 21                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1942-11-28
    ## 22                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1961-10-28
    ## 23                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1923-12-22, 1923-12-26, 1924-01-05, 1924-01-19
    ## 24                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1928-12-01
    ## 25                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1938-11-24
    ## 26                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 1962-12-15, 1962-12-16
    ## 27 1949-10-15, 1949-10-16, 1949-10-19, 1949-10-22, 1949-10-27, 1949-10-29, 1949-10-30, 1949-11-02, 1949-11-05, 1949-11-06, 1949-11-10, 1949-11-12, 1949-11-13, 1949-11-16, 1949-11-19, 1949-11-20, 1949-11-23, 1949-11-24, 1949-11-26, 1949-11-27, 1949-12-01, 1949-12-03, 1949-12-04, 1949-12-08, 1949-12-10, 1949-12-11, 1949-12-14, 1949-12-15, 1949-12-17, 1949-12-18, 1949-12-21, 1949-12-24, 1949-12-25, 1949-12-28, 1949-12-31, 1950-01-01, 1950-01-04, 1950-01-07, 1950-01-11, 1950-01-14, 1950-01-18, 1950-01-19, 1950-01-21, 1950-01-22, 1950-01-25, 1950-01-28, 1950-01-29, 1950-02-01, 1950-02-04, 1950-02-05, 1950-02-08, 1950-02-11, 1950-02-12, 1950-02-16, 1950-02-18, 1950-02-19, 1950-02-22, 1950-02-25, 1950-03-01, 1950-03-04, 1950-03-05, 1950-03-09, 1950-03-11, 1950-03-12, 1950-03-15, 1950-03-18, 1950-03-19, 1950-03-22, 1950-03-25, 1950-03-26
    ## 28                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1944-11-04
    ## 29                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         1929-12-14, 1931-02-28, 1931-03-21, 1933-01-14
    ## 30                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     1953-10-24, 1954-01-02, 1954-01-30, 1954-02-06, 1954-02-13, 1954-02-14, 1954-02-20, 1954-02-28, 1954-03-07, 1954-10-09, 1954-10-23, 1955-01-02, 1955-01-05, 1955-01-08, 1955-01-19
    ## 31                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1945-11-08
    ## 32                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             1956-01-14
    ## 33 1950-10-14, 1950-10-15, 1950-10-18, 1950-10-21, 1950-10-22, 1950-10-25, 1950-10-28, 1950-10-29, 1950-11-01, 1950-11-02, 1950-11-04, 1950-11-08, 1950-11-11, 1950-11-12, 1950-11-16, 1950-11-18, 1950-11-19, 1950-11-22, 1950-11-23, 1950-11-25, 1950-11-26, 1950-11-30, 1950-12-02, 1950-12-03, 1950-12-06, 1950-12-09, 1950-12-10, 1950-12-13, 1950-12-14, 1950-12-16, 1950-12-17, 1950-12-20, 1950-12-23, 1950-12-27, 1950-12-30, 1950-12-31, 1951-01-06, 1951-01-09, 1951-01-13, 1951-01-14, 1951-01-18, 1951-01-20, 1951-01-21, 1951-01-24, 1951-01-27, 1951-01-28, 1951-02-01, 1951-02-03, 1951-02-04, 1951-02-07, 1951-02-10, 1951-02-11, 1951-02-15, 1951-02-17, 1951-02-18, 1951-02-21, 1951-02-24, 1951-03-01, 1951-03-03, 1951-03-05, 1951-03-07, 1951-03-10, 1951-03-11, 1951-03-14, 1951-03-15, 1951-03-17, 1951-03-18, 1951-03-21, 1951-03-24, 1951-03-25
    ##    mostPointsOneGame mostPointsOneSeason
    ## 1                  4                  53
    ## 2                  6                  94
    ## 3                  5                  78
    ## 4                  3                  29
    ## 5                  5                  93
    ## 6                  6                 127
    ## 7                  4                  99
    ## 8                  5                  97
    ## 9                  5                  79
    ## 10                 0                   0
    ## 11                 1                   1
    ## 12                 4                  35
    ## 13                 1                   2
    ## 14                 2                  24
    ## 15                 3                  18
    ## 16                 1                   3
    ## 17                 3                  23
    ## 18                 3                   9
    ## 19                 5                  80
    ## 20                 5                  65
    ## 21                 6                  53
    ## 22                 2                   6
    ## 23                 1                   4
    ## 24                 1                   1
    ## 25                 1                   1
    ## 26                 1                   2
    ## 27                 0                   0
    ## 28                 2                   9
    ## 29                 4                  43
    ## 30                 1                   9
    ## 31                 1                   1
    ## 32                 3                  19
    ## 33                 0                   0
    ##    mostPointsSeasonIds penaltyMinutes playerId points
    ## 1             19611962            726  8444971    713
    ## 2             19961997            748  8451774    987
    ## 3             19761977           1292  8451102    768
    ## 4             20022003           2265  8446454    196
    ## 5             19831984            940  8452152    537
    ## 6             19921993            386  8447206    452
    ## 7             19931994            194  8445000    219
    ## 8             19801981            420  8450107    203
    ## 9             19761977            651  8451990    414
    ## 10            19251926              0  8444851      0
    ## 11            19691970              0  8444859      1
    ## 12            19241925            318  8444861    119
    ## 13            19321933              0  8444863      2
    ## 14            19571958             18  8444868     33
    ## 15            19741975             30  8444869     39
    ## 16            19821983             11  8444896      3
    ## 17            19861987             76  8444914     26
    ## 18            19221923             12  8444936     13
    ## 19            19821983            166  8444944    393
    ## 20            19921993            267  8444945    157
    ## 21            19471948             56  8444954    432
    ## 22            19611962             74  8444955      9
    ## 23            19231924              4  8444956      4
    ## 24            19281929             10  8444964      1
    ## 25            19381939              0  8444972      1
    ## 26            19621963              2  8444973      2
    ## 27            19491950              9  8444979      0
    ## 28            19441945              6  8444995      9
    ## 29            19291930            488  8444998    192
    ## 30            19531954            128  8445001     15
    ## 31            19451946              6  8445003      1
    ## 32            19551956             48  8445004     20
    ## 33            19501951              2  8445018      0
    ##    positionCode rookiePoints seasons
    ## 1             R           25      21
    ## 2             C           NA      13
    ## 3             D           39      16
    ## 4             R            0      12
    ## 5             R           NA       8
    ## 6             C           NA       7
    ## 7             L           NA       4
    ## 8             R           NA       3
    ## 9             D           35       9
    ## 10            L            0       1
    ## 11            C            1       1
    ## 12            C            0       6
    ## 13            L           NA       1
    ## 14            L            6       3
    ## 15            D           18       3
    ## 16            C            3       1
    ## 17            R           NA       2
    ## 18            L            0       4
    ## 19            R           26       8
    ## 20            R           NA       3
    ## 21            C           45      10
    ## 22            D           NA       4
    ## 23            L           NA       1
    ## 24            D           NA       1
    ## 25            C            0       2
    ## 26            R            2       1
    ## 27            D            0       1
    ## 28            D            9       1
    ## 29            R           28       8
    ## 30            R            9       3
    ## 31            D            1       1
    ## 32            L           19       4
    ## 33            R            0       1
    ##  [ reached 'max' / getOption("max.print") -- omitted 868 rows ]
    ## 
    ## $total
    ## [1] 901

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
