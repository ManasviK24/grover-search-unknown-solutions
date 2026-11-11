# Reproducibility and computational scope

This project provides five executed Jupyter notebooks and their saved outputs. The repository is intended to make the *methods, numerical experiments, assumptions, and reported data* inspectable. Exact recreation of the historical software environments cannot be guaranteed because the original package version lists were not recorded.

## Python environments recorded in notebook metadata

| Notebooks | Python version recorded |
| --- | --- |
| 01, 02, 03, 04 | 3.12.0 |
| 05 | 3.13.5 |

These values come from each notebook's `metadata.language_info.version`. They are recorded metadata, not a complete specification of the original environment.

The current [requirements.txt](requirements.txt) lists `numpy`, `pandas`, `matplotlib`, `qiskit`, and `jupyterlab` without exact version pins. A fresh installation may therefore use newer APIs or numerical libraries. **Do not cite an installation using the current requirements file as a byte for byte recreation of the original environment.**

## Local setup

Clone this repository, create a virtual environment, activate it, install dependencies, and launch Jupyter.

```bash
git clone https://github.com/ManasviK24/grover-search-unknown-solutions.git
cd grover-search-unknown-solutions
python -m venv .venv
```

On Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

On Linux or macOS:

```bash
source .venv/bin/activate
```

Then install and launch:

```bash
python -m pip install -r requirements.txt
jupyter lab
```

The first three notebooks use Qiskit statevectors for independent simulation checks. The final two focus on ideal mathematical probabilities and Monte Carlo calculations.

For a record of the environment in which **you** rerun the notebooks:

```bash
python --version
python -m pip freeze > local_environment.txt
```

This creates a record of your installation only. It does not reconstruct the packages used for the saved notebook outputs.

## Experiment assumptions to preserve

* **Search problem:** For the primary strategy comparison, use $N=1024$ and $s\in\{1,2,4,8,16,32,64\}$.
* **Unknown marked count:** The search algorithms may not consult the true $s$ to choose stopping parameters, except for the explicitly labeled known $s$ reference.
* **Adaptive BBHT:** The baseline uses growth factor $6/5$, 3000 complete Monte Carlo trials for each marked count, and the deterministic seeds defined in the notebooks. Notebook 05 checks continuity with Notebook 04.
* **Counting assisted policy:** Estimate once with a chosen fixed budget $P$, select one search depth from that estimate, and reuse that depth after failed verification. This is not an adaptive estimator and not a generic lower bound for all counting assisted search.
* **Fixed point policy:** Use the Yoder, Low, and Chuang Chebyshev construction, a promised lower bound $s\geq1$, and failure tolerance $\delta^2=0.1$. The schedule uses $L=59$ independently of the true $s$.
* **Costs:** Include candidate verification and the stipulated oracle evaluations for any estimation stage. See Notebook 05, Sections 3, 12, and 16 for the precise oracle conventions.

## What can be checked

1. Notebook 01 compares theoretical Grover probabilities with Qiskit statevector probabilities.
2. Notebook 02 tests stopping depth sensitivity to an incorrect marked count.
3. Notebook 03 reproduces the exact randomized BBHT average probability by direct and statevector calculations.
4. Notebook 04 estimates complete adaptive search costs and investigates their distributions.
5. Notebook 05 reproduces BBHT continuity checks, validates the counting probability law and the fixed point Chebyshev curve, and compares strategies on the same finite instance family.

Use the notebooks' assertions and output tables as the primary numerical checkpoints. Randomized sample means can differ if seeds, numerical libraries, random number generator behavior, or sampling conventions are changed. Analytical checks are expected to agree to floating point tolerance.

## Limits of the claims

The notebooks are ideal computational experiments. They do not include a calibrated noise model, compiled hardware gate cost, physical execution time, or a complete empirical study of asymptotic query complexity. Repeating a circuit a specified number of times in an analytical model does not show practical advantage on a quantum device.

The repository includes the source code and saved outputs to allow independent evaluation. A strictly pinned historical software environment would require recovering dependency versions from the original execution machines, which is not currently possible from the notebook metadata alone.
