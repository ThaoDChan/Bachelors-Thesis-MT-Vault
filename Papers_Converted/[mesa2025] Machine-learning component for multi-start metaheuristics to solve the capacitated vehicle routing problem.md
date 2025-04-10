![Image](image_000000_ba81d049bf84f684346f1236351e1a7f5c16fe331ba28cb4dbba9d2ee1aef825.png)

Contents lists available at ScienceDirect

## Applied Soft Computing

journal homepage: www.elsevier.com/locate/asoc

## Machine-learning component for multi-start metaheuristics to solve the capacitated vehicle routing problem

Juan Pablo Mesa a , ∗ , Alejandro Montoya a,1 , Raul Ramos-Pollan b,1 , Mauricio Toro a,1

- a School of Applied Sciences and Engineering, Universidad EAFIT, Carrera 49 N 7 Sur-50, Medellin, 05001, Antioquia, Colombia
- b Facultad de Ingenieria, Universidad de Antioquia, Calle 67 N 53-108, Medellin, 05001, Antioquia, Colombia

## A R T I C L E I N F O

## A B S T R A C T

Keywords: Metaheuristics Vehicle routing problem Machine learning Classification Feature extraction

GRASP

Multi-Start metaheuristics (MSM) are commonly used to solve vehicle routing problems (VRPs). These methods create different initial solutions and improve them through local-search. The goal of these methods is to deliver the best solution found. We introduce initial-solution classification (ISC) to predict if a local-search algorithm should be applied to initial solutions in MSM. This leads to a faster convergence of MSM and higher-quality solutions when computation time is limited. In this work, we extract features of a capacitated VRP (CVRP) solution, by transforming the structure of a solution into quantitative metrics (i.e.number of customers in each route, average compactness of a route, or number of intersections between routes). With these features and a machine-learning classifier (random forest), we show how ISC - significantly - improves the performance of greedy randomized adaptive search procedure (GRASP), over benchmark instances from the CVRP literature. With the objective of evaluating ISC's performance with different local-search algorithms, we implemented a local-search composed of classical neighborhoods from the literature and another local-search with only a variation of Ruin-and-Recreate. In both cases, ISC significantly improves the quality of the solutions found in almost all the evaluated instances.

## 1. Introduction

In  vehicle  routing  problems  (VRPs),  recent  research  [1,2]  has demonstrated that structural properties of VRP solutions can be extracted to obtain valuable information. These features have been used differently  in  heuristics  for  VRP:  from  developing  state-of-the-art metaheuristics - such as the Knowledge Guided local-search (KGLS) of Arnold and Sörensen [3] - to discovering patterns in high-quality solutions [4]. In this paper, we investigate how these features can be used to improve the performance of multi-start metaheuristics (MSM) for VRP. We focus, in particular, on the capacitated VRP (CVRP).

a reasonable computing time [6]. Therefore, metaheuristics are used to solve large instances of the CVRP because they can deliver high-quality solutions: High-quality solutions are solutions with a low total-route cost, computing them in a reasonable amount of time, at the expense of not guaranteeing optimality.

The CVRP is a variant of VRP and has been studied for more than six decades (since it was first proposed by Dantzig and Ramser [5]), and it is still a relevant problem. The objective of the CVRP is to find a set of routes that minimizes the total cost traveled by a fleet of vehicles, which serves a set of customers, starting and ending their routes at a single depot. The capacity of the vehicles is limited, homogeneous, and cannot be exceeded. Each customer has a known demand, must be visited once, and the distance among customers is Euclidean.

The CVRP is an NP-Hard problem, which limits the ability of stateof-the-art exact algorithms to optimally solve instances of the CVRP in

In recent years, there has been a trend to solve the CVRP directly with Artificial Intelligence (AI) such as the works of Nazari et al. [7] and Kool et al. [8], or combining metaheuristics with machine learning (ML) or deep learning [9,10]. This is possible because a large amount of data can be generated during the execution of different heuristics while solving the problem. This data can be used in ML algorithms. ML may be employed with different objectives within the metaheuristics, such as: (1) to improve the quality of the solutions, (2) to guide local-search, and (3) to reduce the computing time.

Of the different types of metaheuristics used to solve the CVRP, MSM have the potential to integrate ML into them easily. MSM typically operate in the  following  manner.  First,  an  initial  solution  to  the  problem is constructed. Second, this initial solution is improved by means of local-search. Third, once local-search cannot further improve the solution, the process starts again. Fourth, this process is repeated until some

∗ Corresponding author.

E-mail addresses: jmesalo@eafit.edu.co (J.P. Mesa), jmonto36@eafit.edu.co (A. Montoya), raul.ramos@udea.edu.co (R. Ramos-Pollan), mtorobe@eafit.edu.co (M. Toro).

1 These authors contributed equally to this work.

## https://doi.org/10.1016/j.asoc.2025.112916

Received 22 April 2024; Received in revised form 13 February 2025; Accepted 18 February 2025 Available online 28 February 2025

1568-4946/© 2025 Elsevier B.V. All rights are reserved, including those for text and data mining, AI training, and similar technologies.

![Image](image_000001_4dbe5f812ddbb747dc47640b5473a2efc9deba0ca9a282083c9166498ed92e4b.png)

![Image](image_000002_4dffe1f0ee225d63db2028c3e348c6f0db7df32396609e8a85b5753278ea5be2.png)

stopping criterion is met. Finally, the best solution found is returned as the final solution of the MSM.

Generally,  constructing  initial  solutions  is  not  a  procedure  that consumes a considerable amount of time compared to local-search, which consumes most of the computing time when solving the CVRP. Usually, in MSM, every time one constructs an initial solution, localsearch is always applied to such initial solution. But what if one can analyze these initial solutions and predict how much local-search will improve them? This would better utilize computing time for improving solutions in promising regions of the solution space. This research path, which aims to reduce the solution search-space on heuristics for VRP, has been recently explored by Lucas et al. [2], but it remains open as a promising direction in the design of heuristics for VRP and its variants. One can expect that incorporating an ML component in a metaheuristic leads to better performance in solving the CVRP. However, the results in the literature show that the problem remains open because no results were achieved where the metaheuristic combined with ML was better than the metaheuristic alone.

To fill this research gap, we introduce a method that enables the classification of initial solutions, within MSM, that would lead to highquality solutions after local-search is applied to such initial solutions. We name this method initial-solution classification (ISC). ISC efficiently predicts if applying local-search to an initial solution will lead to a high-quality solution.

ISC consists of two phases that are widely used in ML: (1) training and (2) classification. The training phase extracts features - for instance, average route length and average route span - from initial solutions, applies local-search to the initial solutions, and trains an ML classifier to choose whether an initial solution will be a high-quality solution or not after applying local-search to such solution. After the classifier is trained for a given time, the training phase is no longer executed, and now the classification phase is performed. In this phase, new initial solutions are created, and the same features are extracted to predict if local-search should be applied or if it is not worth employing computational effort on them. This decision corresponds to whether the ML classifier predicts a high-quality solution or the ML classifier predicts a low-quality solution.

In this work, as a case study, we apply ISC to Greedy Randomized Adaptive Search (GRASP) [11]; however, ISC can be easily replicated in  other  MSM  to  predict  promising  initial  solutions  using  different local-search  algorithms.  In  particular,  we  apply  ISC  to  two  existing local-search algorithms: (1) variable neighborhood descent (VND) of Mladenovic and Hansen [12], and (2) a variation of Ruin-and-Recreate (R&amp;R) [13]. We use random forest as a machine-learning classifier [14]. Nonetheless, this can also be easily replicated to other ML algorithms. The contributions of our work are the following:

- · A new method, ISC, that learns to predict which initial solutions will lead to high-quality solutions after local-search. To the best of our knowledge, no studies evaluate initial solutions for MSM.
- · Finally, we carry out experiments to measure the performance of the integration of ISC to GRASP metaheuristics with two different local-search algorithms.
- · As ISC requires features to be used by an ML classifier, we employ both  the  quantitative  features  to  represent  the  structure  of  a CVRP solution proposed by Arnold and Sörensen [1] and the ones introduced by Lucas et al. [2]. Additionally, we introduce several new features.

The objective of this work is not to propose a method that competes with the state-of-the-art metaheuristics for the CVRP; instead, we introduce a method that enables the improvement of existing MSM for the CVRP.

The rest of this paper is structured as follows. In Section 2,  we discuss the state-of-the-art algorithms for the CVRP and other works related to ML methods applied to the CVRP. In Section 3, we present and explain the ISC method. In Section 4, we present the experimental results and analysis of the performance of the integration of ISC to GRASP to solve the CVRP. Finally, Section 5 concludes our work and presents future-work directions.

## 2. Literature review

We divide this section into two parts. First, we give a brief explanation of the state-of-the-art metaheuristics for the CVRP. We then review the methods that combine the CVRP metaheuristics with ML and highlight the differences with our work. The methods that use only ML to solve routing problems - such as [7,8,15-17] - are out of the scope of our research. Additionally, other methods that use ML to learn to recommend an algorithm or metaheuristic out of a portfolio of options - such as the works from Gutierrez et al. [18] or Jiang et al. [19] - are also out of the scope of our research.

## 2.1. Metaheuristics state-of-the-art for the CVRP

We identified six metaheuristics that are considered state-of-the-art for the CVRP. The hybrid Iterated local-search with Set Partitioning from Subramanian et al. [20] is the only MSM that is advantageous with  respect  to  previous  MSM  for  VRP  variants.  Likewise,  Maximo and Nascimento [21] introduced an hybrid iterated local-search metaheuristic,  that  uses  path-relinking  strategies  and  an  adaptive  diversity control mechanism to escape local-optima during the search. The Unified Hybrid Genetic Search from Vidal et al. [22] is a populationbased adaptive genetic algorithm, with advanced diversity management methods, that give it an edge with respect to previous genetic algorithms for several VRP variants. The Slack Induction by String Removal (SISR)  [23],  introduced  by  Christiaens  and  Vanden  Berghe  [23],  is a  sophisticated,  yet  easily  reproducible,  ruin-and-recreate-based  approach combined with a simulated-annealing metaheuristic [24]. SISR uses simple removal and insertion heuristics to achieve high-quality solutions.

Recently, Accorsi and Vigo [13] presented Fast Iterated-Local-Search Localized Optimization (FILO), a fast and scalable metaheuristic, specialized  for  the  CVRP,  based  on  the  iterated  local-search  paradigm. FILO uses several acceleration and pruning techniques, coupled with strategies to keep the optimization localized in promising regions of the search space, and it is able to deliver high-quality solutions faster than all the other methods presented here. Finally, the KnowledgeGuided local-search (KGLS), proposed by Arnold and Sörensen [3], is a Guided Local-Search is enhanced by knowledge extracted from previous data-mining.

## 2.2. Methods that combine CVRP metaheuristics with ML

The knowledge extracted from data analyses for KGLS comes from the  previous  work  by  Arnold  and  Sörensen,  ''What  makes  a  good VRP solution?'' [1]. Specifically, the authors found that solutions with longer edges and wider, less compact routes have lower quality than solutions with thinner routes and shorter edges between customers. This knowledge is used to construct the guiding strategy in Guided Local-Search. The results and efficiency of KGLS demonstrate that the use  of  local-search  that  have  proven  to  work  well,  and  the  use  of problem  knowledge  enables  the  creation  of  efficient  heuristics  that work well for the CVRP.

