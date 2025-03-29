## Learning to Delegate for Large-scale Vehicle Routing

Sirui Li ∗ MIT

siruil@mit.edu

Zhongxia Yan * MIT

zxyan@mit.edu

Cathy Wu MIT cathywu@mit.edu

## Abstract

Vehicle routing problems (VRPs) form a class of combinatorial problems with wide practical applications. While previous heuristic or learning-based works achieve decent solutions on small problem instances of up to 100 cities, their performance deteriorates in large problems. This article presents a novel learningaugmented local search framework to solve large-scale VRP. The method iteratively improves the solution by identifying appropriate subproblems and delegating their improvement to a black box subsolver. At each step, we leverage spatial locality to consider only a linear number of subproblems, rather than exponential. We frame subproblem selection as regression and train a Transformer on a generated training set of problem instances. Our method accelerates state-of-the-art VRP solvers by 10x to 100x while achieving competitive solution qualities for VRPs with sizes ranging from 500 to 3000. Learned subproblem selection offers a 1.5x to 2x speedup over heuristic or random selection. Our results generalize to a variety of VRP distributions, variants, and solvers.

## 1 Introduction

Vehicle routing problems (VRPs) have enjoyed ample applications in logistics and ride-hailing services [23] around the world for decades. While determining the optimal solution to a VRP is NP-hard [25], there have been numerous attempts to solve VRPs both exactly and approximately: provable algorithms have been designed for specific problem instances up to size 130 [28], and powerful heuristic solvers such as LKH-3 [13] and HGS [49, 48] find good solutions in practice for problems of size more than 1000. However, heuristics methods often suffer from inflexibility due to extensive hand-crafting and the heavy computational burden from lengthy iterative procedures, as in the case of LKH-3. For example, LKH-3 takes more than an hour to solve a size 2000 CVRP instance (Table 1), which is impractical for applications such as large-scale courier or municipal services.

More recently, machine learning methods inspired by the Pointer Network [50] provide alternatives to traditional solvers: the learning-based methods greatly reduce computation time while maintaining decent solution quality on small problem instances (less than 100 cities), by training on diverse sets of problem distributions either via supervised [50] or reinforcement learning [29, 20]. However, these methods remain difficult to scale, and few report results on problems of size more than 200.

Our work aims to address scalability by learning to identify smaller subproblems which can be readily solved by existing methods. Our learned subproblem selector guides the problem-solving process to focus local improvement on promising subregions. While there exists a combinatorial number

of subproblems that can be selected at each step, we leverage spatial locality commonly found in combinatorial optimization problems to restrict the selection space size to be linear in the problem size. Intuitively, objects far away from each other generally have very small influence on each other's solution and are likely to be in different routes, so they should not be part of the same subproblem. The greatly reduced search space enables us to feasibly train an attention-based subproblem selector.

Our framework combines the advantages of learning and heuristics: our network identifies promising subproblems to improve upon, dramatically speeding up solution times. Using a competitive subsolver on subproblems, we achieve good solution quality without the high computational costs of running the subsolver on large problem instances. In summary, our contributions are:

- · We propose learning-to-delegate , a learning-based framework for solving large-scale VRPs by iteratively identifying and solving smaller subproblems.
- · Despite the high dimensionality and NP-hardness of subproblems, we design a Transformer architecture that effectively predicts the subsolver's solution quality for a subproblem.
- · With extensive validation, we show that learning-to-delegate offers significant speedups and/or objective improvements over both its base subsolver and random (or heuristic) subproblem selection, for a variety of VRP variants, distributions, and solvers.

## 2 Preliminary: Capacitated Vehicle Routing Problems (CVRP)

In CVRP, there is a depot node 0 and city nodes { 1 , ..., N } . Each city node i has demand d i to fulfill. A vehicle with capacity C starts and ends at the depot and visits a route of city nodes such that the sum of city demands along the route does not exceed C , after which the vehicle starts a new route again. The objective is to find a valid solution minimizing the solution cost. We define the following:

- · Route: a sequence of nodes, where the first and last node are the depot 0 , and the rest are city nodes. In a valid route, the sum of demands of the city nodes does not exceed C .
- · Route cost: the sum of edge costs for the sequence of nodes. For an edge from node i to node j , the edge cost is the Euclidean distance between node i and j .
- · Solution: a feasible solution consists of a set of valid routes visiting each city exactly once.
- · Solution cost: the sum of route costs for all routes in the solution. An optimal solution is a feasible solution with the minimum solution cost.
- · Subproblem: a CVRP consisting of the depot, a subset of cities, and corresponding demands.

## 3 Related Work

Classical methods. Heuristics for solving combinatorial optimization problems have been studied for decades. The most powerful methods, such as local search [1], genetic algorithms [36], and ant colony methods [10], involve iteratively improving the solution in a hand-designed neighborhood. For example, move, swap [52], and 2-opt [9] are well-known heuristics for the traveling salesman and vehicle routing problems. The competitive VRP solver LKH-3 [13] uses the Lin-Kernighan heuristic [26] as a backbone, which involves swapping pairs of sub-routes to create new routes, whereas the CVRP solver HGS [49, 48] uses a hybrid genetic and local search procedure to achieve state-of-the-art solution qualities on problems up to size 1000. While LKH-3 2 tackles a variety of VRP variants, the publicly available implementation of HGS 3 only solves CVRP.

For large problems, low-level heuristics are combined with meta-heuristics , including Tabu Search with Adaptive Memory [40], guided local search [51], and Large Neighborhood Search [35]. The inspiration for our work derives from Partial OPtimization Metaheuristic Under Special Intensification Conditions (POPMUSIC), which iteratively optimizes problem subparts and has been used to solve problems as diverse as map labeling [24], berth allocation [22], and p -median clustering [39].

Despite the promise of the POPMUSIC framework for large-scale combinatorial optimization, the impact of certain design choices is not well understood, such as the subproblem selection ordering

Figure 1: Our iterative framework for VRPs . (a) At each time step, we start with a current solution X . Circles are city nodes, blue lines are route segments, and the red star is the depot. (b) We aggregate each route by taking the centroid of all city nodes in the route. (c) For each route, we define a corresponding subproblem as the k -nearest neighbors of the route. Two such subproblems with k = 3 are shown. (d) Our subproblem selector selects a subproblem S (yellow). (e) We feed S into the subsolver to get a new subsolution X ′ S . The red edges are updated by the subsolver. (f) We update X to new solution X ′ with X ′ S , then repeat (b)-(f).

![Image](image_000000_045b0be63501aa41d6c8d77f416281e285fde6739d7e28f5d8549269af1c4377.png)

and size [43]. For example, it is not clear how to meaningfully order the subproblems. One early work in this direction demonstrated that a last-in-first-out stack order performs better than random [3]. Our work provides a natural approach to order the subproblems based on predicted improvement.

Deep learning methods. Recently, there has been a surge of interest in training deep neural networks (DNN) to solve combinatorial problems. As observed by Kwon et al. [21], most methods fall into one of the following two categories: 1) construction methods [29, 20], where an autoregressive model such as the Pointer Network [50] directly outputs a solution, and 2) improvement methods [7, 15], where the DNN iteratively performs local updates to the solution, resembling local search.

These methods are approaching the solution quality of LKH-3 [13] on small problem instances. For example, Kwon et al. [21] extends a construction approach [20] to encourage more diverse output samples on N ≤ 100 VRPs. Among improvement methods, Lu et al. [27] learn a meta-controller to select among a set of local search heuristics, marking the first learning-based approach to outperform LKH-3 on N ≤ 100 VRPs. Despite these successes, learning-based approaches for large-scale VRPs are poorly understood. In addition, all of the aforementioned methods are trained using deep reinforcement learning; for large problems, trajectory collection becomes prohibitively expensive.

Scaling up learning methods. A few recent works have begun to investigate scaling of learned networks for NP-hard graph problems. For example, Ahn et al. [2] propose an iterative scheme called learning-what-to-defer for maximum independent set. At each stage, for each node in the graph, the network either outputs the solution for the node or defers the determination to later stages. Song et al. [38] proposes an imitation-learning-based pipeline for Integer Linear Programs (ILP), where at each stage they partition all variables into disjoint subsets, and use the Gurobi [12] ILP solver to solve the partitions. Due to differences in graph structure, our work presents a more natural scheme to handle VRP constraints and structures. A few works attempt to incorporate learning to decompose large-scale VRPs [6, 47, 31]. However, the decomposition approaches proposed appear to be experimentally less effective than ours.

## 4 An Iterative Framework for VRPs

While CVRPs are NP-hard and thus require worst-case exponential time in the problem size to solve optimally, typical CVRPs exhibit structure that practical solvers may exploit. We hypothesize that in

such situations, the larger problem can be efficiently approximately solved as a sequence of smaller subproblems, which can be delegated to efficient subsolvers. To test this hypothesis, we propose a learning-based, iterative framework with two components: 1) a subsolver capable of solving a small problem instance (exactly or approximately), and 2) a learned model for identifying suitable subproblems within a larger problem to delegate to the subsolver.

## Algorithm 1: Learning to Delegate

Input: Problem instance P , initialized solution X , subproblem selector f θ , subsolver Subsolver , number of steps T , parameter k denoting the size of subproblems

- 1 for Step t = 1: T do
- 2 S k, local ← ConstructSubproblems ( P, X, k )
- 3 S ← f θ ( S k, local )
- 5 X ← X ′ S ∪ X P \ S
- 4 X ′ S ← Subsolver ( S )
- 6 end for

We illustrate our iterative framework in Figure 1, which takes in a problem instance P with a feasible initial solution X 0 . At each step, given the current solution X , we select a smaller subproblem S ⊂ P with our learned model f θ then apply the subsolver to solve S ; we then update X with the new solution for S . To maintain feasibility after an update, we restrict S to be the set S of visited cities of a subset of routes from X . Since routes with cities in P \ S remain valid routes, we obtain a new feasible solution X ′ = X ′ S ∪ X P \ S , where X ′ S is the subsolution for S and X P \ S consists of unselected routes from X . Intuitively, a strong f θ should identify a subproblem S such that the subsolver solution X ′ S results in a large improvement in objective from X to X ′ .

## 4.1 The Restricted Subproblem Selection Space

As the number of routes in P is R = O N ( ) , the cardinality of the selection space S is O (2 R ) , which is exponential in the problem size N and difficult for learned models to consider. If we restrict each subproblem to cities from exactly k routes, there are still ( R k ) = O R ( k ) subproblems to consider. Therefore we further restrict selection to subproblems with spatial locality. As shown in Figure 1(d), we only consider subproblems from S k, local , where each subproblem is centered around a particular route r and contains the k routes whose centroids have the smallest Euclidean distance to the centroid of r . In this way, we reduce the selection space to |S k, local | = R = O N ( ) from |S| = O (2 N ) . In Algorithm 1, we refer to this restriction as ConstructSubproblems. Our restriction to a local selection space is motivated by the fact that many combinatorial optimizations problems have inherent spatial locality, i.e. problem entities are more strongly affected by nearby entities than faraway entities. Earlier heuristical methods such as POPMUSIC [42] leverage similar spatial locality.

## 5 Learning to Delegate

In this section, we discuss criteria for selecting subproblems, and how to train the subproblem selector.

Improvement as the criteria for subproblem selection. Given a selected subproblem S with a current solution X S on the subproblem, we obtain from LKH-3 a new solution X ′ S on the same subproblem (step ( d ) to ( e ) in Figure 1). We then define the immediate improvement

$$\delta ( S ) = c ( X _ { S } ) - c ( X _ { S } ^ { \prime } )$$

