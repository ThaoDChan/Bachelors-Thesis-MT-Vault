### 1. VRP-Relevant
#### 1.1 VRP-Varianten und Problemformulierung

- **Klassische VRP-Varianten**:  
  - Capacitated VRP (CVRP)  
  - Vehicle Routing Problem with Time Windows (VRPTW), inkl. „hard time windows“  
  - Vehicle Routing with Backhauls (VRPB)  
  - Multi-Depot VRP (MDVRP), Multi-Center VRP  
  - Open VRP, Open Periodic VRP  
  - Multi-Echelon / Zwei-Echelon VRP  
  - Heterogeneous Vehicle Routing Problem (HVRP)  
  - Deterministische VRP-Varianten (z.B. deterministische Nachfrage, feste Zeitfenster)  
  - Pickup-and-Delivery VRP (mit und ohne Zeitfenster)  
  - Green VRP / Energieorientierte (EMVRP, EVRPTW)  
  - Crowd-shipping (Gelegenheitsfahrer)  
  - Multi-Objective VRP (MO-CVRP, min-max vs. min-sum)  
  - Real-world Extensions (z.B. LoggiBUD, große Flotten, reale Distanzen)

- **Spezielle Settings & Constraints**:  
  - Site-Dependent Constraints (unterschiedliche Fahrzeugtypen)  
  - Emissions- und Energieverbrauchs-Modelle (Green VRP, Electric VRP)  
  - Backhauls und gemischte Lieferungen/Abholungen  
  - Zeitfenster (Soft vs. Hard Time Windows)  
  - Kapazitätsbeschränkungen (Fahrzeugladungen etc.)  
  - (Multi-)Depot, (Multi-)Compartment, Periodic Routenplanung

- **TSP als Spezialfall**  
  - TSP, mTSP (Multiple TSP)  
  - TSP mit Zeitfenstern, Concorde als exakter TSP-Solver

---

#### 1.2 Klassische Heuristiken & Lokale Suchverfahren

- **Konstruktive Heuristiken**  
  - Clarke & Wright Savings Heuristic (ggf. modifizierte Varianten)  
  - Sweep-Heuristic  
  - Nearest Neighbor / Cheapest Link / Route-First-Cluster-Second  
  - Cluster-First-Route-Second (K-means / K-medoids zum Clustern)  

- **Lokale Suchverfahren**  
  - 2-opt, 3-opt, Exchange, Relocate  
  - Variable Neighborhood Search (VNS / VND)  
  - Ruin-and-Recreate (FILO-Varianten)  
  - Iterated Local Search (ILS), KGLS Local Search  
  - Simulated Annealing, Tabu Search  

- **Hybride Lokalsuche**  
  - Lokale Suchoperatoren kombiniert mit Metaheuristiken (z.B. LNS + neuraler „Repair Operator“)  
  - Sampling-Strategien (z.B. stochastische Auswahl von Kandidaten)

---

#### 1.3 Metaheuristiken (Nicht-ML)

- **Evolutionäre Algorithmen**  
  - Genetic Algorithm (GA), Hybrid Genetic Search (HGS)  
  - Multi-Objective GA (z.B. NSGA-II, SPEA2)  
  - Co-evolutionäre Ansätze für MO-CVRP  
  - Memetische Algorithmen (shuffled frog leaping, extremal optimization)  
  - Multi-Population / Parallelisierbare Evolutionary Structures

- **Schwarm- und Ameisenoptimierung**  
  - Ant Colony Optimization (ACO), MAX-MIN, BWAS, ACS  
  - Adaptive Pheromon-Updates, Reinforcement-Interpretation von ACO  
  - Particle Swarm Optimization (PSO)  
  - Hybrid GA-PSO

- **Weitere Metaheuristiken**  
  - GRASP (Greedy Randomized Adaptive Search)  
  - Differential Evolution (DE) (diskrete Varianten)  
  - Imperialist Competitive Algorithm (ICA) + Genetic Local Search  
  - Hybrid GA-Tabu Search-Clarke-Wright  
  - Hybrid Quantum-inspirierte (QUBO) Ansätze (wenn *nicht* ML-spezifisch)

---

#### 1.4 Exakte Methoden & Benchmarking

- **Exakte Algorithmen**  
  - Branch-and-Bound, Branch-and-Cut, Branch-and-Price  
  - Column Generation / Dantzig-Wolfe-Reformulierung für VRPTW  
  - Lagrangian Relaxation (untere/obere Schranke)  
  - Set Partitioning Modelle, MILP (Gurobi, CPLEX)  
  - GCG Solver für strukturierte MIP

- **Benchmarks & Vergleich**  
  - TSPLIB, CVRPLIB, Solomon Instances (C, R, RC), DIMACS Challenge  
  - Uchoa’s X Instances, Li & Golden Instanzen  
  - Vergleich mit klassischen Solvern (LKH, OR-Tools, CPLEX)  
  - Real-World Datasets (z.B. LoggiBUD, große Flotten, universitäre Campus-Daten)

---

