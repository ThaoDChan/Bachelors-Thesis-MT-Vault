![Image](image_000000_22550b9d19c8cdc3188a700f0badb5edf8d2efd8e6640eef1f2d18a591ac6b28.png)

## Learning to Select Initialisation Heuristic for Vehicle Routing Problems

## Joao Guilherme Cavalcanti

## Yi Mei

## Mengjie Zhang

## Costa

joao.costa@ecs.vuw.ac.nz Victoria University of Wellington Wellington, New Zealand yi.mei@ecs.vuw.ac.nz Victoria University of Wellington Wellington, New Zealand

## ABSTRACT

mengjie.zhang@ecs.vuw.ac.nz Victoria University of Wellington Wellington, New Zealand

## 1 INTRODUCTION

The Vehicle Routing Problem (VRP) is a complex problem that comes with a great number of applications in logistics and supply chains. It is non-trivial to select the optimal VRP techniques to solve these applications, especially since there are several possible scenarios. As there is no way to predict how each algorithm would perform until it is (at least partially) deployed, it would make sense in selecting the ones that have higher adaptability to the given environment. In this paper, we consider this idea on the initialisation part of a local search-based metaheuristic. We argue that a proper initialisation is important for obtaining better VRP solutions and apply several machine learning techniques aiming to learn how to use distinct features from four commonly used construction heuristics solutions, predicting the scenarios in which they are the most effective. We also provide relevant discussions on the effects of the initial solution on a local search context. Results show that the proposed method can help select the best or an improving method for the majority of the instances considered, especially for large-scale VRP instances.

## CCS CONCEPTS

- · Applied computing → Transportation ; · Theory of computation → Optimization with randomized search heuristics ; Routing and network design problems .

## KEYWORDS

Vehicle Routing Problem, Machine Learning, Initialisation Heuristic.

## ACMReference Format:

