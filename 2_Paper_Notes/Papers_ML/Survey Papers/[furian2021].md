# A machine learning-based branch and price algorithm for a sampled vehicle routing problem

## Metadata

- **Link to PDF**: [[[furian2021]_A_machine_learning-based_branch_and_price_algorithm_for_a_sampled_vehicle_routing_problem.pdf]]
- **Tags**:  
  #ML-Supervised  
  #ML-SupervisedFromOptimal  
  #ML-AssistedExact  
  #ML-AssistedBranchAndBound  
  #RandomForestPredictor  
  #VRPTW  
  #CVRP  
  #VRPSolverBranchCutPrice  
  #BranchAndCut  
  #TimeWindowConstraints  
- **Relevant**: true  
  - *Reasoning*: The paper addresses a deterministic VRP variant (VRPTW and CVRP) and leverages supervised ML to accelerate an exact branch-and-price solver.  
- **Fit Score**: 9  
- **State of the Art (SoA) Concepts**:
  - Machine Learning integrated with exact methods for VRP  
  - Sampled Vehicle Routing Problem (SVRP) concept  
  - Reliability-based branching with predicted scores  
  - Random forests for branching decisions  
  - Branch-and-price algorithms for VRPTW and SCVRP  
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: Solomon’s VRPTW benchmarks (Solomon 1987), Gehring & Homberger (2005) for VRPTW, and sets A, B, E, M for CVRP  

## Abstract
“Planning of operations, such as routing of vehicles, is often performed repetitively in real-world settings, either by humans or algorithms solving mathematical problems. While humans build experience over multiple executions of such planning tasks and are able to recognize common patterns in different problem instances, classical optimization algorithms solve every instance independently. Machine learning (ML) can be seen as a computational counterpart to the human ability to recognize patterns based on experience. We consider variants of the classical Vehicle Routing Problem with Time Windows and Capacitated Vehicle Routing Problem, which are based on the assumption that problem instances follow specific common patterns. For this problem, we propose a ML-based branch and price framework which explicitly utilizes those patterns. In this context, the ML models are used in two ways: (a) to predict the value of binary decision variables in the optimal solution and (b) to predict branching scores for fractional variables based on full strong branching. The prediction of decision variables is then integrated in a node selection policy, while a predicted branching score is used within a variable selection policy. These ML-based approaches for node and variable selection are integrated in a reliability-based branching algorithm that assesses their quality and allows for replacing ML approaches by other (classical) better performing approaches at the level of specific variables in each specific instance. Computational results show that our algorithms outperform benchmark branching strategies. Further, we demonstrate that our approach is robust with respect to small changes in instance sizes.”  

## Summary
- **Context and Motivation**
  - The paper addresses a new variant called the Sampled Vehicle Routing Problem (SVRP), focusing mainly on the time windows version (SVRPTW) and also briefly covering the capacitated version (SCVRP).  
  - The central premise: real-world routing often reuses the same “base set” of potential customers, but actual demands are subsets of that base. This repeats over time, so it is feasible to learn from previous solves.
  - Traditional exact algorithms (branch-and-price or branch-and-cut) solve each new instance from scratch. The authors propose incorporating machine learning to reuse information about optimal solution structures.

- **Problem Definition**
  - VRPTW: Homogeneous fleet, each vehicle with capacity Q, single depot, and time windows [aᵢ, bᵢ] for each customer i. Objective: minimize total distance while respecting capacity and time windows.
  - SCVRP: Similar, but without time window constraints; cuts are applied in the root node to handle symmetry and capacity aspects.
  - Sampled VRP (SVRP): each new instance is a subset of customers drawn from a larger, fixed “base” set of customers.

