# OPTIMIZATION OF MULTIPLE VEHICLE ROUTING PROBLEMS USING APPROXIMATION ALGORITHMS

## Metadata
- **Link to PDF**: [[[nallusamy2010]_Optimization_of_Multiple_Vehicle_Routing_Problems_using_Approximation_Algorithms.pdf]]
- **Tags**:
  - #DeterministicVRP
  - #ML-Unsupervised
  - #ML-Clustering
  - #ClusterFirstRouteSecond
  - #MultiVehicleRouting
  - #GeneticAlgorithm
  - #TabuSearch
  - #SimulatedAnnealing
  - #LocalSearchHeuristics
  - #PopulationBasedEvolution
- **Relevant**: true  
- **Fit Score**: 7  
- **State of the Art (SoA) Concepts**:
  - K-means clustering as a clustering-first approach for VRP
  - Genetic algorithms for route optimization
  - Comparisons with Tabu Search and Simulated Annealing
- **Performance Evaluation**: no  
- **Performance Evaluation Framework**: -

## Abstract
"This paper deals with generating an optimized route for multiple Vehicle Routing Problems (mVRP). We used a methodology of clustering the given cities depending upon the number of vehicles and each cluster is allotted to a vehicle. k-Means clustering algorithm has been used for easy clustering of the cities. In this way the mVRP has been converted into VRP which is simple in computation compared to mVRP. After clustering, an optimized route is generated for each vehicle in its allotted cluster. Once the clustering had been done and after the cities were allocated to the various vehicles, each cluster/tour was taken as an individual Vehicle Routing problem and the steps of Genetic Algorithm were applied to the cluster and iterated to obtain the most optimal value of the distance after convergence takes place. After the application of the various heuristic techniques, it was found that the Genetic algorithm gave a better result and a more optimal tour for mVRPs in short computational time than other Algorithms due to the extensive search and constructive nature of the algorithm."

## Summary
- **Purpose and Context**  
  - Addresses the Multiple Vehicle Routing Problem (mVRP), a variant of VRP where multiple vehicles start from a single depot and must visit a distinct subset of cities.  
  - Proposes a two-stage approach to reduce complexity: (1) cluster all cities using k-means, (2) solve each resulting cluster as a single VRP with a Genetic Algorithm (GA).

- **Methodology**  
  1. **k-Means Clustering**  
     - Divides 180 cities into 6 clusters based on geographical proximity (random 2D coordinates).  
     - Reduces the original large problem into multiple smaller subproblems; each subproblem is then assigned to a vehicle.  
  2. **Genetic Algorithm (GA)**  
     - Applied separately to each cluster to find an optimized route within that cluster (vehicle’s tour).  
     - Employs a standard GA procedure: initialization with random routes, fitness proportional to inverse of route length, partially matched crossover, and mutation.  
     - Convergence measured through iterative improvement in route distance.

- **Key Results**  
  - GA provided shorter routes compared to Tabu Search and Simulated Annealing when tested on the partitioned subproblems.  
  - For instance, in sample clusters, GA’s total distance consistently outperformed the other heuristics, emphasizing GA’s strong exploitation capabilities.  
  - Demonstrates that a “cluster-first, route-second” approach can effectively scale to larger VRP instances, reducing computational complexity.

- **Strengths**  
  - Uses an unsupervised learning algorithm (k-means) to simplify a large VRP instance; easy to implement.  
  - Demonstrates meta-heuristic synergy: clustering followed by GA.  
  - Clear comparative results against Tabu Search and Simulated Annealing.

- **Limitations and Gaps**  
  - Clustering is purely distance-based; does not account for balancing vehicle workloads or capacity constraints.  
  - GA convergence can be slow if the number of cities in a cluster is large; risk of local optima remains.  
  - Randomly generated city data does not benchmark results against standard VRP libraries.

- **Relevance for Deterministic VRP Research**  
  - Showcases an approach for mVRP under deterministic conditions, aligning with methods that blend clustering and evolutionary heuristics.  
  - Highlights the potential of basic unsupervised algorithms (k-means) to reduce problem dimensionality, followed by advanced meta-heuristics (GA).  
  - Could inform future studies seeking modular or hybrid solutions, especially for multi-depot or larger-scale deterministic contexts.