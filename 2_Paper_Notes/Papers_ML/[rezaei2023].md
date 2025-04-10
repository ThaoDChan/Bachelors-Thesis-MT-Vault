# Combining genetic local search into a multi-population Imperialist Competitive Algorithm for the Capacitated Vehicle Routing Problem

## Metadata
- **Link to PDF**: [[[rezaei2023]_Combining_genetic_local_search_into_a_multi-population_Imperialist_Competitive_Algorithm_for_the_Capacitated_Vehicle_Routing_Problem.pdf]]
- **Tags**: 
  #DeterministicVRP 
  #CVRP 
  #HybridLocalSearch 
  #GeneticAlgorithm 
  #PopulationBasedEvolution 
  #HeuristicOrExactBlends 
  #LocalSearchHeuristics 
  #LargeScaleVRP 
  #RealWorldData 
  #ParallelImplementation
- **Relevant**: false  
- **Fit Score**: 2  
- **State of the Art (SoA) Concepts**:
  - Hybrid metaheuristic combining Imperialist Competitive Algorithm (ICA) and Genetic Local Search  
  - Deterministic CVRP with multi-population approach  
  - Parallelizable evolutionary structures  
  - Real-world dataset (LoggiBUD) in addition to classical CVRP benchmarks
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: Uchoa benchmark, CMT, Golden, LoggiBUD

## Abstract
"The Vehicle Routing Problem (VRP) is one of the most significant problems in operational research today. VRP has a vast range of application fields such as transportation, logistics, manufacturing, relief systems and communication. To suit the needs of different real-world VRP scenarios, many models of VRP have been developed - CVRP (Capacitated VRP) being the classical form. In this article, a hybrid metaheuristic algorithm, ICAHGS, is proposed for solving CVRP. The present study proposes a refined Imperialist Competitive Algorithm (ICA) as the primary evolutionary and multipopulation method for addressing the Capacitated Vehicle Routing Problem (CVRP). In order to further optimize the search process, a Hybrid Genetic Search (HGS-CVRP) algorithm is applied as an enhanced local search and population management strategy within the ICA framework. Additionally, the internal restart step of the HGS-CVRP algorithm is replaced with a multi-step restart mechanism for intensification improvement. One notable aspect of the proposed method is its ability to facilitate parallel processing, with each empire able to be processed on a separate processor. This structure allows for increased computational efficiency in addressing the CVRP. To assess the effectiveness of the proposed algorithm, it has been compared to several state-of-the-art algorithms from the literature. The results of this comparison, which include both classical benchmark instances and real-world applications, demonstrate the competitive performance of the proposed algorithm."

## Summary
- **Purpose & Context**
  - Introduces a hybrid algorithm (ICAHGS) for solving the deterministic Capacitated Vehicle Routing Problem (CVRP).
  - Aims to combine the Imperialist Competitive Algorithm (ICA) — a population-based, multi-population evolutionary technique — with an advanced genetic local search (HGS-CVRP) to improve search intensification and population management.
  - Incorporates a multi-step restart mechanism to diversify solutions and speed up convergence.

- **Methodology**
  - **Imperialist Competitive Algorithm (ICA)**:
    - Uses “empires” composed of an imperialist (best solution) and colonies (remaining solutions).
    - Periodically updates (assimilates) solutions by moving colonies toward better solutions and mutates (revolves) them.
    - Conducts a competition step where weaker empires lose colonies to more successful ones.
  - **Refinements**:
    - Assimilation and mutation (revolution) steps are merged into one step to reduce overhead.
    - The position-swapping step between an imperialist and its better colony is removed, replaced by a sorted list representation.
    - Imperialist competition is randomized (instead of focusing purely on the most powerful empire).
  - **Hybrid Genetic Local Search (HGS-CVRP)**:
    - Incorporates route-improvement neighborhoods (relocate, swap, 2-opt, and enhanced SWAP*).
    - Manages feasible and infeasible solutions separately, adapting penalty parameters for capacity violations.
    - Uses partial restarts to exploit promising areas without fully resetting the solution space.
  - **Multi-Step Restart Mechanism**:
    - Replaces the single-step restart in HGS-CVRP with a two-level approach (partial or total restarts) to maintain diversity and intensify local searches around strong solutions.

- **Experiments & Results**
  - Tested on classic CVRP benchmarks (CMT, Golden, Uchoa) plus a real-world dataset (LoggiBUD).
  - Achieves competitive or superior results compared to baseline ICA and HGS-CVRP alone, often matching or improving best-known solutions on large instances.
  - Parallel implementation possibility: each empire can be processed on a separate core, accelerating runtime.
  - Demonstrates strong performance on large-scale test problems, improving solution quality and sometimes reducing runtime compared to HGS-CVRP alone.

- **Strengths, Weaknesses, Limitations**
  - **Strengths**:
    - Effectively combines multi-population structure (ICA) with a powerful local search (HGS-CVRP).
    - Demonstrates improvements in convergence speed and solution quality on standard benchmarks.
    - Capable of partial restarts, avoiding the cost of re-initializing from scratch.
    - Parallelizable design leverages multi-core computation for speed.
  - **Weaknesses**:
    - Relies on numerous parameters (revolution rate, population size, assimilation mechanism) which may require careful tuning.
    - May not generalize easily to complex VRP variants with additional constraints (time windows, heterogeneous fleets, etc.) without further adaptation.
  - **Limitations**:
    - Does not incorporate machine learning from data; remains a classical metaheuristic approach.
    - Addresses primarily the deterministic CVRP with fixed capacities and no stochastic elements.

- **Relevance to Deterministic VRP & ML**
  - Focuses purely on deterministic CVRP. Benchmarks used are also deterministic.
  - Does not use learning-based methodologies (e.g., supervised, RL); the approach is purely a metaheuristic that does not train on external datasets.
  - Although it improves solution quality in deterministic settings, it does not contribute to the machine-learning-based taxonomy.

- **Potential Impact on Broader Research**
  - Illustrates how multi-population evolutionary strategies can be hybridized with local search for classical VRP.
  - Could guide future extensions of population-based heuristics, though it is not strictly an ML-driven approach.
  - The partial-restart method and parallel design might be adapted in hybrid ML-heuristic frameworks but the paper itself remains in the domain of evolutionary methods, rather than data-driven learning.