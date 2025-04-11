# A new geometric shape-based genetic clustering algorithm for the multi-depot vehicle routing problem

## Metadata

- **Link to PDF**: [[[yucenur2011]_A_new_geometric_shape-based_genetic_clustering_algorithm_for_the_multi-depot_vehicle_routing_problem.pdf]]
- **Tags**:  
  #DeterministicVRP  
  #MultiDepotVRP  
  #ClusterFirstRouteSecond  
  #GeneticAlgorithm  
  #ML-Clustering  
  #ML-AssistedHeuristics  
  #PopulationBasedEvolution  
  #ParameterTuning  
- **Relevant**: true  
- **Fit Score**: 7 (out of 10)  
- **State of the Art (SoA) Concepts**:
  - Genetic algorithms for clustering in VRP
  - Two-phase approach (cluster-first, route-second)
  - Multi-depot VRP
  - Comparison with nearest neighbor heuristic
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: Uses 23 benchmark-like sets from literature (Renaud, Laporte, etc.), but not standard sets like Solomon or Christofides.  

## Abstract
“In this paper, a new type of geometric shape based genetic clustering algorithm is proposed. A genetic algorithm based on this clustering technique is developed for the solution process of the multi-depot vehicle routing problem. A set of problems obtained from the literature is used to compare the efficiency of the proposed algorithm with the nearest neighbor algorithm so as to solve the multi-depot vehicle routing problem. The experimental results show that the proposed algorithm provides a better clustering performance in terms of the distance of each customer to each depot in clusters. This result in a considerably less computation time required, when compared with the nearest neighbor algorithm.”  

## Summary
- **Context & Motivation**  
  - Addresses the multi-depot vehicle routing problem (MDVRP), which involves assigning customers to multiple depots while minimizing travel distance.  
  - MDVRP is an NP-hard variant of the general VRP, making heuristic/metaheuristic methods attractive.  
  - This study proposes a novel **geometric shape-based genetic clustering** approach: circles of random radii drawn from each depot cluster the customers.

- **Methodology**  
  - Uses a **cluster-first, route-second** approach. The authors develop a genetic algorithm (GA) that *initially clusters* customers to depots using randomly generated circles around each depot.  
  - Chromosome representation: Each depot is associated with a circle of random radius. If a customer lies inside or on the circle boundary, it is assigned to that depot; if outside all circles, the nearest circle boundary (by Euclidean distance) determines its assignment.  
  - GA operators:
    - **Binary coding** of solution parameters.  
    - **Selection**: Roulette wheel method based on fitness (distance minimization).  
    - **Crossover**: Cyclical crossover that cyclically swaps genes between parent strings.  
    - **Mutation**: Swaps two adjacent genes randomly.  
  - After clustering, the standard VRP route planning can be solved by a separate step (though the paper’s core focus is the clustering step).  

- **Key Findings**  
  - The authors test the approach on 23 MDVRP instances from the literature (ranging from 50 to 360 customers and 2–9 depots).  
  - Compare the new GA-based clustering to the nearest neighbor (NN) assignment strategy.  
  - The best solutions from the GA-based clustering match those from nearest neighbor in all instances but require “less computation time” (qualitatively stated).  
  - A **parametric study** on crossover rate (60% vs. 3%) shows that a sufficiently high crossover rate is critical to achieving correct cluster assignments.  

- **Strengths**  
  - Proposes a unique geometric (circle-based) random approach to cluster customers for multi-depot VRPs.  
  - Demonstrates that combining geometric clustering with genetic operators can be both accurate (matching nearest neighbor solutions) and efficient.  
  - Provides parametric analysis, highlighting the impact of GA parameters (e.g., crossover rate).  

- **Limitations**  
  - The paper focuses primarily on the clustering component; routing is implied but not deeply elaborated in terms of advanced route optimization.  
  - The proposed technique is still a metaheuristic approach reliant on parameters (population size, mutation/crossover rates), which must be tuned.  
  - Evaluation mainly uses custom MDVRP datasets from the literature; does not employ standard CVRPLIB or widely known large-scale sets, though it references recognized multi-depot instances.  

- **Relevance & Impact**  
  - Shows how GAs can be integrated in a cluster-first, route-second framework for **deterministic** multi-depot VRPs.  
  - Underlines the importance of **clustering** as a pre-processing step, which can significantly reduce overall computational effort for VRP.  
  - Serves as an example of **metaheuristic-based clustering** that can be extended or combined with other heuristics for the route step, offering synergy in large-scale or multi-depot distribution tasks.  
  - This approach aligns with the broader theme of ML-based or metaheuristic solutions to VRPs, highlighting genetic algorithms’ adaptability and the benefits of geometric classification structures.