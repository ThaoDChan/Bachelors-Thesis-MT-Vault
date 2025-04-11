# A Green Vehicle Routing Problem

## Metadata
- **Link to PDF**: [[[erdogan2012]_A_Green_Vehicle_Routing_Problem.pdf]]
- **Tags**:  
  #ML-Unsupervised  
  #GreenVRP  
  #DeterministicVRP  
  #LocalSearchHeuristics  
  #HeuristicOrExactBlends  
  #MultiVehicleRouting  
  #CplexSolver  
  #LargeScaleVRP  
  #ClusterFirstRouteSecond  
  #RealWorldData  
- **Relevant**: true  
- **Fit Score**: 9  
- **State of the Art (SoA) Concepts**:
  - Mixed-integer linear programming (MILP) for distance & refueling constraints
  - Clarke & Wright-based savings heuristic, modified for fuel capacity
  - Density-based clustering (DBSCAN) as an unsupervised ML method
  - Real-world case study demonstrating limited refueling infrastructure
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: Custom-generated problem sets and real-world data (Washington DC region); no standard benchmarks such as Solomon or Christofides used  

## Abstract
"A Green Vehicle Routing Problem (G-VRP) is formulated and solution techniques are developed to aid organizations with alternative fuel-powered vehicle fleets in overcoming difficulties that exist as a result of limited vehicle driving range in conjunction with limited refueling infrastructure. The G-VRP is formulated as a mixed integer linear program. Two construction heuristics, the Modified Clarke and Wright Savings heuristic and the Density-Based Clustering Algorithm, and a customized improvement technique, are developed. Results of numerical experiments show that the heuristics perform well. Moreover, problem feasibility depends on customer and station location configurations. Implications of technology adoption on operations are discussed."

## Summary
- **Context & Motivation**  
  - Addresses the challenge of vehicle routing for fleets powered by alternative (green) fuels where refueling infrastructure is limited.  
  - Focuses on deterministic constraints: each vehicle has a finite fuel tank capacity, must plan visits to refueling stations if necessary, and operate within a maximum route duration.  
  - Explores the operational impact of transitioning to alternative-fuel vehicles, highlighting extra travel or potential need for more vehicles when refueling stations are scarce.

- **Problem Definition**  
  - Defines the “Green VRP” (G-VRP) on a complete undirected graph with customers, one depot, and a set of alternative fueling stations (AFSs).  
  - Each route must respect:
    1. **Fuel tank capacity**: Vehicles can run out of fuel if route segments exceed tank range without stopping.  
    2. **Time/duration limit**: Each route is bounded by a maximum service/travel time.  
    3. **Customers**: Must be served exactly once; AFSs can be optionally visited multiple times or not at all.  

- **Mathematical Formulation**  
  - Proposes a mixed-integer linear programming (MILP) model:  
    - Decision variables track whether an edge is used and the fuel level/time upon arrival at each node.  
    - Subtour elimination is handled via time relationships (similar to standard VRP formulations) but augmented with fuel-level constraints.  
    - Incorporates constraints preventing vehicles from getting stranded or violating route-duration limits.  
  - Solved exactly with CPLEX for small instances, but recognized as NP-hard, so exact approaches struggle for large problem sizes.

- **Solution Methods**  
  - **Modified Clarke & Wright Savings Heuristic (MCWS)**:  
    - Extends the traditional savings concept by inserting refueling stations when needed.  
    - Merges “back-and-forth” routes if feasibility (fuel, time) is maintained or regained by introducing AFS visits.  
    - Allows multiple merges to reduce fleet size, while ensuring route feasibility under driving range constraints.
  - **Density-Based Clustering Algorithm (DBCA)**:  
    - Uses a DBSCAN-inspired unsupervised clustering step to partition customers based on spatial density.  
    - Each cluster is routed separately using the MCWS approach.  
    - The best (ε, minPts) parameters for DBSCAN are chosen experimentally to minimize total distance.  
    - Offers a cluster-first, route-second method, adapted to incorporate refueling stops in the post-clustering routing phase.
  - **Improvement Heuristic**:  
    - Once routes are constructed, a local search step tries to swap vertices among routes or reorder vertices within a route.  
    - Checks if an AFS is redundant or whether repositioning an AFS can yield shorter routes.  
    - Aims to reduce total distance while maintaining feasibility (no extra fuel or time violations).

- **Key Results & Findings**  
  - **Comparison with Exact MILP**:  
    - On small synthetic problems (≤ 20 customers), heuristics deviate on average 2-10% from the optimal solution.  
    - Quality degrades if stations are poorly located, illustrating the importance of strategic placement of AFSs.  
  - **Impact of AFS Availability**:  
    - More AFSs can significantly reduce route length, but location matters more than the sheer number of stations.  
    - With few or poorly placed stations, some customers become infeasible to serve within route-duration or fuel constraints.  
  - **Real-World Case Study**:  
    - Analyzed a medical supply company scenario in the Washington, DC area with 21 biodiesel stations.  
    - Showed that switching to alternative-fuel trucks increases total miles by ~19% in some configurations and may require more routes.  
    - Increasing station density or fuel tank capacity helps reduce the needed fleet size and route lengths.

- **Strengths, Weaknesses, & Limitations**  
  - **Strengths**:
    - Introduces a structured G-VRP formulation for alternative-fuel fleet routing.  
    - Proposes a novel use of DBSCAN-based clustering (an unsupervised ML method) for VRP subproblem decomposition.  
    - Demonstrates feasibility and performance on synthetic and real data.  
  - **Weaknesses**:
    - Heuristics might miss better solutions if suboptimal AFS placement is chosen during merges.  
    - Clustering parameters (ε, minPts) require trial-and-error tuning.  
    - No direct comparison with standard VRP benchmarks, focusing on specialized data sets.  
  - **Limitations**:
    - Model is still a single-day, single-depot scenario with uniform refueling times.  
    - Does not incorporate advanced cost structures (like varying fuel prices).

- **Relevance to Deterministic VRP & ML**  
  - Demonstrates a classical approach to the G-VRP under deterministic conditions.  
  - Employs an **unsupervised ML technique (DBSCAN)** for partitioning customers into clusters.  
  - Showcases how a standard savings heuristic can be adapted with station-insertion for green fleet routing.  
  - Offers valuable insights on how limited infrastructure affects route design—directly relevant to any deterministic VRP with specialized constraints (like partial reloading or fuel capacity).

- **Potential Impact for Future Research**  
  - Hybrid approach bridging MILP, domain heuristics, and clustering-based ML can guide large-scale VRPs in practice.  
  - Encourages deeper exploration of data-driven station placement or dynamic clustering to adapt to new fueling constraints.  
  - Could be extended by incorporating advanced or domain-specific machine learning methods to optimize station usage, or by investigating multi-objective trade-offs (e.g., cost vs. emissions).