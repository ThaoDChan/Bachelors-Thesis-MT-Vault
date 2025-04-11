# Reinforcement Learning with Combinatorial Actions: An Application to Vehicle Routing

## Metadata

- **Link to PDF**: [[[delarue2020]_Reinforcement_learning_with_combinatorial_actions-An_application_to_vehicle_routing.pdf]]
- **Tags**:
  - #ML-ReinforcementLearning
  - #RL-ValueBased
  - #PolicyIteration
  - #ML-AssistedExact
  - #ML-AssistedBranchAndBound
  - #NeuralCombinatorialOptimization
  - #CVRP
  - #BranchAndCut
  - #ORtoolsPackage
  - #DeterministicVRP
- **Relevant**: true
- **Fit Score**: 9
- **State of the Art (SoA) Concepts**:
  - Value-based RL with combinatorial actions
  - Approximate dynamic programming for CVRP
  - Mixed-integer programming (branch-and-cut) for action selection
  - Single-instance vs. distributional RL approaches
- **Performance Evaluation**: yes
- **Performance Evaluation Framework**: CVRPLIB (for benchmark instances) and random instances (from [12])

## Abstract
"Value-function-based methods have long played an important role in reinforcement learning. However, finding the best next action given a value function of arbitrary complexity is nontrivial when the action space is too large for enumeration. We develop a framework for value-function-based deep reinforcement learning with a combinatorial action space, in which the action selection problem is explicitly formulated as a mixed-integer optimization problem. As a motivating example, we present an application of this framework to the capacitated vehicle routing problem (CVRP), a combinatorial optimization problem in which a set of locations must be covered by a single vehicle with limited capacity. On each instance, we model an action as the construction of a single route, and consider a deterministic policy which is improved through a simple policy iteration algorithm. Our approach is competitive with other reinforcement learning methods and achieves an average gap of 1.7% with state-of-the-art OR methods on standard library instances of medium size."

## Summary
- **Context & Motivation**  
  - Addresses the Capacitated Vehicle Routing Problem (CVRP), a classic deterministic VRP variant where a single vehicle (or fleet of identical vehicles) with capacity constraints must serve all cities.  
  - Proposes a reinforcement learning (RL) framework that directly accommodates a large combinatorial action space.  

- **Core Contribution**  
  - Introduces a policy iteration scheme in which the action space (i.e., feasible routes) is combinatorial and cannot be trivially enumerated.  
  - Solves the action selection step via a mixed-integer program (MIP), effectively exploiting branch-and-cut techniques to select optimal routes under the learned value function.  
  - Employs a small neural network with ReLU activations to approximate the cost-to-go (value function), trained on sampled trajectories of the current policy.  

- **Method & Approach**  
  - Frames CVRP as a sequential decision process: each state specifies which cities remain to be served, and each action is a route covering a subset of unvisited cities.  
  - Uses approximate dynamic programming:  
    1. **Policy Evaluation**: Roll out a current policy on random start states, gathering state–action–cost data.  
    2. **Value Function Learning**: Train a neural network on these sampled data points to predict future costs.  
    3. **Policy Improvement**: Solve a MIP (branch-and-cut) at each decision to choose the route (action) that minimizes immediate travel cost plus the learned value of the post-action state.  
  - Incorporates linear lower bounds (e.g., minimum distances) into the MIP objective to tighten the formulation.  

- **Experimental Findings**  
  - Tested on random CVRP instances (up to 51 cities) and standard CVRPLIB instances (up to 78 cities).  
  - Achieves near-state-of-the-art performance: On random instances of 51 cities, obtains total route costs within about 1.7% of specialized operations research solvers (OR-Tools).  
  - Runtime bottleneck is solving MIPs during policy rollouts; parallelization or faster solvers (e.g., Gurobi) significantly reduces training time.  
  - Shows improvements with moderate neural network sizes (4 to 16 ReLU nodes) in terms of solution quality, though large networks can increase MIP complexity.  
  - Outperforms previously published RL methods that rely on one-city-at-a-time action spaces (e.g., pointer networks).  

- **Strengths & Contributions**  
  - Explicitly handles combinatorial action selection via MIP, enabling exact route choices under an approximate value function.  
  - Achieves strong empirical results even with relatively simple neural network architectures.  
  - Demonstrates that single-instance RL solutions can compete favorably with specialized OR heuristics, offering a flexible, general framework.  

- **Weaknesses & Limitations**  
  - The method is comparatively more computationally expensive in the policy evaluation phase, as MIPs must be solved repeatedly.  
  - The approach is tailored to deterministic CVRP; extension to problems with stochastic demand or time windows is non-trivial.  
  - Performance relies on robust MIP solver performance and can require significant compute resources for large instances.  

- **Relevance & Impact**  
  - Highly relevant to deterministic VRP research: merges RL-based approximate dynamic programming with classical branch-and-cut.  
  - Indicates that well-designed combinatorial RL approaches can leverage domain structure to achieve near state-of-the-art solutions.  
  - Offers a strong baseline for future developments in single-instance learning, bridging operations research and machine learning in routing contexts.