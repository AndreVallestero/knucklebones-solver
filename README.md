# knucklebones-solver
Knucklebones Solver using minimax and median-weighting for random probabilities

Check it out here: https://andrevallestero.github.io/knucklebones-solver/

Test it in game here: https://knucklebones.jaredp.co.uk/

## TODO
- optimize
  - alpha-beta pruning
  - profiling and optimizations
  - rewrite in rust -> wasm
  - tail call recursion optimization
  - manually rewrite recursion as a loop
  - configurable multi-threading
    - choose which depth (x) to start multithreading
    - at depth - 0, disable multithreading
    - at depth - 1, use 3 threads
    - at depth - 2, use 54 threads (3x6x3), etc...
    - alpha-beta pruning should start after splitting off into its own threads as it requires thread locality, as such, threads should not exceeded number of cores on computer.
  - profiling and micro optimizations
- implement auto-place (button to automatically place the suggested position)
  - this should also remove dice from the opponent's board if necessary
- implement auto-format (top-align the dice / remove blank cells and move dice to the top)
- add reset button
