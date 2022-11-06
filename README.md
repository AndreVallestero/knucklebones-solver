# knucklebones-solver
Knucklebones Solver using minimax and median-weighting for random probabilities

Check it out here: https://andrevallestero.github.io/knucklebones-solver/

Test it in game here: https://knucklebones.jaredp.co.uk/

Or play PVP: https://knucklebones.jaredp.co.uk/pvp

## TODO
- optimize
  - [x] (45421ms) baseline
  - [x] ~~{40x speedup} alpha beta pruning~~ (not possible due to roll median algorithm)
  - [x] (659ms, 70x speedup) profiling and optimizations 
  - [x] (260ms, 2.5x speedup) use flat array for grids
  - [x] ~~use [Uint8/16/32Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays) which might let us leverage `.reduce()`~~ copying `TypedArray` is slower than `Array`
  - [x] (130ms, 2x speedup) web worker / multithreading (split 3 top branches into its own webworker)
  - [x] (110ms, 1.1x speedup) implement functions as class prototype e.g `Array.prototype.eval = function () {}`
  - [ ] configurable web worker / multithreading (4+ threads)
  - [ ] rewrite in rust -> wasm
  - [ ] tail call recursion optimization
  - [ ] manually rewrite recursion as a loop
  - [ ] profiling and micro optimizations
    - [ ] optimize for branch predictor (test branchless)
    - [ ] optimize for memory locality / reduce cache misses
- [x] implement auto-place (button to automatically place the suggested position)
  - [x] this should also remove dice from the opponent's board if necessary
- [x] add reset button
- [x] compare median vs mean by playing the solver against each other

## Reference Benchmark

![image](https://user-images.githubusercontent.com/39736205/199863284-35712a55-cf26-4e6b-b2d6-967e1b02b5c3.png)

This config with depth=5 results in minimax being called 1,194,069 times

## Cool Profiling Flamegraphs and Other Notes

Baseline: Note, `structuredClone()` and `eval_column()` take up the majority of the runtime. `structuredClone()` was replaced with a custom `clone_grids()`, and eval_column was slow because of `.entries()` which I replaced with regular indexing.
![image](https://user-images.githubusercontent.com/39736205/199864435-0aa6fb2b-db7b-4377-9e4d-13558d8f2fd5.png)

After implementing both of those optimizations:
![image](https://user-images.githubusercontent.com/39736205/199863882-f3516b19-e088-4215-bb9e-696c9ba5fbe6.png)

After finishing first round of optimizations: Note, `clone_grids()` is now taking up 40% of the runtime. If this were C, this would happen in a single `memcpy()` and would likely be 1% of the runtime, hence why I wanted to try storing the grids in a flat array, which would let me use a single `[].slice()`, which maps to `memcpy()`.
![image](https://user-images.githubusercontent.com/39736205/199864034-99848ba2-fd9b-4cbd-bf78-eda8b02043b6.png)

After using a flat array for grids: Note, `clone_grids()` was removed and memcpy now takes up significantly less of the runtime because of this optimization. We are near the edge of what JS performance can be.
![image](https://user-images.githubusercontent.com/39736205/200007470-b7b1c4ad-e7e7-4e9b-9e43-a62fa22823ee.png)

While implementing web workers, I learned that class function are significantly faster than standalone functions if you're storing the data in the class. Because the workers used `structuredClone` to receive data, this would strip away the Board class so I decided making all the functions standalone. This classless implementation was 3x slower than the original. After some research, I found out that classes are able to JIT the function for the specific data types in its internal fields, which results in large performance gains. In the next version, I'm gonna try implementing the functions in the Array prototype to reduce indirection and improve performance.

After class protype functions:
![image](https://user-images.githubusercontent.com/39736205/200122414-b13ef622-a3d9-46e8-a394-74fe1f8a8638.png)

Results of other methods compared to the standard algorithm (mean depth 5) in 100 games with alternating starts:
| Method | Wins | Ties |
|---|---|---|
| Median Depth 5* | 45 | 1 |
| Mean Depth 4* | 48 | 1 |
| Mean Depth 2 | 42 | 2 |
| Naive (Depth 0) | 29 | 1 |
| Random | 12 | 2 |

*tested over 1000 games

