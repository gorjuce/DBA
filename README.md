# DBA Thesis Code — Startup Coopetition (Threshold Hawk-Dove Game)

Agent-based model (NetLogo) and analysis scripts (Python) for a DBA thesis on the emergence of innovation ecosystems. A population of startups chooses between **cooperating** ("dove") and **competing** ("hawk") in groups of size N, following an N-person Hawk-Dove game with an engagement threshold (HDG-T), after Chen et al. (2017) *[full reference to be added]*.

## Contents

| File | Purpose |
|---|---|
| `hawk-dove-model.nlogox` | NetLogo 7.0.4 model: analytical computation of interior equilibria (reproduction of Figure 5 in Chen et al.) and imitation-dynamics simulation |
| `analysis.ipynb` | Analysis of BehaviorSpace outputs (P1 to P5): regime classification, tables and figures used in the manuscript |
| `resultats/` | BehaviorSpace CSV inputs (`P1.csv` … `P5.csv`) and figure outputs (`p1.png` … `p5.png`) |

## Requirements

- NetLogo 7.0.4
- Python 3.12 with `pandas`, `matplotlib` and Jupyter

```bash
pip install pandas matplotlib jupyter
```

## The model

- Fixed population of Z = 100 agents, each a "dove" (cooperator) or a "hawk" (competitor).
- Payoffs are computed for groups of N agents drawn from the population (hypergeometric sampling). If the share of doves reaches the threshold T, doves exclude hawks and share the resource R (cost c_d per excluded hawk); otherwise hawks fight over R (cost c_h per rival).
- At each tick, every agent compares its payoff with that of a randomly drawn agent and imitates it with a Fermi probability (β = 1). Updates are synchronous.
- The simulation stops at fixation (all doves or all hawks) or after 2000 ticks. There is no mutation.

Reference parameters: Z = 100, N = 5, c_h = c_d = 0.8, T = 0.4, R = 1, x₀ = 0.5.

## Reproducing the results

1. Open `hawk-dove-model.nlogox` in NetLogo.
2. Run the BehaviorSpace experiments below (50 replications each, 2000-tick horizon) and export the output in *table* format to `resultats/`.
3. Run `manuscript_version.ipynb`.

| Analysis | BehaviorSpace experiment | Varied parameter(s) | File |
|---|---|---|---|
| P1 | `P1 (variation de c-h et c-d)` | c_h, c_d ∈ [0, 1], step 0.05 | `P1.csv` |
| P2 | `P2 (variation de R)` | R ∈ [0.1, 2], step 0.1 | `P2.csv` |
| P3 | `P3 (variation de T)` | T ∈ [0.1, 1], step 0.1 | `P3.csv` |
| P4 | `P4 (variation de init)` | x₀ ∈ [0.05, 0.95], step 0.05 | `P4.csv` |
| P5 | `P5 (variation de N)` | N ∈ [3, 15] | `P5.csv` |

No random seed is set, so results vary slightly between runs (sampling noise with 50 replications).

## Regime definitions

Each run is classified as follows (threshold = 500 ticks, i.e. ¼ of the horizon):

- **Coexistence**: cooperators and competitors coexist for at least 500 ticks;
- **Cooperation**: fixation on cooperation before 500 ticks;
- **Competition**: fixation on competition before 500 ticks.

For P1, each (c_h, c_d) cell takes the dominant regime across its 50 replications.

## Author

Sébastien — DBA, IAE de Nice / Inria Startup Studio.
