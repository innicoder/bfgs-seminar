# The BFGS Method

A seminar project, written as study work for the PhD course **Advanced Numerical Optimization (20.IDI16)**, Doctoral Academic Studies in Computer Science, Faculty of Sciences and Mathematics, University of Niš.
Instructor: Prof. Marko Miladinović · Author: Elvir Muslić

The notebook runs SciPy's BFGS method (`scipy.optimize.minimize` with `method="BFGS"`) and a steepest descent baseline (SciPy's `line_search`) on the Rosenbrock function and on two test functions from Vilin, the optimization framework of the course. It also computes with NumPy the update that the paper works out by hand, and checks the strong Wolfe conditions and the BFGS update on SciPy's steps. The paper explains, from SciPy's source, how the library call carries out the algorithm.

## Results

- **Computed update.** With H = I, s = (1, 0) and y = (2, 1), the update gives [[3/4, -1/2], [-1/2, 1]] exactly, both from the product form and from the rank-two form. Its eigenvalues are 0.3596 and 1.3904.
- **Rosenbrock function** (n = 2, from (-1.2, 1), stopping when ||g||_2 <= 1e-5): BFGS needs 32 iterations, steepest descent 10 078.
  - Every step of both runs satisfies the strong Wolfe conditions, so s^T y > 0.
  - SciPy returns only its last matrix. Rebuilding the earlier ones with the update from H_0 = I reproduces that last matrix (`hess_inv`) to 4.4e-13. Every rebuilt matrix satisfies the secant equation (relative residual at most 9.5e-14) and is positive definite.
  - The last five BFGS error ratios have geometric mean 0.063, which is consistent with superlinear convergence. Those of steepest descent lie between 0.9985 and 0.9995, close to the linear bound (k - 1)/(k + 1) = 0.9992 for the condition number k = 2508 of the Hessian at the minimizer.
- **Two Vilin test functions** (n = 100, from Vilin's starting point (1, ..., 1), same stopping test): on TRIDIA, a convex quadratic, BFGS needs 111 iterations (124 evaluations of f and of its gradient, final ||g||_2 = 1.7e-8) and steepest descent 5665; on Raydan 1, BFGS needs 58 (78 evaluations, final ||g||_2 = 5.8e-6) and steepest descent 347.

## Run

```bash
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
.venv/bin/jupyter nbconvert --to notebook --execute --inplace bfgs_project.ipynb
```

- Alternatively, open the folder in VS Code, select the `.venv` kernel and choose Run All.
- Tested with Python 3.14. The run takes a few seconds.
- The run rewrites `figures/`.

## Files

| File | Contents |
|---|---|
| `bfgs_project.ipynb` | the notebook, with outputs |
| `figures/` | the two figures of the paper |
| `requirements.txt` | pinned package versions |

## Credits

- **Theory:** J. Nocedal and S. J. Wright, *Numerical Optimization*, 2nd ed., Springer, 2006.
- **Test functions:** TRIDIA and Raydan 1, with their starting point, as defined in [Vilin](https://github.com/markomil/vilin-numerical-optimization) (P. Živadinović and M. Miladinović, MIT License; M. Miladinović and P. Živadinović, [arXiv:1812.10986](https://arxiv.org/abs/1812.10986)), which takes its test functions mostly from N. Andrei, *An Unconstrained Optimization Test Functions Collection*, Adv. Model. Optim. 10 (2008) 147–161.
- **Software:** SciPy (the BFGS method, the line searches and the Rosenbrock function), NumPy and Matplotlib.
