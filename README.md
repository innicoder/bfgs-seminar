# The BFGS Method

Seminar project for the PhD course **Advanced Numerical Optimization (20.IDI16)**, Doctoral Academic Studies in Computer Science, Faculty of Sciences and Mathematics, University of Niš.
Instructor: Prof. Marko Miladinović · Author: Elvir Muslić

The notebook implements the BFGS method with a strong Wolfe line search (Nocedal and Wright, Algorithms 3.5 and 3.6) in NumPy, reproduces the update computed by hand in the paper, checks the properties of the update at every step of a run, and compares BFGS with steepest descent on the Rosenbrock function and on six test functions from [Vilin](https://github.com/markomil/vilin-numerical-optimization). It verifies its results with 17 checks, which stop the run if any checked value deviates.

## Results

- **Computed update.** With H = I, s = (1, 0) and y = (2, 1) the update gives [[3/4, -1/2], [-1/2, 1]] exactly, with eigenvalues 0.3596 and 1.3904.
- **Rosenbrock function** (n = 2, from (-1.2, 1)): BFGS meets the stopping test ||g|| <= 1e-5 max(1, ||x||) after 39 iterations, steepest descent after 8098.
  - Run to the tolerance 1e-10, BFGS takes 40 steps. At every step both strong Wolfe conditions hold, s^T y > 0, and the updated H is symmetric, satisfies H y = s (relative residual at most 4.7e-15) and has a Cholesky factorization.
  - The last five error ratios of BFGS are 0.095, 0.052, 0.13, 1.1e-4 and 7.1e-4 (geometric mean 0.0087), consistent with superlinear convergence. Those of steepest descent stay between 0.998 and 1.000, close to the linear bound (k - 1)/(k + 1) = 0.9992 for the condition number k = 2508 of the Hessian at the minimizer.
- **Six Vilin functions** at n = 100 and n = 1000: BFGS solves all 12 problems in at most 312 iterations and reaches the known optimal value to a relative gap of at most 9.6e-8. Steepest descent fails on 4 of them within 10 000 iterations.
- **Storage.** At n = 1000 the matrix H takes 10^6 numbers, 8 MB.

## Run

```bash
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
.venv/bin/jupyter nbconvert --to notebook --execute --inplace bfgs_project.ipynb
```

- Tested with Python 3.14. The run takes under 10 s.
- The run rewrites `figures/` and `results.json`.

## Files

| File | Contents |
|---|---|
| `bfgs_project.ipynb` | the notebook, with outputs |
| `results.json` | the key numbers written by the run |
| `figures/` | the two figures of the paper |
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
