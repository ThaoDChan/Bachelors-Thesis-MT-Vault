## CONTEXT

The goal of my thesis is to provide a **structured literature review** of **machine learning-based approaches** for the **Vehicle Routing Problem (VRP)** under **deterministic** conditions. Any paper focusing on stochastic or dynamic (non-deterministic) VRPs falls outside of this scope and should be marked as irrelevant.

### Thesis Outline & Focus

1. **Literature Review (Chapter 3)**
   - Papers are clustered by **ML approach**:
     - **Supervised Learning** (e.g., imitation learning from optimal solutions, pointer networks in supervised mode)  
     - **Unsupervised Learning** (e.g., clustering for pre-processing, pattern discovery)  
     - **Reinforcement Learning (RL/DRL)** in deterministic settings (value-based RL, policy gradients, actor-critic)  
     - **Hybrid Methods** (ML-assisted exact solvers, ML-assisted heuristics)
   - We only include **deterministic VRP** research (no random/stochastic route planning).

2. **Performance Evaluation & Benchmarking (Chapter 4)**
   - Standard VRP metrics: solution quality (gap to best-known solutions), runtime, scalability  
   - Common benchmark libraries/datasets (e.g., CVRPLIB, TSPLIB)  
   - Comparison of ML-based models vs. classical optimization methods

3. **Outlook (Chapter 5)**
   - Challenges of applying ML to VRP: data availability, interpretability, integration with real logistics systems  
   - Future directions: combining ML + heuristics for large-scale VRPs, multi-objective VRP, all in deterministic settings

### Research Motivation & Contribution

- Modern supply chains demand efficient, cost-effective, large-scale routing solutions.  
- VRP is computationally hard; exact methods can be slow to scale, while heuristics may yield only approximate solutions.  
- ML-based approaches (e.g., GNNs, RL) show promise in improving solution quality or computational speed, especially when combined with operations research techniques.  
- This thesis will categorize ML models, analyze their VRP applications, compare performance to classical methods, and outline future research directions.

### Summary Format & Focus

- The overall literature review will span approximately 15 pages.
- Each paper will be **summarized in ~60-80 words**, highlighting:
  1. **Core Contribution** or innovation (What problem does it solve? What’s new?)  
  2. **ML Model & VRP Variant** (e.g., supervised pointer network for CVRP, RL-based approach for TSP, etc.)  
  3. **Research Gap** or primary challenge the paper addresses  
  4. **Relevance** to deterministic VRP research (papers on stochastic/dynamic VRP are excluded)

### What to Emphasize When Analyzing Papers

1. **Deterministic VRP Only**: Exclude papers focusing on stochastic or dynamic VRPs.  
2. **ML Model & VRP Variant**: Clearly identify the Machine Learning model used and the specific VRP variant addressed (e.g., TSP, CVRP).  
3. **Core Contribution**: Summarize the main innovation or problem the paper addresses.  
4. **Research Gap**: Identify any notable research gap or challenge the paper aims to solve.  

