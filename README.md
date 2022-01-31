# NBA - First Minutes slump
Repo for an NBA Data project which tests the hypothesis of whether teams underperform offensively during the first 2 minutes of each quarter.

In this project I:
- Extracted Play-by-Play data from the NBA API
- Cleaned the data and performed some feature engineering
- Plotted results to check for patterns
- Tested multiple statistical hypotheses
- Applied What-If analysis to see how much the NBA standings would be different

# Initial Hypothesis
Growing up in Europe and watching NBA games has many ups and downs. On the one hand, you can easily compare the game style of European and American teams. On the other hand, your sleeping schedule gets really messed up.

Something that has always bothered me about the NBA play style is the lack of energy or focus since the jumpball or players taking some possessions off. It's contradicting to the European game style and to what I was taught growing up around the sport.

I noticed that during the first 2-3 minutes of each quarter, players seem more "relaxed" compared to the rest of the quarter and I wanted to check if my hypothesis is True or not.

## First, we're going to check whether there is any pattern in scoring in each minute
For this, we're going to create a metric to count points per minute for each quarter and calculate the average points per minute (ppm) for each minute.
For our analysis we are going to use NBA play-by-play data from the 2021-2022 season up to now (January 31st 2022).

Before doing that we want to clean the dataset:
- We separate the minutes from the clock columns
- We want to take out overtime periods
- We calculate how many minutes are left in the game for each row
- We add a total score column

After collecting our Points per Minute (ppm) data for each game of this season we calculate the average for each minute and we plot them:
 