More  recently,  the  work  of  Arnold  et  al.  [4]  proposed  the  use of pattern mining as an online strategy within vehicle-routing metaheuristics to aid local-search. The pattern injection local-search (PILS) investigated whether pattern mining - the discovery of frequently used patterns or structures in high-quality solutions - can similarly improve the performance of heuristic optimization. PILS consists of using two algorithmic steps: (1) pattern collection and (2) pattern injection. Pattern collection is the step where patterns (i.e., sequences of consecutive visits)  that  frequently  occur  in  high-quality  solutions  are  collected.

Pattern injection then takes a subset from the collected patterns and tries to introduce them in a solution, whose edges adjacent to nodes in the pattern are previously removed, to facilitate the injection.

This pattern mining approach is similar to an approach proposed by Maia et al. [25]. The authors used a hybrid version of an MSM that incorporates a data mining phase for the heterogeneous-fleet-VRP. Frequent patterns are collected from a set of high-quality solutions, these patterns are used as a starting point for the construction of initial solutions. This results in better-quality initial solutions. Consequently, the authors found that this allowed to reach better-quality solutions after local-search.

A different approach consists in using reinforcement-learning [26] to  learn  how  to  execute  local-search  to  improve  heuristics  on  initial solutions. Wu et al. [27] proposed a deep reinforcement-learning framework, based on self-attention, to learn the improvement heuristics based on the 2-opt operator for the Traveling Salesman Problem and the CVRP.

On  the  same  line,  the  work  of  Gao  et  al.  [28]  used  a  graphattention network to learn (1) the destroy operator, removing patterns of customers from a solution, and (2) the repair operator, choosing the insertion order of customers. Gao et al.'s network was applied within a Very Large-scale Neighborhood Search metaheuristic for the CVRP and the CVRP with time windows.

Similarly, Hottung and Tierney [29] proposed a neural large neighborhood search for the split delivery vehicle routing problem and the CVRP, where the task of the repair operator is learned by a deep neural network that is trained via policy-gradient reinforcement-learning [30]. Contrary to using hand-crafted improvement heuristics, all these approaches aim at automating the generation of heuristics in a way that requires no optimization knowledge.

As mentioned earlier, the work of Arnold and Sörensen [1] enabled the extraction of features from VRP solutions to use them in ML models. These features were exploited by Lucas et al. [2]. In the latter work, some additional features were introduced, and a new metaheuristic, called Feature-Guided Multiple Neighborhood Search (FG-MNS), was proposed.  In  FG-MNS,  solutions  are  represented  as  features,  and  a decision tree [31] defines promising areas for local-search exploration according to the features extracted from the solution.

FG-MNS is divided into two phases: (1) a learning phase and (1) an exploitation phase. In the learning phase, solutions are generated and improved with a multiple neighborhood search (MNS). Once there are enough solutions, a decision tree is trained. Finally, the exploitation phase uses the information from the decision tree to analyze a solution and alternate between applying the MNS or guiding the solution from a non-promising area to a promising one. Unfortunately, when MNS and FG-MNS are executed for the same fixed amount of time, MNS gives better results than FG-MNS.

Even if MNS delivers better results than FG-MNS, the method proposed  by  Lucas  et  al.  [2]  is  a  promising  research  direction  in  the intersection of metaheuristics and ML. In this work, we chose to follow up  Arnold  &amp;  Sörensen  [3]  and  Lucas  et  al.'s  [2]  direction,  while focusing on improving MSM for the CVRP. Our research is different from the two works mentioned above in that we focus on analyzing initial solutions and predicting their improvement potential instead of directly modifying the local-search algorithms. Unlike Arnold et al. [4] and Maia et al. [25], we extract features from VRP solutions and use them within ML algorithms instead of extracting recurring patterns in high-quality solutions and inserting them into new solutions.

## 3. Method

In this section, we describe ISC and how to integrate it within MSM. In the following sections, we make use of the notation in Table 1 for easy reference throughout the algorithms.

Table 1 Notation.

| 𝑁       | Number of customers of an instance               |
|---------|--------------------------------------------------|
| 𝛤       | GRASP time limit                                 |
| 𝜃       | Training phase time limit                        |
| 𝛹       | Classification phase time limit                  |
| 𝜏       | Current execution time                           |
| 𝐿𝑆      | Local-Search algorithm                           |
| 𝑆       | Solution                                         |
| 𝑆 𝑜     | Initial solution                                 |
| 𝑆 ′     | Improved solution                                |
| ̂ 𝑆     | Ruined solution                                  |
| 𝑆 ∗     | Best solution                                    |
| 𝐵       | Binary classifier                                |
| 𝐷 𝑠𝑜𝑙   | Solutions dataset                                |
| 𝐷 𝑡𝑟𝑎𝑖𝑛 | Training dataset                                 |
| 𝐹       | Features of a solution                           |
| 𝐿       | Class label                                      |
| 𝑃       | Promising solution                               |
| 𝑈       | Unpromising solution                             |
| 𝑐𝑣      | Improved cost value                              |
| 𝑇       | Temperature                                      |
| 𝜔       | Shaking parameter                                |
| 𝛥 𝑓     | Number of core optimization iterations of FILO0. |
| 𝐻       | Neighborhood                                     |

3.1. Initial solutions classification (ISC)

MSM normally follow a similar pattern as the traditional GRASP proposed  by  Feo  and  Resende  [11].  First,  they  construct  an  initial solution 𝑆𝑜 , usually feasible, but far from being a high-quality solution. After, they try to obtain an improved solution 𝑆 ′ by applying localsearch to 𝑆𝑜 . When a local-search stopping criteria is met, local-search stops. Next, the best solution 𝑆 ∗ found overall is updated if the cost of 𝑆 ′ is lower. This process is repeated until the stopping criterion of the metaheuristic is met: Current execution time 𝜏 exceeds the time limit 𝛤 .  Finally,  the  best  solution  found 𝑆 ∗ is  returned as the final solution of the metaheuristic. This process corresponds to GRASP described in Algorithm 1. On MSM of this kind, for different initial solutions, the same local-search algorithm might lead to different improved solutions. Some of them will be high-quality solutions and others will be low-quality solutions, within the limits and capabilities of the metaheuristic.

Algorithm 1: Greedy Randomized Adaptive Search Procedure (GRASP). Adapted from [11].

```
Input: 𝛤 , 𝛼 1 𝑐𝑜𝑠𝑡 𝑏𝑒𝑠𝑡 _ ← ∞ , 𝜏 ← 0 2 while 𝜏 < 𝛤 do 3 𝑆𝑜 ← GreedyRandomizedConstruction( 𝛼 ) 4 𝑆 ′ ← LS( 𝑆𝑜 ) 5 if 𝑐𝑜𝑠𝑡 𝑆 ( ′ ) < 𝑐𝑜𝑠𝑡 𝑏𝑒𝑠𝑡 _ then 6 𝑐𝑜𝑠𝑡 𝑏𝑒𝑠𝑡 _ ← 𝑐𝑜𝑠𝑡 𝑆 ( ′ ) 7 𝑆 ∗ ← 𝑆 ′ 8 𝜏 ← 𝜏 +1 9 return 𝑆 ∗
```

The structural differences in the initial solutions can be meaningful, and this is what ISC aims to exploit. Instead of applying a local-search to all initial solutions, ISC improves MSM by analyzing the features 𝐹 of each initial solution 𝑆𝑜 , and making predictions using ML. If ISC predicts that the initial solution 𝑆𝑜 and the local-search algorithm 𝐿𝑆 will lead to a good-quality solution, ISC classifies 𝑆𝑜 as a promising solution 𝑃 , or as an unpromising solution 𝑈 otherwise. ISC then applies local-search if  the  initial  solution 𝑆𝑜 is  predicted  as  a  promising  solution 𝑃 . This allows to employ the saved computing time to potentially find better solutions faster.

Arnold and Sörensen [1] created features that represent a VRP solution, such as the average width of routes, or the number of inter-route crosses. Some additional features were introduced by Lucas et al. [2].

ISC uses most of the features from these two works, and we added some new features that help improve the classifier's accuracy. This paper defines the features (metrics) for a CVRP solution using the notation introduced in [1]. The set of all routes in the CVRP solution is denoted by 𝑅 . A route 𝑟 is defined as a set of customers 𝑛 𝑟 𝑖 visited in a specific order, starting and ending at the depot 𝐷 . Thus, a route 𝑟 isrepresented by 𝑟 = { 𝑛 𝑟 1 , 𝑛 𝑟 2 , … , 𝑛 𝑟 | 𝑟 | } ∈ 𝑅 . The Euclidean distance between two nodes 𝑛 1 and 𝑛 2 is denoted by 𝑑 ( 𝑛 , 𝑛 1 2 ) ,  and the 𝑥 and 𝑦 coordinates of a node 𝑛 1 are represented by 𝑥 ( 𝑛 1 ) and 𝑦 ( 𝑛 1 ) , respectively. The x-coordinate of  the  center  of  gravity 𝐺𝑟 of  a  route 𝑟 is  defined  as 𝑥 ( 𝐺𝑟 ) = ∑ ( 𝑥 𝑛 𝑟 𝑖 ) + ( 𝑥 𝐷 ) | 𝑟 | +1 , and the y-coordinate is computed as 𝑦 ( 𝐺𝑟 ) = ∑ ( 𝑦 𝑛 𝑟 𝑖 ) + ( 𝑦 𝐷 ) | 𝑟 | +1 . The distance between a customer 𝑛 1 and the line through 𝐷 and 𝐺𝑟 is denoted by 𝑑 ( 𝐿𝐺𝑟 , 𝑛 1 ) ,  with a positive distance indicating that the customer is on the right side of the line and negative otherwise. The difference in  radians  between  two  nodes 𝑛 1 and 𝑛 2 with respect to the depot 𝐷 is denoted by rad ( 𝑛 , 𝑛 1 2 ) , which is the angle spanned between the lines connecting each node with the depot. The number of intersections between routes 𝑟 1 and 𝑟 2 is denoted by 𝐼 ( 𝑟 1 , 𝑟 2 ) .  Finally,  each customer 𝑖 has a demand of 𝑞 𝑖 to be served by a vehicle. The features 𝐹 used in ISC to characterize and evaluate a CVRP solution are defined as follows. The features taken from [1] are marked with a [1], the features taken from [2] are marked with a [2], and the researchers propose the features without symbols.

Number of intersections per customer: The  number  of  intersections per customer is calculated using the following formula:

<!-- formula-not-decoded -->

From this, we derive the following features:

- (1) Average number of intersections per customer [1];
- (2) The standard deviation of intersections per customer.

Longest distance between two connected customers per route: This feature is calculated using the formula:

<!-- formula-not-decoded -->

The features derived from this calculation are:

- (3) Average longest distance [1];
- (4) The standard deviation of the longest distance;
- (5) The minimum value among the longest distances;
- (6) The maximum value among the longest distances.

## Shortest distance between two connected customers per route:

This is calculated using:

<!-- formula-not-decoded -->

The features derived are:

- (7) Average shortest distance;
- (8) The standard deviation of the shortest distance;
- (9) The minimum value among the shortest distances;
- (10) The maximum value among the shortest distances.

Distance between depot to directly-connected customers: This is calculated as:

<!-- formula-not-decoded -->

The features derived are:

- (11) Average distance [1];
- (12) The shortest distance;
- (13) The longest distance.

Average distance between routes: This is calculated using the formula:

<!-- formula-not-decoded -->

The feature derived is:

- (14) Average distance between routes (their centers of gravity) [1].

Width per route: The width per route is calculated as:

<!-- formula-not-decoded -->

The features derived are:

- (15) The average width [1];
- (16) The standard deviation of the width of each route [2];
- (17) The minimum value among the widths;
- (18) The maximum value among the widths.

Span in radians per route: The span in radians is calculated using:

<!-- formula-not-decoded -->

The features derived are:

- (19) The average span [1];
- (20) The standard deviation of the span [2];
- (21) The minimum value among the spans;
- (22) The maximum value among the spans.

Compactness per route measured by width: This is calculated as:

<!-- formula-not-decoded -->

The features derived are:

- (23) The average compactness [1];
- (24) The minimum value of compactness;
- (25) The maximum value of compactness.

## Compactness per route measured in radians: This  is  calculated

using:

<!-- formula-not-decoded -->

The features derived are:

- (26) The average compactness [1];
- (27) The minimum value of compactness;
- (28) The maximum value of compactness.

Depth per route: The depth per route is calculated as:

<!-- formula-not-decoded -->

The features derived are:

- (29) The average depth [1];
- (30) The standard deviation of the depth of each route [2];
- (31) The minimum value among the depths;
- (32) The maximum value among the depths.

Number of customers per route: This is calculated using:

<!-- formula-not-decoded -->

The features derived are:

- (33) The average number of customers per route;
- (34) The standard  deviation  of  the  number  of  customers  per route [1].

Demand of the farthest customer of each route: The farthest customer is determined by:

<!-- formula-not-decoded -->

Then, the average demand is calculated as:

<!-- formula-not-decoded -->

The features derived are:

- (35) The average demand of the farthest customer [2];
- (36) The standard deviation of the demand of the farthest customer of each route [2].

Demand of the first and last customer of each route: This is  calculated as:

<!-- formula-not-decoded -->

The features derived are:

- (37) The average demand;
- (38) The standard deviation of the demand.

Degree of neighborhood of each customer: The degree of neighborhood for a customer is calculated as:

<!-- formula-not-decoded -->

where 𝐷𝑁𝑖 is  the  degree  of  neighborhood  of  customer 𝑖 ,  and rank 𝑖,𝑗 is the rank of customer 𝑗 in the sorted list of distances from customer 𝑖 to all other customers.

The features derived are:

- (39) The average degree of neighborhood of each customer;
- (40) The  standard  deviation  of  the  degree  of  neighborhood of each customer.

Degree of neighborhood of each route: This is calculated as:

<!-- formula-not-decoded -->

where 𝐷𝑁𝑖 is the degree of neighborhood of the 𝑖 th customer in the route, and | 𝑟𝑜𝑢𝑡𝑒 | is the number of customers in the route. The first and last customers in the route are excluded from the calculation.

The features derived are:

- (41) The average degree of neighborhood [2];
- (42) The standard deviation of the degree of neighborhood of each route.

Length of all routes: The length of a route is calculated as:

<!-- formula-not-decoded -->

The features derived are:

- (43) The average length;
- (44) The standard deviation of the length of each route [2];
- (45) The minimum value among the lengths;
- (46) The maximum value among the lengths;
- (47) The sum of the length of all the routes.

Area of routes: The  formula  for  calculating  the  area  of  a  CVRP route using the shoelace formula can be derived from the 𝑥 and 𝑦 coordinates of the nodes visited in the route. Let 𝑟 be a route in the CVRP solution, with 𝑛 𝑟 𝑖 representing the 𝑖 th node visited in 𝑟 . The coordinates of each node 𝑛 𝑟 𝑖 are denoted by ( 𝑥 𝑖 , 𝑦 𝑖 ) . The area of the route 𝑟 can be calculated using the shoelace formula as:

<!-- formula-not-decoded -->

where | 𝑟 | is the number of nodes in route 𝑟 , 𝑥 0 = 𝑥 𝑟 | | , 𝑦 0 = 𝑦 𝑟 | | , 𝑥 𝑟 | | +1 = 𝑥 1 , and 𝑦 𝑟 | | +1 = 𝑦 1 . This formula calculates the signed area of the polygon formed by connecting the nodes in the order they are visited in the route. The absolute value of the result is taken to obtain the area regardless of the orientation of the polygon.

The features derived from this calculation are:

- (48) The average area of each route;
- (49) The smallest area among the routes;
- (50) The largest area among the routes.

In what follows, we describe the two phases that comprise ISC: (1) the training phase and (2) the classification phase. We also explain how to integrate ISC into an MSM to solve the CVRP.

## 3.1.1. Training phase of ISC

The training phase extracts features 𝐹 from initial solutions 𝑆𝑜 and trains an ML classifier 𝐵 to predict the quality of the solutions 𝑆 ′ after applying local-search. This process is described in Algorithm 2.

After a feasible initial solution 𝑆𝑜 is created, the features mentioned previously are extracted from such solution, and stored into a solutions dataset 𝐷𝑠𝑜𝑙 . Then, a local-search algorithm 𝐿𝑆 is applied to the initial solution 𝑆𝑜 , and, from the improved solution 𝑆 ′ , the 𝑐𝑜𝑠𝑡 𝑆 ( ′ ) -that we named improved cost value ( 𝑐𝑣 ) - is also stored in 𝐷𝑠𝑜𝑙 . This process is repeated until a time limit 𝜃 is reached. When the cost of an improved solution 𝑆 ′ is  lower than the cost of the best solution 𝑆 ∗ ,  the  best solution 𝑆 ∗ is updated.

The algorithm proceeds as follows. The improved cost values , from the solutions dataset 𝐷𝑠𝑜𝑙 , are used to label the solutions as promising ( 𝑃 ) and unpromising ( 𝑈 ) solutions. A percentage 𝜆 𝑝 of solutions with the lowest 𝑐𝑣 are labeled 𝑃 .  A  percentage 𝜆 𝑢 of  solutions  with  the highest 𝑐𝑣 are also labeled 𝑈 . The remaining solutions, neither the best nor the worst, remain unlabeled and discarded. The 𝑐𝑣 of the solutions is  also  discarded.  The  new  dataset,  comprised  only  by  labeled  promising and unpromising solutions, is the training dataset 𝐷𝑡𝑟𝑎𝑖𝑛 .  Next,  the  binary classifier 𝐵 is trained with data from 𝐷𝑡𝑟𝑎𝑖𝑛 . In this work, we train 𝐵 with a Random-Forest algorithm as shown in Algorithm 7. Finally, the trained classifier 𝐵 and the best solution found 𝑆 ∗ are returned.

![Image](image_000003_ef85061bc8642ecf1fb76816651979d6d873e119a75e676fe9efa8a681b6bbbe.png)

## 3.1.2. Classification phase of ISC

The classification phase starts when the training phase ends. This phase is described in Algorithm 3. This phase consists of extracting features from initial solutions 𝑆𝑜 and using the trained binary classifier 𝐵 to classify  (predict  to  which  class  belongs) 𝐿 the solutions into promising 𝑃 or unpromising 𝑈 solutions. If a solution is classified as promising, a local-search algorithm 𝐿𝑆 is applied. Otherwise, the algorithm skips the solutions classified as unpromising, without applying local-search to them. When the cost of an improved solution 𝑆 ′ is lower than the cost  of  the  best  solution 𝑆 ∗ ,  the  best  solution 𝑆 ∗ is  updated.  This  process is repeated until the time limit for the classification phase 𝛹 is reached. Finally, the best solution found is returned as 𝑆 ∗ .

We could choose one of the different MSM that are commonly used in the CVRP, but we chose to work with a GRASP for its simplicity. Regarding the selection of the binary classifier of ISC, we opted for a Random-Forest Classifier [14]. Despite the advances in other artificial  intelligence  methods  such  as  Reinforcement Learning [26], or Deep-learning techniques, tree-based models such as Random-Forest, Gradient Boosting, or XGBoost, still  outperform  the  performance  of these methods when working with tabular data. We refer the reader to the work of Grinsztajn et al. [32] for a more comprehensive study of this comparison. Additionally, the reasons for choosing this classifier are the following:

- · Features do not have to be re-scaled to similar order magnitudes.

## Algorithm 3: ISC's Classification Phase

## Input: 𝐵 𝑆 , ∗ , 𝛹 , 𝛼

- 1 𝑐𝑜𝑠𝑡 𝑏𝑒𝑠𝑡 \_ ← 𝑐𝑜𝑠𝑡 𝑆 ( ∗ ) , 𝜏 ← 0
- 2 𝑠𝑡𝑎𝑟𝑡 𝑡𝑖𝑚𝑒 \_ ← current\_time()
- 3 while 𝜏 &lt; 𝛹 do
- 4 𝑆𝑜 ← GreedyRandomizedConstruction( 𝛼 )
- 5 𝐹 ← extract\_features( 𝑆𝑜 )
- 6 𝐿 ← classify( 𝐵 𝐹 , )
- 7 if 𝐿 is P then

8

𝑆

′

←

LS(

𝑆𝑜

9

10

11

12

13

𝜏

)

𝑐𝑜𝑠𝑡 𝑆

(

′

)

&lt; 𝑐𝑜𝑠𝑡 𝑏𝑒𝑠𝑡

𝑐𝑜𝑠𝑡 𝑏𝑒𝑠𝑡

\_

\_

←

then

𝑐𝑜𝑠𝑡 𝑆

(

𝑆

∗

←

𝑆

′

←

current\_time() -

𝑠𝑡𝑎𝑟𝑡 𝑡𝑖𝑚𝑒

′

)

\_

return

𝑆

∗

- · Training can be done on a single central-processing-unit core in a reasonable amount of time.
- · Simpler to implement and to determine hyperparameter values than methods that use Gradient Boosted Trees.
- · It fits multiple bagged decision trees classifiers, on different subsamples of a dataset, for training, reducing the high variance that individual decision trees exhibit.

## 3.2. Integration of ISC into GRASP for the CVRP

The incorporation of the training phase and the classification phase into an MSM such as GRASP is straightforward: We replace the initialsolution creation step and the local-search step, with the training phase and the classification phase of ISC, and we establish an individual timelimit for each phase. The structure of the integration of ISC into GRASP is described in Algorithm 4.

In the GRASP algorithm for the CVRP, we constructed the initial solutions  using  a  randomized  variation  of  the  well-known  parallel savings algorithm by Clarke and Wright [33]. The Clarke and Wright algorithm (C-W) is a purely greedy and deterministic algorithm; therefore, we introduced a randomization like the one proposed by Hart and Shodan [34].

