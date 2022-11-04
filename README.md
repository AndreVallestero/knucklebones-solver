# knucklebones-solver
Knucklebones Solver using minimax and median-weighting for random probabilities

Check it out here: https://andrevallestero.github.io/knucklebones-solver/

Test it in game here: https://knucklebones.jaredp.co.uk/

## TODO
- optimize
  - [x] ~~{40x speedup} alpha beta pruning~~ (not possible due to roll median algorithm)
  - [x] (200x speedup) profiling and optimizations 
  - [ ] use flat array for grids
  - [ ] web worker / multithreading (split 3 top branches into its own webworker)
  - [ ] rewrite in rust -> wasm
  - [ ] tail call recursion optimization
  - [ ] manually rewrite recursion as a loop
  - [ ] profiling and micro optimizations
- implement auto-place (button to automatically place the suggested position)
  - this should also remove dice from the opponent's board if necessary
- add reset button
