# Publication Years

1956: 1
1959: 1
1991: 1
2000: 1
2001: 1
2005: 2
2007: 1
2008: 1
2009: 1
2010: 2
2011: 2
2012: 1
2013: 1
2014: 4
2015: 2
2016: 2
2017: 6
2018: 3
2019: 12
2020: 17
2021: 13
2022: 21
2023: 23
2024: 16
2025: 2

SUM: 137



# Type of VRP

- **mVRPSTW**: Multi-Vehicle Routing Problem with Soft Time Windows  
- **CVRP**: Capacitated Vehicle Routing Problem  
- **mCVRPTW**: Multi-Vehicle Capacitated Vehicle Routing Problem with Time Windows  
- **EVRPTW**: Electric Vehicle Routing Problem with Time Windows  
- **PDP**: Pickup and Delivery Problem  
- **HCVRP**: Heterogeneous Capacitated Vehicle Routing Problem  
- **TDGVRPTW**: Time-Dependent Green Vehicle Routing Problem with Time Windows  
- **SDVRP**: Split Delivery Vehicle Routing Problem  
- **CC-CVRP**: Constrained Clustering for the Capacitated Vehicle Routing Problem  
- **VRPTW**: Vehicle Routing Problem with Time Windows
- **MO-VRPTW**: Multi-Objective Vehicle Routing Problem with Time Windows  
- **LC-MDVRP**: Low-Carbon Multi-Depot Vehicle Routing Problem  
- **MDVRP**: Multi-Depot Vehicle Routing Problem  
- **MDVRPTW**: Multi-Depot Vehicle Routing Problem with Time Windows  
- **MDCVRP**: Multi-Distribution-Center Vehicle Routing Problem  
- **PVRPTW**: Periodic Vehicle Routing Problem with Time Windows  
- **GLRP**: Green Location Routing Problem  
- **HVRP**: Heterogeneous Vehicle Routing Problem  
- **SB-VRP**: Swap Body Vehicle Routing Problem  
- **VRP**: Vehicle Routing Problem

# Type of ML Algo

  - Reinforcement Learning (RL), especially:
     * Policy gradients (REINFORCE)
     * Q-learning (some use DQN)
     
   - Transformermodels (attention-based, pointer networks)
   - Graph-based networks (GCN, GNN embeddings)
   - Supervised Learning:
     * heuristic operators (insertion, local search)
     * parameter tuning (like hyper-heuristics)
   - Large Neighborhood Search guided by neural nets
     (destroy & repair operators learned via RL)
   - Evolutionary/Metaheuristics (GA, PSO, NSGA-II, etc.)
     combined with ML for operator selection or parameter adaptation
   - Clustering-based heuristics using machine learning
     (like k-means, DBSCAN, or specialized cluster algorithms)
   - Hybrid approaches:
     * RL + local search (2-opt, swap, insertion)
     * Learned value functions + MIP/column generation
     * Data-driven heuristics to refine or select columns/paths

# Overview Chart
<font color="#ff0000">INCOMPLETE</font>