The C-W algorithm starts by creating an initial solution, where each customer is visited in an individual route, from depot to customer, and then back to depot. After, for each pair of customers, the saving that would result from visiting these customers contiguously, before returning to the depot, is determined. The information of each pair of customers and their savings is stored on a savings list. The savings list is sorted in decreasing order of the savings amount. Next, to add randomization, we create a restricted candidate list (RCL) with the highest savings. Now, the algorithm randomly selects a saving from the  RCL,  merging  the  two  routes  of  the  pair  of  customers  of  each saving if it is possible, and decreasing the cost of the solution by the saving amount. After a saving is selected from the RCL, the RCL is updated. This merging is possible when the following three conditions are satisfied. (1) The customers are currently in different routes. (2) Both customers are directly connected to the depot. (3) The sum of demands of the two routes is not larger than the vehicle capacity.

To demonstrate the improvement capacity and generality of ISC, we study its application when two different local-search algorithms ( 𝐿𝑆 1 and 𝐿𝑆 2 ) are used. The first one is variable neighborhood descent (VND), which is a deterministic variant of the Variable Neighborhood Search  proposed  by  Mladenovic  and  Hansen  [12].  The  other  localsearch algorithm is FILO0 [13] by Accorsi and Vigo, a variation of the Ruin-&amp;-Recreate method used in FILO [13], which is itself inspired on the Slack Induction by String Removal (SISR) algorithm of Christiaens if

## Algorithm 4: Integration of ISC into GRASP for the CVRP

Input: 𝛤 , 𝜃 , 𝐵 𝛼 ,

ISC's Training phase

- 1 𝐷𝑠𝑜𝑙 ← ∅ , 𝐷𝑡𝑟𝑎𝑖𝑛 ← ∅ , 𝑐𝑜𝑠𝑡 𝑏𝑒𝑠𝑡 \_ ← ∞ , 𝜏 ← 0 2 𝑠𝑡𝑎𝑟𝑡 𝑡𝑖𝑚𝑒 \_ ← current\_time() 3 while 𝜏 &lt; 𝜃 do 4 𝑆𝑜 ← GreedyRandomizedConstruction( 𝛼 ) 5 𝐷𝑠𝑜𝑙 ← extract\_features( 𝑆𝑜 ) 6 𝑆 ′ ← LS( 𝑆𝑜 ) 7 𝐷𝑠𝑜𝑙 ← store\_cost( 𝑆 ′ ) 8 if 𝑐𝑜𝑠𝑡 𝑆 ( ′ ) &lt; 𝑐𝑜𝑠𝑡 𝑏𝑒𝑠𝑡 \_ then 9 𝑐𝑜𝑠𝑡 𝑏𝑒𝑠𝑡 \_ ← 𝑐𝑜𝑠𝑡 𝑆 ( ′ ) 10 𝑆 ∗ ← 𝑆 ′ 11 𝜏 ← current\_time() -𝑠𝑡𝑎𝑟𝑡 𝑡𝑖𝑚𝑒 \_ 12 𝐷𝑡𝑟𝑎𝑖𝑛 ← label\_solutions( 𝐷𝑠𝑜𝑙 )
- 13 𝐵 ← train\_classifier( 𝐵 𝐷𝑡𝑟𝑎𝑖𝑛 , )

ISC's Classification phase

```
14 𝛹 ← 𝛤 -𝜃 15 while 𝜏 < 𝛹 do 16 𝑆𝑜 ← GreedyRandomizedConstruction( 𝛼 ) 17 𝐹 ← extract_features( 𝑆𝑜 ) 18 𝐿 ← classify( 𝐵 𝐹 , ) 19 if 𝐿 is P then 20 𝑆 ′ ← LS( 𝑆𝑜 ) 21 if 𝑐𝑜𝑠𝑡 𝑆 ( ′ ) < 𝑐𝑜𝑠𝑡 𝑏𝑒𝑠𝑡 _ then 22 𝑐𝑜𝑠𝑡 𝑏𝑒𝑠𝑡 _ ← 𝑐𝑜𝑠𝑡 𝑆 ( ′ ) 23 𝑆 ∗ ← 𝑆 ′ 24 𝜏 ← current_time() -𝑠𝑡𝑎𝑟𝑡 𝑡𝑖𝑚𝑒 _ 25 return 𝑆 ∗
```

and  Vanden  Berghe  [23].  In  both  cases,  we  use  a  Random-Forest classifier [14]. In what follows, we explain VND local-search, FILO0 local-search and random forest classifier.

## 3.3. VND local-search

The  VND algorithm  is  presented  in  Algorithm 5.  The  algorithm proceeds as follows. First, the order of the neighborhoods 𝐻𝑛 is established. After, the first neighborhood to start the exploration procedure is assigned. Now, in a cyclic manner, neighborhood 𝐻𝑛 is completely explored. The neighborhood movement that best improves the current solution 𝑆 is applied and the first neighborhood 𝐻 1 is assigned once again as the next neighborhood. In case no solution improvement is found, the next neighborhood is selected to be explored. When no improvement is available after exploring all neighborhoods, the solution 𝑆 is returned. This sequential neighborhood change step corresponds to the Basic VND (BVND) variant, as described by Hansen et al. [35].

We implemented four different classical neighborhoods: SWAP (S), RELOCATE (R), 2-OPT (O), and 2-OPT* (O*). Both S and R are implemented as inter-route and intra-route  neighborhoods.  These  four neighborhoods enable 24 possible order permutations for the BVND, from which we selected four different BVND permutations: R-S-O-O*, S-R-O*-O, O-S-R-O*, and O*-S-O-R. R-S-O-O* was selected following an increasing order of computational complexity, and the other three were selected randomly. We applied each one of the BVNDs to each initial solution individually,  because  the  order  in  which  the  neighborhoods  are applied affects the solution quality. For ISC, we consider each BVND as a different local-search algorithm. Therefore, we trained a binary classifier 𝐵 for each BVND (i.e., four classifiers in our implementation).

## Algorithm 5: BVND. Adapted from [12]

```
Input: 𝑆𝑜 , 𝑙 1 Set the order of neighborhood structures 𝐻 ,𝐻 ,..., 𝐻 𝑙 1 2 2 𝑆 ← 𝑆𝑜 3 𝑛 ← 1 4 while 𝑛 ≤ 𝑙 do 5 Find the best neighbor 𝑆 ′ of 𝑆 in the neighborhood 𝐻𝑛 (S) 6 if 𝑐𝑜𝑠𝑡 𝑆 ( ′ ) < 𝑐𝑜𝑠𝑡 𝑆 ( ) then 7 𝑆 ← 𝑆 ′ 8 𝑛 ← 1 9 else 10 𝑛 ← 𝑛 +1 11 return 𝑆
```

## 3.4. FILO0 local-search

```
Algorithm 6: FILO0. Adapted from [13] Input: 𝑆𝑜 , 𝛥 𝑓 1 𝑤 ← ( 𝑤 , 𝑤 , ..., 𝑤 0 1 𝑐 ) , 𝑤 𝑖 ← 𝑤𝑏𝑎𝑠𝑒 ∀ 𝑖 ∈ 𝑁 2 𝑆 ← 𝑆𝑜 3 𝑆 ∗ ← 𝑆 , 𝑇 ← 𝑇 0 4 for 𝑖𝑡 ← 1 to 𝛥 𝑓 do 5 ̂ , 𝑖 𝑆 ′ ← 𝑅𝑢𝑖𝑛 𝑆, 𝑤 ( ) 6 𝑆 ′ ← 𝑅𝑒𝑐𝑟𝑒𝑎𝑡𝑒 𝑆 ̂ ( ) 7 if 𝑐𝑜𝑠𝑡 𝑆 ( ′ ) < 𝑐𝑜𝑠𝑡 𝑆 ( ∗ ) then 8 𝑆 ∗ ← 𝑆 ′ 9 𝑤 ← 𝑈𝑝𝑑𝑎𝑡𝑒𝑆ℎ𝑎𝑘𝑖𝑛𝑔𝑃𝑎𝑟𝑎𝑚𝑒𝑡𝑒𝑟𝑠 𝑤, 𝑆 , 𝑆, 𝑖 ( ′ ′ ) 10 if 𝑎𝑐𝑐𝑒𝑝𝑡 𝑐𝑟𝑖𝑡𝑒𝑟𝑖𝑜𝑛 𝑆, 𝑆 , 𝑇 _ ( ′ ) then 11 𝑆 ← 𝑆 ′ 12 𝑇 ← 𝑑𝑒𝑐𝑟𝑒𝑎𝑠𝑒 𝑡𝑒𝑚𝑝𝑒𝑟𝑎𝑡𝑢𝑟𝑒 𝑇 _ ( ) 13 return 𝑆 ∗
```

FILO0 is a simplified  variant  of  the  FILO  algorithm,  which  is  a state-of-the-art metaheuristic for the CVRP [13]. FILO0 is a simulatedannealing  metaheuristic,  with  a  local-search  Ruin-&amp;-Recreate  algorithm,  coupled  with  an  update  strategy  to  control  the  intensity  of the ruin procedure. On each iteration, a feasible solution 𝑆 is ruined by removing some customers from their routes. After, each unrouted customer is greedily inserted into the best possible feasible position in the ruined solution ̂ 𝑆 . Once all customers are routed, a new feasible solution 𝑆 ′ is obtained. When the cost of an improved solution 𝑆 ′ is lower than the cost of the best solution 𝑆 ∗ ,  the  best  solution 𝑆 ∗ is updated. The shaking parameters 𝜔 - that control the intensity of the ruin algorithm - are updated while taking into account the cost of the previous solution 𝑆 , the cost of the new solution 𝑆 ′ , and past executed ruin procedures. FILO0 is presented in Algorithm 6.

To use either BVND or FILO0 within the integration of ISC into GRASP,  we  replace  the  local-search  algorithm,  on  lines  6  and  21, of  Algorithm 4,  with  Algorithm 5 (BVND) or Algorithm 6 (FILO0), respectively. A similar replacement can be done within other MSM.

## 3.5. Random-forest classifier

The binary classifier 𝐵 used in the integration of ISC into GRASP for the CVRP is a Random-Forest Classifier [14]. A Random-Forest is composed by a set of decision trees. A decision tree is an algorithm that splits the dataset recursively into sub-spaces, by selecting the feature that maximizes the separation of the data. At each step, the selected feature should reduce the heterogeneity of the data, according to a specific measure, by splitting one of the sub-spaces. After a split, each partition of the subspace corresponds to a hyper-plane defined in a

single dimension. New data is classified based on the majority of classes of the sub-spaces where the features belong.

A decision tree by itself is likely to overfit the training data, and might exhibit high variance when classifying new data [14]. Therefore, Breiman [14] proposed to combine multiple decision trees into a Random-Forest. Each tree is trained with a randomly selected subset of features from the data. For new data, the predicted class corresponds to the class majority predicted by the trees.

The Random-Forest algorithm is described in Algorithm 7 [36]. The decision trees implement a Classification and Regression Trees (CART) classifier [37]. The maximum depth of the tree 𝑑 𝑚𝑎𝑥 and the weight associated to each class 𝜌 are among the most important parameters for ISC to work. We used the Gini index as the measure of impurity 𝑖𝑚 to decide node splits [37]. Additionally, in ISC, all samples from the dataset are used to train each tree.

Algorithm 7: Random-Forest. Adapted from [36].

<!-- formula-not-decoded -->

## Training