![alt text](https://github.com/MarlinMyrte/First-Minutes/blob/main/ppm_avg.png "Average Combined Points per Minute")
 
*Note that the data refer to the combined point production for both teams in each game

### We can see that there is indeed a drop in the combined point production during the first minute of each quarter, however it bounces back fairly quickly.
- This could be attributed to teams taking longer possessions in the first minutes of each quarter and getting back to rhythm slowly.
- Another aspect that could potentially play a role and we are going to examine later is the pace of the game and the number of possessions.
### Another interesting fact that can be deducted from this graph is that during the very end of each quarter the combined point production skyrockets.
- A possible explanation would be the fact that teams try to take advantage of "2-for-1" opportunities during the last minute of the quarter.

## The next thing we'd like to check is whether the average point production per minute during the first 2 minutes of a quarter is different from the rest of the quarter.

After collecting our data we perform a T-test for the means of the two time periods to check are significantly different.

- Ho: μ1=μ2, There is not a significant difference
- H1: μ1<>μ2, There is a significant difference
 
- First 2 minutes of a quarter mean value: 3.93
- Last 10 minutes of a quarter mean value: 4.62
- First 2 minutes of a quarter std value: 1.53
- Last 10 minutes of a quarter std value: 0.79
- p-value: 3.647500905905782e-101

#### After performing the t-test we reject the null hypothesis at the 5% significance level and we conclude that there is a difference between the offensive performance during the first 2 minutes of each quarter.

### But why does this happen? Is it affected by the pace of the game? Let's check the possessions per minute

![alt text](https://github.com/MarlinMyrte/First-Minutes/blob/main/poss_pm_avg.png "Average Combined Possessions per Minute")

### After plotting the possessions per minute we observe a similar behaviour as the points per minute, spiking at the end of each quarter and very low at the start of them
### Let's check what happens with the average points per possession per minute

![alt text](https://github.com/MarlinMyrte/First-Minutes/blob/main/pppm_avg.png "Average Combined Points per Possession per Minute")

#### Here we get the opposite results. Teams seem more efficient at the start of each quarter and less efficient at the end of it as they probably hurry to get up a low percentage shot at the last seconds of the quarter


## Now, let's examine whether the offensive production (as measured by points per minute) in the first 2 minutes changes between the two halves of the game.
### We want to check if there is a difference between ppm in the first 2 minutes of the first half quarters (Q1 & Q2) and the first 2 minutes of the second half quarters (Q3 & Q4)

Once again, we perform a T-test for the means of the two time periods to check are significantly different.

- Ho: μ1=μ2, There is not a significant difference
- H1: μ1<>μ2, There is a significant difference

- First 2 minutes of the first half quarters mean value: 3.93
- First 2 minutes of the second half quarters mean value: 3.95
- First 2 minutes of the first half quarters std value: 1.53
- First 2 minutes of the second half quarters std value: 0.79
- p-value 0.6509362303181166

After performing a hypothesis test on the two time periods (start of quarter vs the rest) it was confirmed that the ppm during the first 2 minutes of each quarter is significantly different between the ppm during the last 10 minutes of each quarter.

### Although there is a difference in the standard deviation between the ppm scored in the first 2 minutes of the first 2 quarters and the ppm scored in the first 2 minutes of the last 2 quarters, the mean is quite similar and according to the t-test we can't reject the null hypothesis

## What about the point production between the two halves? Which half produces more points and is their difference significant?

- Mean points scored in the first half: 109.56
- Mean points scored in the second half: 106.93

### On average this season, teams have performed better offensively in the first half.

And now for the test
- Ho: μ1=μ2, There is not a significant difference
- H1: μ1<>μ2, There is a significant difference

- p-value 6.970259668915989e-05

### So after performing the t-test we reject the null hypothesis at the 5% significance level and we conclude that there is a difference between the offensive performance between the 2 halves

#### This could be attributed to factors such as:
- the defense being more sluggish in the first half
- strategic changes and changes in the defensive scheme as the game progresses
- higher sense of urgency at the last moments of the game might lead to tighter defense, especially when the score is closer
- lower pace in the second half, which we will examine next

### Let's examine the pace of the game

- Mean possessions in the first half: 99.63
- Mean possessions in the second half: 97.28

### On average, the first half of a games has more possessions than the second one and thus a faster pace

We can also check the hypothesis
- Ho: μ1=μ2, There is not a significant difference
- H1: μ1<>μ2, There is a significant difference

- p-value 2.5427765045058275e-16

### So after performing the t-test we reject the null hypothesis at the 5% significance level and we conclude that there is a difference between the number of possessions between the 2 halves
This means that slower pace could be a potential reason for less offensive productivity in the second half

## A fair question that can be raised here is whether this "2-minute slump" affects the outcome of the game and how different would the team standings be if all teams didn't have this slump.

To answer this question we need to:
- Obtain Average Points per Minute data for all the NBA teams
- Compare the offensive performance of each team in the first 2 minutes vs in the last 10 minutes of each quarter
- Predict how many more points a team would score if the played the same way during all the minutes of the quarter by adding the difference of the means to the total score of each game
- Recalculate the score of each game
- Recalculate the standings

After getting the Average Points per Minute for each team we can see that the most "stable" team so far this year has been the Phoenix Suns. On the other hand, the OKC Thunder have the largest difference in terms of average ppm between the first 2 minutes of a quarter and the last 10

![alt text](https://github.com/MarlinMyrte/First-Minutes/blob/main/Top%20Bottom%20diff.png "Top-Bottom 3")


Now, by adding up the "lost points" of each minute, we can recalculate the results of each game and the standings
*We have to mention that by doing this recalculation of the scores some ties might occur which makes it impossible to determine the eventual winner of a game. That being said we can only hypothesize on what could have happened in these tied games*

**Eastern Conference**
![alt text](https://github.com/MarlinMyrte/First-Minutes/blob/main/East%20Standings.png "East")


**Western Conference**
![alt text](https://github.com/MarlinMyrte/First-Minutes/blob/main/West%20Standings.png "East")

### In the Eastern Conference, we can see that the 76ers jumped a spot from the 3rd place to the 2nd place, the Bulls have 3 ties which could bring them down to the 2nd spot in the East and Charlotte dropped a spot to Toronto and is now 7th
### In the Western Conference, the Los Angeles Clippers might drop 2 wins because of ties and that could bring them to the 9th place and to the play-in tournament while 2 potential wins in ties from the Kings could bring them to the 12th spot
