# Time Complexity Comparisons

A small notebook, [`time_complexity_comparisons.ipynb`](time_complexity_comparisons.ipynb), that measures how the run time of different implementations of the same problem grows with the input size. Each section solves one problem in two or three ways with different time complexities, runs every solution on the same randomly generated inputs, and prints how long each one takes.

## What's in the notebook

| Section | Problem | Slower solution | Faster solution |
|---|---|---|---|
| O(n²) vs O(n) | Count pairs in an array whose sum is divisible by `k` | Nested loops, O(n²) | Remainder counting with a dictionary, O(n) on average |
| Θ(φⁿ) vs O(n) | Find the nth Fibonacci number | Plain recursion, Θ(φⁿ) ≈ Θ(1.618ⁿ) (O(2ⁿ) is a simpler upper bound) | Memoization, O(n) time and O(n) space |
| O(1) vs O(n) | Check whether a number is in a collection | `in` on a list, O(n) | `in` on a set, O(1) on average |
| O(log n) vs O(n) | Find a number's position in a sorted array | Linear search, O(n) | Binary search, O(log n) |
| O(n log n) vs O(n²) | Sort an array | Insertion sort, O(n²) | Merge sort and Python's `sorted()` (Timsort), O(n log n) |

Together these demonstrate O(1), O(log n), O(n), O(n log n), O(n²) and exponential growth.

## Running the notebook

Only the Python standard library is needed. Open `time_complexity_comparisons.ipynb` in Jupyter (or any notebook editor, such as VS Code) and run all cells. A full run takes about one and a half minutes on the machine used for the stored outputs.

Every random input uses a fixed seed, so the inputs and answers are the same on every run. Each timing is the median of several repeated runs after a warm-up. The stored timings are **machine-dependent, illustrative measurements**: compare how each solution's time changes as `n` grows, and how solutions to the same problem compare, rather than the absolute numbers.

## Background

### What Big O describes

Big O notation describes how the amount of work an algorithm does grows as the size of its input, `n`, grows. It keeps only the fastest-growing term and drops constant factors, so an algorithm that takes `3n + 20` steps and one that takes `n / 2` steps are both O(n). Strictly, O gives an upper bound on the growth rate; Θ (theta) is used when the growth rate is known exactly, as for the plain recursive Fibonacci function in the notebook.

| Complexity | Name | When `n` doubles, the work… |
|---|---|---|
| O(1) | constant | stays the same |
| O(log n) | logarithmic | grows by a constant amount (one more step for binary search) |
| O(n) | linear | doubles |
| O(n log n) | linearithmic | grows a little more than 2× |
| O(n²) | quadratic | grows 4× |
| Θ(φⁿ) ≈ Θ(1.618ⁿ) | exponential | is squared (each `+1` to `n` multiplies it by about 1.6) |

### Why input size matters

For small inputs almost any algorithm is fast, and an algorithm with a worse growth rate can even win because it has less overhead. The differences only show as `n` grows: going from 1,000 to 100,000 items multiplies O(n) work by 100 and O(n²) work by 10,000, while O(log n) work only goes from about 10 to about 17 steps. That is why the notebook times most solutions at several input sizes rather than just one.

### Growth rate vs measured wall-clock time

Big O predicts how running time *scales*, not how many seconds a run takes. Measured wall-clock time also depends on things Big O ignores:

- **Constant factors and implementation:** Python's built-in `sorted()` and the notebook's merge sort are both O(n log n), but `sorted()` is written in C and is many times faster than a merge sort written in Python.
- **Memory layout and caches:** scanning a list of 100,000 numbers whose objects are scattered across memory can take several times longer per item than scanning 10,000, even though the work per item is the same, because the CPU waits on main memory more often.
- **Hardware and environment:** CPU speed, other running programs, and the Python version all change timings.
- **Size of the numbers:** Python integers have no size limit, so arithmetic on very large numbers is not constant time.

### Average case vs worst case

Some complexities depend on the input, so it matters which case is being described:

- **Set and dictionary lookups** are O(1) on average, because hashing sends each value straight to a slot in a hash table. In the worst case, if many values collide in the same slots, a lookup can take O(n). Set lookups and the remainder-counting solution both rely on the average case.
- **Linear search** finds a target after about `n / 2` checks on average and `n` checks in the worst case (the target is last or missing); both are O(n). **Binary search** is O(log n) even in the worst case.
- **Insertion sort** is O(n²) in the average and worst cases, but O(n) on input that is already sorted.
- **Merge sort** is O(n log n) in every case. **Timsort**, used by Python's `sorted()`, is O(n log n) in the average and worst cases and O(n) in favorable cases such as input that is already sorted.
