# Transportation problems for intermodal networks: Mathematical models, exact and heuristic algorithms, and machine learning

## Metadata
- **Link to PDF**: [[[gocmen2019]_Transportation_Problems_for_Intermodal_Networks.pdf]]
- **Tags**:
  - #ML-Unsupervised
  - #ML-Hybrid
  - #GeneticAlgorithm
  - #ClusterFirstRouteSecond
  - #PDP
  - #LargeScaleVRP
  - #HeuristicOrExactBlends
  - #DeterministicVRP
  - #3DLoading
  - #IntermodalTransport
- **Relevant**: true  
- **Fit Score**: 9  
- **State of the Art (SoA) Concepts**:
  - K-means for clustering VRP customers
  - 3D bin packing solved via ML-based best-fit-first heuristic
  - Genetic algorithm for the routing component (pick-up routing)
  - Mixed-integer programming for train loading assignment
- **Performance Evaluation**: no
- **Performance Evaluation Framework**: -

## Abstract
“This paper presents a combinatorial problem called a pick-up routing problem with a three-dimensional (3D-PRP) loading constraint, clustered backhauls at the operational level, and train loading at the tactical level for an intermodal transportation network. A two-phase approach, called clustering first, packing-routing second, is proposed for use during the first stage. The clustering of backhauls is carried out using the k-means algorithm. A hybrid approach is provided, which combines the packing of orders by first solving a 3D loading problem for each cluster using machine learning with a best-fit-first strategy, with routing using a genetic algorithm. During the second stage, the train-loading problem is solved using a mixed integer programming approach to minimise the total costs by incorporating various cost types, in which detention and demurrage costs are taken into account. All solution approaches are computationally evaluated on real-world data provided by an international logistics firm and new randomly generated instances. Comparisons are carried out using both exact solution methods and heuristic approaches, and the proposed approach was shown to be more effective for real-world problems.”

## Summary
- **Problem & Motivation**  
  - Addresses an intermodal transportation network that involves a two-level decision process:
    1. **Operational level**: Solve a pickup-routing problem with 3D loading constraints (3D-PRP) and clustered backhauls.  
    2. **Tactical level**: Assign the loaded containers to trains to minimize overall costs, including penalties for delays.
  - Integrates a practical scenario of distributing and collecting orders by road, then loading the resulting units onto block trains.

- **Machine Learning Techniques**  
  - Uses **k-means** (unsupervised) to cluster backhaul customers according to geographic coordinates, aiming for faster and more coherent groupings.  
  - Adopts a **machine learning-based best-fit-first** heuristic for the 3D bin packing sub-problem. Each order is characterized by length, width, height, and weight; the ML approach decides how to pack these orders inside containers while respecting 3D constraints.

- **VRP Variant**  
  - The paper focuses on a **pick-up routing problem** (PRP) with 3D loading constraints, effectively a form of pickup-and-delivery VRP. There are no time windows or stochastic elements, so it is a **deterministic** VRP variant.  
  - Backhaul orders must be collected, then consolidated into standard container/trailer units for shipment by rail.

- **Methodology**
  1. **Clustering**: The authors propose a “clustering first, packing-routing second” method.  
     - They use k-means on the set of backhaul customers to form clusters based on location.  
  2. **3D Loading (Packing)**: Each cluster is passed to a 3D bin-packing solver enhanced by a machine learning best-fit-first approach.  
  3. **Routing**: The consolidated loads in each cluster are then arranged into routes with a **genetic algorithm** that aims to minimize travel distance and the number of trucks used.
  4. **Train Assignment**: At a tactical level, the containers from the road stage are assigned to trains using a mixed-integer programming model. This part considers multiple cost components, including detention (late returns) and demurrage (longer stays).

- **Key Results**
  - Validated on real data from a logistics firm (4444 orders, 103 pickup nodes) and on several synthetic instances of different sizes.
  - The combined approach (k-means clustering + ML-based packing + GA routing) significantly reduced both the total travel distance and the number of carrying units versus straightforward exact or open-source approaches when the problem size was large.
  - The integrated train-loading MIP model further minimized costs by factoring in potential penalties.

- **Strengths & Contributions**
  - Novel “clustering first, packing-routing second” structure for a pickup-and-delivery VRP with 3D loading constraints.
  - Demonstrates the use of **machine learning** for generating feasible (and efficient) 3D load plans.
  - Includes a two-stage solution bridging operational (truck routing) and tactical (train assignment) decisions, reflecting real intermodal logistics.
  - Incorporates real-world cost elements (detention/demurrage) for train assignments.

- **Weaknesses & Limitations**
  - Does not benchmark against common VRP repositories or widely used 3D VRP test sets, limiting comparability to standard literature.  
  - The nature of the “machine learning” for 3D bin packing is described as a best-fit-first strategy, but details on training data or robust predictive modeling remain brief.  
  - Focuses on backhaul orders only; linehaul plus backhaul might be more general.

- **Relevance for Deterministic ML-based VRP**  
  - Provides a concrete example of how unsupervised ML (k-means) and a heuristic ML-based packing approach can be integrated with metaheuristics (GA).  
  - Offers insights on bridging multiple transportation modes under deterministic conditions.  
  - Illustrates how real data with thousands of orders can be handled by a hybrid ML and OR approach.