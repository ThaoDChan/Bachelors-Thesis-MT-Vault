# Green logistics location-routing problem with eco-packages

## Metadata
- **Link to PDF**: [[[wang2020b]_Green_logistics_location-routing_problem_with_eco-packages]]
- **Tags**:
  - #GreenVRP
  - #VRPTW
  - #MultiDepotVRP
  - #MultiVehicleRouting
  - #LargeScaleVRP
  - #ML-Unsupervised
  - #ML-Clustering
  - #GeneticAlgorithm
  - #PopulationBasedEvolution
  - #HeuristicOrExactBlends
- **Relevant**: true  
- **Fit Score**: 7  
- **State of the Art (SoA) Concepts**:
  - Multi-echelon location-routing with time windows  
  - Gaussian mixture clustering for VRP partitioning (unsupervised ML)  
  - Clarke–Wright + Non-dominated Sorting Genetic Algorithm (NSGA-II)  
  - Lagrangian relaxation approach for vehicle routing subproblems  
  - Integration of environmental (“green”) considerations in VRP  
- **Performance Evaluation**: yes
- **Performance Evaluation Framework**: Solomon-based (modified demands/time windows)

## Abstract
“Optimization of the green logistics location-routing problem with eco-packages involves solving a two-echelon location-routing problem and the pickup and delivery problem with time windows. The first echelon is modeled by time-discretized network flow programming in the resource sharing state-space-time (SST) network. The second echelon focuses on cost-minimized synchronization-oriented location routing that minimizes the total generalized cost, including transportation cost, eco-package value, short-term benefits, and environmental externalities. A Gaussian mixture clustering algorithm assigns customers to service providers, while a Clarke–Wright saving method-based non-dominated sorting genetic algorithm II optimizes pickup and delivery routes. A case study in Chengdu, China demonstrates the proposed models and solution methods, with a sensitivity analysis on eco-package life cycles.”

## Summary
- **Problem & Objectives**  
  - Proposes a *Green Logistics Location-Routing Problem with Eco-Packages (GLLRPE)* for a two-echelon system:
    1. **First Echelon**: Transport of large eco-packages by semitrailer trucks from distribution/pickup centers to satellites.  
    2. **Second Echelon**: Delivery of smaller eco-packages to customers and pickup of used eco-packages from them.  
  - Considers location decisions (where to place pickup satellites) and routing decisions (how to schedule pickups/deliveries).  
  - Incorporates *environmental considerations* by encouraging eco-package recycling and analyzing short-term and long-term benefits.

- **Key Methods**  
  1. **Two-phase Modeling**  
     - Phase 1 (*Service Phase*):  
       - A cost-minimized synchronization-oriented location-routing (CMSOLR) model.  
       - **Gaussian Mixture Clustering Algorithm (GMCA)** used to partition customers (an *unsupervised ML* technique) based on location/time-window attributes.  
       - A *Clarke–Wright saving method-based Non-dominated Sorting Genetic Algorithm II (CW\_NSGA-II)* solves the multi-objective routing and satellite selection problem.  
         - Minimizes total costs (transport + operating + externalities) while maximizing synchronization degree (reducing waiting times).  
     - Phase 2 (*Transport Phase*):  
       - A time-discretized transport-concentrated network flow programming (TDTCNFP) model in a 3D state-space-time (SST) network.  
       - *Lagrangian relaxation* decomposes the first-echelon multi-depot pickup and delivery flows into subproblems, each solved as a shortest path in the SST network.  

  2. **Eco-Package Lifecycle Representation**  
     - Differentiates “initial,” “medium,” and “critical” recovery states of packages. Critical-grade eco-packages may be too costly to recycle.  
     - Cost calculations include internal transport, packaging value, externalities, and short-term/long-term benefits from recovered packaging.

- **Implementation & Experiments**  
  - A large-scale numerical study is conducted for a multi-satellite logistics network in Chengdu, China:
    - **Gaussian Mixture Clustering** is used to assign 90–200 customers to satellite clusters.  
    - The authors compare four potential location strategies for eco-package pickup satellites.  
    - The *CW\_NSGA-II* method produces feasible multi-objective solutions balancing cost and synchronization (waiting time).  
    - The *Lagrangian relaxation* approach optimizes the first-echelon flows under time window constraints, identifying minimal-cost transport routes with integrated pickup/delivery states.  
    - They perform sensitivity analysis on:
      1. The ratio of packages in different recovery states (initial, medium, critical).  
      2. Changing demand scenarios (pickup vs. delivery changes).  
      3. The effect of newly opened road segments on route costs.  
    - Results indicate that combining eco-package recycling with carefully chosen pickup-satellite locations reduces total costs and lowers environmental impact.

- **Key Findings**  
  - Using **unsupervised clustering** (GMCA) to partition customers effectively reduces computational complexity for large-scale location-routing.  
  - Hybridizing *Clarke–Wright savings* with an *NSGA-II* metaheuristic balances cost and service synchronization.  
  - The *SST network + Lagrangian relaxation* approach efficiently solves the first-echelon routing subproblem with minimal cost.  
  - Sensitivity analyses highlight that satellites located closer to the packaging center or in areas of dense pickup demand yield lower total costs and improved synchronization.  
  - The approach is fully *deterministic*, focusing on time windows and known demands (no random/stochastic components).

- **Relevance to Deterministic ML-based VRP**  
  - The paper extends a *location-routing problem with time windows* to include “green” considerations and a novel “eco-package” approach.  
  - The use of a **Gaussian mixture clustering** method qualifies as an *unsupervised ML* approach, aligning with interest in ML-based VRP solutions.  
  - Genetic algorithm-based heuristics (NSGA-II) are combined with classical operations research (OR) techniques (Clarke–Wright, Lagrangian relaxation), illustrating a *hybrid approach* to large-scale routing.  
  - Demonstrates *deterministic* problem assumptions (fixed demands, known time windows), fitting within the user’s scope.

- **Strengths & Limitations**  
  - **Strengths**:
    - Integrates a practical eco-friendly packaging concept, capturing both cost and environmental externalities.  
    - Effectively addresses large-scale, multi-echelon VRP with time windows and multiple facilities.  
    - Leverages multiple advanced methods (unsupervised clustering, GA-based multi-objective optimization, Lagrangian relaxation).  
  - **Limitations**:
    - Real-world deployment might require additional constraints (vehicle types, driver shifts, partial recharging for EVs, etc.) not covered here.  
    - Some cost components (long-term environmental benefits) may be harder to quantify precisely.  
    - Sticks to purely deterministic settings; does not address potential demand fluctuations or dynamic arrivals.

- **Conclusion & Impact**  
  - This study offers a *comprehensive multi-echelon approach* to green location-routing with integrated pickups and deliveries of reusable packages.  
  - It demonstrates that “green” strategies—such as eco-package recycling—combined with well-chosen satellite locations can yield significant cost and environmental benefits.  
  - The multi-phase approach (clustering + genetic optimization + Lagrangian relaxation) can be adapted to other large-scale deterministic VRP contexts.  
  - The framework’s *machine learning* component (Gaussian mixture clustering) and *genetic metaheuristic* approach represent valuable additions to the deterministic VRP literature.