# Machine learning approach to solving the capacitated vehicle routing problem

## Metadata
- **Link to Original PDF**: [[[hassine2023]_Machine_learning_approach_to_solving_the_capacitated_vehicle_routing_problem.pdf]]
- **Tags**:  
  #ML-Unsupervised  
  #ML-Clustering  
  #HeuristicOrExactBlends  
  #BranchAndCut  
  #NearestNeighbor  
  #ML-AssistedExact  
  #DeterministicVRP  
  #CVRP  
  #LargeScaleVRP  
  #AlgorithmSelection  
- **Relevant**: true  
- **Fit Score**: 10  
- **State of the Art (SoA) Concepts**:  
  - K-means clustering for VRP  
  - Branch and Cut (exact method)  
  - Nearest Neighbor heuristic  
  - CVRP variant  
- **Performance Evaluation**: yes (custom instances)  
- **Performance Evaluation Framework**: custom dataset (“[0;7]” demand range, up to 100 customers)

## Abstract
“The Vehicle Routing Problem (VRP) is a combinatorial optimization problem of practical importance and complexity that attracts the attention of researchers. The Capacitated Vehicle Routing Problem (CVRP) is one of the best known variants of the VRP. It involves finding the optimal routes for a set of vehicles with limited carrying capacity, while satisfying customer demand deterministically. Many approaches have been proposed to solve CVRP. In our case, we have chosen a solution method that uses a powerful machine learning approach that researchers are currently investing in such concepts. We have developed two algorithms that, in a first phase, use the K-mean method to create groups of customers to be delivered and, in a second phase, use two different optimisation heuristics to find the optimal delivery route. A comparative study of computational results and execution time was carried out to determine the best approach to use in such an operational research problem.”

## Summary
- **Focus & Motivation**  
  - Addresses the Capacitated Vehicle Routing Problem (CVRP), a core VRP variant where vehicles of limited capacity must serve multiple customers.  
  - Aims to improve solution quality and reduce computation times by incorporating machine learning.  
  - Motivated by the persistent complexity of large-scale VRPs in logistics, where faster solution methods are needed.

- **ML Technique & Methodology**  
  - Employs **unsupervised learning** (K-means) to cluster customers based on location and vehicle capacity constraints.  
  - Once clusters are formed, two different methods are used to find optimal or near-optimal routes within each cluster:
    1. **Nearest Neighbor (NN)** heuristic: A classical approximation approach that iterates greedily from a chosen starting customer to the closest unvisited customer.  
    2. **Branch and Cut (B&C)**: An exact algorithm refining partial solutions through branching (splitting problems) and cutting (adding valid inequalities), leading to provably optimal solutions if allowed enough computational resources.

- **Approach & Implementation**  
  - Implemented in Python (Spyder environment, Python 2.7).  
  - Uses standard Python libraries: NumPy, scikit-learn (for clustering), and Matplotlib for visualization.  
  - Conducts experiments on randomly generated data sets (with demands in the interval [0; 7]) for 10, 30, 50, 70, and 100 customers.  
  - K-means divides the customers into as many clusters as the number of vehicles, ensuring each vehicle serves a distinct group of customers.

- **Results & Findings**  
  - Comparing K-means+NN vs. K-means+B&C on each instance size:
    - **K-means+B&C** consistently yields better (shorter) route distances and slightly faster or comparable execution times in the tested scenarios.  
    - Computation times remain relatively low (<1 second for up to 100 customers) due to the pre-clustering by K-means, which reduces the complexity for each subproblem.  
    - K-means+B&C surpasses K-means+NN, showing the combined benefit of clustering + exact solving.

- **Strengths & Limitations**  
  - **Strengths**:  
    - Demonstrates a synergy between **unsupervised ML** for partitioning the problem and **exact/heuristic** methods for route optimization.  
    - Finds good-quality solutions quickly for small- to medium-sized instances (up to 100 customers).  
  - **Limitations**:  
    - Primarily tested on synthetic or small-scale datasets; real-world complexities (time windows, multiple depots, etc.) are not covered.  
    - Does not compare with other advanced metaheuristics or machine learning models (e.g., deep reinforcement learning).

- **Conclusions & Relevance**  
  - Shows that **K-means** can effectively reduce VRP complexity by grouping customers spatially, facilitating a more efficient run of exact or heuristic solvers.  
  - The best performing method (K-means + Branch and Cut) highlights a promising avenue for ML-assisted exact algorithms in deterministic VRP contexts.  
  - Relevant for any research exploring combined or “hybrid” approaches (clustering + exact/heuristic) to tackle CVRP, especially where solutions must be found quickly with high quality.  
  - Encourages future work on using alternative clustering methods or exact+heuristic hybrids for larger, more diverse VRP instances.
