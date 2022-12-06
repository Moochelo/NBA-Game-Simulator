# NBA-Game-Simulator
A simulation of two NBA teams competing against each other and the option for a user to predict which team will win given the matchup.
## Your name

Michael Mucciolo

***

## Year

Senior

***

## Your section leader's name

Michael Mucciolo

***

## Project title

NBA Game Simulator

***

## In just a sentence or two, summarize your project. 

A simulation of two NBA teams competing against each other and the option for a user to predict which team will win given the matchup.

***

## What have you done for your project?

I have created an NBA game simulator that allows the user to simulate a matchup of two NBA teams. The simulator takes the average points per game a team has scored and allowed within the 2021-2022 season and takes the standard deviation of points per game they score and allow into account as well. These factors will determine the likelihood that a team will win in a given matchup. If the user selects to run a simulation, an option is provided where the user can either select two teams themselves or have the random function select two teams for the matchup for them. Once two teams are selected, the user predicts which team will win, given the matchup. The simulator is set up so it will run until the user tells it they no longer wish to simulate another game. After the user wishes to stop simulating games, the accuracy the user guessed with is provided, providing the percentage of matchups guessed correctly per the simulation(s).

***

## What challenges did you encounter and how did you overcome them?

I encountered challenges for invalid inputs, so when the user was prompted to enter ("Yes" or "No") to run the simulation or ("1" or "2") to select a team, I had to ask my TA for assistance in returning ("invalid input, please try again"). The way that this challenge was overcome was by inserting while loops for each of these scenarios. 

For example the team selection process was fixed by using this loop: 

while run_simulation.lower() != 'yes' and run_simulation.lower() != 'no':
    run_simulation = input("Invalid input, please try again.")

***


## In a paragraph or more, detail your project. What will your software do? What features will it have? How will it be executed?

My project takes data from the 2021- 2022 NBA season (which ended April 10th, 2022) . My software uses the readfile function to read a csv file containing all thirty teams within the NBA and the statistics each team has recorded within the eighty-two games they have played throughout the season. Then a nested dictionary is initialized to record the wins and losses, team points, and  points a team allows. From the points and points allowed, the average points per game and points allowed per game for each team are calculated and run through the mean function and standard deviation function to determine how likely each team is to win a given matchup. From the calculated mean and standard deviation per team, the game simulator is created. This game simulator runs a matchup of two teams and allows for the user to either randomly select the two teams in a matchup or select the two teams themself. (This is done with providing the option of selecting 1 for manual selection or selecting 2 for random selection. The random module is used for the randomly generated teams.) Once two teams are selected, the user can predict which team will win, given the matchup (again this is done by inputting 1 or 2 for each team given). The simulator is set up so it will run until the user tells it they no longer wish to simulate another game. After the user wishes to stop simulating games, the accuracy the user guessed with is provided, providing the percentage of matchups guessed correctly per the simulation(s).


***

## What would you have liked to do but unfortunately were unable to accomplish for your project?

I would have liked to output histograms showing the average points per game each team would score and allow in each matchup, therefore providing a visual as to how each team is more/less likely to win a given matchup. Unfortunately, I was unable to accomplish this as I had no experience using histograms in python.

***

## Instructions for user for simulation

1. Input yes or no when prompted to start a game.
2. If yes, input 1 to select two teams yourself, input 2 to randomly select 2 teams
3. If 1, select two teams from the given list  by inputting the number that corresponds to each team (each team corresponds to the number directly to 
    the left of it)
    - After the two teams are selected, predict which team will win the given matchup by inputting 1 to select team 1, or inputting 2 to select team 2
4. If 2, Predict which team will win the given matchup by inputting 1 to select team 1, or inputting 2 to select team 2
5. The user may repeat this process as many times as they desire by simply inputting yes or no at the end of each simulation. Yes to run another 
    simulation, no to stop simulating.
6. When the user has finished simulating, the accuracy the user guessed with is calculated and given in the form of a percentage.


***

## Sources Used:

https://github.com/PlayingNumbers/NBASimulator/blob/master/NBAGameSimulator.py

https://github.com/antdke/Basketball-Simulator-in-Python/blob/master/Basketball3.py

***

