# knucklebones-solver
Knucklebones Solver using minimax and median-weighting for random probabilities

Check it out here: https://andrevallestero.github.io/knucklebones-solver/

Test it in game here: https://knucklebones.jaredp.co.uk/

## TODO
- optimize
  - [x] (45421ms) baseline
  - [x] ~~{40x speedup} alpha beta pruning~~ (not possible due to roll median algorithm)
  - [x] (649ms, 70x speedup) profiling and optimizations 
  - [ ] use flat array for grids
  - [ ] web worker / multithreading (split 3 top branches into its own webworker)
  - [ ] rewrite in rust -> wasm
  - [ ] tail call recursion optimization
  - [ ] manually rewrite recursion as a loop
  - [ ] profiling and micro optimizations
- implement auto-place (button to automatically place the suggested position)
  - this should also remove dice from the opponent's board if necessary
- add reset button

## Reference Benchmark

![image](https://user-images.githubusercontent.com/39736205/199863284-35712a55-cf26-4e6b-b2d6-967e1b02b5c3.png)

## Cool Profiling Flamegraphs

Baseline:
![image](https://user-images.githubusercontent.com/39736205/199864435-0aa6fb2b-db7b-4377-9e4d-13558d8f2fd5.png)

After clone_grids optimization:
![image](https://user-images.githubusercontent.com/39736205/199863882-f3516b19-e088-4215-bb9e-696c9ba5fbe6.png)

After finishing first round of optimizations, note clone_grids is now taking up 40% of the runtime, if this were C, this would happen in a single memcpy and would likely be 1% of the runtime, hence why I wanted to try storing the grids in a flat array next, that way I could use a single array.slice() which maps to memcpy:
![image](https://user-images.githubusercontent.com/39736205/199864034-99848ba2-fd9b-4cbd-bf78-eda8b02043b6.png)

