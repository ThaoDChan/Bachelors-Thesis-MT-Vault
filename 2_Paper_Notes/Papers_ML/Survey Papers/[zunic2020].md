# An Adaptive Data-Driven Approach to Solve Real-World Vehicle Routing Problems in Logistics

## Metadata
- **Link to PDF**: [[[zunic2020]_An_adaptive_data-driven_approach_to_solve_real-world_vehicle_routing_problems_in_logistics.pdf]]
- **Tags**:  
 #ML-Supervised  
#ML-AssistedHeuristics  
#ParameterTuning  
#CVRPTW  
#VRPTW  
#TabuSearch  
#LocalSearchHeuristics  
#HeuristicOrExactBlends  
#DeterministicVRP  
#TimeWindowConstraints  
#RealWorldData  
- **Relevant**: true  
- **Fit Score**: 9  
- **State of the Art (SoA) Concepts**:
  - Data-driven parameter tuning (SVM & GLM) for VRPs  
  - Hybrid heuristic approach combining Clarke-Wright, Tabu search, and local improvements  
  - Deterministic VRP with time windows and site-dependent constraints  
  - Handling of large-scale real-world routing with adaptive module-based methods  
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: Solomon and Homberger benchmark instances, plus proprietary real-world data  

## Abstract
“Transportation occupies one-third of the amount in the logistics costs, and accordingly transportation systems largely influence the performance of the logistics system. This work presents an adaptive data-driven innovative modular approach for solving the real-world vehicle routing problems (VRPs) in the field of logistics. The work consists of two basic units: (i) an innovative multistep algorithm for successful and entirely feasible solving of the VRPs in logistics and (ii) an adaptive approach for adjusting and setting up parameters and constants of the proposed algorithm. The proposed algorithm combines several data transformation approaches, heuristics, and Tabu search. Moreover, as the performance of the algorithm depends on the set of control parameters and constants, a predictive model that adaptively adjusts these parameters and constants according to historical data is proposed. A comparison of the acquired results has been made using the decision support system with predictive models: generalized linear models (GLMs) and support vector machine (SVM). The algorithm, along with the control parameters, which uses the prediction method, was acquired and was incorporated into a web-based enterprise system, which is in use in several big distribution companies in Bosnia and Herzegovina. The results of the proposed algorithm were compared with a set of benchmark instances and validated over real benchmark instances as well. The successful feasibility of the given routes, in a real environment, is also presented.”

## Summary
- **Context & Purpose**  
  - The paper addresses vehicle routing under deterministic conditions, particularly complex real-world distributions involving heterogeneous vehicle fleets, time-window constraints, capacity, and site-dependent restrictions.  
  - The authors propose a comprehensive, modular algorithmic pipeline—composed of data transformations, heuristics, Tabu search, and a post-optimization step—that adapts to real-world logistic constraints.  
  - A key innovation is the use of historical data and predictive models (SVM and GLM) to tune algorithmic parameters automatically (e.g., penalty factors, tolerance thresholds).

- **VRP Variant & Constraints**  
  - Focuses on a heterogeneous fleet VRP with time windows (HVRPTW).  
  - Incorporates additional real-world constraints like site-dependent feasibility (not all vehicles can serve certain locations), multiple trips for vehicles if needed, and legal working-hour restrictions.  
  - Uses cost formulations that penalize weight/volume overload, time-window violations, and site incompatibility.

- **Methodology**  
  - Preprocessing:  
    - Data transformations to handle unloading times and feasible multiple deliveries.  
    - Time-window shifts to align with depot schedules.  
  - First step (Clarke-Wright-based heuristic): generates an initial solution, merging routes where possible, avoiding large capacity or time violations.  
  - Second step (route elimination): attempts to reduce the total number of routes if cost remains stable or improves.  
  - Third step (Tabu search): performs local refinements (relocate, swap operators), while penalizing repeated or disallowed route–customer assignments.  
  - Fourth step (post-optimization): reorders customers within individual routes for final cost improvements.  
  - **Adaptive parameter tuning**:  
    - Uses historical data of feasible routes to build regression models (GLM, SVM) that predict optimal penalty constants for new instances.  
    - A decision support system selects which regression model’s predicted values are adopted.

- **Results**  
  - Evaluated on well-known benchmark sets (Solomon, Homberger) and proprietary real-world data from a major Bosnian distribution firm.  
  - On benchmark data, the algorithm finds near-optimal or sometimes improved cost solutions (though possibly with different numbers of routes than the published best solutions).  
  - On real-world data, the approach reduces logistics costs while ensuring nearly 100% feasibility.  
  - The data-driven parameter tuning (SVM/GLM) significantly improves solution quality and consistency.  
  - Execution times remain practical (seconds for 100-customer problems, up to a few minutes for large-scale or heavily constrained instances).

- **Strengths & Contributions**  
  - A modular pipeline that integrates classical heuristics (Clarke-Wright) and advanced local search (Tabu) with a predictive data-driven parameter adaptation.  
  - Demonstrates successful real-world deployment and cost savings for large distribution problems.  
  - Comprehensive coverage of real constraints (capacity, time windows, site dependence, multi-trip feasibility).  
  - Benchmarked on both standard VRP sets and actual enterprise data, showcasing generalizability.

- **Weaknesses & Limitations**  
  - Primarily tested in a single-country context with a limited range of real fleets; broader validation across different industries or countries may be needed.  
  - Reliance on accurate historical data for parameter tuning; if data distribution changes drastically, the model might need re-training or re-calibration.  
  - Does not delve into advanced machine-learning models (like deep neural networks); focuses on SVM/GLM for parameter regression.

- **Relevance to Deterministic ML-based VRP**  
  - Highly relevant for studies on ML-assisted heuristics: uses supervised learning (SVM/GLM) to optimize hyperparameters.  
  - Addresses a deterministic VRP with time windows (no stochastic or dynamic factors).  
  - Offers insights on bridging classical optimization with data-driven improvements.