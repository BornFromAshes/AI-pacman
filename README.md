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
The first level search algorithm is implemented in the breadthFirstSearch function in the search.py file.
```
python pacman.py -l mediumMaze -p SearchAgent -a fn=bfs
```
```
python pacman.py -l bigMaze -p SearchAgent -a fn=bfs -z .5
```
If Pacman is moving slowly use this option:
```
--frameTime 0
```
This algorithm also works well for EightPuzzle problem:
```
python eightpuzzle.py
```
## Changing the cost function
While BFS finds the path with the fewest actions required to reach the goal, we may want to find paths that are best in other ways. Consider two mazes mediumDottedMaze and mediumScaryMaze. <br>
By changing the cost function, we can encourage Pacman to find different paths. For example, we can consider a higher cost for moving in dangerous areas containing ghosts or a lower cost for moving in areas where food is abundant, and a rational Pacman agent should adjust its behavior according to these costs. The UCS graph search algorithm is implemented in the uniformCostSearch function in the py,search file. <br>
You should now be able to see successful agent behaviors in the three maps below. Agents in all cases are UCS agents that differ only in the cost function they use.
```
python pacman.py -l mediumMaze -p SearchAgent -a fn=ucs
```
```
python pacman.py -l mediumDottedMaze -p StayEastSearchAgent
```
```
python pacman.py -l mediumScaryMaze -p StayWestSearchAgent
```
## A* search
In the file search.py and in the empty function aStarSearch, a graphical search A * is implemented. A, * takes a heuristic function as an input argument. Heuristic functions have two input arguments:
1. Current state in the search problem
2. The search problem itself. The nullHeuristic function located in the py.search file is an obvious prototype for the heuristic function.
<br> 

Test the implemented algorithm A* on the problem of finding the path inside the maze to a specific point, with the help of Manhattan distance heuristic. (This heuristic is implemented in a function called manhattanHeuristic in the py.searchAgents file.) For this purpose, you can run the code with the help of the following command:

```
python pacman.py -l bigMaze -z .5 -p SearchAgent -a fn=astar,heuristic=manhattanHeuristic
```
## Finding all corners
The true power of A* algorithm is only revealed by more challenging search problems. Now we want to formulate a new problem and design a serious heuristic for it.
There are four dots in the corners of the maze, each in one corner. Our new search problem is to find the shortest path in the maze so that the path taken passes through all four corners of the maze (regardless of whether there is food in one corner or not). Note that for some Mazes like tinyCorners, the shortest doesn't always go first to the nearest food. <br>
Now our smart agent can solve the following two problems:
```
python pacman.py -l tinyCorners -p SearchAgent -a fn=bfs,prob=CornersProblem
```
```
python pacman.py -l mediumCorners -p SearchAgent -a fn=bfs,prob=CornersProblem
```
## Heuristics for the corners problem
An adaptive non-trivial heuristic for the CornersProblem is implemented in the cornersHeuristic function. The code can solve the following problem:
```
python pacman.py -l mediumCorners -p AStarCornersAgent -z 0.5
```
### Admissibility and compatibility
Heuristics are functions that take a search condition as input and return a number as output. The output number shows the estimated cost to the nearest target node. More useful heuristics return a value closer to the actual cost to the goal. For a heuristic to be acceptable, the value of the heuristic must be less than the actual cost of the shortest path to the closest goal (and be non-negative). For a heuristic to be consistent In addition to being acceptable, if an action has a cost of c, performing that action only reduces the value of the heuristic to the maximum value of c.
### Non-trivial heuristics
Obvious heuristics of cases that are zero everywhere (UCS) or heuristics that cost
They calculate the actual completion. The former won't save you any time, and the latter will cause the autograder to time out. We need a heuristic that reduces the total calculation time.
## Eating all the points
In this section, we are going to solve a hard search problem: eat all Pacman foods in the least number of steps possible. So we need to define a new search problem that formulates the food cleaning problem, for this purpose the FoodSearchProblem class is implemented in the searchAgents.py file. An acceptable answer is a path
to collect all the food items in the Pacman world. For the current project, the solutions do not consider any souls or power pellets. The answers depend only on the location of the walls, food and pacman.(Of course, ghosts can ruin the implementation of a solution! We will get to it in the next project.)
```
python pacman.py -l testSearch -p AStarFoodSearchAgent
```
## Suboptimal search
Sometimes, even with the help of A* algorithm and a suitable heuristic, it becomes difficult to find the optimal path among all the points. In these cases, we still want to find a good way quickly. In this section, you write an agent that always greedily eats the closest dot. For this purpose, the ClosestDotSearchAgent class is implemented in the searchAgents.py file.
```
python pacman.py -l bigSearch -p ClosestDotSearchAgent -z .5
```
## Known Issues
There aren't currently any issues so far so if you find any please create an issue on this repository. Any suggestions for implementation would also be greatly appreciated.












