# RegimeFree

Most adaptive trading strategies work in two stages: first detect the market regime, then adjust the strategy accordingly. The detector can be wrong. The switching adds latency. And the regimes you define upfront may not be the ones that actually matter.

**RegimeFree skips the detector entirely.**

Instead, the trading agent continuously self-tunes its temporal horizon - how far back it looks when making decisions - using an algorithm called SDPSA (Simultaneous Deterministic Perturbation Stochastic Approximation). When markets are trending, a longer horizon naturally performs better and gets selected. When markets are choppy, a shorter one does. The agent figures this out on its own, from experience, without ever being told what regime it is in.

The result is implicit, continuous regime adaptation - not a pipeline, not a classifier, just an agent that adjusts as the world changes.

---

## The Core Idea

In n-step Temporal Difference (TD) learning, an agent updates its value estimates using n steps of realized experience before bootstrapping. The choice of n controls how far into the future the agent thinks:

- Small n → myopic, reacts fast, noisy
- Large n → far-sighted, smoother, slower to adapt

The optimal n is not fixed. It depends on the environment - and in financial markets, the environment shifts. SDPSA finds and tracks the optimal n online, running on a slow timescale while the core TD learning runs fast. No labels. No regime classifier. No switching logic.

---

## Research Question

> Can online discrete hyperparameter optimization replace explicit regime detection in adaptive trading strategies?

---

## Project Structure

```
RegimeFree/
├── data/           # Data pipeline (yfinance)
├── env/            # Trading environment (MDP definition)
├── agent/          # n-step TD and SDPSA implementation
├── experiments/    # Experiment configs and results
├── notebooks/      # Exploratory analysis and visualizations
├── paper/          # Manuscript drafts
└── tests/          # Unit tests
```

---

## Status

Early stage. MDP formulation in progress.

---

## Theoretical Foundation

Built on:
- Mandal & Bhatnagar (2024). *n-Step Temporal Difference Learning with Optimal n.* arXiv:2303.07068
- Two-timescale stochastic approximation (Borkar, 2022)
- Differential inclusions for convergence under discontinuous dynamics

---

## Data

S&P 500 daily data via `yfinance`. No proprietary data required.

---

## Goals

- Working backtested trading strategy
- Research paper comparing RegimeFree against fixed-n baselines and classical regime-switching approaches
- Clean, readable implementation that others can build on
