# Fleet Optimization of Smart Electric Motorcycle System Using Deep Reinforcement Learning

## Metadata
- **Link to PDF**: [[[alwesabi2022]_Fleet_optimization_of_smart_electric_motorcycle_system_using_deep_reinforcement_learning.pdf]]  
- **Tags**:  
  #ML-ReinforcementLearning  
  #RL-Qlearning  
  #RL-DQN  
  #ExperienceReplay  
  #MDP  
  #DeterministicVRP  
  #SingleVehicleRouting  
  #PDP  
  #RealWorldData  
  #VehicleRebalancing  
- **Relevant**: true  
- **Fit Score**: 7
- **State of the Art (SoA) Concepts**:
  - Deep Q-Network (DQN) for decision-making in routing/rebalancing
  - Deterministic pickup and delivery rebalancing approach
  - Single-vehicle route optimization (truck capacity = 3 motorcycles)
  - Integration of real-world operational data from a university campus
- **Performance Evaluation**: no  
- **Performance Evaluation Framework**: -

## Abstract
"Smart electric motorcycle-sharing systems based on the digital platform are one of the public transportations that we use in daily lives when the sharing economy is considered. This transportation provides convenience for users with low-cost systems while it also promotes environmental conservation. Normally, users rent the vehicle to travel from the origin station to another station near their destination with a one-way trip in which the demand of renting and returning at each station is different. This leads to unbalanced vehicle rental systems. To avoid the full or empty inventory, the electric motorcycle-sharing rebalancing with the fleet optimization is employed to deliver the user experience and increase rental opportunities. In this paper, the authors propose a fleet optimization to manage the appropriate number of vehicles in each station by considering the cost of moving tasks and the rental opportunity to increase business return. Although the increasing number of service stations results in a large action space, the proposed routing algorithm is able filter the size of the action space to enable computing tasks. In this paper, a Deep Reinforcement Learning (DRL) creates the decision-making function to decide the appropriate action for fleet allocation from the last state of the number of vehicles at each station in the real environment at Suranaree University of Technology (SUT), Thailand. The obtained results indicate that the proposed concept can reduce the Operating Expenditure (OPEX)."

## Summary
- **Context & Motivation**  
  - The paper addresses a rebalancing problem in a smart electric motorcycle-sharing system operating under a one-way trip model.  
  - Users rent an e-motorcycle at one station and return it to a different station, causing uneven distribution of vehicles across stations.  
  - Such imbalance may lead to “empty” stations or “overfilled” stations, reducing service availability and raising operating costs.  

- **Problem Statement**  
  - The authors formalize fleet rebalancing as a decision problem that seeks to maximize rental opportunities (revenue) while minimizing relocation (truck operating) costs.  
  - The system uses a single fleet truck with a capacity of 3 motorcycles to pick up and deliver vehicles between up to 10 stations.  

- **Methodology**  
  1. **Routing Algorithm**  
     - Observing that a naive approach to enumerating all possible pickup-and-delivery actions is intractable (the action space grows exponentially with the number of stations), the authors propose a filtering mechanism.  
     - They leverage routes commonly used by human operators, limiting the set of feasible round-trip routes and pickup quantities (–3 to +3 bikes for each link). This reduces the action space from astronomical to manageable levels.  
  2. **Deep Reinforcement Learning (DRL)**  
     - They model the rebalancing problem as a Markov Decision Process (MDP), where the state is the vector of available motorcycles at each station.  
     - Each potential route (from the filtered set) represents an action that picks up and/or delivers motorcycles to certain stations.  
     - The immediate reward combines expected revenue from future rentals (based on station-level demand) minus cost of operating the truck.  
     - A Deep Q-Network (DQN) with experience replay is trained to approximate the Q-values for each state–action pair.  
     - The DQN then guides real-time (or end-of-day) decisions on how many bikes to move and which route to take.

- **Experiments & Implementation**  
  - Conducted at Suranaree University of Technology in Thailand, involving 10 stations and 30 electric motorcycles.  
  - The DQN approach is trained on daily end-of-day data, using the filtered action set derived from historical operator decisions.  
  - The training process compares different sizes of action spaces (5K, 10K, 20K route combinations) to balance learning stability and solution quality.  

- **Results**  
  - The learned policy (decision function) outperforms naive or random rebalancing, yielding higher revenues (due to increased rental availability) and lower OPEX (truck travel distance).  
  - The authors observe that the mid-sized action space (10K) achieves a good trade-off between computational complexity and solution quality.  
  - An on-site pilot application shows reduced travel distances for the rebalancing truck and improved station availability.  

- **Strengths**  
  - Introduces a DRL-based approach to a real-world rebalancing challenge, moving beyond toy or purely simulated VRPs.  
  - Proposes a practical way to shrink the action space through a routing algorithm anchored in operator-intuitive route patterns.  
  - Demonstrates successful cost reduction and system improvement in an actual electric motorcycle-sharing service.  

- **Limitations & Open Issues**  
  - The approach is tailored to a single-truck scenario, potentially limiting extension to multi-vehicle operations.  
  - The method’s scalability to larger station networks may still be constrained by action space growth, though the authors mitigate this with route filtering.  
  - The authors do not benchmark against standard VRP test sets; evaluation is only on real data at one university.  

- **Relevance & Contribution to Deterministic VRP Research**  
  - Although it is not a classic CVRP or VRPTW, the rebalancing challenge is structurally akin to a deterministic pickup-and-delivery VRP with capacity constraints and a single vehicle.  
  - It showcases how DRL can produce a rebalancing policy that balances financial returns (i.e., rental revenue) with operational costs, potentially informing more general VRP solutions where demand is known or estimated deterministically.  
  - The paper illustrates how real-world constraints—limited truck capacity, fixed station capacity, and route preferences—can be integrated into a DRL-based solution approach.  