where c X ( S ) is the total cost of subsolution X S and δ S ( ) is the improvement in the solution cost. In this way, the sum of improvements along T steps is the total improvement in solution quality. As we empirically find that providing the previous subsolution X S to the subsolver may trap the subsolver in a local optimum, we withhold X S from the solver and thus may see non-positive improvement δ S ( ) ≤ 0 , especially after many steps of subproblem selection. At test time, to avoid worsening the objective, we adopt a hill-climbing procedure such that when δ S ( ) ≤ 0 , we keep X S instead of X ′ S (Figure 1, step ( e ) ). With proper masking, we avoid selecting the same non-improving subproblem S again. The hill-climbing and masking procedures are applied to both our subproblem-selection network f θ and the three heuristic selection rules, described in 6.1, to maintain fair comparisons.

Figure 2: Our Transformer architecture . At each step of our framework (Figure 1(c)), we featurize subproblem S ∈ S k, local into a column vector of unordered cities. We apply the Transformer encoder with multiple multi-head attention layers and a final linear layer, before mean-pooling over the cities to generate the predictions f θ ( S ) , which we fit to the subsolution cost c X ( ′ S ) . When possible, we retrieve a previously predicted subproblem from a cache to minimize computation.

![Image](image_000001_bae1bc588f1e448bb379494823202ecb460aa180d7152eced7b11113c62e1a60.png)

Subproblem selection. The goal of our subproblem selector is to select the subproblem leading to the best immediate improvement. While our subproblem selection does not directly optimize for the total improvement, we observe that (1) our subsolver may perform many low-level operations internally, so high-level problem selection may still benefit greatly from maximizing the immediate improvement, and (2) numerous reinforcement learning approaches to VRP [53, 18] choose a small discount factor such as γ = 0 25 . when optimizing a multi-step objective T ∑ t =1 γ δ t t , as doing

so encourage faster convergence. Moreover, our subproblem selector may instead be trained on multi-step search data to select subproblems offering the best multi-step improvement.

Ground-truth labels. Our restricted selection space allows us to enumerate all possible subproblems at each step. In a typical large scale CVRP instance with N = 2000 cities, a solution consists of roughly R = 200 routes, so the size of the selection space is 200. We obtain the immediate improvement δ S ( ) by running the subsolver on each subproblem S ∈ S k, local . Although our enumeration strategy is feasible for generating training data, it is much too slow to execute on a test CVRP instance. However, if our subproblem selector can predict the best immediate improvement at test time (that is, without running the subsolver on multiple subproblems), then we can combine the best of both worlds to obtain a fast and accurate selection strategy.

Selection strategies: regression vs classification. Given a labeled dataset, we may treat the task of identifying the best subproblem as either regression or classification. In the context of imitation learning with a greedy expert, the former learns a value function while the latter learns a policy . Due to space limitation, we focus discussion on regression and reserve comparison with classification for Appendix A.5.1. The regression-based subproblem selector uses a trained f θ to predict the subsolution cost c X ( ′ S ) ; we then simply compute arg max S c X ( S ) -f θ ( S ) to select the best subproblem.

Network architecture. We define f θ with a Transformer encoder architecture [45]. The input representing each subproblem S is the unordered set of featurized cities in S . The features for each city consist of the demand d and the location of the city ( x, y ) relative to the depot. As we do not feed the existing subsolution X S to the subsolver, the dense multi-head attention mechanism does not need to be modified to take the routes in X S into account. The output of the Transformer encoder is fed into a linear layer then mean-pooled to obtain the scalar prediction f θ ( S ) . In Appendix A.5.5, we perform an ablation study with simpler architectures.

Loss function. We empirically find mean squared error to be less stable than Huber loss [16], which we set as our loss function

$$L ( \theta ; S ) = \begin{cases} \frac { 1 } { 2 } ( f _ { \theta } ( S ) - c ( X _ { S } ^ { \prime } ) ) ^ { 2 }, & \text{if} \, | f _ { \theta } ( S ) - c ( X _ { S } ^ { \prime } ) | \leq 1 \\ | f _ { \theta } ( S ) - c ( X _ { S } ^ { \prime } ) | - \frac { 1 } { 2 }, & \text{otherwise} \end{cases} ) ( 2 )$$

![Image](image_000002_2f0a38e7afaee8970b8e4929a35ec70ecc428a2f09cb4917016ed6521bdebd1e.png)

Figure 3: Instances from N = 2000 CVRPdistributions . From left to right: instance from uniform, mixed ( n c = 7 cluster centers), clustered ( n c = 3 cluster centers), and real-world. The red star is the location of the depot, while blue dots are cities sized proportional to demand.

![Image](image_000003_3f041a460f4d1f49640290fd4475d9136bad060505b75c9e7db843e1bac1897a.png)

![Image](image_000004_d6a71fbe64e81d12124f05a3fdca34063142577faa7636ebdffa297dce5b95ca.png)

![Image](image_000005_3d162c3865a83f3b622b5bd840eea8cd08b02bf9d84eafb53ce68289fcd4517c.png)

## 6 Experiments and Analysis

We illustrate the CVRP distributions considered in our work in Figure 3. We perform extensive experiments to evaluate our learning framework, aiming to answer the following questions:

- 1. Uniform distribution . How does our method compare with baselines, in terms of solution time and quality, on problems with uniformly distributed cities?
- 2. Clustered distributions . How does our method perform on problems with clustered cities?
- 3. Out-of-distribution . Can our model generalize, such as to larger or real-world instances?
- 4. VRP variants . Can our method address more sophisticated VRPs? E.g., CVRP with Time Windows (CVRPTW) [37] or VRP with Mixed Pickup and Delivery (VRPMPD) [33].
- 5. VRP solvers . Can our method be adapted to leverage other VRP subsolvers?

We reserve additional ablations on subproblem selection as classification, effect of subproblem size k and discussion of asymptotic behavior, subproblem selection with the HGS subsolver, effect of weaker initialization methods, and comparison with simpler architectures for Appendix A.5.

## 6.1 Setup

We briefly describe experimental setup in the main text and defer full details to Appendix A.1. Given a particular distribution of VRP, we generate separate sets of training, validation, and test problem instances. Unless otherwise stated, our validation and test sets contain 40 instances each. For each problem instance, we generate a rough initial solution by partitioning it into disjoint subsets of cities and briefly running the subsolver on each subset. Due to its compatibility with many VRP variants, we use LKH-3 [13] as the subsolver for all VRP distributions unless otherwise stated.

We generate the training set by running our iterative framework and selecting subproblems by enumerating the subsolver on all S ∈ S k, local . As many subproblems remain unchanged from X to X ′ , we use previously cached subsolutions when possible instead of re-running the subsolver. While generation times differ depending on several factors, typically it takes less than 10 hours with 200 Intel Xeon Platinum 8260 CPUs to generate a training set of 2000 instances.

To avoid training multiple models for different problem sizes N , we train a single model for each VRP distribution with combined data from multiple N ∈ { 500 1000 2000 , , } . Training takes at most 8 hours on a single NVIDIA V100 GPU. To exploit symmetry in problem instances, we apply rotation and flip augmentation at training time. To evaluate trained selectors on a problem instance, we run the iterative selection framework on a single CPU, with disabled multithreading and no GPU.

We select the best hyperparameters via manual search based on validation set performance. We record final results on the test set as the mean and standard error over 5 runs with different random seeds.

Baselines. By default, as we use LKH-3 as the subsolver, we run LKH-3 on the full problem instances with our initialization for 30000 local update steps, which takes 2-3 hours on average for N = 2000 . If using HGS as the subsolver, we also run HGS on the full problem instances for the same amount of time as our LKH-3 baseline. These baselines allow us to compute the speedup of our framework over the subsolver alone. We also compare against OR Tools [30], another open source

Table 1: Performance and computation time for uniform CVRP . For problem instance sizes N ∈ { 500 1000 2000 , , } , we report the objective values ( lower is better) of our method and baseline methods, averaged across all instances in the test set. Note that the cost is the total distance of routes in the solution. LKH-3 (30k) runs LKH-3 for 30k steps to near convergence, while LKH-3 (95%) is the 95% solution quality. Random, Count-based, Max Min Distance, and Ours (Short) run until matching LKH-3 (95%) in solution quality, with the speedup reported in parentheses, while Ours (long) runs for twice amount time as Ours (Short).

|              | N = 500   | N = 500    | N = 1000   | N = 1000      | N = 2000   | N = 2000      |
|--------------|-----------|------------|------------|---------------|------------|---------------|
| Method       | Cost      | Time       | Cost       | Time          | Cost       | Time          |
| LKH-3 (95%)  | 62.00     | 4.4min     | 120.02     | 18min         | 234.89     | 52min         |
| LKH-3 (30k)  | 61.87     | 30min      | 119.88     | 77min         | 234.65     | 149min        |
| OR Tools     | 65.59     | 15min      | 126.52     | 15min         | 244.65     | 15min         |
| AMsampling   | 69.08     | 4.70s      | 151.01     | 17.40s        | 356.69     | 32.29s        |
| AMgreedy     | 68.58     | 25ms       | 142.84     | 56ms          | 307.86     | 147ms         |
| NeuRewriter  | 73.60     | 58s        | 136.29     | 2.3min        | 257.61     | 8.1min        |
| Random       | 61.99     | 71s (3.8x) | 120.02     | 3.2min (5.5x) | 234.88     | 6.4min (8.0x) |
| Count-based  | 61.99     | 59s (4.5x) | 120.02     | 2.1min (8.2x) | 234.88     | 5.3min (10x)  |
| Max Min      | 61.99     | 59s (4.5x) | 120.02     | 2.5min (7.0x) | 234.89     | 5.2min (10x)  |
| Ours (Short) | 61.99     | 38s (7.0x) | 119.87     | 1.5min (12x)  | 234.89     | 3.4min (15x)  |
| Ours (Long)  | 61.70     | 76s        | 119.55     | 3.0min        | 233.86     | 6.8min        |

heuristic VRP solver employing iterative search, terminating runs at 15 minutes, as OR Tools stops improving the solution within this time for all instances.

We include results for previous learning methods AM [20] and NeuRewriter [7]. These do not outperform LKH-3 even on small problem sizes, and learning methods have had more difficulty generalizing to larger instances. In fact, these methods are trained on problems of size N ≤ 100 , and we find that they yield poor solutions on N ≥ 500 without architecture modifications and extensive re-training. The AM and NeuRewriter results demonstrate the difficulty of scaling up previous learning methods. We do not initialize OR-Tools, AM, and NeuRewriter because we empirically find that these methods have limited solution capability and do not improve our decent initialization.

To validate our subproblem selector's ability to identify promising subproblems, we design three additional baselines that employ our iterative framework using hand-crafted heuristics to select subproblems. The three heuristics that we use are: (1) Random, selecting subproblem S from S k, local uniformly; (2) Count-based, which avoids repetitive selections by selecting the subproblem centered at the route whose city nodes have been selected cumulatively the least often in previous steps; and (3) Max Min Distance, which encourages coverage of the entire problem instance by selecting subproblems with the maximum distance from the nearest centroid of previously selected subproblems. We run the heuristic baselines with the same setup as our learned subproblem selector.

Metrics. We refer to two metrics to compare our method against baseline methods.

- 1. Improvement over method X : at a specified computation time, the improvement of method Y over method X is the total improvement of method Y minus that of method X.
- 2. Speedup over method X : at a specified solution quality that method X attains, the speedup of method Y over method X is the computation time required for method X to attain the solution quality divided by the time for method Y to attain the solution quality.

We define 95% solution quality of running a method X over a computation time as the solution quality with 95% of the total improvement. We report speedup at 95% solution quality of a method X because X may take a disproportionate amount of time on the last 5% of improvement; reporting speedup at 100% solution quality inflates the speedup.

