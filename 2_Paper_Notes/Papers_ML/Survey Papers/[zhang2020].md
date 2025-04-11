# Multi-Vehicle Routing Problems with Soft Time Windows: A MultiAgent Reinforcement Learning Approach

## Metadata
- **Link to PDF**: [[[zhang2020]_Multi-vehicle_routing_problems_with_soft_time_windows-A_multi-agent_reinforcement_learning_approach.pdf]]
- **Tags**:
  - #ML-ReinforcementLearning
  - #RL-PolicyGradient
  - #AttentionMechanismAM
  - #PointerNetworks
  - #NeuralCombinatorialOptimization
  - #TransformerModels
  - #mVRPSTW
  - #MultiAgentRLCoordination
  - #TimeWindowConstraints
  - #DeterministicVRP
  - #MultiVehicleRouting
- **Relevant**: true  
- **Fit Score**: 10  
- **State of the Art (SoA) Concepts**:
  - Multi-agent reinforcement learning for multi-vehicle VRP
  - Attention-based encoder-decoder architecture
  - Policy gradient (REINFORCE) for combinatorial optimization
  - Incorporation of soft time windows via penalty functions
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: Uses synthetic data (not standard Solomon or CVRPLIB)

## Abstract
"Multi-vehicle routing problem with soft time windows (MVRPSTW) is an indispensable constituent in urban logistics distribution systems. Over the past decade, numerous methods for MVRPSTW have been proposed, but most are based on heuristic rules that require a large amount of computation time. With the current rapid increase of logistics demands, traditional methods incur the dilemma between computational efficiency and solution quality. To efficiently solve the problem, we propose a novel reinforcement learning algorithm called the Multi-Agent Attention Model that can solve routing problem instantly benefit from lengthy offline training. Specifically, the vehicle routing problem is regarded as a vehicle tour generation process, and an encoder-decoder framework with attention layers is proposed to generate tours of multiple vehicles iteratively. Furthermore, a multi-agent reinforcement learning method with an unsupervised auxiliary network is developed for the model training. By evaluated on four synthetic networks with different scales, the results demonstrate that the proposed method consistently outperforms Google OR-Tools and traditional methods with little computation time. In addition, we validate the robustness of the well-trained model by varying the number of customers and the capacities of vehicles."

## Summary
- **Context & Goal**
  - Addresses the Multi-Vehicle Routing Problem with Soft Time Windows (MVRPSTW), where vehicles have capacity constraints and service time windows can be exceeded with associated penalty costs.
  - Aims to develop a fast, high-quality solution method that balances computational efficiency with solution quality.

- **ML Technique & VRP Variant**
  - Proposes a **Multi-Agent Attention Model (MAAM)** based on **deep reinforcement learning**.
  - Specifically leverages:
    - An **attention-based encoder-decoder** (inspired by Transformer-like architectures).
    - **Multi-agent RL** coordination, where each vehicle is treated as an agent.
  - Variant: **Deterministic MVRP with soft time windows** (MVRPSTW).

- **Methodology**
  - **Encoder**: Uses multi-head self-attention layers to generate embeddings for the depot and customers. Incorporates demand, location, and time-window data.
  - **Decoder**: Sequentially assigns customers to vehicles under capacity constraints. Uses:
    - **Context embedding** that reflects vehicle states (remaining capacity, last visited node).
    - Multi-head attention to derive probabilities of visiting next customers.
  - **Training**:  
    - Applies **REINFORCE** (policy gradient) with a baseline.  
    - Large-scale offline training on randomly generated instances.  
    - Once trained, the model provides near-instant solutions for new problem instances.
  - **Soft Time Windows**: Penalty-based objective function penalizes early and late arrivals, integrated into the RL reward.

- **Experiments & Results**
  - Evaluates on synthetic datasets with 20, 50, 100, and 150 customers.
  - Compares against:
    1. Two forms of Genetic Algorithm (GA)
    2. Two forms of Iterated Local Search (ILS)
    3. Google OR-Tools solver
  - **Key Findings**:
    - MAAM consistently produces better or comparable solutions with much shorter inference times, especially on larger instances (100–150 customers).
    - It outperforms classical heuristics (GA, ILS) and Google OR-Tools in most settings for total cost (travel distance + penalty).
    - Computation time remains roughly constant with problem size after training (inference in seconds vs. minutes/hours for other methods).
  - **Robustness**:
    - Model remains effective when the number of customers or vehicle capacities deviate from training conditions (e.g., fewer/more customers, different capacities).  
    - Demonstrates generalization capability within similar problem scales.

- **Strengths & Contributions**
  - Introduces a **multi-agent** RL approach with attention to handle multi-vehicle dispatch coordination.
  - Achieves high computational efficiency due to offline training; near-instant online inference.
  - Demonstrates adaptability to realistic constraints (soft time windows, capacity).
  - Shows robust generalization to nearby problem variants (slightly changed number of customers or capacities).

- **Limitations & Future Work**
  - Uses synthetic datasets rather than standardized or real-world data (e.g., Solomon benchmarks).
  - Focuses on MVRPSTW but does not explore multi-depot or heterogeneous fleets.
  - Future directions may include extending to more complex constraints or large-scale real logistics systems, as well as exploring online or dynamic settings.

- **Relevance to Deterministic VRP Research**
  - Highly relevant example of **attention-based RL** for deterministic VRP with time window penalties.
  - Offers insight into how multi-agent reinforcement learning can be applied effectively for large-scale or more complex VRP variants.
  - Illustrates potential synergy between advanced neural network architectures (attention/transformers) and combinatorial optimization tasks.