# jsCheckersAI
Checkers game and AI engine written in Javascript that runs in a browser.

The AI engine, implemented 100% in Javascript, uses techniques from adversarial search to choose the best move against a human opponent.

# Game Play

## Start a game
Navigate to http://checkers-js-ai.s3-website-us-east-1.amazonaws.com/
Click "Start New Game"

## Moving Pieces
Drag a red circle to a target square.  If the move is a jump, and a double or triple jump is possible, the computer will automatically complete the jumps for the player.

If a piece makes it to the computer's back row, it is made into a king and marked with a "K".  The piece can now move in any direction.

## End game
Once all of the pieces of either red or black are captured, then the game is over and a winner is announced in the scoreboard.

## View replay of game
After a game is finished, the player can rewatch the entire game by clicking "View Replay".

# User Interface
The game board and all of the game interactions are implemented using d3js.  The board and pieces are svg elements that are bound to data structures representing the board cells and each player's pieces.

# AI Engine
The AI engine leverages two key concepts from adversarial search: Minimax and Alpha-Beta pruning.

## Minimax
Minimax is a strategy that can be used in games based on discrete, alternating moves.  Minimax searches for the move that will minimize the options of the opponent.  This is done by exploring a tree of moves with a predefined depth (if necessary).  The root of the search tree is the current position of the environment (game board).  Each branch off the root represents a possible move by the other player.  The minimax algorithm assumes each player will choose the move that maximizes its position.

The tree continues branching out, alternating between players at each level of the tree until there are no more possbile moves in the game, or until the pre-determined depth has been reached.  At the leaf nodes, the game state is evaluated through the use of a utility function.  The utility function returns a real value, indicating the strength of the board relative to the opponent.

Among a set of children nodes, the maximum score (calculated by the utility function) among them is passed up to their parent node.  This reflects the fact that the player would choose the move that maximizes its position.  Among all the parent nodes that are siblings, and who themselves share a parent, the minimum score is then passed up to the parent, reflecting the strategy that the computer will choose a move that minimizes the opponent's options.

This process continues all the way back up to the root of the tree.  At the root, Minimax can now see which of its possible moves should result in the lowest score for the opponent, based on the assumption that the opponent will play its "hand" perfectly.

In jsCheckersAI, the MiniMax algorithm is the heart of the AI engine.  The algorithm is implemented across 3 Javascript functions: min_value(), max_value, and utility().  Utility() uses various heuristics to calculate the value of the gameboard relative to the human player.  The function calculates three metrics: piece difference (# red pieces - # black pieces), king difference (# red kings = # black kings), and position difference (a metric which values pieces on the edge of the board more than interior pieces).  Weights are applied to each of these metrics and a final value is returned.

## Alpha-Beta Pruning
Minimax is all that is required to run the AI engine.  However, the downside of Minimax is that it has to search the entire tree of moves, which is exponential in depth.  Alpha-beta pruning is a technique that can significantly prune the search tree before performing Minimax.  The basic premise is that entire subtrees can be eliminated if it can be proved that the player would never consider moves that generate a subtree if a better move has already been discovered.  Alpha is a variable that keeps track of the best move for the player trying to maximize the board (red, in our case), and Beta is a variable that keeps track of the best move for the player trying to minimize the board (black, in our case).  The combination of algorithms are shown below from the Russell/Norvig book on AI:

![alt text](https://s3.amazonaws.com/checkers-js-ai/screenshots/MiniMax.png "Minimax Algorithm")

In jsCheckersAI, the alpha_beta_search() function is used to manipulate MiniMax and return the best move for the computer.  One interesting wrinkle that we add to our algorithm is how evaluate the leaf nodes.  If the search reaches the leaf nodes and there is a jump available from a leaf node position, than the search tree expands for that leaf node only.  This strategy was based on insights from the book [Blondie 24]: (https://www.amazon.com/Blondie24-Playing-Kaufmann-Artificial-Intelligence/dp/1558607838).


