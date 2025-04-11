# Machine Learning-Based Parameter Tuned Genetic Algorithm for Energy Minimizing Vehicle Routing Problem

## Metadata
- **Link to PDF**: [[[cooray2017]_Machine_Learning-Based_Parameter_Tuned_Genetic_Algorithm_for_Energy_Minimizing_Vehicle_Routing_Problem.pdf]]
- **Tags**:
  - #DeterministicVRP  
  - #GreenVRP  
  - #EnergyMinimizingVRP  
  - #ML-Unsupervised  
  - #ML-Clustering  
  - #ML-AssistedHeuristics  
  - #ParameterTuning  
  - #GeneticAlgorithm  
  - #PopulationBasedEvolution  
  - #CVRP  
  - #VRPBenchmarks  
- **Relevant**: true  
- **Fit Score**: 9  
- **State of the Art (SoA) Concepts**:
  - Energy Minimizing VRP (EMVRP) as a sub-problem of Green VRP
  - Genetic Algorithm metaheuristics
  - k-means clustering for parameter tuning
  - Use of standard VRP benchmarks (CVRPLib)
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: CVRPLib  

## Abstract
“During the last decade, tremendous focus has been given to sustainable logistics practices to overcome environmental concerns of business practices. Since transportation is a prominent area of logistics, a new area of literature known as Green Transportation and Green Vehicle Routing has emerged. Vehicle Routing Problem (VRP) has been a very active area of the literature with contribution from many researchers over the last three decades. With the computational constraints of solving VRP which is NP-hard, metaheuristics have been applied successfully to solve VRPs in the recent past. This is a threefold study. First, it critically reviews the current literature on EMVRP and the use of metaheuristics as a solution approach. Second, the study implements a genetic algorithm (GA) to solve the EMVRP formulation using the benchmark instances listed on the repository of CVRPLib. Finally, the GA developed in Phase 2 was enhanced through machine learning techniques to tune its parameters. The study reveals that, by identifying the underlying characteristics of data, a particular GA can be tuned significantly to outperform any generic GA with competitive computational times. The scrutiny identifies several knowledge gaps where new methodologies can be developed to solve the EMVRPs and develops propositions for future research.”

## Summary
- **Context and Motivation**  
  - The paper addresses a growing concern for sustainable logistics, focusing on Green Vehicle Routing Problems (GVRP). Specifically, it tackles the Energy Minimizing VRP (EMVRP), where energy consumption is modeled as the product of load and distance traveled.  
  - The authors note that while EMVRP can significantly reduce carbon emissions, large and moderate-sized problem instances are challenging to solve optimally using exact solvers (CPLEX), motivating a metaheuristic approach.

- **Objective**  
  - Propose and implement a Genetic Algorithm (GA) to solve the EMVRP using benchmark data from CVRPLib.  
  - Enhance the GA by tuning parameters (e.g., mutation rate) with machine learning techniques, specifically an unsupervised k-means clustering algorithm.

- **Methodology**  
  1. **Systematic Literature Review**: The paper surveys existing work on Green VRPs and EMVRP, highlighting the gap in solution approaches that combine sustainability and optimization.  
  2. **GA Implementation**:  
     - A classical GA structure is used (population initialization, tournament selection, crossover, mutation, elitism).  
     - The objective function minimizes the sum of load × distance across all routes.  
     - Baseline parameters for the GA (population size, mutation rate, etc.) are chosen via a design of experiments.  
  3. **ML-Based Parameter Tuning**:  
     - Data instances from CVRPLib are clustered via k-means into groups based on total demand and number of cities.  
     - Different mutation rates are empirically tested for each cluster, identifying the rate that yields the best energy reduction for that cluster.  
     - This cluster-specific GA parameterization is compared against a generic “one-size-fits-all” setting.

- **Experimental Setup and Results**  
  - **Datasets**: 100 CVRPLib benchmark instances, each with varying demands, capacities, and city counts.  
  - **Baseline GA Performance**: Achieves up to ~30–50% energy reduction (compared to an initial random route baseline), within reasonable computational times (seconds to minutes per instance).  
  - **Tuned GA with Clustering**:  
    - Shows further improvement in energy reduction. Each cluster of problem instances benefits from a distinct optimal mutation rate.  
    - The approach demonstrates that leveraging underlying instance characteristics (total number of cities, cumulative demand) leads to more effective parameter settings, ultimately improving solution quality.  

- **Key Findings**  
  - EMVRP can be effectively solved by GA-based metaheuristics, showing competitive energy reductions and moderate computation times for medium to large instances.  
  - Machine learning (specifically unsupervised clustering) can systematically identify better parameter choices for GA, outperforming a generic single-parameter approach.  
  - The authors highlight the novelty of combining metaheuristic tuning with ML in the VRP domain, noting that prior studies have seldom exploited instance-based clustering for parameter selection.

- **Strengths and Limitations**  
  - **Strengths**:  
    - Comprehensive experiment across 100 problem instances.  
    - Demonstrates a clear improvement in solution quality via ML-based parameter tuning.  
    - Integrates GVRP objectives into classical VRP benchmarks, highlighting environmental metrics (energy).  
  - **Limitations**:  
    - Focuses on a single metaheuristic (GA). Comparisons with other metaheuristics (Tabu Search, ACO, etc.) are suggested but not performed.  
    - The study uses load × distance as the sole energy measure and does not incorporate vehicle weight or advanced emission models.  
    - Could be extended to more advanced ML methods or more nuanced problem constraints (e.g., multi-depot, time windows).

- **Relevance for Deterministic VRP and ML**  
  - Clearly a deterministic VRP variant (no stochastic elements in demands or travel times).  
  - Demonstrates an unsupervised approach (k-means) to tune GA parameters, aligning well with the thesis objective of applying ML to deterministic VRP.  
  - Showcases potential for future expansions, including hybridizing with other ML or heuristic approaches and exploring additional environmental or operational metrics.

- **Potential Impact on Broader Research**  
  - Establishes a reference point for how unsupervised learning can guide parameter selection in metaheuristics for VRP.  
  - Reinforces the idea that data-driven customization of algorithms to problem-instance features can yield superior performance over “one-size-fits-all” settings.  
  - Encourages further exploration of integrating advanced ML (e.g., meta-learning, advanced clustering) with VRP optimization, especially in green logistics.