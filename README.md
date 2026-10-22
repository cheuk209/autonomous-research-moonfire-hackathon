# autonomous-research-moonfire-hackathon

# LLM-Guided CFD Experiment Selection under Limited Simulation Budgets

**Track:** Auto-Research

## 1. Background & Motivation

Computational Fluid Dynamics (CFD) simulations often involve many parameters, including boundary conditions, fluid properties, and geometry, creating a potentially high-dimensional search space.

Selecting the next simulation requires substantial domain knowledge and careful interpretation of previous results. However, valuable insights from earlier experiments may be overlooked, particularly when computational budgets are limited.

Recent studies have explored LLM-driven scientific experimentation and CFD automation:

- [**AI CFD Scientist (2026)**](https://arxiv.org/abs/2605.06607)**:** Automates CFD research workflows, including parameter sweeps, hypothesis generation, simulation execution, and scientific reporting.
- [**OptMetaOpenFOAM (2026)**](https://doi.org/10.1016/j.taml.2026.100660)**:** Uses LLMs to orchestrate CFD sensitivity analysis and parameter optimisation.
- [**BORA (2026)**](https://pubs.rsc.org/en/content/articlehtml/2026/dd/d5dd00520e)**:** Combines LLM reasoning with Bayesian Optimisation for iterative experiment selection.
- [**Foam-Agent (2026)**](https://doi.org/10.1016/j.cma.2026.119271)**:** Automates OpenFOAM simulation setup, execution, and error correction.

While these approaches address automated experimentation and optimisation, **our focus is specifically on the quality of next-experiment decisions under a strict simulation budget**.

Rather than developing a complete autonomous CFD pipeline, we investigate whether an LLM can systematically use historical simulation results, formulate and revise physical hypotheses, and select more informative experiments.

## 2. Research Question

**Given a limited CFD simulation budget, can an LLM use previous simulation results and physical reasoning to select more informative experiments than conventional sampling strategies?**

Our goal is to determine whether feedback-driven LLM reasoning can improve our understanding of a CFD system using fewer simulations.

## 3. CFD Benchmark — Backward-Facing Step

We will use the OpenFOAM **Backward-Facing Step (**`pitzDaily`**)** tutorial.

Fluid flows through a channel with a sudden expansion, creating a recirculation region behind the step.

![46 questions with answers in TECPLOT | Science topic](https://images.openai.com/static-rsc-4/LzHAgPj_7xVmo6RpGPgMQg_sSbUAlDk82fv3CK9xJBIs5k-hok_ntovShKg-7kM8aUbuY3clTR5KCQDwEiOcHQVNW-WgoN8OAXJPNMZGqXSt7ZBM-Xa3AhrD45B1lcVG-Glj7FRK8hg-am-7uxwtnCtqaeywrjAUqHY4d4kxX_MD4s-7_fcOILJnfSXX-uBQ?purpose=fullsize)

**Input parameters:**

- Reynolds number (Re), representing the flow conditions.
- Expansion ratio (optional), representing the channel geometry.

**Output:**

- Reattachment length: the distance downstream before the separated flow reattaches to the wall.

**Relationship to learn:**
$$
(Re, \mathrm{Expansion\ Ratio}) \rightarrow \mathrm{Reattachment\ Length}
$$
We will begin with Reynolds number only and introduce expansion ratio if time permits.

## 4. Experiment Design

We will precompute a CFD benchmark dataset covering different combinations of Reynolds number and expansion ratio. This dataset represents the reference relationship between the input parameters and reattachment length.

**The objective is to reconstruct this relationship as accurately as possible using as few CFD experiments as possible.**

Both methods start with the **same two initial CFD experiments**. We will decide the budget for additional experiments later.

### Method A — LLM-Guided Experiment Selection

1. Analyse the two initial experiments and their results.
2. Formulate a physical hypothesis about the relationship between the input parameters and reattachment length.
3. Select the next parameter combination to investigate.
4. Reveal the corresponding CFD result from the benchmark.
5. Evaluate the new evidence and revise the hypothesis.
6. Repeat until the agreed experiment budget is exhausted.

### Method B — Random Sampling (Baseline)

Starting from the same two initial experiments, randomly select additional parameter combinations from the same candidate pool.

### Optional Method C — Gaussian Process Active Learning

Use surrogate-model uncertainty to select informative experiments, if time permits.

### Comparison

After each new experiment, all methods use the **same interpolation or surrogate model** to predict reattachment length at unseen parameter combinations.

We compare prediction accuracy against a fixed, held-out CFD test dataset.

**The winner is the method that reconstructs the relationship more accurately with fewer experiments.**

## 5. Evaluation

After each experiment, all methods use the **same interpolation** to reconstruct the relationship between input parameters and reattachment length.

Prediction accuracy is evaluated on a fixed, held-out CFD test dataset.

**Primary metric:** Root Mean Squared Error (RMSE) of predicted reattachment length.

We compare how prediction error changes as additional experiments are selected, under the same simulation budget.

Random sampling will be repeated across multiple runs to establish a reliable baseline.

## 6. Expected Outcome

We aim to build a lightweight, closed-loop CFD research agent that:

- Learns from previous simulation results.
- Formulates and revises physical hypotheses.
- Makes informed decisions about which simulation to run next.
- Demonstrates whether these decisions improve prediction accuracy compared with random sampling and, if feasible, Gaussian Process Active Learning.

**The objective is not to assume that LLM-guided selection is superior, but to measure whether its reasoning leads to better experiment-selection decisions under limited computational resources.**

### Structures

              LLM Agent
                  |
       Select parameters (Re, ER)
                  |
            Python Tool
                  |
               foamlib
                  |
              OpenFOAM
                  |
         CFD Simulation Result
                  |
       Reattachment Length
                  |
            LLM Feedback
                  |
       Next Experiment Decision

## TODO 

for Tiexing: 

- [ ] Generate synthetic data for the case A. 
- [ ] Use foamlib to call functions. 
- [ ] Then interpolation the relationships and treat the benchmarks some where. 
- [ ] Define the loss functions, comparing the results from the experiement b and c
- [ ] We wil also need to store the the number of the parameters we tried fromt th both experiemment 
- [ ] In order to do this, use openfoam.app, install foamlib

For Kai: 

- [ ] Build agentic system? what do you need? 
- [ ] More things to come

For hao ge: 

- [ ] 

What else:

- [ ] Presentations?
- [ ] Possible questions?
- [ ] Logistics?
  - [ ] 20mins to go there from aecom to go there. 
  - [ ] Bring your charger
  - [ ] Tiexing bring mouse
- [ ] Do dry runs
