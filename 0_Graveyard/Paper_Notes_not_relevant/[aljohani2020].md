# Real-Time metadata-driven routing optimization for electric vehicle energy consumption minimization using deep reinforcement learning and Markov chain model

## Metadata
- **Link to PDF**: [[[aljohani2020]_Real-Time_metadata-driven_routing_optimization_for_electric_vehicle_energy_consumption_minimization_using_deep_reinforcement_learning_and_Markov_chain_model.pdf]]
- **Tags**:
  - #ML-ReinforcementLearning
  - #RL-ValueBased
  - #RL-Qlearning
  - #RL-DQN
  - #MDP
  - #GreenVRP
  - #SingleVehicleRouting
  - #RealWorldData
  - #DynamicVRP
  - #OnlineVRP
- **Relevant**: false  
- **Fit Score**: 2  
- **State of the Art (SoA) Concepts**:
  - Double Deep Q-Network (DDQN)  
  - Markov Chain modeling for route/energy consumption  
  - Single-vehicle route optimization (origin to destination)  
  - Real-time data from Google Maps
- **Performance Evaluation**: no  
- **Performance Evaluation Framework**: -

## Abstract
A real-time, data-driven electric vehicle (EVs) routing optimization to achieve energy consumption minimization is proposed in this work. The proposed framework utilizes the concept of Double Deep Q-learning Network (DDQN) in learning the maximum travel policy of the EV as an agent. The policy model is trained to estimate the agent’s optimal action per the obtained reward signals and Q-values, representing the feasible routing options. The agent’s energy requirement on the road is assessed following Markov Chain Model (MCM), with Markov’s unit step represented as the average energy consumption that takes into consideration the different driving patterns, agent’s surrounding environment, road conditions, and applicable restrictions. The framework offers a better exploration strategy, continuous learning ability, and the adoption of individual routing preferences. A real-time simulation in the python environment that considered real-life driving data from Google’s API platform is performed. Results obtained for two geographically different drives show that the proposed energy consumption minimization framework reduced the energy utilization of the EVs to reach its intended destination by 5.89% and 11.82%, compared with Google’s proposed routes originally. Both drives started at 4.30 PM on April 25th, 2019, in Los Angeles, California, and Miami, Florida, to reach EV’s charging stations that are located six miles away from both of the starting locations.

## Summary
- **Purpose & Context**  
  - The paper addresses single-origin to single-destination route optimization for electric vehicles (EV).  
  - Seeks to minimize battery energy consumption by choosing an optimal travel path in real-time.

- **Core Methodology**  
  - Proposes a **Double Deep Q-Network (DDQN)** reinforcement learning approach.  
  - Incorporates a **Markov Chain Model (MCM)** to estimate energy consumption at each step, treating each road segment as a probabilistic state transition.  
  - Uses **real-time metadata** from Google Maps API (traffic, road conditions, elevation) to dynamically update route decisions and reward signals.

- **Approach Details**  
  - The EV is modeled as a learning **agent**; its possible actions are the eight compass directions it can move toward next.  
  - **Q-values** capture the potential future return (energy saving) of each action.  
  - **Overestimation** of Q-values is mitigated by using DDQN (two networks: one for selecting the best action, the other to evaluate the Q-value of that selected action).  
  - Incorporates an **EV battery model** accounting for acceleration, rolling resistance, drag, and road gradient.  
  - **Regenerative braking** is integrated by allowing negative energy consumption entries, then re-adjusted to keep the Markov transition matrix valid.

- **Experimental Setup**  
  - Two routes tested:  
    1. From Florida International University (Miami) to a charging station ~6 miles away.  
    2. From J. Paul Getty Museum (Los Angeles) to another station ~5.8 miles away.  
  - Both are tested under real-time traffic data, traveling at a specific time/date.  
  - Simulation uses Python + TensorFlow, with repeated episodes (up to 140+) to train and converge on an optimal route.  
  - Performance is compared against the standard route suggestions from Google Maps.

- **Key Findings**  
  - The DDQN-based path yields energy savings of ~5.89% (Miami route) and ~11.82% (Los Angeles route) relative to Google’s default directions.  
  - Minor energy feedback was gained via regenerative braking for the downward slope in Los Angeles.  
  - Shows that a data-driven RL approach can adapt routes for an EV’s specific energy management needs, though potential trade-offs in travel time exist.

- **Strengths**  
  - Real-time data integration from a commonly used platform (Google Maps).  
  - Systematic representation of EV energy consumption within a Markov Chain, capturing dynamic influences (speed limits, traffic).  
  - Demonstrates tangible improvements in energy minimization for single-point routing.

- **Limitations**  
  - Focuses only on a **single-origins, single-destination** scenario; no mention of multi-customer or classical multi-stop VRP constraints.  
  - Real-time approach can be considered a **dynamic** or **online** scenario, which moves away from standard deterministic VRP formulations.  
  - Simulation scale is relatively small (only two routes) due to restrictions on API queries.

- **Relevance to Deterministic VRP**  
  - The paper addresses a route optimization for a single EV from one point to another, not a multi-node VRP with customers.  
  - Consequently, it does not align with standard deterministic multi-customer VRP settings; it is more akin to a single-route “shortest path” approach with dynamic, real-time updates.  

- **Conclusion**  
  - Introduces an RL-based framework that exploits real-time data for EV energy minimization on single trips.  
  - Showcases potential for synergy between ML-based route planners and online mapping services.  
  - Not strictly aligned with classical multi-customer VRP tasks but exemplifies how real-time or dynamic route selection can reduce operational costs (energy consumption).