![Image](image_000006_1eee64001185b8cc49aff4d1c4819d77eeff4cf10ab55b5c75406aedca7682d8.png)

Time (s)

Time (s)

Time (s)

Figure 4: Improvement over LKH-3 for uniform CVRP . The x-axis is the computation time and extends until LKH-3 has completed 30k steps. The vertical lines represent the computation times of Ours (Short) and Ours (Long) from Table 1. The three subplots correspond to N = 500 (left), N = 1000 (middle), and N = 2000 (right).

![Image](image_000007_4bc788b575cb897a999063bec7a73d0d8b5e2580779e3c4f4a42284064d224fe.png)

Figure 5: Speedup over LKH-3 for uniform CVRP . The x-axis is the solution quality attained, measured as a percentage of the LKH-3's maximum improvement. The dashed vertical line represents the 95% solution quality used to compute the speedup, as mentioned in Table 1. The three subplots correspond to N = 500 (left), N = 1000 (middle), and N = 2000 (right).

![Image](image_000008_38014f53022746b88a932a2262ed28975736fabbd96ac36feed5536ea6f37ec8.png)

![Image](image_000009_1c67ce2439ed8dbd459024605c9e7a3a4737cb77a2f12cb82ea58b516e3f89e6.png)

## 6.2 Uniform CVRP Distribution

As seen in Table 1, our method achieves the best performance for all problem sizes, matching LKH-3's solution quality with more than 7x to 15x less computation time and offering even more improvements with longer computation time. Although we need to evaluate all R = O N ( ) subproblems of the initial solution with our subproblem selector, subsequent per-step computation time of our method is mostly independent of the problem size N since we only evaluate changed subproblems.

Running LKH-3 for 30k local update steps achieves superior performance to all previous other heuristic and learning-based baselines. Its solution quality scales well to large problem sizes, yet the solution time is significantly longer. Previous learning-based methods, though fast, result in much worse solution qualities. Our heuristic baselines Random, Count-based, and Max Min Distance demonstrate that our iterative framework, even without the learned subproblem selector, may achieve over 5x to 10x speedup over LKH-3. Nevertheless, our results demonstrate that learning the subproblem selector may offer an additional 1.5x speedup over non-learning heuristics.

In Figure 4, we demonstrate the solution quality of our method and baselines compared to LKH-3. We see that, compared to baseline methods, the learned subproblem selector obtains the best solution quality when run for a reasonable amount of time. The improvement of most methods based on our iterative framework converge when run for an excessive amount of time; this is unsurprising, as Random and other baselines are eventually able to select all subproblems offering improvements.

In Figure 5, we demonstrate the speedup of our method over LKH-3, comparing to baseline methods. The speedup is often significant even at low levels of solution quality, and improves with higher solution quality. The speedup is not as meaningful beyond 95% LKH-3 solution quality, as LKH-3 takes a disproportionate amount of time to attain the last 5% of improvement.

## 6.3 Clustered and Mixed CVRP Distributions

We further examine the framework's performance on CVRP distributions with clusters of cities, such as studied in [8, 44]. We generate a clustered instance by first sampling ( x, y ) locations of n c cluster centroids then sampling the cities around the centroids. The mixed distribution samples 50% of the cities from a uniform distribution and the rest from around cluster centers. We generate a dataset

Figure 6: Speedup in out-of-distribution CVRPs . The speedup of our model on the N = 3000 uniform distribution (left) and N = 2000 real-world distribution (right) without finetuning. Ours (Uniform) and Ours (Clustered) are trained on the uniform and clustered CVRP distributions, respectively. Note that LKH-3 is run for 50k steps for N = 3000 instances.

![Image](image_000010_75f7b4ea3b6624fdb05067065e52cabc96c4137395a31132d6d98c54924af2df.png)

![Image](image_000011_71c9a34ce7b44e7eab224b3be9255a86eb011d59e983eae6585b06ccbcf20ba6.png)

of instances for every ( N,n ,m c ) ∈ { 500 1000 2000 , , } × { 3 5 7 , , } × { Clustered , Mixed } and train a single model on the entire dataset. We evaluate the model on validation and test sets of 10 instances per ( N,n ,m c ) combination (i.e. 60 instances per N ). Due to space limitation, we provide more details about the data distribution and full results in Appendix A.2.

Table 2 reports speedups of our method and the Random baseline over LKH-3. Our method sees at least 2x speedup over Random in all settings. We see larger speedups for clustered distributions than for mixed or uniform distributions (Table 1).

## 6.4 Out-of-distribution Generalization

We study how our subproblem selector generalize to a uniform CVRP distribution with larger problem size N = 3000 and to a realworld CVRP distributions, both unseen at

| Setting   | Setting     | n c = 3   | n c = 5   | n c = 7   |
|-----------|-------------|-----------|-----------|-----------|
| Clustered | Ours Random | 26x       | 18x       | 25x       |
|           |             | 11x       | 7.5x      | 9.0x      |
| Mixed     | Ours        | 13x       | 14x       | 14x       |
| Mixed     | Random      | 6.6x      | 6.4x      | 7.6x      |

training time. The real-world CVRP distribution derives from a CVRP dataset [4] consisting of 10 very-large-scale instances on 5 Belgium regions with N ranging from 3000 to 30000 and randomly generated demand distribution. To generate an instance, we subsample the cities in a region without replacement to N = 2000 , while regenerating the demands to match our training distribution. For each original instance, we generate 5 subsampled instances to form the test set of 50 total instances. We visualize the original and subsampled datasets in Appendix A.3.

We apply subproblem selectors trained on uniform and clustered data without finetuning on the new data distributions and report the speedup comparison with the Random baseline in Figure 6. We see that when transferring to N = 3000 , subproblem selectors trained on uniform and clustered data offer similar performance. However, the model trained on the clustered distribution generalizes well to the real-world distribution while the model trained on the uniform distribution fails to generalize, with worse speedup than the Random baseline. These results suggest that the domain variability of the clustered distribution strongly improves generalization performance.

## 6.5 Other VRP Variants

While our previous experiments vary the distribution of city locations in CVRP, here we consider two VRP variants with uniform city distribution but with additional constraints: CVRPTW [37] and VRPMPD [33]. The former specifies a hard time window constraint for visiting each city, while the latter specifies pick up and delivery constraints in addition to capacity constraint. A detailed description of the variants can be found in Appendix A.4.

Similar to CVRP, we observe significant speedup with our iterative framework alone, while learning offers additional speedup. For CVRPTW, our method offers a 8.2x speedup while our Random baseline offers a 5.9x speedup; for VRPMPD, our method offers a 31x speedup while our Random baseline offers a 20x speedup. We suspect that the time window constraint in CVRPTW imposes strict orderings on the order of city visitations, increasing the difficulty for the subproblem selector.

Figure 7: Speedup in other N = 2000 VRP variants , CVRPTW (left) and VRPMPD (right).

![Image](image_000012_51323eb3f2bb327dbd81990a504784f2055ee7c8e61232e397a63f9818d31085.png)

![Image](image_000013_079f06d66dd0ad7d80fb193ff975be749769b73876f7054615a4aece44418c40.png)

Figure 8: Speedup with the HGS subsolver on uniform CVRP . We compare the speedup of our method equipped with LKH-3 or HGS as the subsolver for N = 2000 (left) and N = 3000 (right).

![Image](image_000014_ced2a1ffc33a8cdfaf3bd72a7a39010863d76abb54e826224ef368d08980ea9b.png)

![Image](image_000015_3b7d0dbcd3386273f99e9c159faa384229235194e023c07a7e5e9784cc924ba3.png)

## 6.6 A State-of-the-Art CVRP Subsolver: HGS

While we focus our analysis on LKH-3 due to its applicability to many variants of VRPs, HGS [49, 48] is a state-of-the-art solver focused on CVRP. Thus, we apply our method to train subproblem selectors for the HGS subsolver. We discuss the full experimental setup and results in Appendix A.5.3.

With HGS as the subsolver, we observe a 103x speedup for our method on N = 2000 , compared with a 77x speedup for the Random baseline. Similarly, we observe a 198x speedup for our method on N = 3000 , compared with a 152x speedup for our Random baseline. The large speedup may be due to the fact that HGS is designed and calibrated for medium-scale problems of 500 to 1000 cities, allowing it to function better as a subsolver for large-scale VRPs.

## 7 Conclusion

This paper presents a learning-based framework which learns which subproblems to delegate to a subsolver when solving large VRPs. Spatial locality allows us to learn the subproblem selector over a reduced selection space. The proposed method accelerates competitive VRP solvers on problems of sizes up to 3000 , requiring an order of magnitude less computation time. We identify a 1.5x to 2x speedup over non-learning selection strategies. Our results generalize to a variety of VRP distributions, variants, and solvers.

While most previous learning-based combinatorial optimization methods [20, 21, 2] rely on reinforcement learning due to the unavailability of optimal solutions as high-quality labels, our work highlights the counterintuitive effectiveness of supervised learning with only moderate-quality labels to achieve high-quality solutions with iterative subproblem selection. An interesting line of future work may explore other ways to effectively leverage moderate-quality labels for combinatorial optimization tasks. In particular, we discuss in Appendix A.6 the applicability of our method to other combinatorial optimization problems with spatial locality. We believe that our learning framework can serve as a powerful technique for both the learning and operations research communities to scale up combinatorial optimization solvers.

Our code is publicly available at https://github.com/mit-wu-lab/learning-to-delegate .

Negative Social Impact. Enabling more efficient solutions of large-scale VRPs may exhibit negative externalities such as inducing additional traffic from delivery vehicles and centralizing services that pose a stronger competition to brick-and-mortar retail.

## 8 Acknowledgements

This research was supported by MIT Indonesia Seed Fund, US DOT DDETFP, and the MIT-IBM Watson AI Lab. The authors are grateful to the anonymous reviewers for detailed comments that substantially improved the article. The authors acknowledge the MIT SuperCloud and Lincoln Laboratory Supercomputing Center for providing (HPC, database, consultation) resources that have contributed to the research results reported within this paper. We also thank Zongyi Li for helpful discussions and technical advice throughout the project.

## References