- 1 Select randomly 𝑓 𝑚𝑎𝑥 features from the features in 𝐷𝑡𝑟𝑎𝑖𝑛 . 2 for 𝑛 𝑒 trees do
- 3 for 𝑓 ∈ 𝑓 𝑚𝑎𝑥 features do
- 4 a. Calculate the Gini impurity index 𝐺 at each node 𝑑 . 5

<!-- formula-not-decoded -->

𝑐 where 𝑡 𝑐 is the weight of all observations in a potential child node 𝑐 , 𝑛 𝑖 is the number of observations of class 𝑖 in 𝑐 , and 𝜌 𝑖 is the class weight. The impurity of 𝑐 is 𝑖 𝑐 , and 𝑡 𝑝 is the weight of all observations in the parent node (before the split).

- b. Select the node 𝑑 which has the lowest impurity value 𝐺 𝑑 ( ) , with a minimum impurity value 𝑖𝑠 𝑚𝑖𝑛 , and a minimum number of samples 𝑠𝑠 𝑚𝑖𝑛 .
- c. Split the samples from 𝑑 into sub-nodes, each resulting sub-node must have at least 𝑠𝑙 𝑚𝑖𝑛 samples.
- d. Repeat steps a, b, and c to construct the tree 𝑒 until reaching the maximum depth of a tree 𝑑 𝑚𝑎𝑥 .

𝐵 ← 𝑒 : Consolidate tree 𝑒 in the Random-Forest 𝐵 .

6

7

8

9

Classification

- 10 Use the features 𝐹 and the rules of each decision tree 𝑒 ∈ 𝐵 to predict the class.
- 11 Calculate the number of votes for each predicted class.
- 12 Consider the highest-voted predicted class as the final prediction 𝐿 .
- 13 return 𝐿

In this section, we presented ISC, a method that uses ML to improve MSM, by choosing which initial solutions should be improved with local-search. We introduced new features to describe a CVRP solution. These features are complementary to the features proposed in [1,2]. We integrated ISC with two MSM. These integrations' computational experiments and results are presented in Section 4.

## 4. Computational experiments

Computational experiments evaluate the benefits of ISC when it is integrated into an MSM, and how different instance characteristics affect its performance. To this end, we evaluated the algorithms on tagmcbeginthe 𝑋 dataset proposed by Uchoa et al. [38]. This dataset contains 100 benchmark instances, with a varying number of customers ranging from 100 to 1000 (small if less than 250, medium if from 250 to 500, and large if 500 or more), different customer positioning (C = clustered, R = random, and RC = mixed), and different depot locations (C = central, E = eccentric, and R = random).

The metaheuristics GRASP-BVND (G-BVND), GRASP-FILO0 (G-FILO0), along with their respective variants GRASP-BVND-ISC (GBVND-ISC) and GRASP-FILO0-ISC (G-FILO0-ISC) are specified in Section 3. G-BVND is a GRASP to solve the CVRP that uses BVND and G-FILO0 is a GRASP to solve the CVRP that uses FILO0. Variant GBVND-ISC is the integration of ISC to G-BVND and G-FILO0-ISC is the integration of ISC to G-FILO0.

All algorithms 2 were coded in C++ and compiled using Intel oneAPI DPC++/C++ Compiler, Boost 1.78 library [39], and Twigy library [40]. The experiments were run on an Intel Xeon Gold 6130 CPU, running at 2.10 GHz, with 64 GB of RAM, on a Rocky Linux 8.5 Operating System. The experiments for G-BVND and G-BVND-ISC were run on four parallel cores. The experiments for G-FILO0 and G-FILO0-ISC were run on a single core. Each algorithm with and without the ISC procedure was executed within the same processing time limit in any given instance. Because of the randomized nature of the GRASP algorithm, we executed each experiment 10 times, with a different random seed, and reported the  average  gap  (GAP)  of  the  solution  found,  with  respect  to  the best-known solution (BKS), as shown below.

<!-- formula-not-decoded -->

## 𝐵𝐾𝑆

Recently, to objectively assess the performance differences between two algorithms, Accorsi et al. [41] recommended using a one-tailed Wilcoxon signed-rank test for paired observations [42]. This allows us to determine whether two sets of paired observations are statistically different or if the two algorithms are considered to provide equivalent results. This test can be performed even if no assumption can be made on the distributions of the paired observations sets. We followed this recommendation and evaluated the statistical significance of the performance difference between each pair of methods using a one-tailed Wilcoxon signed-rank test at a significance level of 𝑝 = 0 025 . .

## 4.1. Parameters

The parameters used in the different algorithms of this work are summarized in Table 2. The values of some of them were selected from the algorithms found in the literature (i.e., FILO0), while others were tuned when considering the performance on the small instances of dataset 𝑋 . For ease of notation, 𝑁 represents the number of customers of each instance.

As previously mentioned, the randomized Clarke and Wright construction method uses the well-known Clarke and Wright Savings algorithm [33], with a slight variation to randomize the generated solutions.  Instead  of  using  the  sorted  savings  to  construct  an  initial solution (this would lead to the same solution being constructed in every iteration), the savings are sorted, and an RCL of size ten is used to select which saving will be evaluated. This means that the customers to be joined by the savings algorithm are randomly selected at each step out of the 10 best possible savings in the RCL.

The training dataset 𝐷𝑡𝑟𝑎𝑖𝑛 that is used to train 𝐵 is obtained from the solutions dataset 𝐷𝑠𝑜𝑙 as follows. Fifteen percent of the solutions with the lowest improved cost value 𝑐𝑣 are labeled as promising solutions. Fifty percent of the solutions with the highest 𝑐𝑣 are labeled as unpromising solutions. The remaining 35% of the solutions, which are  unlabeled,  are  discarded  to  create  a  ''gap''  between  promising

2 All algorithms are available at https://github.com/mesax1/ISC-CVRP.

Table 2 Parameters of the different algorithms.

| GRASP                                                                     |                                                                                                                                                                                                                                                                                                                                                  |
|---------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 𝑁 𝛤 |RCL| = 10                                                            | Number of customers of the instance. Execution time limit for GRASP in seconds. Size of the restricted candidate list.                                                                                                                                                                                                                           |
| FILO0                                                                     |                                                                                                                                                                                                                                                                                                                                                  |
| 𝛥 𝑓 = 100 ∗ 𝐶 𝑇 0 , 𝑇 𝑓 𝜔 𝑏𝑎𝑠𝑒 = | 𝑙𝑛 | 𝐶 || 𝐼 𝐿𝐵 = 0 . 375 𝐼 𝑈𝐵 = 0 . 85 | Number of core optimization iterations of FILO0. Initial and final simulated annealing temperature. Initial shaking intensity of ruin procedure. Lower bound of the shaking factors. Upper bound of the shaking factors.                                                                                                                         |
| ISC                                                                       |                                                                                                                                                                                                                                                                                                                                                  |
| 𝜃 = 0 . 25 ∗ 𝛤 𝜆 𝑝 = 15% 𝜆 𝑢 = 50% Random Forest                          | Training time limit in seconds. Percentage of the solutions with the lowest 𝑐𝑣 to be labeled as promising solutions. Percentage of the solutions with the highest 𝑐𝑣 to be labeled as unpromising solutions.                                                                                                                                     |
| 𝑠𝑙 𝑚𝑖𝑛 = 1 𝑑 𝑚𝑎𝑥 = 3 𝑠𝑠 𝑚𝑖𝑛 = 2 𝑓 𝑚𝑎𝑥 = 8 𝑖𝑠 𝑚𝑖𝑛 = 0 . 00 𝑛 𝑒 = 100       | Minimum number of samples at a leaf node. Maximum depth to which a tree is grown. Minimum number of samples for a node to be split. Maximum number of randomly selected features to be considered at each split. Minimal impurity value for a node to be considered for another split. Number of decision tree estimators to use as classifiers. |

and unpromising solutions. This gap between solutions allows for better differentiation between promising and unpromising solutions for the classifier.  Without  the  gap,  it  is  harder  for  the  ML  classifier  to learn  which  value  of  a  feature  should  be  used  as  a  threshold.  The difference in the number of solutions labeled as promising and unpromising provokes an imbalance in the class labels of the training dataset. This imbalance in the class labels requires an adjustment of the weights [37], which is inversely proportional to the class frequencies in 𝐷𝑡𝑟𝑎𝑖𝑛 . Therefore, class weights 𝜌 are the following:

<!-- formula-not-decoded -->

## and

<!-- formula-not-decoded -->

When  including  BVND  into  GRASP,  the  neighborhoods  are  exhaustively explored, and an improving movement is executed if the best possible improvement is found. This is typically called the bestimprovement strategy.

GRASP's time limit, in seconds, 𝛤 , for the execution of G-BVND and G-BVND-ISC, is proportional to the size of the instance as follows. For small instances with less than 250 customers, 𝛤 = 𝑁 .  For medium instances, with 250 or more customers and less than 500 customers, 𝛤 = 3 ∗ 𝑁 . Finally, for large instances, with 500 customers or more, 𝛤 = 10 ∗ 𝑁 . Similarly, for the execution of G-FILO0 and G-FILO0-ISC, 𝛤 is also proportional to the size of the instance, with lower time limit values, as FILO0 is faster than BVND for medium and large instances. The values are as follows. For small instances 𝛤 = 𝑁 . For medium instances 𝛤 = 2 ∗ 𝑁 . Finally, for large instances 𝛤 = 3 ∗ 𝑁 . The parameter 𝜃 is proportional to the 𝛤 of every instance. For G-BVND-ISC and G-FILO0-ISC, ISC's training time limit is 𝜃 = 0 25 ∗ . 𝛤 .

The parameters of FILO0 are taken directly from the work of Accorsi and Vigo [13]. The only exception is 𝛥 𝑓 , which corresponds to 𝛥 𝐶𝑂 in [13]. In our case, we established the number of ruin-and-recreate iterations  to  be  proportional  to  the  size  of  the  instance;  therefore, 𝛥 𝑓 = 100 ∗ 𝑁 . The initial temperature 𝑇 0 and final temperature 𝑇 𝑓 , for simulated annealing, are proportional to the average cost of an arc in an instance. Specifically, the value of 𝑇 0 is equal to 0.1 times the average instance arc cost, and 𝑇 𝑓 is 0.01 times 𝑇 0 .

## 4.2. Results

Our  experiments  evaluated  the  performance  of  ISC  on  different instances and for two local-search algorithms. Fig. 1 shows the results

Table 3

Comparison of the performance of MSM with and without ISC. The best average performance per subset is in bold.

