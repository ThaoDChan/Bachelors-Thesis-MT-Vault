# Learning 2-Opt Heuristics for Routing Problems via Deep Reinforcement Learning

## Metadata
- **Link to PDF**: [[[dacosta2021]_Learning_2-opt_heuristics_for_routing_problems_via_deep_reinforcement_learning.pdf]]
- **Tags**:
  - #ML-ReinforcementLearning
  - #RL-PolicyGradient
  - #ML-AssistedHeuristics
  - #PointerNetworks
  - #AttentionMechanismAM
  - #LocalSearchHeuristics
  - #MultiVehicleRouting
  - #TSP
  - #CVRP
  - #NeuralCombinatorialOptimization
  - #GraphNeuralNetworks
- **Relevant**: true
- **Fit Score**: 10
- **State of the Art (SoA) Concepts**:
  - DRL-based 2-opt local search for TSP, mTSP, and CVRP
  - Pointer Networks with attention mechanisms
  - Graph convolutional layers (GCN) for embedding
  - Policy gradient (REINFORCE) approach for combinatorial optimization
- **Performance Evaluation**: yes
- **Performance Evaluation Framework**: TSPLIB (and custom random instances)

## Abstract
"Recent works using deep learning to solve routing problems such as the traveling salesman problem (TSP) have focused on learning construction heuristics. Such approaches find good quality solutions but require additional procedures such as beam search and sampling to improve solutions and achieve state-of-the-art performance. However, few studies have focused on improvement heuristics, where a given solution is improved until reaching a near-optimal one. In this work, we propose to learn a local search heuristic based on 2-opt operators via deep reinforcement learning. We propose a policy gradient algorithm to learn a stochastic policy that selects 2-opt operations given a current solution. Moreover, we introduce a policy neural network that leverages a pointing attention mechanism, which can be easily extended to more general k-opt moves. Our results show that the learned policies can improve even over random initial solutions and approach near-optimal solutions faster than previous state-of-the-art deep learning methods for the TSP. We also show we can adapt the proposed method to two extensions of the TSP: the multiple TSP and the Vehicle Routing Problem, achieving results on par with classical heuristics and learned methods."

## Summary
- **Context & Motivation**
  - Addresses a gap in applying deep learning to *improvement* heuristics for routing problems. Prior DL methods primarily focused on *construction* (sequential route generation). This paper proposes a learned 2-opt local search to enhance existing solutions iteratively.
  - Aims to approach near-optimal solutions for deterministic TSP variants (TSP, mTSP, CVRP) by training a policy through reinforcement learning rather than relying on handcrafted improvement rules.

- **Methodology**
  - Proposes a **deep reinforcement learning (DRL)** framework to learn a stochastic policy over 2-opt moves.
  - Uses **policy gradient** (REINFORCE) to maximize expected reward, where the reward is the reduction in route length over the best solution found so far.
  - Builds a **dual encoder** with graph convolutions (for capturing pairwise distance structure) and bi-directional LSTM layers (for encoding the tour order). 
    - The policy decoder employs a pointing (attention) mechanism similar to Pointer Networks. 
    - Outputs two node indices for 2-opt edge swaps in each step.
  - Learns to sample beneficial 2-opt moves that reduce tour cost, balancing exploration with exploitation.

- **Key Contributions**
  1. **Learning an improvement heuristic** rather than purely a construction heuristic. The method can start from any feasible (even random) solution and iteratively improve it.
  2. **Unified DRL architecture** for TSP, multiple TSP (mTSP), and the capacitated VRP (CVRP). Shows how the model can be adapted with masking to accommodate multi-route constraints and capacity restrictions.
  3. **Efficient policy** that can handle 2-opt moves and scales to k-opt expansions by design of its attention-based decoder (though primarily demonstrated with 2-opt).

- **Experimental Setup**
  - Trained on random TSP instances in a 2D plane (20–100 nodes), uniformly distributed. Also tested on:
    - **mTSP** (multiple salesmen) with artificially generated data.
    - **CVRP** with fixed vehicle capacity and random demands.
  - Compared solutions against classical baseline heuristics (greedy insertions, standard 2-opt, Google’s OR-Tools) and other state-of-the-art learning methods (e.g., GAT, Pointer Networks, GCN, GAT-T).
  - Also tested on real TSPLIB benchmarks for TSP, with up to 299 nodes.

- **Results & Findings**
  - **Near-optimal solutions**: On TSP up to 100 nodes, the learned 2-opt approach finds tours with less than ~1% gap after sufficient sampling steps, often outperforming or matching the best known deep RL methods. 
  - Faster improvement and better sample efficiency relative to prior improvement-based models (especially GAT-T).
  - For **mTSP** and **CVRP**, the policy achieves solution quality close to classical heuristics (e.g., LKH3) within fewer iterations, showing strong generalization from smaller to larger node sets.
  - Transfer from TSP50 to TSP100 remains competitive, indicating potential for cross-size generalization.

- **Strengths**
  - **Unified approach** for multiple routing tasks (TSP, mTSP, CVRP) without heavily customized designs. 
  - Shows the **benefits of a learned improvement heuristic**, which can be combined with other constructions or even random solutions.
  - Demonstrates **competitive performance** vs. well-known OR solvers and prior ML-based construction or improvement strategies.

- **Limitations**
  - **Large training data requirement**: policy gradient demands many episodes, leading to high computational cost for training.
  - **Feasibility checks** in mTSP/CVRP slow down the approach for larger problem sizes, since capacity constraints need repeated verification.
  - Does not surpass advanced specialized TSP solvers (like LKH) on all large-scale instances, though it is still close.

- **Relevance to Deterministic VRP**
  - Entirely focused on deterministic VRP settings (Euclidean TSP, mTSP, CVRP). 
  - Proposes an ML-based improvement approach that can integrate with classical heuristics or run standalone.
  - Aligns closely with the thesis goal of reviewing DRL solutions for VRP in deterministic scenarios.

- **Potential Research Extensions**
  - Expanding from 2-opt to full k-opt or more flexible local search moves.
  - Investigating how partial dynamic or stochastic elements could be incorporated (though not the paper’s focus).
  - Improving training efficiency, possibly via advanced RL methods (e.g., actor-critic or off-policy algorithms).