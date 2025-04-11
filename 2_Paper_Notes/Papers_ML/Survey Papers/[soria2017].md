# A methodology for determining an effective subset of heuristics in selection hyper-heuristics

## Metadata
- **Link to PDF**: [[[soria2017]_A_methodology_for_determining_an_effective_subset_of_heuristics_in_selection_hyper-heuristics.pdf]]
- **Tags**:
  - #AlgorithmSelection
  - #ML-AssistedHeuristics
  - #ML-AssistedLocalSearch
  - #RL-ValueBased
  - #HyperHeuristic
  - #LocalSearchHeuristics
  - #ParameterTuning
  - #HybridLocalSearchRL
  - #VRPTW
  - #DeterministicVRP
- **Relevant**: true  
- **Fit Score**: 8  
- **State of the Art (SoA) Concepts**:
  - Multi-armed bandit–based operator selection
  - Dynamic hyper-heuristic framework
  - Selection of effective subsets of local search heuristics
  - Deterministic Vehicle Routing with Time Windows (VRPTW)
  - ParamILS-based parameter tuning
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: Solomon & Gehring/Homberger VRPTW benchmarks (via HyFlex / CHeSC 2011)

## Abstract
"We address the important step of determining an effective subset of heuristics in selection hyper-heuristics. Little attention has been devoted to this in the literature, and the decision is left at the discretion of the investigator. The performance of a hyper-heuristic depends on the quality and size of the heuristic pool. Using more than one heuristic is generally advantageous, however, an unnecessary large pool can decrease the performance of adaptive approaches. Our goal is to bring methodological rigour to this step. The proposed methodology uses non-parametric statistics and fitness landscape measurements from an available set of heuristics and benchmark instances, in order to produce a compact subset of effective heuristics for the underlying problem. We also propose a new iterated local search hyper-heuristic using multi-armed bandits coupled with a change detection mechanism. The methodology is tested on two real-world optimisation problems: course timetabling and vehicle routing. The proposed hyper-heuristic with a compact heuristic pool, outperforms state-of-the-art hyper-heuristics and competes with problem-specific methods in course timetabling, even producing new best-known solutions in 5 out of the 24 studied instances."

## Summary
- **Paper Scope & Motivation**
  - Proposes a rigorous empirical methodology to select a smaller, more effective subset of low-level heuristics in a selection hyper-heuristic framework.
  - Addresses a key practical challenge: large heuristic pools can degrade performance due to memory/overhead, yet too few heuristics can limit search diversity.
  - Demonstrates the approach on two domains: course timetabling and the Vehicle Routing Problem with Time Windows (VRPTW), both under deterministic conditions.

- **Methodology for Selecting Heuristic Subsets**
  - Employs a *three-stage approach*:
    1. **Ranking Heuristics**: Uses non-parametric statistical tests (Friedman, Quade) on two proposed landscape probing metrics (Evolvability and Landmarking) to rank individual heuristics.
    2. **Incremental Grouping**: Builds subsets of increasing size (top-N heuristics) to see which subset yields the best performance within a given hyper-heuristic algorithm.
    3. **Subset Selection**: Chooses the subset that provides best results across benchmark instances, effectively balancing performance and overhead.
  - Compares this selection method with off-line parametric tuning (using ParamILS) for operator probabilities as an alternative ranking approach.

- **Hyper-Heuristic Framework**
  - Uses an **Iterated Local Search (ILS)** as the main high-level strategy.
  - Within ILS, a pool of local search heuristics (“low-level heuristics”) is adaptively selected via a **multi-armed bandit mechanism** (Upper Confidence Bound approach) with **change detection** (Page-Hinkley test).
  - An “extreme value” credit assignment function emphasizes the best fitness improvement among recent heuristic applications.

- **Results on Vehicle Routing Problem with Time Windows**
  - Benchmarks from the Solomon and Gehring/Homberger sets, as well as the CHeSC 2011 competition framework (HyFlex VRPTW module).
  - The methodology successfully identifies a small subset of four heuristics that consistently outperforms the use of all available heuristics.
  - The proposed adaptive hyper-heuristic outperforms both a uniform-random selection of heuristics and a static selection-probability approach tuned via ParamILS.
  - Achieves competitive or superior results compared to many of the CHeSC contestants, obtaining a top-3 rank under competition scoring rules.

- **Key Observations & Strengths**
  - Demonstrates the importance of carefully choosing a smaller, well-performing set of heuristics for adaptive selection hyper-heuristics in deterministic VRP contexts.
  - Showcases that multi-armed bandits with change detection (for operator selection) effectively balance exploration and exploitation of heuristics.
  - The combination of non-parametric statistical ranking + iterative grouping is relatively parameter-light, making it replicable for other deterministic optimization settings (e.g., CVRP, VRPTW).

- **Limitations & Future Work**
  - Method focuses heavily on improvement heuristics for single-problem classes (timetabling, VRPTW). Constructive heuristics or other VRP variants (e.g., rich constraints) are not explored.
  - Practical usage requires a benchmark set of instances representative of the target problem domain, otherwise the “best subset” may not generalize to all instance types.
  - Authors suggest investigating interactions among heuristics (cooperative or overlapping neighborhoods) and extending to constructive or hybrid hyper-heuristics for broader coverage.

- **Relevance to Deterministic VRP & ML-based Methods**
  - Combines reinforcement-learning-based selection (bandits) with heuristic ranking, thereby illustrating a form of “ML-assisted” local search for deterministic VRPTW.
  - Contributes to hyper-heuristic literature by systematically narrowing the pool for efficiency gains, which can be impactful for large-scale routing problems.
  - Provides evidence that well-tuned adaptive selection frameworks can rival specialized problem-specific methods while remaining general.