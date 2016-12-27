# jsCheckersAI
Checkers game and AI engine written in Javascript that runs in a browser.

The AI engine, implemented 100% in Javascript, uses techniques from adversarial search to choose the best move against a human opponent.

# Game Play

## Start a game
Navigate to https://
Click "Start New Game"

## Moving Pieces
Drag a red circle to a target square.  If the move is a jump, and a double or triple jump is possible, the computer will automatically complete the jumps for the player.

If a piece makes it to the computer's back row, it is made into a king and marked with a "K".  The piece can now move in any direction.

## End game
Once all of the pieces of either red or black are captured, then the game is over and a winner is announced in the scoreboard.

## View replay of game
After a game is finished, the player can rewatch the entire game by clicking "View Replay".

# AI Engine
The AI engine leverages two key concepts from adversarial search: Minimax and Alpha-Beta pruning.

## Minimax
Minimax is a strategy that is used often in games based on discrete, alternating moves.  Minimax searches for the move that will minimize the options of the opponent.  This is done by exploring a tree of moves with a predefined depth (if necessary).  The root of the search tree is the current position of the environment (game board).  Each branch off the root represents a possible move by the player.  The minimax algorithm assumes the player will choose the move that maximizes its position.

The tree continues branching out, alternating between players at each level of the tree until there are no more possbile moves in the game or the pre-determined depth has been reached.  At the leaf nodes, the game state is evaluated  through the use of a utility function.  The utility function returns a real value, indicating the strength of the board relative to the human player.

Among a set of children nodes, the maximum score (calculated by the utility function) among them is passed up to their parent node.  This reflects the fact that the player would choose the move that maximizes its position.  Among all the parent nodes that are siblings, and who themselves share a parent, the minimum score is passed up to the parent, reflecting the strategy that the computer will choose a move that minimizes the player's options.

This process continues all the way back up to the root of the tree.  Minimax can now see which of its possible moves should result in the lowest score for the opponent, based on the assumption that the opponent will play its "hand" perfectly.


