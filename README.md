# Data607-Project1

# Overview

For this project we were given a text file with chess tournament results where the information has some structure. The task was to generate a .CSV file containing the name, pre-rating score of an individual player, their total points and the average of all their opponents pre-ratings. The tournament results are stored in the text file called tournamentinfo. 

Guidelines for this project

* The rounds that end with whats detailed below will not count:  
    + B(ye)  
    + H(alf point)  
    + U(nplayed)  

* Instead we will include:   
    + W(in)   
    + L(ose)   
    + D(raw)   

As of right now the data looks like:  
![tournament table](https://github.com/moiyajosephs/Data607-Project1/blob/main/tournament_sc%20(2).png)


The final format we want is:


| Player's Name        | Player's State     | Total # of points  | Player's Pre Rating | Avg Pre-Chess of Opponents |
|:--------------------:|:------------------:|:------------------:|:-------------------:|:--------------------------:|
| Gary Hua             | ON                 | 6.0                | 1794                | 1605                       | 

That will be output into a CSV
