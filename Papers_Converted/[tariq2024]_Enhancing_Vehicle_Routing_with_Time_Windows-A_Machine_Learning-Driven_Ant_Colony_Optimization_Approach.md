## Enhancing Vehicle Routing with Time Windows: A Machine Learning-Driven Ant Colony Optimization Approach

## Khizer Tariq

## Ali Awais Safdar

School of Electrical Engineering and Computer Science National University of Sciences &amp; Technology

Islamabad, Pakistan mtariq.bscs21seecs@seecs.edu.pk

## Mubashir Zaman

School of Electrical Engineering and Computer Science National University of Sciences &amp; Technology Islamabad, Pakistan mzaman.bscs21seecs@seecs.edu.pk

Abstract -The Vehicle Routing Problem with Time Windows (VRPTW) is a challenging optimization problem in logistics and transportation, where the objective is to efficiently plan routes for vehicles to service a set of customers within specified time windows while minimizing costs. This paper introduces a novel approach to solving VRPTW using a Machine LearningDriven Ant Colony Optimization (ML-ACO) algorithm, referred to as Machine Learning-Driven Ant Colony Optimization for Vehicle Routing with Time Windows (ML-ACO-VRPTW). The proposed algorithm enhances traditional ACO by integrating a machine learning model, to predict heuristic values that capture the urgency of time windows and vehicle capacity constraints. The algorithm begins with initializing pheromone levels and parameters, followed by constructing routes based on probabilistic path selection influenced by pheromone intensity and machine learning-predicted heuristic values. Key innovations include dynamic pheromone updates that adapt to the algorithm's progress and the incorporation of penalties for time window violations and vehicle capacity exceedances. The pheromones are used to maintain a memory of the paths, and the machine learning model ensures adaptive and accurate heuristic predictions.

Index Terms -Vehicle Routing Problem with Time Windows, Machine Learning-Driven Ant Colony Optimization, Decision Tree Regressor, Heuristic Prediction, Optimization Algorithms

## I. Introduction

In today's fast-paced global economy, efficient logistics management is critical for business success across various sectors. The growing complexity of supply chains and rising customer expectations for timely deliveries have intensified demands on transportation and distribution systems. Central to these challenges is the Vehicle Routing Problem with Time Windows (VRPTW), an intricate optimization problem in logistics.

The VRPTW is an extension of the classic Vehicle Routing Problem (VRP) and is classified as NP-hard [1], indicat-

School of Electrical Engineering and Computer Science National University of Sciences &amp; Technology Islamabad, Pakistan asafdar.bscs21seecs@seecs.edu.pk

## Syed Usman Ali Shah

School of Electrical Engineering and Computer Science National University of Sciences &amp; Technology Islamabad, Pakistan sshah.bscs21seecs@seecs.edu.pk ing significant computational complexity. This classification means that as the problem size increases, the time required to find an optimal solution grows exponentially, making it impractical to solve large instances using exact methods. In the VRPTW, a fleet of vehicles must service customers within specified time windows while minimizing total costs, including travel distance, vehicle usage, and penalties for time window violations.

Efficiently solving the VRPTW is essential in today's logistics landscape, as optimal or near-optimal solutions can yield substantial cost savings, improve customer satisfaction, and reduce environmental impact through resource efficiency. However, the NP-hard nature of VRPTW necessitates sophisticated heuristic and metaheuristic approaches to find highquality solutions within reasonable computational times.

In this paper, we introduce a novel method for addressing the VRPTW: a Machine Learning-Driven Ant Colony Optimization algorithm, termed ML-ACO-VRPTW. This innovative approach builds on the traditional Ant Colony Optimization (ACO) framework by incorporating a machine learning model, to predict heuristic values that consider both time window urgency and vehicle capacity constraints. Our goal is to balance solution quality with computational efficiency, meeting the practical needs of logistics planners and decision-makers.

The remainder of the paper is structured as follows: Section II reviews the related literature, Sections III and IV define the VRPTW problem, Section V presents the proposed ML-ACOVRPTW algorithm, Section VI describes the experimental study and results, and Section VII concludes the paper.

## II. Literature Review

The Vehicle Routing Problem with Time Windows (VRPTW) is a significant combinatorial optimization prob-

lem with numerous applications in transportation and logistics. This literature review explores recent advancements and methodologies in solving VRPTW, highlighting approaches that combine traditional optimization techniques with modern machine learning methods.

