# The BFGS and L-BFGS Methods

A seminar paper with its code, written for the PhD course **Advanced Numerical Optimization (20.IDI16)**, Doctoral Academic Studies in Computer Science, Faculty of Sciences and Mathematics, University of Niš.
Mentor: Prof. Marko Miladinović · Author: Elvir Muslić

The paper and the notebook are one version: the notebook computes every number, table and figure of the paper and cites the paper's sections, equations and algorithms by their numbers.

## Contents

| File | What it is |
|---|---|
| [`seminar.pdf`](seminar.pdf) | the paper; `seminar.tex` and `seminar.bib` are its LaTeX source |
| [`project/bfgs_project.ipynb`](project/bfgs_project.ipynb) | the notebook, with its outputs |
| `project/figures/` | the three figures of the paper |
| `project/requirements.txt` | the pinned package versions |

## What the notebook does

- **Methods, our own code:** gradient descent, BFGS (Algorithm 1) and L-BFGS with the two-loop recursion (Algorithm 3), all with one line search for the strong Wolfe conditions (Algorithm 2).
- **Checks of the theory:** the update computed by hand in Section 2.3, the two-loop recursion against the explicitly built matrix, and both Wolfe conditions at every step.
- **Experiments:** the Rosenbrock function, and TRIDIA and Raydan 1 from Vilin, the course's MATLAB framework, from n = 100 to n = 10^6 (Section 5).
- **SciPy only as a check:** SciPy's BFGS and L-BFGS-B against our methods (Section 5.4).

## Results

- **Rosenbrock function** (n = 2): BFGS needs 38 iterations, L-BFGS 37 and gradient descent 5508. The error ratios of BFGS fall toward 0; those of gradient descent stay near 0.999.
- **TRIDIA and Raydan 1** (n = 100): BFGS needs 73 and 92 iterations, L-BFGS 135 and 55, gradient descent 2547 and 220.
- **Larger n:** L-BFGS solves both functions at n = 10^6, in 1104 and 239 iterations, with 80 MB of stored pairs; a dense BFGS matrix would need 8 TB.
- **SciPy:** our update, applied along the steps of SciPy's BFGS, reproduces SciPy's matrix to a relative difference of 3.0e-13.

## Run

```bash
cd project
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
.venv/bin/jupyter nbconvert --to notebook --execute --inplace bfgs_project.ipynb
```

- The run takes less than a minute and rewrites `project/figures/`. Iteration counts are exact; times depend on the machine.
- Alternatively, open `project/` in VS Code, select the `.venv` kernel and choose Run All.
- To rebuild the paper: `latexmk -pdf seminar.tex` (with biber).

## Credits

- **Theory:** J. Nocedal and S. J. Wright, *Numerical Optimization*, 2nd ed., Springer, 2006; D. C. Liu and J. Nocedal, On the limited memory BFGS method for large scale optimization, *Math. Program.* 45 (1989) 503–528.
- **Test functions:** TRIDIA and Raydan 1, with their starting point, as defined in [Vilin](https://github.com/markomil/vilin-numerical-optimization) (P. Živadinović and M. Miladinović, MIT License; M. Miladinović and P. Živadinović, [arXiv:1812.10986](https://arxiv.org/abs/1812.10986)), which takes them from N. Andrei, *An Unconstrained Optimization Test Functions Collection*, Adv. Model. Optim. 10 (2008) 147–161.
- **Software:** NumPy and Matplotlib; SciPy only for the checks.