- [1] Aarts, E., Aarts, E. H., and Lenstra, J. K. (2003). Local search in combinatorial optimization . Princeton University Press.
- [2] Ahn, S., Seo, Y., and Shin, J. (2020). Learning what to defer for maximum independent sets. In International Conference on Machine Learning , pages 134-144. PMLR.
- [3] Alvim, A. C. and Taillard, E. D. (2013). Popmusic for the world location-routing problem. EURO Journal on Transportation and Logistics , 2(3):231-254.
- [4] Arnold, F., Gendreau, M., and Sörensen, K. (2019). Efficiently solving very large-scale routing problems. Computers &amp; Operations Research , 107:32-42.
- [5] Bła˙ zewicz, J., Domschke, W., and Pesch, E. (1996). The job shop scheduling problem: Conventional and new solution techniques. European journal of operational research , 93(1):1-33.
- [6] Bosman, P. A. and La Poutré, H. (2006). Computationally intelligent online dynamic vehicle routing by explicit load prediction in an evolutionary algorithm. In Parallel Problem Solving from Nature-PPSN IX , pages 312-321. Springer.
- [7] Chen, X. and Tian, Y. (2019). Learning to perform local rewriting for combinatorial optimization. In Wallach, H., Larochelle, H., Beygelzimer, A., d Alché-Buc, F., Fox, E., and Garnett, R., editors, Advances in Neural Information Processing Systems , volume 32. Curran Associates, Inc.
- [8] Christofides, N., Mingozzi, A., and Toth, P. (1979). Loading problems. N. Christofides and al., editors, Combinatorial Optimization , pages 339-369.
- [9] Croes, G. A. (1958). A method for solving traveling-salesman problems. Operations research , 6(6):791-812.
- [10] Dorigo, M., Birattari, M., and Stutzle, T. (2006). Ant colony optimization. IEEE computational intelligence magazine , 1(4):28-39.
- [11] Gehring, H. and Homberger, J. (1999). A parallel hybrid evolutionary metaheuristic for the vehicle routing problem with time windows. In Proceedings of EUROGEN99 , volume 2, pages 57-64. Citeseer.
- [12] Gurobi Optimization, L. (2021). Gurobi optimizer reference manual.
- [13] Helsgaun, K. (2017). An extension of the lin-kernighan-helsgaun tsp solver for constrained traveling salesman and vehicle routing problems. http://akira.ruc.dk/~keld/research/ LKH-3/ .
- [14] Helsgaun, K. (2018). Using popmusic for candidate set generation in the lin-kernighan-helsgaun tsp solver. Roskilde Universitet , 7.
- [15] Hottung, A. and Tierney, K. (2020). Neural large neighborhood search for the capacitated vehicle routing problem. In 24th European Conference on Artificial Intelligence (ECAI 2020) .
- [16] Huber, P. J. (1992). Robust estimation of a location parameter. In Breakthroughs in statistics , pages 492-518. Springer.
- [17] Kallehauge, B., Larsen, J., Madsen, O. B., and Solomon, M. M. (2005). Vehicle routing problem with time windows. In Column generation , pages 67-98. Springer.

| [18] Khalil, E. B., Dai, H., Zhang, Y., Dilkina, B., and Song, L. (2017). Learning combinatorial optimization algorithms over graphs. In Advances in Neural Information Processing Systems .                                                                                                                                                        |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [19] Kohl, N., Desrosiers, J., Madsen, O. B., Solomon, M. M., and Soumis, F. (1999). 2-path cuts for the vehicle routing problem with time windows. Transportation Science , 33(1):101-116.                                                                                                                                                         |
| [20] Kool, W., van Hoof, H., and Welling, M. (2019). Attention, learn to solve routing problems! In International Conference on Learning Representations .                                                                                                                                                                                          |
| [21] Kwon, Y.-D., Choo, J., Kim, B., Yoon, I., Gwon, Y., and Min, S. (2020). Pomo: Policy optimization with multiple optima for reinforcement learning. In Larochelle, H., Ranzato, M., Hadsell, R., Balcan, M. F., and Lin, H., editors, Advances in Neural Information Processing Systems , volume 33, pages 21188-21198. Curran Associates, Inc. |
| [22] Lalla-Ruiz, E. and Voss, S. (2016). Popmusic as a matheuristic for the berth allocation problem. Annals of Mathematics and Artificial Intelligence , 76(1-2):173-189.                                                                                                                                                                          |
| [23] Laporte, G. (2009). Fifty years of vehicle routing. Transportation science , 43(4):408-416.                                                                                                                                                                                                                                                    |
| [24] Laurent, M., Taillard, É. D., Ertz, O., Grin, F., Rappo, D., and Roh, S. (2009). From point feature label placement to map labelling. In Proceedings, metaheuristic international conference (MIC'09), Hamburg . Citeseer.                                                                                                                     |
| [25] Lenstra, J. K. and Kan, A. R. (1981). Complexity of vehicle routing and scheduling problems. Networks , 11(2):221-227.                                                                                                                                                                                                                         |
| [26] Lin, S. and Kernighan, B. W. (1973). An effective heuristic algorithm for the traveling-salesman problem. Operations research , 21(2):498-516.                                                                                                                                                                                                 |
| [27] Lu, H., Zhang, X., and Yang, S. (2020). A learning-based iterative method for solving vehicle routing problems. In International Conference on Learning Representations .                                                                                                                                                                      |
| [28] Lysgaard, J., Letchford, A. N., and Eglese, R. W. (2004). A new branch-and-cut algorithm for the capacitated vehicle routing problem. Mathematical Programming , 100(2):423-445.                                                                                                                                                               |
| [29] Nazari, M., Oroojlooy, A., Snyder, L., and Takac, M. (2018). Reinforcement learning for solving the vehicle routing problem. In Bengio, S., Wallach, H., Larochelle, H., Grauman, K., Cesa-Bianchi, N., and Garnett, R., editors, Advances in Neural Information Processing Systems , volume 31. Curran Associates, Inc.                       |
| [30] Perron, L. and Furnon, V. (2019). Or-tools.                                                                                                                                                                                                                                                                                                    |
| [31] Poullet, J. (2020). Leveraging machine learning to solve The vehicle Routing Problem with Time Windows . PhD thesis, Massachusetts Institute of Technology.                                                                                                                                                                                    |
| [32] Renaud, J. and Boctor, F. F. (2002). A sweep-based algorithm for the fleet size and mix vehicle routing problem. European Journal of Operational Research , 140(3):618-628.                                                                                                                                                                    |
| [33] Salhi, S. and Nagy, G. (1999). A cluster insertion heuristic for single and multiple depot vehicle routing problems with backhauling. Journal of the operational Research Society , 50(10):1034- 1042.                                                                                                                                         |
| [34] Santini, A., Schneider, M., Vidal, T., and Vigo, D. (2021). Decomposition strategies for vehicle routing heuristics.                                                                                                                                                                                                                           |
| [35] Shaw, P. (1998). Using constraint programming and local search methods to solve vehicle routing problems. In International conference on principles and practice of constraint programming , pages 417-431. Springer.                                                                                                                          |
| [36] Sivanandam, S. and Deepa, S. (2008). Genetic algorithms. In Introduction to genetic algorithms , pages 15-37. Springer.                                                                                                                                                                                                                        |
| [37] Solomon, M. M. (1987). Algorithms for the vehicle routing and scheduling problems with time window constraints. Operations research , 35(2):254-265.                                                                                                                                                                                           |

- [38] Song, J., Lanka, R., Yue, Y., and Dilkina, B. (2020). A general large neighborhood search framework for solving integer linear programs. In Larochelle, H., Ranzato, M., Hadsell, R., Balcan, M. F., and Lin, H., editors, Advances in Neural Information Processing Systems , volume 33, pages 20012-20023. Curran Associates, Inc.
- [39] Taillard, É. D. (2003). Heuristic methods for large centroid clustering problems. Journal of heuristics , 9(1):51-73.
- [40] Taillard, É. D., Gambardella, L. M., Gendreau, M., and Potvin, J.-Y. (2001). Adaptive memory programming: A unified view of metaheuristics. European Journal of Operational Research , 135(1):1-16.
- [41] Taillard, É. D. and Helsgaun, K. (2019). Popmusic for the travelling salesman problem. European Journal of Operational Research , 272(2):420-429.
- [42] Taillard, É. D. and V oss, S. (2002). Popmusic-partial optimization metaheuristic under special intensification conditions. In Essays and surveys in metaheuristics , pages 613-629. Springer.
- [43] Taillard, É. D. and Voß, S. (2018). POPMUSIC , pages 687-701. Springer International Publishing, Cham.
- [44] Uchoa, E., Pecin, D., Pessoa, A., Poggi, M., Vidal, T., and Subramanian, A. (2017). New benchmark instances for the capacitated vehicle routing problem. European Journal of Operational Research , 257(3):845-858.
- [45] Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., Kaiser, L. u., and Polosukhin, I. (2017). Attention is all you need. In Guyon, I., Luxburg, U. V., Bengio, S., Wallach, H., Fergus, R., Vishwanathan, S., and Garnett, R., editors, Advances in Neural Information Processing Systems , volume 30. Curran Associates, Inc.
- [46] Veliˇ ckovi´, P., Cucurull, G., Casanova, A., Romero, A., Liò, P., and Bengio, Y. (2018). Graph c Attention Networks. International Conference on Learning Representations .
- [47] Ventresca, M., Ombuki-Berman, B., and Runka, A. (2013). Predicting genetic algorithm performance on the vehicle routing problem using information theoretic landscape measures. In European Conference on Evolutionary Computation in Combinatorial Optimization , pages 214-225. Springer.
- [48] Vidal, T. (2020). Hybrid genetic search for the cvrp: Open-source implementation and swap* neighborhood. arXiv preprint arXiv:2012.10384 .
- [49] Vidal, T., Crainic, T. G., Gendreau, M., Lahrichi, N., and Rei, W. (2012). A hybrid genetic algorithm for multidepot and periodic vehicle routing problems. Operations Research , 60(3):611624.
- [50] Vinyals, O., Fortunato, M., and Jaitly, N. (2015). Pointer networks. In Cortes, C., Lawrence, N., Lee, D., Sugiyama, M., and Garnett, R., editors, Advances in Neural Information Processing Systems , volume 28. Curran Associates, Inc.
- [51] Voudouris, C. and Tsang, E. P. (2003). Guided local search. In Handbook of metaheuristics , pages 185-218. Springer.
- [52] Wu, C., Shankari, K., Kamar, E., Katz, R., Culler, D., Papadimitriou, C., Horvitz, E., and Bayen, A. (2016). Optimizing the diamond lane: A more tractable carpool problem and algorithms. In 2016 IEEE 19th International Conference on Intelligent Transportation Systems (ITSC) , pages 1389-1396. IEEE.
- [53] Yolcu, E. and Poczos, B. (2019). Learning local search heuristics for boolean satisfiability. In Wallach, H., Larochelle, H., Beygelzimer, A., d'Alché-Buc, F., Fox, E., and Garnett, R., editors, Advances in Neural Information Processing Systems , volume 32. Curran Associates, Inc.

## Checklist

- 1. For all authors...
- (a) Do the main claims made in the abstract and introduction accurately reflect the paper's contributions and scope? [Yes]
- (b) Did you describe the limitations of your work? [Yes] See Section 6.5. We discuss our method gets slightly less speedup on CVRPTW than CVRP or VRPMPD; in section 6.2, we also discuss our learned subproblem-selection order at convergence are the same as Random and Min Count order, which can be seen as a potential limitation, but we provide justification in the same Section (6.2) as well.
- (c) Did you discuss any potential negative societal impacts of your work? [Yes] See the negative social impact paragraph in Section 7 (Conclusion)
- (d) Have you read the ethics review guidelines and ensured that your paper conforms to them? [Yes]
- 2. If you are including theoretical results...
- (a) Did you state the full set of assumptions of all theoretical results? [N/A]
- (b) Did you include complete proofs of all theoretical results? [N/A]
- 3. If you ran experiments...
- (a) Did you include the code, data, and instructions needed to reproduce the main experimental results (either in the supplemental material or as a URL)? [Yes] As stated in Section 7 (Conclusion), our code is released at https://github.com/mit-wu-lab/ learning-to-delegate .
- (b) Did you specify all the training details (e.g., data splits, hyperparameters, how they were chosen)? [Yes] We briefly mention the training details in Section 6.1 of the main paper, and we put the full details in Appendix A.1.
- (c) Did you report error bars (e.g., with respect to the random seed after running experiments multiple times)? [Yes] We plot error bars on all of our experiment figures in Section 6.
- (d) Did you include the total amount of compute and the type of resources used (e.g., type of GPUs, internal cluster, or cloud provider)? [Yes] See Section 6.1.
- 4. If you are using existing assets (e.g., code, data, models) or curating/releasing new assets...
- (a) If your work uses existing assets, did you cite the creators? [Yes] We use the LKH-3 and HGS VRP solvers as a part of our component. We cite the creator as [13] (LKH-3) and [49, 48] (HGS) in our paper. We compare with existing learning and heuristic baseline using their code. We cite the creators in our paper, and provide exerpeiment setup (including github links to the repos) in the supplementary material.
- (b) Did you mention the license of the assets? [N/A] All the assets (code and data) we use are open source.
- (c) Did you include any new assets either in the supplemental material or as a URL? [No]
- (d) Did you discuss whether and how consent was obtained from people whose data you're using/curating? [N/A] We generate synthetic data for most of our experiments. The only real dataset in Section 6.4 is open source.
- (e) Did you discuss whether the data you are using/curating contains personally identifiable information or offensive content? [N/A]
- 5. If you used crowdsourcing or conducted research with human subjects...
- (a) Did you include the full text of instructions given to participants and screenshots, if applicable? [N/A]
- (b) Did you describe any potential participant risks, with links to Institutional Review Board (IRB) approvals, if applicable? [N/A]
- (c) Did you include the estimated hourly wage paid to participants and the total amount spent on participant compensation? [N/A]

## A Appendix

## Contents

| A.1   | Experiment Setup . . . . . . . . . . . . . . . . . . . . . . . .                                                                                                   | . . . . . . . .                                                                                                                                                    |   15 |
|-------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|------|
| A.2   | Clustered and Mixed CVRP . . .                                                                                                                                     | . . . . . . . . . . . . . . . . . . . . . . .                                                                                                                      |   17 |
| A.3   | Out-of-distribution CVRPs . .                                                                                                                                      | . . . . . . . . . . . . . . . . . . . . . . . . .                                                                                                                  |   17 |
|       | A.3.1                                                                                                                                                              | Uniform CVRP with N = 3000 . . . . . . . . . . . . . . . . . . . .                                                                                                 |   17 |
|       | A.3.2                                                                                                                                                              | Real-world CVRP . . . . . . . . . . . . . . . . . . . . . . . . . . .                                                                                              |   17 |
| A.4   | VRP Variants . . . . . . . . . . . . . . . . . . . . . . .                                                                                                         | . . . . . . . . . . .                                                                                                                                              |   19 |
|       | A.4.1                                                                                                                                                              | CVRPTW (Capacitated Vehicle Routing Problem with Time Windows)                                                                                                     |   19 |
|       | A.4.2                                                                                                                                                              | VRPMPD (Vehicle Routing Problem with Mixed Pickup and Delivery)                                                                                                    |   20 |
| A.5   | Ablation Studies . . . .                                                                                                                                           | . . . . . . . . . . . . . . . . . . . . . . . . . . . . .                                                                                                          |   21 |
|       | A.5.1                                                                                                                                                              | Regression vs Classification for Subproblem Selection . . . . . . . .                                                                                              |   21 |
|       | A.5.2                                                                                                                                                              | Subproblem Size k and Asymptotic Behavior in Uniform CVRP . . .                                                                                                    |   24 |
|       | A.5.3                                                                                                                                                              | CVRP Subsolvers: LKH-3 vs HGS . . . . . . . . . . . . . . . . . .                                                                                                  |   24 |
|       | A.5.4                                                                                                                                                              | Effect of Initial Solution Quality . . . . . . . . . . . . . . . . . . . .                                                                                         |   26 |
|       | A.5.5                                                                                                                                                              | Transformer vs Simpler Architectures . . . . . . . . . . . . . . . . .                                                                                             |   27 |
| A.6   | Applicability of Our learning-to-delegate Framework to Other Problems in Combina- torial Optimization (CO) . . . . . . . . . . . . . . . . . . . . . . . . . . . . | Applicability of Our learning-to-delegate Framework to Other Problems in Combina- torial Optimization (CO) . . . . . . . . . . . . . . . . . . . . . . . . . . . . |   33 |

## A.1 Experiment Setup

We discuss the full experimental setup for training and evaluating the subproblem selector on the uniform CVRP distribution in this section while reserving any minor modifications for the clustered CVRP distributions, real-world CVRP distribution, VRP variants, and ablation studies for Appendix A.2, A.3, A.4, and A.5, respectively.

Uniform CVRP distribution. We generate CVRP instances of size N = 500 1000 2000 , , , and 3000 following the same distribution of cities locations and demands as previous works [29, 20, 27, 21]: the depot and cities' ( x, y ) locations are sampled uniformly from [0 , 1] 2 , each city's demand d is sampled uniformly from { 1 2 , , ..., 9 } , and each vehicle has capacity C = 50 . For each N , we sample n train = 2000 , n val = 40 , and n test = 40 problem instances for the training, validation, and test set.

| Table 3: Uniform CVRP parameters.   | Table 3: Uniform CVRP parameters.   |                       |                       |
|-------------------------------------|-------------------------------------|-----------------------|-----------------------|
| Choices of N                        | { 500 1000 2000 3000 , , , }        | Table 4: Data splits. | Table 4: Data splits. |
| Dist. of depot location             | U ([0 , 1] 2 )                      | Training              | 2000 instances        |
| Dist. of city location              | U ([0 , 1] 2 )                      | Validation            | 40 instances          |
| Dist. of city demand                | U { ( 1 , . . . 9 } )               | Test                  | 40 instances          |
| Vehicle capacity                    | 50                                  |                       |                       |

Initialization solution. Our iterative learning-to-delegate framework (Figure 1) requires a feasible initial solution X 0 . To generate X 0 for each problem instance, we employ a fixed initialization scheme in which the space is partitioned into 10 equally-sized angular sectors radiating outward from the depot. We then run the LKH-3 solver for a brief 100 steps on each partition to produce initial routes. Initialization on average takes 6 seconds for N = 500 , 18 seconds for N = 1000 , 50 seconds for N = 2000 , and 94 seconds for N = 3000 on a single CPU. Our initialization scheme is similar to the sweep-based algorithm proposed by Renaud and Boctor [32], which is a commonly used initialization scheme for iterative VRP solvers. Unless stated otherwise, we use the same initialization

Table 5: Best architecture hyperparameters.

| Input dimension             | 3           | Table 6: Best training hyperparameters.   | Table 6: Best training hyperparameters.   |
|-----------------------------|-------------|-------------------------------------------|-------------------------------------------|
| Base model                  | Transformer | Optimizer                                 | Adam                                      |
| Model dimension d model     | 128         |                                           |                                           |
| Number of heads n heads     | 8           | Learning rate Learning rate schedule      | 0.001 Cosine                              |
| Number of layers n layers   | 6           |                                           |                                           |
| Feed-forward dimension d ff | 512         | Batch size                                | 512                                       |
| Activation                  | ReLU        | Number of gradient steps                  | 40000                                     |
| Layer Normalization         | Yes         | GPU                                       | NVIDIA V100                               |
| Dropout                     | 0           |                                           |                                           |

scheme in all methods to fairly compare each method's ability to improve from a rough initialization. We additionally explore the effect of initialization quality on our method in Appendix A.5.4.

Training data generation. We generate the training set by running our iterative framework and selecting subproblems as follows: for each instance P with initial solution X in the training set, we run the subsolver on each S ∈ S k, local to compute the subsolution X S and improvement δ S ( ) . We update the solution from X to X ′ with arg max S δ S ( ) then repeat the process on X ′ for a fixed number of steps D train = 30 of our iterative framework. For the uniform CVRP distribution, for each route r in the current solution X , we construct S k, local with k = 10 nearest routes. We compute the subsolution by running the LKH-3 subsolver for 500 steps, which takes around 6.7 seconds on a single CPU for a k = 10 subproblem composed of around 100 cities. We concatenate training data from multiple N 's to train the subproblem regression model. As mentioned in the main text, most subproblems remain unchanged from X to X ′ , so we do not repeat unchanged subproblems in our generated training set; therefore, the number of unique subproblems in our training set may be around 3-8 times smaller than the total number of non-unique subproblems n train D train E [ R ] ≈ n train D train N E [ d ] in the training set, where E [ R ] is the average number of routes in a solution.

As we find that the performance of the subproblem regression model trained on the concatenated N = 500 and 1000 data offers satisfactory performance, even when transferred to N = 2000 and 3000 , we do not collect training data for the latter two cases. Collecting training data is the most computationally intensive component of our work, and the computation time increases with larger N , larger k , and larger D train . By restricting training data collection to N = 500 and 1000 , k = 10 , and D train = 30 , the total time taken is around 10 hours on 200 CPUs.

Input features. To convert each subproblem S into the input to our Transformer subproblem selector, we create a 3 -dim input feature vector for each city consisting of the city's ( x, y ) locations (centered around the depot's ( x, y ) location) and the demand d (rescaled by the capacity).

