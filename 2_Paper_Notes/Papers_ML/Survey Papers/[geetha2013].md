# Nested particle swarm optimisation for multi-depot vehicle routing problem

## Metadata
- **Link to PDF**: [[[geetha2013]_Nested_particle_swarm_optimisation_for_multi-depot_vehicle_routing_problem.pdf]]
- **Tags**:
  - #DeterministicVRP
  - #MultiDepotVRP
  - #PopulationBasedEvolution
  - #GeneticAlgorithm
  - #LocalSearchHeuristics
  - #HeuristicOrExactBlends
  - #ParticleSwarmOptimization
- **Relevant**: true  
- **Fit Score**: 6  
- **State of the Art (SoA) Concepts**:
  - Multi-depot VRP (MDVRP) with capacity constraints
  - Particle Swarm Optimization (PSO) metaheuristic
  - Hybrid approach (PSO + genetic operators + local search)
  - Clustering-first, route-second approach
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**:  
  - Uses well-known MDVRP benchmarks by Christofides, Eilon, Gillet, Johnson, and Chao  

## Abstract
"Vehicle routing problem (VRP) is a well-known non-deterministic polynomial hard problem in operations research. VRP is more suited for applications having one warehouse. A variant of VRP called multi-depot vehicle routing problem (MDVRP) has more than one warehouse. Cluster first and route second is the methodology used for solving MDVRP. An improved k-means algorithm is proposed for clustering that reduces the MDVRP to multiple VRP. In this work, MDVRP is considered with more than one objective and nested particle swarm optimisation with genetic operators is proposed for solving each VRP. Master particle swarm optimisation forms the group within each cluster. Slave particle swarm optimisation generates the route for each group. The objective of MDVRP is to minimise the total travel length along with route and load balance among the depots and vehicles. The results obtained are better in balancing load, route length and the number of vehicles, rather than minimisation of total cost."

## Summary
- **Context & Motivation**  
  - Addresses a **deterministic multi-depot vehicle routing problem (MDVRP)** with capacity constraints.
  - Traditional MDVRP focuses on minimizing travel distance from multiple depots to customers. This paper extends the focus to also **balance loads among vehicles and routes** across depots.

- **Core Contribution**  
  - Proposes a **Nested Particle Swarm Optimisation (NPSO)** approach that integrates an improved k-means clustering method, PSO, and genetic operators:
    - **Improved k-means** for “cluster first” step, adding a capacity-based constraint.  
    - A **Master PSO** (assignment phase) for grouping customers within each cluster to available vehicles, respecting capacity.  
    - A **Slave PSO** (route construction phase) to determine the visit order for the assigned customers, enhanced with genetic operators (crossover, mutation) and a 2-opt local improvement.

- **Methodology**  
  - **Clustering**:  
    - Uses a modified k-means that prioritizes customers with higher demands first (minimax principle), ensuring they are more carefully assigned to depots so that capacity is utilized effectively.
    - First-fit decreasing logic ensures customers are packed into clusters without exceeding depot capacity (sum of vehicles’ capacities).
  - **Particle Swarm Optimisation**:  
    - **Master PSO**: Integer encoding to assign each customer to a vehicle. Particle velocity and position updates guided by local/global best solutions, plus GA operators.  
    - **Slave PSO**: Real-coded solution for each vehicle’s route, then converted back to permutations through rank-order value. Local search 2-opt refines routes.
    - The “nested” idea is that each cluster (depot) optimization spawns sub-problems that are tackled by the PSO with a route-generation subroutine.
  - **Objectives**: 
    - Minimize total travel cost (sum of vehicle tours)  
    - Balance load and route usage among depots/vehicles

- **Experiments & Results**  
  - Tested on classical MDVRP benchmark sets (Christofides, Eilon, Gillet, Johnson, Chao).  
  - Achieves solutions that are not always the best in pure distance minimization but demonstrates **better load and route balancing** across depots.  
  - The paper reports **lower standard deviations of load** among vehicles and more evenly distributed routes.  
  - Shows feasibility for real-life scenarios such as **waste collection** and **home delivery** pharmacy distribution, emphasizing practical fairness constraints.

- **Strengths**  
  - Incorporates a capacity-based clustering heuristic (improved k-means) that effectively partitions the problem into smaller VRP instances.  
  - Demonstrates synergy between PSO and local improvement (2-opt), plus genetic operators to avoid local optima.  
  - Experimental results suggest good trade-offs between route cost and load fairness.

- **Weaknesses & Limitations**  
  - Does not claim global optimal solutions; purely metaheuristic.  
  - The approach can deviate from known best solutions if strict minimal distance is the priority.  
  - PSO, combined with GA operators, introduces many parameters (inertia, crossover rates, etc.) that may require careful tuning for new instances.

- **Relevance to Deterministic VRP**  
  - The paper focuses on a **deterministic version** of MDVRP, with no random/stochastic or real-time changes.  
  - Illustrates a **population-based evolutionary** heuristic, which can be considered within the broader scope of machine learning-based or computational intelligence approaches.  
  - Emphasizes capacity constraints, multi-depot structures, and fairness among routes, making it relevant to practical distribution use-cases.

- **Key Takeaways**  
  - Demonstrates how **clustering-first** can simplify MDVRP.  
  - Nested PSO approach (master-slave) provides a flexible and adaptive route construction method.  
  - Balancing route length and load can be prioritized over pure cost minimization for practical fairness or operational constraints.