| **Author**        | **ML Category**                  | **ML Variant**                      | **VRP Problem**        | **Notes**                                                             |
| ----------------- | -------------------------------- | ----------------------------------- | ---------------------- | --------------------------------------------------------------------- |
| [qi2022]          | Hybrid Methods                   | Q-Learning + NSGA-II                | TDGVRPTW               | Multiobjective approach mixing Q-learning with evolutionary algorithm |
| [syed2019]        | Hybrid Methods                   | Supervised NN + LNS Heuristic       | VRPTW (Ride-Hailing)   | Learned insertion decisions combined with large neighborhood search   |
| [wang2020b]       | Hybrid Methods                   | GA/Heuristic-Based                  | Deterministic VRP      | Location-routing with eco-packages, no RL but combined metaheuristics |
| [zong2022]        | Hybrid Methods                   | PCA, LSTM + Genetic Search          | CVRP                   | Hierarchical RL-like pipeline with a generator + metaheuristic        |
| [moradi2020]      | Hybrid Methods                   | Learnable Evolution Model           | Multi-Objective VRPTW  | Evolutionary approach with an embedded ML mechanism                   |
| [zhao2021]        | Hybrid Methods                   | Deep RL + Local Search              | CVRP, VRPTW            | Combines policy gradient with OR-Tools LNS                            |
| [santana2023]     | Hybrid Methods                   | Neural Nets for Local Search        | VRP                    | NN-based local search and crossover ("Is it overkill?")               |
| [furian2021]      | Hybrid Methods                   | ML + Branch and Price               | Sampled VRP            | Data-driven column generation approach                                |
| [wang2022]        | Hybrid Methods                   | 3D-k-means + GA/PSO                 | Collaborative MC-VRPTW | Multi-center VRP with time windows + pickups                          |
| [chen2019]        | Hybrid Methods                   | RL + VNS                            | Periodic VRPTW         | A real-life open-route scenario, variable neighborhood search plus RL |
| [zhou2022]        | Hybrid Methods                   | RL + Adaptive Ant Colony            | VRPTW                  | Gradient-based adjustments in ACO for time window routing             |
| [qin2021]         | Hybrid Methods                   | RL-based Hyper-Heuristic            | Heterogeneous VRP      | Learns to select metaheuristic operators                              |
| [kubra2019]       | Hybrid Methods                   | Route-First-Cluster-Second          | Personal Service RP    | Genetic approach for the partitioning step                            |
| [nallusamy2010]   | Hybrid Methods                   | Approximation / Metaheuristics      | Multiple VRPs          | Combination of multiple heuristic strategies                          |
| [reed2014]        | Hybrid Methods                   | Ant Colony Algorithm                | Multi-Compartment VRP  | Deterministic approach with specialized compartments                  |
| [xu2018]          | Hybrid Methods                   | Enhanced ACO                        | Dynamic VRP            | Time-varying environment, though not purely stochastic                |
| [geetha2013]      | Hybrid Methods                   | Nested PSO                          | Multi-Depot VRP        | Particle swarm for multi-depot routing                                |
| [qi2011]          | Hybrid Methods                   | Spatiotemporal Distance + Heuristic | VRPTW                  | No explicit stochastics, uses specialized distance measure            |
| [luo2014]         | Hybrid Methods                   | Modified Shuffled Frog Leaping      | MDVRP, MDVRPTW         | Multi-phase approach with resource constraints                        |
| [gao2016]         | Hybrid Methods                   | ACO + Clustering                    | Dynamic LRP            | Ant-colony with dynamic location routing                              |
| [yao2019]         | Hybrid Methods                   | ADMM-based Decomposition + Heur.    | VRPTW                  | No direct stochastics, uses splitting approach                        |
| [bai2007]         | Hybrid Methods                   | RL/Memory-based Hyper-Heuristic     | VRP with Time Windows  | Adapts a set of heuristics using RL feedback                          |
| [sabar2015]       | Hybrid Methods                   | Multiarmed Bandit + GEP             | Combinatorial VRPs     | Hyper-heuristic with RL mechanism for operator choice                 |
| [soria2017]       | Hybrid Methods                   | Heuristic Subset Selection          | VRP (Deterministic)    | Searching for the best subset of heuristics                           |
| [chen2020b]       | Hybrid Methods                   | Neural Network for LNS              | CVRP                   | Deep model guiding partial removal in large neighborhood search       |
| [zhang2020]       | Reinforcement Learning (RL/DRL)  | REINFORCE + Multi-Head Attention    | mVRPSTW                | Multi-agent RL for multiple vehicles, soft time windows               |
| [lin2022]         | Reinforcement Learning (RL/DRL)  | REINFORCE (Greedy Rollout), S2V     | EVRPTW                 | Electric vehicle routing with time windows                            |
| [falkner2020]     | Reinforcement Learning (RL/DRL)  | REINFORCE (Self-Attention)          | mCVRPTW                | Joint attention mechanism for multi-vehicle time-window routing       |
| [li2022]          | Reinforcement Learning (RL/DRL)  | RL (Multi-Head Attention)           | PDP                    | Pickup and Delivery with deterministic setup                          |
| [li2022a]         | Reinforcement Learning (RL/DRL)  | REINFORCE + Multi-Head Attention    | HCVRP                  | Heterogeneous capacity problem solved via deep RL                     |
| [duan2020]        | Reinforcement Learning (RL/DRL)  | RL (REINFORCE), GCN-Embeddings      | CVRP                   | Uses graph embeddings in a policy-based RL approach                   |
| [hottung2020]     | Reinforcement Learning (RL/DRL)  | Policy-based RL (LNS Operators)     | CVRP, SDVRP            | Neural large neighborhood search using destroy/repair                 |
| [silva2019]       | Reinforcement Learning (RL/DRL)  | Q-Learning for Local Search         | VRPTW                  | Adaptive local search approach                                        |
| [delarue2020]     | Reinforcement Learning (RL/DRL)  | Value-Based RL + MIP                | CVRP                   | Combines combinatorial RL with a mixed-integer solver                 |
| [ma2022]          | Reinforcement Learning (RL/DRL)  | Policy Gradient (Actor-Critic)      | CVRP                   | Iterative solver with local operators like 2-opt, swap, insert        |
| [wu2020]          | Reinforcement Learning (RL/DRL)  | Policy Gradient + Local Operators   | VRPTW                  | Improving tours (destroy & repair) with learned heuristics            |
| [gao2020]         | Reinforcement Learning (RL/DRL)  | Actor–Critic (GNN)                  | CVRP, CVRPTW           | LNS destroy & repair guided by a graph neural net                     |
| [ren2022]         | Reinforcement Learning (RL/DRL)  | Multi-Agent RL                      | VRP (Capacity)         | Multiple vehicles, route coordination                                 |
| [chen2023]        | Reinforcement Learning (RL/DRL)  | GNN Encoder + Coordination          | Pickup & Delivery      | RL solution for mixed delivery & pickup routes                        |
| [chen2022]        | Reinforcement Learning (RL/DRL)  | Deep Q-Learning                     | Same-Day Delivery      | Vehicles + drones, deterministic approach                             |
| [alqahtani2022]   | Reinforcement Learning (RL/DRL)  | Deep RL                             | EV Routing             | Multi-EV scheduling and routing, deterministic                        |
| [alwesabi2022]    | Reinforcement Learning (RL/DRL)  | Deep RL                             | EV Fleet               | Fleet optimization for smart electric motorcycles                     |
| [aljohani2020]    | Reinforcement Learning (RL/DRL)  | Deep RL + Markov Chain              | EV Routing             | Real-time metadata-driven approach for energy minimization            |
| [gupta2022]       | Reinforcement Learning (RL/DRL)  | Deep RL                             | CVRPTW                 | Large-scale capacity + time windows routing                           |
| [liu2022]         | Reinforcement Learning (RL/DRL)  | Deep RL                             | Large-scale VRPTW      | Deterministic approach with time windows                              |
| [jiang2022]       | Reinforcement Learning (RL/DRL)  | Improved RL                         | Multi-Distribution VRP | Parallel distribution routing optimization                            |
| [shou2022]        | Reinforcement Learning (RL/DRL)  | Multi-Agent RL (Markov Games)       | Traffic Assignment     | Treated as a routing game, though deterministic                       |
| [ding2021]        | Reinforcement Learning (RL/DRL)  | Multi-Agent RL                      | Urban Crowd Sensing    | For-hire vehicles in crowd sensing tasks                              |
| [dacosta2021]     | Reinforcement Learning (RL/DRL)  | Deep RL (Learned 2-opt)             | VRP                    | Learns local search operators for routing                             |
| [xin2020]         | Reinforcement Learning (RL/DRL)  | Multi-Decoder Attention             | CVRP                   | Pointer-network-style solution for capacity routing                   |
| [lu2020]          | Reinforcement Learning (RL/DRL)  | Learning-based Iterative Method     | CVRP                   | Policy that iteratively improves route solutions                      |
| [peng2020]        | Reinforcement Learning (RL/DRL)  | Dynamic Attention Model             | CVRP, SDVRP            | Transformer-based RL approach using graph attention                   |
| [sheng2020]       | Reinforcement Learning (RL/DRL)  | Pointer Network                     | VRP (Task Priority)    | RL-based pointer net, limited resources                               |
| [bello2017]       | Reinforcement Learning (RL/DRL)  | Neural Combinatorial Opt (Pointer)  | TSP                    | Pioneering pointer network approach with policy gradient              |
| [deudon2018]      | Reinforcement Learning (RL/DRL)  | Policy Gradient (PN)                | TSP                    | Learning heuristics for TSP via RL                                    |
| [kool2019]        | Reinforcement Learning (RL/DRL)  | Attention Model                     | TSP, CVRP              | Transformer approach for route construction                           |
| [ma2020]          | Reinforcement Learning (RL/DRL)  | Graph Pointer + Hierarchical RL     | TSP                    | Layered RL approach to build tours                                    |
| [hu2020]          | Reinforcement Learning (RL/DRL)  | RL for mTSP                         | mTSP                   | Cooperative approach with agent-based RL                              |
| [li2021]          | Supervised Learning Approaches   | Supervised + MIP Delegation         | Large-Scale CVRP       | Model learns to delegate tasks to an exact solver                     |
| [poullet2020]     | Supervised Learning Approaches   | ML-based VRPTW Solver               | Large-Scale VRPTW      | Uses ML for large-scale time-window routing                           |
| [zou2022]         | Supervised Learning Approaches   | Transformer (Multi-Head Attention)  | Low-Carbon MDVRP       | An improved attention-based model, deterministic green routing        |
| [morabit2022]     | Supervised Learning Approaches   | ML for Arc Selection                | VRP with Time Windows  | Constrained shortest paths in column generation                       |
| [mandi2021]       | Supervised Learning Approaches   | NN for Hidden Preferences           | VRP with Preferences   | Learns arc probabilities from data                                    |
| [deb2023]         | Supervised Learning Approaches   | State-of-Charge Prediction          | E-Vehicle Relocation   | ML approach for battery level forecasting in relocation               |
| [calvet2016b]     | Supervised Learning Approaches   | Statistical Learning (Param Tuning) | Multi-Depot VRP        | Fine-tuning metaheuristic parameters                                  |
| [kadaba1991]      | Supervised Learning Approaches   | Adaptive ML & KB System             | Routing & Scheduling   | Early work blending expert systems with neural nets                   |
| [cooray2017]      | Supervised Learning Approaches   | ML-tuned Genetic Algorithm          | Energy Minimizing VRP  | Parameter tuning for GA in a VRP scenario                             |
| [zunic2020]       | Supervised Learning Approaches   | Data-Driven Hyperparameter          | Real-world VRPs        | Adaptive approach to choose best heuristics                           |
| [desaulniers2020] | Supervised Learning Approaches   | GNN for Column Selection            | Deterministic VRPs     | ML-based approach to pick best columns in generation                  |
| [kruber2017]      | Supervised Learning Approaches   | KNN Classifier (DW Decomposition)   | Decomposition for VRP  | Learns when to decompose via Dantzig–Wolfe                            |
| [vinyals2015]     | Supervised Learning Approaches   | Pointer Networks (Seq2Seq)          | TSP                    | Introduces pointer net architecture, trained in a supervised manner   |
| [joshi2019a]      | Supervised Learning Approaches   | GCN for TSP Feasibility             | TSP                    | Predicts edges to form tours in a supervised manner                   |
| [kaempfer2019]    | Supervised Learning Approaches   | Permutation Invariant Pooling       | mTSP                   | Proposed network architecture for multiple TSP                        |
| [alesiani2022]    | Unsupervised Learning Approaches | Constrained Clustering              | CC-CVRP                | Focus on cluster-first routing for the capacitated problem            |
| [wang2020a]       | Unsupervised Learning Approaches | Clustering/Heuristic                | Deterministic VRP      | State–space–time representation with cluster-based optimization       |
| [hottung2021]     | Unsupervised Learning Approaches | VAE (latent space)                  | TSP, CVRP              | Learns a latent representation to guide search                        |
| [erdogan2012]     | Unsupervised Learning Approaches | DBSCAN + Clarke-Wright              | Green VRP              | Clustering approach for routing with emission constraints             |
| [comert2017]      | Unsupervised Learning Approaches | Clustering + Heuristic              | VRP w/ Hard Time Win.  | Two-stage solution, no stochastics                                    |
| [comert2018]      | Unsupervised Learning Approaches | K-Means / K-Medoids + Routing       | CVRP                   | Cluster-first, then route-second                                      |
| [gocmen2019]      | Unsupervised Learning Approaches | K-Means for Intermodal Networks     | VRP + 3D Bin Packing   | Clustering-based distribution planning                                |
| [he2009]          | Unsupervised Learning Approaches | Balanced K-Means                    | Large-Scale VRP        | Regional partitioning to reduce complexity                            |
| [rautela2019]     | Unsupervised Learning Approaches | Capacitated Clustering + VRP        | Distribution Planning  | Combining clustering with a capacity-based routing plan               |
| [yucenur2011]     | Unsupervised Learning Approaches | Shape-Based Genetic Clustering      | Multi-Depot VRP        | Genetic approach focusing on geometric partition                      |
| [miranda2017]     | Unsupervised Learning Approaches | Cluster-First, Route-Second         | Swap Body VRP          | Deterministic scenario with specialized vehicles                      |
