# AI-pacman
In this project, your Pacman agent finds his way to reach a certain place and collect food optimally in a spiral. We build general search algorithms and apply them to Pacman game scenarios. To debug and test the correctness of your algorithms, you can execute the following command and see its details:
```
python autograder.py
```
## Project structure
- search.py : The file where all our search algorithms are placed.
- seachAgents.py : The file where all the search agents are placed.
- pacman.py : The main file that runs Pacman games. This file describes the GameState class for the Pacman game that you use in this project.
- game.py : The implemented logic for the Pacman world is in this file. This file contains several classes such as AgentState, Agent, Grid, and Direction.
- util.py : Useful data structures for implementing search algorithms are located in this file.
- graphicsDisplay.py : Graphics implemented for Pacman game.
- graphicsUtils.py : Support for game graphics.
- textDisplay : ASCII graphics for Pacman.
- ghostAgent.py : Ghost controller.
- keyboardAgents.py : Keyboard interface to control Pacman.
- layout.py : Program to read map files and save their information.
- autograder.py : Project auto corrector.
- testParser.py : Parse autocorrect tests and solution files.
- testClasses.py : General automatic test classes.
- testCases/ :  Folder containing different tests for each question.
- searchTestClasses.py : Automated testing classes.

## Welcome to pacman!
You can run the Pacman game by typing the following commands:
```
python pacman.py
```
Pacman lives in a shiny blue world full of winding corridors and delicious food. Optimum movement in this world is Pacman's first step to success in this world.
The simplest agent in the py.searchAgents file is the agent named GoWestAgent, which always moves west (a simple reactive agent). This agent can sometimes win:
```
python pacman.py --layout testMaze --pacman GoWestAgent
```
But this factor does not work well when turning is needed:
```
python pacman.py --layout tinyMaze --pacman GoWestAgent
```
If Pacman gets stuck somewhere in the game, you can exit the game by entering c+Ctrl in your terminal. <br>
Soon your Pacman can solve not only tinyMaze but also any other maze we want. <br>
The py.pacman file also supports several options, each of which can be entered in long (-layout) or short (-l) form. To see a list of all options and their default values, you can enter the following command:
```
python pacman.py -h
```
Also, all the commands given in this project are placed in the command.txt file for ease of use. In Mac and Unix operating systems, you can execute all these commands at once by entering the following command:
```
bash commands.txt
```
## Finding a food fixed point using depth-first search
You can find the fully implemented SearchAgent class in the py.searchAgents file. This agent determines a path and walks it step by step.
First, make sure that SearchAgent is working properly by running the following command:
```
python pacman.py -l tinyMaze -p SearchAgent -a fn=tinyMazeSearch
```
The command above tells the search agent (SearchAgent) to use tinyMazeSearch, which is implemented in the search.py file, for the search algorithm. Pacman must be able to navigate this spiral successfully. <br>
Note that a search node has information about the state as well as the information necessary to reconstruct the path that reaches that state. <br>
Important note: All lookup functions must return a list of actions that take the agent from the start state to the goal. <br>
All these actions must be allowed movements (allowed directions, you must not cross the walls). <br>
After the implementations Pacman can quickly find the solution for the following situations: 
```
python pacman.py -l tinyMaze -p SearchAgent
```
```
python pacman.py -l mediumMaze -p SearchAgent
```
```
python pacman.py -l bigMaze -z .5 -p SearchAgent
```
The Pacman screen shows the states that have been explored and the order in which they were explored (the brighter the red, the earlier it was explored).
## Breadth-first search
























