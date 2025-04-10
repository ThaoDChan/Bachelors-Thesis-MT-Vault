# Learning to repeatedly solve routing problems

## Metadata
- **Link to PDF**: [[[morabit2024]_Learning_to_repeatedly_solve_routing_problems.pdf]]
- **Tags**: 
  #ML-Supervised 
  #ML-AssistedHeuristics 
  #ML-AssistedExact 
  #HeuristicOrExactBlends 
  #CVRP 
  #LocalSearchHeuristics 
  #LargeScaleVRP 
  #VRPSolverBranchCutPrice 
  #ColumnGenerationHeuristics 
  #BranchAndBound
- **Relevant**: true
- **Fit Score**: 9  
- **State of the Art (SoA) Concepts**:
  - Learned edge-fixing for re-optimizing CVRP solutions
  - Branch-and-price (column generation) for VRP
  - Integration of ML predictions within an exact solver framework
  - Iterated local search heuristics (FILO)
- **Performance Evaluation**: yes
- **Performance Evaluation Framework**: X benchmark (Uchoa et al. 2017)

## Abstract
“In the last years, there has been a great interest in machine-learning-based heuristics for solving NP-hard combinatorial optimization problems. The developed methods have shown potential on many optimization problems. In this paper, we present a learned heuristic for the reoptimization of a problem after a minor change in its data. We focus on the case of the capacited vehicle routing problem with static clients (i.e., same client locations) and changed demands. Given the edges of an original solution, the goal is to predict and fix the ones that have a high chance of remaining in an optimal solution after a change of client demands. This partial prediction of the solution reduces the complexity of the problem and speeds up its resolution, while yielding a good quality solution. The proposed approach resulted in solutions with an optimality gap ranging from 0% to 1.7% on different benchmark instances within a reasonable computing time.”

## Summary
- **Core Focus & Motivation**  
  - Addresses the problem of repeatedly solving a Capacitated Vehicle Routing Problem (CVRP) in which the set of customers remains the same but demands change slightly.  
  - Aims to leverage machine learning to predict which edges from a reference (previous-day) solution will remain in a new solution, thereby reducing computational effort when re-optimizing.

- **ML Technique**  
  - Proposes a **supervised learning** method to classify edges in the original solution as “likely to remain in the new solution” (label = 1) or “likely to change” (label = 0).  
  - Uses an **artificial neural network (ANN)** as a binary classifier (tested against other classification methods).  
  - Training data is generated from solutions of a fast heuristic (FILO), thus the model learns from previously solved instances.

- **Methodology**  
  - **Data Collection**:  
    - For each instance, they generate multiple “perturbed” versions by randomly modifying customer demands within a specified range.  
    - Solve each perturbed instance with the FILO heuristic, capturing which edges overlap with the original solution.  
    - Extract features (e.g., node coordinates, demands, edge costs) to train the classifier.  
  - **Prediction & Edge Fixing**:  
    - The trained model predicts the probability each edge from the original solution will appear in the re-optimized solution.  
    - Edges predicted as “in” are fixed to 1 in the master program of a branch-and-price solver (VRPSolver).  
    - Performs a network reduction step by merging fixed edge sequences.  
    - Solves the reduced CVRP exactly (branch-and-price), typically in seconds or minutes for large instances.

- **Key Findings**  
  - Achieves an **average accuracy** of ~70–80% in predicting overlap edges.  
  - Fixing predicted edges significantly shrinks the search space, leading to a large **speed-up** in re-optimization times.  
  - The final solutions exhibit **small gaps** (<2% on average) compared to the heuristic solution used for training. In some cases, the method hits the same cost as the best heuristic solution.  
  - Outperforms a strong ML-based baseline (DACT) in terms of both solution quality and runtime for these re-optimization tasks.

- **Strengths & Contributions**  
  - Demonstrates a practical approach for **fast re-optimization** in a daily/recurring routing context with minor demand changes.  
  - Offers a straightforward supervised classification pipeline that can be integrated with any branch-and-price or exact VRP solver.  
  - Includes a significant computational study on **X benchmark** instances, providing evidence of effectiveness and scalability.

- **Limitations & Future Work**  
  - Restricted to scenarios where the solution structure (customer set and location) is stable, and only demands change.  
  - “Slight changes” are not formally defined but empirically tested.  
  - Potential improvements lie in using more advanced (graph-based) neural architectures or sequence-based models to enhance prediction accuracy.

- **Relevance to Deterministic VRP**  
  - Entirely focuses on a standard **CVRP with deterministic demands**, except for the variation in demand magnitudes across multiple days.  
  - Provides a novel vantage point on how learned patterns from a known solution can accelerate re-optimization.  
  - Highly relevant for the thesis theme of **machine learning–based approaches** in deterministic routing.
