# A scalable learning approach for the capacitated vehicle routing problem

## Metadata
- **Link to PDF**: [[[fitzpatrick2024]_A_scalable_learning_approach_for_the_capacitated_vehicle_routing_problem.pdf]]
- **Tags**: 
  #ML-ReinforcementLearning 
  #RL-PolicyGradient 
  #AttentionMechanismAM 
  #ML-AssistedHeuristics 
  #ML-AssistedExact 
  #LargeScaleVRP 
  #CVRP 
  #GreenVRP 
  #NeuralCombinatorialOptimization 
  #HybridLocalSearchRL

- **Relevant**: true  
- **Fit Score**: 10  
- **State of the Art (SoA) Concepts**:
  - POMO approach (policy gradient RL for VRP)
  - Decomposition-based sub-problem solving (using ML)
  - Set partitioning formulation for recombination
  - Extensions to Green VRP
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: Uchoa et al. 2017 benchmark (for CVRP), Erdoğan & Miller-Hooks 2012 (for GVRP)

## Abstract
Designing efficient heuristics for different variants of the vehicle routing problem (VRP) and customizing them to various input distributions is time-consuming and requires expert knowledge. In recent years, end-to-end machine learning (ML) methods have offered a generic alternative, but they often struggle to scale to larger problem instances. This paper proposes a new hybrid approach that dynamically decomposes a large-scale VRP instance into smaller sub-problems, solves each with a learned constructive heuristic, and uses a set-partitioning formulation to combine sub-solutions. Experimental results on capacitated VRP instances with hundreds to thousands of nodes demonstrate small optimality gaps compared to best-known solutions, significantly improving on baseline ML models. Furthermore, it generalizes to the green VRP with minimal modifications.

## Summary
- **Core Purpose**  
  - The paper addresses the challenge of creating scalable ML-based heuristics for large-scale capacitated vehicle routing problems (CVRPs). Traditional exact or metaheuristics can be effective, but are labor-intensive to adapt for new problem variants. Current ML approaches (e.g., attention models) typically struggle with scalability on large VRP instances or fail to enforce constraints such as fleet size. This paper’s goal is to build on existing ML solvers and scale them to hundreds or thousands of customers while guaranteeing feasibility.

- **Key Contributions**  
  1. **Dynamic Decomposition**  
     - The large CVRP is iteratively split into sub-problems. Each sub-problem corresponds to a subset of the current solution’s routes, ensuring sub-problem size is bounded by a predefined threshold (e.g., 125 customers).
     - By restricting each sub-problem to a smaller instance size, a neural network trained for that size can be employed effectively.
  2. **Learned Heuristic for Sub-Problems**  
     - A reinforcement learning (RL) constructive heuristic, based on the POMO (Policy Optimization with Multiple Optima) attention model, is used to generate multiple solutions for each sub-problem.
     - The model is trained on diverse instances (varying in size and structure) to improve generalization. Masks are used to enforce vehicle capacity constraints.
  3. **Set Partitioning Recombination**  
     - The union of routes from the sub-problem solutions is combined via an integer linear programming set-partitioning (SPP) formulation.
     - This step ensures the fleet-size constraint is satisfied and selects an optimal subset of routes. It serves as a post-processing technique that systematically picks the best possible recombination of partial solutions.
  4. **Green VRP Extension**  
     - With minor modifications to the masking (to enforce fuel/battery constraints) and to node features (fuel or battery levels, recharging stations), the authors adapt their approach to green VRP instances, showing the generality of their method.

- **Methodology**  
  - **Initial Construction**: The Clarke-Wright savings heuristic quickly provides a feasible solution with the correct fleet size, which is then decomposed into sub-problems.
  - **Sub-Problem Generation**: Sub-routes are selected by proximity in the solution, ensuring no sub-problem exceeds the size the NN can handle efficiently.
  - **RL Solver**: An attention-based pointer network variant (POMO) constructs multiple routes. Unlike naive sampling, POMO uses multiple greedy decoders (with diverse starting nodes and geometric augmentations) to yield a high-quality set of candidate routes.
  - **Set Partitioning**: The final recombination step is formulated as a small ILP, solved with a standard MIP solver. It ensures route selection stays within fleet limits.

- **Experimental Results**  
  - Benchmarks: Tested on large CVRP instances from Uchoa et al. (2017) and extended to GVRP instances from Erdoğan & Miller-Hooks (2012).
  - **Comparisons**:
    - Baseline ML solvers (AM, POMO) degrade substantially in solution quality on large instances, often violating route constraints.
    - Clarke-Wright yields feasible solutions quickly but relatively higher gaps.
    - The proposed decomposition + set-partitioning approach outperforms these baselines, achieving gaps under ~3% on medium-to-large instances with up to 1000 customers.
    - Although specialized CVRP heuristics such as HGS (hybrid genetic search) and LKH find better solutions, they require more custom engineering and do not directly extend to other variants (unlike the authors’ approach).

- **Strengths & Impact**  
  - Demonstrates how to incorporate existing RL-based VRP models into a hybrid method for large-scale CVRPs.
  - Simple sub-problem selection procedure: sub-routes chosen around route centroids. This is easily adapted to variants that share the same notion of route constraints.
  - The set-partitioning ILP ensures a feasible, fleet-limited solution—tackling a known shortcoming of many ML heuristics.
  - Extensible to the green VRP by small modifications in masking and embeddings.

- **Limitations & Future Directions**  
  - Currently, the sub-problem selection is heuristic-based; more sophisticated or adaptive selection might improve outcomes.  
  - Although set-partitioning significantly improves solutions, it may be expensive in terms of runtime for very large sub-problems.  
  - The method is tested on CVRP and GVRP but can be extended to other deterministic VRP variants with additional constraints (e.g., time windows), provided the masking logic is updated accordingly.  
  - They suggest advanced strategies like instance-based retraining or dynamic sub-problem selection as potential enhancements.

- **Relevance to Deterministic VRP ML Research**  
  - This paper fits well within the domain of using reinforcement learning for deterministic VRP.  
  - It addresses a key question of scalability and constraint enforcement, which is highly relevant for real-world large-scale deployments.  
  - The approach demonstrates promising generalization to other deterministic variants like green VRP, meeting the core interest of research that aims to unify VRP solution design under an ML framework.