## Code for NBA Game Simulator

#Step 1. Correctly use readfile Function and import data
#Step 2. Initialilze nested dictionary
#Step 3. Create mean function to determine average ppg per team
#Step 4. Create standard deviation function
#Step 5. Given mean and standard deviation function, create game simulator
#Step 6. Create option to allow user to run the simulation
#Step 7. Create option where user can select teams
#Step 8. Create option where user can have random function select 2 teams for them
#Step 9. Prompt user to choose select teams or have random function select for them
#Step 10. Calculate accuracy user guessed with

from csv import field_size_limit
from dataclasses import field
import random as rnd
import numpy as np 

#1. Function that reads the file and initializes nested dictionary 
def readFile(filename):
    TeamName = []
    points = []
    opp_points =[]
    data = open(filename, "r")
    next(data)
    teamStatisticsDict = {}
    for line in data.readlines():
        fields = line.split(",")
        TeamName = fields[0]
        points = fields[6]
        opp_points = fields[7]      
        #2. Initialize nested dictionary to record wins/loss ratio, team points, and opponents points
        teamStatisticsDict[TeamName] = {'wins':0,'losses':0,'points':[],'opp_points':[]}
        #Reset CSV file to beginning
    data.seek(0)
    #skip first line
    next(data)
    for line in data.readlines():
        fields = line.split(",")
        TeamName = fields[0]
        WinorLose = fields[5]
        points = fields[6]
        opp_points = fields[7]
        if WinorLose == 'W':
            teamStatisticsDict[TeamName]['wins'] = teamStatisticsDict[TeamName]['wins'] + 1
        else:
            teamStatisticsDict[TeamName]['losses'] = teamStatisticsDict[TeamName]['losses'] + 1

        #Convert points to integers for later Numpy use
        teamStatisticsDict[TeamName]['points'].append(int(points))
        teamStatisticsDict[TeamName]['opp_points'].append(int(opp_points))

    return teamStatisticsDict


#3. Returns average ppg per team
def average(pointslist):
    return np.mean(pointslist)

#4. Return standard deviation of ppg per team
def stdev(pointslist):
    return np.std(pointslist)

#5. Simulates a single game, returns team1 if team 1 wins, returns team2 if team 2 wins
#This function is based off of code I found here: <https://github.com/PlayingNumbers/NBASimulator/blob/master/NBAGameSimulator.py> written by <Ken Jee>
def gameSim(teamStatisticsDict, team1, team2):
    Team1PointsList = teamStatisticsDict[team1]['points']
    Team1OppPointsList = teamStatisticsDict[team1]['opp_points']
    Team2PointsList = teamStatisticsDict[team2]['points']
    Team2OppPointsList = teamStatisticsDict[team2]['opp_points']
    #Averages the random sample of a teams points with a random sample of the number of points the opponent allows
    #Randomly samples from the two gaussian distributions to produce a probabilistic outcome

    Team1PointAvg = average(Team1PointsList)
    Team1PointsStd = stdev(Team1PointsList)
    Team1OppPointsAvg = average(Team1OppPointsList)
    Team1OppPointsStd = stdev(Team1OppPointsList)


    Team2PointAvg = average(Team2PointsList)
    Team2PointsStd = stdev(Team2PointsList)
    Team2OppPointsAvg = average(Team2OppPointsList)
    Team2OppPointsStd = stdev(Team2OppPointsList)    
    
    Team1Prob = (rnd.gauss(Team1PointAvg,Team1PointsStd)+ rnd.gauss(Team2OppPointsAvg,Team2OppPointsStd))/2
    Team2Prob = (rnd.gauss(Team2PointAvg,Team2PointsStd)+ rnd.gauss(Team1OppPointsAvg,Team1OppPointsStd))/2
    if int(round(Team1Prob)) > int(round(Team2Prob)):
        return team1
    else:
        return team2



#Begin Code Execution Here
#The welcome/closing messages alongside some of the game simulator code are based off of code I found here: <https://github.com/antdke/Basketball-Simulator-in-Python/blob/master/Basketball3.py> written by <Anthony Diké>
print()
print("--------------------------------------------")
print("| HELLO, WELCOME TO THE NBA Game SIMULATOR |")
print("--------------------------------------------")
print()

