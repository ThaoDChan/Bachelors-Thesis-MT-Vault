# Neural Networks for Local Search and Crossover in Vehicle Routing: A Possible Overkill?

## Metadata
- **Link to PDF**: [[[santana2023]_Neural_Networks_for_Local_Search_and_Crossover_in_Vehicle_Routing-A_Possible_Overkill?.pdf]]
- **Tags**:
  - #ML-SupervisedFromOptimal
  - #ML-AssistedHeuristics
  - #ML-AssistedLocalSearch
  - #GraphNeuralNetworks
  - #NeuralCombinatorialOptimization
  - #GeneticAlgorithm
  - #LocalSearchHeuristics
  - #CVRP
  - #LargeScaleVRP
  - #VRPBenchmarks
- **Relevant**: true
- **Fit Score**: 10
- **State of the Art (SoA) Concepts**:
  - Hybrid Genetic Search (HGS)
  - Local search with learned “heatmap” edges
  - Graph Neural Networks for edge probability prediction
  - Granular search (pruning neighborhoods)
  - Ablation study comparing distance-based vs. GNN-based relatedness
- **Performance Evaluation**: yes
- **Performance Evaluation Framework**: Benchmarks from Uchoa et al. (2017) “Set X” and Queiroga et al. (2022) “Set XML”  

## Abstract
Extensive research has been conducted, over recent years, on various ways of enhancing heuristic search for combinatorial optimization problems with machine learning algorithms. In this study, we investigate the use of predictions from graph neural networks (GNNs) in the form of heatmaps to improve the Hybrid Genetic Search (HGS), a state-of-the-art algorithm for the Capacitated Vehicle Routing Problem (CVRP). The crossover and local-search components of HGS are instrumental in finding improved solutions, yet these components essentially rely on simple greedy or random choices. It seems intuitive to attempt to incorporate additional knowledge at these levels. Throughout a vast experimental campaign on more than 10,000 problem instances, we show that exploiting more sophisticated strategies using measures of node relatedness (heatmaps, or simply distance) within these algorithmic components can significantly enhance performance. However, contrary to initial expectations, we also observed that heatmaps did not present significant advantages over simpler distance measures for these purposes. Therefore, we faced a common -though rarely documented- situation of overkill: GNNs can indeed improve performance on an important optimization task, but an ablation analysis demonstrated that simpler alternatives perform equally well.

## Summary
- **Core Idea & Motivation**
  - Investigates whether incorporating neural-network-derived “edge probabilities” (heatmaps) can enhance a leading metaheuristic for the Capacitated Vehicle Routing Problem (CVRP).
  - Focus is on two critical heuristic components of the Hybrid Genetic Search (HGS): local search neighborhoods and crossover operator.

- **Methods & ML Techniques**
  - Employs a Graph Neural Network (GNN) trained to predict the likelihood (probability) of edges appearing in near-optimal solutions (supervised from optimal/high-quality solutions).
  - Integrates these learned “edge probabilities” (heatmaps) into:
    1. Granular local search: restricts the neighborhood to highly “related” customers based on either distance or GNN output.
    2. A modified ordered crossover (OX): chooses reconnection points in the offspring by referencing the most “related” nodes.

- **VRP Variant Addressed**
  - Capacitated VRP (CVRP) under deterministic settings.
  - Benchmarks used include the diverse “X” set (100 to 1000 nodes) and “XML” set (10,000 instances, each with 100 nodes).

- **Key Results & Findings**
  - The addition of a “relatedness metric” (whether distance-based or GNN-based) to guide local search and crossover improves solution quality significantly over the original HGS baseline.
  - Surprisingly, GNN-based heatmaps do not outperform a simpler distance metric in either local search or crossover improvements.
  - Ablation studies show that the complexity of GNN-based predictions offers no clear advantage if a well-tuned distance-based measure is also used.
  - Demonstrates a practical “overkill” scenario: neural networks can help, but simpler heuristic-based relatedness achieves similar gains.

- **Strengths & Contributions**
  - Comprehensive experimental campaign over 10,000+ instances ensures robust statistical evidence.
  - Proposes new ways to adapt metaheuristics by replacing random or purely distance-based choices with ML-derived probabilities.
  - Shows how to extend a fixed-size GNN inference to instances of varying size via subproblem decomposition and aggregation of edge scores.

- **Limitations & Observations**
  - Training and inference times can become a bottleneck for larger instances, though reduced GNN architectures or ignoring inference time alters the trade-off.
  - Methods remain specialized to CVRP; generalization to other VRP variants or other combinatorial problems might need further customization.
  - Highlights the importance of carefully conducted ablation studies before attributing performance gains to advanced ML approaches.

- **Relevance to Thesis**
  - Perfectly aligns with a study of ML-assisted heuristic approaches to the deterministic VRP.
  - Offers insights into balancing GNN complexity vs. simpler heuristics for local search/crossover improvements.
  - Illustrates a cautionary tale: advanced ML approaches can be beneficial, yet simpler methods sometimes suffice or perform equally well.