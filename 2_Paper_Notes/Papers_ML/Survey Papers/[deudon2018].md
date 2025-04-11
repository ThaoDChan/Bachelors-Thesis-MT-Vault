# Learning Heuristics for the TSP by Policy Gradient

## Metadata
- **Link to PDF**: [[[deudon2018]_Learning_heuristics_for_the_TSP_by_policy_gradient.pdf]]
- **Tags**:
  - #ML-ReinforcementLearning
  - #RL-PolicyGradient
  - #RL-ActorCritic
  - #NeuralCombinatorialOptimization
  - #PointerNetworks
  - #TransformerModels
  - #AttentionMechanismAM
  - #HeuristicOrExactBlends
  - #ML-AssistedLocalSearch
  - #HybridLocalSearchRL
  - #TSP
  - #TransferLearning
- **Relevant**: true
- **Fit Score**: 10
- **State of the Art (SoA) Concepts**:
  - Policy gradient with actor-critic
  - Attention-based neural network (transformer-like) for TSP
  - Pointer network decoder
  - Hybrid method with local search (2-opt)
  - Transfer to unseen TSP sizes
- **Performance Evaluation**: yes
- **Performance Evaluation Framework**: 
  - Custom synthetic TSP instances (n=20,50,100)
  - Comparison with Concorde (exact), Christofides, OR-Tools, Lin-Kernighan Heuristic

## Abstract
“The aim of the study is to provide interesting insights on how efficient machine learning algorithms could be adapted to solve combinatorial optimization problems in conjunction with existing heuristic procedures. More specifically, we extend the neural combinatorial optimization framework to solve the traveling salesman problem (TSP). In this framework, the city coordinates are used as inputs and the neural network is trained using reinforcement learning to predict a distribution over city permutations. Our proposed framework differs from the one in [1] since we do not make use of the Long Short-Term Memory (LSTM) architecture and we opted to design our own critic to compute a baseline for the tour length which results in more efficient learning. More importantly, we further enhance the solution approach with the well-known 2-opt heuristic. The results show that the performance of the proposed framework alone is generally as good as high performance heuristics (ORTools). When the framework is equipped with a simple 2-opt procedure, it could outperform such heuristics and achieve close to optimal results on 2D Euclidean graphs. This demonstrates that our approach based on machine learning techniques could learn good heuristics which, once being enhanced with a simple local search, yield promising results.”

## Summary
- **Context & Motivation**
  - Addresses the challenge of solving the Traveling Salesman Problem (TSP), a core combinatorial optimization task that is also a special case of the Vehicle Routing Problem (VRP).
  - Motivated by the need for data-driven heuristics that learn directly from instance structure without relying on domain-specific handcrafted features.

- **Key Contribution**
  - Proposes a reinforcement learning approach using policy gradient to learn heuristics for the TSP.
  - Employs a self-attentive encoder (instead of LSTM) to create representations of input cities, thus reducing complexity and improving learning efficiency.
  - Integrates a custom critic network for a strong baseline that stabilizes training.
  - Augments the learned policy with a simple 2-opt local search to refine solutions post-inference.

- **Methodology**
  - **Encoder**: Uses an attention-based “transformer-like” architecture on city coordinates. The architecture encodes each city into a continuous representation, capturing global context through multiple heads of attention.
    - Batch normalization and multi-head attention handle variable-sized inputs.
    - ReLU and feed-forward layers transform features at each step.
  - **Decoder**: 
    - Adopts a pointer-network style mechanism that sequentially “points” to the next city. 
    - Instead of an LSTM state, it only remembers the last three visited cities in a query vector. 
    - Produces a probability distribution over the remaining unvisited cities at each decoding step.
  - **Policy Gradient & Actor-Critic**:
    - Trained via REINFORCE with a learned baseline (critic). 
    - The reward is the negative of tour length (or directly the tour length if formulated as a minimization).
    - The critic shares the same encoder as the actor, using attention to produce a global “glimpse” embedding that is mapped to an estimated tour length. 
    - The difference between actual tour length and critic’s predicted baseline reduces gradient variance.

- **Experiments & Results**
  - Evaluated on synthetic 2D Euclidean TSP instances with 20, 50, and 100 nodes.
  - Compares solution quality against:
    - Concorde (exact solver),
    - Christofides heuristic,
    - Google OR-Tools,
    - Lin-Kernighan Heuristic (LK-H),
    - Google Brain’s original Pointer Network approach.
  - **Performance**:
    - Achieves near-optimal tour lengths (close to Concorde) for TSP20 and TSP50 with low inference time.
    - When extended to TSP100 (using only the model trained on TSP50), the approach remains competitive, showing some capability for generalization/transfer to larger problem sizes.
    - The 2-opt refinement further reduces the final tour length, outperforming some standard heuristics.
  - **Computation Time**:
    - Inference is fast on GPUs; improvements gained by sampling multiple tours quickly and selecting the best, then refining with 2-opt.
    - Suggests a practical synergy of learned policy plus small local search for improved solution quality.

- **Strengths & Limitations**
  - **Strengths**:
    - Demonstrates that attention-based neural networks can effectively learn partial policies for routing tasks, removing the LSTM overhead.
    - Shows synergy between machine learning (to find good initial tours) and 2-opt local search (to refine them).
    - Indicates possible transfer learning effects: A model trained on TSP50 can handle TSP100 with only moderate performance degradation.
  - **Limitations**:
    - Focuses on the TSP without additional capacity or time-window constraints; the extension to more complex VRPs is not fully explored here.
    - Uses synthetic, uniformly distributed data, so real-world coordinate distributions or specific VRP constraints might require additional adaptation.

- **Implications for Deterministic VRP**
  - Although tested only on TSP, the approach is relevant to broader deterministic VRPs: 
    - The underlying policy gradient architecture and attention mechanisms could be adapted to variants (e.g., capacitated VRP).
    - The local search enhancement is generalizable to other route-based tasks.
  - Emphasizes that combining ML-based policies with well-known OR heuristics can yield faster solutions without sacrificing performance.

- **Conclusion & Future Work**
  - Reinforcement learning with attention-based encoders can learn competitive heuristics for TSP, with or without local refinement.
  - Highlights the promise of “AI-OR hybrid” solutions.
  - The paper suggests investigating constrained TSP variants, e.g., TSP with time windows, as a logical extension—relevant for real-life VRP constraints.
  - Encourages further research on bridging data-driven models and existing optimization engines for more complex deterministic routing problems.