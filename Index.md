# 1: Introduction

## 1.1 Context and Motivation
- Background on logistics, e-commerce, distribution costs
- Relevance of Vehicle Routing Problems (VRP) in reducing operational Aufwand
- NP Hard is Hard -> Funnel

## 1.2 Problem Statement and Approach
- Deterministic VRP as the main scope (keine stochastischen oder dynamischen Elemente)
- Theoretical foundation: VRP + ML
- Literature Review (110 paper analyzed in different VRP ans ML domains)
- Performance eval of ML vs. traditional optimization
- Common challenges, Future directions, combining ML + heuristics 

---

# 2: State of the Art

## 2.1 Vehicle Routing Problems (VRP)

### 2.1.1 Fundamentals and MIP Formulation
- Kurzdefinition: Set of customers + depot, goal = minimal total route cost
- Ableitung TSP
- MIP-Modell (Knoten, Kanten, Binärvariablen), capacity constraints, Depot, objective
- set partitioning formulation
- Deterministic vs stochastic differentiation; Annahmen (Demand, Costs, etc. sind fix)
- NP Hard

### 2.1.2 Common VRP Variants

<font color="#ff0000">GO Through once more and match avriant with Lit Review</font>

| **Abbreviation** | **Term**                           | **Explanation**                                                                                                                                               |
|------------------|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| MV               | Multiple Vehicles                  | Multiple vehicles potentially having their own capacities or cost structures, used simultaneously.                                                             |
| STW              | Soft Time Windows                  | Time windows with penalties for early/late arrival, but no strict exclusion rule (deviations can be "paid for").                                               |
| HTW              | Hard Time Windows                  | Time windows that must be strictly observed; if the vehicle arrives too early or too late, the job cannot be fulfilled.                                        |
| CAP              | Capacity Constraints               | Maximum vehicle load constraints that must not be exceeded (e.g., weight, volume).                                                                             |
| HF               | Heterogeneous Fleets               | Different vehicle types, each having unique capacities, costs, emissions, etc.                                                                                 |
| TD               | Time-Dependent Travel Times        | Travel times vary throughout the day (e.g., due to traffic jams, peak hours), making route planning more dynamic.                                              |
| GVRP             | Green / Emissions Constraints      | Inclusion of energy/fuel consumption, CO₂ emissions, etc., either as a dedicated objective function or constraint.                                             |
| SD               | Split Deliveries                   | A customer may receive multiple partial deliveries from different vehicles instead of a single delivery within one route.                                      |
| MD               | Multi-Depot / Multi-Center         | Multiple depots or distribution centers from which vehicles start and to which they may return.                                                                |
| PRD              | Periodic Scheduling (VRP)          | Routes spanning multiple days with recurring or scheduled deliveries optimized within an overall planning horizon.                                              |
| PDP              | Pickup and Delivery Pairing        | Tasks include both pickup and delivery locations, often with the constraint that pickup must occur before delivery.                                             |
| LS               | Large Scale                        | A very large customer base or extensive networks, often requiring specialized or scalable heuristic/ML-based approaches.                                        |
| MO               | Multi-Objective Goals              | Multiple objectives such as minimizing cost, minimizing delays, minimizing emissions, etc., are considered simultaneously.                                      |
| OR               | Open Routes                        | Vehicles do not necessarily have to return to the depot or may start from elsewhere (routes may end at a different point).                                      |
| SB               | Swap Body / Trailer Constraints    | Specific constraints regarding coupling/decoupling swap bodies or trailers, complicating route planning.                                                        |
| CMA              | Collaborative / Multi-Agent        | Multiple operators or agents cooperate, share resources, or coordinate routes (e.g., within supply chain networks).                                             |
| CL               | Clustering-Based Constraints       | Partitioning customers into clusters (regions, groups) to pre-structure routes (e.g., k-Means, DBSCAN, etc.).                                                   |
| EV               | Electric Vehicles                  | Consideration of battery capacity, charging times, selection of charging locations, range anxiety – special constraints in the routing process.                 |
| STOCH            | Stochastic Components              | Uncertain demand, uncertain travel times, random delays – requiring probabilistic or scenario-based optimization methods.                                       |

