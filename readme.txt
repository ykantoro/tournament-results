{\rtf1\ansi\ansicpg1252\cocoartf1265\cocoasubrtf210
{\fonttbl\f0\fswiss\fcharset0 Helvetica;\f1\fnil\fcharset0 Menlo-Regular;}
{\colortbl;\red255\green255\blue255;\red38\green38\blue38;\red52\green110\blue183;\red103\green103\blue103;
}
{\*\listtable{\list\listtemplateid1\listhybrid{\listlevel\levelnfc23\levelnfcn23\leveljc0\leveljcn0\levelfollow0\levelstartat1\levelspace360\levelindent0{\*\levelmarker \{disc\}}{\leveltext\leveltemplateid1\'01\uc0\u8226 ;}{\levelnumbers;}\fi-360\li720\lin720 }{\listname ;}\listid1}
{\list\listtemplateid2\listhybrid{\listlevel\levelnfc0\levelnfcn0\leveljc0\leveljcn0\levelfollow0\levelstartat1\levelspace360\levelindent0{\*\levelmarker \{decimal\}.}{\leveltext\leveltemplateid101\'02\'00.;}{\levelnumbers\'01;}\fi-360\li720\lin720 }{\listname ;}\listid2}
{\list\listtemplateid3\listhybrid{\listlevel\levelnfc0\levelnfcn0\leveljc0\leveljcn0\levelfollow0\levelstartat1\levelspace360\levelindent0{\*\levelmarker \{decimal\}.}{\leveltext\leveltemplateid201\'02\'00.;}{\levelnumbers\'01;}\fi-360\li720\lin720 }{\listname ;}\listid3}
{\list\listtemplateid4\listhybrid{\listlevel\levelnfc0\levelnfcn0\leveljc0\leveljcn0\levelfollow0\levelstartat1\levelspace360\levelindent0{\*\levelmarker \{decimal\}.}{\leveltext\leveltemplateid301\'02\'00.;}{\levelnumbers\'01;}\fi-360\li720\lin720 }{\listname ;}\listid4}}
{\*\listoverridetable{\listoverride\listid1\listoverridecount0\ls1}{\listoverride\listid2\listoverridecount0\ls2}{\listoverride\listid3\listoverridecount0\ls3}{\listoverride\listid4\listoverridecount0\ls4}}
\margl1440\margr1440\vieww17840\viewh13660\viewkind0
\deftab720
\pard\pardeftab720

\f0\b\fs24 \cf2 Overview
\b0 \
In this project contains a Python module that uses the PostgreSQL database to keep track of players and matches in a Swiss system pairing game tournament.\
In the Swiss pairing game, players are not eliminated, and each player should be paired with another player with the same number of wins, or as close as possible.\
In addition to the basic requirements, I implemented an additional feature:\
\pard\tx220\tx720\pardeftab720\li720\fi-720
\ls1\ilvl0\cf2 {\listtext	\'95	}Support games where a tied game is possible.\
\pard\pardeftab720\sl540
\cf3 \
\pard\pardeftab720

\b \cf2 Usage
\b0 \
\pard\tx220\tx720\pardeftab720\li720\fi-720
\ls2\ilvl0\cf2 {\listtext	1.	}Install {\field{\*\fldinst{HYPERLINK "http://vagrantup.com/"}}{\fldrslt \cf3 Vagrant}} and {\field{\*\fldinst{HYPERLINK "https://www.virtualbox.org/"}}{\fldrslt \cf3 VirtualBox}}\
{\listtext	2.	}Download the source code and unzip it\
{\listtext	3.	}Launch the Vagrant VM and log in\
\pard\pardeftab720
\cf2 	
\f1\fs22 \cf4 $ vagrant up\
	$ vagrant ssh\
	$ cd user/vagrant/fullstack/tournament/
\f0\fs24 \cf2 \
\pard\tx220\tx720\pardeftab720\li720\fi-720
\ls3\ilvl0\cf2 {\listtext	4.	}Initialize database\
\pard\pardeftab720
\cf2 	
\f1\fs22 \cf4 $ psql tournament\
	=> \\i tournament.sql\
	=> \\q
\f0\fs24 \cf2 \
\pard\tx220\tx720\pardeftab720\li720\fi-720
\ls4\ilvl0\cf2 {\listtext	5.	}Run test script\
\pard\pardeftab720
\cf2 	
\f1\fs22 \cf4 $ python tournament_test.py
\fs24 \
\pard\pardeftab720\sl380

\f0 \cf2 \
\pard\pardeftab720
\cf2 \

\b File Descriptions
\b0 \
\pard\pardeftab720
\cf2 \ul \ulc2 Tournament.py\ulnone \
#def connect()\
Function used to connect to the PostreSQL database, returns a database connection.\
\
#def deleteMatches()\
Function used for removal of all match records from the 
\i game
\i0  table.\
\
#def deletePlayers()\
Function used for removal of registered players from the 
\i players
\i0  table.\
\
#def countPlayers()\
Function used for counting all players registered in 
\i players
\i0  table. Returns integer result.\
\
#def registerPlayer()\
Function used for adding players to the 
\i players
\i0  table, by inserting a players name. A serial id will be auto-generated when player name is registered in the 
\i players
\i0  table.\
\
#def playerStandings()\
Function queries players standings from 
\i players_standings
\i0  view. Results are then parsed into tuple format (int, str, float, int) and returned. Float is used instead of integer because a match can result in a tie.\
\
#def reportMatch()\
Function takes in 3 parameters, which are passed in once a match has completed. A winner player ID, a loser player ID (optional), and a winner2 player ID (optional). A winner parameter must always be passed in. If the match does not result in a tie, only a winner and a loser parameter will be passed. If the match results in a tie a winner and winner2 parameter will be passed in, loser parameter will default to 0. Depending on the results, player IDs are inserted into the winner column, loser column or winner2 column in the 
\i game
\i0  table. These proper insertions are then used to score the game.\
\
#def swissPairings()\
Query is fetched to get current player standings (from highest to lowest score). The query is then parsed and returned in a tuple format, each which contains (id1, name1, id2, name2).\
\
\
\ul Tournament.sql\ulnone \
#Table Players\
Holds information on the registered players, such as id and name.\
\
#Table game\
Holds information on the matches played, including winners, losers, ties, as well as player ids.\
\
#View player_count\
This view is used to use count the number of players registered.\
\
#View num_matches\
This view is used to determine the number of matches played by each player (id and name).\
\
#View scores\
This view adds scores to each player depending on the outcome of their matches. 1 point is given if the match has a single winner, 0 points are given if the player lost the match. Half (0.5) points are given if the match ends in a tie.\
\
#View num_wins\
This view sums up the score for each player for every match and orders the players by highest to lowest score.\
\
#View player_standings\
This view joins view 
\i num_wins
\i0  and view 
\i num_matches
\i0  to return a snapshot of the current players names and ids, their current score and how many matches they have played.\
}