Keskin's work in 'Multi-Criteria Decision Making With Machine Learning for Vehicle Routing Problem' proposes a three-stage approach to address VRPTW. The methodology involves clustering customers using a fuzzy c-means (FCM) algorithm, predicting travel times with support vector regression (SVR), and selecting optimal routes through multi-criteria decision analysis techniques like the Analytic Hierarchy Process (AHP) and the Technique for Order of Preference by Similarity to Ideal Solution (TOPSIS) [2]. This approach integrates machine learning for enhanced prediction accuracy and decision-making, improving the feasibility and effectiveness of route planning in real-world scenarios.

The paper 'Combining variable neighborhood search and machine learning to solve the vehicle routing problem with crowd-shipping' introduces an innovative crowd-shipping model, which leverages underused resources such as occasional drivers to enhance delivery operations [3]. The authors apply a variable neighborhood search (VNS) meta-heuristic, integrating machine learning techniques to explore promising areas of the solution space. This hybrid approach demonstrates improved effectiveness over traditional methods, showcasing the potential of combining meta-heuristics with machine learning for complex routing problems.

Sun et al.'s research on 'Boosting ant colony optimization via solution prediction and machine learning' presents a hybrid approach that combines machine learning with ant colony optimization (ACO) to solve combinatorial optimization problems [4]. Their ML-ACO algorithm involves training classification models to predict high-quality solutions and integrating these predictions into the ACO framework. By using predicted probabilities to bias pheromone updates and route construction, the algorithm significantly enhances performance across various problem instances. This integration of machine learning with meta-heuristics not only boosts solution quality but also generalizes well to different problem domains, including large synthetic and real-world instances.

Building upon the basics of using a Hybrid ACO algorithm with VRPTW, as discussed in [5] , the presented paper enhances the original approach by replacing heuristics with Machine Learning techniques.

## III. Vehicle Routing with Time Windows

The Vehicle Routing Problem with Time Windows (VRPTW) is an extension of the classic Vehicle Routing Problem (VRP), which seeks to optimize the routes of a fleet of vehicles delivering goods to a set of customers. In the VRPTW, each customer is associated with a specific time window during which the delivery must occur. The objective is to minimize the total travel cost while ensuring that each delivery is made within the specified time window and that vehicle capacities are not exceeded.

Formally, let 𝐺 = ( 𝑉, 𝐸 ) be a directed graph where 𝑉 = { 0 1 , , . . . , 𝑛 } is the set of vertices, and 𝐸 is the set of edges connecting these vertices. Vertex 0 represents the depot, and vertices 1 to 𝑛 represent the customers. Each customer 𝑖 ∈ 𝑉 \{ 0 } is associated with a demand 𝑑 𝑖 and a time window [ 𝑒 , 𝑙 𝑖 𝑖 ] , where 𝑒 𝑖 is the earliest start time and 𝑙 𝑖 is the latest start time for the service at customer 𝑖 . Each edge ( 𝑖, 𝑗 ) ∈ 𝐸 has an associated travel time 𝑡 𝑖 𝑗 and travel cost 𝑐 𝑖 𝑗 .

Theconstraints of the VRPTW can be summarized as follows:

- · Each route starts and ends at the depot.
- · Each customer is visited exactly once by one vehicle.
- · The total demand of the customers on each route does not exceed the vehicle's capacity.
- · The service at each customer starts within the specified time window [ 𝑒 , 𝑙 𝑖 𝑖 ] .
- · Vehicles are allowed to wait if they arrive before the earliest start time 𝑒 𝑖 , but they cannot arrive after the latest start time 𝑙 𝑖 .

The VRPTW is a well-studied problem in the field of combinatorial optimization and operations research due to its practical applications in logistics and distribution. Many exact and heuristic methods have been proposed to solve the VRPTW.Amongthese,heuristicapproachessuchasAntColony Optimization (ACO) have gained significant attention due to their ability to find good quality solutions within reasonable computational times [6], [7].

In this paper, we propose an enhanced heuristic-driven Ant Colony Optimization approach that incorporates domainspecific knowledge to improve the solution quality for the VRPTW. Our method is designed to balance exploration and exploitation more effectively, leveraging the structured nature of the problem to guide the search process.