teamStatisticsDict = readFile("2022_python_nba_season_data.csv")

#6. Allow user to run the simulation

listOfTeamNames = list(teamStatisticsDict.keys())

proceed = False
team1 = ''
team2 = ''
correctGuesses = 0
incorrectGuesses = 0
print("Would you like to start a game?")
print("input YES or NO")
run_simulation = input()

while run_simulation.lower() != 'yes' and run_simulation.lower() != 'no':
    run_simulation = input("Invalid input, please try again.")

while proceed == False:

    if run_simulation.lower() == "yes":
        
        #Ask if user wants to select teams or have random function select for them
        #9. Prompt user to choose select teams or have random function select for them
        print()
        randomOrNot = input("Input 1 to select two teams yourself, input 2 to randomly select 2 teams: ")
        while randomOrNot != '1' and randomOrNot != '2':
            randomOrNot = input("Invalid input, please try again.")
        #7. Create option where user can select teams 
        if(randomOrNot == '1'):
            print("Here are the teams you may select from:")
            print()
            val = 1
            for teamName in listOfTeamNames:
                print(str(val) + '. ' + teamName)
                val = val + 1
            print()

            team1Val = input('Enter Value For Desired Team 1: ')

            while(team1Val.isalpha()==True):
                print()
                print("Invalid input, please try again.")
                team1Val = input('Enter Value For Desired Team 1: ')

            while(int(team1Val)< 1 or int(team1Val)>30):
                print()
                print("Invalid input, please try again.")
                team1Val = input('Enter Value For Desired Team 1: ')
            
            team2Val = input('Enter A Different Value For Desired Team 2: ')

            while(team2Val.isalpha()==True):
                print()
                print("Invalid input, please try again.")
                team2Val = input('Enter Value For Desired Team 2: ')
            
            while(int(team2Val)<1 or int(team2Val)>30 or int(team1Val)==int(team2Val)):
                print()
                print("Invalid input, please try again.")
                team2Val = input('Enter Value For Desired Team 2: ')

            print(team1Val)
            print(team2Val)
        
            team1 = listOfTeamNames[int(team1Val) - 1]
            team2 = listOfTeamNames[int(team2Val) - 1]
            
        #8. Create option where user can have random function select 2 teams for them           
        else:
            
            team1 = rnd.choice(tuple(listOfTeamNames))
            team2 = rnd.choice(tuple(listOfTeamNames))


        print()
        print('Team 1: ' + team1)
        print('Team 2: ' + team2)
        print()

        print("Which team will likely win this matchup?")

        winningTeam = gameSim(teamStatisticsDict, team1, team2)

        print('Predict which team will win')
        userPrediction = (input("Input 1 to select " + team1 + ", input 2 to select " + team2 + ": "))

        while userPrediction != '1' and userPrediction != '2':
            print("Invalid input, please try again")
            userPrediction = (input("Input 1 to select " + team1 + ", input 2 to select " + team2 + ": "))

        if(userPrediction == '1'):
            userPrediction = team1
        elif(userPrediction == '2'):
            userPrediction = team2

        if(winningTeam == userPrediction):
            correctGuesses = correctGuesses + 1
            print("Correct")
        else:
            incorrectGuesses = incorrectGuesses + 1
            print("Incorrect")

        print('Winning Team: ' + winningTeam)

        print()
        print("Would you like to simulate another game?")
        print("input YES or NO")
        run_simulation = input()

        while run_simulation.lower() != 'yes' and run_simulation.lower() != 'no':
            run_simulation = input("Invalid input, please try again.")
        
        if run_simulation.lower() == "yes":
            proceed = False
        else:
            proceed = True

    else:
        proceed = True


if((correctGuesses != 0) or (incorrectGuesses != 0)):
    percentGuessedCorrectly = ((correctGuesses)/(correctGuesses + incorrectGuesses))*100

    #Step 10. Calculate accuracy user guessed with
    print()
    print('You guessed with ' + str(percentGuessedCorrectly) + '% accuracy')

print()
print("-------------------------------------------")
print("|       GOODBYE, THANKS FOR PLAYING! |")
print("-------------------------------------------")
print()