|                 | G-BVNDG-BVND-ISC   | G-BVNDG-BVND-ISC   | G-BVNDG-BVND-ISC   | G-BVNDG-BVND-ISC   | Sign.G-FILO0G-FILO0-ISC   | Sign.G-FILO0G-FILO0-ISC   | Sign.G-FILO0G-FILO0-ISC   | Sign.G-FILO0G-FILO0-ISC   | Sign.   |
|-----------------|--------------------|--------------------|--------------------|--------------------|---------------------------|---------------------------|---------------------------|---------------------------|---------|
| Subset          | # Gap(%) Gap(%)    | # Gap(%) Gap(%)    | # Gap(%) Gap(%)    | T*(min)            | T*(min)                   | Gap(%) Gap(%)             | Gap(%) Gap(%)             | T(min)                    | T(min)  |
| All             |                    | 1002.632           | 2.563              | 45.74              | Yes                       | 0.775                     | 0.747                     | 16.67                     | Yes     |
| Small           | 32                 | 2.097              | 2.020              | 2.88               | Yes                       | 0.370                     | 0.353                     | 2.88                      | No      |
| Medium          | 36                 | 2.732              | 2.654              | 17.45              | Yes                       | 0.828                     | 0.784                     | 11.64                     | Yes     |
| Large           | 32                 | 3.056              | 3.005              | 120.43             | Yes                       | 1.121                     | 1.099                     | 36.13                     | Yes     |
| Depot (C)       | 32                 | 2.864              | 2.794              | 42.93              | Yes                       | 0.747                     | 0.723                     | 15.78                     | Yes     |
| Depot (E)       | 34                 | 2.205              | 2.142              | 45.89              | Yes                       | 0.700                     | 0.673                     | 16.67                     | Yes     |
| Depot (R)       | 34                 | 2.840              | 2.768              | 48.25              | Yes                       | 0.877                     | 0.843                     | 17.51                     | Yes     |
| Customer (C)    | 32                 | 2.618              | 2.539              | 43.66              | Yes                       | 0.761                     | 0.740                     | 16.03                     | Yes     |
| Customer (R)    | 33                 | 2.798              | 2.733              | 48.18              | Yes                       | 0.891                     | 0.859                     | 17.44                     | Yes     |
| Customer (RC)35 |                    | 2.489              | 2.425              | 45.35              | Yes                       | 0.680                     | 0.648                     | 16.54                     | Yes     |

T* is for parallel execution in 4 threads.

of the comparison of G-BVND Vs. G-BVND-ISC on all instances and among  different  instances  sizes,  obtained  over  ten  executions  with different random seeds on each instance of the 𝑋 dataset. Similarly, Fig. 2 shows the results of the comparison of G-FILO0 Vs. G-FILO0-ISC.

