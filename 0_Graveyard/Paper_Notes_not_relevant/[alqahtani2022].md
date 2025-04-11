# Dynamic energy scheduling and routing of multiple electric vehicles using deep reinforcement learning

## Metadata
- **Link to PDF**: [[[alqahtani2022]_Dynamic_energy_scheduling_and_routing_of_multiple_electric_vehicles_using_deep_reinforcement_learning.pdf]]
- **Tags**:
  - #ML-ReinforcementLearning
  - #RL-ValueBased
  - #RL-DQN
  - #DynamicVRP
  - #StochasticVRP
  - #GreenVRP
  - #MultiVehicleRouting
  - #MDP
  - #NeuralCombinatorialOptimization
  - #ExperienceReplay
- **Relevant**: false
- **Fit Score**: 2
- **State of the Art (SoA) Concepts**:
  - Deep Q-Network (DQN) for energy-aware vehicle routing
  - Dynamic and uncertain EV routing with solar-powered energy supply
- **Performance Evaluation**: no (not using standard deterministic VRP benchmarks)
- **Performance Evaluation Framework**: -

## Abstract
"The demand on energy is uncertain and subject to change with time due to several factors including the emergence of new technology, entertainment, divergence of people's consumption habits, changing weather conditions, etc. Moreover, increases in energy demand are growing every day due to increases in world's population and growth of global economy, which substantially increase the chances of disruptions in power supply. This makes the security of power supply a more challenging task especially during seasons (e.g. summer and winter). This paper proposes a reinforcement learning model to address the uncertainties in power supply and demand by dispatching a set of electric vehicles to supply energy to different consumers at different locations. An electric vehicle is mounted with various energy resources (e.g., PV panel, energy storage) that share power generation units and storages among different consumers to power their premises to reduce energy costs. The performance of the reinforcement learning model is assessed under different configurations of consumers and electric vehicles, and compared to the results from CPLEX and three heuristic algorithms. The simulation results demonstrate that the reinforcement learning algorithm can reduce energy costs up to 22.05%, 22.57%, and 19.33% compared to the genetic algorithm, particle swarm optimization, and artificial fish swarm algorithm results, respectively."

## Summary
- **Focus & Motivation**  
  - Addresses the problem of providing on-demand power to various locations using a fleet of electric vehicles (EVs) equipped with photovoltaic (PV) panels and battery storage.
  - The authors target uncertainty in both energy demand and solar irradiance, proposing a dynamic approach to simultaneously schedule EV movements and energy dispatch.

- **Problem & Approach**  
  - Formulates a combined vehicle routing and energy scheduling problem where EVs act as mobile energy sources.
  - Uncertainties in load demand and renewable generation introduce a dynamic, stochastic component.
  - The authors reformulate the optimization problem as a Markov Decision Process (MDP).
  - They train a Deep Q-Network (DQN) to approximate the optimal policy for:
    - Deciding EV mobility (where to move each hour).
    - Deciding energy transactions (charging from grid or discharging to consumers).

- **Key Methodology**  
  - **State Space** includes EV location, battery state of charge, solar irradiance, and local demand.
  - **Action Space** includes directional moves (up, down, left, right, or stay) and battery charge/discharge decisions.
  - The DQN uses historical data of energy demands and solar irradiance for training.
  - The solution is compared against CPLEX (exact solver) and three population-based metaheuristics (GA, PSO, AFSA).

- **Experiments & Findings**  
  - Experiments conducted on scenarios with different numbers of EVs (4, 8, 12, 16) and sets of consumers (20, 40).
  - DQN achieves faster solution times and competitive or better cost performance than heuristics, especially as the system scales.
  - Notably, the DQN’s solution quality remains robust even under large variations in demand and solar irradiance.
  - Overall conclusion: A reinforcement learning approach can handle dynamic changes more effectively than static heuristics and at lower computational times than exact methods once training is completed.

- **Strengths, Limitations, Observations**  
  - **Strengths**: Demonstrates RL can manage real-time, uncertain VRP settings by leveraging historical data to learn adaptive policies. Encouraging results for fast dispatch decisions.
  - **Limitations**: 
    - The routing environment is inherently dynamic and partially stochastic; solutions require significant data and training episodes.
    - Does not address purely deterministic scenarios and relies on scenario-based or historical data for training.
  - **Applicability**: Suitable for applications that require on-demand local energy supply from mobile sources, with uncertain demand.

- **Relevance to Deterministic VRP**  
  - The study specifically addresses a dynamic problem with uncertainty in both demands and supply from PV. 
  - Since this is not a purely deterministic VRP approach, it falls outside the strict scope of a deterministic VRP survey.

- **Potential Impact**  
  - For broader VRP research, it highlights how RL can integrate with operational constraints in power systems and flexible vehicle routes.
  - Methods could inspire future deterministic or partially observable expansions, but as presented, it remains a dynamic/stochastic approach.