Architecture and training hyperparameters. Our best Transformer model uses model dimension d model = 128 , n heads = 8 heads, n layers = 6 layers, feed-forward dimension of d ff = 512 , ReLU activation, layer normalization, and no dropout. We train with Adam optimizer with a learning rate of 0.001, cosine learning rate decay, and a batch size of 2048 for 40000 gradient steps. All hyperparameters are selected on the validation set and frozen before evaluating on the test set.

Baseline learning methods. In Table 1, we compare our methods with two learning baselines: Attention Model (AM) [20] and NeuRewriter [7]. We use the pre-trained AM model provided by the author: https://github.com/wouterkool/attention-learn-to-route . The model is trained on problems of size 100 following the same distribution as ours. The model has two modes of prediction: greedy and sampling. We perform 1280 samples per instance, yet find the sampling performance worse than the greedy result. While the original paper reports better sampling performance than greedy when N ≤ 100 , we believe that AM sampling suffers from the fact that the solution space is exponentially larger with our large problem instance sizes. Therefore, any small stochasticity and imperfections from AM sampling may autoregressively compound much more in our setting than in previous AM settings. We use a single NVIDIA V100 GPU with 20 CPUs for evaluation.

For NeuRewriter, we re-train the network on problems of size 100 using the code provided by the author: https://github.com/facebookresearch/neural-rewriter , as no pre-trained model is available. Similarly to AM, we choose size 100 since it is the largest training problem size from the paper. We run into memory and fitting issue when trying to rerun their code on larger problem sizes, so we do not report the numbers here. We use a single NVIDIA V100 GPU with 20 CPUs to perform the evaluation.

## A.2 Clustered and Mixed CVRP

Clustered CVRP distribution. We generate clustered CVRP distribution by modifying the distribution of city locations within the uniform CVRP distribution specified in Appendix A.1. Given the number of clusters n c , we first generate n c cluster centroids by sampling from U ([0 2 . , 0 8] . 2 ) as the ( x, y ) locations. Then, we generate the city nodes by first sampling a centroid uniformly at random, and then sampling the ( x, y ) location normally distributed with the mean at the centroid and standard deviation 0 07 . . The ( x, y ) locations are clipped within the [0 , 1] 2 box.

Mixed CVRP distribution. For the mixed distributions, we sample half of the city nodes according to the uniform distribution, and the other half according to a clustered distribution with n c centers. This is the common practice used in standard CVRP benchmark datasets [44]. Note that we generate our new dataset instead of using the benchmark datasets because the CVRP benchmark datasets include mostly small scale N ≤ 500 instances and have very few instances with N ≥ 100 .

Dataset composition. As we experiment over all combinations of N ∈ { 500 1000 2000 , , } , n c ∈ { 3 5 7 , , } , and m ∈ { Clustered , Mixed , we generate smaller training, validation, and test set sizes } with 500, 10, and 10 instances respectively for each combination. We visualize one clustered and one mixed instance in Figure 3.

