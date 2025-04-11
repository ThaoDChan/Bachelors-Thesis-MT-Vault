# ROUTE FIRST-CLUSTER SECOND METHOD FOR PERSONAL SERVICE ROUTING PROBLEM

## Metadata
- **Link to PDF**: [[[kubra2019]_Route_First-Cluster_Second_Method_For_Personal_Service_Routing_Problem.pdf]]
- **Tags**:
  - #ML-Unsupervised
  - #GeneticAlgorithm
  - #HeuristicOrExactBlends
  - #CVRP
  - #DeterministicVRP
  - #MultiVehicleRouting
  - #LocalSearchHeuristics
  - #RealWorldData
  - #RouteFirstClusterSecond
- **Relevant**: true  
- **Fit Score**: 6  
- **State of the Art (SoA) Concepts**:
  - Route-first, cluster-second heuristic  
  - Genetic Algorithm for route optimization  
  - K-means, K-medoids, K-modes clustering  
  - Capacity-constrained VRP for service routing  
- **Performance Evaluation**: no  
- **Performance Evaluation Framework**: -

## Abstract
"The Vehicle Routing Problem (VRP), which has many sub-branches, is a difficult problem that cannot be solved using classical methods. This study includes a case study for Service Routing Problem, which is one of the sub-branches of VRP. The case study is a problem of determining service routes for staffs of a company. In this context, we first assigned the employees to the stations, and then we reached a solution using the route first-cluster second heuristic method. We used the Genetic Algorithm (GA) to improve the route and compared the results by creating different scenarios in clustering methods."

## Summary
- **Scope and Motivation**  
  - Addresses a *deterministic* personnel transport problem framed as a VRP.  
  - Focuses on reducing total travel distance while respecting vehicle capacities.  
  - The paper targets practical constraints: a company needs to plan staff transportation using multiple service vehicles with limited capacity.

- **Methodology**  
  - Uses the *route-first, cluster-second* heuristic approach:
    - First, treats the problem as a TSP to get a single route through all stops.
    - Improves that route using a *Genetic Algorithm (GA)* (population-based, with selection, crossover, and mutation).
    - Clusters the stops to allocate them to separate vehicles, ensuring capacities are not exceeded.
  - **Genetic Algorithm details**:
    - Tournament selection is used to pick individuals for reproduction.
    - Linear crossover (swaps segments between parents) and placement mutation (randomly moves a gene) are implemented.
    - Stopping criterion based on iteration count (e.g., 1000+ iterations, generating 2000 crossovers).
  - **Clustering**:
    - K-means, K-medoids, and K-modes clustering algorithms are tested.
    - Distance metrics: Euclidean, Manhattan, and Square Euclidean.
    - Each clustering partitions the improved single route (TSP solution) into sub-routes for different vehicles.

- **VRP Variant**  
  - Essentially a capacity-constrained VRP (CVRP) with multiple vehicles.
  - No stochastic or time-window constraints: purely deterministic.
  - Personal service routing for 93 staff, aggregated at 39 stops plus a destination node.

- **Key Results**  
  - Compares total travel distance for combinations of 3 clustering algorithms and 3 distance metrics.
  - Best total distance is ~178.66 km, achieved by several combinations (e.g., K-medoids + Euclidean).
  - The approach yields vehicle assignments with occupancy rates > 88% and up to 100%.

- **Strengths and Contributions**  
  - Proposes and implements a practical route-first, cluster-second scheme for a real-world staff transport scenario.
  - Integrates a Genetic Algorithm (metaheuristic) with standard clustering to achieve near-optimal solutions.
  - Demonstrates high vehicle occupancy rates, indicating efficient assignment of staff to each route.

- **Limitations and Observations**  
  - Uses a custom dataset (no comparison on standard VRP benchmarks).
  - Mainly focuses on distance minimization; does not explore broader objectives such as arrival time reliability or operational costs.
  - The clustering step might be sensitive to the chosen distance metric; Manhattan distance produced less favorable routes in some scenarios.

- **Relevance to Deterministic VRP Research**  
  - Clearly addresses a *deterministic* capacity VRP (no stochastic or dynamic components).
  - Showcases how metaheuristics (Genetic Algorithm) and unsupervised learning methods (clustering) can be combined for routing optimization.
  - Offers insights on route-first, cluster-second methodology which can be adapted to other deterministic VRP settings.