- **Machine Learning Approaches**
  1. **Predicting Solution Structure**  
     - The authors build two families of supervised models:
       - **Edge-based models (Mᴱ)**: For each potential edge (i, j) in the base set, predict if that edge appears in the optimal solution. Input features include a binary vector that identifies which customers are in the instance, plus additional instance statistics.  
       - **Node-based models (Mᴺ)**: For each node i in the base set, predict its successor in the optimal route. Returns a single node index.  
     - Predictions from these two models are combined and then “post-processed” to enforce basic consistency (i.e., remove impossible degrees or time window violations). This yields a set of predicted edges that approximate the optimal route structure.

  2. **Predicting Strong Branching Scores**  
     - Within a branch-and-price framework, edges with fractional flows must be selected for branching.  
     - Full Strong Branching (FSB) can produce small search trees but is computationally expensive.  
     - The authors train regression forests to imitate FSB scores for each edge. Training data come from running FSB on many solved instances and extracting the bound improvements (∆₁, ∆₂) as labels.

- **Integration into Branch-and-Price**
  - Two major points of integration:
    - **Node Selection**: The node in the search tree is chosen based on whether the branching decision that led to that node matches the predicted solution edges from Mᴱ+Mᴺ. Nodes that align with predicted optimal edges are prioritized.  
    - **Variable (Edge) Selection**: 
      - **Prediction Branching (PB)** simply picks the edge with the highest predicted strong-branching score from the learned models.
      - **Reliability Prediction Branching (RPB)** merges the idea of Reliability Branching with the predicted FSB scores. Early in the tree, a smaller number of full (costly) strong-branching checks occur, and the algorithm compares them to the predicted scores. If the model’s predictions pass a quality threshold, subsequent branching decisions for that edge rely on the learned model. Otherwise, the algorithm falls back to pseudo-cost or conventional branching.

- **Experiments and Benchmarks**
  - Training data: 1000 randomly sampled instances (per base set) were solved with (full) strong branching or reliability branching to collect ground-truth.  
  - Evaluation: 200 new random-sampled test instances for each base instance:
    - Solomon’s VRPTW sets (r, c, rc classes) for the SVRPTW portion.
    - Several CVRP standard sets (A, B, E, M) for the SCVRP portion.
  - Results:
    - **Solution-Structure Predictions**: Achieved high precision (often 70–85%) for edges that truly appear in the optimal solution, with strong correlation metrics. This is beneficial for node selection policies that skip subtrees unlikely to contain the optimum.
    - **Branching Performance**:  
      - PB and RPB led to fewer explored nodes compared to standard heuristics (Most Fractional or Pseudo-Cost) and in many cases improved or matched reliability branching.  
      - For VRPTW instances of size 50, RPB consistently achieved the smallest search trees.  
      - PB was often fastest in wall-clock times, as it avoids additional LP solves (full strong branching checks) once the model is deemed reliable.  
    - The approach was robust to modest changes in instance size (e.g., from 50 to ~60 customers), retaining good predictions without retraining on that exact size.

- **Strengths and Contributions**
  - Demonstrates that supervised ML can accelerate an exact branch-and-price solver for VRPTW and SCVRP, leveraging “experience” from previously solved problem instances.  
  - Proposes a reliability-based approach that detects when the ML predictions are poor and switches back to classical branching heuristics.  
  - Achieves notable reductions in the search tree size and computational time for many benchmark instances.

- **Limitations and Future Directions**
  - The method relies on a potentially expensive offline training phase that must solve many sample instances to optimality via strong branching or reliability branching.  
  - The solution quality might deteriorate if the distribution of future instances deviates significantly from the training distribution (i.e., from the same base set).  
  - Future work could investigate whether models trained on smaller instances or partial coverage of a base set can generalize to larger, more complex subsets.

- **Relevance to Deterministic VRP Research**
  - Although the authors call it a “sampled VRP,” each instance is solved deterministically once the subset of customers is known. This aligns well with a deterministic approach.  
  - Offers a clear example of how classical branch-and-price methods can integrate ML-based predictions to reduce solve times and node counts, which is directly relevant for advanced VRP solution frameworks in practice.