# knucklebones-solver
Knucklebones Solver using minimax and mean-weighting for random probabilities

Try it out at https://andrevallestero.github.io/knucklebones-solver/

## TODO
- optimize
  - alpha-beta pruning
  - profiling and micro optimizations
  - rewrite in rust -> wasm
  - profiling and micro optimizations
  - tail call recursion optimization
  - manually rewrite recursion as a loop
  - multi-threading (each branch after depth n-2 should be split into it's own thread allowing us to leverage up to 54 threads (3x6x3)
  - profiling and micro optimizations
- implement auto-place (button to automatically place the suggested position)
  - this should also remove dice from the opponent's board if necessary
- implement auto-format (top-align the dice / remove blank cells and move dice to the top)
- add reset button
