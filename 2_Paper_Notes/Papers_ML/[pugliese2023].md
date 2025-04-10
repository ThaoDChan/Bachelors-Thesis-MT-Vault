# Combining variable neighborhood search and machine learning to solve the vehicle routing problem with crowd-shipping

## Metadata
- **Link to PDF**: [[[pugliese2023]_Combining_variable_neighborhood_search_and_machine_learning_to_solve_the_vehicle_routing_problem_with_crowd-shipping.pdf]]
- **Tags**: 
  #DeterministicVRP 
  #MultiVehicleRouting 
  #VRPTW 
  #ML-AssistedLocalSearch 
  #RL-ValueBased 
  #RL-Qlearning 
  #HybridLocalSearchRL 
  #LargeScaleVRP 
  #VRPCrowdShipping
- **Relevant**: true  
- **Fit Score**: 9  
- **State of the Art (SoA) Concepts**:
  - Deterministic VRP with time windows and multiple vehicles
  - Crowd-shipping via occasional drivers
  - Hybrid Local Search with Reinforcement Learning (Q-learning)
  - Variable Neighborhood Search (VNS)
  - Performance comparison on Solomon benchmark instances
- **Performance Evaluation**: yes
- **Performance Evaluation Framework**: Solomon benchmarks (C, R, RC) and additional large-scale data (up to 400 customers)

## Abstract
"Crowd-shipping is an innovative delivery model based on the sharing economy concept. In this framework, delivery operations are carried out by non-professional couriers, also called "occasional drivers." These individuals, who travel with their own vehicles, may deviate from their ordinary routes to deliver items. We model a single-depot problem where both regular trucks and occasional drivers deliver goods within given time windows. We develop a Variable Neighborhood Search metaheuristic, enhanced with machine learning and reinforcement learning techniques, to explore promising areas of the search space. Computational results on benchmark instances, including large-scale scenarios, show that our approach yields improved solutions compared to existing methods."

## Summary
- **Problem & Motivation**
  - Proposes a **deterministic last-mile delivery** setting combining conventional fleet with “occasional drivers” (crowd-shipping).
  - Each occasional driver (OD) starts at a main depot and ends at their personal destination, can serve multiple customers if capacity/time constraints allow.
  - Objective: minimize total route costs (fuel, distance) plus OD compensation.

- **VRP Variant**
  - A **Vehicle Routing Problem with Time Windows (VRPTW)** extended to crowd-shipping (VRPODTWmd).
  - There is exactly one central depot, a fleet of conventional trucks, and multiple ODs each with individual capacity and time windows.

- **Core Methodology**
  - Builds on a **Variable Neighborhood Search (VNS)** framework:
    1. **Shaking Phase**: Moves the current solution to a random neighborhood (perturbation).
    2. **Local Search Phase**: Applies improvement moves (2-opt, swaps, node relocations, new-route creation).
    3. **Neighborhood Change/Selection**: Decides which local search operators to apply next.
  - Introduces **machine learning** at two points in the metaheuristic:
    1. **Shaking Phase with Attractiveness** (inspired by an ant-colony-like trail system):  
       - Tracks how frequently certain node-to-node (C-C) or node-to-driver (C-OD) assignments appear in high-quality solutions.  
       - During “shaking,” the method biases random moves toward those with higher “trail” (attractiveness) values.
    2. **Local Search with Reinforcement Learning**:  
       - Uses **Q-learning** or probability-based adaptive selection to pick which local search move to try next.  
       - Adjusts operator selection probabilities or Q-values in real-time based on solution improvements.

- **ML Techniques & Integration**
  - **Q-learning**: A value-based RL approach where each neighborhood operator is considered an “action,” and the reward is the improvement in solution quality.  
  - **Trail-based Shaking (Thevenin approach)**: Maintains pheromone-like intensities to guide random solution perturbations.  
  - **Probabilistic local search** (ProbLS): Updates probabilities to invoke certain local search routines more often if they yield better solutions.

- **Key Experiments & Results**
  - Benchmarks include:
    - **Solomon’s classical VRPTW** instances (C, R, and RC categories, types 1 & 2).
    - Extended sets with up to **100 customers**.
    - Additional new datasets reaching **200 and 400 customers**, covering large-scale VRP scenarios.
  - **Performance**:
    - On smaller instances (5–15 customers), approaches often reach known optimal solutions.
    - On medium and large sets, the new ML-based variants consistently outperform a baseline VNS (dubbed VNS-I) by reducing solution costs 2–6%.
    - **T QL** (the Thevenin-based shaking combined with Q-learning local search) typically achieves the best overall solution quality but at slightly higher computational cost.  
    - **T P** (Thevenin-based shaking + probabilistic local search) also performs strongly, with marginally lower cost solutions than simpler methods.
    - The largest test sets (200–400 customers) show that the ML-enhanced VNS scales effectively while keeping solution quality high.

- **Strengths**
  - Demonstrates how **reinforcement learning** can systematically guide local search neighborhoods for a **deterministic VRP** variant.
  - Handles **large-scale** (hundreds of customers) with strong performance improvements over a purely metaheuristic baseline.
  - The **trail-based shaking** helps avoid local minima and fosters exploration of promising routes.

- **Limitations / Open Gaps**
  - Requires careful parameter tuning (evaporation factor, learning rate, etc.), conducted via automated methods (e.g., irace).
  - Focuses on a **single-depot** scenario; multi-depot or more complex constraints might need further adaptation.
  - Paper targets static data; real-time dynamic arrivals or stochastic demands are out of scope (kept fully deterministic).

- **Relevance & Impact**
  - Shows an effective **hybrid approach** that merges classical optimization with ML-based selection and perturbation strategies.
  - Directly relevant to the thesis’s deterministic VRP scope, with emphasis on **time windows** and advanced **ML-driven local search**.
  - Illustrates potential improvements from learning-based heuristics in last-mile logistics, offering a route to handle scale efficiently in practice.