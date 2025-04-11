# Data Driven VRP: A Neural Network Model to Learn Hidden Preferences for VRP

## Metadata
- **Link to PDF**: [[[mandi2021]_Data_Driven_VRP-A_Neural_Network_Model_to_Learn_Hidden_Preferences_for_VRP]]
- **Tags**:
  - #DeterministicVRP
  - #CVRP
  - #MultiVehicleRouting
  - #ML-Supervised
  - #ImitationLearning
  - #NeuralCombinatorialOptimization
  - #RealWorldData
  - #ML-AssistedExact
  - #BranchAndCut
  - #Generalization
- **Relevant**: true
- **Fit Score**: 10
- **State of the Art (SoA) Concepts**:
  - Neural networks for cost-function learning in VRP
  - Decision-focused learning integrated with VRP optimization
  - Arc-based probability estimation
  - Learning from historical (real-world) routing solutions
- **Performance Evaluation**: no (the authors use proprietary real-world data, no standard benchmark dataset)
- **Performance Evaluation Framework**: -

## Abstract
“The traditional Capacitated Vehicle Routing Problem (CVRP) minimizes the total distance of the routes under the capacity constraints of the vehicles. But more often, the objective involves multiple criteria including not only the total distance of the tour but also other factors such as travel costs, travel time, and fuel consumption. Moreover, in reality, there are numerous implicit preferences ingrained in the minds of the route planners and the drivers. Drivers, for instance, have familiarity with certain neighborhoods and knowledge of the state of roads, and often consider the best places for rest and lunch breaks. This knowledge is difficult to formulate and balance when operational routing decisions have to be made. This motivates us to learn the implicit preferences from past solutions and to incorporate these learned preferences in the optimization process. These preferences are in the form of arc probabilities, i.e., the more preferred a route is, the higher is the joint probability. The novelty of this work is the use of a neural network model to estimate the arc probabilities, which allows for additional features and automatic parameter estimation. This first requires identifying suitable features, neural architectures and loss functions, taking into account that there is typically few data available. We investigate the difference with a prior weighted Markov counting approach, and study the applicability of neural networks in this setting.”

## Summary
- **Core Motivation**  
  - The paper addresses the challenge that real-world VRP solutions often deviate from the classic “optimal” routes because planners and drivers have hidden preferences (e.g., local knowledge about traffic, stops for breaks, etc.).  
  - The authors propose to *learn* these hidden or implicit preferences from historical routing solutions in a deterministic CVRP setting.

- **Key Approach**  
  - They introduce a *neural network* that estimates *arc-level transition probabilities* (the probability of moving from one node to another).  
  - These probabilities serve as a learned cost function, replacing the typical distance-based objective. They then solve the CVRP by maximizing the joint probability of the route (i.e., performing a Maximum Likelihood Estimation [MLE] routing).

- **Methodology**  
  1. **Data Representation**:  
     - The training data consists of past (realized) solutions, each representing a single day’s route(s) in a small transportation company.  
     - Each node pair \((s, r)\) gets assigned a conditional probability \(\Pr(r \mid s)\).  
  2. **Neural Network Design**:  
     - The network takes as inputs various features, e.g., day of week, distance, number of vehicles, Markov-derived probabilities, and so on.  
     - It outputs an unnormalized score per potential next stop, which is then normalized via softmax into a probability vector.  
     - The neural model is deliberately kept *parsimonious* (few parameters) to avoid overfitting, given the limited real-world dataset (only ~200 historical daily solutions).
  3. **Training Objective**:  
     - They train the network in a *supervised* manner, using cross-entropy loss between the network’s predicted arc probabilities and the actual arcs chosen in historical routes.  
     - Additionally, they experiment with a *decision-focused learning* variant, which backpropagates a routing-difference loss through a VRP solver, but they find this approach can overfit with too little data.

- **Comparison with Markov Model**  
  - The authors compare the neural network approach to a previous “Markov counting” method (which estimated arc frequencies by simple counts, weighted by recency or day-of-week).  
  - Incorporating neural predictions leads to marginally better alignment with the planners’ actual routes (fewer mismatches).  
  - Their experiments on the real dataset show that the neural model yields lower cross-entropy error and slightly smaller arc/route differences compared to the baseline Markov approach.

- **Relevance to Deterministic VRP**  
  - The work explicitly addresses the CVRP with fixed, known demands and no stochastic changes.  
  - It aims to incorporate additional “soft” preferences within a standard integer programming framework, demonstrating a new *learning-from-data* angle in deterministic VRP.

- **Key Findings**  
  - Using neural networks to learn transition probabilities can capture hidden driver/planner preferences more effectively than simpler frequency-based methods.  
  - Markov probability estimates can be included as an input (feature) to the neural network, leading to improved generalization.  
  - A smaller neural architecture (minimal layers) was necessary due to data scarcity; deeper or more complex models risked overfitting.

- **Performance and Limitations**  
  - They validate on real, non-public data, so no direct comparison to known benchmark sets.  
  - The final solutions closely match the actual routes used, but the approach’s generalizability to larger or different datasets remains an open question.  
  - Decision-focused training did not outperform standard supervised training, likely due to the limited sample size.

- **Overall Contribution**  
  - Proposes a *data-driven, preference-learning framework* for CVRP, bridging historical route data and MIP-based optimization.  
  - Shows how to systematically incorporate extra contextual features (e.g., day-of-week) into a VRP solver’s objective.  
  - Encourages further research on decision-focused approaches, acknowledging the challenge of limited data in real logistics contexts.