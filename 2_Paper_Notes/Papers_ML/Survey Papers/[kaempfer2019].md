# Learning the Multiple Traveling Salesmen Problem with Permutation Invariant Pooling Networks

## Metadata
- **Link to PDF**: [[[kaempfer2019]_Learning_the_Multiple_Traveling_Salesmen_Problem_with_Permutation_Invariant_Pooling_Networks.pdf]]
- **Tags**:  
  #ML-Supervised  
  #ML-SupervisedFromOptimal  
  #ML-AssistedExact  
  #ML-AssistedHeuristics  
  #LocalSearchHeuristics  
  #BeamSearch  
  #GraphNeuralNetworks  
  #MultiVehicleRouting  
  #DeterministicVRP  
  #HeuristicOrExactBlends  

- **Relevant**: true  
- **Fit Score**: 9  
- **State of the Art (SoA) Concepts**:  
  - Supervised learning using solutions from an ILP solver  
  - Permutation-invariant neural architecture for combinatorial optimization  
  - Multiple Traveling Salesmen Problem (mTSP) as a multi-vehicle routing extension  
  - Comparison to OR-Tools local search heuristics on TSPLib instances  
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: TSPLib and custom synthetic datasets  

## Abstract
> “While there are optimal TSP solvers, as well as recent learning-based approaches, the generalization of the TSP to the Multiple Traveling Salesmen Problem is much less studied. Here, we design a neural network solution that treats the salesmen, cities and depot as three different sets of varying cardinalities. We apply a novel technique that combines elements from recent architectures that were developed for sets, as well as elements from graph networks. Coupled with new constraint enforcing output layers, a dedicated loss, and a search method, our solution is shown to outperform all the meta-heuristics of the leading solver in the field.”

## Summary

- **Problem & Motivation**  
  - Addresses the Multiple Traveling Salesmen Problem (mTSP), a natural generalization of TSP in which multiple salesmen start from a single depot, visit cities exactly once in total, and return to the depot.  
  - Seeks a machine learning-based approach that can directly learn solutions from small optimally solved instances and then generalize to larger or more complex instances.

- **Core Method & Architecture**  
  - Introduces a **permutation-invariant pooling network** capable of handling three different sets: the salesmen, the depot, and the cities.  
  - Each entity (salesman, city) is embedded into a representation space of fixed dimension.  
  - Uses **leave-one-out (LOO) pooling**: for each element, the pooled vector excludes that element’s own embedding, providing more stable context features.  
  - Incorporates **learned spatial weighting** inspired by graph neural networks: distances between elements are input to a learned weighting function, which adjusts how information is pooled.

- **Training Setup**  
  - **Supervised learning** with ground-truth solutions obtained from an ILP solver.  
  - Defines a novel **soft multi-dimensional assignment** layer (an extension of softmax) to ensure that the network output is “semi multi-stochastic,” respecting core mTSP constraints like each salesman leaving and returning once, each city visited exactly once, etc.  
  - A **representation-invariant loss** function ensures that permuting salesmen or reversing routes does not cause ambiguity during training. This fosters a stable network output rather than producing an “average” solution.  

- **Solution Decoding**  
  - After forward pass, a **beam search** is employed to convert the continuous soft assignments into a discrete set of tours. The beam search enumerates partial routes in a probabilistic manner, selecting the final best solution.

- **Results & Comparisons**  
  - Demonstrated on synthetic mTSP instances (1 to 5 salesmen, up to 20 cities) and on **TSPLib**-derived instances with up to 99 cities.  
  - Compared against **OR-Tools** local search metaheuristics (Guided Local Search, Tabu Search, Simulated Annealing, etc.).  
  - Outperforms OR-Tools metaheuristics in most settings, especially when the beam size is limited (fewer candidate solutions).  
  - Also tested on regular TSP benchmarks (TSP5/TSP10/TSP20). Achieved results comparable to other neural TSP solvers, validating generalization to single-salesman TSP.

- **Strengths & Contributions**  
  - **Permutation-invariant pooling** specifically adapted for multi-set inputs (salesmen, depot, cities), ensuring the model scales to different problem sizes and permutations.  
  - **Robust supervised approach**: Gains from the guarantee of ILP-based solutions on small instances but generalizes well to bigger ones.  
  - A **flexible architecture** that can be extended to other variants (e.g., VRP constraints like capacities) by modifying the final projection layer or the assignment procedure.

- **Weaknesses & Limitations**  
  - Heavily reliant on **ILP-solvable instances** for training data, which may be computationally expensive for large-scale problems.  
  - Requires a beam search to extract discrete solutions; large beam sizes can become computationally costly.  
  - Focuses primarily on the single-depot mTSP. Adapting to multi-depot scenarios or further constraints (e.g., capacity) would need additional tweaks.

- **Relevance to Deterministic VRP Research**  
  - Demonstrates how a neural network can be trained in a supervised manner to solve a **deterministic** multi-vehicle routing problem (mTSP) with strong performance.  
  - Presents a viable alternative to classical local search or exact methods, especially in scenarios where repeated fast inference is necessary (e.g., large-scale or real-time routing contexts).  
  - Underlines the importance of designing **permutation-invariant** architectures for VRP-like tasks to handle variable numbers of cities/vehicles.  

- **Potential Impact**  
  - Contributes a **new deep learning approach** that merges ideas from set-based neural networks and graph embeddings, relevant for researchers exploring hybrid ML-and-OR methods in VRP.  
  - May inspire further research on advanced assignment layers that enforce constraints in a differentiable manner.  
  - Could lead to extended applications (e.g., multi-depot VRP, capacity constraints) by altering the training set and incorporating additional constraints into the output transformation steps.
