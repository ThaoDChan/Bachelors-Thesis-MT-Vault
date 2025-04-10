# Solving capacitated vehicle routing problem with demands as fuzzy random variable

## Metadata

- **Link to PDF**: [[[singh2023] Solving capacitated vehicle routing problem with demands as fuzzy random variable.pdf]]
- **Tags**: 
  #CVRP 
  #StochasticVRP 
  #MultiVehicleRouting 
  #VRPBenchmarks  
- **Relevant**: false  
- **Fit Score**: 0  
- **State of the Art (SoA) Concepts**:
  - Handling fuzzy and stochastic demands in CVRP  
  - Branch-and-bound exact solution for small-sized symmetric CVRP instances  
  - Use of fuzzy random and intuitionistic fuzzy variables for customer demands  
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: Modified versions of Augerat’s benchmark sets (Sets P, A) and Christofides/Eilon sets (Set E), adapted to fuzzy/stochastic demands

## Abstract
“Capacitated vehicle routing problem (CVRP) is a classical combinatorial optimization problem in which a network of customers with specified demands is given. The objective is to find a set of routes which originates as well as terminates at the depot node. These routes are to be traversed in such a way that the demands of all the customers in the network are satisfied and the cost associated with traversal of these routes come out to be a minimum. In real-world situations, the demand of any commodity depends upon various uncontrollable factors, such as season, delivery time, and market conditions. Due to these factors, the demand can always not be told in advance and a precise information about the demand is nearly impossible to achieve. Hence, the demands of the customers always experience impreciseness and randomness in real life. The decisions made by the customers about the demands may also have some scope of hesitation as well. In order to handle such demands of customers in the network, fuzzy random variables and intuitionistic fuzzy random variables are used in this work. The work bridges the gap between the classical version of CVRP and the real-life situation and hence makes it easier for the logistic management companies to determine the routes that should be followed for minimum operational cost and maximum profit. Mathematical models corresponding to CVRP with fuzzy stochastic demands (CVRPFSD) and CVRP with intuitionistic fuzzy stochastic demands (CVRPIFSD) have been presented. A two-stage model has been proposed to find out the solution for the same. To explain the working of the methodology defined in this work, different examples of networks with fuzzy and intuitionistic fuzzy demands have been worked out. The proposed solution approach is also tested on modified fuzzy stochastic versions of some benchmark instances.”

## Summary
- **Problem & Motivation**  
  - Addresses the classical Capacitated Vehicle Routing Problem (CVRP) but extends it to handle uncertain, imprecise, and stochastic customer demands.  
  - Proposes fuzzy random variables (and their intuitionistic fuzzy extensions) to model demand in real scenarios where demand data is incomplete or imprecise.  

- **Key Contribution**  
  - Defines two specialized mathematical formulations: one for CVRP with fuzzy stochastic demands (CVRPFSD) and another for CVRP with intuitionistic fuzzy stochastic demands (CVRPIFSD).  
  - Introduces a two-stage approach:  
    1. **Stage 1**: Construct an a priori route using a branch-and-bound method (treating it similarly to TSP).  
    2. **Stage 2**: Execute that route but account for random and fuzzy demand realizations; if capacity is exceeded at a customer, the vehicle performs a depot return (route failure) and restarts from there.  
  - Demonstrates how “effective failure costs” can be computed at each node, reflecting the probability of a route failure due to insufficient vehicle capacity.

- **Methodology**  
  - Employs branch-and-bound to find a minimum-cost Hamiltonian circuit (a TSP-like solution) under normal circumstances.  
  - Manages the imprecise and random nature of demands by using fuzzy random variables, with probability mass functions given in triangular fuzzy form (or triangular intuitionistic fuzzy form).  
  - Provides formulas for expected failure cost if a route must deviate back to the depot when demands exceed vehicle capacity.

- **Results**  
  - Experiments on small synthetic examples show how the methodology obtains a baseline route and then adds a fuzzy-stochastic correction for route failures.  
  - The approach is also tested on modified standard CVRP benchmark sets (Augerat’s sets P, A, and Christofides/Eilon sets), artificially introducing fuzzy demands to reflect real-world uncertainty.  
  - Comparisons with classic TSP/CVRP heuristics (e.g., Clark and Wright, Christofides, Genetic Algorithms) highlight differences in total operational cost once the demand uncertainty is factored in.

- **Relevance & Limitations**  
  - The paper focuses on stochastic VRP with fuzzy/imprecise demands, which addresses real-life variability but is not aligned with deterministic VRP settings.  
  - The proposed method is limited in scalability due to branch-and-bound’s exponential complexity, so only smaller symmetric instances can be tackled exactly.  
  - The approach does not incorporate machine learning techniques; it is an exact method plus fuzzy-stochastic modeling, rather than an ML-based approach.

- **Conclusion & Future Work**  
  - The study opens avenues for modeling hesitation and imprecise demand estimates in vehicle routing problems.  
  - Future extensions mentioned include service-time scheduling with random/imprecise time windows and using advanced fuzzy or neutrosophic techniques.  