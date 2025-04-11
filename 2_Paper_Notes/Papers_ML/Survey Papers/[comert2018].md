# A cluster first-route second approach for a capacitated vehicle routing problem: a case study

## Metadata
- **Link to PDF**: [[[comert2018]_A_cluster_first-route_second_approach_for_a_capacitated_vehicle_routing_problem.pdf]]
- **Tags**:
  - #ML-Unsupervised
  - #ML-Clustering
  - #DeterministicVRP
  - #CVRP
  - #LargeScaleVRP
  - #ML-AssistedExact
  - #HeuristicOrExactBlends
  - #BranchAndBound
  - #MultiVehicleRouting
  - #RealWorldData
- **Relevant**: true  
- **Fit Score**: 8  
- **State of the Art (SoA) Concepts**:
  - Cluster-first, route-second approach (hierarchical heuristic)
  - K-means, K-medoids (unsupervised clustering)
  - CVRP with capacity constraints (32-40 pallets)
  - Branch and Bound exact solver for TSP subproblems
- **Performance Evaluation**: no (uses a custom real-world dataset, not a standard benchmark)
- **Performance Evaluation Framework**: -

## Abstract
"In this study, a capacitated vehicle routing problem (CVRP) which dealt with minimum distance routes for vehicles that serve customers having specific demands from a common warehouse under a capacity constraint. This problem is NP hard. We solved the problem in a hierarchical way (i.e., cluster-first route-second method). Firstly, customers were clustered using three different clustering algorithms; K-means, K-medoids and random clustering with considering a vehicle capacity. Secondly, routing problems for each cluster were solved using a branch and bound algorithm. The proposed solution strategy was employed on a case study in a supermarket chain. Results of numerical investigation were presented to illustrate the effectiveness of the algorithms using paired sample t tests. The results illustrated that the K-medoids algorithm provided better solution than the others."

## Summary
- **Scope & Focus**  
  - Addresses a **Capacitated Vehicle Routing Problem (CVRP)** under deterministic conditions, aiming to minimize total distance traveled and respect vehicle capacity (32–40 pallets).
  - Proposes a **hierarchical two-phase approach** (cluster-first, route-second).

- **Methodology: Cluster-First Route-Second**  
  - **Phase 1 (Clustering)**:
    - Three clustering algorithms tested:
      1. **K-means** (classical centroid-based clustering)
      2. **K-medoids** (selects actual data points as centers, robust to outliers)
      3. **Random Clustering** (assigns customers to clusters in a random manner)
    - Each cluster must not exceed the vehicle capacity range (32–40 pallets).  
    - If capacity is not satisfied, the number of clusters is adjusted.
  - **Phase 2 (Route Problem)**:
    - Each cluster is served by **one vehicle**.
    - The routing within each cluster is solved as a **Traveling Salesman Problem (TSP)**.
    - An **exact Branch and Bound** (B&B) method is used to solve each TSP.

- **Experiments & Results**  
  - Case study from a **supermarket chain** in Turkey, encompassing **78 stores** with known weekly demands.
  - Distribution days are classified into “busy” and “not busy.” The approach is applied to four subproblems (fresh/dry product deliveries, busy/not busy day).
  - **Total distance** and **vehicle count** are recorded for each clustering method across 20 data sets.
  - Statistical **paired t-tests** compare solution performance:
    - **K-medoids** consistently achieves the **lowest total distance**.
    - **K-means** is second best in terms of distance, but outperforms random clustering in both distance and vehicle count.
    - K-means and K-medoids produce similar vehicle counts, but K-medoids yields better cost (distance).
    - Random clustering typically performs worst.

- **Key Contributions & Findings**  
  - Demonstrates that clustering-based preprocessing can reduce a large-scale CVRP into multiple smaller TSPs solvable by an exact method within a reasonable time.
  - Shows that **K-medoids** is an effective unsupervised ML approach to generate balanced and spatially efficient clusters for capacity-constrained distribution.

- **Strengths & Limitations**  
  - **Strengths**:
    - Straightforward hierarchical framework, using common clustering methods and an exact B&B solver.
    - Real-world case data underscores practical viability.
    - Rigorous statistical analysis of results.
  - **Limitations**:
    - Clustering stages are limited to classical algorithms (K-means, K-medoids, random); no advanced ML beyond standard unsupervised clustering.
    - No standardized benchmark usage (only custom data from a single firm).
    - The method’s scalability for significantly larger networks is not fully explored (though it suggests a partial remedy for NP-hardness).

- **Relevance to Deterministic ML-based VRP**  
  - Clearly **deterministic**: demands and travel distances are fixed, with no stochastic or dynamic elements.
  - Uses **unsupervised ML** (K-means, K-medoids) to partition the problem space, combined with an exact solver.
  - Offers an example of a **hybrid approach** (machine learning + classical optimization) for large-scale CVRPs.