Training and evaluation. Like in uniform CVRP, we do not collect the training set for the N = 2000 instances and concatenate the training set for all remaining combinations to create a single training set of 2 ∗ 3 ∗ 2 ∗ 500 = 6000 problem instances. We use identical architecture and training hyperparameters as listed in Appendix A.1, as we find that these hyperparameters perform reasonably well. We report results on the test set of 10 instances for each of the 3 ∗ 3 ∗ 2 = 18 settings separately.

Additional results. We include the full evaluation results on the test set for all clustered and mixed distributions. Figure 18 contains results for N = 500 , Figure 19 contains results for N = 1000 , and Figure 20 contains results for N = 2000 . As mentioned in the above paragraph, a single trained model for our method is evaluated on all combinations of ( N,n ,m c ) , which demonstrates superior speedup and improvement (when run for a reasonable time period) over the Random baseline. To save space, we also include results with the HGS subsolver in the same set of plots.

## A.3 Out-of-distribution CVRPs

Here we describe the CVRP distributions and full results supporting the out-of-distribution generalization performance of our trained uniform CVRP and clustered CVRP models, with full training specifications in Appendix A.1 and Appendix A.2 respectively.

## A.3.1 Uniform CVRP with N = 3000

We do not describe the setup here as it was previously described with the uniform CVRP distribution in Appendix A.1. In Figure 9, we see that the model trained on the clustered CVRP demonstrates slightly better improvement than the model trained on the uniform CVRP; otherwise the model performances are similar.

## A.3.2 Real-world CVRP

Excitingly, we see robust generalization performance of our previously trained models to problem instances from an unseen real-world CVRP distribution derived from the large-scale real-world CVRP dataset generated by Arnold et al. [4], which is open source at https://antor.uantwerpen.be/ xxlrouting/ .

Figure 9: Uniform CVRP distributions, N = 3000 . Graphs of speedup (left, higher is better), improvement (middle, higher is better), and total cost (right, lower is better). The left graph is the same as the left graph in Figure 6. The right vertical axis in the improvement graph (middle) indicates the improvement over LKH-3 as a percentage of the total improvement that LKH-3 attains over 30k steps. The horizontal dotted black line in the total cost graph (right) indicates the initial solution quality for all methods. The vertical dashed black line in the speedup graph (left) and the horizontal LKH-3-colored line in the total cost graph (right) indicate the 95% LKH-3 solution quality. The overlapping colored vertical lines in the improvement graph (middle) and the total cost graph (right) indicate computation times required for the corresponding learning-based methods to reach the aforementioned solution quality.

![Image](image_000016_d2e412462b178a3b48717a28ab95d2d2740f5da0c66afb85f0f93a210a5a8f75.png)

![Image](image_000017_767517d315afe0202174671f1fa583162f5454a7a211977fc71bff5d5bea915d.png)

![Image](image_000018_4539685eb9401b3ab88cce6f4f7696560c631704c5382ca778447fb9c04090f3.png)

Table 7: Instance sizes N of original real-world instances from Arnold et al. [4]. Instances are subsampled without replacement to N = 2000 before feeding into our model.

| Region   |   Centralized Depot |   Eccentric Depot |
|----------|---------------------|-------------------|
| Leuven   |                3000 |              4000 |
| Antwerp  |                6000 |              7000 |
| Ghent    |               10000 |             11000 |
| Brussels |               15000 |             16000 |
| Flanders |               20000 |             30000 |

Original real-world CVRP distribution. The original dataset contains 10 instances representing 5 different Belgium regions: Leuven, Antwerp, Ghent, Brussels, and Flanders. For each region, two instances are provided: one with a depot located at the center of all cities and the other with an eccentric depot located in the outskirts of the regions. We list the original sizes of each instance in Table 7 and visualize them in Figure 10.

This dataset focuses on long-haul deliveries, with the average solution route length ranging from 14.8 to 117.2. The authors randomly sample demand for each node from { 1 2 3 , , } with probabilities 0.5,

Figure 10: Original real-world CVRP instances. Five instances (top row) have centralized depots and five (bottom row) have eccentric depots.

![Image](image_000019_4689b50d577805d963924e08966cebffc5c6e81de47af40a0339793257a63a91.png)

0.3, and 0.2 respectively, and the capacity of the vehicle ranges from 20 to 50 for central depot and ranges from 100 to 200 for eccentric depot.

Our real-world CVRP dataset. To apply our pre-trained models, we first convert the original real-world CVRP to be reasonably close to our training distributions. To generate each instance in our real-world CVRP distribution from an instance in the original dataset, we subsample the original instance to N = 2000 without replacement, resample each demand from our demand distribution U { ( 1 2 , , . . . , 9 } ) , then set the vehicle capacity to C = 50 . For each original instance, we generate 5 instances from our real-world CVRP distribution. Therefore, our validation and test sets contain 50 instances in total. One instance from our test set is visualized in Figure 3.

Additional results. We show the full transfer performance of the models trained on uniform and clustered CVRP to our real-world CVRP distribution in Figure 11. We see that the model trained on the clustered CVRP distributions significantly outperforms both the model trained on the uniform CVRP distribution and the Random baseline, whereas the model trained on the uniform CVRP distribution sometimes performs worse than the Random baseline. Encouragingly, the clustered model shows strong generalization performance in CVRP distributions which may be of practical value in the real world.

![Image](image_000020_d495c15fa096e53ce479c8d1103775b846b784713462028a1fbcd6eab154adec.png)

![Image](image_000021_a7c63d9bee71c6a74249cd72a93168b57051bc6d158130a916a50dd3bda262c8.png)

![Image](image_000022_0929770cf0cfa14b61e7200193cd50236ae125a192b7368e36bcde6e75baff4f.png)

Total Cost

## A.4 VRP Variants

Here we provide the full definition, experimental setup, and results for VRP variants discussed in Section 6.5: CVRPTW and VRPMPD.

## A.4.1 CVRPTW (Capacitated Vehicle Routing Problem with Time Windows)

Problem formulation. We use the standard CVRPTW formulation found in Gehring and Homberger [11] and Solomon [37]. In additional to all CVRP constraints, each city node i has a time window [ e , l i i ] , the earliest and latest time to visit city i , respectively, and a service time s i . The depot has a time window [ e , l 0 0 ] but no service time. Each vehicle departs the depot at time e 0 and follows a route r to satisfy the additional time window constraint. Let i and j be consecutive cities on route r , and let b r i denote the arrival time at city i of the vehicle serving route r . Then the departure time at city i is b r i + s i and the arrival time at the next city j is b r j = max { e , b j r i + s i + t ij } where t ij is the travel time that equals the Euclidean distance from i to j . This specifies that if a vehicle arrives too early at j , then it needs to wait until the start time e j of j . Meanwhile, the route becomes infeasible if the vehicle arrives at j later than l j . Finally, the depot's time window requires the route to travel back to the depot before l 0 . The number of vehicles is unconstrained. The objective is the same as in CVRP, i.e. minimizing the total Euclidean edge costs of all the routes, which is the standard objective used in the literature [19, 17]. The time window constraint imposed by this variant is related to other combinatorial optimization problems such as job shop scheduling [5].

Problem distribution. We generate CVRPTW instances following a similar procedure as in Solomon [37]: we first generate CVRP instances following the same uniform location and demand distribution as described in Section A.1. For the time window constraint, we set the time window for the depot as [ e , l 0 0 ] = [0 3] , , and the service time at each i to be s i = 0 2 . . We further set the time window for city node i by (1) sampling the time window center c i ∼ U ([ e 0 + t 0 ,i , l 0 -t i, 0 -s i ]) , where t 0 ,i = t i, 0 is the travel time, equaling the Euclidean distance, from the depot to node i ; (2) sampling the time window half-width h i uniformly at random from [ s / i 2 , l 0 / 3] = [0 1 . , 1] ; (3) setting the time window for i as [max( e , c 0 i -h i ) , min( l 0 , c i + h i )] .

Training data generation. Unlike for CVRP distributions, we generate training data using k = 5 routes per subproblem instead of k = 10 ; doing so does not change the number of subproblems at each step, but significantly speeds up the generation process for two reasons: 1) running the subsolver on each subproblem takes around 3.4 seconds instead of 6.7 seconds for k = 10 and 2) updating current X with the subsolution X S for a smaller subproblem S means that there will be more repeated subproblems from X to X ′ as other subproblems are less likely to be affected. Therefore, in around 10 hours total on 200 CPUs, we are able to generate training data with 2000 instances per N and D train = 40 80 , , and 160 for N = 500 1000 , , and 2000 , respectively. An ablation study on the effect of k in uniform CVRP is provided in Appendix A.5.2.

