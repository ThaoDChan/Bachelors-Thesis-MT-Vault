# Hybrid Algorithms for Energy Minimizing Vehicle Routing Problem: Integrating Clusterization and Ant Colony Optimization

## Metadata
- **Link to PDF**: [[[frias2023]_Hybrid_Algorithms_for_Energy_Minimizing_Vehicle_Routing_Problem-Integrating_Clusterization_and_Ant_Colony_Optimization.pdf]]
- **Tags**: 
  #ML-Unsupervised 
  #ML-Clustering 
  #ML-Hybrid 
  #GreenVRP 
  #CVRP 
  #AntColonySystem 
  #LocalSearchHeuristics 
  #HeuristicOrExactBlends 
  #VRPBenchmarks 
  #DeterministicVRP
- **Relevant**: true  
  - *Reasoning*: The paper develops a deterministic VRP approach (energy-minimizing VRP) with unsupervised ML (K-Means/K-Medoids) combined with an ACO-based metaheuristic, aligning with the thesis focus on deterministic ML-based VRP.
- **Fit Score**: 9
- **State of the Art (SoA) Concepts**:
  - EMVRP (energy-oriented, “green” VRP variant)
  - Capacitated VRP structures
  - Ant Colony Optimization (BWAS, MAX-MIN variants)
  - K-Means and K-Medoids clustering for route pre-processing
  - Local search (VNS) integration
- **Performance Evaluation**: yes
- **Performance Evaluation Framework**: CVRPLIB (Augerat, Christofides et al., Golden et al.)

## Abstract
“In the field of engineering, complex problems often arise that require solutions. The implementation of these algorithms plays a crucial role in achieving favorable outcomes with the available resources. The Vehicle Routing Problem (VRP) has been a central topic in distribution and logistics for decades. New VRP models and tools are developed to address the challenges of modern logistics. The Energy Minimizing Vehicle Routing Problem (EMVRP) is a ‘green’-oriented variant of the VRP where the objective is to minimize the total amount of energy consumed by a fleet of vehicles. The VRP literature has focused on solving the problem using a variety of approaches and techniques, including exact methods, heuristics, metaheuristics, and hybrid algorithms. Hybrid algorithms combine different techniques to obtain more effective and better solutions. This work presents four innovative hybrid algorithms to address the EMVRP problem. These algorithms combine Machine Learning (ML) clustering techniques with metaheuristic approaches inspired by an Ant Colony Optimization (ACO). The proposed algorithms are: Free Ant + K-Means, Free Ant + K-Medoids, Restricted Ant + K-Means, and Restricted Ant + K-Medoids. Each of them combines the benefits of clustering with the optimization capacity of ACO. Proposed algorithms were subjected to testing using instances from CVRPLIB. Both Free Ant and Restricted Ant efficiently solved EMVRP problems. The results obtained were analyzed and compared with the proposals of other authors in the literature. Overall, the results are promising, but they also indicate a significant scope for experimentation and parameter tuning of the proposed algorithms.”

## Summary
- **Purpose & Problem**  
  - Addresses the Energy Minimizing Vehicle Routing Problem (EMVRP), a “green” VRP variant emphasizing energy reduction rather than simple distance.  
  - Seeks to merge clustering-based strategies with Ant Colony Optimization (ACO) metaheuristics to solve EMVRP more efficiently.

- **Methodology & ML Approach**  
  - Uses **unsupervised learning** (K-Means and K-Medoids) to cluster customer nodes under a vehicle capacity constraint.  
  - Proposes two main ACO-based metaheuristics: **Free Ant** (ants choose any node probabilistically) and **Restricted Ant** (ants operate within fixed clusters).  
  - **Free Ant** initializes pheromones based on within-cluster arcs but still allows crossing clusters.  
  - **Restricted Ant** decomposes the problem into smaller TSP-like subproblems per cluster. Each group’s best solution is merged at the end.  
  - Incorporates local search via **Variable Neighborhood Search (VNS)** to refine solutions (with single-route and multi-route perturbation operators).

- **Algorithms & Key Innovations**  
  - **Hybrid ACO + Clustering**: Both K-Means and K-Medoids cluster assignments feed the initial pheromone matrix or restrict the solution space.  
  - Two distinct ACO variants:
    1. **Free Ant**: Explores the entire node set but leverages cluster-based pheromone initialization.  
    2. **Restricted Ant**: Strictly assigns nodes to clusters (sub-tours), focusing on smaller TSP subproblems.  
  - Introduces the concept of mixing distance, energy consumption, and savings (Clarke-Wright’s saving metric) into a single heuristic for ACO.

- **Experimental Setup**  
  - Benchmarked on known CVRPLIB sets:  
    - “Set A” (Augerat),  
    - Christofides, Mingozzi & Toth,  
    - Golden et al.  
  - Compared solutions against existing EMVRP references (ML-TS and ILS-SP-SOA).  
  - Evaluated solution quality (energy objective) and runtime.

- **Results & Findings**  
  - **Free Ant** typically finds better energy-minimizing solutions but takes more runtime, especially for larger instances.  
  - **Restricted Ant** is faster due to subproblem decomposition but can converge to higher-cost local optima.  
  - The clustering approach (K-Means vs. K-Medoids) affects the initial routes and pheromone distribution: both improved solution quality relative to pure ACO, but K-Medoids sometimes yields slightly better clusters in certain instances.  
  - Overall solutions are competitive with earlier EMVRP metaheuristics, showing promise but not always matching the best-known references on large-scale sets.

- **Strengths**  
  - Showcases an effective **hybrid** design that unites ML-based clustering with a flexible metaheuristic.  
  - Explores two distinct ways (Free vs. Restricted) to incorporate cluster knowledge into ACO.  
  - Conducts thorough tests on standard VRP benchmarks, demonstrating reproducibility and direct comparisons.

- **Limitations & Observations**  
  - Parameter tuning is critical (pheromone bounds, number of iterations, etc.).  
  - Free Ant’s scalability is limited by high runtime on large instances.  
  - Restricted Ant might fail if the clustering doesn’t neatly assign all nodes under tight capacity constraints, leading to unassigned nodes.  
  - Performance is not always better than advanced references, especially for bigger problems (Christofides, Golden sets).

- **Relevance for Deterministic VRP & Future Work**  
  - Demonstrates a deterministic approach to “green” VRP, aligning well with the thesis scope.  
  - Illustrates how unsupervised ML can guide solution space partitioning.  
  - Future enhancements: dynamic re-clustering, advanced parameter self-tuning (e.g., reinforcement learning or advanced ML for adapting the ACO parameters), and parallelization.  
  - Highlights the need for more specialized benchmark instances specifically designed for energy objectives.