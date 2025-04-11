# Deep Reinforcement Learning Algorithm for Fast Solutions to Vehicle Routing Problem with Time-Windows

## Metadata
**Link to PDF**: [[[gupta2022]_Deep_reinforcement_learning_algorithm_for_fast_solutions_to_vehicle_routing_problem_with_timewindows.pdf]]
**Tags**:  
- #ML-ReinforcementLearning  
- #RL-ValueBased  
- #RL-DQN  
- #AttentionMechanismAM  
- #ExperienceReplay  
- #NeuralCombinatorialOptimization  
- #CVRPTW  
- #DeterministicVRP  
- #ML-AssistedHeuristics  
- #LargeScaleVRP  

**Relevant**: true  
**Fit Score**: 10  
**State of the Art (SoA) Concepts**:  
- Deep Reinforcement Learning with Value-Based Method (DQN)  
- Attention-based Encoder-Decoder  
- Experience Replay  
- Capacitated VRP with Time Windows  
- Post-processing Insertion Heuristic  
**Performance Evaluation**: yes  
**Performance Evaluation Framework**: Solomon benchmark datasets  


## Abstract
“Vehicle routing problem (VRP) is a well known NP-hard combinatorial optimization problem having several variants. In this paper, we consider VRP along with additional constraints of capacity and time-windows (CVRPTW) and aim to provide a fast and approximately optimal solutions to large-scale CVRPTW problems. We present a deep Q-network with encoder-decoder based reinforcement learning approach to solve CVRPTW. The encoder is based on the attention mechanism whereas decoder is fully connected neural network. Via numerical experiments on benchmark datasets, we show the efficacy and computational speed our approach compared to baseline heuristics, a meta-heuristic algorithm, and a multi-agent reinforcement learning (RL) based framework.”


## Summary
- **Problem & Motivation**
  - Addresses the **Capacitated Vehicle Routing Problem with Time Windows (CVRPTW)**, a variant of VRP that includes both vehicle capacity constraints and strict customer time windows.
  - Motivated by large-scale delivery challenges, especially last-mile delivery under time constraints. Seeks a fast, near-optimal solution method.
  - States that while exact MILP approaches can yield optimal solutions, they do not scale well; meta-heuristics handle larger instances but may be slow or require extensive tuning. Reinforcement Learning (RL) is proposed as a way to scale more gracefully.

- **Core Contribution**
  - Proposes a **deep Q-network (DQN)** approach, framed as a value-based RL solution to CVRPTW.
  - Uses an **attention-based encoder** to extract features of the nodes (customers) and a fully connected **decoder** to output Q-values over feasible actions (vehicle-customer pairs).
  - Incorporates feasibility checks (capacity and time windows) by masking infeasible customers. Multiple vehicles operate in parallel, each choosing its next customer based on maximum Q-value, with conflicts resolved by picking the highest Q-value assignment.
  - Post-processing with an **insertion heuristic** refines routes by redistributing customers among underutilized vehicles to reduce total distance or the number of vehicles used.

- **Methodology**
  1. **State Representation**: 
     - State inputs include distance between current and feasible customers, time-window start and end, waiting times, current vehicle load, and total time traveled.
     - Normalization is used on distances, times, and demands to keep the input range consistent.
  2. **Action Space**: 
     - For each active vehicle, all customers not yet served and deemed “feasible” by capacity and time-window constraints form the action set.
     - A single environment step involves assigning each active vehicle to its best Q-value customer, resolving any conflicts where multiple vehicles select the same customer.
  3. **Reward Structure**:
     - Step-wise reward combines three terms: negative distance, positive leftover time in the time window, and negative waiting time before service.
     - A global reward term boosts the solution if it achieves a high fulfillment ratio (fraction of served customers).
  4. **Model Architecture**:
     - **Encoder**: Multi-head self-attention with 8 heads and 2 layers, embedding size = 90.
     - **Decoder**: Two fully connected layers, with layer normalization and ReLU activation in between, then a linear output for Q-values.
     - **Training**: Uses an experience replay buffer of size 100,000, batch size = 32, and stochastic gradient descent with MSE loss. An \(\epsilon\)-greedy exploration strategy decays over training episodes.

- **Experiments**
  - **Training Data**: 20 small-scale synthetic instances (25 customers, 13 vehicles). Customer locations randomly distributed in a 2D plane, demands from an exponential distribution, time windows from a uniform or Gaussian distribution.
  - **Testing**: Evaluated on **Solomon benchmark** instances (25- and 50-customer sets), covering three location patterns (clustered, random, and mixed) under two capacity types.
  - **Baselines**: 
    1. Genetic Algorithm meta-heuristic
    2. MARDAM (a multi-agent deep RL approach)
    3. Two greedy heuristics (distance-first vs. time-window-first)
  - **Results**:
    - The proposed RL + insertion heuristic (RH) produces feasible solutions that often match or reduce the *number of vehicles* compared to all baselines, sometimes outperforming GA on that metric.
    - *Distance traveled* is higher than GA, but typically competitive or better than purely heuristic or multi-agent RL baselines.
    - *Computation time* is notably faster than the GA approach (often by factors of 10 or more) and roughly comparable to the multi-agent approach.  
    - The authors observe that the reward term penalizing waiting time sometimes causes vehicles to pick more distant customers with overlapping time windows, increasing total distance slightly.

- **Key Findings & Strengths**
  - Demonstrates that a **value-based RL architecture** with an attention-based encoder and a simple insertion heuristic can yield good solutions to the CVRPTW quickly, especially in terms of vehicle count minimization.
  - Emphasizes a *scalable* approach: each decision step is relatively straightforward, allowing for parallel assignment of multiple vehicles.
  - Clearly shows potential in balancing solution quality (vehicle usage) with speed, which is critical for large-scale real-world applications.

- **Limitations & Future Work**
  - The total distance cost sometimes becomes larger because the model strongly penalizes waiting, thus favoring “time-close” over “distance-close” customers.
  - The authors note ongoing work to close this distance gap by refining the reward function or post-processing steps.
  - Plan to extend to **heterogeneous fleets** or dynamic routing (though the current paper remains deterministic).

- **Relevance to Deterministic VRP Research**
  - A direct example of a **reinforcement learning** approach for a classical deterministic VRP variant (CVRPTW).
  - The method integrates well with conventional heuristics, illustrating a popular “hybrid” approach in modern VRP solution strategies.
  - Demonstrates standard use of **Solomon** benchmarks, a key dataset for deterministic VRP research and performance evaluation.

- **Overall Significance**
  - Shows that **attention-based RL** can handle the additional complexity of capacity and time windows without excessive computational overhead.
  - Provides a stepping stone for more advanced frameworks that combine RL with other optimization strategies or incorporate refined cost functions.
  - Reinforces the growing trend of applying deep learning to core operations research problems, especially where time constraints and large instance sizes are crucial.