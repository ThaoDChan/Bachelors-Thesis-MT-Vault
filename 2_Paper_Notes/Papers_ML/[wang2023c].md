# Routing optimization with Monte Carlo Tree Search-based multi-agent reinforcement learning

## Metadata
- **Link to PDF**: [[[wang2023c] Routing optimization with Monte Carlo Tree Search-based multi-agent reinforcement learning.pdf]]  
- **Tags**:  
  #ML-SupervisedFromOptimal  
  #ML-ReinforcementLearning  
  #RL-PolicyGradient  
  #MonteCarloTreeSearch  
  #GraphNeuralNetworks  
  #TransformerModels  
  #PointerNetworks  
  #NeuralCombinatorialOptimization  
  #MultiAgentRL  
  #CVRP  
  #TSP  
  #LargeScaleVRP  

- **Relevant**: true  
- **Fit Score**: 10  
- **State of the Art (SoA) Concepts**:
  - Multi-agent reinforcement learning (MARL) with communication
  - Monte Carlo Tree Search (MCTS) combined with policy gradient
  - Graph Neural Networks (GNN) with transformer-based encoder
  - Hybrid supervised-imitation + RL pipeline (inspired by AlphaGo)
  - TSP and CVRP benchmarks
- **Performance Evaluation**: yes (custom benchmark instances)  
- **Performance Evaluation Framework**: random Euclidean instances (not standard public benchmarks)

## Abstract
"Vehicle routing (VRP) and traveling salesman problems (TSP) are classical and interesting NP-hard routing combinatorial optimization (CO) with practical significance. While moving forward with artificial intelligence, researchers are paying more and more attention to applying machine learning to classical CO problems. However, traditional reinforcement learning faces challenges like reward sparsity and unstable training, so it is necessary to assist agents in finding high-quality routings in the initial model training stage to obtain more positive feedback. This paper proposes a novel Monte Carlo Tree Search (MCTS)-based two-stage multi-agent reinforcement learning training pipeline (MCRL) in which we also design a multifunctional reward function, improving efficiency, accuracy, and diversity to guide agents to learn the routings over graphs better. Besides, previous approaches are frequently too sluggish in runtime to be useful in contexts with sparsely connected networks and uncertain traffic. As an alternative, we design a model based on graph neural networks that can execute multi-agent routing in a sparsely connected graph with constantly changing traffic circumstances. Also, the agents are better equipped to collaborate online and adjust to changes thanks to our learned communication module."

## Summary
- **Core Contribution & Motivation**  
  - Proposes a *two-stage training pipeline* for vehicle routing that combines *supervised learning* (to mitigate reward sparsity) and *reinforcement learning* (to optimize routing policies).  
  - Integrates *Monte Carlo Tree Search (MCTS)* with a *graph neural network (GNN)*-based policy architecture to handle TSP and CVRP.  
  - Introduces a *multi-agent reinforcement learning (MARL) scheme* to coordinate multiple vehicles (agents) in a fleet, enabling information exchange through a learned communication module.

- **Methodology**  
  - **Modeling**:  
    - Casts TSP/VRP into a Markov decision process (MDP) where each vehicle (agent) selects next nodes in sequence.  
    - Represents the problem using a GNN (named *Graph Message Passing Pointer Network*, GMPPN) that includes an attention-based encoder (Transformer-like) and multiple decoders (one per agent) for constructing diverse routing policies.  
    - Incorporates multi-agent communication via an attention-based mechanism that exchanges each agent’s state and future plans.
  - **Training Pipeline**:  
    1. **Supervised Pre-Training**: Trains a policy network on solutions derived from a solver (e.g., LKH3) to overcome initial reward sparsity, effectively providing a “hot start.”  
    2. **Reinforcement Learning**: Enhances the policy by optimizing an explicit *reward function* factoring global route feasibility, path length, and local efficiency.  
    3. **MCTS Integration**: Uses WU-UCT (a parallel MCTS variant) guided by an A* heuristic to explore promising expansions of partial solutions. MCTS results further refine the policy gradient updates.

- **VRP Variant(s) Addressed**  
  - Primarily **TSP** and **CVRP** under deterministic settings.  
  - Demonstrates the approach on up to 100-node random Euclidean instances for both TSP and CVRP.

- **Key Results**  
  - Outperforms or is on-par with other learning-based methods (e.g., Attention Model, NeuRewriter, etc.) on small to medium TSP/CVRP, especially after the supervised pre-training.  
  - Achieves lower route lengths (closer to optimal) than many pure RL or classical heuristic baselines (2-opt, Clarke-Wright, nearest insertion).  
  - Multi-agent communication + multi-decoder approach yields faster convergence and more diverse solutions than a single-agent RL model.  
  - Shows stable training curves (reduced variance) thanks to the two-stage pipeline—compared to purely reinforcement-based schemes that often struggle with reward sparsity early on.  

- **Strengths**  
  - **Hybrid pipeline** integrating best practices from imitation learning, reinforcement learning, and MCTS.  
  - **Flexible GNN architecture** that can incorporate both local node/edge features and global multi-agent communications.  
  - Demonstrates improved runtime vs. exact solvers (LKH3), with better generalization than standard heuristics.  
  - Applicable to TSP and CVRP with minimal changes (via subgraph sampling for VRP demands).

- **Weaknesses / Limitations**  
  - The approach includes multiple complex components (MCTS, GNN, multi-decoder), which may be challenging to implement and tune.  
  - Tests on randomly generated Euclidean instances rather than standard public VRP benchmarks (making direct comparisons with classic results more difficult).  
  - May still be relatively heavy computationally for very large-scale instances (though multi-agent can help speed up the search).

- **Relevance to Deterministic VRP**  
  - Demonstrates a purely deterministic VRP setting in experiments, even though it hints at possible extensions to changing traffic conditions.  
  - Provides insights on how supervised pre-training from solver-based solutions can mitigate reward sparsity in RL-based VRP methods.

- **Conclusion & Potential Impact**  
  - The paper’s methodology—combining GNN, multi-agent RL, supervised imitation, and MCTS—underlines a robust framework for VRP.  
  - Highlights stable learning via a staged pipeline, offering improved solution quality over purely heuristic or purely RL-based methods.  
  - Potentially valuable in a *deterministic VRP* setting for large-scale, multi-vehicle routing, offering strong generalization and runtime benefits.  
  - Could be integrated into next-generation ML-augmented routing solvers as a multi-agent parallel approach with improved exploration via MCTS.