The VRPTW has been addressed by various methods, including exact algorithms, such as branch-and-bound and branch-andcut, and metaheuristic approaches, such as Genetic Algorithms (GA), Tabu Search (TS), and Particle Swarm Optimization (PSO)[8]-[10]. However, the computational complexity of exact methods often limits their applicability to small or moderately sized instances. On the other hand, metaheuristic approaches can handle larger instances more efficiently but may not always guarantee optimal solutions. The ACO algorithm, introduced by Dorigo and Gambardella [6], has shown promise in providing high-quality solutions for VRPTW by simulating the foraging behavior of ants [7].

## IV. Ant Colony Optimization

Ant Colony Optimization (ACO) is a nature-inspired metaheuristic that has been successfully applied to various combinatorial optimization problems, including the Vehicle Routing Problem with Time Windows (VRPTW). The ACO algorithm is based on the foraging behavior of ants, specifically their use of pheromone trails to find the shortest paths to food sources. This section explores the foundational principles of ACO and reviews recent advancements in its application.

## A. Foundational Principles

The ACO algorithm was first introduced by Marco Dorigo in his PhD thesis and later elaborated in subsequent research papers. The algorithm simulates the behavior of ants as they traverse paths and deposit pheromones, which influence the likelihood of other ants following the same path. Over time, shorter paths accumulate more pheromones, guiding the colony towards optimal solutions. Key components of the ACO include:

- · Pheromone Update Rules : Pheromone levels are updated based on the quality of the solutions found. This involves both the deposition of new pheromones by successful paths and the evaporation of pheromones over time to avoid convergence to local optima.
- · Heuristic Information : A heuristic function helps guide the ants' decisions, balancing exploration and exploitation by incorporating problem-specific knowledge.
- · Stochastic Solution Construction : Each ant constructs a solution incrementally, probabilistically choosing the next step based on pheromone levels and heuristic information.

## B. Recent Advances

Recent research has enhanced the ACO algorithm's efficiency and applicability to complex problems like VRPTW. Innovations include hybrid approaches, improved pheromone update mechanisms, and specialized strategies for handling dynamic and stochastic environments.

- 1) Hybrid Approaches: Hybrid algorithms combine ACO with other optimization techniques to leverage their complementary strengths. For instance, recent studies have integrated ACO with machine learning methods to enhance feature selection processes, resulting in improved performance on highdimensional datasets [11]. Additionally, combining ACO with genetic algorithms has shown promising results in optimizing the arrangement of viscous dampers in steel frames [12].
- 2) Improved Pheromone Update Mechanisms: Advancements in pheromone update strategies have focused on reducing premature convergence and improving solution quality. For example, new update rules consider both local and global pheromone information, balancing exploration and exploitation more effectively. These mechanisms have been applied to various scheduling and routing problems, demonstrating significant improvements in solution robustness and efficiency [13].
- 3) Dynamic and Stochastic Environments: Addressing dynamic and stochastic environments remains a critical challenge for ACO. Recent research has explored adaptive ACO algorithms that adjust parameters in real-time based on environmental feedback. This approach has been particularly effective in applications such as real-time vehicle routing and adaptive network optimization [14].

## V. Proposed Algorithm

Our algorithm is an enhancement of the Ant Colony Optimization (ACO) approach used for vehicle routing with time windows, inspired by the method described in [5]. Unlike previous works, our method integrates a machine learningdriven heuristic for path selection and introduces adaptive parameters to improve convergence and robustness.

## A. Graph Representation

We represent the problem using a graph 𝐺 = ( 𝑉, 𝐸 ) where 𝑉 is the set of nodes (delivery points) and 𝐸 is the set of edges (possible paths).

- · Nodes include the depot and customer locations, each with specific time windows and demand.
- · Edges have associated distances or travel times.

## B. Parameters

- · 𝜏 𝑖 𝑗 : Initial pheromone level on edge ( 𝑖, 𝑗 ) .
- · 𝛼 : Pheromone influence parameter.
- · 𝛽 : Heuristic influence parameter.
- · 𝜌 : Pheromone evaporation rate.
- · 𝑄 : Constant for the amount of pheromone deposited.
- · 𝑚 : Number of ants (vehicles), maximum number of iterations max iter, and initial pheromone level 𝜏 0 .

## C. Heuristic Value Calculation

We incorporate a machine learning model to calculate the heuristic value 𝜂 𝑖 𝑗 for moving from node 𝑖 to node 𝑗 . This model considers distance, time windows, and vehicle capacity constraints. The heuristic value is computed as:

