# Learning to Select Initialisation Heuristic for Vehicle Routing Problems
## Metadata
- **Link to PDF**: [[[cavalcanti2023]_Learning_to_select_initialisation_heuristic_for_vehicle_routing_problems.pdf]]
- **Tags**:  
  #ML-Supervised 
  #ML-AssistedLocalSearch 
  #AlgorithmSelection 
  #CVRP 
  #DecisionTreeML 
  #RandomForestPredictor 
  #LargeScaleVRP 
  #HyperHeuristic 
  #DeterministicVRP 
  #SVM
- **Relevant**: true  
- **Fit Score**: 9  
- **State of the Art (SoA) Concepts**:
  - Automatic algorithm selection in VRP  
  - Constructive heuristics (Clark and Wright, Nearest Neighbor, Sweep)  
  - Supervised learning (Random Forest, Decision Trees, SVM, MLP)  
  - Large-scale benchmark instances (Uchoa’s X instances, Li, Golden)  
  - KGLS local search  
  - Role of initial solutions in local search  
- **Performance Evaluation**: yes  
- **Performance Evaluation Framework**: Uchoa’s “X” dataset, Li dataset, and Golden dataset

## Abstract
"The Vehicle Routing Problem (VRP) is a complex problem that comes with a great number of applications in logistics and supply chains. It is non-trivial to select the optimal VRP techniques to solve these applications, especially since there are several possible scenarios. As there is no way to predict how each algorithm would perform until it is (at least partially) deployed, it would make sense in selecting the ones that have higher adaptability to the given environment. In this paper, we consider this idea on the initialisation part of a local search-based metaheuristic. We argue that a proper initialisation is important for obtaining better VRP solutions and apply several machine learning techniques aiming to learn how to use distinct features from four commonly used construction heuristics solutions, predicting the scenarios in which they are the most effective. We also provide relevant discussions on the effects of the initial solution on a local search context. Results show that the proposed method can help select the best or an improving method for the majority of the instances considered, especially for large-scale VRP instances."

## Summary
- **Context and Motivation**  
  - The paper addresses the Capacitated Vehicle Routing Problem (CVRP) under deterministic settings.  
  - Traditional local search-based VRP solvers often focus on powerful improvement steps, overlooking the importance of initial solution construction.  
  - Authors claim that choosing the most suitable constructive heuristic for a given instance can yield better final solutions, especially for large-scale scenarios.  
  - They highlight that, due to the No-Free-Lunch principle, no single heuristic is best for all VRP instances.

- **Core Idea**  
  - Propose a supervised machine learning framework to select one of four initialisation heuristics—Clark and Wright (CW), a large-scale adaptation of it (CW100), Nearest Neighbor (NN), and Sweep—before running a local search (KGLS).  
  - The selection is based on a classification model that predicts which of these heuristics will likely lead to better final solutions after a fixed local search time budget.  
  - The authors label each instance based on the best performing initial heuristic in the final solution obtained by KGLS.

- **Methodology**  
  1. **Initialisation Heuristics**:  
     - **CW** and **CW100**: Clarke and Wright’s savings method (with CW100 limiting the savings table to the 100 closest customers).  
     - **Nearest Neighbor (NN)**: Repeatedly choose the nearest unvisited customer until the vehicle reaches capacity.  
     - **Sweep**: Group customers by rotating a sweep line and form routes until vehicle capacity is reached.  

  2. **Local Search Engine (KGLS)**:  
     - Knowledge-Guided Local Search (KGLS) is used as the improvement procedure.  
     - KGLS incorporates specialized penalization strategies and advanced operators (like Cross-Exchange, Relocation Chain, LKH for intra-route optimization).  

  3. **Feature Extraction**:  
     - For each instance, each heuristic generates an initial solution.  
     - The authors extract various **solution-specific** and **instance-level** features (e.g., route width, average distance, cost, number of trucks used) to characterize the instance+solution pair.  
     - Notably, total solution cost and number of vehicles are included, along with 10 other solution-level features proposed in prior work (e.g., average route radial span, route compactness).  
     - Instance-level features capture instance size, average inter-customer distance, distribution of demands, etc.

  4. **ML Training**:  
     - The final “label” for each instance is which of the four heuristics (CW, CW100, NN, or Sweep) yields the best KGLS outcome after 15 minutes.  
     - The paper uses various ML classifiers (Random Forest, Decision Tree, Support Vector Machines, K-Nearest Neighbors, Multi-Layer Perceptron, Gaussian Process, and others).  
     - Data: 100 X-instances from Uchoa’s library, with diverse sizes and distributions. Split 70% for training and 30% for testing, repeated 30 times to validate consistency.

- **Key Results and Findings**  
  - **Surprising Advantage of “Bad” Heuristics**: Despite producing poorer initial routes, NN sometimes leads to better final solutions than advanced heuristics. This suggests that some heuristics may help local search escape early local optima.  
  - **Classifier Accuracy vs. Final Cost**: Though classification accuracy varies, even modest accuracy can yield a final solution quality that outperforms a baseline fixed heuristic.  
  - **Random Forest, KNN, and SVM**: These often perform better at predicting the best initial heuristic. In many runs, using the model’s recommendation leads to a small but consistent improvement over always using CW100.  
  - **Generality**: The models, when tested on the Li and Golden instances (previously unseen data sets), show mixed results. Due to slight performance differences among heuristics and fewer test instances, the ML-based approach did not consistently outperform the baseline.  
  - **Feature Importance**: Route-level metrics (cost, number of vehicles, route span) dominated the classifier’s decisions, while instance-level features like average distance to depot provided helpful normalization factors.  

- **Strengths**  
  - Highlights the non-trivial impact of initial solutions on final local search outcomes.  
  - Proposes a clear supervised-learning strategy that is easy to integrate into existing local search frameworks with minimal overhead (constructing all four heuristics is fast).  
  - Demonstrates empirical improvements on large-scale benchmark datasets.  

- **Limitations and Future Work**  
  - The training approach currently relies on offline labeling using full 15-minute runs of KGLS. This labeling might be expensive if expanded to larger sets of heuristics or instance variations.  
  - The number of instance features and heuristics considered is limited; further expansions or more advanced constructive heuristics could yield larger gains.  
  - The authors plan to incorporate more heuristics, explore cross-dataset generalization, and refine feature sets.  

- **Relevance to Deterministic VRP and Thesis**  
  - This work fits squarely into a deterministic VRP framework, investigating how to use ML to improve classical local search approaches.  
  - It showcases a direct synergy of supervised learning and heuristic-based VRP solving in large-scale CVRP contexts.  
  - Results underscore that “one-size-fits-all” heuristics are suboptimal for large or diverse sets of instances, emphasizing the potential of data-driven algorithm selection.  