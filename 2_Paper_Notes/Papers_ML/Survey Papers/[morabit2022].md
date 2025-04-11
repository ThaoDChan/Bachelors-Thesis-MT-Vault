# Machine-learning-based arc selection for constrained shortest path problems in column generation

## Metadata
- **Link to PDF**: [[[morabit2022]_Machine-learning-based_arc_selection_for_constrained_shortest_path_problems_in_column_generation.pdf]]
- **Tags**:
  - #ML-Supervised
  - #ML-SupervisedFromOptimal
  - #ImitationLearning
  - #ML-AssistedExact
  - #ML-AssistedBranchAndBound
  - #ML-AssistedHeuristics
  - #DeterministicVRP
  - #VRPTW
  - #ColumnGenerationHeuristics
  - #CrewScheduling
- **Relevant**: true  
- **Fit Score**: 9
- **State of the Art (SoA) Concepts**:
  - Column Generation for VRPTW
  - Supervised Learning (imitation learning) for arc selection
  - Vehicle Crew Scheduling (second application example)
  - Machine Learning–based pricing heuristics
- **Performance Evaluation**: yes
- **Performance Evaluation Framework**: Homberger & Gehring benchmark sets for VRPTW

## Abstract
“Column generation is an iterative method used to solve a variety of optimization problems. It decomposes the problem into two parts: a master problem, and one or more pricing problems (PP). The total computing time taken by the method is divided between these two parts. In routing or scheduling applications, the problems are mostly defined on a network, and the PP is usually an NP-hard shortest path problem with resource constraints. In this work, we propose a new heuristic pricing algorithm based on machine learning. By taking advantage of the data collected during previous executions, the objective is to reduce the size of the network and accelerate the PP, keeping only the arcs that have a high chance to be part of the linear relaxation solution. The method has been applied to two specific problems: the vehicle and crew scheduling problem in public transit and the vehicle routing problem with time windows. Reductions in computational time of up to 40% can be obtained.”

## Summary
- **Objective & Context**
  - Proposes a machine-learning-based approach to accelerate column generation (CG) by reducing the network used in the pricing subproblem.
  - Targets two main applications: (i) Vehicle and Crew Scheduling Problem (VCSP), (ii) Vehicle Routing Problem with Time Windows (VRPTW).
  - Focuses on the linear relaxation of these problems, where the subproblem (pricing problem) is a constrained shortest path problem (SPPRC or ESPPRC).

- **Key Idea & ML Method**
  - Uses **supervised learning** in an imitation-learning style: trains a binary classifier to predict which arcs are likely to appear in profitable routes or schedules.
  - During a “data collection” phase on prior runs, arcs used in generated columns (i.e., with negative reduced cost) are labeled “1,” others labeled “0.”
  - The trained model then filters out arcs that are less likely to be useful, effectively reducing the graph for the pricing problem.

- **Approach & Algorithmic Details**
  - In the column generation procedure, a standard RMP (restricted master problem) is solved iteratively. The subproblem involves finding negative reduced cost paths via a labeling algorithm.
  - The proposed ML approach constructs a **reduced subproblem network** before the CG begins. Only arcs predicted as “promising” by the classifier are retained.
  - If the reduced network fails to generate enough negative reduced cost columns, the algorithm temporarily switches to the full network.
  - Arcs remain dynamically included or excluded based on thresholds (minimum or maximum number of columns discovered) to balance network size and solution quality.

- **Applications & Implementation**
  - **Vehicle & Crew Scheduling**:
    - The network is structured around trip nodes (departure, intermediate, arrival) and arcs representing bus or walking movements.
    - Only walking arcs (the vast majority of arcs) are subjected to the ML-based selection because bus arcs are strictly dictated by operational constraints.
    - Demonstrates significant speedups in the pricing step without harming solution quality.
  - **VRPTW**:
    - Standard single-depot VRPTW with time windows for each customer, capacity constraints, and an ESPPRC-based subproblem.
    - Uses Gehring & Homberger benchmarks for 200- and 400-customer problems.
    - Compares performance with a well-known reduced-cost-based arc filtering strategy. Shows that ML-based filtering competes well and can yield further improvements, especially for randomly distributed customer instances.

- **Results & Performance**
  - Computational experiments show up to **40%** (and sometimes more) reduction in total column generation time.
  - Greatest speedups occur when the subproblem (pricing) is the computational bottleneck.
  - Performance depends on instance structure: for randomly distributed customers (R2 category), strong gains; for clustered customers (C2, RC2), the approach still reduces time but can lead to more CG iterations.
  - Combination with classical reduced-cost arc filtering can further enhance performance in some cases.

- **Strengths**
  - Demonstrates a **data-driven** way to cut down pricing problem complexity.
  - Potentially generalizable to other CG-based routing or scheduling formulations.
  - Straightforward integration with existing CG pipelines: only the subproblem network changes, no major modeling overhaul.

- **Limitations & Observations**
  - Partial reliance on prior runs for label assignment: collecting data requires solving or partially solving similar instances beforehand.
  - The method might increase the number of total CG iterations, but often the PP time savings offset this.
  - Results can vary based on instance characteristics (clustered vs. random geographies).

- **Relevance & Impact on Deterministic VRP Research**
  - Confirms the promise of **imitation learning** to reduce computational overhead in exact or near-exact VRP solutions under deterministic conditions.
  - Offers an efficient synergy between established OR techniques (column generation) and modern ML (supervised classification).
  - Potential model for future ML-driven dimension-reduction strategies in VRPs, especially in large-scale or time-window settings.