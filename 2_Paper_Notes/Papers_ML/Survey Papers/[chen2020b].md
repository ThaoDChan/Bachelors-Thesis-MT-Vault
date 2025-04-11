# Dynamic Partial Removal: A Neural Network Heuristic for Large Neighborhood Search

## Metadata
- **Link to PDF**: [[[chen2020b]_Dynamic_Partial_Removal-A_Neural_Network_Heuristic_for_Large_Neighborhood_Search.pdf]]
- **Tags**:
  - #ML-ReinforcementLearning
  - #RL-PolicyGradient
  - #RL-ActorCritic
  - #ProximalPolicyOptimization
  - #GraphNeuralNetworks
  - #ML-AssistedLocalSearch
  - #HybridLocalSearchRL
  - #LargeNeighborhoodSearch
  - #AdaptiveLargeNeighborhoodSearch
  - #CVRPTW
  - #DeterministicVRP
  - #LargeScaleVRP
  - #TimeWindowConstraints
  - #HeuristicOrExactBlends
- **Relevant**: true  
- **Fit Score**: 10  
- **State of the Art (SoA) Concepts**:
  - Neural-network-based destroy operator within LNS
  - Hierarchical Recurrent Graph Convolutional Networks (HRGCN)
  - Proximal Policy Optimization (PPO) for policy training
  - Deterministic VRP with time windows (CVRPTW)
  - Adaptive heuristics (ALNS-style) for large-scale routing
- **Performance Evaluation**: yes
- **Performance Evaluation Framework**: Solomon, Gehring & Homberger benchmarks (plus synthetic datasets)

## Abstract
"This paper presents a novel neural network design that learns the heuristic for Large Neighborhood Search (LNS). LNS consists of a destroy operator and a repair operator that specify a way to carry out the neighborhood search to solve the Combinatorial Optimization problems. The proposed approach in this paper applies a Hierarchical Recurrent Graph Convolutional Network (HRGCN) as a LNS heuristic, namely Dynamic Partial Removal, with the advantage of adaptive destruction and the potential to search across a large scale, as well as the context-awareness in both spatial and temporal perspective. This model is generalized as an efficient heuristic approach to different combinatorial optimization problems, especially to the problems with relatively tight constraints. We apply this model to vehicle routing problem (VRP) in this paper as an example. The experimental results show that this approach outperforms the traditional LNS heuristics on the same problem as well."

## Summary
- **Context and Motivation**  
  - Proposes a machine learning-based destroy operator within Large Neighborhood Search (LNS), referred to as “Dynamic Partial Removal” (DPR).  
  - Motivated by the need to solve combinatorial optimization problems like VRP at scale, especially when classical or static heuristics struggle with large problem sizes.

- **VRP Variant and Core Focus**  
  - Focuses on the Capacitated Vehicle Routing Problem with Time Windows (CVRPTW), which is a deterministic VRP where vehicles have limited capacity and each customer has a time window.  
  - Emphasizes the importance of adaptive destroy operators in LNS to handle time-window constraints and large-scale instances (up to 800 nodes).

- **Methodology & Novel Contributions**  
  - **Dynamic Partial Removal (DPR) Destroy Operator**  
    - Selects an “anchor node” and removes a cluster of nodes near it, guided by a learned “route coefficient.”  
    - Degree of destruction is partly randomized and adaptive: it can cluster node removals along routes or by distance.  
    - The removed nodes are reinserted using a least-cost insertion heuristic.
  - **Hierarchical Recurrent Graph Convolutional Network (HRGCN)**  
    - Learns how to choose the anchor node and route coefficients at each iteration.  
    - Integrates a spatial hierarchy:
      1. A graph convolutional layer (GCN) built on k-nearest neighbor arcs.  
      2. Two additional GCN layers built along existing VRP routes, forward and backward, to capture route context.  
    - Introduces a temporal dimension (via Gated Recurrent Units, GRUs) that retains information from previous iterations (i.e., previous anchor choices).  
    - Outputs: probability distribution over nodes (for anchor selection) and parameters of a Beta distribution for route coefficients (to control how many nodes are removed).

- **Reinforcement Learning Approach**  
  - Uses Proximal Policy Optimization (PPO), an actor-critic policy gradient method, to train the HRGCN.  
  - Reward is defined by improvement in the objective (reduction of route cost), balancing exploration and exploitation.  
  - Training is carried out on synthetic CVRPTW instances, mixing different scales (25–200 nodes, and a separate model for 400–800 nodes).

- **Experimental Results**  
  - Benchmarked against classical heuristics (random removal, ALNS, SISR) on Solomon and Gehring & Homberger datasets.  
  - Achieves better solution quality (lower total distance/cost) and comparable or lower computational times (seconds for up to 200 nodes, tens of seconds for 800 nodes).  
  - Shows that combining multiple anchors each iteration (e.g., picking multiple nodes to destroy) and incorporating GRU-based memory yields faster convergence and better solutions.  
  - Demonstrates scalability to large instances, surpassing typical LNS heuristics in performance.

- **Strengths**  
  - **Adaptive**: The learned DPR operator automatically adjusts removal intensity and location, leveraging both route structure and iterative feedback.  
  - **Scalability**: Demonstrates potential for VRP instances with 800 customers.  
  - **Integration with LNS**: Enhances classical LNS by using advanced GNN-based learning to replace hand-crafted removal heuristics.

- **Limitations & Future Work**  
  - Approach relies on a well-trained model for large-scale data; model capacity and training time can increase with instance size.  
  - Currently tested primarily on CVRPTW; authors suggest it can generalize to other VRP variants (e.g., multi-depot, pickup and delivery) but no direct evidence provided in the paper.  
  - Reinsertions still use a standard greedy method; might further improve if the reinsertion step is also learned or optimized.

- **Relevance to Deterministic VRP**  
  - Directly addresses deterministic VRP with explicit time-window constraints.  
  - Uses a novel ML-based method (reinforcement learning with GCN) to tackle large-scale, deterministic problems.  
  - Contributes a strong example of ML-assisted local search, aligning well with emerging trends in combining deep learning with heuristic optimization.

- **Key Takeaways for Thesis**  
  - Showcases how a specialized GNN architecture (HRGCN) can handle spatiotemporal relationships in route-based data.  
  - Demonstrates effective synergy between advanced RL (PPO) and classical neighborhood search frameworks (LNS).  
  - Offers compelling evidence that carefully designed neural heuristics can outperform existing metaheuristics in deterministic VRP settings, especially as problem size grows.