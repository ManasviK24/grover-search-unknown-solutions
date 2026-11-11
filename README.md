# Grover Search with an Unknown Number of Solutions

A reproducible notebook study of Grover search when the number of marked states is not known in advance. The project progresses from standard Grover amplification to sensitivity analysis, randomized iteration schedules, adaptive BBHT search, and finite instance comparisons with classical search, quantum counting assisted search, and optimal fixed point amplitude amplification.

## Research question

How can quantum search be used when the number of marked items, denoted by $s$, is unknown, and how do different strategies trade query cost, information assumptions, and success guarantees?

For a search space of size $N$, the marked fraction is $a=s/N$. In conventional Grover search, the success probability after $j$ iterations is

$$
P(j;N,s)=\sin^2\!\left((2j+1)\arcsin\sqrt{s/N}\right).
$$

A stopping time that is effective for one $s$ can overshoot for another. This project studies alternatives without assuming the exact marked count is known to every method.

## Notebooks

| Notebook | Focus |
| --- | --- |
| [01: Standard Grover validation](notebooks/01_standard_grover_validation.ipynb) | Analytical success curves, statevector checks, amplification and overshooting |
| [02: Incorrect solution count sensitivity](notebooks/02_incorrect_s_sensitivity.ipynb) | Consequences of selecting a stopping depth from an incorrect estimate of $s$ |
| [03: Randomized iteration search](notebooks/03_randomized_iteration_search_analysis.ipynb) | Randomized stopping times and the BBHT average success probability |
| [04: Adaptive BBHT](notebooks/04_adaptive_bbht_search.ipynb) | Adaptive window growth, complete search cost, distributional diagnostics, growth factor experiments |
| [05: Strategy comparison](notebooks/05_unknown_s_strategy_comparison.ipynb) | Known $s$ reference, classical sampling, BBHT, counting assisted search and optimal fixed point search |

The pre extension version of Notebook 05 is retained in [`archive/`](archive/05_unknown_s_strategy_comparison_original.ipynb) to preserve the project history. The version in `notebooks/` is the completed one.

## Experimental methodology

* The methods do **not** receive the true marked count unless explicitly identified as an **oracle informed reference**.
* The main finite instance comparison uses $N=1024$ and $s\in\{1,2,4,8,16,32,64\}$.
* The adaptive BBHT comparison uses repeated Monte Carlo trials; the original study uses 3000 completed trials per marked count in its primary comparison.
* Complete query costs include final candidate verification as defined in each notebook. Counting query units and generalized phase oracle implementation assumptions are documented separately to avoid misleading hardware comparisons.
* The fixed point method uses Yoder, Low, and Chuang's Chebyshev phase sequence with the prior promise $s\geq 1$, a failure tolerance of $0.1$, and a sequence independent of the actual $s$.
* The project analyzes **ideal mathematical oracle models**. It does not claim noisy hardware validation or a demonstrated physical quantum advantage.

## Findings within the tested scope

* A predetermined Grover iteration count can perform very poorly when it is chosen using an incorrect solution count.
* Randomized iterations mitigate dependence on a single destructive stopping depth; adaptive BBHT achieves favorable expected costs without exact knowledge of $s$.
* The **specific counting assisted policy studied here**, which counts once and then repeatedly uses one selected Grover depth, can suffer very large expected restart costs from rare inaccurate estimates. This does not characterize every counting assisted search policy.
* Yoder, Low, and Chuang fixed point search provides a success guarantee over a specified range of marked fractions. With the conservative prior $s\geq1$ and target failure probability $0.1$, the example $N=1024$ schedule uses $L=59$, corresponding to 58 Boolean oracle evaluations for the generalized amplification sequence and one final verification per attempt.
* On the seven tested marked counts and the chosen assumptions, the observed adaptive BBHT mean complete query cost is lower than that of the conservative fixed point schedule. This is a **finite instance observation**, not a universal ranking.

## Run locally

Python with Jupyter is sufficient for the notebook workflow. Install dependencies and open the notebooks in numbered order:

```bash
python -m pip install -r requirements.txt
jupyter lab
```

The notebooks include saved output figures for browsing directly on GitHub. The first three notebooks use Qiskit quantum state tools; later experiments mainly use NumPy based analytical and Monte Carlo calculations.

## References

See [REFERENCES.md](REFERENCES.md) for the foundational literature and links to the original publications.

## Status

The five notebook study is published as a research snapshot. Other strategies, alternative counting policies, circuit level benchmarking, and noisy hardware experiments can be investigated separately later. These are not prerequisites to sharing the present results.