$$\eta _ { i j } = \text{MLModel} ( d _ { i j }, \Delta T _ { i j }, q _ { j } ) \text{ \quad \ \ } ( 1 )$$

where:

- · 𝑑 𝑖 𝑗 : Distance between nodes 𝑖 and 𝑗 .
- · Δ 𝑇 𝑖 𝑗 : Tightness of the time window between nodes 𝑖 and 𝑗 .
- · 𝑞 𝑗 : Demand at node 𝑗 .

## D. Probabilistic Path Selection

Each vehicle constructs a solution by choosing the next node 𝑗 with probability 𝑝 𝑖 𝑗 :

$$p _ { i j } = \frac { ( \tau _ { i j } ) ^ { \alpha } \cdot ( \eta _ { i j } ) ^ { \beta } } { \sum _ { l \in N _ { i } } ( \tau _ { i l } ) ^ { \alpha } \cdot ( \eta _ { i l } ) ^ { \beta } } \quad \quad ( 2 )$$

where 𝑁 𝑖 𝑖 .

is the set of feasible nodes for the vehicle from node

## E. Route Construction

Each vehicle starts from the depot and constructs a route by repeatedly applying the probabilistic path selection until all customers are visited within their time windows and vehicle capacity limits.

## Algorithm 1 Construct Solution

1: Input: Graph 𝐺 , Pheromone matrix 𝜏 , Heuristic informa- tion 𝜂 , Parameters 𝛼 𝛽 , , Number of vehicles 𝑚

2: Output: Solutions

3: Solutions ← [ ]

4: for each vehicle 𝑘 in 1 , . . . , 𝑚 do

5: Solution ← [Start node]

6: while not all nodes are visited do

7: FeasibleNodes ← GetFeasibleNodes(Solution, 𝐺 )

8: NextNode ← ChooseNextNode( 𝜏 , 𝜂 , 𝛼 , 𝛽 , Feasi- bleNodes)

9: Append NextNode to Solution

10: end while

11: Append Solution to Solutions

12: end for

13: return Solutions

## F. Local Update

After each vehicle completes its route, update the pheromone level on the visited edges as follows:

$$\tau _ { i j } ( t + 1 ) = ( 1 - \rho ) \cdot \tau _ { i j } ( t ) + \rho \cdot \Delta \tau _ { i j } ( t ) \quad \ \ ( 3 ) \quad \ \text{ } \quad \text{ }$$

where:

$$\Delta \tau _ { i j } ( t ) = \begin{cases} \frac { Q } { L _ { k } } & \text{if edge (i, j) is visited by vehicle $k$} \\ 0 & \text{otherwise} \end{cases} \quad ( 4 ) & \text{OR-Too} \\ \text{Into} \, \text{IR} \, \end{cases}$$

Here, 𝐿 𝑘 is the route length of vehicle 𝑘 , and 𝜌 is the evaporation rate of pheromones.

## G. Global Update

At the end of each iteration, update the pheromone levels globally based on the best solution found so far:

$$\tau _ { i j } ( t + 1 ) = ( 1 - \rho ) \cdot \tau _ { i j } ( t ) + \Delta \tau _ { i j } ^ { b e s t } \quad \ \ ( 5 ) \ \ A. \ M \$$

where:

$$\Delta \tau _ { i j } ^ { b e s t } = \begin{cases} \frac { Q } { L _ { b e s t } } & \text{ if edge $(i,j)$ is part of the best} \\ 0 & \text{ otherwise} \end{cases} \text{ \quad \ \ $(6)$} \text{ \quad Gradien} \\ 1 ) \ D.$$



Here, 𝐿 𝑏𝑒𝑠𝑡 is the route length of the best solution found so far.

## H. Novel Aspects and Enhancements

- 1) Adaptive Parameters: We adaptively adjust 𝜌 and other parameters based on the progress of the algorithm to balance exploration and exploitation, enhancing convergence and solution quality.
- 2) Hybrid Approaches: This work combines ACO with elements of genetic algorithms and particle swarm optimization, utilizing multiple optimization strategies for robust results.
- 3) Pheromone Initialization: Using historical traffic data to initialize pheromone levels accelerates convergence by incorporating problem-specific knowledge.

Fig. 1. VRP Graph (credits: Google OR-Tools)

![Image](image_000000_4bba014437568c7a5dfe03722292f11a2fe7823e7fe81c45023f11d925e5626c.png)

