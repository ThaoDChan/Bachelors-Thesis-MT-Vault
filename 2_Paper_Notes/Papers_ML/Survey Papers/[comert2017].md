# Anew approach for solution of vehicle routing problem with hard time window: an application in a supermarket chain

## Metadata

- **Link to PDF**: [[[comert2017]_A_new_approach_for_solution_of_vehicle_routing_problem_with_hard_time_window-An_application_in_a_supermarket_chain.pdf]]
- **Tags**:
  - #ML-Unsupervised
  - #ML-AssistedExact
  - #HeuristicOrExactBlends
  - #DeterministicVRP
  - #VRPTW
  - #LargeScaleVRP
  - #RealWorldData
  - #ClusterFirstRouteSecond
- **Relevant**: true  
  (Uses unsupervised clustering techniques for a deterministic VRPTW and combines them with an exact solver, matching the thesis focus on ML-based deterministic VRP methods.)  
- **Fit Score**: 7  
- **State of the Art (SoA) Concepts**:
  - VRP with Hard Time Windows (VRPHTW)
  - Clustering-based solution approaches (K-means, K-medoids, DBSCAN)
  - Hierarchical “cluster-first route-second” strategy
  - Exact solution (MILP) for route optimization
- **Performance Evaluation**: no  
- **Performance Evaluation Framework**: -

## Abstract

"In this study, a vehicle routing problem with hard time windows (VRPHTW) that appears to meet demands of customers’ service within time intervals in a supermarket chain is solved. In VRPHTW, to reach a solution by an exact method is quite difficult and sometimes impossible if number of constraints is large enough (i.e., NP-hard), and solution time of a VRPHTW grows exponentially. As the size of the problem grows, a near optimal solution can be found using a heuristic method. A hierarchical approach consisting of two stages as “cluster-first route-second” is proposed. In the first stage, customers are assigned to vehicles using three different clustering algorithms (i.e., K-means, K-medoids and DBSCAN). In the second stage, a VRPHTW is solved using a MILP. The main contribution of the article is that the proposed hierarchical approach enables us to deal with a large size real problem and to solve it in a short time using the exact method. Finally, the proposed approach is employed on a supermarket chain. An instance of the algorithm is demonstrated to illustrate the applicability of the proposed approach and the results obtained are highly favourable."

## Summary
- **Problem & Motivation**  
  - Addresses the Vehicle Routing Problem with Hard Time Windows (VRPHTW) for a supermarket chain in Turkey.  
  - Key distribution constraints: each customer (store) must be served within a strict time window [a_i, b_i].  
  - Fleet of homogeneous vehicles with a capacity of 40 pallets; vehicles should not run under 32 pallets.  
  - The scale (78 stores, real data from 21 weeks) makes pure exact methods computationally expensive.

- **Approach & Methodology**  
  - Proposes a **two-phase hierarchical framework**:
    1. **Clustering (Unsupervised ML)**:  
       - Uses three clustering algorithms (K-means, K-medoids, DBSCAN) to assign customers to vehicles (each cluster corresponds to a vehicle’s route group).  
       - Ensures each cluster respects the 32–40 pallets capacity constraint.  
    2. **Route Optimization (Exact Solver)**:  
       - Solves a Mixed Integer Linear Programming (MILP) formulation (node-based) to find an optimal visiting sequence (a traveling-salesman-like tour) inside each cluster.  
       - Objective: minimize total travel time + waiting time, subject to hard time windows.

- **Notable ML Component**  
  - Clustering via K-means, K-medoids, and DBSCAN is an **unsupervised machine learning** approach to divide stores into capacity-feasible sets.  
  - By clustering first, the authors circumvent the combinatorial explosion of attempting a single large MILP on 78 customers.

- **Key Findings**  
  - Each clustering method was tested on 21 weeks of demand data.  
  - After forming capacity-controlled clusters, the route optimization stage provided feasible schedules meeting all time windows.  
  - The method outperformed the supermarket’s existing (manual) routing approach in total distribution time:  
    - K-means yielded the best improvement (about 6% average savings in total route time).  
    - DBSCAN next (about 5% improvement),  
    - K-medoids improved about 1% over the baseline.  
  - Statistical analysis (ANOVA) showed that the solution times from all three ML-based approaches were significantly better than the firm’s actual results.

- **Strengths & Contributions**  
  - Demonstrates that **hierarchical “cluster-first, route-second”** can handle a real-life large-scale VRPHTW within short computational times.  
  - Combines standard unsupervised clustering with a well-defined MILP, showing that even basic ML clustering can improve upon naive or manual distribution planning.  
  - Provides a full empirical assessment on multi-week real demand data rather than a single instance or purely synthetic benchmark.

- **Limitations & Gaps**  
  - The clustering approach depends on trial-and-error to set certain algorithm parameters (e.g., number of clusters for K-means, Eps/MinPts for DBSCAN).  
  - Does not compare to advanced or specialized VRPTW heuristics or metaheuristics (e.g., Adaptive Large Neighborhood Search or Tabu Search variants).  
  - No standardized benchmark results are presented (all data are from a private supermarket chain).  
  - ML usage here is limited to clustering rather than more advanced predictive or learning-to-optimize frameworks (e.g., RL or neural combinatorial optimization).

- **Relevance to Deterministic VRP Research**  
  - Illustrates an **unsupervised ML** approach combined with an **exact solver** for a **deterministic VRPTW**.  
  - Valid example of how basic ML techniques can reduce problem complexity by grouping customers prior to routing.  
  - Contributes to the discussion on “cluster-first route-second” approaches in large-scale VRP.  
  - Shows practical potential in real-world distribution tasks.

- **Potential Impact**  
  - This hierarchical method is scalable and can be adapted to other large VRPTW settings.  
  - Confirms that a simple but well-structured clustering + MILP pipeline can achieve cost/time savings in real logistics contexts, making it a notable reference for those exploring ML-driven VRP partitioning.