Joao Guilherme Cavalcanti Costa, Yi Mei, and Mengjie Zhang. 2023. Learning to Select Initialisation Heuristic for Vehicle Routing Problems. In Genetic and Evolutionary Computation Conference (GECCO '23), July 15-19, 2023, Lisbon, Portugal. ACM, New York, NY, USA, 9 pages. https://doi.org/10.1145/ 3583131.3590397

Permission to make digital or hard copies of all or part of this work for personal or classroom use is granted without fee provided that copies are not made or distributed for profit or commercial advantage and that copies bear this notice and the full citation on the first page. Copyrights for components of this work owned by others than the author(s) must be honored. Abstracting with credit is permitted. To copy otherwise, or republish, to post on servers or to redistribute to lists, requires prior specific permission and/or a fee. Request permissions from permissions@acm.org.

GECCO '23, July 15-19, 2023, Lisbon, Portugal

© 2023 Copyright held by the owner/author(s). Publication rights licensed to ACM.

ACM ISBN 979-8-4007-0119-1/23/07...$15.00

https://doi.org/10.1145/3583131.3590397

The Vehicle Routing Problem (VRP) is one of the most studied combinatorial optimisation problems for the past few decades [20]. The VRP has a wide range of applications in logistics scenarios, such as the distribution and delivery of goods by large companies, but also in day-to-day life maintenance, such as in rubbish or recycling collection and even military applications [37]. The VRP consists of finding optimal routes (i.e. with minimal cost/distance) for a set of customers distributed across a geographical area.

In literature, the size of the instances being solved is often not practical for real-world scenarios since they can be quite small [1] or have quite different geographical distributions, such as being more densely located [31]. Hence, methods that were developed around non-real-world scenarios might struggle to solve the real-world ones, such as the one argued in [31]. The argument is based on the work of [7] (for the VRP with Time-Windows, a slightly different variant from the CVRP which also adds specific time-windows for the customers to be visited), where, although their method was performing very well for the benchmark instances, when tested in real-world instances it suddenly was much slower, taking up to several hours to run. This was because the real-world instances had a customer distribution very different from the ones in the benchmarks. This is not desirable in real-world applications, as it could lead to unexpected delays or behaviours. An employed method should perform well across different configurations, avoiding the described scenario.

The performance of algorithms depends on the problem characteristics such as graph size and topology, and it is hard to manually design effective algorithms for the given type of problem to be solved. There is no single technique that would work the best for all cases, as per No-Free Lunch theorem [40]. Therefore, each existing method is theoretically bound to perform better in specific situations. In this case, Machine Learning (ML) can help automatically identify those cases by learning from training data. The ML techniques would then learn when to apply each method, similar to how Hyper-Heuristics work [8, 33].

Learning to design algorithms is not new in the field of Operations Research or the VRP, but still, there are many challenges regarding their applications, which can be noticed by the majority of methods being heuristic-based (approximation techniques based on the problem's knowledge [32]). When considering supervised learning algorithms, they typically require a lot of data to be trained. For example, most existing methods require optimal solutions, which are used as the target in a supervised learning manner. It is, however, often unpractical to obtain the optimal solutions with existing techniques due to being too expensive to

compute. This is especially true for large-scale instances, where optimal approaches still struggle with a few hundred customers [2, 35]. Another issue comes from the data not being available for the scientific community (if considering real data only).

Because of these challenges and also due to the historical trends in the development of VRP methods, most successful methods utilise Local Search in solving VRP [2]. Starting from an initial solution, a local search algorithm will keep exploiting some (or all) of the neighbouring solutions of the current solution and move to a neighbour until some stopping criteria are met. The local search needs a starting point - which is not heavily considered as it is thought to provide little gain when compared to high-performing improvement techniques. What ends up happening is that a generic construction technique will be used based on just its overall quality or the designer's affinity to that heuristic. However, there also seems to be a correlation between the starting point and the quality of the local optima [26]. Hence, we question whether the importance of this step of the search process should be reconsidered.

In this paper, we analyse how the initial point of an individualbased search can affect its development and we apply several ML techniques to match the best-performing ones. The main idea behind this is to minimise the dependency on an expert's bias towards specific construction algorithms, while also helping us understand a bit more about how the heuristics and the instances are correlated. We utilise the Knowledge-Guided Local Search (KGLS) [2] as our local search method since it has shown very good performance in several instances, especially considering large-scale ones outperforming most existing methods. We apply ML techniques such as Random Forest, Decision Tree and Support Vector Machines, which are used to learn instance and solution characteristics from 4 different initialisation heuristics: the KGLS' adaptation of the savings heuristic [1] (CW100), as well as the original Clark and Wright's savings heuristic [10] (CW), Nearest Neighbour [4] (NN), and Sweep [15, 41]. We extract a comprehensive set of features from the solutions generated by these initialisation heuristics and feed them into the ML training process. The ML method will then classify the instances based on pre-labelled classes, which are labelled based on KGLS' performance considering the four initial heuristics. We then apply the trained model to predict unseen instances.

The rest of the paper is organised as follows: Section 2 presents the related literature. Section 3 is a preliminary analysis showing the impact of the initial solution on the VRP search. Section 4 presents our methodology and the ML approaches used for automating the heuristic selection. Experiments details are given in Section 5. Results and discussions are given in Section 6, followed by the conclusions and final remarks in Section 7.

## 2 RELATED WORK

## 2.1 Vehicle Routing Problems

The VRP was first introduced in [12] as a way to deliver fuel across a city. The traditional VRP can be represented by a graph 𝐺 = ( 𝑉, 𝐸 ) , where the set of vertex 𝑉 = { 0 1 2 , , , ..., 𝑛 } represents the customers, each with a demand 𝑞 𝑖 that needs to be fulfilled. There is a special point, usually the node 0, which is the depot, from where all vehicles start and end their routes. The set of edges 𝐸 = {( 𝑖, 𝑗 ) ∈ 𝐺,𝑖 ≠ 𝑗 } expresses the edges between all points and the connections or paths among the customers' pairs. The graph is often considered to be fully connected. A solution for the VRP needs to contain a set of routes that visit every customer, attending to every demand without exceeding the vehicle's capacity Q. The overall goal is to find the routes which minimise the total distance travelled.

This problem falls into the NP-hard category [21]. Hence, it is more often to try to tackle it by using heuristic methods, as exact approaches still struggle to solve medium to large instances. Arguably the most common approaches for solving VRP use local search as their search engine, especially for local improvement. Local search has been used successfully in solving the VRP, such as the Iterative Local Search (ILS) [34], the Adaptive Large Neighbourhood Search (ALNS) [28], the Guided Local Search [38, 39] and the Knowledge-Guided Local Search (KGLS, which will be detailed in the next subsection) [2]. Even in population-based methods, local search had to be applied to find success, such as the Ant-Colony Optimisation (ACO) [42], Genetic Algorithm (GA) in the works of [29] and [36]. More on the VRP and its variants can be found in the reviews [20], [13], [37] and [6].

## 2.2 The KGLS and ML for the VRP

The KGLS is a recent local search-based method that was able to find exceptional success. It considers a new solution characteristic, the width of a route, in its penalisation step. Apart from this novel use of the width feature for changing the objective function, the KGLS also admits several improvements regarding the handling of the large search space, such as pre-evaluating the possible moves, setting up a deterministic strategy for the moves and by limiting the number of customers to be searched in a move. Although there are some downfalls to such methodology, as pointed out in [11], the metaheuristic still performs very well across diversified scenarios, proving to be robust, especially considering its execution time [1]. The success of this heuristic also depends largely on the chosen components of the local search step, including the efficient LKH [17, 24] for intra-route, and the Cross-Exchange and Relocation Chain for inter-route. For full details on the KGLS please refer to [1] and [2].

Recent literature has also given ML increased attention to solving VRP. Such as the work of [43], where Deep Reinforcement Learning (DRL) is used to learn how to generate solutions for VRPs with the use of routing policies. These initial routes outperform other constructive heuristics (including the CW and NN, used in this work), finding (although marginally) better final solutions across small and medium instances, even when considering different improvement steps afterwards. They still present lower quality to these constructive heuristics for some instances, which agrees with the No Free-Lunch Theorem already mentioned. Similarly, in the work of [25], the authors utilised Reinforcement Learning with a recurrent neural network to learn how to create VRP solutions. They have similar drawbacks as the previously mentioned work, being a bit less competitive. These, however, are relatively new ways to apply to solve the VRP, and should be treated as the first stepping stones in this mix.

Other than solving the VRP, some recent work used ML methods to help or configure existing methods. For example, in [9] the authors consider a supervised learning classification approach with

Genetic Programming (GP) for the Large-Scale variant (LSVRP, when an instance has at least 100 customers [14]), aiming to increase the efficacy of the KGLS's penalisation phase. The work of [30] proposes feature extractors to be used for VRP instances and for the configuration of metaheuristics. Finally, the work of [23] transforms the VRP into a regression problem to learn how to improve the solutions in the local search stage. A recent review on the use of ML in metaheuristic contexts is found in [19], and for ML for combinatorial optimisation in [5]. The survey of [18] shows how ML (among other techniques) is used for automatic parameter tuning in metaheuristics.

The presented works show some of the recent advances in the use of ML for solving combinatorial optimisation problems - or in their assistance. However, we believe solving the VRP as a whole is a task too complex for the current strategies applied. As most methods still rely on the use of local search for improvement and efficiency, we believe that ML has more success when used to improve components of existing local search-based methods. For example, the work of [43], especially, indirectly confirms that the initial solution plays a role in the performance of the local search methods by showing that the more advanced RL method proposed still fails to beat the basic constructive heuristics in certain scenarios. Next, we present a discussion on this effect and its repercussions.

## 3 THE INITIAL SOLUTION IMPACT

Although it is already known that the starting point affects the performance of the local search-based methods (as shown in [26]), there is little evidence of how much it is affected in the recent literature for the VRP. Most local search-based methods utilise a simple constructive heuristic to build the initial solution, since they are mostly very fast (generally less than one second, even for largescale instances), and focus on the improvement phase which is more expensive (minutes to hours of execution). We present some preliminary experiments on the impact of the initial search and argue their relevance. The results presented here were obtained by changing only the algorithm utilised in the construction of the initial solution.

The KGLS originally uses a modified version of the savings heuristic introduced in [10]. Clarke and Wright's savings heuristic (CW) builds the solution by starting with trivial routes ( 𝑛 vehicles serving the 𝑛 customers in back-and-forth routes), they are then merged at each following iteration maximising the loss of overall distance when connecting them, if feasible, based on a savings table (hence it is popularly known as the savings heuristic). The modified version introduced in [1] works about the same way, but it is optimised for large instances, where the savings table only holds the 100 closest customers (hence we use the acronym CW100), to avoid a very large (memory-wise) table to be recorded. Although this modification limits the search space, it was shown to be almost as good as the original CW heuristic for a lot of instances. We confirm this similarity in our results, but also argue that the original CW might still be a valid heuristic to be considered even in large-scale instances, as memory limitations tend to be overcome by technological advances.

Apart from these two methods, we also utilise other two popular approaches that are used in VRP as constructive heuristics: the

Nearest Neighbour (NN) and the Sweep heuristic (SWEEP). The first was originally introduced in [4] for the Travelling Salesman Problem but was then adapted for the VRP. The idea is simple, starting from the depot, select the nearest customer and add to the first's vehicle route. Next, add the closest customer from the first customer and repeat the process until the vehicle's capacity is reached, skipping the closest customers which do not allow for a feasible addition (whose demand added would exceed the vehicle's total capacity load). Then, the next vehicle is chosen while selecting the closest unvisited customer from the depot, repeating the process as the previous vehicle. This greedy heuristic tends to perform very badly when compared to other constructive heuristics since it does a poor job of accounting for the next routes.

The Sweep heuristic [15, 41] builds the routes by considering an initial angle and 'sweeping' a line segment across the customers starting a new route whenever the vehicle is full. This method attempts to put the customers that are close to each other (considering the angulation from the depot, forming sort of 'petals') in the same route. The routes generated are usually not very effective, as the method also suffers from the greediness of maximising the vehicles' load. An example of the solutions generated by the four constructive heuristics for the X-n176-k26 instance and their total costs are shown in Figure 1.

Table 1: The costs given by the different initialisation heuristics for some representative instances. The average represents the value over the 100 instances of [35].

| Instance    | Topology   |    CW100 |       NN |       CW |   SWEEP |
|-------------|------------|----------|----------|----------|---------|
| X-n125-k30  | C          |  59659   |  69054   |  59659   |   73647 |
| X-n157-k13  | C          |  17831   |  18979   |  17831   |   26418 |
| X-n172-k51  | RC         |  48228   |  64976   |  48228   |   69382 |
| X-n233-k16  | RC         |  20433   |  30950   |  20433   |   42959 |
| X-n247-k47  | C          |  40870   |  52358   |  40870   |   61692 |
| X-n367-k17  | C          |  25348   |  28746   |  25343   |   64245 |
| X-n384-k52  | R          |  69524   |  74913   |  69526   |  128751 |
| X-n420-k130 | RC         | 112317   | 140018   | 112604   |  147419 |
| X-n469-k138 | R          | 235279   | 242364   | 234913   |  330625 |
| X-n524-k137 | R          | 166509   | 217803   | 165536   |  240520 |
| X-n641-k35  | RC         |  68128   |  71182   |  67988   |  214738 |
| X-n670-k126 | R          | 158564   | 206695   | 158545   |  270490 |
| X-n749-k98  | C          |  79313   | 105117   |  79698   |  158132 |
| X-n837-k142 | RC         | 201348   | 207337   | 201119   |  355154 |
| X-n979-k58  | C          | 123528   | 143373   | 123224   |  328968 |
| Average     | -          |  66395.9 |  76384.2 |  66352.2 |  129836 |

For the analysis, we first ran the above four constructive heuristics over the 100 instances of the "X" VRP dataset [35] 1 . This dataset provides instances with different customer distributions (network topologies), such as Clustered (customers can be easily grouped), Random (customers are spread randomly) and the mix RandomClustered (RC, where both clusters and random customers are present). As can be seen in Table 1, it is very clear that both CW and CW100 perform much better than the other 2 heuristics (CW wins overall by a bit). Although the table is showing only a few randomly selected instances, the averages presented are for all 100 instances from the dataset, which are not fully presented due to space limitations. There is also no clear pattern on which topology

1 The instances' name represented as "X-n -k 𝑖 𝑗 " are a code for their number of nodes ( 𝑖 ) and the minimum number of vehicles ( 𝑗 ).

![Image](image_000001_71a2a07cc989f7720f74bdaf42f0a70c88b0a7b3084533441887345d1263c432.png)

(a)

CW100 - 52570

![Image](image_000002_300ba2d426f64c55a62c97d11d01bc6c8702df1f40a27fc1756a0bcda24df62f.png)

X-n176-k26.vrp

![Image](image_000003_783d2b5570a4dfa87215483247376b67835b09c49573585784a8c448c8659630.png)

![Image](image_000004_febd94b33d010bf5f0d79ee4c5e927d9f739ef5bbd8ae51127244e6bebe59b12.png)

X-n176-k26.vrp

Figure 1: Example of routes created by the 4 heuristics considered - for instance X-n176-k26: (a) CW100 (with cost 52570) (b) CW(with cost 52551) (c) NN (with cost 66134) (d) Sweep (with cost 95138)

is preferred for each instance. Based on this table, one might expect the same distribution for the solutions' quality after the local search phase, as both CWs are leading the "race" by quite a big margin.

However, as Table 2 shows, that is not true. The table presents a summary of the instances from the "X" dataset (the same instances from the previous table 1 are shown, with averages from all 100 instances as well), after 10 runs 2 of 15 minutes of the KGLS algorithm with each initial heuristic. The Best represents the best improving GAP to the default mode (CW100). A fixed time approach was selected rather than a number of fixed iterations as these are dependent on the instance being solved, while time is a criterion that is employed in real-world applications. Surprisingly, the NN heuristic actually has a better overall final value over the 100 instances. Considering it is much worse in the initial stage at around 15% higher cost, the starting point was able to overcome the large gap and provide a final better solution on 34 instances (52

2 As the KGLS is deterministic, unlike the original GLS, and so are the selected heuristics, there is no need for statistical analysis on these runs. We still run it 10 times to account for possible oscillations in computer performance.

if including the ones where it matches the CW100). Perhaps even more surprising is the Sweep heuristic, which starts at an enormous 95% GAP to the CW100 and still was able to finish by a mere 0 04% . higher cost in the same time frame. The topology also does not seem to have any influence on this, just like in the initial solution. All constructive methods were able to perform the best for at least 13 instances. These additional statistics are shown in Table 3.

Table 3 also shows why just choosing a fixed option such as the Nearest Neighbour (or any other in particular) is a sub-optimal strategy. The table shows that although overall the performance can be better (improvement of up to 0 11%), it would also hinder a lot of . solutions (39 out of 100 which would have an average worsening of 0 15%, if considering the NN). This agrees with the No-Free Lunch . theorem [40] since it is clear that some instances are better solved by different methods, but also perform worse for others.

One might argue that it is much easier to find improvement in a worse solution than for an already good one. Although that is true, it would not explain how the worse solution ended up being better. A counterargument for that could be: an already good solution leads to

Table 2: The results obtained by KGLS with different initial solutions on some representative instances. The average represents the value over the 100 instances of [35]. Column Best represents the largest improving GAP to CW100.

| Instance    | Top.   |    CW100 |       NN |       CW |    SWEEP | Best   |
|-------------|--------|----------|----------|----------|----------|--------|
| X-n125-k30  | C      |  56167.6 |  56144.5 |  56169.5 |  56129.3 | -0.07% |
| X-n157-k13  | C      |  16876   |  16876   |  16876   |  16876   | 0.00%  |
| X-n172-k51  | RC     |  45664   |  45789.8 |  45664   |  45643   | -0.05% |
| X-n233-k16  | RC     |  19360   |  19363.6 |  19360   |  19359.9 | 0.00%  |
| X-n247-k47  | C      |  37701   |  37662   |  37685   |  37701.6 | -0.10% |
| X-n367-k17  | C      |  23442.8 |  22898.3 |  23415.5 |  23033   | -2.32% |
| X-n384-k52  | R      |  66585   |  66599   |  66475   |  66490   | -0.17% |
| X-n420-k130 | RC     | 108374   | 108371   | 108425   | 108379   | 0.00%  |
| X-n469-k138 | R      | 223403   | 224030   | 223263   | 224154   | -0.06% |
| X-n524-k137 | R      | 157292   | 156046   | 157716   | 157529   | -0.79% |
| X-n641-k35  | RC     |  64175   |  64302.5 |  64223   |  64444   | 0.00%  |
| X-n670-k126 | R      | 152500   | 149961   | 152206   | 152103   | -1.67% |
| X-n749-k98  | C      |  78365   |  78344.5 |  78273   |  78258   | -0.14% |
| X-n837-k142 | RC     | 195403   | 195259   | 195393   | 195419   | -0.07% |
| X-n979-k58  | C      | 120061   | 119890   | 119794   | 119855   | -0.22% |
| Average     | -      |  63699.1 |  63630   |  63688.1 |  63721.5 | -0.16% |

a local optimum much faster, and escaping it could be harder for the selected local search, hence the initialisation methods that produce worse initial solutions are less bound to such traps. However, if that is the case, and given the complexity of the search space of a problem such as the VRP, it would be also equally easy to trap the worse initial solutions in their own bad local optimums, not allowing for a better solution to be found given the same operators. In fact, given that this experiment is bound in time (all heuristics run for 15 minutes), we can argue that the time lost to make the obvious improvements on the worse initial solutions would make up for the best initial solutions escaping their local optimum. Therefore, there should be other factors contributing to such differences.

This leads us to believe that there are some characteristics in each initial set of routes that allow for the local search method to find an "easier" path to a better final solution. If such characteristics exist, they could be extracted and, therefore, a machine learning method could be used to learn their patterns. Doing so would allow for the local search methods to be tuned for the given instance based on the extracted characteristics of these easy (and fast) to compute heuristic methods. Although the gains are potentially small, learning which initial method to be used can provide a gain that costs very little, as there is no need to modify the underlying complex local search method and the training process happens offline. This concept could theoretically be applied to systems already deployed with minimal effort (if the initial solution process is detached from the main core of the search).

Table 3: A summary of additional statistics for the preliminary experiments over the 100 instances of [35].

|                                              | CW100   | NN     | CW     | SWEEP   |
|----------------------------------------------|---------|--------|--------|---------|
| Total Best                                   | 28      | 34     | 16     | 22      |
| Avg. GAP to CW100                            | -       | -0.11% | -0.02% | 0.04%   |
| No. of instances worse performing than CW100 | -       | 48     | 38     | 57      |
| Avg. GAP when worse                          | -       | 0.13%  | 0.16%  | 0.17%   |

## 4 LEARNING TO INITIALISE

In this section, we discuss how we extract the routes' characteristics from the different initial constructive heuristics considered, the instances' characteristics and how we use them to predict which method to be used for unseen instances. The biggest challenge here is to determine which features are relevant for such a task. The features proposed in [3] were applied successfully to distinguish good and near-optimal solutions, so they were the primary candidates for our approach. At first glance, however, some features would also make sense for this task, such as the number of routes used in each method and the total solution cost (which are not features proposed in [3]). Therefore, we also added those two additional features. A brief description of the features is presented in Table 4, where we divide them based on whether they are specific to the solutions built by the heuristics, or to the instance-specific ones, which are used as a form of normalisation, as they do not change between the heuristics. For more details on the other features the reader can follow [3]

In order to learn the relationship between these features and the final solution's class we need a classification algorithm. Since it is not in the scope of this work to find the most suitable classification algorithm for this task, we utilise several methods in order to avoid bias. We utilise all default implementations and default parameters of the Scikit python library [27], which includes the following:

- · Random Forest (RF)
- · Linear Support Vector Machines (SVM)
- · Decision Tree (DT)
- · Multi-layer Perceptron (MLP)
- · K-Nearest Neighbour (KNN)
- · Gaussian Process (GPC)
- · RBF Support Vector Machines (RBF)
- · Ada Boost (ABC)
- · Gaussian Bayes Network (GNB)
- · Quadratic Discriminant Analysis (QDA)

Having the features and the ML methods, we still need one final element for training them: the labelled data. Although there might not be an exact solution since these are approximation algorithms, we still believe their difference in performance (as shown in Section 3) is not by chance, but rather their characteristics that lead to specific regions of the solution search space. Therefore, we utilise the difference in performance in a 15 minutes run as the label for each class. For example, the instance "X-n469-k138" has the best performing solution starting with the CW heuristic, hence its class would be labelled as CW (we can easily convert these categories to numerical values). We do the same labelling for all instances, and, in case of ties (such as instance "X-n157-k13", which all heuristics find the best solution) the instance gets labelled with the default KGLS method (CW100). Hence, the number of instances in each class is the same as presented in Table 3. Then, we perform a random split on the data. We use 70% of the data for training, and the remaining 30% for testing.

Given the labelled data, we can now train our ML methods and learn how to select the initialisation heuristic. The training and test processes are shown in Figure 2. For each training instance, first, we extract the instance-specific features. Then, we apply the 4 methods (CW100, CW, NN and SWEEP) to the instance to generate the

Table 4: Solution-specific ( ∗ ) and Instance-specific ( + ) features. All features are proposed in [3], except for Cost and NTS.

| Feature   | Description                                                    | Feature   | Description                                                      |
|-----------|----------------------------------------------------------------|-----------|------------------------------------------------------------------|
| ∗ Cost    | Total cost                                                     | ∗ NTS     | No. of vehicles/trucks used                                      |
| ∗ S1      | Avg. number of intersections per customer                      | ∗ S6      | Average span in radian per route                                 |
| ∗ S2      | Longest distance between two connected customers per route     | ∗ S7      | Average compactness per route, measured by width                 |
| ∗ S3      | Average distance between depot to directly-connected customers | ∗ S8      | Average compactness per route, measured by radian                |
| ∗ S4      | Average distance between routes centres of gravity             | ∗ S9      | Average depth per route                                          |
| ∗ S5      | Average width per route                                        | ∗ S10     | Standard deviation of the number of customers per route          |
| + I1      | Number of customers                                            | + I5      | Standard deviation of the pairwise distance between customers    |
| + I2      | Minimum number of routes                                       | + I6      | Average distance from customers to the depot                     |
| + I3      | Degree of capacity utilisation                                 | + I7      | Standard deviation of the distance from customers to the depot   |
| + I4      | Average distance between each pair of customers                | + I8      | Standard deviation of the radians of customers towards the depot |

![Image](image_000005_5f86fb682e86a0a38ccdfda9ab30db30ec016089eb18403f97209eb58092abc7.png)

with distinct: depot locations (either at a Random location, at the Centre or at a Corner), customer demand distribution (either the same demands or varying them, and also with different ranges), number of vehicles (from small to large) and number of customers (ranging from 100 to 1000). This dataset provides sufficient diversity of features when applying our methodology. The dataset is randomly split into 70%/30% for training and testing, respectively. Additionally, we perform the training and test on 30 independent random data splits, allowing us to measure the robustness of the features across distinct data splits. As mentioned in Section 4, all the 100 instances were labelled by running the KGLS for 15 minutes and selecting the best-performing initialisation heuristic.

Figure 2: An overall flowchart for the: (a) Training phase (b) Test/Deployment phase

corresponding solutions and extract the solution-specific features. These are fed into the ML method of choice which will predict the output considering the labelled instance class as the expected output. After the training, we obtain the best classifier for selecting the best initialisation heuristic. Then we move on to the test phase. During the test phase, the 4 methods are first applied to the test instance. From each of the 4 solutions, we extract the solutionspecific features, which are used in the ML prediction step. The ML output will be matched to the generated label (as specified above) and we can calculate the accuracy.

In the case of deployment of the method for a real operation, which would include unseen and unlabelled data, the ML would still output a predicted initialisation heuristic, which would then be used by the KGLS as its starting solution, moving on to the improvement phase. It is important to note that this process is feasible due to the heuristics used being incredibly fast, allowing us to collect all these features within the first few seconds of execution, and not damaging the overall performance of the algorithm across most scenarios (which we assume would require the algorithm to run for more than a couple of minutes).

## 5 EXPERIMENTS

As already mentioned, the dataset utilised for the experiments is the 100 "X" instances from [35]. Apart from the different topologies as already mentioned in Section 3, this dataset has instances

The main drawback of these experiments is the lack of additional instances and not be able to guaranteeing balanced data, as the number of instances of each class is also imbalanced. However, we are satisfied with the diversity of the approach and its ability to match the initial heuristic. The results are detailed in the next section. It is important to note that the accuracy of the results is not of the type "correct-or-wrong". This is because selecting the wrong class does not mean the solution will be worse due to the existence of ties. For example, in an instance where CW100 and CW have the same final solution, its label would be CW100, but selecting CW would provide the same solution quality.

## 6 RESULTS AND DISCUSSIONS

In this section we explore the results of the ML test, comparing them to the use of a single heuristic. As can be seen in Table 5, which shows one single test run (out of the 30) to exemplify the data, the accuracy for some methods can be quite low. However, when looking at the overall costs, most ML methods were able to learn how to select initialisation heuristics with some degree of success. When comparing to the baseline (i.e. when utilising the default approach of only using one fixed initialisation heuristic, the CW100), most of the 10 methods were able to learn how to improve it, with the exception of GPC, for several of the runs. We also compare these predictors to the optimal selection, i.e. if all best heuristics were to be selected perfectly. Several methods were just barely worse, even with very low accuracy, indicating that the learning strategy was able to succeed.

To verify that these values are not random, Figure 3 summarise the performance of the chosen methods over the 30 runs. The first violin-plot (Figure 3 (a)) presents the accuracy, which varies quite a lot, but also shows that KNN seems to perform the best, followed by the RBF-SVM and MLP. Both linear-SVM and GPC also are able

Table 5: Results for all the considered ML methods and their respective costs for one of the runs with 30 randomly selected test instances. They are also compared to the default mode (using only CW100) and to a theoretical 100% accuracy method.

| Method                      | CW100   | Best possible   | RF       | DT       | SVM      | KNN      | MLP      | GPC      | RBF      | ABC      | GNB      | QDA      |
|-----------------------------|---------|-----------------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|
| Accuracy                    | -       | 100%            | 43.33%   | 43.33%   | 33.33%   | 60.00%   | 33.33%   | 16.66%   | 53.33%   | 46.66%   | 26.66%   | 46.66%   |
| Average Cost                | 85466.8 | 85192.8         | 85430.2  | 85446.5  | 85230.1  | 85228.2  | 85236.2  | 85448.9  | 85234.1  | 85243.3  | 85458.7  | 85240.5  |
| Difference to Default       | -       | -0.3206%        | -0.0428% | -0.0237% | -0.2769% | -0.2792% | -0.2699% | -0.0210% | -0.2723% | -0.2616% | -0.0095% | -0.2648% |
| Difference to best possible | 0.322%  | -               | 0.279%   | 0.298%   | 0.044%   | 0.041%   | 0.051%   | 0.301%   | 0.048%   | 0.059%   | 0.312%   | 0.056%   |

![Image](image_000006_4eb1a5d921e2e7810e03ad77e8bf27625eeca6497516efa5ac0655eac6ed092d.png)

(a) Accuracy

(b) Gap to optimal classification

![Image](image_000007_08b37ea472b79aa7aef5cd13a49334defb905d93ed2113871a6958aa539b0702.png)

Figure 3: The overall performance of the ML methods over the 30 runs by two different statistics: (a) Accuracy (b) GAP in solution cost to optimal classification.

to find good peaks, but fail to generalise well, having a very large variance, although this can be argued for most methods.

When looking at their ability to select good heuristics (i.e. not the best, but still better or equal to the baseline), we see a slightly different trend. The second violin plot (Figure 3 (b)) presents this information, with the GAP to the best possible selection. Here we also add the baseline performance (as CWH) - which shows how the default strategy would miss on quality over the different splits of the data, and the additional modes (CW, NN and SWP), in order to verify whether it is worth to select a fixed mode. In the figure, we can see that most methods are better than the baseline. They also show consistency, as a lot of them do not present a big variation, meaning they are likely miss-classifying instances with either one of the same quality or with the second-best heuristic. Here we notice that KNN also performs the best, but this time followed by the linear-SVM as well as the RBF. Although a few outliers exist, these methods still have a higher chance of finding a heuristic selection that improves over the baseline. A notorious exception, when compared to fixed strategies, can be seen in the Nearest Neighbour. It shows a good performance overall and with less variance than other heuristics.

## 6.1 Further testing and feature analysis

To verify the generability of the applied learning, since we have a limited number of instances to train, we also test the ML methods in both Li [22] and Golden [16] datasets. As they have very different characteristics from the "X" VRP dataset, we can validate if the features used are enough to differentiate the initialisation heuristics performance. These datasets vary by the number of customers from 240 up to 1200, although having a smaller number of instances (12 for the Li dataset and 20 for the Golden one) 3 . We test with all the 32 instances after each training split from the previous experiment.

As for testing the algorithms in this new data, we show the results in Figures 4 (a) and (b). Accuracy-wise, we see a different behaviour, where most methods have a higher classification accuracy. The most notable difference is the drop in performance from the KNN method, while the MLP and SVM actually improve their performance. Performance-wise, we see a different picture from the first test. Here, most methods struggle to outperform the baseline. However, it is important to note that the lack of instances and how closely their solutions are, play a big role in this performance. For example, the difference between the best-performing mode, the baseline CW100, and the second-best, CW, is only 0 002%, indi-. cating that they are almost the same in the 10 minute run. These results, however, do not discourage our findings, as they still show some insights into initialisation heuristics behaviour.

We also look into the features for more clarification on the ML behaviour. For this, we look into Random Forest's ability to rank the importance of the features. We show the top 15 features in Figure 5 for the RF's best run. We can see that the solution features are playing a bigger role in the classification (which is expected from the showings in [3]), but varying between the heuristics. As can be seen, the two features introduced (cost and NTS) also seem to be relevant in distinguishing the methods' performance. These features make

3 The instances have the following best initialisation heuristic in our 10 minutes run test: CW100 -9, CW - 1, NN - 2, SWEEP - 0, for the Li dataset; CW100 -9, CW 2, NN - 7, SWEEP - 2, for the Golden dataset.

![Image](image_000008_b7e47ea3f4da1a7e83dc4c85aa7e694f3effde1a96f95dccf869d77d7c9006ce.png)

(a) Accuracy

![Image](image_000009_4746932ad20b184ba20d0f25b28c26391f774559066a7cf85b325ed09c36fa4c.png)

(b) Gap to optimal classification

Figure 4: The overall accuracy and performance of the ML methods over the 30 runs considering the Li and Golden datasets.

sense as they are often quite different between methods (as clearly seen in Figure 1). The only instance characteristic that showed in the top 15 was the average distance to the depot, which can indicate the density of the instance can be decisive for normalising the solution's characteristics.

## 7 CONCLUSIONS AND FUTURE WORK

In this paper, we argue that the initialisation heuristic plays a role in the performance of Local Search-based methods and should not be neglected when developing solution approaches. We present evidence that even with very bad costs some initial solutions can still find better final solutions than the ones with a good initial cost. For example, we observe that the knowingly bad NN heuristic ends up having the best performance overall by the end of a Guided Local Search run. Although we do not claim this observation is generalisable for other metaheuristics, we raise the question of whether it is worth investigating this step of VRP search methods.

Figure 5: Feature ranking from one of the runs of the Random Forest.

![Image](image_000010_5a286ecaab9e11e745170fba4b3739576cc1d2889ebf3c05d0105f6eb1b27004.png)

Motivated by this question and by observing the difference in solution quality among different initial heuristics, we also utilise features from the solutions and instances to train 10 ML methods to learn how to identify the cases in which each of the 4 constructive heuristics analysed can perform better. The different ML techniques show that there is some correlation between the features selected and the ability to predict which method would find better performance as if learning how to predict in which part of the search space the selected GLS would perform better. This was achieved by randomly splitting the training data over 30 runs and showing that the overall costs are reduced. The KNN method seemed to outperform the others in both prediction accuracy and solution quality. Overall, we still achieve our objective of showing that we can learn how to select the initial heuristics and that, although relatively small, they provide a reduction in costs across the majority of the instances tested. Another surprising result comes from the outperformance of the NN heuristic across most instances considered, a heuristic that is often used as a bad example of a greedy heuristic.

Although the work here is conducted using only one specific local search-based method, we believe that the techniques and results presented here are transferable to other methods. We expect the approach used in this paper to be easily extendable to different local search-based methods with minor adjustments. For future work we want to expand this concept to more initial heuristics - more complex construction methods could be considered if the gain in search performance outgains the loss of time in calculating all initial solutions. For example, we could use this approach to rank solutions from a population-based metaheuristic, increasing their selection chance as they could be likely to provide better offspring. To consider this and other possible improvements, we would also have to increase the sample for training, feeding more data so the ML techniques can better distinguish the classes. Finally, a relevant next step would be to explore in more detail how each feature influences the final classification, followed by tuning and a possible reduction in the feature space size - or the addition of more relevant ones.

Learning to Select Initialisation Heuristic for VRPs

## REFERENCES

- [1] Florian Arnold, Michel Gendreau, and Kenneth Sörensen. 2019. Efficiently solving very large-scale routing problems. Computers &amp; OR 107 (2019), 32-42.
- [2] Florian Arnold and Kenneth Sörensen. 2019. Knowledge-guided local search for the vehicle routing problem. Computers &amp; Operations Research 105 (may 2019), 32-46. https://doi.org/10.1016/j.cor.2019.01.002
- [3] Florian Arnold and Kenneth Sörensen. 2019. What makes a VRP solution good? The generation of problem-specific knowledge for heuristics. Computers &amp; Operations Research 106 (2019), 280-288.
- [4] M. Bellmore and G. L. Nemhauser. 1968. The Traveling Salesman Problem: A Survey. Operations Research 16, 3 (1968), 538-558.
- [5] Yoshua Bengio, Andrea Lodi, and Antoine Prouvost. 2021. Machine learning for combinatorial optimization: A methodological tour d'horizon. European Journal of Operational Research 290, 2 (April 2021), 405-421. https://doi.org/10.1016/j. ejor.2020.07.063
- [6] Kris Braekers, Katrien Ramaekers, and Inneke Van Nieuwenhuyse. 2016. The vehicle routing problem: State of the art classification and review. Computers &amp; Industrial Engineering 99 (sep 2016), 300-313. https://doi.org/10.1016/j.cie.2015. 12.007
- [7] Olli Bräysy. 2003. A Reactive Variable Neighborhood Search for the VehicleRouting Problem with Time Windows. INFORMS Journal on Computing 15, 4 (nov 2003), 347-368. https://doi.org/10.1287/ijoc.15.4.347.24896
- [8] Edmund K Burke, Michel Gendreau, Matthew Hyde, Graham Kendall, Gabriela Ochoa, Ender Özcan, and Rong Qu. 2013. Hyper-heuristics: A survey of the state of the art. Journal of the Operational Research Society 64, 12 (2013), 1695-1724.
- [9] Joao Guilherme Cavalcanti Costa, Yi Mei, and Mengjie Zhang. 2021. An Evolutionary Hyper-Heuristic Approach to the Large Scale Vehicle Routing Problem. 2021 IEEE Congress on Evolutionary Computation (CEC) (June 2021).
- [10] Geoff Clarke and John W Wright. 1964. Scheduling of vehicles from a central depot to a number of delivery points. Operations research 12, 4 (1964), 568-581.
- [11] Joao Guilherme Cavalcanti Costa, Yi Mei, and Mengjie Zhang. 2022. Guided Local Search with an Adaptive Neighbourhood Size Heuristic for Large Scale Vehicle Routing Problems. In Proceedings of the Genetic and Evolutionary Computation Conference (Boston, Massachusetts) (GECCO '22) . Association for Computing Machinery, New York, NY, USA, 213-221. https://doi.org/10.1145/3512290.3528865
- [12] G. B. Dantzig and J. H. Ramser. 1959. The Truck Dispatching Problem. Management Science 6, 1 (oct 1959), 80-91. https://doi.org/10.1287/mnsc.6.1.80
- [13] Michel Gendreau, Jean-Yves Potvin, Olli Bräysy, Geir Hasle, and Arne Løketangen. 2008. Metaheuristics for the Vehicle Routing Problem and Its Extensions: A Categorized Bibliography. In Operations Research/Computer Science Interfaces . Springer US, 143-169. https://doi.org/10.1007/978-0-387-77778-8\_7
- [14] Michel Gendreau and Christos D Tarantilis. 2010. Solving large-scale vehicle routing problems with time windows: The state-of-the-art . Cirrelt Montreal.
- [15] Billy E Gillett and Leland R Miller. 1974. A heuristic algorithm for the vehicledispatch problem. Operations research 22, 2 (1974), 340-349.
- [16] Bruce L. Golden, Edward A. Wasil, James P. Kelly, and I-Ming Chao. 1998. The Impact of Metaheuristics on Solving the Vehicle Routing Problem: Algorithms, Problem Sets, and Computational Results. Fleet Management and Logistics (1998), 33-56. https://doi.org/10.1007/978-1-4615-5755-5\_2
- [17] Keld Helsgaun. 2000. An effective implementation of the Lin-Kernighan traveling salesman heuristic. European Journal of Operational Research 126, 1 (oct 2000), 106-130. https://doi.org/10.1016/s0377-2217(99)00284-2
- [18] Changwu Huang, Yuanxiang Li, and Xin Yao. 2020. A Survey of Automatic Parameter Tuning Methods for Metaheuristics. IEEE Transactions on Evolutionary Computation 24, 2 (2020), 201-216. https://doi.org/10.1109/TEVC.2019.2921598
- [19] Maryam Karimi-Mamaghan, Mehrdad Mohammadi, Patrick Meyer, Amir Mohammad Karimi-Mamaghan, and El-Ghazali Talbi. 2022. Machine learning at the service of meta-heuristics for solving combinatorial optimization problems: A state-of-the-art. Eur. J. Oper. Res. 296, 2 (Jan. 2022), 393-422.
- [20] Gilbert Laporte. 2009. Fifty Years of Vehicle Routing. Transportation Science 43, 4 (nov 2009), 408-416. https://doi.org/10.1287/trsc.1090.0301
- [21] Jan Karel Lenstra and A. H. G. Rinnooy Kan. 1981. Complexity of vehicle routing and scheduling problems. Networks 11, 2 (1981), 221-227.
- [22] Feiyue Li, Bruce Golden, and Edward Wasil. 2005. Very large-scale vehicle routing: new test problems, algorithms, and results. Computers &amp; Operations Research 32, 5 (may 2005), 1165-1179. https://doi.org/10.1016/j.cor.2003.10.002
- [23] Sirui Li, Zhongxia Yan, and Cathy Wu. 2021. Learning to delegate for large-scale vehicle routing. In Advances in Neural Information Processing Systems , M. Ranzato, A. Beygelzimer, Y. Dauphin, P.S. Liang, and J. Wortman Vaughan (Eds.), Vol. 34. Curran Associates, Inc., 26198-26211.
- [24] S. Lin and B. W. Kernighan. 1973. An Effective Heuristic Algorithm for the Traveling-Salesman Problem. Operations Research 21, 2 (apr 1973), 498-516. https://doi.org/10.1287/opre.21.2.498
- [25] MohammadReza Nazari, Afshin Oroojlooy, Lawrence Snyder, and Martin Takac. 2018. Reinforcement Learning for Solving the Vehicle Routing Problem. In Advances in Neural Information Processing Systems , S. Bengio, H. Wallach, H. Larochelle, K. Grauman, N. Cesa-Bianchi, and R. Garnett (Eds.), Vol. 31. Curran

Associates, Inc.

- [26] Christos H Papadimitriou and Kenneth Steiglitz. 1998. Combinatorial optimization: algorithms and complexity . Courier Corporation.
- [27] F. Pedregosa, G. Varoquaux, A. Gramfort, V. Michel, B. Thirion, O. Grisel, M. Blondel, P. Prettenhofer, R. Weiss, V. Dubourg, J. Vanderplas, A. Passos, D. Cournapeau, M. Brucher, M. Perrot, and E. Duchesnay. 2011. Scikit-learn: Machine Learning in Python. Journal of Machine Learning Research 12 (2011), 2825-2830.
- [28] David Pisinger and Stefan Ropke. 2007. A general heuristic for vehicle routing problems. Computers &amp; Operations Research 34, 8 (aug 2007), 2403-2435. https: //doi.org/10.1016/j.cor.2005.09.012
- [29] Christian Prins. 2004. A simple and effective evolutionary algorithm for the vehicle routing problem. Computers &amp; Operations Research 31, 12 (oct 2004), 1985-2002. https://doi.org/10.1016/s0305-0548(03)00158-8
- [30] Jussi Rasku, Tommi Kärkkäinen, and Nysret Musliu. 2016. Feature extractors for describing vehicle routing problem instances. OASICS; 50 (2016).
- [31] Peter Ross. 2013. Hyper-heuristics. In Search Methodologies . Springer US, 611-638. https://doi.org/10.1007/978-1-4614-6940-7\_20
- [32] Stuart Russell and Peter Norvig. 2018. Artificial Intelligence: A Modern Approach, Global Edition . Addison Wesley. https://www.ebook.de/de/product/25939961/ stuart\_russell\_peter\_norvig\_artificial\_intelligence\_a\_modern\_approach\_ global\_edition.html
- [33] Melissa Sanchez, Jorge M. Cruz-Duarte, Jose Carlos Ortiz-Bayliss, Hector Ceballos, Hugo Terashima-Marin, and Ivan Amaya. 2020. A Systematic Review of Hyper-Heuristics on Combinatorial Optimization Problems. IEEE Access 8 (2020), 128068-128095. https://doi.org/10.1109/access.2020.3009318
- [34] Anand Subramanian, Eduardo Uchoa, and Luiz Satoru Ochi. 2013. A hybrid algorithm for a class of vehicle routing problems. Computers &amp; Operations Research 40, 10 (oct 2013), 2519-2531. https://doi.org/10.1016/j.cor.2013.01.013
- [35] Eduardo Uchoa, Diego Pecin, Artur Alves Pessoa, Marcus Poggi, Thibaut Vidal, and Anand Subramanian. 2017. New benchmark instances for the Capacitated Vehicle Routing Problem. Eur. J. Oper. Res. 257, 3 (2017), 845-858.
- [36] Thibaut Vidal. 2022. Hybrid genetic search for the CVRP: Open-source implementation and SWAP ∗ neighborhood. Computers &amp; Operations Research 140 (apr 2022), 105643. https://doi.org/10.1016/j.cor.2021.105643
- [37] Thibaut Vidal, Teodor Gabriel Crainic, Michel Gendreau, and Christian Prins. 2013. Heuristics for multi-attribute vehicle routing problems: A survey and synthesis. European Journal of Operational Research 231, 1 (nov 2013), 1-21. https://doi.org/10.1016/j.ejor.2013.02.053
- [38] Christos Voudouris and Edward Tsang. 1999. Guided local search and its application to the traveling salesman problem. European Journal of Operational Research 113, 2 (mar 1999), 469-499. https://doi.org/10.1016/s0377-2217(98)00099-x
- [39] Christos Voudouris, Edward P.K. Tsang, and Abdullah Alsheddy. 2010. Guided Local Search. In Handbook of Metaheuristics . Springer US, 321-361. https: //doi.org/10.1007/978-1-4419-1665-5\_11
- [40] David H Wolpert and William G Macready. 1997. No free lunch theorems for optimization. IEEE transactions on evolutionary computation 1, 1 (1997), 67-82.
- [41] Anthony Wren and Alan Holliday. 1972. Computer Scheduling of Vehicles from One or More Depots to a Number of Delivery Points. Journal of the Operational Research Society 23, 3 (sep 1972), 333-344. https://doi.org/10.1057/jors.1972.53
- [42] Bin Yu, Zhong-Zhen Yang, and Baozhen Yao. 2009. An improved ant colony optimization for vehicle routing problem. European Journal of Operational Research 196, 1 (jul 2009), 171-176. https://doi.org/10.1016/j.ejor.2008.02.028
- [43] Jiuxia Zhao, Minjia Mao, Xi Zhao, and Jianhua Zou. 2021. A Hybrid of Deep Reinforcement Learning and Local Search for the Vehicle Routing Problems. IEEE Transactions on Intelligent Transportation Systems 22, 11 (nov 2021), 7208-7218. https://doi.org/10.1109/tits.2020.3003163