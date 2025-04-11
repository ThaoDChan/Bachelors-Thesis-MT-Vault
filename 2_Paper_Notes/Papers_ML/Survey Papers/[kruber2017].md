# Learning When to Use a Decomposition

## Metadata
- **Link to PDF**: [[[kruber2017]_Learning_When_to_Use_a_Decomposition.pdf]]
- **Tags**:
  - #ML-Supervised
  - #SupportVectorMachines
  - #RandomForestPredictor
  - #AlgorithmSelection
  - #ML-AssistedExact
  - #ML-AssistedBranchAndBound
  - #BranchAndCut
  - #VRPSolverBranchCutPrice
  - #CVRP
  - #DeterministicVRP
- **Relevant**: true  
  *Reasoning: The paper includes a supervised learning approach to determine whether to apply Dantzig-Wolfe decomposition for MIPs, and it explicitly uses CVRP as one of its structured test families.*
- **Fit Score**: 6
- **State of the Art (SoA) Concepts**:
  - Supervised classification (KNN, SVM, Random Forest) for solver/decomposition selection  
  - Dantzig-Wolfe reformulation and branch-and-price method  
  - GCG solver for structured MIP (including CVRP)  
  - Integration of ML into an exact MIP/branch-and-price framework  
- **Performance Evaluation**: no
- **Performance Evaluation Framework**: -

## Abstract
*(From the paper’s Introduction and summary)*  
"Applying a Dantzig-Wolfe (DW) decomposition to a mixed-integer program (MIP) can bring dramatic performance benefits if the model has a suitable structure, but it can fail otherwise. The authors propose a supervised learning method to decide when a DW-based approach outperforms solving the MIP with a standard branch-and-cut solver. They train a binary classifier (e.g., nearest neighbors, SVM, random forests) on a large dataset of MIPs. The classifier predicts whether a decomposition will yield better performance than solving with a generic MIP solver, and also selects among multiple candidate decompositions. Preliminary results show significant gains on instances where structure is detectable, with minimal deterioration on other instances."

## Summary
- **Goal and Motivation**
  - The paper aims to automate the decision of whether to apply a Dantzig-Wolfe decomposition to an MIP by using supervised learning.
  - Although DW reformulation can greatly accelerate the solution of structured problems (e.g., vehicle routing, cutting stock), it can also slow down or fail on problems that lack the relevant structure.
  - The authors want to equip the GCG solver with a predictive model that decides if DW reformulation is beneficial and, if so, which decomposition to choose.

- **Methodology and Approach**
  - They frame the choice “Use Dantzig-Wolfe or stay with branch-and-cut (SCIP)?” as a binary classification problem:
    - For each (problem, decomposition) pair, they gather features—size, types of constraints/variables, detection statistics, linking structures, graph-adjacency insights, etc.
    - They also incorporate the time remaining after decomposition detection as a feature.
    - Several classification methods are tested: k-nearest neighbors (KNN), SVM with RBF kernels, and random forests.
  - Once a decomposition is identified, a second classifier selects the best decomposition among all identified by GCG.

- **Experiments**
  - The authors compiled a dataset of 400 MIP instances covering many structured families (e.g., CVRP, bin packing, network design) plus some “unstructured” MIPLIB instances.
  - Each instance is run with both SCIP (branch-and-cut) and GCG (branch-and-price) under various decompositions; results are recorded as “GCG better” or “SCIP better.”
  - The classifiers are trained on a subset (3/5) of these instances and evaluated on the remaining (2/5).
  - Results:
    - A naive approach that always does DW decomposition fails on many unstructured cases.
    - The trained classifiers significantly reduce the false-positive scenario (i.e., picking DW decomposition when it is harmful).
    - KNN yields solid precision in picking GCG only when beneficial, yielding an overall performance improvement vs. using SCIP alone.

- **Key Findings**
  - Even a limited set of features can predict fairly well when a DW decomposition is advantageous.
  - The “best” model is problem-dependent; k-nearest neighbors show robust results by capturing local instance similarity.
  - Automated decomposition selection is crucial for bridging the gap between specialized (DW-based) and general (branch-and-cut) solvers.
  - For structured cases (including CVRP), the supervised learning approach captures a substantial portion of the gains from the optimal solver choice.

- **Strengths and Limitations**
  - *Strengths*:
    - Demonstrates a practical method to integrate ML into an exact MIP solver.
    - Balances the risk of picking a bad decomposition with the opportunity for huge speedups on structured MIPs.
    - Incorporates multiple decomposition types and uses a wide range of instance families (including CVRP).
  - *Limitations*:
    - Focuses on a two-hour time limit in experiments; generalization to very large or tighter time limits is less explored.
    - Feature engineering is heuristic; more elaborate or domain-specific features could further boost accuracy.
    - The method requires an upfront decomposition detection phase, which can be costly if the final verdict is to use standard branch-and-cut.

- **Relevance to Deterministic VRP**
  - Among the structured problems tested is the Capacitated Vehicle Routing Problem (CVRP).
  - The approach shows how ML can direct the solver to either use a specialized column generation approach or remain with branch-and-cut, depending on predicted runtime gains.
  - This fits into deterministic VRP solution strategies where advanced MIP-based methods sometimes rely on decomposition for large-scale routing.

- **Conclusion and Impact**
  - The study paves the way for “learning-based solver selection” in MIP frameworks, including VRP instances.
  - It highlights how even a simple classification step can save significant computational effort and potentially solve harder VRP instances more efficiently.
  - Points toward future work in “strong detection,” where partial runs of column generation provide deeper features, and in reorganizing detection loops to yield more (and higher-quality) candidate decompositions.