Table 3 shows the results of the comparison of the four configurations of the GRASP metaheuristics: G-BVND Vs. G-BVND-ISC and GFILO0 Vs. G-FILO0-ISC, obtained over ten executions on each instance of the 𝑋 dataset. The instances are divided into subsets according to their characteristics. The table shows the number of instances (#) of each subset, the average gap value (Gap(%)) obtained for each of these subsets, along with the execution time (T) in minutes, and the results of the statistical difference significance (Sign.) of the Wilcoxon test.

The improvement of the performance of GRASP combined with ISC for the CVRP is shown in Table 3. Overall, both local-search algorithms are  guided  towards  promising  initial  solutions,  delivering  solutions with better quality than the methods without ISC, with an average improvement  of  0.067%  in  the  BVND  method,  and  0.028%  in  the FILO0 method. However, FILO0 delivers solutions with state-of-the-art quality, where an improvement of 0.028% is difficult to achieve.

An example that illustrates the improvement that ISC provides is shown  on Fig.  3.  The  average  Gap(%)  reported  corresponds  to  10 different executions of G-FILO0-ISC and G-FILO0 on instance X-n33115 at different time steps. After 25% to 30% of the CPU time limit, ISC starts classifying the initial solutions, boosting the search performance. Therefore, G-FILO0-ISC converges faster to better-quality solutions than G-FILO0.

Fig. 1. Comparison of the performance of MSM using BVND local-search with and without ISC.

![Image](image_000004_feaf866bfc99a3a4679d9d1c71d15e7c3b98199055d6966ce2af15f9bfaa1269.png)

Fig. 2. Comparison of the performance of MSM using FILO0 local-search with and without ISC.

![Image](image_000005_dd24e90f6046c7695d74d0e2c383281cdf6f104ca57f4ea68565afcc21e8ab36.png)

When  analyzing  the  performance  along  the  different  subsets  of instances, we observe significant improvements on all the subsets for both BVND and FILO0. The only exception is the subset of small instances, where the difference between G-FILO0 and G-FILO0-ISC is not statistically significant. In small instances, the improvement is limited when using ISC to improve FILO0 methods, because FILO0 by itself can find high-quality solutions with ease.

Compared  to  G-BVND,  G-BVND-ISC  delivers  a  better  averagesolution  quality  on  99  out  of  the  100  instances  of  the 𝑋 dataset. Similarly, G-FILO0-ISC delivers better results than G-FILO0 on 81 out of 100 instances. These results confirm the usefulness of ISC when applied to two GRASP metaheuristics with different local-search algorithms. ISC compensates for the computational effort introduced by the featureextraction  and  solution  classification  procedures,  with  a  significant improvement of MSM's performance. For information on the results obtained for each instance, see Appendix.

## 5. Conclusions and future-work directions

In this paper, we presented ISC: a method to classify and detect promising initial solutions within MSM for the CVRP. ISC combines (1) feature extraction, (2) machine-learning classification, and (3) vehicle routing heuristics to improve the performance of existing MSM. We successfully integrated ISC with two GRASP metaheuristics with different local-search algorithms.

We performed experiments to evaluate the performance of ISC. ISC improves the performance of MSM, accelerating the convergence to better-quality  solutions.  Considering  the  Wilcoxon  test,  the  average improvements for an MSM when using ISC with either BVND localsearch or FILO0 local-search, are statistically significant. For almost

Fig. 3. Average convergence of G-FILO0-ISC and G-FILO0 solutions over time for instance X-n331-k15.

![Image](image_000006_f136f7bf29cfbe21930fac5d5a9f822dd17828d24172c8ac144eb1676b6f2af3.png)

Table A.1 Computational results on the small instances of 𝑋 dataset. * Corresponds to execution time in four parallel cores.

| INSTANCE   | BKS    |   G-FILO0(%) |   G-FILO0-ISC(%) |   TIME(min) |   G-BVND(%) |   G-BVND-ISC(%) |   TIME*(min) |
|------------|--------|--------------|------------------|-------------|-------------|-----------------|--------------|
| X-n101-k25 | 27591  |        0.016 |            0.003 |        1.67 |       1.771 |           1.723 |         1.67 |
| X-n106-k14 | 26362  |        0.236 |            0.176 |        1.75 |       1.175 |           1.173 |         1.75 |
| X-n110-k13 | 14971  |        0     |            0     |        1.82 |       1.303 |           1.235 |         1.82 |
| X-n115-k10 | 12747  |        0     |            0     |        1.9  |       1.18  |           1.298 |         1.9  |
| X-n120-k6  | 13332  |        0.001 |            0     |        1.98 |       1.596 |           1.539 |         1.98 |
| X-n125-k30 | 55539  |        0.557 |            0.522 |        2.07 |       3.123 |           2.969 |         2.07 |
| X-n129-k18 | 28940  |        0.091 |            0.06  |        2.13 |       1.275 |           1.246 |         2.13 |
| X-n134-k13 | 10916  |        0.325 |            0.254 |        2.22 |       1.524 |           1.429 |         2.22 |
| X-n139-k10 | 13590  |        0.046 |            0.071 |        2.3  |       0.76  |           0.615 |         2.3  |
| X-n143-k7  | 15700  |        0.076 |            0.011 |        2.37 |       1.24  |           1.208 |         2.37 |
| X-n148-k46 | 43448  |        0.119 |            0.104 |        2.45 |       1.35  |           1.203 |         2.45 |
| X-n153-k22 | 21220  |        0.69  |            0.731 |        2.53 |       5.423 |           5.397 |         2.53 |
| X-n157-k13 | 16876  |        0.007 |            0.017 |        2.6  |       1.214 |           1.181 |         2.6  |
| X-n162-k11 | 14138  |        0.057 |            0.059 |        2.68 |       2.053 |           2.003 |         2.68 |
| X-n167-k10 | 20557  |        0.17  |            0.034 |        2.77 |       1.918 |           1.78  |         2.77 |
| X-n172-k51 | 45607  |        0.363 |            0.361 |        2.85 |       1.725 |           1.675 |         2.85 |
| X-n176-k26 | 47812  |        1.952 |            2.072 |        2.92 |       3.245 |           3.129 |         2.92 |
| X-n181-k23 | 25569  |        0.084 |            0.079 |        3    |       1.162 |           1.079 |         3    |
| X-n186-k15 | 24145  |        0.212 |            0.139 |        3.08 |       1.214 |           1.187 |         3.08 |
| X-n190-k8  | 16980  |        0.358 |            0.346 |        3.15 |       2.151 |           2.047 |         3.15 |
| X-n195-k51 | 44225  |        0.402 |            0.373 |        3.23 |       1.994 |           1.974 |         3.23 |
| X-n200-k36 | 58578  |        0.775 |            0.775 |        3.32 |       2.803 |           2.77  |         3.32 |
| X-n204-k19 | 19565  |        0.492 |            0.48  |        3.38 |       2.717 |           2.659 |         3.38 |
| X-n209-k16 | 30656  |        0.312 |            0.256 |        3.47 |       2.287 |           2.26  |         3.47 |
| X-n214-k11 | 10856  |        1.138 |            1.099 |        3.55 |       2.917 |           2.777 |         3.55 |
| X-n219-k73 | 117595 |        0.1   |            0.09  |        3.63 |       0.126 |           0.113 |         3.63 |
| X-n223-k34 | 40437  |        0.414 |            0.402 |        3.7  |       1.986 |           1.904 |         3.7  |
| X-n228-k23 | 25742  |        0.376 |            0.356 |        3.78 |       3.135 |           3     |         3.78 |
| X-n233-k16 | 19230  |        0.44  |            0.356 |        3.87 |       2.378 |           2.23  |         3.87 |
| X-n237-k14 | 27042  |        0.366 |            0.369 |        3.93 |       2.658 |           2.586 |         3.93 |
| X-n242-k48 | 82751  |        0.494 |            0.467 |        4.02 |       1.547 |           1.467 |         4.02 |
| X-n247-k50 | 37274  |        1.16  |            1.23  |        4.1  |       6.147 |           5.777 |         4.1  |
| Mean       | Mean   |        0.37  |            0.353 |        2.88 |       2.097 |           2.02  |         2.88 |

all  the  instances  subsets  that  we  evaluated,  the  improvements  are statistically significant, even considering the additional computational effort that ISC requires. The results of this work demonstrate that it is possible for an algorithm to learn to determine if an initial solution

Table A.2 Computational results on the medium instances of 𝑋 dataset. * Corresponds to execution time in four parallel cores.

| INSTANCE    | BKS    |   G-FILO0(%) |   G-FILO0-ISC(%) |   TIME(min) |   G-BVND(%) | G-BVND-ISC(%)   |   TIME*(min) |
|-------------|--------|--------------|------------------|-------------|-------------|-----------------|--------------|
| X-n251-k28  | 38684  |        0.733 |            0.639 |       8.33  |       2.198 | 2.147           |        12.5  |
| X-n256-k16  | 18839  |        0.289 |            0.254 |       8.5   |       2.263 | 2.203           |        12.75 |
| X-n261-k13  | 26558  |        1.066 |            0.926 |       8.67  |       3.54  | 3.423           |        13    |
| X-n266-k58  | 75478  |        1.019 |            0.935 |       8.83  |       2.401 | 2.340           |        13.25 |
| X-n270-k35  | 35291  |        0.509 |            0.495 |       8.97  |       2.355 | 2.286           |        13.45 |
| X-n275-k28  | 21245  |        0.366 |            0.358 |       9.13  |       2.522 | 2.484           |        13.7  |
| X-n280-k17  | 33503  |        1.048 |            0.969 |       9.3   |       3.037 | 3.011           |        13.95 |
| X-n284-k15  | 20226  |        1.303 |            1.138 |       9.43  |       4.008 | 3.852           |        14.15 |
| X-n289-k60  | 95151  |        1.076 |            1.033 |       9.6   |       2.274 | 2.190           |        14.4  |
| X-n294-k50  | 47161  |        0.467 |            0.46  |       9.77  |       1.635 | 1.563           |        14.65 |
| X-n298-k31  | 34231  |        0.821 |            0.724 |       9.9   |       3.368 | 3.354           |        14.85 |
| X-n303-k21  | 21736  |        0.727 |            0.719 |      10.07  |       2.775 | 2.715           |        15.1  |
| X-n308-k13  | 25859  |        0.84  |            0.879 |      10.23  |       3.05  | 2.953           |        15.35 |
| X-n313-k71  | 94043  |        0.902 |            0.909 |      10.4   |       2.563 | 2.542           |        15.6  |
| X-n317-k53  | 78355  |        0.074 |            0.065 |      10.53  |       0.551 | 0.543           |        15.8  |
| X-n322-k28  | 29834  |        0.785 |            0.677 |      10.7   |       3.191 | 2.947           |        16.05 |
| X-n327-k20  | 27532  |        0.892 |            0.793 |      10.87  |       3.535 | 3.469           |        16.3  |
| X-n331-k15  | 31102  |        0.492 |            0.416 |      11     |       3.324 | 3.132           |        16.5  |
| X-n336-k84  | 139111 |        0.938 |            0.923 |      11.17  |       2.326 | 2.255           |        16.75 |
| X-n344-k43  | 42050  |        0.976 |            0.912 |      11.43  |       2.944 | 2.888           |        17.15 |
| X-n351-k40  | 25896  |        1.113 |            1.014 |      11.67  |       2.56  | 2.512           |        17.5  |
| X-n359-k29  | 51505  |        0.86  |            0.795 |      11.93  |       2.318 | 2.284           |        17.9  |
| X-n367-k17  | 22814  |        0.768 |            0.75  |      12.2   |       3.527 | 3.385           |        18.3  |
| X-n376-k94  | 147713 |        0.063 |            0.058 |      12.5   |       0.342 | 0.339           |        18.75 |
| X-n384-k52  | 65940  |        0.823 |            0.804 |      12.77  |       2.319 | 2.244           |        19.15 |
| X-n393-k38  | 38260  |        0.749 |            0.737 |      13.07  |       2.486 | 2.376           |        19.6  |
| X-n401-k29  | 66154  |        0.622 |            0.605 |      13.33  |       1.855 | 1.763           |        20    |
| X-n411-k19  | 19712  |        1.141 |            1.157 |      13.67  |       3.759 | 3.579           |        20.5  |
| X-n420-k130 | 107798 |        0.622 |            0.613 |      13.97  |       2.635 | 2.577           |        20.95 |
| X-n429-k61  | 65449  |        0.861 |            0.87  |      14.27  |       2.609 | 2.584           |        21.4  |
| X-n439-k37  | 36391  |        0.382 |            0.348 |      14.6   |       2.887 | 2.814           |        21.9  |
| X-n449-k29  | 55233  |        1.338 |            1.302 |      14.93  |       3.095 | 2.980           |        22.4  |
| X-n459-k26  | 24139  |        1.634 |            1.6   |      15.27  |       4.658 | 4.614           |        22.9  |
| X-n469-k138 | 221824 |        1.697 |            1.685 |      15.6   |       4.577 | 4.517           |        23.4  |
| X-n480-k70  | 89449  |        0.671 |            0.647 |      15.97  |       2.45  | 2.388           |        23.95 |
| X-n491-k59  | 66483  |        1.138 |            1.029 |      16.33  |       2.409 | 2.285 2.654     |        24.5  |
| Mean        |        |        0.828 |            0.784 |      11.636 |       2.732 |                 |        17.45 |

would lead to a high-quality solution after local-search. In the future, we propose to explore the integration of ISC within other local-search algorithms.

influence the work reported in this paper.

ISC uses a Random-Forest classifier, which is a simple, efficient, and well-known machine-learning algorithm. Another future-work direction  could  exploit  the  progress  that  has  been  recently  made  on deep  learning,  such  as  (1)  deep  neural  networks,  (2)  graph  neural networks, or (3) deep reinforcement learning, to obtain classifiers with better performance. Finally, we demonstrated with the computational experiments  that  ISC  can  improve  the  performance  of  two  GRASP variants for the CVRP. Thus, ISC with a Random-Forest classifier is a component that can be easily integrated to any other MSM for the CVRP.

## CRediT authorship contribution statement

Juan Pablo Mesa: Writing  -  review  &amp;  editing,  Writing  -  original  draft,  Validation,  Methodology,  Investigation,  Formal  analysis, Data curation, Conceptualization. Alejandro Montoya: Writing - review &amp; editing, Validation, Supervision, Resources, Methodology, Funding acquisition, Conceptualization. Raul Ramos-Pollan: Writing - review &amp; editing, Supervision, Methodology, Investigation, Formal analysis, Conceptualization. Mauricio Toro: Writing - review &amp; editing, Supervision, Investigation, Funding acquisition, Conceptualization.

## Declaration of competing interest

The authors  declare  that  they  have  no  known  competing  financial  interests  or  personal  relationships  that  could  have  appeared  to

## Acknowledgments

The authors acknowledge supercomputing resources made available by  the  Universidad  EAFIT  scientific  computing  center  (APOLO)  to conduct  the  research  reported  in  this  work  and  for  its  support  for the computational experiments. The authors would like to thank Luca Accorsi for providing access to FILO code, and guidance on how to turn it into the simplified FILO0 variant. Additionally, the authors would like  to  thank  Alejandro  Guzmán and Sebastián Pérez for their help during the early stages of the heuristics implementation. Finally, the first author is grateful to Universidad EAFIT, Colombia for the Ph.D. Grant from project 974-000005.

## Appendix. Computational details of results on the 𝑿 dataset

The results in Tables A.1-A.3 show the Average Gap (%) obtained over ten executions on each instance of the 𝑋 dataset, along with the CPU time (min), for the metaheuristics G-FILO0, G-FILO0-ISC, G-BVND, and G-BVND-ISC.

## Data availability

Data will be made available on request.

Table A.3 Computational results on the large instances of 𝑋 dataset. * Corresponds to execution time in four parallel cores.

| INSTANCE    | BKS    |   G-FILO0(%) |   G-FILO0-ISC(%) |   TIME(min) |   G-BVND(%) |   G-BVND-ISC(%) |   TIME*(min) |
|-------------|--------|--------------|------------------|-------------|-------------|-----------------|--------------|
| X-n502-k39  | 69226  |        0.12  |            0.119 |      25.05  |       0.952 |           0.91  |        83.5  |
| X-n513-k21  | 24201  |        0.904 |            0.853 |      25.6   |       4.507 |           4.426 |        85.33 |
| X-n524-k153 | 154593 |        0.751 |            0.771 |      26.15  |       4.732 |           4.641 |        87.17 |
| X-n536-k96  | 94846  |        1.282 |            1.261 |      26.75  |       3.12  |           3.026 |        89.17 |
| X-n548-k50  | 86700  |        0.248 |            0.266 |      27.35  |       1.516 |           1.485 |        91.17 |
| X-n561-k42  | 42717  |        0.711 |            0.644 |      28     |       3.099 |           3.077 |        93.33 |
| X-n573-k30  | 50673  |        0.722 |            0.705 |      28.6   |       1.63  |           1.609 |        95.33 |
| X-n586-k159 | 190316 |        1.267 |            1.24  |      29.25  |       3.659 |           3.566 |        97.5  |
| X-n599-k92  | 108451 |        1.037 |            1.039 |      29.9   |       2.552 |           2.505 |        99.67 |
| X-n613-k62  | 59535  |        1.568 |            1.473 |      30.6   |       3.201 |           3.104 |       102    |
| X-n627-k43  | 62164  |        1.168 |            1.167 |      31.3   |       2.8   |           2.733 |       104.33 |
| X-n641-k35  | 63684  |        1.387 |            1.341 |      32     |       3.149 |           3.083 |       106.67 |
| X-n655-k131 | 106780 |        0.179 |            0.161 |      32.7   |       0.53  |           0.527 |       109    |
| X-n670-k130 | 146332 |        2.579 |            2.577 |      33.45  |       6.302 |           6.231 |       111.5  |
| X-n685-k75  | 68205  |        1.282 |            1.232 |      34.2   |       2.705 |           2.645 |       114    |
| X-n701-k44  | 81923  |        1.061 |            1.031 |      35     |       2.208 |           2.189 |       116.67 |
| X-n716-k35  | 43373  |        1.583 |            1.534 |      35.75  |       2.777 |           2.693 |       119.17 |
| X-n733-k159 | 136187 |        0.777 |            0.775 |      36.6   |       1.602 |           1.592 |       122    |
| X-n749-k98  | 77269  |        1.417 |            1.401 |      37.4   |       2.146 |           2.121 |       124.67 |
| X-n766-k71  | 114417 |        0.936 |            0.893 |      38.25  |       2.629 |           2.518 |       127.5  |
| X-n783-k48  | 72386  |        1.356 |            1.312 |      39.1   |       3.256 |           3.216 |       130.33 |
| X-n801-k40  | 73311  |        0.621 |            0.605 |      40     |       2.921 |           2.878 |       133.33 |
| X-n819-k171 | 158121 |        1.303 |            1.307 |      40.9   |       3.915 |           3.906 |       136.33 |
| X-n837-k142 | 193737 |        0.99  |            0.994 |      41.8   |       2.73  |           2.714 |       139.33 |
| X-n856-k95  | 88965  |        0.466 |            0.466 |      42.75  |       2.196 |           2.184 |       142.5  |
| X-n876-k59  | 99299  |        1.151 |            1.136 |      43.75  |       1.791 |           1.784 |       145.83 |
| X-n895-k37  | 53860  |        1.997 |            1.92  |      44.7   |       4.866 |           4.802 |       149    |
| X-n916-k207 | 329179 |        1.105 |            1.089 |      45.75  |       3.474 |           3.394 |       152.5  |
| X-n936-k151 | 132715 |        2.389 |            2.374 |      46.75  |       7.801 |           7.77  |       155.83 |
| X-n957-k87  | 85465  |        0.59  |            0.578 |      47.8   |       2.565 |           2.531 |       159.33 |
| X-n979-k58  | 118976 |        0.996 |            0.983 |      48.9   |       2.405 |           2.311 |       163    |
| X-n1001-k43 | 72355  |        1.945 |            1.907 |      50     |       4.043 |           3.978 |       166.67 |
| Mean        | Mean   |        1.121 |            1.099 |      36.128 |       3.056 |           3.005 |       120.43 |

## References

- [14] L.  Breiman,  Random  forests,  Mach.  Learn.  45 (1)  (2001)  5-32, http://dx.doi.org/ 10.1023/A:1010933404324.
- [1] F. Arnold,  K.  Sörensen,  What  makes  a  VRP  solution  good?  The  generation of  problem-specific  knowledge  for  heuristics,  Comput.  Oper.  Res.  106  (2019) 280-288, http://dx.doi.org/10.1016/j.cor.2018.02.007.
- [2] F.  Lucas,  R. Billot,  M.  Sevaux,  K.  Sörensen,  Reducing  space  search  in  combinatorial optimization using machine learning tools, in: International Conference on Learning and Intelligent Optimization, Springer,  2020,  pp.  143-150, http: //dx.doi.org/10.1007/978-3-030-53552-0\_15.
- [3] F. Arnold, K. Sörensen, Knowledge-guided local search for the vehicle routing problem,  Comput.  Oper.  Res.  105  (2019)  32-46, http://dx.doi.org/10.1016/j.cor. 2019.01.002.
- [4] F. Arnold, Í. Santana, K. Sörensen, T. Vidal, PILS: Exploring high-order neighborhoods  by  pattern  mining  and  injection, Pattern  Recognit.  116  (2021)  107957, http://dx.doi.org/10.1016/j.patcog.2021.107957.
- [5] G.B. Dantzig, J.H. Ramser, The truck dispatching problem, Manag. Sci. 6 (1) (1959) 80-91, http://dx.doi.org/10.1287/mnsc.6.1.80.
- [6] G.  Laporte,  S.  Ropke,  T.  Vidal,  Chapter  4:  Heuristics  for  the vehicle  routing problem, in: Vehicle Routing: Problems, Methods, and Applications, Second  Edition, SIAM,  Philadelphia, PA,  2014,  pp. 87-116, http://dx.doi.org/ 10.1137/1.9781611973594.ch4, arXiv:https://epubs.siam.org/doi/pdf/10.1137/ 1.9781611973594.ch4.
- [7] M. Nazari,  A.  Oroojlooy,  L. Snyder,  M.  Takac,  Reinforcement  learning  for  solving the vehicle routing problem, 31, 2018, pp. 9839-9849,
- [8] W. Kool,  H.  van  Hoof,  J.  Gromicho,  M.  Welling,  Deep  policy  dynamic  programming  for  vehicle  routing  problems,  2021, http://dx.doi.org/10.48550/arXiv. 2102.11756, arXiv preprint arXiv:2102.11756.
- [9] E.-G.  Talbi, Machine  learning  into  metaheuristics:  A  survey  and  taxonomy,  ACM Comput. Surv. 54 (6) (2021) 1-32, http://dx.doi.org/10.1145/3459664.
- [10] M. Karimi-Mamaghan, M. Mohammadi, P. Meyer, A.M. Karimi-Mamaghan, E.-G. Talbi, Machine learning at the service of meta-heuristics for solving combinatorial optimization problems: A state-of-the-art, European J. Oper. Res. 296 (2) (2022) 393-422, http://dx.doi.org/10.1016/j.ejor.2021.04.032.
- [11] T.A.  Feo,  M.G.  Resende,  Greedy  randomized  adaptive  search  procedures,  J. Global Optim. 6 (2) (1995) 109-133, http://dx.doi.org/10.1007/BF01096763.
- [12] N. Mladenović, P. Hansen, Variable neighborhood search, Comput. Oper. Res. 24 (11) (1997) 1097-1100, http://dx.doi.org/10.1016/S0305-0548(97)00031-2.
- [13] L.  Accorsi, D.  Vigo,  A  fast and  scalable  heuristic  for the  solution  of largescale capacitated vehicle routing problems, Transp. Sci. 55 (4) (2021) 832-856, http://dx.doi.org/10.1287/trsc.2021.1059.
- [15] A.K. Kalakanti, S. Verma, T. Paul, T. Yoshida, RL SolVeR pro: Reinforcement learning for solving vehicle routing problem, in: 2019 1st International Conference on Artificial  Intelligence  and  Data  Sciences,  AiDAS,  IEEE,  2019,  pp.  94-99, http://dx.doi.org/10.1109/AiDAS47888.2019.8970890.
- [16] B.  Peng,  J. Wang, Z. Zhang, A deep reinforcement  learning  algorithm  using dynamic attention model for vehicle routing problems, in: International Symposium  on  Intelligence Computation  and  Applications,  Springer,  2019,  pp. 636-650, http://dx.doi.org/10.1007/978-981-15-5577-0\_51.
- [17] A.  Delarue,  R. Anderson,  C. Tjandraatmadja,  Reinforcement  learning  with  combinatorial  actions: An application  to  vehicle  routing, in: H.  Larochelle, M. Ranzato, R.  Hadsell, M.F.  Balcan,  H.  Lin  (Eds.), in: Advances  in  Neural  Information Processing Systems, vol. 33, Curran Associates, Inc., Vancouver, Canada, 2020, pp. 609-620.
- [18] A.E. Gutierrez-Rodríguez, S.E. Conant-Pablos, J.C. Ortiz-Bayliss, H. TerashimaMarín, Selecting meta-heuristics for solving vehicle routing problems with time windows via meta-learning, Expert Syst. Appl. 118 (2019) 470-481, http://dx. doi.org/10.1016/j.eswa.2018.10.036.
- [19] H. Jiang, Y. Wang, Y. Tian, X. Zhang, J. Xiao, Feature construction for metaheuristic  algorithm  recommendation  of  capacitated  vehicle  routing  problems, ACM Trans. Evol. Learn. Optim. 1 (1) (2021) 1-28, http://dx.doi.org/10.1145/ 3447540.
- [20] A. Subramanian, E. Uchoa, L.S. Ochi, A hybrid algorithm for a class of vehicle routing  problems,  Comput.  Oper.  Res.  40  (10)  (2013)  2519-2531, http://dx.doi. org/10.1016/j.cor.2013.01.013.
- [21] V.R. Máximo, M.C. Nascimento, A hybrid adaptive iterated local search with diversification control to the capacitated vehicle routing problem, European J. Oper.  Res.  294  (3) (2021)  1108-1119, http://dx.doi.org/10.1016/j.ejor.2021.02. 024.
- [22] T. Vidal, T.G. Crainic, M. Gendreau, C. Prins, A unified solution framework for multi-attribute  vehicle  routing  problems,  European  J.  Oper.  Res.  234  (3)  (2014) 658-673, http://dx.doi.org/10.1016/j.ejor.2013.09.045.
- [23] J. Christiaens,  G.  Vanden  Berghe,  Slack  induction  by  string  removals  for  vehicle routing  problems,  Transp.  Sci.  54  (2)  (2020)  417-433, http://dx.doi.org/10. 1287/trsc.2019.0914.
- [24] S.  Kirkpatrick, C.D.  Gelatt Jr., M.P.  Vecchi, Optimization  by  simulated  annealing, Science 220 (4598) (1983) 671-680.
- [25] M.R. de Holanda Maia, A. Plastino, P.H.V. Penna, Hybrid data mining heuristics for the heterogeneous fleet vehicle routing problem, RAIRO- Oper. Res. 52 (3) (2018) 661-690.

- [26] R.S.  Sutton, A.G.  Barto, Reinforcement  learning:  an  introduction,  second  ed., The MIT Press, Cambridge, 2018.
- [27] Y. Wu, W. Song, Z. Cao, J. Zhang, A. Lim, Learning improvement heuristics for solving routing problems, IEEE Trans. Neural Netw. Learn. Syst. (2021) 1-13, http://dx.doi.org/10.1109/TNNLS.2021.3068828.
- [28] L. Gao,  M.  Chen,  Q.  Chen,  G. Luo,  N. Zhu,  Z. Liu, Learn  to design  the heuristics for vehicle routing problem, 2020, http://dx.doi.org/10.48550/arXiv. 2002.08539, arXiv preprint arXiv:2002.08539.
- [29] A. Hottung, K. Tierney, Neural large neighborhood search for the capacitated vehicle  routing  problem,  in: 24th European Conference on Artificial  Intelligence, ECAI 2020, 2020.
- [30] R.S. Sutton, D. McAllester, S. Singh, Y. Mansour, Policy gradient methods for reinforcement learning with function approximation, Adv. Neural Inf. Process. Syst. 12 (1999) 1057-1063.
- [31] P.H. Swain, H. Hauska, The decision tree classifier: Design and potential, IEEE Trans.  Geosci.  Electron.  15 (3)  (1977)  142-147, http://dx.doi.org/10.1109/TGE. 1977.6498972.
- [32] L. Grinsztajn, E. Oyallon, G. Varoquaux,  Why  do  tree-based models  still outperform  deep  learning  on  tabular  data?  2022,  arXiv  preprint arXiv:2207. 08815.
- [33] G.  Clarke,  J.W. Wright,  Scheduling  of  vehicles  from  a central  depot  to a number of  delivery points,  Oper. Res.  12 (4)  (1964)  568-581, http://dx.doi.org/10.1287/ opre.12.4.568.
- [34] J.P. Hart, A.W. Shogan, Semi-greedy heuristics: An empirical study, Oper. Res. Lett. 6 (3) (1987) 107-114, http://dx.doi.org/10.1016/0167-6377(87)90021-6.
- [35] P.  Hansen,  N.  Mladenović,  R.  Todosijević,  S.  Hanafi,  Variable  neighborhood search:  basics  and  variants,  EURO  J.  Comput.  Optim.  5  (3)  (2017)  423-454, http://dx.doi.org/10.1007/s13675-016-0075-x.
- [36] N. Hassan, W. Gomaa, G. Khoriba, M. Haggag, Supervised Learning Approach for  Twitter Credibility  Detection,  2018,  pp. 196-201, http://dx.doi.org/10.1109/ ICCES.2018.8639315.
- [37] L.  Breiman,  J.H. Friedman,  R.A.  Olshen,  C.J.  Stone, Classification  and  Regression Trees,  Chapman  and  Hall/CRC,  Boca  Raton,  FL,  1984, http://dx.doi.org/10. 1201/9781315139470.
- [38] E. Uchoa,  D.  Pecin, A.  Pessoa, M.  Poggi,  T. Vidal, A.  Subramanian,  New benchmark instances for the capacitated vehicle routing problem, European J. Oper. Res. 257 (3) (2017) 845-858, http://dx.doi.org/10.1016/j.ejor.2016.08. 012.
- [39] Boost C++ Libraries, 2022, URL http://www.boost.org/. (Accessed 15 January 2022).
- [40] Twigy random forest C++ and Python Library, 2022, URL https://github.com/ christophmeyer/twigy. (Accessed 16 March 2022).
- [41] L.  Accorsi, A. Lodi,  D. Vigo,  Guidelines  for the  computational  testing  of  machine learning approaches to vehicle routing problems, Oper. Res. Lett. 50 (2) (2022) 229-234, http://dx.doi.org/10.1016/j.orl.2022.01.018.
- [42] F.  Wilcoxon,  Individual  comparisons  by  ranking  methods,  in:  S.  Kotz,  N.L. Johnson (Eds.), Breakthroughs in Statistics: Methodology and Distribution, Springer New York, New York, NY, 1992, pp. 196-202, http://dx.doi.org/10.1007/9781-4612-4380-9\_16.