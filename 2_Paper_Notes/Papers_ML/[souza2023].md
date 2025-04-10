# A robust algorithm based on Differential Evolution with local search for the Capacitated Vehicle Routing Problem

## Metadata
- **Link to PDF**: [[[souza2023]_A_robust_algorithm_based_on_Differential_Evolution_with_local_search_for_the_Capacitated_Vehicle_Routing_Problem.pdf]]
- **Tags**:
  - #DeterministicVRP
  - #CVRP
  - #PopulationBasedEvolution
  - #LocalSearchHeuristics
  - #BenchmarkTesting
  - #DifferentialEvolution
- **Relevant**: false  
- **Fit Score**: 2  
- **State of the Art (SoA) Concepts**:
  - Discrete Differential Evolution (DE) metaheuristic for CVRP
  - Hybridization with local search (swapping, drop-one-point)
  - Benchmarking on classical CVRP instances
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: CVRPLIB  

## Abstract
"The Capacitated Vehicle Routing Problem is a well-known combinatorial problem. In this paper, we propose a hybrid algorithm based on a discrete adaptation of the Differential Evolution metaheuristic, which is designed for continuous problems, combined with local search procedures to solve CVRP. An individual in the algorithm represents a CVRP solution. The algorithm presents a discrete adaptation for DE that consists on exchanging customers considering their routes positions, instead of arithmetically modifying the individuals. The proposed algorithm, called CDELS, is compared with three state-of-the-art algorithms. The computational experiments are performed on six classical datasets, with an extensive study for parameter tuning. A statistical analysis of the results was also conducted. It showed that the CDELS algorithm is highly significantly better than state-of-the-art methods with a confidence level of 99%. CDELS is open-source."

## Summary
- **Core Idea & Motivation**
  - The paper addresses the Capacitated Vehicle Routing Problem (CVRP) and presents a hybrid metaheuristic called CDELS (Combinatorial Differential Evolution with Local Search).
  - It adapts the classic Differential Evolution (DE) operators, originally for continuous domains, to a discrete problem setting.
  - Local search methods (swapping, drop-one-point) further refine solutions.

- **Methodology**
  - **Representation**: Each CVRP solution is encoded as a matrix where rows correspond to routes and columns to positions of customers in those routes.
  - **Discrete Differential Evolution**: 
    - Mutation is reinterpreted to exchange the positions of customers rather than applying arithmetic differences.
    - Crossover similarly swaps customer positions based on a probability threshold.
  - **Local Search**:
    - A set of route-improvement heuristics (Swapping, Strong Drop One Point, and Drop One Point Infeasible) is applied to each newly generated solution to reduce route cost or restore feasibility.
  - **Evaluation & Selection**:
    - Solutions are scored by total travel distance, with penalization if capacity constraints are violated.
    - A standard DE selection mechanism chooses between trial and target solutions for the next generation.

- **Experimental Setup**
  - Compared against three other well-known CVRP algorithms (HGA-VNS, MGV-CP, and 2PH) on six datasets from CVRPLIB (A, B, P, E, F, M).
  - Conducted an extensive parameter tuning for the DE factors (F and CR).
  - Reported computational time, number of optimum solutions found, and statistical tests (Friedman and Wilcoxon) to assess significance.

- **Key Results & Findings**
  - CDELS achieved high success rates in matching known optimal solutions, especially on smaller and medium-sized instances in sets A, B, E, F, and P.
  - The method struggled slightly on some larger instances (M-n200), but remained competitive overall.
  - Statistical analysis suggested that CDELS outperformed or equaled most compared methods under a 99% confidence level for the tested sets.

- **Strengths, Weaknesses & Limitations**
  - **Strengths**:
    - Simple adaptation of DE for discrete routing with well-integrated local search, yielding robust performance.
    - Thorough parameter tuning to ensure stable outcomes.
    - Comprehensive benchmarks and rigorous statistical validation.
  - **Weaknesses/Limitations**:
    - The approach is a standard population-based metaheuristic, not leveraging any data-driven or advanced ML paradigm.
    - Performance can degrade on very large-scale instances (e.g., >200 customers).
  - **Scalability** remains partly dependent on large generation counts and population sizes.

- **Relevance to Deterministic VRP and Machine Learning**
  - The paper focuses on a deterministic CVRP variant and proposes a pure metaheuristic approach rather than a data-driven or learning-based model.
  - While Differential Evolution is an evolutionary technique, it is not applied here in a supervised or reinforcement learning context. Consequently, it does not align with typical ML-based methods that learn from external data.
  - May still be cited as a reference point for classical metaheuristic performance on deterministic CVRP but not for ML-based solution frameworks.