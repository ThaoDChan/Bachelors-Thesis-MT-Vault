# A cluster-first route-second approach for the swap body vehicle routing problem

## Metadata
- **Link to PDF**: [[[miranda2017]_A_cluster-first_route-second_approach_for_the_swap_body_vehicle_routing_problem.pdf]]
- **Tags**:
  - #DeterministicVRP
  - #LargeScaleVRP
  - #ML-Unsupervised
  - #ClusterFirstRouteSecond
  - #LocalSearchHeuristics
  - #HeuristicOrExactBlends
  - #RealWorldData
  - #ParameterTuning
  - #SwapBodyVRP
  - #GRASP
- **Relevant**: true  
- **Fit Score**: 5
- **State of the Art (SoA) Concepts**:
  - Swap Body Vehicle Routing Problem (SB-VRP)
  - k-means clustering for partitioning customers
  - Cluster-first route-second heuristic design
  - GRASP (Greedy Randomized Adaptive Search Procedure)
  - Local search operators (relocate, exchange, restricted Or-Opt, etc.)
- **Performance Evaluation**: yes
- **Performance Evaluation Framework**: VeRoLog Solver Challenge 2014 benchmark instances

## Abstract
“The swap body vehicle routing problem (SB-VRP) is a generalization of the classical vehicle routing problem where a particular structure as well as several operational aspects for the trucks composing the fleet are considered. This research has been motivated by the VeRoLog Solver Challenge 2014, organized together by VeRoLog and PTV group, aiming to motivate the study of real-world logistic problems. A truck can carry either only one swap body or, in addition, an extra trailer with an extra swap body. For the latter, special depots, called swap locations, can be used to drop and pickup the swap bodies. These operations may affect the feasibility and the cost of a route, and therefore the overall operational cost. In this paper, we propose a cluster-first route-second heuristic for the SB-VRP. Computational experiments are conducted over the benchmark instances proposed for the competition, simulating a practical environment by considering limited resources and execution time. The results obtained are of very good quality, where our approach ended as runner-up in the final set of instances and performs similarly to the other algorithms in the remaining cases, showing its potential to be applied in practice.”

## Summary
- **Context and Motivation**  
  - Proposes a new approach for the Swap Body Vehicle Routing Problem (SB-VRP), which generalizes the classical VRP by allowing trucks to operate with single or double swap bodies (trailers) and optional use of special “swap locations.”  
  - Developed in response to the VeRoLog Solver Challenge 2014, focusing on real-world logistic scenarios with strict time and resource constraints.  

- **Core Contributions**  
  1. **Cluster-First, Route-Second (CFRS) Heuristic**:  
     - Uses **k-means** (an unsupervised ML technique) to partition all locations (customers + swap depots) into clusters.  
     - Within each cluster, a Greedy Randomized Adaptive Search Procedure (GRASP) combined with local search refines the partial solution.  
     - Afterwards, partial cluster solutions are merged, and a further improvement phase adjusts routes globally.  

  2. **Route Construction and Local Search**:  
     - Employs GRASP with a “greedy randomized” selection of feasible insertions, combined with local search operators such as relocate, exchange, restricted Or-Opt, and a specialized 2-Opt restricted to segments of the route with consistent trailer use.  
     - Incorporates an **Iterated Local Search** (ILS) framework to escape local optima via a shaking procedure (random removal and reinsertion of customers).  

  3. **Swap Location Handling**:  
     - Designed a set of efficient feasibility checks for using “swap locations,” ensuring compliance with capacity and the user-defined maximum route duration.  
     - Notable design decision: the algorithm restricts each customer to only a handful of potential swap depots based on proximity (k-nearest swap locations).  

- **Methodology Details**  
  - **Clustering Phase**:  
    - Applies **k-means** on the coordinates of customers and swap locations to create up to `ncl` clusters.  
    - Each cluster is solved independently for a fraction of the total runtime, leveraging the GRASP + local search pipeline.  
  - **Integration and Final Improvement**:  
    - Merges the cluster-level solutions into a single feasible solution.  
    - Runs an overall improvement stage (ILS or local search) on this merged solution for the remaining time.  
  - **Parameter Tuning**:  
    - Experiments systematically vary the number of clusters (`k`), fraction of time allocated to the cluster-level solve, and local search iteration parameters.  
    - Finds best trade-off between cluster granularity and final improvement time.  

- **Experimental Results**  
  - Tested on challenge benchmark instances with up to 500+ customers and ~150 swap locations.  
  - The approach ranked second in the final round of the VeRoLog Solver Challenge 2014, demonstrating high-quality solutions within the limited 600s runtime.  
  - Comparison with other heuristics (e.g., local search, metaheuristics from the competition) showed that using clustering plus an integrated improvement stage yields highly competitive solutions, especially for large-scale cases.  

- **Key Findings & Observations**  
  1. **Clustering** helps to manage large instances quickly:  
     - Breaking the instance into smaller clusters shortens the time needed for local search expansions.  
     - Balancing the number of clusters and final integrated improvement time is crucial.  
  2. **Swap Bodies** can reduce cost but add complexity:  
     - Operational constraints (e.g., route duration, no load-sharing across swap bodies) require specialized feasibility checks and restricted local search moves.  
     - Dropping / picking up trailers at certain depots can be beneficial only if they align well with route geometry and time limits.  
  3. **Performance Gains** were primarily attributed to:  
     - Thorough local search exploring multiple neighborhood operators.  
     - The combined GRASP-ILS approach’s ability to escape local minima effectively.  
     - The synergy of cluster-first route-second design, which effectively harnesses a modest “unsupervised” clustering step (k-means) for partitioning.  

- **Strengths and Limitations**  
  - **Strengths**:  
    - Demonstrates that even a basic ML method (k-means) can substantially improve solution speed and scalability for large VRP variants.  
    - Robust local search operators tailored to the SB-VRP’s capacity and trailer constraints.  
    - Achieves top-ranking in a prominent competition, confirming its practical relevance.  
  - **Limitations**:  
    - The approach is still heuristic, so no formal optimality guarantees.  
    - Relies on the quality of the initial k-means partitioning and local search heuristics.  
    - The performance benefits from heavy parameter tuning, which might reduce out-of-the-box generality.  

- **Relevance to Deterministic VRP and ML**  
  - Addresses a **deterministic** variant of VRP (SB-VRP), aligning with typical routing constraints but adding trailer and route duration complexities.  
  - Leverages **k-means** (an **unsupervised ML** technique) to divide and conquer large-scale problem instances.  
  - Illustrates how a seemingly simple clustering approach can significantly improve scalability and solution quality for specialized VRP variants.  
  - Provides a strong demonstration of how hybrid approaches (a classical heuristic layered over an unsupervised learning step) can handle real-world, large-scale VRPs under strict runtime constraints.  