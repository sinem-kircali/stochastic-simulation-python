# Stochastic Simulation from Scratch: Random Variates to Discrete-Event Models

A single notebook covering the main building blocks of a simulation study, from generating random numbers to running replicated SimPy experiments with confidence intervals. Each part is checked against a known answer.

Built for the *Modeling and Simulation* course (Industrial Engineering, Koç University).

## What's inside
| Part | Technique | Validation |
|---|---|---|
| 1. Random-variate generation | inverse transform for a two-sided exponential, a piecewise CDF, a discrete distribution (P ∝ x²) and a discrete uniform | KS / chi-square tests against the true distribution |
| 2. Input modelling | exponential Q-Q plot; chi-square normality test with equal-probability intervals | samples with a known answer; power at N = 100 vs 1,000 |
| 3. Dump-truck system | the same model written twice: **event scheduling** with a hand-built future event list, and **process interaction** in SimPy | long-run utilisations agree: loaders 0.309 vs 0.309, scale 0.859 vs 0.859 |
| 4. Job shop | 4 stations, 4 job types with fixed routes; SimPy, 200 h warm-up + 800 h run, 20 replications | utilisations match the theoretical offered load |

## Key results
- **Dump truck:**
  - The scale is the bottleneck: busy 86% of the time in the long run, while the two loaders are busy about 31%.
  - In the simulated shift, trucks wait on average 9 minutes for the scale vs 1 minute for a loader.
- **Job shop** (95% CIs over 20 replications):
  - Station 3 is the most loaded at 73.4% [70.9, 76.0]; the other stations are at about 50%.
  - Mean response times are 147.0, 89.5, 82.6 and 45.3 h for job types 1–4. That is only 1–2 h above pure processing time, so the shop is not capacity-constrained.

## Notes on this version
This notebook combines and reworks four separate homework notebooks:
- **Dump truck, event scheduling:** the original was incomplete. It was rewritten with a proper future event list, state updates and busy-time accumulation, and a printed trace.
- **Dump truck, SimPy:** utilisation tracking was added, which the original did not report.
- **Job shop:** several bugs were fixed:
  - a missing process argument;
  - a typo that reused the wrong processing time at station 2;
  - utilisation counting queued jobs as busy workers;
  - statistics leaking between replications;
  - the warm-up period not being removed;
  - confidence intervals for utilisation using a binomial formula instead of a t-interval across replications.
- **Generators:** these now use vectorised code (`np.searchsorted` for discrete inverse transform) and `numpy.random.Generator`.

## Running it
```bash
pip install -r requirements.txt
jupyter notebook stochastic_simulation.ipynb
```
No data is needed. Everything is generated and seeded. A full run takes about a minute.

## Tech stack
Python · SimPy · NumPy · SciPy · matplotlib