4) Dynamic Constraints Handling: The algorithm includes dynamic adjustment mechanisms for real-world factors such as traffic or delivery changes, maintaining robust solutions under varying conditions.

## VI. Experiments and Results

The proposed Machine Learning-Driven Ant Colony Optimization for VRPTW (ML-ACO-VRPTW)algorithmwastested in Python. Due to the unavailability of specialized testing datasets, we utilized Time and Distance matrices from Google OR-Tools. All experiments were conducted on a machine with Intel(R) Core(TM) i5-7300U CPU clocked at 2.60 GHz and 8 GB RAM.

Google OR-Tools provides a platform with fast and portable software for combinatorial optimization. The dataset includes time matrices of travel times between locations and distance matrices. Figure 1 provides an insight of how the dataset is replicated from Google OR-Tools.

## A. Model Selection

Weevaluated various regression models for predicting heuristic values in the Ant Colony Optimization (ACO) algorithm, focusing on the Decision Tree Regressor, Random Forest, Gradient Boosting, XGBoost, and a Sequential Artificial Neural Network (ANN).

1) DECISION TREE REGRESSOR: The Decision Tree Regressor is interpretable and captures non-linear relationships through recursive feature space partitioning.

- · Advantages: Clear decision rules; handles both numerical and categorical data with minimal preprocessing.
- · Disadvantages: Prone to overfitting, especially with deep trees, requiring pruning for generalization.
- 2) RANDOM FOREST REGRESSOR: To counter overfitting, we used the Random Forest Regressor, which combines multiple trees for better accuracy and robustness.
- · Advantages: Stabilizes predictions by averaging trees, reduces overfitting, and provides feature importance scores.
- · Disadvantages: More computationally intensive with the need for hyperparameter tuning.

- 3) GRADIENT BOOSTING REGRESSOR: The Gradient Boosting Regressor builds sequentially to correct previous errors, enhancing predictive accuracy.
- · Advantages: Focuses on difficult cases for higher accuracy.
- · Disadvantages: Sensitivity to hyperparameters, with generally longer training times than Random Forests.
- 4) XGBOOST: XGBoost, an optimized gradient boosting implementation, offers regularization and computational efficiency improvements.
- · Advantages: High accuracy and speed, especially suited for large datasets.
- · Disadvantages: Requires careful tuning, similar to Gradient Boosting.

5) SEQUENTIAL ARTIFICIAL NEURAL NETWORK (ANN): The Sequential ANN was included for its capability to capture complex data patterns.

- · Advantages: Strong predictive power, particularly for large datasets with non-linear relationships.
- · Disadvantages: Longer training times and higher computational demands compared to other regressors.

6) Performance Comparison: Weassessed the models based on RMSE, R-squared (R ), MAE, and training time. As shown ² in Table I, while the Decision Tree provided a baseline, Random Forest and XGBoost achieved superior accuracy and generalization. The Sequential ANN excelled in accuracy but was less suitable for real-time applications due to training time.

TABLE I Performance Comparison of Regression Models

| Model             |   RMSE |    R ² |    MAE |   Training Time (s) |
|-------------------|--------|--------|--------|---------------------|
| Decision Tree     | 0.0333 | 0.9114 | 0.0151 |                4.52 |
| Random Forest     | 0.0261 | 0.9947 | 0.0105 |               26.8  |
| Gradient Boosting | 0.2279 | 0.6032 | 0.138  |                4.52 |
| XGBoost           | 0.0217 | 0.9963 | 0.0071 |               22.91 |
| Sequential ANN    | 0.0043 | 0.9999 | 0.0031 |               85.39 |

## B. Small Datasets

For smaller datasets, traditional heuristics, such as the Nearest Neighbor (NN) algorithm, often perform well because of their simplicity and low computational overhead. NN quickly generates a feasible solution by selecting the nearest unvisited customer, which can be effective for small-scale problems where time window constraints are less critical.

## C. Real-World Scenario

The ML-ACO-VRPTWalgorithm excels in complex logistics operations, leveraging machine learning-driven heuristics and adaptive pheromone updates to effectively manage time window urgency and vehicle capacity constraints, which are challenging for the NN heuristic.

We tested the ML-ACO-VRPTW algorithm on realistic logistics datasets with multiple time windows and varying vehicle capacities. The results demonstrated significant cost reductions in travel distances and time-window penalties compared to the NN heuristic.

Key advantages of ML-ACO-VRPTW in real-world scenarios include:

