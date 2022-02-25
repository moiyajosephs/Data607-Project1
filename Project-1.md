Project 1
================
Moiya Josephs
2/15/2022

# Overview

For this project we were given a text file with chess tournament results
where the information has some structure. The task was to generate a
.CSV file containing the name, pre-rating score of an individual player,
their total points and the average of all their opponents pre-ratings.
The tournament results are stored in the text file called
tournamentinfo.

Guidelines for this project

-   The rounds that end with whats detailed below will not count:
    -   B(ye)  
    -   H(alf point)  
    -   U(nplayed)
-   Instead we will include:
    -   W(in)  
    -   L(ose)  
    -   D(raw)

As of right now the data looks like:

![Tournament](https://raw.githubusercontent.com/moiyajosephs/Data607-Project1/main/tournament_sc%20(2).png)

The final format we want is:

| Player’s Name | Player’s State | Total # of points | Player’s Pre Rating | Avg Pre-Chess of Opponents |
|:-------------:|:--------------:|:-----------------:|:-------------------:|:--------------------------:|
|   Gary Hua    |       ON       |        6.0        |        1794         |            1605            |

## Read in the text file

``` r
file = "https://raw.githubusercontent.com/moiyajosephs/Data607-Project1/main/tournamentinfo.txt"
tm= read.delim(file,header=FALSE,sep="|", dec = ".")
```

# Data cleaning

## Initial data frame

Each players information is on two separate rows enclosed between the –.
To start, remove any row in the data frame that starts with –. This is
not needed for the data to be separated any longer.

``` r
tm = tm %>% 
  filter(!grepl('^-', V1))
head(tm) %>% kbl() %>% kable_styling(bootstrap_options = c("striped", "hover"))
```

<table class="table table-striped table-hover" style="margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
V1
</th>
<th style="text-align:left;">
V2
</th>
<th style="text-align:left;">
V3
</th>
<th style="text-align:left;">
V4
</th>
<th style="text-align:left;">
V5
</th>
<th style="text-align:left;">
V6
</th>
<th style="text-align:left;">
V7
</th>
<th style="text-align:left;">
V8
</th>
<th style="text-align:left;">
V9
</th>
<th style="text-align:left;">
V10
</th>
<th style="text-align:left;">
V11
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
Pair
</td>
<td style="text-align:left;">
Player Name
</td>
<td style="text-align:left;">
Total
</td>
<td style="text-align:left;">
Round
</td>
<td style="text-align:left;">
Round
</td>
<td style="text-align:left;">
Round
</td>
<td style="text-align:left;">
Round
</td>
<td style="text-align:left;">
Round
</td>
<td style="text-align:left;">
Round
</td>
<td style="text-align:left;">
Round
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:left;">
Num
</td>
<td style="text-align:left;">
USCF ID / Rtg (Pre->Post)
</td>
<td style="text-align:left;">
Pts
</td>
<td style="text-align:left;">
1
</td>
<td style="text-align:left;">
2
</td>
<td style="text-align:left;">
3
</td>
<td style="text-align:left;">
4
</td>
<td style="text-align:left;">
5
</td>
<td style="text-align:left;">
6
</td>
<td style="text-align:left;">
7
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:left;">
1
</td>
<td style="text-align:left;">
GARY HUA
</td>
<td style="text-align:left;">
6.0
</td>
<td style="text-align:left;">
W 39
</td>
<td style="text-align:left;">
W 21
</td>
<td style="text-align:left;">
W 18
</td>
<td style="text-align:left;">
W 14
</td>
<td style="text-align:left;">
W 7
</td>
<td style="text-align:left;">
D 12
</td>
<td style="text-align:left;">
D 4
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:left;">
ON
</td>
<td style="text-align:left;">
15445895 / R: 1794 ->1817
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:left;">
2
</td>
<td style="text-align:left;">
DAKSHESH DARURI
</td>
<td style="text-align:left;">
6.0
</td>
<td style="text-align:left;">
W 63
</td>
<td style="text-align:left;">
W 58
</td>
<td style="text-align:left;">
L 4
</td>
<td style="text-align:left;">
W 17
</td>
<td style="text-align:left;">
W 16
</td>
<td style="text-align:left;">
W 20
</td>
<td style="text-align:left;">
W 7
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
14598900 / R: 1553 ->1663
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
NA
</td>
</tr>
</tbody>
</table>

Next remove the two rows that contain header information in the start of
the text file, I will provide actual column names.

``` r
tm <- tm[-c(1:2),-11]
```

<table class="table table-striped table-hover" style="margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
V1
</th>
<th style="text-align:left;">
V2
</th>
<th style="text-align:left;">
V3
</th>
<th style="text-align:left;">
V4
</th>
<th style="text-align:left;">
V5
</th>
<th style="text-align:left;">
V6
</th>
<th style="text-align:left;">
V7
</th>
<th style="text-align:left;">
V8
</th>
<th style="text-align:left;">
V9
</th>
<th style="text-align:left;">
V10
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
3
</td>
<td style="text-align:left;">
1
</td>
<td style="text-align:left;">
GARY HUA
</td>
<td style="text-align:left;">
6.0
</td>
<td style="text-align:left;">
W 39
</td>
<td style="text-align:left;">
W 21
</td>
<td style="text-align:left;">
W 18
</td>
<td style="text-align:left;">
W 14
</td>
<td style="text-align:left;">
W 7
</td>
<td style="text-align:left;">
D 12
</td>
<td style="text-align:left;">
D 4
</td>
</tr>
<tr>
<td style="text-align:left;">
4
</td>
<td style="text-align:left;">
ON
</td>
<td style="text-align:left;">
15445895 / R: 1794 ->1817
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
</tr>
<tr>
<td style="text-align:left;">
5
</td>
<td style="text-align:left;">
2
</td>
<td style="text-align:left;">
DAKSHESH DARURI
</td>
<td style="text-align:left;">
6.0
</td>
<td style="text-align:left;">
W 63
</td>
<td style="text-align:left;">
W 58
</td>
<td style="text-align:left;">
L 4
</td>
<td style="text-align:left;">
W 17
</td>
<td style="text-align:left;">
W 16
</td>
<td style="text-align:left;">
W 20
</td>
<td style="text-align:left;">
W 7
</td>
</tr>
<tr>
<td style="text-align:left;">
6
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
14598900 / R: 1553 ->1663
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
</tr>
<tr>
<td style="text-align:left;">
7
</td>
<td style="text-align:left;">
3
</td>
<td style="text-align:left;">
ADITYA BAJAJ
</td>
<td style="text-align:left;">
6.0
</td>
<td style="text-align:left;">
L 8
</td>
<td style="text-align:left;">
W 61
</td>
<td style="text-align:left;">
W 25
</td>
<td style="text-align:left;">
W 21
</td>
<td style="text-align:left;">
W 11
</td>
<td style="text-align:left;">
W 13
</td>
<td style="text-align:left;">
W 12
</td>
</tr>
<tr>
<td style="text-align:left;">
8
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
14959604 / R: 1384 ->1640
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
</tr>
</tbody>
</table>

### Divide the data

Split by each row to individual data frames after skipping every two,
that way I can append the second row to the first row. Tm1 represents
the first set of rows while Tm2 represents the rest.

``` r
tm1 <- tm[row.names(tm) %in% seq(1,132,2),] #first part of the players data
tm2 <- tm[row.names(tm) %in% seq(2,132,2),] #second part of the players data
```

Name the columns of each divided data frame so it will be clear when we
combine them. There is the players number, their name, total points and
seven rounds total.

``` r
names(tm1) <- c("player_num", "player_name", "total points", "round1", "round2", "round3","round4","round5","round6","round7")
head(tm2) %>% kbl() %>% kable_styling(bootstrap_options = c("striped", "hover"))
```

<table class="table table-striped table-hover" style="margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
V1
</th>
<th style="text-align:left;">
V2
</th>
<th style="text-align:left;">
V3
</th>
<th style="text-align:left;">
V4
</th>
<th style="text-align:left;">
V5
</th>
<th style="text-align:left;">
V6
</th>
<th style="text-align:left;">
V7
</th>
<th style="text-align:left;">
V8
</th>
<th style="text-align:left;">
V9
</th>
<th style="text-align:left;">
V10
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
4
</td>
<td style="text-align:left;">
ON
</td>
<td style="text-align:left;">
15445895 / R: 1794 ->1817
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
</tr>
<tr>
<td style="text-align:left;">
6
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
14598900 / R: 1553 ->1663
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
</tr>
<tr>
<td style="text-align:left;">
8
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
14959604 / R: 1384 ->1640
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
</tr>
<tr>
<td style="text-align:left;">
10
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
12616049 / R: 1716 ->1744
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
B
</td>
</tr>
<tr>
<td style="text-align:left;">
12
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
14601533 / R: 1655 ->1690
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
</tr>
<tr>
<td style="text-align:left;">
14
</td>
<td style="text-align:left;">
OH
</td>
<td style="text-align:left;">
15055204 / R: 1686 ->1687
</td>
<td style="text-align:left;">
N:3
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
</tr>
</tbody>
</table>

The second part of the players information contains the state the player
is from, their pre-rating score, points, and some additional round
information.

``` r
names(tm2)<- c("state","pre_rating", "pts","round1", "round2", "round3","round4","round5","round6","round7")
head(tm2) %>% kbl() %>% kable_styling(bootstrap_options = c("striped", "hover"))
```

<table class="table table-striped table-hover" style="margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
state
</th>
<th style="text-align:left;">
pre_rating
</th>
<th style="text-align:left;">
pts
</th>
<th style="text-align:left;">
round1
</th>
<th style="text-align:left;">
round2
</th>
<th style="text-align:left;">
round3
</th>
<th style="text-align:left;">
round4
</th>
<th style="text-align:left;">
round5
</th>
<th style="text-align:left;">
round6
</th>
<th style="text-align:left;">
round7
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
4
</td>
<td style="text-align:left;">
ON
</td>
<td style="text-align:left;">
15445895 / R: 1794 ->1817
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
</tr>
<tr>
<td style="text-align:left;">
6
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
14598900 / R: 1553 ->1663
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
</tr>
<tr>
<td style="text-align:left;">
8
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
14959604 / R: 1384 ->1640
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
</tr>
<tr>
<td style="text-align:left;">
10
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
12616049 / R: 1716 ->1744
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
B
</td>
</tr>
<tr>
<td style="text-align:left;">
12
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
14601533 / R: 1655 ->1690
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
</tr>
<tr>
<td style="text-align:left;">
14
</td>
<td style="text-align:left;">
OH
</td>
<td style="text-align:left;">
15055204 / R: 1686 ->1687
</td>
<td style="text-align:left;">
N:3
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
</tr>
</tbody>
</table>

To get the pre score number may take more cleaning. Currently it has
additional and unnecessary characters. We want everything remove
everything before the R: but before the ->. remove the n:2 columns also.

### Clean the individual data frames

I cleaned the second data frame step by step.

First split by the R.

``` r
tm2$`pre_rating`<-sub(".*R:","",tm2$`pre_rating`)
```

<table class="table table-striped table-hover" style="margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
state
</th>
<th style="text-align:left;">
pre_rating
</th>
<th style="text-align:left;">
pts
</th>
<th style="text-align:left;">
round1
</th>
<th style="text-align:left;">
round2
</th>
<th style="text-align:left;">
round3
</th>
<th style="text-align:left;">
round4
</th>
<th style="text-align:left;">
round5
</th>
<th style="text-align:left;">
round6
</th>
<th style="text-align:left;">
round7
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
4
</td>
<td style="text-align:left;">
ON
</td>
<td style="text-align:left;">
1794 ->1817
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
</tr>
<tr>
<td style="text-align:left;">
6
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1553 ->1663
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
</tr>
<tr>
<td style="text-align:left;">
8
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1384 ->1640
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
</tr>
<tr>
<td style="text-align:left;">
10
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1716 ->1744
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
B
</td>
</tr>
<tr>
<td style="text-align:left;">
12
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1655 ->1690
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
</tr>
<tr>
<td style="text-align:left;">
14
</td>
<td style="text-align:left;">
OH
</td>
<td style="text-align:left;">
1686 ->1687
</td>
<td style="text-align:left;">
N:3
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
</tr>
</tbody>
</table>

Next remove after the “->”

``` r
tm2$`pre_rating`<- sub("->.*","",tm2$`pre_rating`)
```

<table class="table table-striped table-hover" style="margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
state
</th>
<th style="text-align:left;">
pre_rating
</th>
<th style="text-align:left;">
pts
</th>
<th style="text-align:left;">
round1
</th>
<th style="text-align:left;">
round2
</th>
<th style="text-align:left;">
round3
</th>
<th style="text-align:left;">
round4
</th>
<th style="text-align:left;">
round5
</th>
<th style="text-align:left;">
round6
</th>
<th style="text-align:left;">
round7
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
4
</td>
<td style="text-align:left;">
ON
</td>
<td style="text-align:left;">
1794
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
</tr>
<tr>
<td style="text-align:left;">
6
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1553
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
</tr>
<tr>
<td style="text-align:left;">
8
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1384
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
</tr>
<tr>
<td style="text-align:left;">
10
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1716
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
B
</td>
</tr>
<tr>
<td style="text-align:left;">
12
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1655
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
</tr>
<tr>
<td style="text-align:left;">
14
</td>
<td style="text-align:left;">
OH
</td>
<td style="text-align:left;">
1686
</td>
<td style="text-align:left;">
N:3
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
</tr>
</tbody>
</table>

Keep anything before the P.

``` r
tm2$`pre_rating`<- sub("P.*","",tm2$`pre_rating`)
```

<table class="table table-striped table-hover" style="margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
state
</th>
<th style="text-align:left;">
pre_rating
</th>
<th style="text-align:left;">
pts
</th>
<th style="text-align:left;">
round1
</th>
<th style="text-align:left;">
round2
</th>
<th style="text-align:left;">
round3
</th>
<th style="text-align:left;">
round4
</th>
<th style="text-align:left;">
round5
</th>
<th style="text-align:left;">
round6
</th>
<th style="text-align:left;">
round7
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
4
</td>
<td style="text-align:left;">
ON
</td>
<td style="text-align:left;">
1794
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
</tr>
<tr>
<td style="text-align:left;">
6
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1553
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
</tr>
<tr>
<td style="text-align:left;">
8
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1384
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
</tr>
<tr>
<td style="text-align:left;">
10
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1716
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
B
</td>
</tr>
<tr>
<td style="text-align:left;">
12
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1655
</td>
<td style="text-align:left;">
N:2
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
</tr>
<tr>
<td style="text-align:left;">
14
</td>
<td style="text-align:left;">
OH
</td>
<td style="text-align:left;">
1686
</td>
<td style="text-align:left;">
N:3
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
W
</td>
<td style="text-align:left;">
B
</td>
</tr>
</tbody>
</table>

Realized I do not need anything passed pts. Columns 3 to 10 should be
removed.

``` r
tm2<- tm2[,1:2]
```

<table class="table table-striped table-hover" style="margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
state
</th>
<th style="text-align:left;">
pre_rating
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
4
</td>
<td style="text-align:left;">
ON
</td>
<td style="text-align:left;">
1794
</td>
</tr>
<tr>
<td style="text-align:left;">
6
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1553
</td>
</tr>
<tr>
<td style="text-align:left;">
8
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1384
</td>
</tr>
<tr>
<td style="text-align:left;">
10
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1716
</td>
</tr>
<tr>
<td style="text-align:left;">
12
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1655
</td>
</tr>
<tr>
<td style="text-align:left;">
14
</td>
<td style="text-align:left;">
OH
</td>
<td style="text-align:left;">
1686
</td>
</tr>
</tbody>
</table>

Now to combine.

### Combine the data frames

To append the two data frames use c bind. This will be saved into tm3.

``` r
tm3 <- cbind(tm1,tm2)
```

<table class="table table-striped table-hover" style="margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:left;">
player_num
</th>
<th style="text-align:left;">
player_name
</th>
<th style="text-align:left;">
total points
</th>
<th style="text-align:left;">
round1
</th>
<th style="text-align:left;">
round2
</th>
<th style="text-align:left;">
round3
</th>
<th style="text-align:left;">
round4
</th>
<th style="text-align:left;">
round5
</th>
<th style="text-align:left;">
round6
</th>
<th style="text-align:left;">
round7
</th>
<th style="text-align:left;">
state
</th>
<th style="text-align:left;">
pre_rating
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
3
</td>
<td style="text-align:left;">
1
</td>
<td style="text-align:left;">
GARY HUA
</td>
<td style="text-align:left;">
6.0
</td>
<td style="text-align:left;">
W 39
</td>
<td style="text-align:left;">
W 21
</td>
<td style="text-align:left;">
W 18
</td>
<td style="text-align:left;">
W 14
</td>
<td style="text-align:left;">
W 7
</td>
<td style="text-align:left;">
D 12
</td>
<td style="text-align:left;">
D 4
</td>
<td style="text-align:left;">
ON
</td>
<td style="text-align:left;">
1794
</td>
</tr>
<tr>
<td style="text-align:left;">
5
</td>
<td style="text-align:left;">
2
</td>
<td style="text-align:left;">
DAKSHESH DARURI
</td>
<td style="text-align:left;">
6.0
</td>
<td style="text-align:left;">
W 63
</td>
<td style="text-align:left;">
W 58
</td>
<td style="text-align:left;">
L 4
</td>
<td style="text-align:left;">
W 17
</td>
<td style="text-align:left;">
W 16
</td>
<td style="text-align:left;">
W 20
</td>
<td style="text-align:left;">
W 7
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1553
</td>
</tr>
<tr>
<td style="text-align:left;">
7
</td>
<td style="text-align:left;">
3
</td>
<td style="text-align:left;">
ADITYA BAJAJ
</td>
<td style="text-align:left;">
6.0
</td>
<td style="text-align:left;">
L 8
</td>
<td style="text-align:left;">
W 61
</td>
<td style="text-align:left;">
W 25
</td>
<td style="text-align:left;">
W 21
</td>
<td style="text-align:left;">
W 11
</td>
<td style="text-align:left;">
W 13
</td>
<td style="text-align:left;">
W 12
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1384
</td>
</tr>
<tr>
<td style="text-align:left;">
9
</td>
<td style="text-align:left;">
4
</td>
<td style="text-align:left;">
PATRICK H SCHILLING
</td>
<td style="text-align:left;">
5.5
</td>
<td style="text-align:left;">
W 23
</td>
<td style="text-align:left;">
D 28
</td>
<td style="text-align:left;">
W 2
</td>
<td style="text-align:left;">
W 26
</td>
<td style="text-align:left;">
D 5
</td>
<td style="text-align:left;">
W 19
</td>
<td style="text-align:left;">
D 1
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1716
</td>
</tr>
<tr>
<td style="text-align:left;">
11
</td>
<td style="text-align:left;">
5
</td>
<td style="text-align:left;">
HANSHI ZUO
</td>
<td style="text-align:left;">
5.5
</td>
<td style="text-align:left;">
W 45
</td>
<td style="text-align:left;">
W 37
</td>
<td style="text-align:left;">
D 12
</td>
<td style="text-align:left;">
D 13
</td>
<td style="text-align:left;">
D 4
</td>
<td style="text-align:left;">
W 14
</td>
<td style="text-align:left;">
W 17
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1655
</td>
</tr>
<tr>
<td style="text-align:left;">
13
</td>
<td style="text-align:left;">
6
</td>
<td style="text-align:left;">
HANSEN SONG
</td>
<td style="text-align:left;">
5.0
</td>
<td style="text-align:left;">
W 34
</td>
<td style="text-align:left;">
D 29
</td>
<td style="text-align:left;">
L 11
</td>
<td style="text-align:left;">
W 35
</td>
<td style="text-align:left;">
D 10
</td>
<td style="text-align:left;">
W 27
</td>
<td style="text-align:left;">
W 21
</td>
<td style="text-align:left;">
OH
</td>
<td style="text-align:left;">
1686
</td>
</tr>
</tbody>
</table>

# Calculations

Now to get to the average of each players opponents, I needed to find
the opponents for each person. This is specified in rounds 1 through 7.
Currently the data has unnecessary values B,H, and U, the extra letters
before the opponent. I need to do a little bit more data wrangling in
order to get the opponents pre-ratings and each opponent.

## Find the average of the Opponents

``` r
player_info <- tm3 %>% select(c(player_num, round1:round7,pre_rating,state, `total points`)) %>% melt(id.var=c("player_num","pre_rating","state", "total points"),value.name = "round") %>% mutate(opponent = as.numeric(str_extract(round," .*"))) %>% select(c(-round)) %>% mutate(pre_rating= as.numeric(pre_rating)) 
head(player_info)
```

    ##   player_num pre_rating  state total points variable opponent
    ## 1         1        1794    ON         6.0     round1       39
    ## 2         2        1553    MI         6.0     round1       63
    ## 3         3        1384    MI         6.0     round1        8
    ## 4         4        1716    MI         5.5     round1       23
    ## 5         5        1655    MI         5.5     round1       45
    ## 6         6        1686    OH         5.0     round1       34

### Opponent table

I will need to know each players opponents pre-rating as well so this
will be its own table with the columns renamed to avoid ambiguous error.

``` r
opponent_scores <- tm3 %>% select(player_num, pre_rating) %>% mutate(player_num= as.numeric(player_num)) %>% mutate(pre_rating = as.numeric(pre_rating))
names(opponent_scores)[names(opponent_scores)=="player_num"] = "opponent2"
names(opponent_scores)[names(opponent_scores)=="pre_rating"] = "o_ratings"
```

<table class="table table-striped table-hover" style="margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
opponent2
</th>
<th style="text-align:right;">
o_ratings
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
3
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1794
</td>
</tr>
<tr>
<td style="text-align:left;">
5
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1553
</td>
</tr>
<tr>
<td style="text-align:left;">
7
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1384
</td>
</tr>
<tr>
<td style="text-align:left;">
9
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1716
</td>
</tr>
<tr>
<td style="text-align:left;">
11
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
1655
</td>
</tr>
<tr>
<td style="text-align:left;">
13
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
1686
</td>
</tr>
</tbody>
</table>

Now join the two data frames using sql, by selecting and grouping by the
player num and averaging the opponent ratings.

### Opponent Average

To get the opponent average, I have to select the player num from
player_info table group by it and average the opponent ratings for each
respective player. I achieved this using SQL.

``` r
opponent_avg <- sqldf('select player_num, round(avg(o_ratings),0 ) as opponent from player_info inner join opponent_scores on opponent = opponent2 group by player_num')
```

<table class="table table-striped table-hover" style="margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
player_num
</th>
<th style="text-align:right;">
opponent
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
1
</td>
<td style="text-align:right;">
1605
</td>
</tr>
<tr>
<td style="text-align:left;">
2
</td>
<td style="text-align:right;">
1469
</td>
</tr>
<tr>
<td style="text-align:left;">
3
</td>
<td style="text-align:right;">
1564
</td>
</tr>
<tr>
<td style="text-align:left;">
4
</td>
<td style="text-align:right;">
1574
</td>
</tr>
<tr>
<td style="text-align:left;">
5
</td>
<td style="text-align:right;">
1501
</td>
</tr>
<tr>
<td style="text-align:left;">
6
</td>
<td style="text-align:right;">
1519
</td>
</tr>
</tbody>
</table>

Now we need to combine our final data frame with all the information so
it can look like our final tournament table.

``` r
tournament <- tm3 %>% full_join(opponent_avg, by=c("player_num")) %>% select(c(-(round1:round7)))
```

<table class="table table-striped table-hover" style="margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
player_num
</th>
<th style="text-align:left;">
player_name
</th>
<th style="text-align:left;">
total points
</th>
<th style="text-align:left;">
state
</th>
<th style="text-align:left;">
pre_rating
</th>
<th style="text-align:right;">
opponent
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
1
</td>
<td style="text-align:left;">
GARY HUA
</td>
<td style="text-align:left;">
6.0
</td>
<td style="text-align:left;">
ON
</td>
<td style="text-align:left;">
1794
</td>
<td style="text-align:right;">
1605
</td>
</tr>
<tr>
<td style="text-align:left;">
2
</td>
<td style="text-align:left;">
DAKSHESH DARURI
</td>
<td style="text-align:left;">
6.0
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1553
</td>
<td style="text-align:right;">
1469
</td>
</tr>
<tr>
<td style="text-align:left;">
3
</td>
<td style="text-align:left;">
ADITYA BAJAJ
</td>
<td style="text-align:left;">
6.0
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1384
</td>
<td style="text-align:right;">
1564
</td>
</tr>
<tr>
<td style="text-align:left;">
4
</td>
<td style="text-align:left;">
PATRICK H SCHILLING
</td>
<td style="text-align:left;">
5.5
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1716
</td>
<td style="text-align:right;">
1574
</td>
</tr>
<tr>
<td style="text-align:left;">
5
</td>
<td style="text-align:left;">
HANSHI ZUO
</td>
<td style="text-align:left;">
5.5
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
1655
</td>
<td style="text-align:right;">
1501
</td>
</tr>
<tr>
<td style="text-align:left;">
6
</td>
<td style="text-align:left;">
HANSEN SONG
</td>
<td style="text-align:left;">
5.0
</td>
<td style="text-align:left;">
OH
</td>
<td style="text-align:left;">
1686
</td>
<td style="text-align:right;">
1519
</td>
</tr>
</tbody>
</table>

# Final Step

The final step is to save to a tournament csv which would be the correct
schema.

``` r
write_csv(tournament, "./tournament.csv")
```

The resulting csv can be found on my GitHub:
<a href="https://github.com/moiyajosephs/Data607-Project1/blob/main/tournament.csv">
Tournament</a>