#### 1.5 ML-basierte Ansätze speziell *für* VRP

- **Reinforcement Learning für VRP**  
  - Policy-Gradient-Methoden (REINFORCE, POMO, DPPO/PPO, Actor-Critic) direkt zur Routen-Konstruktion (TSP, CVRP, VRPTW etc.)  
  - Multi-Agent Reinforcement Learning (MARL) für Multi-Fahrzeug-Routing, ggf. mit Kommunikation  
  - Hierarchische RL-Ansätze („Rewriter + Generator“, Learn to Improve)  
  - Integration von Lokalsuche (2-opt etc.) in RL (z.B. Hybrid RL-heuristic)  
  - MARL in VRPTW, Q-Learning für Operator-Auswahl, L2I (Learn2Improve)  

- **Neural Combinatorial Optimization / Deep Learning**  
  - Pointer Networks (Ptr-Nets) / Transformer-basierte Encoder-Decoder für VRP  
  - Attention-Mechanismen (Multi-Head, Glimpse Layer, AOA), Graph Embeddings  
  - Graph Neural Networks (GNN, GCN, HRGCN) für VRP-Encodings, Feature-Extraktion, Kantenfixierung  
  - Sequence-to-Sequence-Modelle (RNN/GRU-basierte Decoder)  
  - Kombination mit Beam Search, Active Search, Iterativer Verbesserung  
  - Imitation Learning / Supervised Learning von exakten Solver-Lösungen (Concorde, LKH, Gurobi)  
  - Transfer Learning (TSP→CVRP, Generalisierung auf größere Instanzen)

- **Hybride ML + Metaheuristik/Exakt**  
  - ML-unterstützte Initialisierung (z.B. ACO-Pheromon, Clarke-Wright mit ML, Frequent Pattern Mining)  
  - ML-basierte Destroy/Repair-Operatoren in Large Neighborhood Search (LNS)  
  - Integration von ML-Vorhersagen in exakte Solver (Branch-and-Price, MIP, Column Generation)  
  - ML-gestützte Parameter-/Algorithmusauswahl (SVM, Random Forest)  
  - Data-driven Tuning (z.B. GLM) für Metaheuristiken, Automatic Algorithm Selection  
  - Neural Network-basierte Einfügeheuristiken, Kantenwahl („heatmap edges“)

- **Quantum-/QUBO-basierte VRP-Ansätze** (wenn konkret auf VRP angewandt)  
  - Ising/QUBO-Formulierung für Vehicle Routing  
  - Hybrid Quantum-Classical (VQE, QAOA) zur Lösung von VRP  
  - Parallelisierung / Noise Channel Analysis (sofern VRP-Kontext genannt)

---

### 2. ML-Relevante (allgemein, *ohne* expliziten VRP-Bezug)

> Konzepte/Methoden, die rein als KI-/ML-Verfahren aufgeführt sind und nicht konkret in direktem VRP-Kontext genannt wurden.

- **Allgemeine Reinforcement-Learning-Ansätze**  
  - Policy Gradient RL (REINFORCE) in combinatorial optimization (ohne VRP-Erwähnung)  
  - Actor-Critic / PPO / Q-Learning / MCTS in generischem Kontext  
  - Multi-Agent RL mit Kommunikation (generisches MARL)  
  - Approximate Dynamic Programming, Value-based RL (DQN) für „combinatorial tasks“  

- **Deep Learning / Neural Networks**  
  - Pointer Networks (als generisches Seq2Seq)  
  - Transformer-Modelle, Attention-basierte Encoder-Decoder (ohne VRP-Referenz)  
  - Graph Neural Networks (GNN), Graph Convolutional Networks (GCN) (generisch)  
  - Denoising Autoencoder (allgemeine Domain Mapping)  
  - Time Delay Neural Networks (TDNN) für Sequenz-Entscheidungen  
  - Non-autoregressive Decoding, Embedding Glimpse Layer (allgemein)  

- **Klassische ML-Verfahren**  
  - Supervised Learning (Random Forest, Gradient Boosting, SVM, MLP, k-NN, AdaBoost)  
  - Regression-Modelle (Decision Trees, XGBoost, Linear Regression)  
  - Multi-armed Bandit für Operator-Selektion (generisch)  
  - ParamILS / Automatische Parameteroptimierung (ohne VRP-Kontext)

- **Evolutionäre / Meta-Learning**  
  - Multi-Objective Genetic Algorithm (allgemein)  
  - Hyper-Heuristics / Heuristic Selection (generisch)  
  - Meta-Learning für schnelle Adaption (ohne VRP-Spezifikation)

- **Quantum Machine Learning**  
  - Quantum Support Vector Machines (QSVM)  
  - Encoding-Strategien (amplitude, angle, IQP)  
  - Hybrid Quantum-Classical Optimization (VQE, QAOA) (wenn *nicht* konkret VRP-gebunden)  
  - Noise Channel Analysis in Quantencomputing (generisch)

- **Sonstige**  
  - 3D Bin Packing via ML-basierter Heuristik (kein direkter VRP-Bezug)  

