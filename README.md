# knucklebones-solver
Knucklebones Solver using minimax and median-weighting for random probabilities

Check it out here: https://andrevallestero.github.io/knucklebones-solver/

Test it in game here: https://knucklebones.jaredp.co.uk/

## TODO
- optimize
  - [x] (45421ms) baseline
  - [x] ~~{40x speedup} alpha beta pruning~~ (not possible due to roll median algorithm)
  - [x] (659ms, 70x speedup) profiling and optimizations 
  - [x] (260ms, 2.5x speedup) use flat array for grids
  - [ ] use [Uint8Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays) which might let us leverage reduce for sum without needing a slice/memcpy
  - [ ] web worker / multithreading (split 3 top branches into its own webworker)
  - [ ] configurable web worker / multithreading (4+ threads)
  - [ ] rewrite in rust -> wasm
  - [ ] tail call recursion optimization
  - [ ] manually rewrite recursion as a loop
  - [ ] profiling and micro optimizations
    - [ ] optimize for branch predictor (test branchless)
    - [ ] optimize for memory locality / reduce cache misses
- implement auto-place (button to automatically place the suggested position)
  - this should also remove dice from the opponent's board if necessary
- add reset button

## Reference Benchmark

![image](https://user-images.githubusercontent.com/39736205/199863284-35712a55-cf26-4e6b-b2d6-967e1b02b5c3.png)

This config with depth=5 results in minimax being called 1,194,069 times

## Cool Profiling Flamegraphs

Baseline: Note, `structuredClone()` and `eval_column()` take up the majority of the runtime. `structuredClone()` was replaced with a custom `clone_grids()`, and eval_column was slow because of `.entries()` which I replaced with regular indexing.
![image](https://user-images.githubusercontent.com/39736205/199864435-0aa6fb2b-db7b-4377-9e4d-13558d8f2fd5.png)

After implementing both of those optimizations:
![image](https://user-images.githubusercontent.com/39736205/199863882-f3516b19-e088-4215-bb9e-696c9ba5fbe6.png)

After finishing first round of optimizations: Note, `clone_grids()` is now taking up 40% of the runtime. If this were C, this would happen in a single `memcpy()` and would likely be 1% of the runtime, hence why I wanted to try storing the grids in a flat array, which would let me use a single `[].slice()`, which maps to `memcpy()`.
![image](https://user-images.githubusercontent.com/39736205/199864034-99848ba2-fd9b-4cbd-bf78-eda8b02043b6.png)

After using a flat array for grids: Note, `clone_grids()` was removed and memcpy now takes up significantly less of the runtime because of this optimization. We are near the edge of what JS performance can be.
![image](https://user-images.githubusercontent.com/39736205/200007470-b7b1c4ad-e7e7-4e9b-9e43-a62fa22823ee.png)

