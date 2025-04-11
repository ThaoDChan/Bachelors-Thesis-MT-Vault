# Constrained Clustering for the Capacitated Vehicle Routing Problem

## Metadata
- **Link to PDF**: [[[alesiani2022]_Constrained_clustering_for_the_capacitated_vehicle_routing_problem_(CC-CVRP).pdf]]
- **Tags**:
  - #ML-Unsupervised
  - #ML-Clustering
  - #HeuristicOrExactBlends
  - #LocalSearchHeuristics
  - #LargeScaleVRP
  - #CVRP
  - #DeterministicVRP
  - #TSP
  - #BranchAndCut
  - #VRPBenchmarks
- **Relevant**: true  
- **Fit Score**: 9  
- **State of the Art (SoA) Concepts**:
  - Constrained k-means-style clustering for large-scale deterministic CVRP
  - Two-stage method: cluster-level VRP + TSP sub-routes
  - Use of standard CVRP benchmarks (Uchoa 2017)
  - Demonstrates near real-time solutions for large instances with modest optimality gaps
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: Uchoa 2017 CVRP library (A, X, and other large instance sets)

## Abstract
"eCommerce, postal and logistics' planners require to solve large-scale capacitated vehicle routing problems (CVRPs) on a daily basis. CVRP problems are NP-Hard and cannot be easily solved for large problem instances. Given their complexity, we propose a methodology to reduce the size of CVRP problems that can be later solved with state-of-the-art optimization solvers. Our method is an efficient version of clustering that considers the constraints of the original problem to transform it into a more tractable version. We call this approach Constrained Clustering Capacitated Vehicle Routing Solver (CC-CVRS) because it produces a soft-clustered vehicle routing problem with reduced decision variables. We demonstrate how this method reduces the computational complexity associated with the solution of the original CVRP and how the computed solution can be transformed back into the original space. Extensive numerical experiments show that our method allows to solve very large CVRP instances within seconds with optimality gaps of less than 16%. Therefore, our method has the following benefits: it can compute improved solutions with small optimality gaps in near real-time, and it can be used as a warm-up solver to compute an improved solution that can be used as an initial solution guess by an exact solver."

## Summary
- **Context & Motivation**  
  - The paper addresses the classic Capacitated Vehicle Routing Problem (CVRP), focusing specifically on large-scale instances that are typically intractable via exact methods in limited runtime.
  - It proposes a novel clustering-based solution method (“Constrained Clustering for the CVRP,” or CC-CVRP) that transforms the problem into a smaller instance.

- **Core Idea: Constrained Clustering**  
  - CC-CVRP builds on a k-means-like clustering approach but applies additional CVRP-related constraints (capacity, distance limits) when assigning customers to clusters.
  - Each cluster can be served by one vehicle, and clusters are dynamically adapted to ensure feasibility (e.g., maximum cluster load cannot exceed the vehicle capacity).
  - The algorithm is iterative and “self-adaptive,” meaning the number of clusters can grow or shrink until all customers are assigned without violating capacity or distance constraints.

- **Methodology**  
  1. **Cluster Formation**:  
     - Uses a k-means-style assignment: each cluster has a “centroid” (the cluster head).  
     - Customers are assigned to the nearest feasible centroid, but constraints on capacity and maximum cluster size are enforced.  
     - The cluster heads are updated after each assignment, recalculating the centroid coordinates to represent the geometric mean of its assigned customers.  
  2. **Reduced-Dimension CVRP**:  
     - After clustering, each cluster is treated as a “virtual node” with aggregated demand.  
     - A smaller CVRP is then solved (either by an exact solver or a heuristic) at the level of cluster heads to obtain the high-level route plan.  
  3. **Vehicle Routing at the Customer Level**:  
     - Once the cluster-level routes are decided, each cluster’s internal set of customers is sequenced by solving a TSP subproblem, typically with a local search (e.g., 2-opt).  
     - This step refines the route, replacing the cluster head with actual customer visits in the best order.

- **Results & Benchmarking**  
  - Evaluated on established CVRP benchmark sets (e.g., A-n, X-n, and even “super” large-scale up to 16,000 customers).
  - Reported average optimality gaps range from about 8% to 16% for very large-scale problems, often computed within seconds or a few minutes.
  - For smaller benchmarks (fewer than 100 nodes), CC-CVRP can achieve near-optimal solutions in limited runtime and can outperform trying to solve the unclustered CVRP with the same time budget.

- **Contributions & Strengths**  
  - Demonstrates that constrained clustering significantly reduces computational effort for large CVRPs, yielding feasible solutions faster than directly tackling the full problem.
  - Maintains solution quality despite the dimension-reduction.
  - Scalable approach: can handle thousands of nodes and return solutions in near real-time.
  - Can also act as an “initial solution generator” for an exact solver, accelerating subsequent optimization phases.

- **Weaknesses & Limitations**  
  - The cluster-head approximation can omit inter-customer distances inside a cluster, introducing approximation errors.
  - The solution quality might degrade for extremely high tightness demands or for particular geometric distributions where clustering is less effective.
  - No advanced machine learning model beyond classical k-means clustering is employed (though it is indeed an unsupervised ML technique).

- **Relevance & Impact**  
  - Shows an unsupervised machine-learning-based approach (clustering) for deterministic, large-scale CVRP.
  - Potentially valuable for logistics settings needing fast solutions and willing to accept small optimality gaps.
  - Aligns with the thesis interest in ML (unsupervised) for deterministic VRPs and provides relevant performance comparisons using standard CVRPLIB-type datasets.