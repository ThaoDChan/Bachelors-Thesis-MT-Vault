# An Efficient Graph Convolutional Network Technique for the Travelling Salesman Problem

## Metadata
- **Link to PDF**: [[[joshi2019a]_An_Efficient_Graph_Convolutional_Network_Technique_for_the_Travelling_Salesman_Problem.pdf]]
- **Tags**:
  - #DeterministicVRP
  - #TSP
  - #SingleVehicleRouting
  - #ML-Supervised
  - #ML-SupervisedFromOptimal
  - #GraphNeuralNetworks
  - #NeuralCombinatorialOptimization
  - #ImitationLearning
  - #BeamSearch
  - #ML-AssistedHeuristics
- **Relevant**: true
- **Fit Score**: 10 
- **State of the Art (SoA) Concepts**:
  - Deep Graph Convolutional Networks for TSP
  - Non-autoregressive decoding with beam search
  - Supervised learning from optimal Concorde solutions
- **Performance Evaluation**: yes
- **Performance Evaluation Framework**: Random TSP instances in the 2D unit square; Concorde used to generate optimal solutions

## Abstract

“This paper introduces a new learning-based approach for approximately solving the Travelling Salesman Problem on 2D Euclidean graphs. We use deep Graph Convolutional Networks to build efficient TSP graph representations and output tours in a non-autoregressive manner via highly parallelized beam search. Our approach outperforms all recently proposed autoregressive deep learning techniques in terms of solution quality, inference speed and sample efficiency for problem instances of fixed graph sizes. In particular, we reduce the average optimality gap from 0.52% to 0.01% for 50 nodes, and from 2.26% to 1.39% for 100 nodes. Finally, despite improving upon other learning-based approaches for TSP, our approach falls short of standard Operations Research solvers.”

## Summary
- **Overview & Motivation**  
  - The paper addresses the 2D Euclidean Travelling Salesman Problem (TSP), a core single-vehicle routing task.  
  - It proposes a supervised machine learning framework where a Graph Convolutional Network (GCN) is trained to predict edges of near-optimal TSP tours.  
  - Motivation stems from the desire to automate or learn TSP heuristics that approximate or replace hand-crafted approaches while retaining competitive solution quality.

- **Core ML Technique**  
  - Employs a deep **Graph Convolutional Network** architecture capable of learning rich node and edge embeddings from 2D TSP data.  
  - Training is done via **supervised learning** (i.e., imitation learning from ground-truth tours) using a large training set of TSP instances and their **optimal solutions** produced by Concorde.  
  - The model outputs, for each pair of nodes, the probability that an edge belongs to the final TSP route.

- **Non-Autoregressive Decoding**  
  - Unlike previous autoregressive or sequence-to-sequence methods, the approach produces a **full adjacency matrix in one shot** rather than a node-by-node tour.  
  - The authors apply a **beam search** procedure over the model’s probabilistic adjacency matrix. This ensures they extract a valid Hamiltonian cycle from the raw edge probabilities.  
  - The beam search is highly parallelizable on GPUs, yielding faster inference compared to autoregressive decoding, which typically processes one node at a time.

- **Key Results & Empirical Findings**  
  - Benchmarked on three fixed graph sizes: TSP20, TSP50, and TSP100.  
  - Achieves a **lower optimality gap** than previous deep learning methods: 
    - For TSP50: gap reduced from 0.52% to 0.01%.  
    - For TSP100: gap reduced from 2.26% to 1.39%.  
  - Notably faster than many competing autoregressive models during inference, mainly because the beam search can be parallelized.  
  - Despite strong performance among learning-based methods, it still does not surpass top classical OR solvers (e.g., Concorde, LKH3).

- **Sample Efficiency & Training Approach**  
  - The supervised approach, trained on labeled examples of optimal solutions, converges faster and requires fewer problem instances to achieve the same or better performance relative to purely reinforcement learning (RL)-based methods.  
  - The authors highlight that RL approaches often rely on a sparse reward signal (e.g., partial tour length) and therefore need extensive exploration and more training epochs to reach comparable performance.

- **Strengths**  
  - **High solution quality** relative to other ML-based TSP solvers, particularly when using a large beam width.  
  - **Fast inference** on large batches of instances due to non-autoregressive decoding.  
  - **Easy interpretability** of edge probabilities, which can be viewed as a heat map of the predicted TSP solution.

- **Limitations & Challenges**  
  - Trained on **fixed-size** instances, meaning the model can struggle to generalize to different (especially larger) graph sizes.  
  - The best solutions arise when combining beam search with a shortest-tour selection among the beams, which can increase runtime.  
  - Even with these optimizations, the method **does not outperform** specialized OR solvers like Concorde, especially for very large instances.

- **Relevance to Deterministic VRP**  
  - The TSP is a fundamental, deterministic single-vehicle routing problem.  
  - This paper’s approach exemplifies how **deep GCNs** and **supervised learning** from optimal solutions can yield near-optimal results for a key VRP variant.  
  - Showcases a **non-autoregressive** strategy, which could inspire broader VRP applications where parallelization is beneficial.

- **Potential Impact & Future Directions**  
  - Demonstrates that **learning from ground-truth solutions** (imitation learning) is efficient for smaller-scale TSP.  
  - Encourages further exploration of **transfer learning** and model adaptation to handle larger, more variable VRP instances.  
  - Suggests combining learned approaches with local search or classical heuristics to refine solutions for real-scale problems.