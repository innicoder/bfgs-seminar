# Quasi-Newton Methods BFGS and L-BFGS: Implementation and Comparison

Seminar project for the PhD course **Advanced Numerical Optimization (20.IDI16)**, Doctoral Academic Studies in Computer Science, Faculty of Sciences and Mathematics, University of Niš.
Instructor: Prof. Marko Miladinović · Author: Elvir Muslić

The notebook implements steepest descent, BFGS and L-BFGS (two-loop recursion), all with a strong Wolfe line search (Nocedal & Wright, Algorithms 3.5–3.6). It verifies them against the theory with 33 checks and compares them on 15 test functions from [Vilin](https://github.com/markomil/vilin-numerical-optimization). A second notebook improves the gradient method with two-point step sizes.

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
- **Two-point step sizes** (`two_point_steps.ipynb`):
  - The gradient method with the Barzilai–Borwein step or the Scalar Correction step of Miladinović, Stanimirović and Miljković (2011), both with Grippo's nonmonotone line search, solves 15/15 at n = 100 and 14/15 at n = 1000. Steepest descent solves 12 and 7.
  - On the problems both solve it needs about a tenth of the iterations of steepest descent, and it comes within about 2x of L-BFGS (Scalar Correction: 2.11 at n = 100, 1.74 at n = 1000).
  - Using the same scalars as the L-BFGS scaling does not help. The standard choice s^T y / y^T y stays the best.
  - This notebook reuses the main notebook's code, reproduces its 60 stored runs exactly, and reports iteration and evaluation counts only.

## Run

```bash
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
.venv/bin/jupyter nbconvert --to notebook --execute --inplace bfgs_lbfgs_project.ipynb
.venv/bin/jupyter nbconvert --to notebook --execute --inplace two_point_steps.ipynb
```

- Tested with Python 3.14.
- The main run rewrites `figures/` and `results.json`. The second notebook (22 checks, about 20 s) writes `results_two_point.json` and `figures/two_point_profiles.pdf`.

## Files

| File | Contents |
|---|---|
| `bfgs_lbfgs_project.ipynb` | the notebook, with outputs |
| `two_point_steps.ipynb` | the two-point step sizes, with outputs |
| `results.json` | the key numbers written by the main run |
| `results_two_point.json` | the numbers of the two-point notebook |
| `figures/` | the seven figures |
| `requirements.txt` | pinned package versions |

## Credits

- **Test functions and starting points:** ported from Vilin (M. Miladinović and P. Živadinović, [arXiv:1812.10986](https://arxiv.org/abs/1812.10986)). Vilin takes its functions from N. Andrei, *An Unconstrained Optimization Test Functions Collection*, Adv. Model. Optim. 10 (2008) 147–161.
- **Two-point steps:** J. Barzilai and J. M. Borwein (1988); L. Grippo, F. Lampariello and S. Lucidi (1986); M. Raydan (1997); M. Miladinović, P. Stanimirović and S. Miljković, *Scalar correction method for solving large scale unconstrained minimization problems*, JOTA 151 (2011) 304–320.
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
