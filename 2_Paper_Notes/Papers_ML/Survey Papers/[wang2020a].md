# Collaborative two-echelon multicenter vehicle routing optimization based on state–space–time network representation

## Metadata
- **Link to PDF**: [[[wang2020a]_Collaborative_two-echelon_multicenter_vehicle_routing_optimization_based_on_state–space–time_network_representation.pdf]]
- **Tags**:
  - #ML-Unsupervised
  - #ML-Clustering
  - #GeneticAlgorithm
  - #MultiDepotVRP
  - #MultiVehicleRouting
  - #TimeWindowConstraints
  - #HeuristicOrExactBlends
  - #GreenVRP
  - #GameTheory
  - #LargeScaleVRP
- **Relevant**: true  
- **Fit Score**: 7  
- **State of the Art (SoA) Concepts**:
  - Two-echelon (multicenter) VRP
  - Unsupervised K-means clustering for customer grouping
  - Hybrid methodology (DP + GA-based multi-objective algorithm)
  - Game-theoretic cost allocation (CGA, Shapley-like approach)
  - Incorporation of emission considerations (environment-friendly logistics)
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: custom dataset (no standard benchmarks)

## Abstract
"Collaboration among service providers in a logistics network can greatly increase their operation efficiencies and reduce transportation emissions. This study proposes, formulates and solves a collaborative two-echelon multicenter vehicle routing problem based on a state–space–time (CTMCVRP-SST) network to facilitate collaboration and resource sharing in a multiperiod state–space–time (SST) logistics network. The CTMCVRP-SST aims to leverage spatial-temporal properties of logistics demands and resources to optimize resource distribution in space and time. A three-component solution framework is proposed: (1) a bi-objective linear programming model for resource sharing, minimizing fleet size and total cost, (2) an integrated algorithm combining SST-based dynamic programming, improved K-means clustering, and an improved non-dominated sorting genetic algorithm-II (Im-NSGAII), and (3) a cost gap allocation model for fair profit sharing. Empirical results from a Chongqing logistics network indicate that the proposed mechanism and SST representation significantly reduce costs, improve resource utilization, and promote efficient and sustainable logistics operations."

## Summary
- **Problem & Context**
  - Investigates a *collaborative* two-echelon vehicle routing network with multiple depots (logistics centers) and distribution centers.
  - Introduces a “state–space–time” (SST) representation to capture the truck loading state, spatiotemporal constraints, and scheduling windows.
  - Main objective: Reduce total transportation cost, number of vehicles, and incorporate emission-related factors.

- **Methodology**
  1. **Model**: Formulates a bi-objective mixed integer programming model:
     - Minimizes total cost (transportation + penalty + maintenance + operational) and total fleet size.
     - Incorporates multi-period scheduling, resource sharing (e.g., shared trucks), and service-time constraints in an SST network.
     - Accounts for two echelons: from logistic centers to distribution centers (first echelon) and from distribution centers to customers (second echelon).
  2. **Solution Framework**:
     - *Unsupervised K-means Clustering*: Used to group customers based on 4D features (space + time windows), simplifying the second-echelon routing.
     - *Dynamic Programming (DP)*: Applied to optimize truck scheduling/routes in the first echelon, respecting capacity states and timing windows.
     - *Improved NSGA-II*: A multi-objective evolutionary algorithm (with partial-mapped crossover, polynomial mutation) to refine routes and reduce cost/vehicle usage. Integrates a savings-based initialization (Clarke-Wright style) to speed up convergence.
  3. **Profit Allocation**:
     - Uses a game-theoretic cost gap allocation (CGA) strategy to ensure fair distribution of the cost savings among the distribution centers. This encourages collaboration and coalition stability.

- **Key Results**
  - Empirical case study on a Chongqing-based logistics network (6 distribution centers, 2 logistics centers, 180 customers).
  - Collaboration + the SST-based optimization significantly reduce total cost (up to ~30-40% in examples) and the number of required vehicles/trucks.
  - The authors also compare the proposed Im-NSGA-II with other heuristics (ACO, MOPSO) on test instances, demonstrating that the integrated approach yields lower total cost while maintaining feasible runtimes.
  - The cost gap allocation model ensures stable coalitions by distributing savings in a way that each participant benefits as new members join.

- **Relevance to Deterministic ML-based VRP**
  - The second-echelon routing leverages *unsupervised K-means clustering* for customer grouping, a recognized ML technique.
  - The main VRP setting is deterministic (no random or stochastic variations).
  - Demonstrates how classical OR methods (DP, GA) can be integrated with unsupervised ML (clustering) to solve large-scale multi-depot routing.

- **Strengths & Limitations**
  - **Strengths**:
    - Holistic approach to route optimization: first echelon (dynamic programming) + second echelon (clustering + GA).
    - Addresses collaboration, resource sharing, and emission considerations.
    - Incorporates a game-theoretic profit allocation to maintain stable collaboration.
  - **Limitations**:
    - The method’s computation times may grow for very large problem sets; performance mainly illustrated on custom instances.
    - The multi-objective aspect (cost vs. number of vehicles) is tested, but real-world constraints (like variable traffic) are not deeply addressed.
    - The K-means-based clustering might lack advanced ML refinements or domain-driven constraints.

- **Potential Impact**
  - Useful reference for adopting *unsupervised clustering* to partition large VRP instances in two-echelon or multi-depot contexts.
  - Shows viability of partial ML integration (K-means) in synergy with advanced OR techniques for collaborative networks.
  - Could be extended to other real-life logistics frameworks seeking sustainable, cost-sharing-based solutions.