Additional results. We show the full CVRPTW results in Figure 14, which also includes a comparison between subproblem regression and subproblem classification. Interestingly, while our method offers significant speedup over baseline methods and offers significant improvement over LKH-3 (relative to LKH-3's total improvement over 30k steps) when run for a reasonable amount of time, the improvement over the LKH-3 baseline diminishes when run for a very long time. Indeed, our method terminates at a slightly worse total cost than LKH-3 for N = 500 when run for a very long time. However, as we show in the ablation over subproblem size k in Appendix A.5.2, we see that using k = 10 tends to offer better eventual improvement in uniform CVRP, so the diminishing improvement effect may be partially attributed to the fact that we use k = 5 for the CVRPTW experiments.

## A.4.2 VRPMPD (Vehicle Routing Problem with Mixed Pickup and Delivery)

Problem formulation. This VRP variant models the backhauling problem where the vehicles are both delivering items to cities from the depot and picking up items from cities to bring back to the depot. We use the standard VRPMPD formulation as in Salhi and Nagy [33], where each city i either has a pickup order with load p i or delivery order with load d i . Each vehicle with a capacity C starts by loading all delivery orders along the route. Each time the vehicle visits a delivery city i , the vehicle's load decreases by d i ; conversely, each time the vehicle visits a pickup city j , the vehicle's load increases by p j . A valid route requires that the vehicle's load is no greater than C at every point along the route. Therefore, this variant imposes a more complicated capacity constraint than CVRP as the order of city visitation affects the feasibility of a route.

Problem distribution. We generate VRPMPD instances following a similar procedure as in Salhi and Nagy [33]: we take the CVRP instances following the same uniform location and demand distribution as defined in Appendix A.1, and we randomly assign half the cities pickup orders with p i ∼ U { ( 1 2 , , . . . , 9 } ) and the other half delivery orders with d i ∼ U { ( 1 2 , , . . . , 9 } ) . We set the vehicle's capacity to be C = 25 instead of 50 (as done for other VRPs) because each route in VRPMPD visits roughly 2x more cities than those in CVRP or CVRPTW with the same capacity, as the vehicle gains empty space after visiting the cities with delivery orders and can serve cities with pickup orders afterward.

Training data generation. We use the same k = 5 data generation process as described in Appendix A.4.1, but for VRPMPD instead of CVRPTW.

Additional results. We show the full VRPMPD results in Figure 15, which also includes a comparison between subproblem regression and subproblem classification. Similar to the behavior in CVRPTW, while our method offers significant speedup over baseline methods and offers significant improvement over LKH-3 (relative to LKH-3's total improvement over 30k steps) when run for a reasonable amount of time, the improvement over the LKH-3 baseline diminishes when run for a very long time. However, as we show in the ablation over subproblem size k in Appendix A.5.2, we see

that using k = 10 tends to offer better eventual improvement in uniform CVRP, so the diminishing improvement effect may be partially attributed to the fact that we use k = 5 for the VRPMPD experiments.

## A.5 Ablation Studies

## A.5.1 Regression vs Classification for Subproblem Selection

While we focus on regression in Section 5, here we discuss how to frame subproblem selection as classification, and compare results and discuss trade-offs with regression-based subproblem selection.

Subproblem selection as classification. The goal of a classification-based subproblem selector is to select the subproblem which results in the best immediate improvement when solved by the subsolver.

Label transformation. Optimally, we would like our subproblem selector to select the subproblem with the best immediate improvement. However, since we want to avoid overpenalizing the subproblem selector for putting probability mass on a slightly suboptimal selection, we construct a softmax soft label based on δ S ( ) with temperature parameter τ rather than a one-hot hard label, which corresponds to τ → 0

$$\cdot \, \check { \lambda } \\ \ell ( S ) = \frac { e ^ { \delta ( S ) / \tau } } { \sum _ { S ^ { \prime } \in \mathcal { S } _ { k, \text{local} } } e ^ { \delta ( S ^ { \prime } ) / \tau } }$$

Figure 12: Our Graph Attention Network (GAT) architecture . At each step of our iterative framework (Figure 1(c)), we featurize the problem instance P with current solution X into citynode (filled circle) and route-node (empty diamond) features and city-to-subproblem and route-tosubproblem edges. The city-node input features at the i th graph attention layer is passed through a FC network to obtain the input city-node features at the i +1 th graph attention layer. The subproblemnode (filled diamond) output features of the i th graph attention layer is passed through a FC network to obtain the input route-node features at the i +1 th graph attention layer. The final graph attention layer's subproblem-node output features are fed into a linear layers with softmax activation to obtain the classification probabilities p φ ( S ) for each subproblem.

![Image](image_000023_b9cd86ebf865f775602ee00b1c984b534f16357ed8cdb4e9022386024340ee11.png)

Network architecture. An important consequence of subproblem classification is that all subproblems must be considered jointly. This requires a different architecture than presented in Section 5. Intuitively, in order to consider which subproblem is best , the other subproblems must be taken into account. In the regression context, a local measure of improvement is all that is needed. More precisely, as subproblem classification inherently requires feed-forward and back-propagation computation over all subproblems S ∈ S k, local for a single problem P , our choice of architecture must

address this constraint to feasibly train with a large enough batch of problems. Unlike in subproblem regression, we cannot simply sample batches of subproblems from the set of all problem instances to fit individually; each computation in classification entails a softmax over | S k, local | subproblems from the same problem instance. Therefore, our classification-based subproblem selector consists of a Graph Attention Network (GAT) backbone [46], which shares computation and memory between overlapping subproblems in the same problem instance, permitting the use of large batches of problem instances P to increase training stability.

We illustrate the nodes and edges of the graph attention in Figure 12. Given a problem instance P , we define three types of graph nodes: one city-node per city in P , one route-node per route r in the current solution X , and one subproblem node per S ∈ S k, local . We define two types of directed edge connections: a city-to-subproblem edge connects a city-node to a subproblem-node if the city is in the subproblem and a route-to-subproblem edge connects a route-node to a subproblem-node if the route is in the subproblem. The input city-node features is fed into a fully connected (FC) network to obtain the input city-node features of the next graph attention layer, while the output subproblem-node features is fed into another FC network to obtain the input route-node features for the next graph attention layer, as each subproblem S corresponds to a single route r by construction as described in Subsection 4.1. The subproblem-node features of the final graph attention layer are fed into a linear layer with softmax activation to obtain classification probabilities.

KL divergence loss. We minimize the KL divergence between the ground-truth soft labels glyph[lscript] ( S ) and the subproblem selector output probability distribution p φ ( S ) , where φ is the parameters of the subproblem selector

$$L ( \phi ; P ) = D _ { \text{KL} } ( \ell \| p _ { \phi } ) = \sum _ { S \in \mathcal { S } _ { k, \text{local} } } \ell ( S ) \log \left ( \frac { \ell ( S ) } { p _ { \phi } ( S ) } \right ).$$

One possible advantage of the KL divergence loss is that it places the most weight on subproblems with high glyph[lscript] ( S ) , which may help the neural network focus on a few subproblems with high immediate improvements rather than the many subproblems with low immediate improvement. In contrast, the Huber loss used in subproblem regression places equal emphasis on fitting subproblems with low and high immediate improvements.

Classification setup. While we previously describe the setup of the regression model in Appendix A.1, here we describe the additional modifications made in the classification setup compared with the regression setup.

- 1. While we do not need to collect separate training data from the regression setup, we need to process the label to be glyph[lscript] ( S ) rather than δ S ( ) .
- 2. In addition to extracting 3-dim feature vector for each city in P , we also extract 8-dim feature vector for each route in the current solution X consisting of: the average, max, and min ( x, y ) locations of cities in the route (shifted by the depot's ( x, y ) location); the sum of demands of cities in the route; and the route's distance.
- 3. We find the best GAT architecture hyperparameters to be a model (hidden) dimension of 128, 1 attention head, and 3 graph attention layers.
- 4. As the classification network does not handle multiple problem sizes (e.g. N = 500 and N = 2000 ) naturally due to large differences in the number of subproblems, we train each classification model on data from a single problem instance size. However, this is not a fundamental limitation, and future works may explore training a subproblem classification network on multiple problem sizes.
- 5. We train our best classification models with batch sizes of 256 256 , , and 512 problem instances respectively for N = 500 1000 2000 , , . We emphasize that the batches here are batches of problem instances , where each problem instance graph inherently contains |S k, local | subproblems as illustrated earlier in Figure 12, rather than batches of subproblems as in the regression setup.
- 6. Training takes around 6 hours for N = 500 , 12 hours for N = 1000 , and 24 hours for N = 2000 on a single NVIDIA V100 GPU.

Table 8: Training time of subproblem regression and subproblem classification. Subproblem regression models are simultaneously trained over all problem sizes where subproblem classification models are trained on individual problem sizes. Training times are similar for subproblems with size k = 5 and k = 10 , so we do not distinguish between the two cases in this table. Note that we do not use N = 2000 data for subproblems with size k = 10 for training, as explained in Appendix A.1, but we do use N = 2000 data for subproblems with size k = 5 for training, as explained in Appendix A.5.2.

| N             | Regression    | Classification            |
|---------------|---------------|---------------------------|
| 500 1000 2000 | 6 hours total | 6 hours 12 hours 24 hours |

Table 8 summarizes a key advantage of subproblem regression over subproblem classification: subproblem regression is much faster to train and only requires a single trained model over all problem instance sizes.

![Image](image_000024_93ef3b56d62e87e8344661b19f7134db025bb7f4e3d58622a7ab1979b5bc0e7b.png)

Time (s)

Comparison on uniform CVRP. We compare the performance of subproblem regression and subproblem classification for N = 500 and 1000 uniform CVRP and k = 10 . Despite requiring significantly less training time, we observe that subproblem regression does not suffer a loss in performance against the more specialized subproblem classification, as seen in Figure 13. Thus, we chose to focus on subproblem regression in this paper as it offers generalization over multiple problem sizes while enjoying significantly less training time.

Comparison on VRP variants. We compare the performance of subproblem regression and subproblem classification for CVRPTW and VRPMPD instances of size N = 500 1000 , and 2000 with k = 5 . Interestingly, we see that subproblem classification somewhat outperforms subproblem regres-

sion in Figure 14 and Figure 15. Therefore, future works may benefit from trying both regression and classification and possibly adapting subproblem classification to train over multiple problem sizes.

Figure 14: Uniform CVRPTW distributions. Graphs of speedup (left column, higher is better), improvement (middle column, higher is better), and total cost (right column, lower is better). The right vertical axis in each improvement graph (middle column) indicates the improvement over LKH-3 as a percentage of the total improvement that LKH-3 attains over 30k steps. We use loosely dashed lines to indicate classification results. The horizontal dotted black line in the total cost graph (right) indicates the initial solution quality for all methods. The vertical dashed black line in the speedup graph (left) and the horizontal LKH-3-colored line in the total cost graph (right) indicate the 95% LKH-3 solution quality. The styled vertical lines in the improvement graph (middle) and the total cost graph (right) indicate computation times required for the corresponding learning-based methods to reach the aforementioned solution quality.

![Image](image_000025_298f2bdfcd7fe33eb497d7902f21a9988e6f8ff5875aa030d6bc1e6a80bf7f5f.png)

## A.5.2 Subproblem Size k and Asymptotic Behavior in Uniform CVRP

We explore the effect of the subproblem size k , which specifies the number of nearest neighbor routes used to create each subproblem, and show that the choice of k may affect the asymptotic behavior of our iterative framework. We compare results between k = 5 and k = 10 , using the same k = 5 data generation process as described in Appendix A.4.1 for uniform CVRP (not CVRPTW).

Experimental results. As shown in Figure 16, our method and the Random baseline with k = 5 may offer additional speedup over their k = 10 counterparts. However, they tend to converge earlier to worse eventual solution qualities when run for a very long time, though still better than the eventual solution qualities of LKH-3. These results suggest that subproblem selection incorporating both k = 5 and k = 10 could combine the superior speedup of k = 5 with the better eventual solution qualities of k = 10 .

## A.5.3 CVRP Subsolvers: LKH-3 vs HGS

We seek to demonstrate the independence of our framework from the LKH-3 subsolver by applying our framework with HGS [49, 48] as the subsolver to the problem instances from Appendix A.1 and Appendix A.2. Here we discuss our HGS-related experimental setup and results.

Figure 15: Uniform VRPMPD distributions. Graphs of speedup (left column, higher is better), improvement (middle column, higher is better), and total cost (right column, lower is better). The right vertical axis in each improvement graph (middle column) indicates the improvement over LKH-3 as a percentage of the total improvement that LKH-3 attains over 30k steps. We use loosely dashed lines to indicate classification results. The horizontal dotted black line in the total cost graph (right) indicates the initial solution quality for all methods. The vertical dashed black line in the speedup graph (left) and the horizontal LKH-3-colored line in the total cost graph (right) indicate the 95% LKH-3 solution quality. The styled vertical lines in the improvement graph (middle) and the total cost graph (right) indicate computation times required for the corresponding learning-based methods to reach the aforementioned solution quality.

![Image](image_000026_608dacf8dfe8ba00d93b4a3e4c472f9dc77f58e479ca1961128c6cfb689049f4.png)

Initialization solution. To facilitate comparison with previous results, we use the exact same LKH-3-based initialization solutions for all instances as described in Appendix A.1. One note is that the HGS solver does not take an initial solution, so we cannot start running the HGS baseline from the same LKH-3-based initialization as the other methods. To compensate for this limitation, we instead subtract the mean LKH-3-based initialization time from the HGS baseline's total runtime. After this runtime adjustment, we calculate the total HGS improvement by defining the initial HGS solution quality to be the solution quality of our initialization for all other methods. Overall, the initialization time is negligible compared to the total computation time.

Training data generation. As the HGS subsolver is significantly faster than LKH-3, we run the subsolver for 1 second on each subproblem (compared to 6.7 seconds for LKH-3 on size k = 10 subproblems). This allows us to collect data for N = 500 1000 , , and 2000 and D train = 80 160 , , and 320 , respectively, within 10 hours total on 200 CPUs. The remaining uniform and clustered/mixed setup details are described in Appendix A.1 and Appendix A.2, respectively. We collect HGS-based training data for both uniform CVRP and clustered/mixed CVRP, then train a subproblem selector for each data distribution.

Baseline. We compute the speedup and improvement of HGS-based subproblem selectors relative to the HGS solver itself. For uniform CVRP, we run the HGS baseline for the same amount of computation time as our LKH-3 baseline: 1800 seconds, 4620 seconds, 8940 seconds, and 30000 seconds for N = 500 1000 2000 , , , and 3000 , respectively. However, since there are multiple

Figure 16: Uniform CVRP distributions, k = 10 and k = 5 . Graphs of speedup (left column, higher is better), improvement (middle column, higher is better), and total cost (right column, lower is better). The right vertical axis in each improvement graph (middle column) indicates the improvement over LKH-3 as a percentage of the total improvement that LKH-3 attains over 30k steps. We use loosely dashed lines to indicate k = 5 results. The horizontal dotted black line in the total cost graph (right) indicates the initial solution quality for all methods. The vertical dashed black line in the speedup graph (left) and the horizontal LKH-3-colored line in the total cost graph (right) indicate the 95% LKH-3 solution quality. The styled vertical lines in the improvement graph (middle) and the total cost graph (right) indicate computation times required for the corresponding learning-based methods to reach the aforementioned solution quality.

![Image](image_000027_8f28dfbed95bee7c4c62394d90cc0bd3a0e959f6f5090a3d69a915090576ede0.png)

combinations of ( N,n ,m c ) for clustered and mixed distributions with different computation times, we run the HGS baseline for 2000 seconds, 5000 seconds, and 10000 seconds for N = 500 1000 , , and 2000 for every ( N,n ,m c ) .

Experimental results. Figure 17 compares the speedup over the subsolver when using either LKH3 or HGS as the subsolver for uniform CVRP of size N = 500 1000 2000 , , , and 3000 . Similarly, Figures 18, 19, and 20 contain results for mixed and clustered CVRP distributions of size N = 500 1000 , , and 2000 , respectively. We observe that for N = 500 , our method with the HGS subsolver offers a small speedup over the HGS solver for attaining intermediate range of solution qualities; however, the HGS solver converged to better final solution quality in all cases. This observation is unsurprising, as HGS is designed and calibrated to obtain state-of-the-art solutions for instances of size 500 to 1000. Nevertheless, as the problem size N increases, the speedup of our method over HGS increases, demonstrating the effectiveness of subproblem selection in large-scale problems. For N = 2000 and 3000 we are not able to run HGS until full convergence, which would require another order of magnitude more time. Our learned method outperformed the Random baseline in all cases, demonstrating that learning further accelerates subproblem selection.

## A.5.4 Effect of Initial Solution Quality

As our iterative framework relies on an initial solution generated according to Appendix A.1, we explore the effect of poorer, out-of-distribution initial solutions on our subproblem selector for uniform CVRP instances.

Figure 17: Uniform CVRP distributions, LKH-3 and HGS subsolvers. Graphs of speedup (left column, higher is better), improvement (middle column, higher is better), and total cost (right column, lower is better). We use loosely dashed lines to indicate HGS results. The right vertical axes in each improvement graph (middle column) indicates the improvement over the corresponding subsolver as a percentage of the best total improvement that the subsolver baseline attains. The horizontal dotted black line in the total cost graph (right) indicates the initial solution quality for all methods. The vertical dashed black line in the speedup graph (left) and the horizontal subsolver-colored lines in the total cost graph (right) indicate 95% solution qualities for the corresponding subsolvers. The styled vertical lines in the improvement graph (middle) and the total cost graph (right) indicate computation times required for the corresponding learning-based methods to reach the corresponding aforementioned solution qualities.

![Image](image_000028_6fac5513ba510667b49907b104c962f853121c0faf85eed5d9e547030b3c2749.png)

Initialization solution. Let L denote the number of LKH steps run on each partition at initialization. Previously, L = 100 as stated in Appendix A.1. To generate initial solutions of poorer quality, we additionally generate initial solutions with L = 1 5 10 , , , and 50 . As a last initialization method, which does not use LKH-3 and is denoted L = 0 , we uniformly randomly chain cities into routes with no consideration for their location. We do not train or finetune any models for this ablation, instead evaluating the performance of our previous uniform CVRP model (trained on L = 100 initializations) on all other initialization methods. Note that we calculate the speedup and improvement of all methods with respect to LKH-3 with L = 100 initialization.

Experimental results. We provide experimental results in Figure 21. We find that for each subproblem selection method (Ours and Random), worse initialization corresponds to somewhat worse speedup and worse improvement over LKH-3 with L = 100 . Nevertheless, Ours outperforms Random for every L considered, demonstrating that our subproblem selector trained on L = 100 data generalizes well to out-of-distribution initial solutions. We are also able to show that subproblem selection with even the worst initialization offers a speedup above 1x for attaining 95% of L = 100 LKH-3 solution quality.

## A.5.5 Transformer vs Simpler Architectures

We seek to illustrate the importance of our Transformer regression architecture illustrated in Figure 2 by comparing with subproblem selectors with simpler architectures trained on uniform CVRP.

Figure 18: Clustered and mixed CVRP distributions, N = 500 , LKH-3 and HGS subsolvers.

![Image](image_000029_002d2b2051c0f2e81600690fcd3e956587797f80dd60ecf7571cdc99da89dc35.png)

Figure 19: Clustered and mixed CVRP distributions, N = 1000 , LKH-3 and HGS subsolvers.

![Image](image_000030_99c7afd55c28506432fa1bea2abff63afa2d5255a7d53b38f372bb50bbbd57ec.png)

Figure 20: Clustered and mixed CVRP distributions, N = 2000 , LKH-3 and HGS subsolvers.

![Image](image_000031_6ffc3dceafde6c789e406552eb5ebbeca8a7b99b3de2e3b14a624bfc243977e6.png)

Figure 21: Uniform CVRP distributions, various initialization qualities. Graphs of speedup (left column, higher is better), improvement (middle column, higher is better), and total cost (right column, lower is better). The right vertical axis in each improvement graph (middle column) indicates the improvement over LKH-3 as a percentage of the total improvement that LKH-3 attains over 30k steps. We use increasingly dotted lines to indicate increasingly worse initialization methods. The horizontal dotted black line in the total cost graph (right) indicates the initial solution quality for all methods. The vertical dashed black line in the speedup graph (left) and the horizontal LKH-3-colored line in the total cost graph (right) indicate solution qualities equal to 95% of the corresponding best LKH-3 solution quality. The styled vertical lines in the improvement graph (middle) and the total cost graph (right) indicate computation times required for the corresponding learning-based methods to reach the corresponding aforementioned solution quality.

![Image](image_000032_c1e6adaef43b12e401abbf50af3a957e73da47bafafa6eb284b159310d1123c1.png)

Experimental setup. We describe several simpler architectures, some of which require changes to the input subproblem representation.

- · FCNN : using the same data (positions and demands of cities) as described in Section A.1, we replace the Transformer attention layers by a fully-connected neural network.
- · Linear, MLP, RandomForest : we design a new input representation which featurizes each subproblem into 33 summary features, including the number of cities, the bounds of the subproblem, the spread of cities, 10 radial distance percentiles of cities from the depot, 10 radial distance percentiles of cities from the subproblem centroid, and the distribution of city demands. On top of this input representation, we fit simple regression models: linear regression (Linear), a shallow fully-connected neural network (MLP), and a Random Forest regressor (RandomForest).

To train the simpler subproblem selectors, we use the same instances and training data as training the Transformer subproblem selector. Hyperparameters were lightly tuned by hand.

Experimental results. The validation mean squared error for ablation architectures are similar to each other (ranging from 0.02 to 0.03) and much higher than that of our Transformer architecture (0.003). Due to the large CPU memory consumption of RandomForest at inference time and its similar validation mean squared error to the other ablation architectures, we do not evaluate RandomForest's subproblem selection ability. Figure 22 demonstrate the performance of our iterative framework with

other subproblems selectors. Interestingly, we see that ablation architectures initially, for roughly the first 5 seconds, perform comparably to our Transformer architecture. However, the quality of the solution gradually diverges from our Transformer architecture and converges to the Random baseline over the first 100 to 400 seconds. MLP, which uses summary feature inputs, performs the best out of all three ablations, though Linear offers similar performance on N = 500 and 1000 . Our studies show that our Transformer architecture is key for obtaining good performance over the Random baseline, especially for obtaining higher solution qualities, though summary features are surprisingly effective for subproblem selection even when combined with the interpretable Linear architecture.

Figure 22: Uniform CVRP distributions, ablation architectures. Graphs of speedup (left column, higher is better), improvement (middle column, higher is better), and total cost (right column, lower is better). The right vertical axis in each improvement graph (middle column) indicates the improvement over LKH-3 as a percentage of the total improvement that LKH-3 attains over 30k steps. We indicate ablation architectures in pink. The horizontal dotted black line in the total cost graph (right) indicates the initial solution quality for all methods. The vertical dashed black line in the speedup graph (left) and the horizontal LKH-3-colored line in the total cost graph (right) indicate solution qualities equal to 95% of the corresponding best LKH-3 solution quality. The styled vertical lines in the improvement graph (middle) and the total cost graph (right) indicate computation times required for the corresponding learning-based methods to reach the corresponding aforementioned solution quality.

![Image](image_000033_7a1d66756a39b533557ee96e3ac19ff6ab4ab4236d667308729a361427efda66.png)

## A.6 Applicability of Our learning-to-delegate Framework to Other Problems in Combinatorial Optimization (CO)

First, our method can be seen as a learning approach to accelerate the broadly applicable POPMUSIC framework on large-scale CO problems (discussed in the related work, Section 3), where we learn principled improvement-based criteria for subproblem selection, rather than relying on random or heuristic subproblem selection. In short, as long as a restricted subproblem space can be defined, we expect an existing subsolver can be accelerated with our learning-to-delegate method. We thus give a few such example CO problems:

- · Traveling salesman problem (TSP): in the closely related TSP problem, a restricted subproblem selection space can be defined as subpaths of a fixed length in the current solution path, as done in [41, 14]. Then, at each iteration, the solution for the unselected path segment is held fixed, while the selected subpath is finetuned by a subsolver.
- · Other graph problems: similarly, the general POPMUSIC framework is also applied to graph problems such as map labeling (max independent set) [22], where each subproblem can be defined as the k-hop neighborhoods of a centroid node, which is also a linear subproblem selection space. Naturally, the POPMUSIC framework should also apply to related graph problems such as graph cut and minimum vertex cover.
- · Other problems with spatial or temporal locality: similar to our subproblem space definition, problems with spatial or temporal locality can leverage this property to reduce the space. Such examples include berth allocation (which closely relates to job scheduling) [39] and p-median clustering [34].

The concrete examples given here provide a wide array of important and large-scale CO problems which we expect to benefit from learning-to-delegate. Although our method evaluation focuses on VRP, the key contribution of our method is that, given a subproblem space of tractable size, it can learn a selection strategy to drastically accelerate a subsolver, by pinpointing promising subproblems to increase the objective. On the other hand, the previous methods [41, 14, 22, 39, 34] rely on random subproblem selection or simple heuristics to select the subproblems, which expends a large amount of computation solving subproblems that already have good subsolutions.

Second, our method also has the potential to apply to CO problems with a larger subproblem space. For instance, augmenting our method with sampling-based strategies could be promising. We could consider training our subproblem regression network by sampling a number of subproblems at each step to generate a training set. To generate full training trajectories across multiple steps, we can 1) naively choose the greedy optimal subproblem, or 2) use an active learning approach to balance exploration (understanding what kinds of subproblems are promising) with exploitation (improving the objective). While 1) is a straightforward application of our current framework, 2) is an interesting extension that is worthy of further exploration.

Finally, we note that although we focus on VRPs, we design the evaluation of the approach with an eye towards generality. Specifically, VRPs have a rich family of variations, including CVRP, CVRPTW, VRPMPD, as well as different data distributions (among many others), which allows us to demonstrate a degree of generality of the method. Although they are related, each of these problem modifications considerably changes the nature of the underlying CO problem; for example, CVRPTW exhibits an element of scheduling, whereas (C)VRP does not.