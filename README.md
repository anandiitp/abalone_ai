# Abalone-AI


## Abstract
Abalone-AI is an intelligent game-playing agent for the strategic board game [Abalone](https://en.wikipedia.org/wiki/Abalone_(board_game)). I discuss the implementation of the game-playing agent using several artificial intelligence algorithms.


## Artificial intelligence algorithms
In total, I have implemented six adversarial search algorithms that are commonly used in zero-sum games like Abalone.

**1.** Minimax  
**2.** Alpha-Beta Pruning  
**3.** Alpha-Beta Pruning with Transposition Table Optimization  
**4.** Principle Variation Search  
**5.** Principle Variation Search with Transposition Table Optimization  
**6.** Monte-Carlo Tree Search  

### State representation
Unlike many board games, Abalone uses a hexagonal grid which is not feasible to represent with a Cartesian or matrix coordinate system as done with square boards.

The coordinates are read in terms of the diagonal columns (x), and the horizontal rows (z). The columns increase diagonally, while the rows increase vertically. The coordinates of a single marble are expressed as a named tuple with two entries:

> Hex(x=−3, z=1)

When moving, a tuple with the same format is added to the coordinate which will shift the coordinate in that direction:

> Hex(−3, 1) + Direction (1,0) = Hex(−2,1)  
> *Single marble moving in direction (1,0)*

Similarly, groups of marbles (those which are adjacent to each other and can be moved together) are represented the same way, except as a list:

> True: [Hex(0,0),Hex(0,1),Hex(0,2)], where True = white piece  
> False: [Hex(1,0),Hex(1,1),Hex(1,2)], where False = black piece

**Finally, for an entire state representation of the board, we add each group of marbles to a hash table:**

> {  
> True: [Hex(0,0),Hex(0,1),Hex(0,2)],  
> False: [Hex(1,0),Hex(1,1),Hex(1,2)]  
> }  

### Heuristics and evaluation function
The heuristics used by some of the algorithms are outlined below:

#### Center proximity (h<sub>1</sub>)
Abalone requires very defensive moves. Being in the center of the board is the best place to be because it forces the opponent towards the edges, which is the most vulnerable spot in the board. The distance between each marble to the center of the grid is calculated for both players, then their difference is taken (maximizer favours low h<sub>1</sub>).

#### Cohesion (h<sub>2</sub>)
Marbles that are closer together form a population, which is the set of adjacent marbles. The more cohesion means the less populations. It is ideal for a player to have the minimal amount of populations and keep their marbles together. Cohesion is measured by the distance between all marbles (maximizer favours low (h<sub>2</sub>).

#### Marbles on board (h<sub>3</sub>)
The last heuristic is given by the difference in the amount of marbles on the board. This is an offensive heuristic rather than a defensive heuristic like the previous ones. It is given by the difference of marbles between the two players (maximizer favours high h<sub>3</sub>).

The evaluation function, then, is the sum of the scores given by all the heuristics: *eval = h<sub>1</sub> + h<sub>2</sub> + h<sub>3</sub>*  



## Results
Overall, the Principle Variation Search with the Transposition Table Optimization performed best, while Minimax performed worst. Monte-Carlo Tree Search was not testable on my machine.

# Running the code
## Prerequisites
Python 3.8*

## Generating a state file
Open `dump.py` and fill out the game state template and run the program using the Python interpreter. The program will output a JSON file which can be loaded in the main program and used to either generate the next best move, or simulate a game starting from that state.

## Configuring the game agent
1. Open `abalone/config.py` and set the initial starting board by filling in values of your choosing (default is `mini` for a miniaturized version of the game for demo purposes)
2. The grid size can be changed using `GRID_RADIUS`
3. The number of marbles required to win can be changed using `GAME_OVER`
4. The maximum number of attacking marbles in a row can be changed using `GROUP_LENGTHS` (default is `3`)
5. The states of the players can be changed as well using `BLACK` and `WHITE` (default: `BLACK = False`, `WHITE = True`)

## Running the game agent
1. Install dependencies: `python setup.py develop`
2. Run: `start.py` and follow the command-line interface to either load a JSON state (see Generating a state file) or simulate a game between two AI computers in a game of Abalone
