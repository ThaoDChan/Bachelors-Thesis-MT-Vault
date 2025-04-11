# Learning to Solve Vehicle Routing Problems with Time Windows through Joint Attention

## Metadata
- **Link to PDF**: [[[falkner2020]_Learning_to_solve_vehicle_routing_problems_with_time_windows_through_joint_attention.pdf]]  
- **Tags**:  
  - #ML-ReinforcementLearning  
  - #RL-PolicyGradient  
  - #NeuralCombinatorialOptimization  
  - #AttentionMechanismAM  
  - #TransformerModels  
  - #PointerNetworks  
  - #Seq2Seq  
  - #PolicySearch  
  - #CVRPTW  
  - #TimeWindowConstraints  
- **Relevant**: true  
- **Fit Score**: 10  
- **State of the Art (SoA) Concepts**:  
  - Joint attention approach for parallel route construction  
  - Policy gradient (REINFORCE) for CVRP with time windows  
  - Comparison with OR-Tools meta-heuristics  
  - Enhanced transformer-based (attention) architectures for constrained routing  
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: Solomon-based distributions (R201-like), comparisons with Google OR-Tools  

## Abstract

"Many real-world vehicle routing problems involve rich sets of constraints with respect to the capacities of the vehicles, time windows for customers etc. While in recent years first machine learning models have been developed to solve basic vehicle routing problems faster than optimization heuristics, complex constraints rarely are taken into consideration. Due to their general procedure to construct solutions sequentially route by route, these methods generalize unfavorably to such problems. In this paper, we develop a policy model that is able to start and extend multiple routes concurrently by using attention on the joint action space of several tours. In that way the model is able to select routes and customers and thus learns to make difficult trade-offs between routes. In comprehensive experiments on three variants of the vehicle routing problem with time windows we show that our model called JAMPR works well for different problem sizes and outperforms the existing state-of-the-art constructive model. For two of the three variants it also creates significantly better solutions than a comparable meta-heuristic solver."

## Summary
- **Motivation & Problem Addressed**  
  - Extends machine learning (ML) approaches for Vehicle Routing Problems (VRP) to variants involving **time windows** (CVRPTW).  
  - Observes that many ML-based solvers for simpler VRPs (like basic CVRP) underperform on richer constraints (time windows, capacity) if they construct one route at a time.  

- **Key Contribution**  
  - Proposes **JAMPR (Joint Attention Model for Parallel Route-Construction)**:  
    - Learns to construct multiple routes **in parallel** rather than strictly one route at a time.  
    - Uses **attention** on both vehicle and node embeddings, enabling the policy network to access a more comprehensive global state (remaining capacity, time feasibility, etc.).  
  - Demonstrates that constructing several tours concurrently improves solution quality on VRPTW variants compared to sequential methods.  

- **Machine Learning Approach**  
  1. **Reinforcement Learning**:  
     - Uses **policy gradient (REINFORCE)** to train the constructive policy.  
     - Learns a parameterized policy \(\pi_\theta\) that sequentially assigns customers to active vehicles with time-window constraints.  
  2. **Attention Mechanism**:  
     - Builds on the **transformer-style** self-attention encoder from Kool et al. (the “Attention Model” or AM).  
     - Adds **vehicle embeddings** (including current time, remaining capacity, last visited node) and **tour embeddings** (aggregations of visited nodes).  
     - Maintains a joint action space of \((\text{vehicle}, \text{customer})\) pairs so the model directly learns to pick which vehicle serves which node next.  

- **Methodology**  
  - **State Representation**:  
    - Extended beyond node demand and coordinates; now includes each vehicle’s route, position, and timing.  
    - Parallel route construction: at each step, the policy attends over all feasible (vehicle, node) actions.  
  - **Training**:  
    - Employs REINFORCE with rollout baseline.  
    - Trained on synthetic instances that mimic Solomon’s R201-type distributions (for time windows).  
  - **Evaluation**:  
    - Compares performance against:
      1. **AM+TW** (an adapted single-tour-at-a-time baseline using attention).  
      2. **Google OR-Tools** (meta-heuristic with guided local search or automatic local search).  
    - Conducted experiments on three CVRPTW variants (hard time windows, partially soft, fully soft with penalties for early/late arrivals).  
    - Also tested on classic CVRP for completeness.  

- **Results & Key Findings**  
  - **Superior solutions on CVRPTW**:  
    - Outperforms the single-route construction approach AM+TW by a significant margin in solution cost and fewer vehicles used.  
    - Outperforms or closely matches Google OR-Tools (often beating it on two of three CVRPTW settings).  
    - Achieves improved solution feasibility and better usage of vehicle capacity/time feasibility constraints.  
  - **Parallel route construction** clearly helps in scenarios where time-window constraints reduce the feasible search space.  
  - **Runtime Efficiency**:  
    - In typical single-instance inference mode, JAMPR can be slower than the naive baseline for small sample sizes, but remains comparable and often faster than advanced local search (in many cases).  
    - Sampling from the policy consistently improves final solution quality.  

- **Strengths**  
  - **Novel architecture** that introduces a more expressive representation and joint action space for multi-vehicle assignment.  
  - Learns a constructive policy without requiring an external initial solution or iterative local search.  
  - Demonstrates robust performance across different CVRPTW variants, underscoring its potential for real-world constrained routing problems.  

- **Limitations & Gaps**  
  - Focused primarily on **static, deterministic** time windows; does not address dynamic or stochastic changes.  
  - Evaluated up to problem sizes of 50 customers; real-world routing may require scaling to larger instances.  
  - Potentially memory-intensive since it tracks a richer state representation (especially as concurrency grows).  
  - Jampr does not incorporate advanced local-search improvement steps; combining with local search might further enhance solution quality but is left for future work.  

- **Conclusion & Relevance**  
  - This paper showcases that a **joint attention** model with parallel route expansion can significantly improve time-window–constrained VRP solutions over purely sequential ML-based strategies.  
  - The approach aligns with the emerging direction in **neural combinatorial optimization**—particularly for **deterministic** VRP with capacity and time-window constraints.  
  - It is highly relevant for any research aiming to unify **reinforcement learning** with advanced **attention-based** policies for complex VRPs.  
  - Suggests future efforts in **online decoding** or partial route scheduling as an extension once real-time data arrives, though that is outside the scope of purely deterministic offline VRP.  