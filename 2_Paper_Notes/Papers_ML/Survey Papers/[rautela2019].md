# Distribution planning using capacitated clustering and vehicle routing problem: A case of Indian cooperative dairy

## Metadata

- **Link to PDF**: [[[rautela2019]_Distribution_planning_using_capacitated_clustering_and_vehicle_routing_problem.pdf]]
- **Tags**:
  - #ML-Unsupervised
  - #ML-Clustering
  - #DeterministicVRP
  - #CVRP
  - #HCVRP
  - #ClusterFirstRouteSecond
  - #HeuristicOrExactBlends
  - #BalancedClustering
  - #RealWorldData
- **Relevant**: true  
- **Fit Score**: 8  
- **State of the Art (SoA) Concepts**:
  - k-means clustering for CVRP pre-processing  
  - Cluster-first-route-second methodology  
  - Cheapest link algorithm for route construction  
  - Heterogeneous vehicle capacities in a deterministic VRP setting  
- **Performance Evaluation**: no (uses real-world case study data, not a standard benchmark)  
- **Performance Evaluation Framework**: -

## Abstract
"Purpose – The purpose of this paper is to reduce the distribution cost of an Indian cooperative dairy. The reduction of cost was achieved with the application of the clustering method (k-means clustering) and capacitated vehicle routing problem (cheapest link algorithm (CLA)).
Design/methodology/approach – Capacitated k-means clustering was used to split delivery locations into similar size groups (i.e. clusters) based on proximity without exceeding a specified total cluster capacity. Each cluster would be served by a local stockist. CLA was then used to find delivery routes from dairy (i.e. depot) to stockist in each cluster and from stockist to all other delivery locations within the cluster.
Findings – K-means clustering and CLA suggested optimal delivery routes on which vehicles will run. The complete algorithm was able to provide a solution within 30 s.
Practical implications – Clustering of delivery locations and use of heterogeneous fleet of delivery vehicles can result in considerable savings in daily operational cost.
Originality/value – Most of the research related to the use of demand clustering to improve distribution routes has been theoretical, which do not take into account real-world limitations like vehicle’s specific limitations. The authors tried to address that gap by taking a real-world case of a cooperative dairy and compared the result with existing distribution routes used by dairy. This work can be used by other dairies or distribution companies according to their scenario."

## Summary
- **Context and Motivation**  
  - Addresses rising distribution costs in an Indian cooperative dairy that operates a daily milk delivery service.  
  - Existing distribution system uses only high-capacity vehicles on multiple routes, leading to inefficiencies when demand on some routes is relatively small.  
  - Proposes a hybrid framework combining unsupervised clustering (k-means) with a VRP heuristic (cheapest link algorithm) to reduce costs and improve delivery efficiency.

- **Approach: Cluster First, Route Second**  
  - **K-means Clustering**:  
    - Groups 58 delivery points into six clusters based on Euclidean distance while respecting capacity constraints.  
    - Each cluster has a “stockist” point that acts as a local hub for that cluster.  
    - Capacitated clustering ensures that the total demand in each cluster does not exceed the capacity limit of the large vehicle used to supply that cluster’s stockist.  
  - **Capacitated Vehicle Routing with Cheapest Link Algorithm (CLA)**:  
    - Split into two stages: (1) route from dairy (depot) to the cluster stockists, (2) routes from each stockist to its cluster’s delivery locations.  
    - Uses a heterogeneous fleet: large-capacity trucks (2,700 L) supply each stockist, and smaller-capacity trucks (500 L) make final deliveries within each cluster.  
    - CLA constructs Hamiltonian circuits by repeatedly selecting the lowest-cost edges while avoiding early circuit closures.  
    - Applied separately for (a) factory-to-stockist routes and (b) stockist-to-retailers routes.

- **Methodology and Data**  
  - Empirical case study set in Varanasi, India.  
  - Demand distribution: 58 retail locations with varying daily requirements.  
  - Objective: minimize total daily transportation cost, factoring in vehicle cost per kilometer, labor cost, and small overhead for stockist refrigeration.  
  - Time constraints: each route must be serviced within an acceptable duration (∼2 hours).  
  - Implemented in Python, run time approx. 30 seconds.

- **Key Findings**  
  - Proposed cluster-first, route-second approach cut daily distribution cost by ~22.35% compared to the existing homogeneous fleet approach (from INR 13,378.30 to INR 10,387.46).  
  - Demonstrates that combining a simple unsupervised clustering technique with a capacitated routing heuristic can yield sizable cost savings in a real-world scenario.  
  - Confirms that properly sizing vehicles per route (large vs. small trucks) and grouping delivery points around local stockists can significantly reduce operational expenses.

- **Strengths and Contributions**  
  - Shows practical feasibility of ML-based (k-means) clustering integrated with a classic VRP heuristic for a real-life distribution problem.  
  - Clear cost assessment incorporating both vehicle running expenses and daily overhead for maintaining refrigeration at stockist locations.  
  - Rapid solution times (∼30 seconds) offer flexibility if the network or demands change frequently.  
  - Offers a blueprint for other dairies or distribution networks considering cluster-first-route-second frameworks with capacity constraints.

- **Limitations**  
  - Relies on the availability of suitable stockist sites with refrigeration; if an optimal stockist location is not practical, results may be suboptimal.  
  - The cheapest link algorithm is relatively simple and may not always match advanced optimization methods’ solution quality, though it is fast.  
  - Does not use standardized benchmarking; focuses solely on one cooperative dairy’s operational data.

- **Relevance to Deterministic VRP and ML**  
  - Employs unsupervised clustering (k-means) for partitioning service points.  
  - Focuses on a deterministic setting (fixed daily demands, no uncertainty).  
  - Demonstrates the cost benefits of cluster-first-route-second strategies in a real-world distribution scenario.  
  - Contributes to the growing body of research combining ML-based clustering with classical VRP heuristics.