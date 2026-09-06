# Quasi-Newton Methods BFGS and L-BFGS: Implementation and Comparison

Seminar project for the PhD course **Advanced Numerical Optimization (20.IDI16)**, Doctoral Academic Studies in Computer Science, Faculty of Sciences and Mathematics, University of Niš.
Instructor: Prof. Marko Miladinović · Author: Elvir Muslić

The notebook implements steepest descent, BFGS and L-BFGS (two-loop recursion), all with a strong Wolfe line search (Nocedal & Wright, Algorithms 3.5–3.6). It verifies them against the theory with 33 checks and compares them on 15 test functions from [Vilin](https://github.com/markomil/vilin-numerical-optimization).

## Results

- **Rosenbrock function** (n = 2):
  - Iterations: BFGS 41, L-BFGS (m = 5) 40, steepest descent 18 404.
  - BFGS's last error ratios fall to about 0.006, consistent with superlinear convergence. Steepest descent stays at 0.999, which is linear.
- **Benchmark** on the 15 Vilin functions:
  - BFGS and L-BFGS solve all 15 at n = 100 and at n = 1000.
  - Steepest descent solves 12 and 7.
  - Dolan–Moré performance profiles are in `figures/`.
- **L-BFGS memory study:** m = 3, 5, 10 and 20.
- **Large scale:** the extended Rosenbrock function with n = 10^6, which is the two-dimensional problem replicated.
  - L-BFGS needs 39 iterations from Vilin's start and 1127 from a perturbed start.
  - Its stored pairs take 80 MB; a dense BFGS matrix would need 8000 GB.
- **Timings** come from a run on a heavily loaded machine (90.9 s in total), so they are noisy.

## Run

```bash
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
.venv/bin/jupyter nbconvert --to notebook --execute --inplace bfgs_lbfgs_project.ipynb
```

- Tested with Python 3.14.
- The run rewrites `figures/` and `results.json`.

## Files

| File | Contents |
|---|---|
| `bfgs_lbfgs_project.ipynb` | the notebook, with outputs |
| `results.json` | the key numbers written by the run |
| `figures/` | the six figures |
| `requirements.txt` | pinned package versions |

## Credits

- **Test functions and starting points:** ported from Vilin (M. Miladinović and P. Živadinović, [arXiv:1812.10986](https://arxiv.org/abs/1812.10986)). Vilin takes its functions from N. Andrei, *An Unconstrained Optimization Test Functions Collection*, Adv. Model. Optim. 10 (2008) 147–161.
- **Vilin's license:**

```
MIT License

Copyright (c) 2018 Predrag Živadinović, Marko Miladinović

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