- · Improved Solution Quality : The algorithm consistently identified the optimal routes while effectively managing time windows and vehicle capacities.
- · Enhanced Scalability : Its capability to handle larger problem instances and complex constraints makes it suitable for logistics and cargo shipping applications.
- · Adaptability : The dynamic pheromone updates, combined with machine learning heuristics, allow the algorithm to adjust more effectively to changing conditions than static heuristics.

## D. Parameter Sensitivity Analysis

We conducted an extensive sensitivity analysis of the MLACO-VRPTW algorithm to understand parameter impacts on performance and stability. The analysis framework, implemented in Python, systematically evaluated six key parameters across multiple runs.

Fig. 2. Fully Connected Distance Matrix

![Image](image_000001_9925119fe211693d67fc4b97bf8ff96f5f396e6373fc2c631ea1f2ea15c30013.png)

Our analysis framework employed the following methodology:

- · Multiple runs ( 𝑛 = 3) for each parameter configuration to ensure statistical significance
- · Computation of mean and standard deviation for both solution quality and execution time
- · Sensitivity scores calculation based on percentage deviation from optimal solutions
- · Error-bar plots generation for visual analysis of parameter effects

The results revealed varying levels of parameter sensitivity. The initial pheromone level ( 𝜏 0 ) and evaporation rate ( 𝜌 ) showed the highest sensitivity (1.27%), indicating their critical role in algorithm performance. The pheromone influence ( 𝛼 ) and heuristic influence ( 𝛽 ) parameters demonstrated lower sensitivity (0.42%), suggesting greater robustness to these parameter

TABLE II Parameter Ranges for Sensitivity Analysis

| Parameter   | Description                | Test Range           |
|-------------|----------------------------|----------------------|
| 𝜏 0         | Initial pheromone level    | [0.1, 0.5, 1.0, 2.0] |
| 𝛼           | Pheromone influence        | [0.5, 1.0, 1.5, 2.0] |
| 𝛽           | Heuristic influence        | [1.0, 2.0, 3.0, 4.0] |
| 𝜌           | Evaporation rate           | [0.1, 0.2, 0.3, 0.4] |
| 𝑄           | Pheromone deposit constant | [50, 100, 150, 200]  |
| 𝑚           | Number of ants             | [5, 10, 15, 20]      |

changes. The pheromone deposit constant ( 𝑄 ) and number of ants ( 𝑚 ) showed moderate sensitivity (0.84%).

TABLE III Sensitivity Analysis Results

| Parameter   |   Best Value |   Worst Value |   Sensitivity (%) |
|-------------|--------------|---------------|-------------------|
| 𝜏 0         |          0.1 |           0.5 |              1.27 |
| 𝛼           |          0.5 |           1   |              0.42 |
| 𝛽           |          1   |           4   |              0.42 |
| 𝜌           |          0.1 |           0.3 |              1.27 |
| 𝑄           |         50   |         150   |              0.84 |
| 𝑚           |         15   |          10   |              0.84 |

Sensitivity was quantified to capture how changes in parameter values impact solution quality. We calculated the sensitivity of each parameter using the formula:

$$\text{sensitivity} = \frac { \text{worst value} \, \text{-- best value} } { \text{best value} } \times 100 \quad ( 7 ) \quad \text{$^{th}_{\tau}$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$} \quad \text{$\tau$}}$$

The sensitivity percentage highlights the extent to which variations in each parameter affect the solution quality. A high sensitivity value suggests that the parameter has a substantial impact on the algorithm's performance, while a lower value indicates robustness to changes in that parameter. This analysis allowed us to identify which parameters most significantly influence the performance of the ML-ACO-VRPTW algorithm.

## VII. Conclusions

This paper presents a novel Machine Learning-Driven Ant Colony Optimization approach for Vehicle Routing with Time Windows (ML-ACO-VRPTW). The proposed algorithm integrates Ant Colony Optimization (ACO) with machine learningenhanced heuristic values to address the complexities of vehicle routing problems involving time windows.

## A. Key Findings

- · Algorithm Efficiency: The ML-ACO-VRPTW algorithm outperforms traditional heuristics like Nearest Neighbor, particularly on larger datasets. It optimizes routes effectively by managing time windows and vehicle capacities.
- · Enhanced Solution Quality: By combining ML-driven heuristics and adaptive pheromone updates, the algorithm achieves higher-quality solutions, reducing travel distances, time window violations, and improving capacity management.
- · Scalability and Applicability: ML-ACO-VRPTW scales well for large, complex problems, making it viable for logistics and other time-sensitive routing tasks. Its adaptability ensures robust performance across varied scenarios.

