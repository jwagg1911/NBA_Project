# **Context**


The dataset provided is from the 2022-2023 NBA regular season. This information contains all 672 NBA players as well as every major statistic for all the players. The goal is to perform a detailed analysis of this dataset and provide detailed information based on the players' stats per game in the regular season.We will break down the data to show the NBA's leaders in the major statistical categories. The major statistical categories include: points, rebounds, assists steals, and blocks. This information will allow us to determine which player averages the most points, rebounds, assists, blocks, and steals per game.


# **Data Description:**
The detailed data dictionary is provided below:

* Player - The name of the player
* Pos - The basketball position of the player
* Age - The age of the player
* Tm - The team the player competes for
* G - How many games the player played
* GS - How many games the player started
* MP - How many minutes the player averages
* FG - Average of how many field goals the player make per game
* FGA - Average of how many field goals the player attempts per game
* FG% - The percentage of the player's made field goals based off of how many field goals the player attempts
* 3P - Amount of three pointers made per game
* 3PA - Amount of three pointers attmpted per game
* 3P% - The percentage of three pointers made based off of three pointers attempted
* 2P - Amount of two pointers made per game
* 2PA - Amount of two pointers attempted per game
* 2P% - Percentage of two pointers made based off of two pointers attempted
* eFG% - Effective field goal percentage which measures a player's shooting success from the field including both 2-point FG and 3-point FG.
* FT - Amount of free-throws made per game
* FTA - Amount of free-throws attempted per game
* FT% - Percentage of free-throws made based off of free-throws attempted
* ORB - Amount of offensive rebounds per game
* DRB - AMount of defensive rebounds per game
* TRB - Total rebounds per game combining both offensive and defensive
* AST - Amount of assists per game
* STL - Amount of steals per game
* BLK - Amount of blocks per game
* TOV - Amount of turnovers per game
* PF - Amount of personal fouls per game
* PTS - Amount of points per game

#Data Cleansing

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

from google.colab import drive
drive.mount('/content/drive')

data = pd.read_csv('/content/drive/MyDrive/Python Course/nba_data.csv', encoding= 'latin-1')

**Viewing the first 25 rows**

data.head(26)

data.tail(25)

data.info()



*   There are only two data dtypes in the data which are **floats** and **objects**
*   There are 3 columns that are considered **floats** but should be **int**. Lets change that



#Here I am changing the data type to int64 to
#remove the decimals where they are not needed
data['Age'] = data['Age'].astype('Int64')
data['G'] = data['G'].astype('Int64')
data['GS'] = data['GS'].astype('Int64')

Run the data again to make sure that the data types have changed

data.info()

* I now have 26 floats, 3 objects, and 3 Int64 data types.
* The code ran successfully.

data.duplicated().sum()

data.drop_duplicates(inplace=True)

data.reset_index(drop=True,inplace=True)

We will check the data again to ensure that all duplicates were dropped.

data.duplicated().sum()

Some players are still showing up more than once in the data because they played for different teams that same season. This is ok considering the data is giving separate statistics for each team the player competed for.

data.isnull().sum()

As you can see we have missing values in all of the columns. We will drop those so there are no desrepancies in the data

#This will drop the missing values in the data
data.dropna(inplace=True)

Lets run the code again to make sure the missing values are all gone.

data.isnull().sum()

data.describe().T

* The average age for all of the NBA players in the 2022-2023 season is 26yrs old.
* The youngest NBA player is 19yrs old, while the oldest NBA player is 42yrs old.
* On average, 672 NBA players average about 9ppg on approximately 7FGA per game.

#NBA's Statistical Leaders

#This is a list of all players grouped by their respective teams.
team_players = data.groupby(['Tm', 'Player','Pos'])
team_players.first()

This dataset shows the top 10 players who average the most points

ppg = data.sort_values(by=['PTS'], ascending=False)
ppg.head(11)

This dataset shows the top 10 players who average the most assists

ast_leaders = data.sort_values(by=['AST'], ascending=False)
ast_leaders.head(11)

This dataset shows the top 10 players who average the most rebounds

reb_leaders = data.sort_values(by=['TRB'], ascending=False)
reb_leaders.head(11)

Here we will add a column to determine which player lead in a combined Points, Rebounds, and Assists

data['PRA'] = data['PTS'] + data['TRB'] + data['AST']
data.head()

#Now we will determine the top 10 players who averaged the most PRA per game
pra_leaders = data.sort_values(by=['PRA'], ascending=False)
pra_leaders.head(11)

#Visualizations

ppg = data.sort_values(by=['PTS'], ascending=False)
player = ppg['Player'].head(11)
points = ppg['PTS'].head(11)

fig, ax = plt.subplots(figsize =(16, 9))

ax.barh(player, points)

ax.set_title('Top 10 players who average the most points in the NBA',
             loc ='left', )

plt.show()

ast_leaders = data.sort_values(by=['AST'], ascending=False)
player = ast_leaders['Player'].head(11)
points = ast_leaders['AST'].head(11)

fig, ax = plt.subplots(figsize =(16, 9))

ax.barh(player, points)

ax.set_title('Top 10 players who average the most assists in the NBA',
             loc ='left', )

plt.show()

reb_leaders = data.sort_values(by=['TRB'], ascending=False)
player = reb_leaders['Player'].head(11)
points = reb_leaders['TRB'].head(11)

fig, ax = plt.subplots(figsize =(16, 9))

ax.barh(player, points)

ax.set_title('Top 10 players who average the most rebounds in the NBA',
             loc ='left', )

plt.show()
