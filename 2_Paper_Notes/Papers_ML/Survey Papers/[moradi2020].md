# The new optimization algorithm for the vehicle routing problem with time windows using multi-objective discrete learnable evolution model

## Metadata
- **Link to PDF**: [[[moradi2020]_The_new_optimization_algorithm_for_the_vehicle_routing_problem_with_time_windows_using_multi-objective_discrete_learnable_evolution_model.pdf]]
- **Tags**:
  - #DeterministicVRP
  - #VRPTW
  - #TimeWindowConstraints
  - #MultiObjective
  - #DecisionTreeML
  - #PopulationBasedEvolution
  - #ML-AssistedHeuristics
  - #HeuristicOrExactBlends
- **Relevant**: true
- **Fit Score**: 9
- **State of the Art (SoA) Concepts**:
  - Multi-objective VRPTW (Vehicle Routing Problem with Time Windows)
  - Learnable Evolution Model (LEM) with decision-tree-based learning
  - SPEA2? (Strength Pareto Evolutionary Algorithm)
  - Hybrid ML-assisted metaheuristics
- **Performance Evaluation**: yes
- **Performance Evaluation Framework**: Solomon’s VRPTW benchmark

## Abstract
"This paper presents a new multi-objective discreet learnable evolution model (MODLEM) to address the vehicle routing problem with time windows (VRPTW). Learnable evolution model (LEM) includes a machine learning algorithm, like the decision trees, that can discover the correct directions of the evolution leading to significant improvements in the fitness of the individuals. We incorporate a robust strength Pareto evolutionary algorithm in the LEM presented here to govern the multi-objective property of this approach. A new priority-based encoding scheme for chromosome representation in the LEM as well as corresponding routing scheme is introduced. To improve the quality and the diversity of the initial population, we propose a novel heuristic manner which leads to a good approximation of the Pareto fronts within a reasonable computational time. Moreover, a new heuristic operator is employed in the instantiating process to confront incomplete chromosome formation. Our proposed MODLEM is tested on the problem instances of Solomon's VRPTW benchmark. The performance of this proposed MODLEM for the VRPTW is assessed against the state-of-the-art approaches in terms of both the quality of solutions and the computational time. Experimental results and comparisons indicate the effectiveness and efficiency of our proposed intelligent routing approach."

## Summary
- **Purpose & Context**  
  - Addresses a deterministic Vehicle Routing Problem with Time Windows (VRPTW), where the objectives are:
    1. Minimize the number of vehicles employed  
    2. Minimize total traveled distance  
  - Introduces a **multi-objective discrete learnable evolution model (MODLEM)** to improve solution quality and reduce computational time.  
  - Operates under deterministic conditions (fixed, known demands and time windows).

- **Key Innovation**  
  - Proposes a **non-Darwinian evolutionary** framework called Learnable Evolution Model (LEM). Unlike standard genetic or evolutionary algorithms, LEM uses **decision-tree-based induction** to guide the generation of new solutions (“hypotheses”), thereby avoiding semi-blind random variation.  
  - Integrates **SPEA2?** (an advanced Strength Pareto Evolutionary Algorithm) to manage the multi-objective aspect, ensuring both objectives are optimized simultaneously.  
  - Employs **heuristic operators** to:
    - Generate an initial population with broad coverage (improving diversity)
    - Instantiate new solutions in a more targeted manner (reducing incomplete or infeasible solution generation).

- **Methodology**  
  1. **Chromosome Representation**:
     - Uses a **priority-based** (indirect) encoding scheme. Each gene represents a customer’s priority, rather than directly listing customers in route order. This allows a decoding mechanism that builds routes incrementally based on higher priority first, reducing wasted capacity or early route termination.
  
  2. **Initialization**:
     - Combines random chromosome generation with a **stochastic push-forward insertion heuristic** (adapted from Solomon’s insertion logic) to seed the population with diverse and partially optimized solutions.
  
  3. **Learning Phase (LEM)**:
     - Divides solutions into high-performance (H-group) and low-performance (L-group) sets based on multi-objective fitness.
     - A **decision tree** (C4.5/J48) is induced to extract priority “rules” that contributed to high or low quality. This yields hypotheses about which ranges of priority values are promising.
  
  4. **Instantiation / Offspring Generation**:
     - Employs a heuristic technique to instantiate new chromosomes satisfying the high-performance rules:
       - Chooses customers with the smallest “freedom degree” (shortest allowed priority range) first
       - Assigns the feasible priority values with lowest “presence degree” (minimal conflicts) next
       - This approach greatly reduces incomplete chromosome formation or infeasible solutions.
  
  5. **SPEA2? Fitness Evaluation**:
     - Uses non-dominated sorting, a strength measure, and a distance metric to maintain both convergence and diversity in the Pareto set.  
     - Maintains and updates two archive sets of non-dominated solutions, applying truncation if they exceed a size limit.

- **Experiments & Findings**  
  - Benchmarked on **Solomon’s VRPTW** problems (56 instances separated into 6 categories: C1, C2, R1, R2, RC1, RC2).  
  - **Results** showed that:
    - MODLEM obtains solutions close or equal to many established best-known results, frequently matching known optima in the clustered benchmarks (C1, C2).
    - On more challenging R-series and RC-series benchmarks, MODLEM yields high-quality solutions (either matching or improving on prior multi-objective methods in terms of the number of vehicles, total distance, or both).
    - The approach demonstrates **notable runtime efficiency** compared to certain state-of-the-art metaheuristics (e.g., Tabu-ABC) due to its guided, non-random search step.
    - The heuristic generation of initial and new solutions significantly shortens time to convergence.

- **Strengths**  
  - **Reduced randomness**: Inductive (decision-tree) learning ensures more direct improvement by focusing on promising solution “regions.”  
  - **Multi-objective** synergy: SPEA2? effectively balances conflicting objectives.  
  - **Efficient**: Outperforms or equals other advanced metaheuristics in many test cases while maintaining reasonable computational times.  
  - **Generic structure**: Though specialized for VRPTW, the framework can be adapted to other combinatorial optimization problems.

- **Weaknesses & Limitations**  
  - **Problem-specific heuristics**: The approach relies on specialized route-construction heuristics and a domain-focused instantiation strategy (harder to generalize across drastically different VRP variants).  
  - **Parameter tuning** is mostly offline; real-world usage might require adaptive or automated parameter selection.  
  - **No direct local search**: The method focuses on evolutionary generation and systematic learning, but synergy with local improvement strategies might further refine solutions.

- **Relevance to Deterministic VRP & ML**  
  - Demonstrates a **machine learning-based** technique (decision trees) integrated with an evolutionary search for a **deterministic** VRPTW.  
  - Serves as a strong example of combining metaheuristics with data-driven rules to guide the search.  
  - Highlights how learning from previous generations can speed up multi-objective optimization and yield near-optimal solutions on established benchmarks.

- **Potential Impact on Thesis**  
  - Provides an instance of a **ML-Assisted Heuristic** approach in VRPTW, reinforcing the argument that data-driven or learning-based guidance can outperform purely random genetic operators.  
  - Contrasts with more typical Darwinian genetic algorithms, clarifying that specialized knowledge extraction can reduce search iterations.  
  - May prompt future extension: adopting online parameter tuning or mixing local search with the learnable model to potentially handle large-scale or real-time VRP scenarios.