### 2.1.3 Approaches to Solve Deterministic VRPs
- **Exact Methods**:
	- Branch-and-Bound
	- Branch-and-Cut
	- (Column Generation)
		-->Guaranteed Optimum (aber rechenintensiv)
- **Heuristics & Metaheuristics**: 
	- constructive heuristics
	-  improvement heuristics
	- metaheuristics (Laporte and Semet (2002)).Clarke & Wright, Tabu Search, Genetic Algorithms
		  --> Vorteil: schneller auf größeren Instanzen, aber keine Guaranteed Optimality

### 2.1.4 **Benchmarking & Test Sets**: 
<font color="#ff0000">NOT REALLY NECESSARY</font>

- z.B. Solomon
  <font color="#ff0000">TODO, after Kap 4</font>
- Wichtig zur Vergleichbarkeit von Ansätzen in Kap 5.

## 2.2 Machine Learning Foundations

### 2.2.1 Basic ML Paradigms
high-level Kategorien, die beschreiben, wie ML formal organisiert ist
- **Supervised Learning**
- **Unsupervised Learning** 
- **Reinforcement Learning** 
### 2.2.2 Key ML Techniques
spezifische Algorithmen, Modelle oder Frameworks, die innerhalb dieser Paradigmen verwendet werden
- **Neural Networks** (Multi-Layer, CNN, RNN – je nach Anwendung)
- **SVM** (Support Vector Machines) für Klassifikation/Regression
- **Clustering** (K-Means, DBSCAN etc. für Segmentierung)
- Kurz, ohne tiefe Formeln, Fokus auf mögliche Anwendung im deterministischen VRP

# 3: Literature Analysis

- literature clustered by **ML approach** (to get a systematic overview)
- metadata: num paper, year, VRP flavor, ML flavor
- ONLY DETERMINISTIC

## 3.1 Supervised Learning Approaches
- Solution Construction / Routing (Imitation learning, Supervised classification)
- Approaches that directly learn from optimal VRP solutions (TSP, CVRP) to replicate them
- Ggf. pointer to pointer-network-based methods (wenn sie supervised trainiert wurden)

## 3.2 Unsupervised Learning Approaches
Clustering for Preprocessing (K-Means, DBSCAN, etc.)
Pattern Discovery

## 3.3 Reinforcement Learning (RL/DRL)
- **3.4.1 Value-based RL** (Q-Learning, DQN) in a *deterministic* scenario
  - Some papers still do RL in a static environment (no stochastics), e.g. partial route construction
- **3.4.2 Policy Gradient / Actor-Critic** for deterministic VRP
  - E.g. pointer networks or attention models that train via policy gradients
  - Noting the difference from dynamic VRP: “Here everything is known, but RL is used for end-to-end route generation”


## 3.4 Hybrid Methods
- ML-Assisted Exact Solvers (branch-and-bound decisions, cutting-plane selection)
- ML-Assisted Heuristics (Local search, ALNS with ML-based destroy/repair)

## 3.5 Summary of Findings
<font color="#ff0000">NOT REALLY NECESSARY</font>
- Compare the different approaches (supervised vs. RL vs. hybrids) in terms of:
  - Implementation complexity
  - Typical instance sizes tackled
  - Reported performance gains
- Identify any major research gaps: e.g. missing interpretability, limited generalization, data-hungry

---

# 4: Performance evaluation and Benchmarking

## 4.1 Evaluation Metrics and Datasets??
- Standard metrics: solution quality (gap to best-known), runtime, scalability
- Benchmark libraries: CVRPLIB, TSPLIB, or custom sets
- Possibly mention open-source frameworks used in the literature (e.g. OR-Tools, HGS-CVRP, etc.)

## 4.2 ML Models vs. Classical Optimization??
TOTO: 

---

# 5: Outlook

## 5.1 Challenges in Applying ML to VRP
- Data availability & labeling (wer hat so viel VRP-Daten?)
Interpretability and trust (why pick route X? black-box nets)
- Integration with real logistics systems (compatibility, IT infrastructure)

## 5.2 Future Directions and Combining ML + Heuristics
- Large-scale VRP: hybrid ML + advanced metaheuristics
- multi-objective VRP (cost, emissions, etc.)
- use Stochastic 

---

# 6: Conclusion

