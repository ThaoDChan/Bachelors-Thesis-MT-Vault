# Constructing selection hyper-heuristics for open vehicle routing with time delay neural networks using multiple experts

## Metadata
- **Link to PDF**: [[[tyasnurita2024]_Constructing_selection_hyper-heuristics_for_open_vehicle_routing_with_time_delay_neural_networks_using_multiple_experts.pdf]]
- **Tags**:
  - #ImitationLearning
  - #HyperHeuristic
  - #ML-AssistedHeuristics
  - #ML-AssistedLocalSearch
  - #LocalSearchHeuristics
  - #LargeNeighborhoodSearch
  - #LargeScaleVRP
  - #VRPBenchmarks
  - #DeterministicVRP
  - #Generalization
  - #OpenVRP
  - #TimeDelayNeuralNetworks
- **Relevant**: true 
- **Fit Score**: 10  
- **State of the Art (SoA) Concepts**:
  - Hyper-heuristics for heuristic selection
  - Apprenticeship / imitation learning from expert solutions
  - Time Delay Neural Networks for sequence-based decision-making
  - Open VRP variant with large-scale benchmarks
- **Performance Evaluation**: yes
- **Performance Evaluation Framework**: Standard OVRP benchmarks (Brandão, Li et al.) via HyFlex VRP

## Abstract
“Hyper-heuristics are general purpose search methods for solving computationally difficult problems. A selection hyper-heuristic is composed of two key components: a heuristic selection method and move acceptance criterion. Under an iterative single-point search framework, a solution is modified by selecting and applying a predefined low-level heuristic, with a decision then taken to accept or reject the resulting solution. Designing a selection hyper-heuristic is an extremely challenging task. In this study, we investigate computer-aided design of a selection hyper-heuristic for the open vehicle routing problem. A time delay neural network is used as an offline apprenticeship learning method. Our approach first observes the search behaviour of multiple expert human-designed selection hyper-heuristics on a selected sample of training instances, before automatically generating a selection hyper-heuristic capable of solving unseen instances effectively. The proposed approach is tested on open vehicle routing problem instances of different sizes to examine the performance and generality of the selection hyper-heuristics generated. Improved performance is demonstrated over a set of well-known benchmarks from the literature when compared to using the existing expert systems directly.”

## Summary
- **Context & Motivation**  
  - Paper addresses the Open Vehicle Routing Problem (OVRP), a VRP variant where vehicles do not return to depot.  
  - Proposes a new approach to automatically generate selection hyper-heuristics using offline imitation learning from multiple “expert” hyper-heuristics.  
  - Builds on prior work illustrating that machine learning can enhance or generate metaheuristics for hard combinatorial problems.

- **Core ML Methodology**  
  - Uses a Time Delay Neural Network (TDNN) to learn from data labeled by two state-of-the-art selection hyper-heuristics (AdapHH, MSHH).  
  - **Imitation/apprenticeship learning** framework: gather sequences of (solution improvement, distance metric) → (selected heuristic, move acceptance) from experts on training instances.  
  - Train two TDNN-based components:
    1. **Heuristic Selection Classifier**: decides which low-level heuristic to apply.  
    2. **Move Acceptance Classifier**: determines acceptance/rejection for non-improving solutions, customized per low-level heuristic.  
  - Incorporates domain-independent features (objective difference) and domain-level features (distance between solutions) to inform the TDNN decision.

- **VRP Variant & Setup**  
  - Focuses on **deterministic** Open VRP (OVRP) benchmarks.  
  - Benchmarks come in three scales: “Standard,” “Large,” and “Very Large,” with up to 1200+ customers.  
  - Evaluates the generated hyper-heuristics with 600-second time limits, showing both scalability and generalization to unseen instance sizes.

- **Method & Experiments**  
  - **Data Collection Phase**: Each expert hyper-heuristic is run on representative OVRP training instances; relevant decisions (heuristic chosen, acceptance) and feature vectors are recorded only for accepted moves.  
  - **TDNN Training Phase**: 
    - A separate network is trained for heuristic selection (multiclass classification over 8 low-level heuristics).  
    - For move acceptance, each low-level heuristic has its own acceptance classifier to handle equally good or worsening solutions.  
  - **Deployment**: The trained TDNN-based hyper-heuristic is then tested on unseen OVRP instances. The standard single-point search hyper-heuristic loop is maintained, but with the TDNN-based modules controlling selection and acceptance.

- **Key Results**  
  - The learned approach (trained from both expert systems) outperforms:  
    - Each expert hyper-heuristic individually.  
    - The hyper-heuristic learned from only a single expert’s demonstrations.  
  - Demonstrates strong **generalization** across different instance scales (standard → large, large → very large, etc.).  
  - The paper provides detailed analyses of low-level heuristic usage, showing that the learned policy adapts heuristics well at different stages of the search and merges beneficial expert patterns.

- **Strengths & Relevance**  
  - Shows that **offline apprenticeship learning** can effectively generate robust hyper-heuristics.  
  - Addresses large-scale deterministic OVRP instances up to hundreds or thousands of customers.  
  - Incorporates new insights by merging policies from multiple experts, not just a single approach.

- **Limitations & Future Work**  
  - The approach relies on the quality and diversity of expert data. If experts are suboptimal, the learned policy might inherit those biases.  
  - The fixed set of low-level heuristics and problem features may limit broader cross-domain usage.  
  - Potential future directions include training from additional experts or extending to other deterministic VRP variants, and investigating further cross-domain generalization.

- **Overall Contribution to Deterministic VRP & ML Literature**  
  - Demonstrates a successful combination of domain knowledge, imitation learning, and time-delay neural networks for a deterministic VRP variant (OVRP).  
  - Provides evidence that merging heuristic strategies from multiple experts yields improved solution quality, especially on large-scale VRP instances.  
  - Relevant for research exploring machine learning-based hyper-heuristics, offline policy generation, and extension to other deterministic routing scenarios.