## B. Implications

The ML-ACO-VRPTW algorithm's success in vehicle routing highlights its utility for logistics. By integrating MLenhanced heuristics and dynamic pheromone updates, it offers a sophisticated solution for complex routing tasks beyond traditional heuristics.

Future work will focus on enhancing efficiency and adaptability through:

- · Parameter Optimization: Refining parameters for improved robustness and performance.
- · Hybrid Approaches: Combining ACO with other techniques to boost solution quality and computational efficiency.
- · Real-World Testing: Applying the algorithm to diverse datasets to validate its practical effectiveness.
- · Advanced Techniques: Incorporating LLMs for an integrated ML-ACO-VRPTW route optimization as explored in [15].

## References

- [1] G. B. Dantzig and J. H. Ramser, 'The truck dispatching problem,' Management Science , vol. 6, no. 1, pp. 80-91, 1959.
- [2] F. D. Keskin, Multi-Criteria Decision Making With Machine Learning for Vehicle Routing Problem . City, Country: Publisher Name, 2023.
- [3] L. D. P. Pugliese, D. Ferone, P. Festa, F. Guerriero, and G. Macrina, 'Combining variable neighborhood search and machine learning to solve the vehicle routing problem with crowd-shipping,' Optimization Letters , vol. 17, pp. 1981-2003, 2023.
- [4] Y. Sun, S. Wang, Y. Shen, X. Li, A. T. Ernst, and M. Kirley, 'Boosting ant colony optimization via solution prediction and machine learning,' Computers and Operations Research , vol. 138, p. 105769, 2022.
- [5] H. Wu, Y. Gao, and W. e. a. Wang, 'A hybrid ant colony algorithm based on multiple strategies for the vehicle routing problem with time windows,' Complex Intell. Syst. , vol. 9, pp. 2491-2508, 2023.
- [6] M. Dorigo and L. M. Gambardella, 'Ant colonies for the travelling salesman problem,' Biosystems , vol. 43, no. 2, pp. 73-81, 1997.
- [7] L. M. Gambardella, E. Taillard, and G. Agazzi, Ant Colonies for Vehicle Routing Problems . London, UK: McGraw-Hill, 1999, pp. 63-76.
- [8] M. M. Solomon, 'Algorithms for the vehicle routing and scheduling problems with time window constraints,' Operations Research , vol. 35, no. 2, pp. 254-265, 1987.
- [9] J.- Y . Potvin and S. Bengio, 'The vehicle routing problem with time windows part ii: Genetic search,' INFORMS Journal on Computing , vol. 8, no. 2, pp. 165-172, 1996.
- [10] K. C. Tan, L. H. Lee, Q.-L. Zhu, and K. Ou, 'Heuristic methods for vehicle routing problem with time windows,' Artificial Intelligence in Engineering , vol. 23, no. 1, pp. 309-315, 2005.
- [11] T. Dokeroglu, A. Deniz, and H. Kiziloz, 'A comprehensive survey on recent metaheuristics for feature selection,' Neurocomputing , vol. 494, pp. 269-296, 2022.
- [12] R. Azam, M. Riaz, M. Farooq, F. Ali, M. Mohsan, and A. D. et al., 'Optimization-based economical flexural design of singly reinforced concrete beams: a parametric study,' Materials (Basel) , vol. 15, p. 3223, 2022.
- [13] A. Dalvand, M. Dowlatshahi, and A. Hashemi, 'Sgfs: a semi-supervised graph-based feature selection algorithm based on the pagerank algorithm,' in 27th International Computer Conference, Computer Society of Iran (CSICC) , 2022, pp. 1-6.
- [14] G. Bekdas, S. Nigdeli, A. Kayabekir, and X.-S. Yang, Optimization in civil engineering and metaheuristic algorithms: a review of state-of-the-art developments . Springer, 2022, pp. 111-137.
- [15] C. C. Sartori, C. Blum, F. Bistaffa, and G. R. Corominas, 'Metaheuristics and large language models join forces: Towards an integrated optimization approach,' arXiv preprint , vol. 2405.18272, 2024, submitted for publication in an international journal. [Online]. Available: https://doi.org/10.48550/arXiv.2405.18272