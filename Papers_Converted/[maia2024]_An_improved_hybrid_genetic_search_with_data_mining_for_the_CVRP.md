![Image](image_000000_d5f9ea3ef9d1ae02331059f91a86918336b1e9a5c591ab8b79e11ab4c080895b.png)

## An improved hybrid genetic search with data mining for the CVRP

Marcelo Rodrigues de Holanda Maia 1,2 Alexandre Plastino 1 Uéverton dos Santos Souza 1

1

## Abstract

The hybrid genetic search (HGS) metaheuristic has produced outstanding results for several variants of the vehicle routing problem. A recent implementation of HGS specialized to the capacitated vehicle routing problem (CVRP) is a state-of-the-art method for this variant. This paper proposes an improved HGS for the CVRP obtained by incorporating a new solution generation method into its (re-)initialization process to guide the search more efficiently and effectively. The solution generation method introduced in this work combines an approach based on frequent patterns extracted from good solutions by a data mining process and a randomized version of the Clarke and Wright savings heuristic. As observed in our experimental comparison, the proposed method significantly outperforms the original algorithm regarding the final gap to the best known solutions and the primal integral.

## KEYWORDS

data mining, hybrid genetic search, machine learning, metaheuristics, network optimization, vehicle routing

## 1 INTRODUCTION

In the last decade, the hybrid genetic search (HGS) metaheuristic has produced outstanding results for several variants of the vehicle routing problem (VRP), standing as a state-of-the-art method for this family of combinatorial optimization problems [58-63]. Its most recent implementation, specialized to the capacitated vehicle routing problem (CVRP), has outperformed the main algorithms available for this VRP variant in extensive experimental comparisons [59]. The results obtained by this specialized algorithm, referred to as HGS-CVRP, put it in a leading position among the existing CVRP methods. As improving the current top-performing method's results would represent a significant accomplishment, we aimed to produce an improved version of HGS for the CVRP in this work.

The combination of crossover- and neighborhood-based search in HGS, especially with the SWAP* neighborhood structure introduced in HGS-CVRP, provides an excellent balance of diversification and solution improvement. Hence, there is little room for improvement in those components. On the other hand, its solution initialization component relies on random permutations of customers. An adequate balance of diversification and intensification in the initialization of metaheuristics can also be crucial for their performance [49]. Therefore, we saw a research direction with great potential for improvement in this gap. By introducing a new solution generation method into the population (re-)initialization process, we could guide the search more efficiently and effectively.

Hence, this paper proposes an improved version of HGS for the CVRP, obtained by incorporating a new pattern-based solution generation method into the (re-)initialization process, invoked when the algorithm starts and whenever a certain number of iterations without improvement is reached. This new solution generation method relies on (i) an approach we refer to as MDM

Networks . 2024;83:692-711.

Instituto de Computação, Universidade Federal Fluminense, Niterói, Brazil 2 Escola Nacional de Ciências Estatísticas, Instituto Brasileiro de Geografia e Estatística,

Rio de Janeiro, Brazil

## Correspondence

Marcelo Rodrigues de Holanda Maia, Instituto Brasileiro de Geografia e Estatística, Rio de Janeiro, RJ, Brazil. Email: marcelo.h.maia@ibge.gov.br

## Funding information

Fundação Carlos Chagas Filho de Amparo à Pesquisa do Estado do Rio de Janeiro, Grant/Award Numbers: E-26/201.344/2021, E-26/201.139/2022; Conselho Nacional de Desenvolvimento Científico e Tecnológico, Grant/Award Numbers: 309832/2020-9, 315750/2021-9; Instituto Brasileiro de Geografia e

Estatística (IBGE, Brazil).

(which stands for Multi Data Mining ) that uses frequent patterns extracted from good solutions by a data mining process [41] and (ii) a randomized version of the Clarke and Wright savings heuristic [16].

The Clarke and Wright heuristic effectively produces a relatively good solution in a short computing time. It has been used for initializing solutions in some recent state-of-the-art methods [2, 3, 5].

The MDM approach was initially proposed for a hybrid version of the GRASP metaheuristic (MDM-GRASP). However, it was later used with other multi-start metaheuristics, including a multi-start iterated local search for the heterogeneous fleet VRP [32]. The MDM approach has been applied exclusively in single-point metaheuristics, usually memoryless ones. To the best of our knowledge, this work reports the first attempt to apply it in a population-based metaheuristic with an evolutionary framework. Hence, one of the contributions of the present work is that it helps to elucidate whether this approach can also be helpful in such a context.

We have compared the original HGS-CVRP and our proposed algorithm, which we call MDM-HGS, in computational experiments evaluating both with respect to the final gap to the best known solutions and the primal integral (PI), a performance measure that rewards a balance of convergence speed and solution quality [8]. The results show that our version of HGS significantly outperforms the original one, improving solution quality and converging faster.

Furthermore, our proposed method was compared to several new algorithms in the 12th DIMACS Implementation Challenge. 1 The challenge results show that our proposed method is very competitive. In Phase 1, it ranked first for two sets of instances from the literature, and in Phase 2, it ranked first for three classes of instances and tied in first for another one.

The remainder of this paper is organized as follows. Section 2 discusses related work. We describe our proposed method in Section 3. Section 4 reports results obtained in our computational experiments and in the 12th DIMACS Implementation Challenge. Finally, conclusions are presented in Section 5.

## 2 RELATED WORK

## 2.1 State-of-the-art for the CVRP

This work considers the canonical CVRP [54], described as follows. Let G = ( V E , ) be a graph, where V = { 0 1 , , … , n - } 1 is a set composed of n vertices and E = {{ i , j } ∶ i , j ∈ V i , &lt; j } is the set of edges. Vertex 0 is the depot from where the vehicles depart, whereas the set V ′ = V /uni29F5 { 0 } consists of the remaining vertices representing n -1 customers. Each customer i ∈ V ′ is associated with a non-negative quantity qi that specifies the demand for some resource, and a vehicle can carry a maximum quantity Q of the resource. Each edge { i , j } ∈ E has an associated cost /u1D451 ij representing the distance between vertices i and j . A route is defined by a sequence of vertices R = ( i 1 , i 2 , … , i | R | ) , i 1 = i | R | = 0, and { i 2 , … , i | R | -1 } ⊆ V ′ ; that is, each route is a circuit in G that starts and ends at the depot. Route R is feasible if the sum of all customers' demands on R is within the vehicle capacity Q . A route's cost is the sum of the costs (distances) associated with each traversed edge. The aim is to discover feasible routes where each customer is visited precisely once, and the sum of all route costs is minimized. There are no restrictions on the number of routes in a solution.

CVRP is a well-studied variant among vehicle routing problems. Hence, several methods to solve it can be found in the literature. Here we list recent algorithms which are representative of the current state-of-the-art for this problem: ILS-RVND-SP, a hybrid algorithm that combines an exact procedure based on a Set Partitioning formulation with an iterated local search (ILS) heuristic [52]; KGLS, a guided local search with knowledge-driven penalization [5]; SISRs, an algorithm based on a ruin-and-recreate approach [13]; FILO, an algorithm that combines an ILS with simulated annealing acceptance criteria, designed for large-scale instances [2]; AILS-PR, a hybrid adaptive ILS with Path Relinking [38]; and finally, HGS-CVRP, an implementation of the HGS metaheuristic specialized to the CVRP [59], which was compared to several other algorithms, including four of the five mentioned above (the only exception being AILS-PR). The reported experimental results demonstrated that HGS-CVRP outperformed all other algorithms regarding solution quality and convergence speed, standing as the leading method in the state-of-the-art for the CVRP.

Several new methods were recently introduced in the CVRP track of the 12th DIMACS Implementation Challenge. The eight finalist methods, in particular, achieved quite competitive results. Unsurprisingly, six of those eight methods are based on the HGS metaheuristic. Apart from the MDM-HGS algorithm, which we present in this paper:

- · FHCSolver [27], which combines HGS-CVRP with an adapted version of FILO;
- · POP-HGS [42], an implementation of the POPMUSIC metaheuristic [43] where subproblems are solved by HGS-CVRP;
- · HGSRR [50], which combines HGS-CVRP with the ruin-and-recreate approach from SISRs;

- · MAESN [68], an extension of HGS-CVRP with six new local search operators; and
- · GA TBD [65], an adaptation of the HALNS algorithm [64] with a local search based on HGS-CVRP. +

The other two finalist methods were: AILS-II [37], which is a version of AILS [38] adapted for large-scale instances; and FSP4D [1], which is a combination of FILO with a set partitioning heuristic.

## 2.2 Data science techniques applied to VRP solving

Research on the synergies between metaheuristics and data science-which relates to fields like data mining and machine learning (ML)-has become increasingly popular in the combinatorial optimization literature [9, 17, 28, 34, 35, 53, 67]. Metaheuristics can help solve ML problems that can be modelled as optimization problems. Conversely, ML-based approaches have been applied to improve the performance of metaheuristics. Concerning VRPs, ML-based approaches have been applied for diverse purposes.

Problem decomposition approaches aim at breaking large-scale optimization problems into smaller subproblems, which can be solved more efficiently. Clustering techniques have been used to partition the input graph of VRPs [21, 23, 40, 44]. Frequent itemset mining techniques have been applied to the online discovery of patterns in good solutions, which are used to decompose the decision space by fixing some variable values and then solving the associated reduced subproblem [33].

ML-based approaches have also been applied in the design of search operators. Approaches that extract and combine patterns with greedy operators have been incorporated into constructive procedures by applying clustering [18] and frequent itemset mining [19, 32, 47]. As reinforcement learning techniques constitute a natural sequential learning framework, they have also been applied in constructive procedures for VRPs [20, 29, 30, 39, 66]. Local search move operators have also been designed with the application of reinforcement learning [11] and frequent itemset mining [6].

Optimization approaches often combine multiple search operators. Hence, the strategies used for search operator selection play an essential role in those approaches. The neighborhood selection in variable neighborhood search algorithms has been addressed using the multi-armed bandit framework [12] and reinforcement learning [10].

As multiple and diverse algorithms for solving VRPs have arisen in the literature, the algorithm selection problem gained attention. New approaches have been proposed that use a pool of distinct algorithms and select which to apply based on the features of each problem instance. Classification techniques have been used to learn algorithm selectors for VRPs [26, 55, 56].

Classification techniques have also been used to dynamically guide the search in metaheuristics based on solution features. This strategy was applied to identify and penalize 'bad' edges [4, 5], to identify promising regions in the features space and make the search focused on those regions [31], and to decide which initial solutions are worthy applying local search in a multi-start algorithm [36].

In this work, we adapt the approach from [32] for the heterogeneous fleet VRP to the CVRP. According to Talbi's taxonomy [53], the proposed method is classified as an online low-level ML-supported metaheuristic since data mining is used in a process that drives the initialization of solutions based on knowledge gathered during the search while solving the problem.

## 3 HGS WITH DATA MINING FOR THE CVRP

This section presents the improved version of HGS with data mining we propose for the CVRP. A general description of HGS-CVRP is given in Section 3.1. Section 3.2 describes MDM-HGS, our proposed version of HGS-CVRP.

## 3.1 HGS-CVRP: The base algorithm

HGS-CVRP is a population-based hybrid algorithm that combines neighborhood search strategies with a genetic crossover operator. Its general structure is presented in Figure 1A.

At a high abstraction level, HGS-CVRP works as follows. In the initialization phase, it generates 4 /u1D707 solutions (where /u1D707 is a parameter representing the minimum population size) by applying an optimal split algorithm to random permutations of the customers, improves them through local search, and inserts them into the population. Every solution insertion is followed by a population management mechanism that balances solution quality and diversity. Once the population is initialized, the algorithm starts an iterative process comprising the blocks labeled 'hybrid evolutionary framework' and 'insertion in the population' in Figure 1A. The first consists of three steps: (i) selecting two parent individuals from the population, (ii) recombining them by applying the crossover operator to produce an offspring, and (iii) improving the offspring solution with a local search. The following block inserts the resulting solution into the population and applies the population management mechanism. This iterative process is repeated until N IT consecutive iterations without improvement are completed or a time limit TMAX is exceeded.

FIGURE 1 General structures of HGS-CVRP and MDM-HGS. (A) HGS-CVRP (B) MDM-HGS.

![Image](image_000001_57a06d9424824e839f6981a8628b53ee96415fde342e28037c36428b20a82cd1.png)

If TMAX is defined, the algorithm restarts, reinitializing the population, whenever it completes N IT consecutive iterations without improvement.

## 3.2 MDM-HGS

Multi Data Mining (MDM) is an approach for incorporating data mining into metaheuristics that was initially proposed for a hybrid version of GRASP named MDM-GRASP [41], which was, in turn, an adaptive version of the DM-GRASP metaheuristic [45, 48]. In DM-GRASP, an elite set  keeps the best solutions found in the first half of the multi-start iterations. Then, a data mining method is used to extract patterns from  , which are used to build initial solutions in the second half.

In the MDM approach,  keeps the /u1D6FC best solutions found during the whole execution of the metaheuristic. The data mining method is invoked whenever  is considered stable (usually when it does not change for a number of consecutive multi-start iterations), and the obtained patterns are used in the subsequent iterations.

This approach has been coupled exclusively with single-point metaheuristics. It has been typically used to improve originally memoryless multi-start local search methods, which benefit from the knowledge accumulated in the elite set and mined from it in the form of patterns. Nonetheless, it has been shown that even methods with some other memory-based strategy, like path-relinking, can benefit from this approach [34]. However, to the best of our knowledge, this work is the first attempt to apply the MDM approach in a population-based metaheuristic with an evolutionary framework. Hence, the present study helps to elucidate whether this approach can also be helpful in such a context.

The data mining method is based on the FPmax ∗ algorithm [25], which mines maximal frequent itemsets. An itemset is considered frequent if it achieves a given minimum support value, that is, if it is present in at least a given minimum portion of the elite set solutions.

MDM-HGS is a new version of HGS-CVRP that applies the MDM approach. Figure 1B presents its general structure. In contrast with Figure 1A, two new components are introduced: the pattern mining method and a new individual initialization method, which uses a pattern-based randomized version of the well-known Clarke and Wright (CW) heuristic to generate

10970037, 2024, 4, Downloaded from https://onlinelibrary.wiley.com/doi/10.1002/net.22213 by Technische UniversitGLYPH&lt;228&gt;t München, Wiley Online Library on [30/03/2025]. See the Terms and Conditions (https://onlinelibrary.wiley.com/terms-and-conditions) on Wiley Online Library for rules of use; OA articles are governed by the applicable Creative Commons License

FIGURE 2 Four distinct solutions for a CVRP instance and a pattern with 100% support.

![Image](image_000002_08955e9873fe411d855b04f070d4383af6535be5bc7ef43bc9842f0487f62f84.png)

solutions. Additionally, the component responsible for inserting individuals into the population is modified to manage the elite set updates.

The realization of the MDM approach elements in our MDM-HGS implementation 2 is similar to that adopted in an existing multi-start ILS for the heterogeneous fleet VRP [32]. The size of a pattern is defined as the number of edges in it. The data mining method returns the /u1D6FD largest frequent patterns with minimum support /u1D6FE found in  . Each pattern is a set of paths connecting customers, which appear together in at least ⌊ /u1D6FE |  |⌋ solutions from  . The difference to the heterogeneous fleet variant is the assignment of a vehicle type to each path, which does not apply to the canonical CVRP addressed in this work. These sets of paths represent elements that frequently appear together in good solutions. Hence, the mined patterns are used for solution initialization to guide the search through more promising regions in the solution space. Figure 2 illustrates this data mining process with four distinct solutions for a CVRP instance. Suppose these solutions compose an elite set for the corresponding instance and let the minimum support be 100%. In that case, the data mining method would extract a pattern composed of the paths that appear in all four solutions (thick lines in the figure).

 is updated whenever a new feasible solution is inserted in the population if  is not full or the new solution is better than the worst solution in  . Recall that HGS-CVRP reinitializes the population whenever it completes N IT consecutive iterations without improvement.  is considered stable when it remains unchanged after a number of population restarts. That number is dynamically defined based on an algorithm parameter ( /u1D6FF ) and an estimated total number of restarts within the time limit: RMAX = ⌊ TMAX ∕ T R ⌋ , where T R is the computed average time the algorithm has taken to restart so far. Precisely,  is considered stable when one of the following criteria is met: (i)  has not been mined yet, and ⌊ /u1D6FF RMAX ⌋ consecutive restarts were carried out since its last change, or (ii)  has changed after it was last mined, but ⌊ /u1D6FF RMAX ⌋ consecutive restarts were carried out since its last change. Note that ⌊ /u1D6FF RMAX ⌋ may be zero when RMAX is small (which happens for large CVRP instances). In that case,  is potentially mined upon every restart (as long as it changes between restarts).

Figure 3 presents the general structure of our new individual initialization component, which is based on mined patterns and a randomized version of the CW heuristic.

Its randomization mechanism is a roulette wheel selection as in the CW implementation discussed in [51]. The savings list S is built when the problem instance data is loaded. It contains all edges that connect two customers in the input graph, sorted in

FIGURE 3 Individual initialization using a pattern-based randomized CW heuristic.

![Image](image_000003_56af27b4fda252e076d7e934017f6da8f1eaeef436912779c4b313b056e212ac.png)

## Algorithm 1. Pattern-based randomized CW solution generation

- 1: Initialize a route with each path in pattern p ;
- 2: Initialize a route with each customer that is not in p ;
- 3: while S contains unvisited positive-valued entries do
- 4: t ← a random number from the interval [ 2 6 ; , ]
- 5: ⟨ i , j ⟩ ← ROULETTEWHEEL( , ); S t
- 6: if i and j are in two different routes, both are adjacent to the depot, and the vehicle capacity is sufficient then
- 7: Concatenate the two routes through ⟨ i , j ⟩ ;
- 8: end if
- 9: end while
- 10: for all routes with only one customer i do
- 11: if there is a route with more than one customer and at least qi residual vehicle capacity then

12:

- Make the cheapest feasible move of i to the end of a route with more than one customer;
- 13: else
- 14: Make the cheapest infeasible move of i to the end of a route with more than one customer;
- 15: end if
- 16: end for
- 17: Return the generated solution;

decreasing order of their corresponding savings values. A savings value represents the cost savings obtained by concatenating two routes, one where customer i is adjacent to the depot and another where customer j is adjacent to the depot, via the insertion of the edge ⟨ i , j ⟩ in the solution. It is given by sij = /u1D451 0 i + /u1D451 0 j -/u1D451 ij , where /u1D451 ij is the distance between vertices i and j , and vertex 0 is the depot.

The randomized CW solution generation based on a pattern p , which comprises the first two blocks in Figure 3, is described in Algorithm 1. The solution is initialized with one route for each path in p (line 1) and one for each customer not in p (line 2). Then the savings list is iteratively processed (lines 3-9). At each step, an edge ⟨ i , j ⟩ is selected among the t (a random number between 2 and 6, inclusive) first unvisited entries in S using the roulette wheel method (line 5). If customers i and j are both adjacent to the depot, on two different routes, and the vehicle capacity is sufficient for the total demand of both routes, then these routes are concatenated through the insertion of ⟨ i , j ⟩ in the solution (line 7). Finally, after S has been processed, we adopt a greedy strategy to avoid an excessive number of routes: for each route with only one customer i , i is moved to the end of a route with more than one customer. Each of those moves is the cheapest among all options, feasible if possible (line 12), infeasible otherwise (line 14).

In the population initialization, MDM-HGS generates ( 1 -/u1D702 ) /u1D700/u1D707 solutions using the method described in Algorithm 1 and /u1D702 /u1D700/u1D707 random solutions using the original method from HGS-CVRP. Every generated solution is improved through the original HGS-CVRP local search method and inserted into the population, as shown in Figure 3.

Two new parameters are introduced. /u1D702 ∈ ( 0 1 , ) controls the initial population randomness to keep an appropriate level of diversification. /u1D700 replaces the constant 4 from HGS-CVRP as we considered that generating 4 /u1D707 solutions for initializing the population could excessively delay convergence, so we wish to try lower values for this factor.

Note that the pattern set is empty when the population is first initialized. In that case, pattern p in Algorithm 1 is an empty set, so the method behaves like a simple randomized CW heuristic, skipping the pattern insertion step as shown in Figure 3. After the data mining method is activated, the mined patterns are organized into a circular list, ordered by size. Then,

the next pattern in that list is selected for each solution generated by Algorithm 1. Since we adopt a randomized version of the CW heuristic, even if the same patterns are selected multiple times (when /u1D6FD &lt; ( 1 -/u1D702 ) /u1D700/u1D707 ), different individuals will be generated.

## 4 EXPERIMENTAL ANALYSES

This section reports experimental analyses and results of our proposed method, MDM-HGS. Our computational environment comprised AMD EPYC 7532 @ 2.4 GHz CPUs running Linux. The benchmark instances and the assessment metrics adopted in this work are presented in Sections 4.1 and 4.2, respectively. In Section 4.3, we describe the parameters introduced in the method and report a sensitivity analysis that evaluates the impact of variations in the values of these parameters. Section 4.4 describes an experimental analysis conducted to assess the proposed strategies' impact on performance, whereas Section 4.5 presents results from the Challenge comparing MDM-HGS to the other competitor algorithms.

## 4.1 Benchmark instances

This work considers the benchmark instances adopted in the CVRP track of the 12th DIMACS Implementation Challenge, which was divided into two phases. Phase 1 used 141 instances, available in CVRPLIB. 3 They can be grouped into five subsets:

- · Classic , which comprises seven classic benchmark instances from the literature: E-n101-k8 and E-n101-k14 (from Set E in CVRPLIB) [14]; CMT4 and CMT5 [15]; F-n135-k7 (from Set F in CVRPLIB) [22]; P-n101-k4 (from Set P in CVRPLIB) [7]; and tai385 [46].
- · Golden , which comprises 12 instances: Golden\_9-Golden\_20 [24].
- · Belgium , which comprises 10 very large instances (having from 3000 to 30 000 customers): Antwerp1, Antwerp2, Brussels1, Brussels2, Flanders1, Flanders2, Ghent1, Ghent2, Leuven1, and Leuven2 [3].
- · X , a large and diversified set comprising 100 benchmark instances [57].
- · DIMACS , which comprises 12 instances contributed to the Challenge by companies Loggi and ORTEC: Loggi-n401-k23, Loggi-n501-k24, Loggi-n601-k19, Loggi-n601-k42, Loggi-n901-k42, Loggi-n1001-k31, ORTEC-n242-k12, ORTEC-n323-k21, ORTEC-n405-k18, ORTEC-n455-k41, ORTEC-n510-k23, and ORTEC-n701-k64.

We adopt the same conventions used in the Challenge for those benchmarks. Specifically:

- · No constraints are applied to the number of routes in a solution.
- · Distances for instances from the DIMACS subset are explicitly given in the instance files.
- · Euclidian distances with full precision are used for 'CMT', 'Golden' and 'tai' instances.
- · Euclidian distances rounded to the nearest integers are used for all other instances.

Phase 2 of the Challenge used 100 new instances (not disclosed), statistically similar to the X instances, produced by the same random generator using a different seed.

## 4.2 Assessment metrics

In this work, we center our experimental analyses on a metric for measuring the quality of solutions obtained within a given time limit. Specifically, after multiple runs of an algorithm on a CVRP instance, each one outputting the cost of the best solution found ( ), we compute the percentage gap of the average cost obtained to that instance's best known solution (BKS): z Gap = 100 ( z -BKS )∕ BKS .

Secondarily, we also consider the metric used to measure and compare the performances of the algorithms in the CVRP track of the 12th DIMACS Implementation Challenge, which was the primal integral (PI) [8]. This metric combines solution quality and convergence speed into a single score, rewarding a balance between these features.

The PI score is computed as follows. The BKS for the instance in consideration and the time limit for the algorithm to run are given a priori. A relative gap to the BKS of 10% is assumed when the algorithm starts. Then, whenever the algorithm finds an improving solution, the new gap to the BKS and the portion of the total time limit spent to find it are recorded. Figure 4 presents a chart that illustrates this scenario.

The data recorded during the entire execution of the algorithm form a curve like the one presented in the chart. The point marked for time zero refers to the initial gap, each internal point refers to an improving solution found during the search, and the

FIGURE 4 Primal integral.

![Image](image_000004_98a863b053490ad2540095bf1b4090d3066cfa937d4c77799974c890fae9161c.png)

TABLE 1 MDM-HGS parameters and values considered while tuning for the DIMACS challenge.

| Parameter   | Description                                          | Considered values                |
|-------------|------------------------------------------------------|----------------------------------|
| /u1D6FC     | Elite set capacity                                   | { 5 , 10 , 15 }                  |
| /u1D6FD     | Number of patterns mined from the elite set          | { 1 , 2 , … , 10 }               |
| /u1D6FE     | Relative minimum support of mined patterns           | { 0 . 4 , 0 . 5 , … , 1 . 0 }    |
| /u1D6FF     | Coefficient used in elite set stabilization criteria | { 0 . 03 , 0 . 04 , … , 0 . 07 } |
| /u1D700     | Controls the number of initial individuals generated | { 1 , 2 , 3 , 4 }                |
| /u1D702     | Initial population randomness level                  | { 0 . 1 , 0 . 2 , … , 0 . 9 }    |

point marked for time 1 (100% of the time limit) refers to the final gap. The PI score is given by the area between the curve and the 'zero-gap" line (the smaller, the better). Note that an algorithm that does not find a solution with a gap to the BKS under 10% gets PI = 10, the worst possible score. If the algorithm finds solutions better than the BKS, the gap will become negative, like in the chart. In that case, the PI score may also be negative (if the area below the zero-gap line is greater than above it).

On the one hand, if the algorithm takes too long to reach good solutions, even if they are near-optimal, the area under the curve will be large. On the other hand, if it quickly converges to a good solution but cannot improve afterwards, the area under the curve will also be large. Hence, to obtain a good PI score, the algorithm should rapidly converge to a good solution and keep improving it as much as possible.

Note that the BKS values used for computing the metrics in our experimental analyses were retrieved from the CVRPLIB website on May 4, 2023. In contrast, the BKS values adopted in the Challenge are from December 16, 2021. Furthermore, in our experiments, we adopted CPU time as the time reference, whereas wall clock time was adopted in the Challenge. A CPU time limit of 1800 s was set for instances with n ≤ 200, 3600 s for 200 &lt; n ≤ 400, and 7200 s for n &gt; 400 (the same values adopted in the Challenge).

## 4.3 Parameters tuning and sensitivity analysis

The values for the HGS-CVRP parameters were kept as defined in [59]. The parameters introduced in MDM-HGS and the respective values considered in the tuning process carried out for the CVRP track of the 12th DIMACS Implementation Challenge are shown in Table 1. They have been tuned using the benchmark X instances via an iterative process where, at each step, one parameter value varied while the remaining parameter values were fixed. We adopted an adaptive parameter configuration strategy, where the best configuration is chosen depending on the instance size. The instances have been separated into three groups corresponding to size ranges based on the number of vertices n . The tuning process was conducted to minimize the average primal integral for each range independently. Table 2 presents the best configurations found, which were applied in the Challenge.

To evaluate the sensitivity of MDM-HGS to changes in these parameters, we started from a baseline configuration obtained by averaging the values from each column in Table 2: /u1D6FC = 7, /u1D6FD = 5, /u1D6FE = 0 8, . /u1D6FF = 0 05, . /u1D700 = 1, /u1D702 = 0 4. .

TABLE 2 Best parameter configurations from the initial tuning (used in the DIMACS challenge).

| Range         |   /u1D736 |   /u1D737 |   /u1D738 |   /u1D739 |   /u1D73A |   /u1D73C |
|---------------|-----------|-----------|-----------|-----------|-----------|-----------|
| n ≤ 200       |         5 |         5 |       0.8 |      0.05 |         1 |       0.1 |
| 200 < n ≤ 400 |        10 |         5 |       0.8 |      0.05 |         1 |       0.8 |
| n > 400       |         5 |         5 |       0.8 |      0.05 |         1 |       0.2 |

TABLE 3 Parameter sensitivity analysis.

| Parameter configuration   | Parameter configuration   | Parameter configuration   | Parameter configuration   | Parameter configuration   | Parameter configuration   | n ≤ 200   | n ≤ 200   | 200 < n ≤ 400   | 200 < n ≤ 400   | 400 < n ≤ 2000   | 400 < n ≤ 2000   | n > 2000   | n > 2000   |
|---------------------------|---------------------------|---------------------------|---------------------------|---------------------------|---------------------------|-----------|-----------|-----------------|-----------------|------------------|------------------|------------|------------|
| /u1D736                   | /u1D737                   | /u1D738                   | /u1D739                   | /u1D73A                   | /u1D73C                   | Gap       | Avg PI    | Gap             | Avg PI          | Gap              | Avg PI           | Gap        | Avg PI     |
| 7                         | 5                         | 0.8                       | 0.05                      | 1                         | 0.4                       | 0.003%    | 0.011154  | 0.031%          | 0.053212        | 0.148%           | 0.224468         | 5.152%     | 5.750385   |
| 7                         | 5                         | 0.8                       | 0.05                      | 1                         | 0.1                       | 0.006%    | 0.014091  | 0.025%          | 0.053938        | 0.146%           | 0.231431         | 4.990%     | 5.569519   |
| 7                         | 5                         | 0.8                       | 0.05                      | 1                         | 0.2                       | 0.003%    | 0.010968  | 0.028%          | 0.055210        | 0.139%           | 0.218805         | 5.000%     | 5.623506   |
| 7                         | 5                         | 0.8                       | 0.05                      | 1                         | 0.3                       | 0.004%    | 0.010972  | 0.026%          | 0.052954        | 0.150%           | 0.233628         | 5.069%     | 5.687517   |
| 7                         | 5                         | 0.8                       | 0.05                      | 1                         | 0.5                       | 0.004%    | 0.011087  | 0.024%          | 0.050604        | 0.149%           | 0.231711         | 5.131%     | 5.741699   |
| 7                         | 5                         | 0.8                       | 0.05                      | 1                         | 0.6                       | 0.007%    | 0.014953  | 0.022%          | 0.048346        | 0.143%           | 0.229144         | 5.114%     | 5.791826   |
| 7                         | 5                         | 0.8                       | 0.05                      | 1                         | 0.7                       | 0.006%    | 0.012548  | 0.022%          | 0.047835        | 0.153%           | 0.234664         | 5.258%     | 5.854093   |
| 7                         | 5                         | 0.8                       | 0.05                      | 1                         | 0.8                       | 0.006%    | 0.013537  | 0.020%          | 0.042631        | 0.152%           | 0.230257         | 5.208%     | 5.924860   |
| 7                         | 5                         | 0.8                       | 0.05                      | 2                         | 0.4                       | 0.006%    | 0.012172  | 0.027%          | 0.053408        | 0.143%           | 0.219421         | 5.174%     | 5.904006   |
| 7                         | 5                         | 0.8                       | 0.05                      | 3                         | 0.4                       | 0.005%    | 0.012586  | 0.029%          | 0.054362        | 0.158%           | 0.241011         | 5.390%     | 6.004973   |
| 7                         | 5                         | 0.8                       | 0.05                      | 4                         | 0.4                       | 0.004%    | 0.011640  | 0.016%          | 0.042462        | 0.140%           | 0.226378         | 5.413%     | 6.035327   |
| 7                         | 5                         | 0.8                       | 0.03                      | 1                         | 0.4                       | 0.005%    | 0.012627  | 0.030%          | 0.052638        | 0.149%           | 0.226250         | -          | -          |
| 7                         | 5                         | 0.8                       | 0.04                      | 1                         | 0.4                       | 0.004%    | 0.011699  | 0.028%          | 0.051661        | 0.147%           | 0.223151         | -          | -          |
| 7                         | 5                         | 0.8                       | 0.06                      | 1                         | 0.4                       | 0.004%    | 0.011601  | 0.029%          | 0.051716        | 0.147%           | 0.223737         | -          | -          |
| 7                         | 5                         | 0.8                       | 0.07                      | 1                         | 0.4                       | 0.004%    | 0.012164  | 0.024%          | 0.050283        | 0.148%           | 0.223620         | -          | -          |
| 7                         | 5                         | 0.6                       | 0.05                      | 1                         | 0.4                       | 0.004%    | 0.012779  | 0.029%          | 0.051693        | 0.155%           | 0.230169         | -          | -          |
| 7                         | 5                         | 0.7                       | 0.05                      | 1                         | 0.4                       | 0.004%    | 0.012775  | 0.030%          | 0.052166        | 0.151%           | 0.225471         | -          | -          |
| 7                         | 5                         | 0.9                       | 0.05                      | 1                         | 0.4                       | 0.005%    | 0.012049  | 0.018%          | 0.044777        | 0.149%           | 0.220308         | -          | -          |
| 7                         | 3                         | 0.8                       | 0.05                      | 1                         | 0.4                       | 0.004%    | 0.012266  | 0.028%          | 0.053841        | 0.136%           | 0.214851         | -          | -          |
| 7                         | 4                         | 0.8                       | 0.05                      | 1                         | 0.4                       | 0.004%    | 0.012133  | 0.023%          | 0.048540        | 0.143%           | 0.221744         | -          | -          |
| 7                         | 6                         | 0.8                       | 0.05                      | 1                         | 0.4                       | 0.004%    | 0.012089  | 0.019%          | 0.050330        | 0.147%           | 0.224100         | -          | -          |
| 7                         | 7                         | 0.8                       | 0.05                      | 1                         | 0.4                       | 0.004%    | 0.011565  | 0.023%          | 0.049183        | 0.143%           | 0.220270         | -          | -          |
| 5                         | 5                         | 0.8                       | 0.05                      | 1                         | 0.4                       | 0.004%    | 0.011906  | 0.030%          | 0.049128        | 0.147%           | 0.225914         | -          | -          |
| 6                         | 5                         | 0.8                       | 0.05                      | 1                         | 0.4                       | 0.004%    | 0.012007  | 0.028%          | 0.054521        | 0.149%           | 0.226271         | -          | -          |
| 8                         | 5                         | 0.8                       | 0.05                      | 1                         | 0.4                       | 0.005%    | 0.012667  | 0.023%          | 0.048078        | 0.136%           | 0.216938         | -          | -          |
| 9                         | 5                         | 0.8                       | 0.05                      | 1                         | 0.4                       | 0.004%    | 0.011389  | 0.020%          | 0.048504        | 0.149%           | 0.224240         | -          | -          |
| 10                        | 5                         | 0.8                       | 0.05                      | 1                         | 0.4                       | 0.004%    | 0.011643  | 0.019%          | 0.048623        | 0.152%           | 0.226201         | -          | -          |

Then, we varied each parameter, applying an 'one factor at a time' analysis. We used 50 instances from benchmark X and five from benchmark Belgium (the even-numbered ones, considering an increasing size order). For each configuration and problem instance, we ran MDM-HGS five times with different random seeds to measure its impact on the method's performance regarding the average Gap and PI. Those averages were computed separately for each instance size range. In contrast with the size range separation used in the previous parameter tuning, we added a group for very large instances ( n &gt; 2000). The algorithm does not restart within the time limit for those very large instances, so the pattern mining strategy is not used. Hence, for that instance size range, we only evaluate changes in /u1D700 and /u1D702 . The results are reported in Table 3.

We report the average Gap and PI values obtained in instances of each size range for each parameter configuration. The first row presents the baseline configuration. In each of the other rows, only one parameter value (highlighted) is changed with respect to the baseline.

The method's performance is most sensitive to changes in the values of /u1D700 and /u1D702 (whether we consider very large instances or not). In contrast, it is least sensitive to changes in the values of /u1D6FD and /u1D6FF . Remarkably, the value that yields the best average Gap for a parameter also yields the best average PI in most cases. However, there are some exceptions.

For large instances (400 &lt; n ≤ 2000), when varying /u1D700 , the best average Gap is achieved with /u1D700 = 4, whereas the best average PI is achieved with /u1D700 = 2. Hence, while generating a larger initial population may delay convergence, it allows the method to find better solutions in the long term.

For medium instances (200 &lt; n ≤ 400), when varying /u1D6FC , the best average Gap is achieved with /u1D6FC = 10, whereas the best average PI is achieved with /u1D6FC = 8. It indicates that the overhead of mining from a larger elite set, which may delay convergence, is compensated by finding more robust patterns, which lead to better solutions.

In the best parameter configurations derived from the sensitivity analysis results, shown in Table 4, the value for each combination of parameter and instance size range is the one that leads to the lowest Gap and, in the case of a tie, the one that leads to the lowest PI. Those configurations were used in the experiments reported in the next section.

## 4.4 Performance assessment

We ran extensive computational experiments to assess the proposed strategies' impact on performance. All metrics are averaged over ten independent runs of the algorithms with different seeds for each instance. For each combination of instance and algorithm, we register the average solution cost (Avg Cost), the percentage gap of Avg Cost to the BKS (Gap), the best solution cost (Best Cost), and the average primal integral (Avg PI).

Note that parameter /u1D700 was a constant value (4) in the original HGS-CVRP, and we adopt different values for that parameter (depending on instance size) based on the results of our sensitivity analysis. To avoid the risk of the performance impact observed in our experiments being influenced by this parameter calibration, we first apply the same calibration to HGS-CVRP.

Table 5 shows a summarized comparison of the results obtained by HGS-CVRP with the original constant value for /u1D700 (referred to as HGS /u1D700 = 4 ) and with the calibration adopted in this work (referred to as HGS). It shows for each algorithm: the average Gap (Avg Gap); the global average PI; the number of wins in Avg Cost, Best Cost and Avg PI; and the number of best known solutions found. Improving values are highlighted in bold. We present a comparison for all 141 instances and separate ones for each instance subset. Furthermore, we have assessed the statistical significance of the differences in Gap, Avg Cost, Best Cost and Avg PI, using a paired Wilcoxon signed-rank test with a significance level of 0.05 for each comparison. Statistically significant advantages are signalled in the table.

The table confirms that the calibration adopted for /u1D700 has a positive impact on the performance of HGS-CVRP, which is slight but statistically significant. Hence, for fairness, we apply that calibration in all experiments.

## 4.4.1 Comparing MDM-HGS to HGS-CVRP

The main comparison in these experimental analyses is between HGS-CVRP (the baseline method) and the improved version we propose in this work, MDM-HGS. Detailed results from this comparison, with the metrics obtained for each instance, are

TABLE 4 Best parameter configurations from the sensitivity analysis (used in the present experiments).

| Range          | /u1D736   | /u1D737   | /u1D738   | /u1D739   |   /u1D73A |   /u1D73C |
|----------------|-----------|-----------|-----------|-----------|-----------|-----------|
| n ≤ 200        | 7         | 5         | 0.8       | 0.05      |         1 |       0.2 |
| 200 < n ≤ 400  | 10        | 6         | 0.9       | 0.07      |         4 |       0.8 |
| 400 < n ≤ 2000 | 8         | 3         | 0.8       | 0.04      |         4 |       0.2 |
| n > 2000       | -         | -         | -         | -         |         1 |       0.1 |

TABLE 5 Summarized comparison of HGS-CVRP with /u1D700 = 4 to HGS-CVRP with calibrated /u1D700 .

|                | All (141)       | All (141)   | X (100)         | X (100)   | Belgium (10)    | Belgium (10)   | Classic (7)     | Classic (7)   | Golden (12)     | Golden (12)   | DIMACS (12)     | DIMACS (12)   |
|----------------|-----------------|-------------|-----------------|-----------|-----------------|----------------|-----------------|---------------|-----------------|---------------|-----------------|---------------|
|                | HGS /u1D73A = 4 | HGS         | HGS /u1D73A = 4 | HGS       | HGS /u1D73A = 4 | HGS            | HGS /u1D73A = 4 | HGS           | HGS /u1D73A = 4 | HGS           | HGS /u1D73A = 4 | HGS           |
| Avg Gap        | 0.43%           | 0.40% *     | 0.08%           | 0.08%     | 5.13%           | 4.74% *        | 0.00%           | 0.00%         | 0.07%           | 0.07%         | 0.09%           | 0.09%         |
| Global Avg PI  | 0.5143          | 0.4884 *    | 0.1176          | 0.1176    | 5.7637          | 5.3981 *       | 0.0048          | 0.0047        | 0.1190          | 0.1190        | 0.1384          | 0.1384        |
| Wins Avg Cost  | 3               | 11 *        | 1               | 2         | 2               | 8 *            | 0               | 1             | 0               | 0             | 0               | 0             |
| Wins Best Cost | 3               | 9 *         | 1               | 0         | 2               | 8 *            | 0               | 1             | 0               | 0             | 0               | 0             |
| Wins Avg PI    | 8               | 21 *        | 8               | 10        | 0               | 9 *            | 0               | 2             | 0               | 0             | 0               | 0             |
| Nb BKS         | 69              | 69          | 51              | 50        | 0               | 0              | 6               | 7             | 7               | 7             | 5               | 5             |

* Statistically significant advantage.

TABLE 6 Summarized comparison of HGS-CVRP to MDM-HGS.

|                | All (141)   | All (141)   | X (100)   | X (100)   | Belgium (10)   | Belgium (10)   | Classic (7)   | Classic (7)   | Golden (12)   | Golden (12)   | DIMACS (12)   | DIMACS (12)   |
|----------------|-------------|-------------|-----------|-----------|----------------|----------------|---------------|---------------|---------------|---------------|---------------|---------------|
|                | HGS         | MDM         | HGS       | MDM       | HGS            | MDM            | HGS           | MDM           | HGS           | MDM           | HGS           | MDM           |
| Avg Gap        | 0.40%       | 0.33% *     | 0.08%     | 0.07% *   | 4.74%          | 3.75% *        | 0.00%         | 0.00%         | 0.07%         | 0.07%         | 0.09%         | 0.08% *       |
| Global Avg PI  | 0.4884      | 0.3981 *    | 0.1176    | 0.1145    | 5.3981         | 4.1644 *       | 0.0047        | 0.0044        | 0.1190        | 0.1124        | 0.1384        | 0.1387        |
| Wins Avg Cost  | 34          | 61 *        | 24        | 40 *      | 1              | 9 *            | 1             | 1             | 4             | 4             | 4             | 7             |
| Wins Best Cost | 21          | 47 *        | 14        | 32 *      | 1              | 9 *            | 1             | 0             | 2             | 2             | 3             | 4             |
| Wins Avg PI    | 53          | 81 *        | 41        | 54        | 0              | 10 *           | 2             | 3             | 5             | 7             | 5             | 7             |
| Nb BKS         | 68          | 72 †        | 50        | 53        | 0              | 0              | 7             | 6             | 7             | 8             | 4             | 5 †           |

* Statistically significant advantage.

† Includes a new best solution.

presented in Appendix B (Tables B1-B3). Winning values in the comparisons are highlighted in bold, best costs matching the corresponding BKS values are highlighted in italics and new best solution costs are underlined.

Table 6 summarizes the results. This summary table refers to HGS-CVRP as 'HGS" and MDM-HGS as 'MDM". This comparison shows that MDM-HGS obtains solutions of higher quality and better PI values than HGS-CVRP in most instances. The only exception is the DIMACS subset, where HGS-CVRP has a slightly lower PI, but that difference is not statistically significant. On the other hand, MDM-HGS has a statistically significant advantage in the average gap for the same subset. Furthermore, MDM-HGS found a new best solution for instance Loggi-n601-k42 from that subset (cost 347 046). This new solution is presented in Appendix A.

The larger gains are observed on the Belgium subset instances. As we pointed out in Section 4.3, these are very large instances for which the algorithm does not restart within the time limit, and therefore the pattern mining strategy is not used. Hence, the gains in those cases come solely from using the randomized CW heuristic for solution initialization.

Nonetheless, even though the gains for other subsets may seem modest, it is important to note that solution improvements are challenging to obtain in those subsets as the average baseline gaps are very small (below 0.1%). Besides, the BKS costs for several instances (precisely 65) are optimal. That means the margin for improvement is very narrow. Hence, even a slight performance improvement of the order of 0.01% over the state-of-the-art is a significant achievement. This observation is further supported by the fact that the Gap improvements in subsets X (the largest one, with 100 instances) and DIMACS are statistically significant.

## 4.4.2 Comparing MDM-HGS to additional baselines

The results presented in the previous section show that MDM-HGS performs significantly better than HGS-CVRP. However, they are not sufficient to evaluate the MDM approach's impact fully. Therefore, we complement the experimental analyses with comparisons to additional baselines.

One relevant observation from the initial analysis is that the improvements achieved for the Belgium subset are due exclusively to the randomized CW heuristic since pattern mining is not used for those instances. That raises an important question: Could the randomized CW heuristic alone improve the performance of HGS-CVRP on the other instance subsets up to the same level achieved by MDM-HGS?

To answer the above question, we performed experiments with a first additional baseline, HGS-CVRP with an initialization method based on the randomized CW heuristic but without pattern mining (referred to as HGS CW ). The results are presented in Table 7. Although the difference is smaller than the one observed in the comparison to HGS-CVRP (0.003% for the X subset and the overall average Gap), MDM-HGS still achieves the best performance for most instances with statistically significant advantages regarding both solution quality and PI.

Conversely, the opposite question is also reasonable: Could the pattern-based strategy alone improve the performance of HGS-CVRP up to the same level achieved by MDM-HGS with CW?

To answer this question, we performed experiments with a second additional baseline (referred to as MDM R /u1D451 ), a version of MDM-HGS that, after pattern insertion, uses the original HGS-CVRP strategies to complement solution initialization (random permutation of customers and optimal route split) instead of the randomized CW heuristic. The results are presented in Table 8. Again, the version of MDM-HGS that uses the randomized CW heuristic achieves the best performance for most instances, with statistically significant advantages regarding average solution cost in the Golden subset and regarding the best solution cost in the X subset and the overall comparison.

TABLE 7 Summarized comparison of HGS-CVRP with CW to MDM-HGS.

|                | All (131)   | All (131)   | X (100)   | X (100)   | Classic (7)   | Classic (7)   | Golden (12)   | Golden (12)   | DIMACS (12)   | DIMACS (12)   |
|----------------|-------------|-------------|-----------|-----------|---------------|---------------|---------------|---------------|---------------|---------------|
|                | HGS CW      | MDM         | HGS CW    | MDM       | HGS CW        | MDM           | HGS CW        | MDM           | HGS CW        | MDM           |
| Avg Gap        | 0.07%       | 0.07%       | 0.07%     | 0.07%     | 0.00%         | 0.00%         | 0.07%         | 0.07%         | 0.09%         | 0.08% *       |
| Global Avg PI  | 0.1113      | 0.1106 *    | 0.1150    | 0.1145    | 0.0044        | 0.0044        | 0.1122        | 0.1124        | 0.1415        | 0.1387        |
| Wins Avg Cost  | 32          | 49 *        | 26        | 34 *      | 1             | 1             | 3             | 5             | 2             | 9 *           |
| Wins Best Cost | 13          | 34 *        | 9         | 27 *      | 1             | 0             | 2             | 2             | 1             | 6 *           |
| Wins Avg PI    | 43          | 60 *        | 35        | 45        | 2             | 1             | 4             | 5             | 2             | 9             |
| Nb BKS         | 71          | 72 †        | 53        | 53        | 7             | 6             | 7             | 8             | 4             | 5 †           |

* Statistically significant advantage.

† Includes a new best solution.

TABLE 8 Summarized comparison of MDM-HGS with random initialization to MDM-HGS with CW.

|                | All (131)     | All (131)   | X (100)       | X (100)   | Classic (7)   | Classic (7)   | Golden (12)   | Golden (12)   | DIMACS (12)   | DIMACS (12)   |
|----------------|---------------|-------------|---------------|-----------|---------------|---------------|---------------|---------------|---------------|---------------|
|                | MDM R /u1D485 | MDM         | MDM R /u1D485 | MDM       | MDM R /u1D485 | MDM           | MDM R /u1D485 | MDM           | MDM R /u1D485 | MDM           |
| Avg Gap        | 0.07%         | 0.07%       | 0.07%         | 0.07%     | 0.00%         | 0.00%         | 0.08%         | 0.07% *       | 0.08%         | 0.08%         |
| Global Avg PI  | 0.1120        | 0.1106      | 0.1154        | 0.1145    | 0.0051        | 0.0044        | 0.1289        | 0.1124        | 0.1292        | 0.1387        |
| Wins Avg Cost  | 33            | 50          | 27            | 36        | 0             | 1             | 2             | 6 *           | 4             | 7             |
| Wins Best Cost | 20            | 33 *        | 15            | 25 *      | 0             | 0             | 3             | 3             | 2             | 5             |
| Wins Avg PI    | 63            | 61          | 48            | 45        | 3             | 3             | 5             | 7             | 7             | 6             |
| Nb BKS         | 68            | 72 †        | 52            | 53        | 6             | 6             | 6             | 8             | 4             | 5 †           |

* Statistically significant advantage.

† Includes a new best solution.

TABLE 9 Summarized comparison of HGS-CVRP with elite solutions reinsertion to MDM-HGS.

|                | All (131)   | All (131)   | X (100)   | X (100)   | Classic (7)   | Classic (7)   | Golden (12)   | Golden (12)   | DIMACS (12)   | DIMACS (12)   |
|----------------|-------------|-------------|-----------|-----------|---------------|---------------|---------------|---------------|---------------|---------------|
|                | HGS        | MDM         | HGS      | MDM       | HGS          | MDM           | HGS          | MDM           | HGS          | MDM           |
| Avg Gap        | 0.10%       | 0.07% *     | 0.11%     | 0.07% *   | 0.00%         | 0.00%         | 0.13%         | 0.07% *       | 0.12%         | 0.08% *       |
| Global Avg PI  | 0.1360      | 0.1106 *    | 0.1387    | 0.1145 *  | 0.0087        | 0.0044        | 0.1658        | 0.1124 *      | 0.1579        | 0.1387 *      |
| Wins Avg Cost  | 10          | 87 *        | 9         | 66 *      | 0             | 1             | 0             | 10 *          | 1             | 10 *          |
| Wins Best Cost | 18          | 43 *        | 15        | 33 *      | 0             | 0             | 1             | 4             | 2             | 6 *           |
| Wins Avg PI    | 22          | 102 *       | 19        | 76 *      | 2             | 3             | 0             | 12 *          | 1             | 11 *          |
| Nb BKS         | 51          | 72 †        | 46        | 53        | 6             | 6             | 6             | 8             | 3             | 5 †           |

* Statistically significant advantage.

† Includes a new best solution.

The results presented in Tables 7 and 8 show that both the randomized CW heuristic and the pattern-based strategy from the MDM approach can, individually, improve the performance of HGS-CVRP. However, although their contributions partially overlap, none of them alone provides the same level of improvement as their combination.

As Section 3.2 points out, HGS-CVRP differs from the methods on which the MDM approach has been applied so far (single-point metaheuristics). As the population evolves, it intrinsically accumulates some knowledge about favorable decisions encoded in the genes of the fittest individuals across generations. Hence, it is plausible that an additional learning strategy like the one in the MDM approach could be redundant to some extent. That raises another question: Could the observed improvements be similarly achieved by simply using complete high-quality solutions (instead of mined patterns) when reinitializing the population?

To answer the latter question, we performed experiments with two additional baselines: a version of HGS-CVRP that reinserts the solutions from  in the population when it is reinitialized (referred to as HGS  ), and another one that does the same and also uses the randomized CW heuristic (referred to as HGS CW +  ). The results are presented in Tables 9 and 10. They show that MDM-HGS performs significantly better than both baselines. We can observe from their results that not only the elite solution reinsertion strategy cannot improve the performance of HGS-CVRP but makes it worse.

TABLE 10 Summarized comparison of HGS-CVRP with CW and elite solutions reinsertion to MDM-HGS.

|                | All (131)   | All (131)   | X (100)    | X (100)   | Classic (7)   | Classic (7)   | Golden (12)   | Golden (12)   | DIMACS (12)   | DIMACS (12)   |
|----------------|-------------|-------------|------------|-----------|---------------|---------------|---------------|---------------|---------------|---------------|
|                | HGS CW +   | MDM         | HGS CW +  | MDM       | HGS CW +     | MDM           | HGS CW +     | MDM           | HGS CW +     | MDM           |
| Avg Gap        | 0.10%       | 0.07% *     | 0.10%      | 0.07% *   | 0.00%         | 0.00%         | 0.12%         | 0.07% *       | 0.13%         | 0.08% *       |
| Global Avg PI  | 0.1362      | 0.1106 *    | 0.1403     | 0.1145 *  | 0.0079        | 0.0044        | 0.1504        | 0.1124 *      | 0.1628        | 0.1387 *      |
| Wins Avg Cost  | 6           | 88 *        | 6          | 66 *      | 0             | 1             | 0             | 10 *          | 0             | 11 *          |
| Wins Best Cost | 11          | 48 *        | 9          | 36 *      | 0             | 1             | 2             | 3             | 0             | 8 *           |
| Wins Avg PI    | 19          | 98 *        | 14         | 77 *      | 2             | 1             | 2             | 10 *          | 1             | 10 *          |
| Nb BKS         | 64          | 72 †        | 49         | 53        | 5             | 6             | 7             | 8             | 3             | 5 †           |

* Statistically significant advantage.

† Includes a new best solution.

TABLE 11 Rankings from the 12th DIMACS implementation challenge-CVRP track-Phase 1.

| All (141)   | All (141)   | X (100)     | X (100)   | Belgium (10)   | Belgium (10)   | Classic (7)   | Classic (7)   | Golden (12)   | Golden (12)   | DIMACS (12)   | DIMACS (12)   |
|-------------|-------------|-------------|-----------|----------------|----------------|---------------|---------------|---------------|---------------|---------------|---------------|
| Solver      | Score       | Solver      | Score     | Solver         | Score          | Solver        | Score         | Solver        | Score         | Solver        | Score         |
| FHCSolver   | 931         | MDM-HGS     | 663       | FSP4D          | 100            | MDM-HGS       | 66            | FHCSolver     | 86            | FHCSolver     | 83            |
| MDM-HGS     | 896         | FHCSolver   | 642       | FHCSolver      | 80             | POP-HGS       | 51            | POP-HGS       | 83            | FSP4D         | 75            |
| POP-HGS     | 814         | POP-HGS     | 605       | AILS-II        | 56             | FHCSolver     | 40            | FSP4D         | 81            | POP-HGS       | 74            |
| FSP4D       | 737         | HGSRR       | 480       | HGSRR          | 44             | FSP4D         | 34            | MDM-HGS       | 80            | HGSRR         | 73            |
| HGSRR       | 678         | FSP4D       | 447       | LKHSP          | 36             | HGSRR         | 28            | HGSRR         | 53            | MDM-HGS       | 62            |
| AILS-II     | 558         | AILS-II     | 416       | MDM-HGS        | 25             | MAESN         | 19            | MAESN         | 35            | AILS-II       | 52            |
| MAESN       | 406         | MAESN       | 352       | optaplanner    | 19             | ILS-SP        | 12            | AILS-II       | 25            | GA + TBD      | 29            |
| GA + TBD    | 291         | GA + TBD    | 222       | ALNS ++        | 17             | AILS-II       | 9             | GA + TBD      | 25            | LKHSP         | 8             |
| ILS-SP      | 70          | ILS-SP      | 49        | GA + TBD       | 7              | GA + TBD      | 8             | LKHSP         | 0             | ALNS ++       | 8             |
| LKHSP       | 57          | ALNS ++     | 17        | ILS-SP         | 5              | LKHSP         | 6             | ALNS ++       | 0             | ILS-SP        | 4             |
| ALNS ++     | 42          | LKHSP       | 7         | POP-HGS        | 1              | optaplanner   | 0             | ILS-SP        | 0             | optaplanner   | 0             |
| optaplanner | 19          | optaplanner | 0         | MAESN          | 0              | ALNS ++       | 0             | optaplanner   | 0             | Clowder       | 0             |
| Clowder     | 0           | Clowder     | 0         | Clowder        | 0              | Clowder       | 0             | Clowder       | 0             | MAESN         | 0             |
| Resilience  | 0           | Resilience  | 0         | Resilience     | 0              | OR-Tools      | 0             | OR-Tools      | 0             | Resilience    | 0             |
| DDVRP       | 0           | DDVRP       | 0         | OR-Tools       | 0              | Resilience    | 0             | Resilience    | 0             | OR-Tools      | 0             |
| OR-Tools    | 0           | OR-Tools    | 0         | DDVRP          | 0              | DDVRP         | 0             | DDVRP         | 0             | DDVRP         | 0             |

## 4.5 Results from the 12th DIMACS Implementation Challenge

The 12th DIMACS Implementation Challenge had 16 teams competing in the CVRP track. All competing algorithms were ranked according to their PI value for each instance. The best algorithm got 10 points, the second 8, then 6, 5, 4, 3, 2, and 1. The final ranking is based on the total point score obtained by each method in all instances. Table 11 presents the rankings from Phase 1 for all 141 instances and the five subsets separately.

The Phase 1 results show that MDM-HGS was very competitive. It ranked first in two of the five instance subsets: X, the largest one with 100 instances, and Classic, for which MDM-HGS achieved an almost perfect score with 66 points of the 70 possible. Besides, despite ranking fourth in the Golden subset, that was a good performance, considering that the margin among the top-4 algorithms in this subset was very tight (only six points). The results obtained in the remaining two subsets were consistent. Since MDM-HGS performed slightly worse than HGS-CVRP in the DIMACS subset (regarding PI), it was unsurprising that it also performed slightly worse than other HGS-based methods, which is the case of three out of four algorithms that ranked better: FHCSolver, POP-HGS and HGSRR. It was also expected that methods specifically designed for large-scale instances would perform better than MDM-HGS in the Belgium subset, which is the case of three out of five methods that ranked better: FSP4D and FHCSolver, both based on FILO [2], and AILS-II. Nevertheless, MDM-HGS achieved good results in these subsets, ranking fifth and sixth, respectively.

The top-8 competitors from Phase 1 qualified for the second and final phase, in which 100 new (undisclosed) instances were used. Table 12 presents the rankings from Phase 2 for all 100 instances and partitions concerning customer positioning, instance size and average route size.

Phase 2 results confirmed that MDM-HGS is very competitive. The instances partitioning reveals that it outperforms all competitors for three classes and ties in first for another.

TABLE 12 Rankings from the 12th DIMACS implementation challenge-CVRP track-Phase 2.

| All (100)   | Clustered (32)   | Clustered (32)   | Clustered (32)   | Random (34)   | Random (34)   | Random- clustered (34)   | Random- clustered (34)   | Small (50)   | Small (50)   | Large (50)   | Large (50)   | Long- route (50)   | Long- route (50)   | Short- route (50)   | Short- route (50)   |
|-------------|------------------|------------------|------------------|---------------|---------------|--------------------------|--------------------------|--------------|--------------|--------------|--------------|--------------------|--------------------|---------------------|---------------------|
| Solver      | Score Solver     | Score Solver     | Score Solver     | Score Solver  | Score Solver  | Score Solver             | Score Solver             | Score Solver | Score Solver | Score Solver | Score Solver | Score Solver       | Score Solver       | Score Solver        | Score               |
| FHCSolver   | 643              | MDM-HGS          | 213              | FHCSolver     | 217           | FHCSolver                | 236                      | MDM-HGS      | 357          | AILS-II      | 342          | MDM-HGS            | 334                | FHCSolver           | 329                 |
| MDM-HGS     | 622              | FHCSolver        | 190              | MDM-HGS       | 217           | MDM-HGS                  | 192                      | FHCSolver    | 355          | FHCSolver    | 288          | FHCSolver          | 314                | AILS-II             | 308                 |
| AILS-II     | 536              | POP-HGS          | 184              | AILS-II       | 184           | AILS-II                  | 179                      | POP-HGS      | 271          | FSP4D        | 269          | FSP4D              | 274                | MDM-HGS             | 288                 |
| POP-HGS     | 525              | AILS-II          | 173              | HGSRR         | 177           | FSP4D                    | 175                      | HGSRR        | 236          | MDM-HGS      | 265          | POP-HGS            | 272                | POP-HGS             | 253                 |
| HGSRR       | 477              | FSP4D            | 136              | POP-HGS       | 175           | HGSRR                    | 166                      | AILS-II      | 194          | POP-HGS      | 254          | HGSRR              | 236                | HGSRR               | 241                 |
| FSP4D       | 458              | HGSRR            | 134              | FSP4D         | 147           | POP-HGS                  | 166                      | FSP4D        | 189          | HGSRR        | 241          | AILS-II            | 228                | MAESN               | 203                 |
| MAESN       | 389              | MAESN            | 132              | MAESN         | 132           | MAESN                    | 125                      | MAESN        | 187          | MAESN        | 202          | MAESN              | 186                | FSP4D               | 184                 |
| GA + TBD    | 250              | GA + TBD         | 86               | GA + TBD      | 77            | GA + TBD                 | 87                       | GA + TBD     | 161          | GA + TBD     | 89           | GA + TBD           | 106                | GA + TBD            | 144                 |

Regarding customer positioning, the instances are partitioned into three classes: 'clustered', 'random', and 'random-clustered' (half of the customers are clustered, and the remaining customers are randomly positioned). MDM-HGS ranked first in the clustered instances. Naturally, these instances are more prone to the occurrence of patterns than random ones, favouring the pattern-based strategy applied in the MDM approach. Nevertheless, MDM-HGS also achieved high scores for the other partitions, tying for first place with FHCSolver in the random instances and ranking second in the random-clustered ones.

Regarding instance size, the instances are partitioned into 'small" (the 50 smallest ones) and 'large' (the 50 largest ones). As in Phase 1, the algorithms based on methods designed to tackle large-scale instances (AILS-II, FHCSolver and FSP4D) outperformed MDM-HGS in the 50 largest ones. On the other hand, MDM-HGS ranked first in the 50 smallest instances.

The average route size is defined as the number of customers divided by the minimum number of vehicles. Regarding this aspect, the instances are partitioned into 'long-route' (the 50 with longest routes) and 'short-route" (the 50 with shortest routes). MDM-HGSranked first in the long-route instances. The longer the routes in solutions to an instance, the more likely long paths will occur in patterns mined from those solutions. Hence, long-route instances seem to favour the pattern-based strategy applied in the MDM approach more than short-route ones.

## 5 CONCLUSION

This paper presented MDM-HGS, an improved version of the HGS metaheuristic for the CVRP (HGS-CVRP) [59]. MDM-HGS was produced by introducing a new solution generation method into the population (re-)initialization process, invoked when the algorithm starts and whenever a certain number of iterations without improvement is reached. This new method is based on (i) patterns extracted from good solutions using data mining and (ii) a randomized version of the Clarke and Wright savings heuristic.

We conducted extensive experiments to compare the performance of the proposed MDM-HGS with that of HGS-CVRP. MDM-HGS outperformed HGS-CVRP with respect to solution quality and the primal integral, a performance measure that rewards a balance of convergence speed and solution quality. Furthermore, we carried out computational experiments with additional baselines to separately assess the impact of each strategy applied in the proposed method. Our experimental results show that the proposed pattern-based initialization method significantly improves the search performance, achieving higher solution quality and faster convergence.

To the best of our knowledge, this study was the first to apply the MDM approach in a population-based algorithm, which depicts an evolutionary framework. Hence, the reported results add evidence to the literature that this pattern-based strategy can improve performance in such a context.

The contribution of MDM-HGS was further evidenced by the results obtained in the 12th DIMACS Implementation Challenge. It earned a close second place in the overall ranking of the CVRP track, outperforming 14 other methods. Some other noticeable results can be observed. Six of the eight finalist methods are based on HGS-CVRP. MDM-HGS outperformed all these methods in the overall ranking except for FHCSolver, the Challenge winner. Furthermore, besides being close to FHCSolver in the overall ranking, MDM-HGS ranked first for some classes of instances.

FHCSolver combines the original HGS-CVRP with an adapted version of FILO. Its strategy relies primarily on selecting which algorithm to use based on the instance's number of customers and average route size. Since our experimental results provide evidence that MDM-HGS outperforms HGS-CVRP, ultimately, our proposed approach could improve FHCSolver's performance by simply replacing HGS-CVRP with MDM-HGS within its framework.

In other directions for future work, leveraging the knowledge encoded in the mined patterns to perform other tasks than solution initialization could increase the method's performance. In particular, we see good candidate strategies in pattern-based local search move operators [6] and problem decomposition [33]. Another interesting idea is to incorporate a classifier-based approach like the one proposed in [36] to learn which individuals should be rejected before applying local search, as it could provide considerable efficiency improvements.

## ACKNOWLEDGMENTS

This work was enabled in part by computational resources and support provided by Calcul Québec (https://calculquebec.ca), the Digital Research Alliance of Canada (https://alliancecan.ca) and UFF's Laboratório de Inteligência Computacional (LabIC). This support is gratefully acknowledged. We would also like to thank Thibaut Vidal, who granted us access to computational resources of Calcul Québec/DRAC and provided helpful tips on how to use them, besides contributing valuable insights about his HGS-CVRP metaheuristic.

## DATA AVAILABILITY STATEMENT

Data sharing is not applicable to this article as no new data were created or analyzed in this study.

## ORCID

https://orcid.org/0000-0002-7207-1698

Marcelo Rodrigues de Holanda Maia Alexandre Plastino https://orcid.org/0000-0003-4039-0915 Uéverton dos Santos Souza https://orcid.org/0000-0002-5320-9209

## REFERENCES

- [1] L. Accorsi, F. Cavaliere, and D. Vigo. Iterative fast optimization for the capacitated vehicle routing problem, Paper presented at: 12th DIMACS implementation challenge: Vehicle routing problems. 2022.
- [2] L. Accorsi and D. Vigo, Afast and scalable heuristic for the solution of large-scale capacitated vehicle routing problems , Transp. Sci. 55 (2021), 832-856.
- [3] F. Arnold, M. Gendreau, and K. Sörensen, Efficiently solving very large-scale routing problems , Comput. Oper. Res. 107 (2019), 32-42.
- [4] F. Arnold and K. Sörensen, Knowledge-guided local search for the vehicle routing problem , Comput. Oper. Res. 105 (2019), 32-46.
- [5] F. Arnold and K. Sörensen, What makes a VRP solution good? The generation of problem-specific knowledge for heuristics , Comput. Oper. Res. 106 (2019), 280-288.
- [6] F. Arnold, Í. Santana, K. Sörensen, and T. Vidal, PILS: Exploring high-order neighborhoods by pattern mining and injection , Pattern Recogn. 116 (2021), 107957.
- [7] P. Augerat, D. Naddef, J. M. Belenguer, E. Benavent, A. Corberan, and G. Rinaldi, Computational results with a branch and cut code for the capacitated vehicle routing problem . Tech. report, Institut National Polytechnique, Grenoble, France, 1995.
- [8] T. Berthold, Measuring the impact of primal heuristics , Oper. Res. Lett. 41 (2013), 611-614.
- [9] L. Calvet, J. de Armas, D. Masip, and A. A. Juan, Learnheuristics: Hybridizing metaheuristics with machine learning for optimization with dynamic inputs , Open Math. 15 (2017), 261-280.
- [10] B. Chen, R. Qu, R. Bai, and W. Laesanklang, A variable neighborhood search algorithm with reinforcement learning for a real-life periodic vehicle routing problem with time windows and open routes , RAIRO-Oper. Res. 54 (2020), 1467-1494.
- [11] X. Chen and Y. Tian, ' Learning to perform local rewriting for combinatorial optimization ,' Advances in neural information processing systems , Vol 32 , Curran Associates, Inc, Red Hook, NY, 2019.
- [12] Y. Chen, P. Cowling, F. Polack, and P. Mourdjis, A multi-arm bandit neighbourhood search for routing and scheduling problems . Research report, University of York, Heslington, York, 2016.
- [13] J. Christiaens and G. Vanden Berghe, Slack induction by string removals for vehicle routing problems , Transp. Sci. 54 (2020), 417-433.
- [14] N. Christofides and S. Eilon, An algorithm for the vehicle-dispatching problem , J. Oper. Res. Soc. 20 (1969), 309-318.
- [15] N. Christofides, A. Mingozzi, and P. Toth, The vehicle routing problem, combinatorial optimization , N. Christofides, A. Mingozzi, P. Toth, and C. Sandi (eds.), Wiley, Chichester, 1979, pp. 315-338.
- [16] G. Clarke and J. W. Wright, Scheduling of vehicles from a central depot to a number of delivery points , Oper. Res. 12 (1964), 568-581.
- [17] D. Corne, C. Dhaenens, and L. Jourdan, Synergies between operations research and data mining: The emerging use of multi-objective approaches , Eur. J. Oper. Res. 221 (2012), 469-479.
- [18] J. C. Créput and A. Koukam, The memetic self-organizing map approach to the vehicle routing problem , Soft. Comput. 12 (2008), 1125-1141.
- [19] F. L. Dalboni, L. S. Ochi, and L. M. A. Drummond. On improving evolutionary algorithms by using data mining for the oil collector vehicle routing problem, Proceedings of the International Network Optimization Conference. 2003 182-188.
- [20] I. Drori, A. Kharkar, W. R. Sickinger, B. Kates, Q. Ma, S. Ge, E. Dolev, B. Dietrich, D. P. Williamson, and M. Udell. Learning to solve combinatorial optimization problems on real-world graphs in linear time, Paper presented at: 19th IEEE international conference on machine learning and applications (ICMLA). 2020 pp. 19-24.
- [21] S. Erdo˘ gan and E. Miller-Hooks, A green vehicle routing problem , Transp. Res. Part E: Logist Transp. Rev. 48 (2012), 100-114.
- [22] M. L. Fisher, Optimal solution of vehicle routing problems using minimum k-trees , Oper. Res. 42 (1994), 626-642.
- [23] S. Gao, Y. Wang, J. Cheng, Y. Inazumi, and Z. Tang, Ant colony optimization with clustering for solving the dynamic location routing problem , Appl. Math. Comput. 285 (2016), 149-173.
- [24] B. L. Golden, E. A. Wasil, J. P. Kelly, and I. M. Chao, The impact of metaheuristics on solving the vehicle routing problem: Algorithms, problem sets, and computational results , F. Management, T. G. C. Logistics, and G. Laporte (eds.), Springer, US, Boston, MA, 1998, pp. 33-56.

- [25] G. Grahne and J. Zhu. Efficiently using prefix-trees in mining frequent itemsets, Proceedings of the IEEE ICDM workshop on frequent itemset mining implementations. 2003.
- [26] A. E. Gutierrez-Rodríguez, S. E. Conant-Pablos, J. C. Ortiz-Bayliss, and H. Terashima-Marín, Selecting meta-heuristics for solving vehicle routing problems with time windows via meta-learning , Expert Syst. Appl. 118 (2019), 470-481.
- [27] S. Jiang, Z. He, W. Lin, F. Ma, and Z. Lü. FHCSolver: Fast hybrid CVRP solver, Paper presented at: 12th DIMACS implementation challenge: Vehicle routing problems. 2022.
- [28] L. Jourdan, C. Dhaenens, and E. G. Talbi, Using data mining techniques to help metaheuristics: A short survey , Hybrid Metaheuristics, Springer Berlin Heidelberg, Berlin, Heidelberg, 2006, 57-69.
- [29] W. Kool, H. van Hoof, and M. Welling. Attention, learn to solve routing problems!, International conference on learning representations. 2019.
- [30] H. Lu, X. Zhang, and S. Yang. A learning-based iterative method for solving vehicle routing problems, International conference on learning representations. 2020.
- [31] F. Lucas, R. Billot, M. Sevaux, and K. Sörensen, Reducing space search in combinatorial optimization using machine learning tools , Learning and Intelligent Optimization, Springer International Publishing, Cham, 2020, 143-150.
- [32] M. R. H. Maia, A. Plastino, and P. H. V. Penna, Hybrid data mining heuristics for the heterogeneous fleet vehicle routing problem , RAIRO-Oper. Res. 52 (2018), 661-690.
- [33] M. R. H. Maia, A. Plastino, and P. H. V. Penna, MineReduce: An approach based on data mining for problem size reduction , Comput. Oper. Res. 122 (2020), 104995.
- [34] S. L. Martins, I. Rosseti, and A. Plastino, Data mining in stochastic local search , Springer International Publishing, Cham, 2018, 39-87.
- [35] N. Mazyavkina, S. Sviridov, S. Ivanov, and E. Burnaev, Reinforcement learning for combinatorial optimization: A survey , Comput. Oper. Res. 134 (2021), 105400.
- [36] J. P. Mesa, A. Montoya, R. Ramos-Pollan, and M. Toro. Machine-learning component for multi-start metaheuristics to solve the capacitated vehicle routing problem. preprint 2022.

[37]

V. R. Máximo, J. F. Cordeau, and M. C. V. Nascimento. AILS-II: An adaptive iterated local search heuristic for the large-scale capacitated vehicle routing problem. 2022.

- [38] V. R. Máximo and M. C. V. Nascimento, A hybrid adaptive iterated local search with diversification control to the capacitated vehicle routing problem , Eur. J. Oper. Res. 294 (2021), 1108-1119.
- [39] M. Nazari, A. Oroojlooy, L. Snyder, and M. Takáˇ c, ' Reinforcement learning for solving the vehicle routing problem ,' Advances in neural information processing systems , Vol 31 , Curran Associates, Inc, Red Hook, NY, 2018.
- [40] A. Ostertag, K. F. Doerner, and R. F. Hartl, A variable neighborhood search integrated in the POPMUSIC framework for solving large scale vehicle routing problems , Hybrid Metaheuristics, Springer Berlin Heidelberg, Berlin, Heidelberg, 2008, 29-42.
- [41] A. Plastino, H. Barbalho, L. F. M. Santos, R. Fuchshuber, and S. L. Martins, Adaptive and multi-mining versions of the DM-GRASP hybrid metaheuristic , J. Heuristics 20 (2014), 39-74.
- [42] E. Queiroga and R. Sadykov. POP-HGS: Hybrid genetic search as a subsolver in a POPMUSIC algorithm, Paper presented at: 12th DIMACS implementation challenge: Vehicle routing problems. 2022.

[43]

E. Queiroga, R. Sadykov, and E. Uchoa,

A POPMUSIC matheuristic for the capacitated vehicle routing problem

,

Comput. Oper. Res.

136

(2021), 105475.

- [44] M. Reimann, K. Doerner, and R. F. Hartl, D-ants: Savings based ants divide and conquer the vehicle routing problem , Comput. Oper. Res. 31 (2004), 563-591.
- [45] M. H. Ribeiro, A. Plastino, and S. L. Martins, Hybridization of GRASP metaheuristic with data mining techniques , J. Math. Model. Algorithms 5 (2006), 23-41.
- [46] Y. Rochat and E. D. Taillard, Probabilistic diversification and intensification in local search for vehicle routing , J. Heuristics 1 (1995), 147-167.
- [47] H. Santos, L. Ochi, E. Marinho, and L. Drummond, Combining an evolutionary algorithm with data mining to solve a single-vehicle routing problem , Neurocomputing 70 (2006), 70-77.
- [48] L. F. Santos, S. L. Martins, and A. Plastino, Applications of the DM-GRASP heuristic: A survey , Int. Trans. Oper. Res. 15 (2008), 387-416.
- [49] M. Sarhani, S. Voß, and R. Jovanovic, Initialization of metaheuristics: Comprehensive review, critical analysis, and research directions , Int. Trans. Oper. Res. 30 (2023), 3361-3397.
- [50] M. Simensen, G. Hasle, and M. Stålhane, Combining hybrid genetic search with ruin-and-recreate for solving the capacitated vehicle routing problem , J. Heuristics 28 (2022), 653-697.
- [51] K. Sörensen, F. Arnold, and D. P. Cuervo, A critical analysis of the 'improved Clarke and Wright savings algorithm' , Int. Trans. Oper. Res. 26 (2019), 54-63.
- [52] A. Subramanian, E. Uchoa, and L. S. Ochi, Ahybrid algorithm for a class of vehicle routing problems , Comput. Oper. Res. 40 (2013), 2519-2531.
- [53] E. G. Talbi, Machine learning into metaheuristics: A survey and taxonomy , ACM Comput. Surv. 54 (2021), 1-32.
- [54] P. Toth and D. Vigo, Vehicle routing: Problems, methods, and applications , 2nd ed., Society for Industrial and Applied Mathematics, USA, 2014.
- [55] D. Tuzun, M. A. Magent, and L. I. Burke, Selection of vehicle routing heuristic using neural networks , Int. Trans. Oper. Res. 4 (1997), 211-221.
- [56] R. Tyasnurita, E. Özcan, and R. John. Learning heuristic selection using a time delay neural network for open vehicle routing, IEEE Congress on Evolutionary Computation (CEC). 2017 1474-1481.
- [57] E. Uchoa, D. Pecin, A. Pessoa, M. Poggi, T. Vidal, and A. Subramanian, New benchmark instances for the capacitated vehicle routing problem , Eur. J. Oper. Res. 257 (2017), 845-858.
- [58] T. Vidal, Node, edge, arc routing and turn penalties: Multiple problems-one neighborhood extension , Oper. Res. 65 (2017), 992-1010.
- [59] T. Vidal, Hybrid genetic search for the CVRP: Open-source implementation and SWAP* neighborhood , Comput. Oper. Res. 140 (2022), 105643.
- [60] T. Vidal, T. G. Crainic, M. Gendreau, N. Lahrichi, and W. Rei, A hybrid genetic algorithm for multidepot and periodic vehicle routing problems , Oper. Res. 60 (2012), 611-624.
- [61] T. Vidal, T. G. Crainic, M. Gendreau, and C. Prins, A unified solution framework for multi-attribute vehicle routing problems , Eur. J. Oper. Res. 234 (2014), 658-673.
- [62] T. Vidal, N. Maculan, L. S. Ochi, and P. H. V. Penna, Large neighborhoods with implicit customer selection for vehicle routing problems with profits , Transp. Sci. 50 (2016), 720-734.
- [63] T. Vidal, R. Martinelli, T. A. Pham, and M. H. Hà, Arc routing with time-dependent travel times and paths , Transp. Sci. 55 (2021), 706-724.
- [64] S. Voigt, M. Frank, P. Fontaine, and H. Kuhn, Hybrid adaptive large neighborhood search for vehicle routing problems with depot location decisions , Comput. Oper. Res. 146 (2022), 105856.
- [65] S. Voigt, M. Frank, P. Fontaine, and H. Kuhn. Hybrid large neighborhood search for the capacitated vehicle routing problem and the vehicle routing problem with time windows, Paper presented at: 12th DIMACS implementation challenge: Vehicle routing problems. 2022.

- [66] J. J. Q. Yu, W. Yu, and J. Gu, Online vehicle routing with neural combinatorial optimization and deep reinforcement learning , IEEE Trans. Intell. Transp. Syst. 20 (2019), 3806-3817.
- [67] J. Zhang, Evolutionary computation meets machine learning: A survey , IEEE Comput. Intell. Mag. 6 (2011), 68-75.
- [68] J. Zheng, X. Zheng, Y. Wei, and K. He. MAESN: Solver description, Paper presented at: 12th DIMACS implementation challenge: Vehicle routing problems. 2022.

How to cite this article: M. R. H. Maia, A. Plastino, and U. S. Souza, An improved hybrid genetic search with data mining for the CVRP , Networks. 83 (2024), 692-711. https://doi.org/10.1002/net.22213

## APPENDIX A: NEW BEST SOLUTION

## Instance Loggi-n601-k42 (found by MDM-HGS):

```
APPENDIX A: NEW BEST SOLUTION

Instance Loggi-n601-k42 (found by MDM-HGS):
    Route #1: 507 10 284 51 234 306 89 99 496 385 59 373 590 530 235
    Route #2: 163 586 8 316 256 576 468 335 303 176 534 478 437 326
    Route #3: 291 169 286 83 351 345 109 204 31 72 459 164 341 471
    Route #4: 511 541 317 158 492 562 332 111 472 174 16 571 367 115 252 161 151
    Route #5: 462 66 452 39 442 346 259 412 94 287 381 105 26 426 595 116
    Route #6: 260 159 535 108 230 319 202 543 464 593 166 339 139 191
    Route #7: 405 133 157 232 435 205 447 283 432 393 563 127 574
    Route #8: 32 584 429 523 480 285 506 38 168 427 136 380 579 124
    Route #9: 209 564 313 48 461 436 243 540 421 175 456 334 529 263 597 321
    Route #10: 396 187 320 194 12 310 218 212 143 587
    Route #11: 238 512 537 156 141 86 257 365 371 402 581 196 419 11
    Route #12: 62 184 279 264 344 315 582 167 142 361 350 197 69 410 329
    Route #13: 349 353 154 343 386 71 68 145 40 49 389 290 236 269 403 228 20 475
    Route #14: 88 181 58 17 400 369 23 328 73 4 318 542
    Route #15: 29 101 148 79 409 457 211 460 323 508 399 458 415 298
    Route #16: 486 25 536 225 294 354 128 482 229 36 501 342 355 65 77 165 588
    Route #17: 85 275 22 295 27 569 198 550 84 241 565 185 596 18 414 441 376
    Route #18: 293 336 153 270 362 411 440 538 599 548 223 254 504 147 388
    Route #19: 150 424 477 489 155 182 178 575 407 498 490 272 561 268
    Route #20: 21 195 87 33 491 296 120 179 76 309 502 337 280 469 470
    Route #21: 476 416 122 137 247 505 514 413 13 510 539 262
    Route #22: 338 573 100 473 314 112 322 390 265 308 431 239 7 397
    Route #23: 125 384 210 224 123 98 533 244 357 493 558 121 107 589 162
    Route #24: 307 188 360 448 570 430 56 255 129 592 446 50 82 333
    Route #25: 370 443 233 222 207 423 180 102 438 516 214 219 428 499 331
    Route #26: 226 140 172 577 422 330 408 170 544 92 567 302 387 547 6 453
    Route #27: 372 267 37 206 281 299 276 208 526 528 97 183
    Route #28: 488 138 594 532 445 406 251 444
    Route #29: 242 509 347 126 378 483 61 248 551 132 545
    Route #30: 192 583 57 391 60 24 553 220 487 398 81 474 598 114
    Route #31: 578 131 240 135 497 216 91 231 356 503
    Route #32: 359 253 35 554 237 193 44 517 417 305 199 70
    Route #33: 494 520 383 78 171 42 282 219 311 450 451 481 568 2 325 292
    Route #34: 401 266 304 454 1 271 177 434 200 382 152 524 546 379 30 572
    Route #35: 130 250 249 93 149 274 585 404 215 146 258 515 525 327 273
    Route #36: 75 559 340 90 47 118 395 189 104 425 28 521
    Route #37: 479 14 364 527 418 500 52 227 9 484 375 324 34 566
    Route #38: 54 485 278 203 217 186 466 46 246 465 580 134
    Route #39: 289 173 366 352 394 5 160 591 555 420 64 495 552 117 531 190 15 549
```

10970037, 2024, 4, Downloaded from https://onlinelibrary.wiley.com/doi/10.1002/net.22213 by Technische UniversitGLYPH&lt;228&gt;t München, Wiley Online Library on [30/03/2025]. See the Terms and Conditions (https://onlinelibrary.wiley.com/terms-and-conditions) on Wiley Online Library for rules of use; OA articles are governed by the applicable Creative Commons License

Route #40: 467 439 67 55 358 113 433 41 513 53 80 74 3 Route #41: 377 19 45 312 245 43 518 297 221 368 449 556 201 119 103 110 106 144 277 213 261

Route #42: 374 557 463 600 300 363 348 392 288 95 455 96 560 301 522 63 Cost 347046

## APPENDIX B: DETAILED RESULTS

TABLE B1 Comparison of HGS-CVRP and MDM-HGS over ten runs (1 of 3).

|            |           | HGS-CVRP   | HGS-CVRP   |        |         | MDM-HGS   | MDM-HGS    |       |        |
|------------|-----------|------------|------------|--------|---------|-----------|------------|-------|--------|
| Instance   | BKS       | Best cost  | Avg cost   | Gap    | Avg PI  | Best cost | Avg cost   | Gap   | Avg PI |
| E-n101-k8  | 815       | 815        | 815.00     | 0.00%  | 0.0002  | 815       | 815.00     | 0.00% | 0.0003 |
| E-n101-k14 | 1067      | 1067       | 1067.00    | 0.00%  | 0.0007  | 1067      | 1067.00    | 0.00% | 0.0006 |
| CMT4       | 1028.42   | 1028.42    | 1028.42    | 0.00%  | 0.0010  | 1028.42   | 1028.42    | 0.00% | 0.0013 |
| CMT5       | 1291.29   | 1291.29    | 1291.42    | 0.01%  | 0.0166  | 1291.45   | 1291.45    | 0.01% | 0.0158 |
| F-n135-k7  | 1162      | 1162       | 1162.00    | 0.00%  | 0.0002  | 1162      | 1162.00    | 0.00% | 0.0002 |
| P-n101-k4  | 681       | 681        | 681.00     | 0.00%  | 0.0001  | 681       | 681.00     | 0.00% | 0.0001 |
| Tai385     | 24366.41  | 24366.41   | 24367.29   | 0.00%  | 0.0142  | 24366.41  | 24367.03   | 0.00% | 0.0128 |
| Golden_9   | 579.70    | 579.70     | 579.96     | 0.05%  | 0.1096  | 579.70    | 579.89     | 0.03% | 0.1037 |
| Golden_10  | 735.43    | 736.14     | 736.81     | 0.19%  | 0.3010  | 736.14    | 736.91     | 0.20% | 0.2875 |
| Golden_11  | 911.98    | 912.58     | 913.19     | 0.13%  | 0.2313  | 912.97    | 913.54     | 0.17% | 0.2415 |
| Golden_12  | 1100.67   | 1102.37    | 1103.42    | 0.25%  | 0.3579  | 1102.17   | 1102.93    | 0.21% | 0.3037 |
| Golden_13  | 857.19    | 857.19     | 857.19     | 0.00%  | 0.0187  | 857.19    | 857.19     | 0.00% | 0.0307 |
| Golden_14  | 1080.55   | 1080.55    | 1080.55    | 0.00%  | 0.0086  | 1080.55   | 1080.55    | 0.00% | 0.0114 |
| Golden_15  | 1337.27   | 1337.95    | 1338.69    | 0.11%  | 0.1569  | 1338.12   | 1338.74    | 0.11% | 0.1654 |
| Golden_16  | 1611.28   | 1611.84    | 1612.82    | 0.10%  | 0.1658  | 1611.28   | 1612.25    | 0.06% | 0.1367 |
| Golden_17  | 707.76    | 707.76     | 707.76     | 0.00%  | 0.0009  | 707.76    | 707.76     | 0.00% | 0.0008 |
| Golden_18  | 995.13    | 995.13     | 995.13     | 0.00%  | 0.0222  | 995.13    | 995.13     | 0.00% | 0.0165 |
| Golden_19  | 1365.60   | 1365.60    | 1365.60    | 0.00%  | 0.0149  | 1365.60   | 1365.64    | 0.00% | 0.0157 |
| Golden_20  | 1817.59   | 1817.59    | 1817.94    | 0.02%  | 0.0398  | 1817.59   | 1817.80    | 0.01% | 0.0357 |
| Antwerp1   | 477 277   | 486 354    | 487584.50  | 2.16%  | 2.7141  | 485 974   | 487322.60  | 2.10% | 2.3726 |
| Antwerp2   | 291 350   | 304 889    | 305618.20  | 4.90%  | 5.8814  | 304 530   | 305170.00  | 4.74% | 5.3370 |
| Brussels1  | 501 719   | 525 832    | 527105.70  | 5.06%  | 6.1061  | 520 762   | 521529.80  | 3.95% | 4.0832 |
| Brussels2  | 345 468   | 367 572    | 368243.70  | 6.59%  | 7.6594  | 365 544   | 366704.80  | 6.15% | 6.5744 |
| Flanders1  | 7 240 118 | 7 528 013  | 7546338.80 | 4.23%  | 4.9961  | 7 407 543 | 7413862.70 | 2.40% | 2.5232 |
| Flanders2  | 4 373 245 | 4 823 840  | 4839712.90 | 10.67% | 10.0000 | 4 598 971 | 4611867.00 | 5.46% | 6.0669 |
| Ghent1     | 469 531   | 485 140    | 486015.90  | 3.51%  | 4.1479  | 482 315   | 482516.40  | 2.77% | 2.8971 |
| Ghent2     | 257 748   | 271 972    | 272587.40  | 5.76%  | 6.6180  | 271 522   | 272121.40  | 5.58% | 6.1752 |
| Leuven1    | 192 848   | 194 794    | 195247.00  | 1.24%  | 1.8181  | 194 996   | 195308.20  | 1.28% | 1.7168 |
| Leuven2    | 111 391   | 114 761    | 115072.50  | 3.31%  | 4.0396  | 114 578   | 114824.50  | 3.08% | 3.8972 |
| X-n101-k25 | 27 591    | 27 591     | 27591.00   | 0.00%  | 0.0003  | 27 591    | 27591.00   | 0.00% | 0.0003 |
| X-n106-k14 | 26 362    | 26 362     | 26366.60   | 0.02%  | 0.0478  | 26 362    | 26367.10   | 0.02% | 0.0454 |
| X-n110-k13 | 14 971    | 14 971     | 14971.00   | 0.00%  | 0.0001  | 14 971    | 14971.00   | 0.00% | 0.0002 |
| X-n115-k10 | 12 747    | 12 747     | 12747.00   | 0.00%  | 0.0001  | 12 747    | 12747.00   | 0.00% | 0.0002 |
| X-n120-k6  | 13 332    | 13 332     | 13332.00   | 0.00%  | 0.0006  | 13 332    | 13332.00   | 0.00% | 0.0005 |
| X-n125-k30 | 55 539    | 55 539     | 55539.00   | 0.00%  | 0.0012  | 55 539    | 55539.00   | 0.00% | 0.0013 |
| X-n129-k18 | 28 940    | 28 940     | 28940.00   | 0.00%  | 0.0012  | 28 940    | 28940.00   | 0.00% | 0.0011 |
| X-n134-k13 | 10 916    | 10 916     | 10916.00   | 0.00%  | 0.0038  | 10 916    | 10916.00   | 0.00% | 0.0038 |
| X-n139-k10 | 13 590    | 13 590     | 13590.00   | 0.00%  | 0.0004  | 13 590    | 13590.00   | 0.00% | 0.0003 |
| X-n143-k7  | 15 700    | 15 700     | 15700.00   | 0.00%  | 0.0016  | 15 700    | 15700.00   | 0.00% | 0.0015 |
| X-n148-k46 | 43 448    | 43 448     | 43448.00   | 0.00%  | 0.0008  | 43 448    | 43448.00   | 0.00% | 0.0008 |
| X-n157-k13 | 16 876    | 16 876     | 16876.00   | 0.00%  | 0.0003  | 16 876    | 16876.00   | 0.00% | 0.0003 |
| X-n162-k11 | 14 138    | 14 138     | 14138.00   | 0.00%  | 0.0008  | 14 138    | 14138.00   | 0.00% | 0.0005 |
| X-n167-k10 | 20 557    | 20 557     | 20557.00   | 0.00%  | 0.0067  | 20 557    | 20557.00   | 0.00% | 0.0023 |
| X-n172-k51 | 45 607    | 45 607     | 45607.00   | 0.00%  | 0.0013  | 45 607    | 45607.00   |       | 0.0015 |
|            |           |            |            |        |         |           |            | 0.00% |        |

10970037, 2024, 4, Downloaded from https://onlinelibrary.wiley.com/doi/10.1002/net.22213 by Technische UniversitGLYPH&lt;228&gt;t München, Wiley Online Library on [30/03/2025]. See the Terms and Conditions (https://onlinelibrary.wiley.com/terms-and-conditions) on Wiley Online Library for rules of use; OA articles are governed by the applicable Creative Commons License

TABLE B2 Comparison of HGS-CVRP and MDM-HGS over ten runs (2 of 3).

|                        | HGS-CVRP       | HGS-CVRP       | HGS-CVRP           | MDM-HGS     | MDM-HGS       | MDM-HGS        | MDM-HGS           | MDM-HGS     | MDM-HGS       |
|------------------------|----------------|----------------|--------------------|-------------|---------------|----------------|-------------------|-------------|---------------|
| Instance               | BKS            | Best cost      | Avg cost           | Gap         | Avg PI        | Best cost      | Avg cost          | Gap         | Avg PI        |
| X-n176-k26             | 47 812         | 47 812         | 47812.00           | 0.00%       | 0.0041        | 47 812         | 47812.00          | 0.00%       | 0.0052        |
| X-n181-k23             | 25 569         | 25 569         | 25569.00           | 0.00%       | 0.0017        | 25 569         | 25569.00          | 0.00%       | 0.0022        |
| X-n186-k15             | 24 145         | 24 145         | 24145.00           | 0.00%       | 0.0015        | 24 145         | 24145.00          | 0.00%       | 0.0015        |
| X-n190-k8              | 16 980         | 16 980         | 16981.30           | 0.01%       | 0.0379        | 16 980         | 16981.00          | 0.01%       | 0.0396        |
| X-n195-k51             | 44 225         | 44 225         | 44225.00           | 0.00%       | 0.0037        | 44 225         | 44225.00          | 0.00%       | 0.0059        |
| X-n200-k36             | 58 578         | 58 578         | 58578.00           | 0.00%       | 0.0085        | 58 578         | 58578.00          | 0.00%       | 0.0080        |
| X-n204-k19             | 19 565         | 19 565         | 19565.00           | 0.00%       | 0.0016        | 19 565         | 19565.00          | 0.00%       | 0.0014        |
| X-n209-k16             | 30 656         | 30 656         | 30656.00           | 0.00%       | 0.0054        | 30 656         | 30656.00          | 0.00%       | 0.0037        |
| X-n214-k11             | 10 856         | 10 856         | 10856.30           | 0.00%       | 0.0280        | 10 856         | 10857.40          | 0.01%       | 0.0488        |
| X-n219-k73             | 117 595        | 117 595        | 117595.00          | 0.00%       | 0.0006        | 117 595        | 117595.00         | 0.00%       | 0.0007        |
| X-n223-k34             | 40 437         | 40 437         | 40437.00           | 0.00%       | 0.0053        | 40 437         | 40437.00          | 0.00%       | 0.0062        |
| X-n228-k23             | 25 742         | 25 742         | 25742.10           | 0.00%       | 0.0035        | 25 742         | 25742.00          | 0.00%       | 0.0026        |
| X-n233-k16             | 19 230         | 19 230         | 19230.00           | 0.00%       | 0.0046        | 19 230         | 19230.00          | 0.00%       | 0.0071        |
| X-n237-k14             | 27 042         | 27 042         | 27042.00           | 0.00%       | 0.0022        | 27 042         | 27042.00          | 0.00%       | 0.0026        |
| X-n242-k48             | 82 751         | 82 751         | 82771.30           | 0.02%       | 0.0541        | 82 751         | 82768.40          | 0.02%       | 0.0438        |
| X-n247-k50             | 37 274         | 37 274         | 37274.00           | 0.00%       | 0.0186        | 37 274         | 37274.00          | 0.00%       | 0.0214        |
| X-n251-k28             | 38 684         | 38 684         | 38684.00           | 0.00%       | 0.0191        | 38 684         | 38684.00          | 0.00%       | 0.0155        |
| X-n256-k16             | 18 839         | 18 839         | 18839.00           | 0.00%       | 0.0121        | 18 839         | 18839.00          | 0.00%       | 0.0119        |
| X-n261-k13             | 26 558         | 26 558         | 26558.00           | 0.00%       | 0.0257        | 26 558         | 26558.00          | 0.00%       | 0.0232        |
| X-n266-k58             | 75 478         | 75 529         | 75569.10           | 0.12%       | 0.1645        | 75 478         | 75551.90          | 0.10%       | 0.1405        |
| X-n270-k35             | 35 291         | 35 303         | 35303.00           | 0.03%       | 0.0368        | 35 303         | 35303.00          | 0.03%       | 0.0371        |
| X-n275-k28             | 21 245         | 21 245         | 21245.00           | 0.00%       | 0.0022        | 21 245         | 21245.00          | 0.00%       | 0.0020        |
| X-n280-k17             | 33 503         | 33 503         | 33508.80           | 0.02%       | 0.0859        | 33 503         | 33510.60          | 0.02%       | 0.0829        |
| X-n284-k15             | 20 215         | 20 216         | 20228.30           | 0.07%       | 0.1447        | 20 216         | 20227.30          | 0.06%       | 0.1149        |
| X-n289-k60             | 95 151         | 95 223         | 95240.30           | 0.09%       | 0.1404        | 95 151         | 95232.10          | 0.09%       | 0.1434        |
| X-n294-k50             | 47 161         | 47 161         | 47169.70           | 0.02%       | 0.0423        | 47 161         | 47174.60          | 0.03%       | 0.0581        |
| X-n298-k31             | 34 231         | 34 231         | 34232.10           | 0.00%       | 0.0152        | 34 231         | 34231.00          | 0.00%       | 0.0076        |
| X-n303-k21             | 21 736         | 21 736         | 21740.90           | 0.02%       | 0.0567        | 21 738         | 21739.60          | 0.02%       | 0.0477        |
| X-n308-k13             | 25 859         | 25 859         | 25862.70           | 0.01%       | 0.0491        | 25 861         | 25863.10          | 0.02%       | 0.0436        |
| X-n313-k71             | 94 043         | 94 044         | 94057.00           | 0.01%       | 0.0478        | 94 044         | 94070.20          | 0.03%       | 0.0584        |
| X-n317-k53             | 78 355         | 78 355         | 78355.00           | 0.00%       | 0.0046        | 78 355         | 78355.00          | 0.00%       | 0.0040        |
| X-n322-k28             | 29 834         | 29 834         | 29835.40           | 0.00%       | 0.0361        | 29 834         | 29838.90          | 0.02%       | 0.0470        |
| X-n327-k20             | 27 532         | 27 532         | 27533.00           | 0.00%       | 0.0266        | 27 532         | 27532.00          | 0.00%       | 0.0462        |
| X-n331-k15             | 31 102         | 31 102         | 31102.50           | 0.00%       | 0.0181        | 31 102         | 31102.60          | 0.00%       | 0.0199        |
| X-n336-k84             | 139 111        | 139 176        | 139236.40          | 0.09%       | 0.1403        | 139 192        | 139220.90         | 0.08%       | 0.1410        |
| X-n344-k43             | 42 050         | 42 055         | 42065.90           | 0.04%       | 0.0687        | 42 055         | 42061.00          | 0.03%       | 0.0543        |
| X-n351-k40             | 25 896         | 25 929         | 25938.40           | 0.16%       | 0.2055        | 25 932         | 25938.20          | 0.16%       | 0.2036        |
| X-n359-k29             | 51 505         | 51 527         | 51561.50           | 0.11%       | 0.2123        | 51 506         | 51552.30          | 0.09%       | 0.1930        |
| X-n367-k17             | 22 814         | 22 814         | 22814.00           | 0.00%       | 0.0086        | 22 814         | 22814.00          | 0.00%       | 0.0065        |
| X-n376-k94             | 147 713        | 147 713        | 147713.00          | 0.00%       | 0.0049        | 147 713        | 147713.00         | 0.00%       | 0.0050        |
| X-n384-k52             | 65 928         | 65 978         | 66017.00 38260.00  | 0.13%       | 0.1802        | 65 961         | 65993.60          | 0.10%       | 0.1548        |
| X-n393-k38             | 38 260         | 38 260         |                    | 0.00%       | 0.0146        | 38 260         | 38260.00          | 0.00%       | 0.0114        |
| X-n401-k29             | 66 154         | 66 188         | 66204.20           | 0.08% 0.02% | 0.1171        |                | 19715.90          | 0.03%       | 0.0800        |
|                        | 19 712 107 798 | 19 712 107 810 | 19715.80 107817.00 | 0.02%       | 0.0286        | 66 163         | 66175.00          | 0.02%       | 0.0394 0.0315 |
| X-n411-k19 X-n420-k130 |                |                |                    |             | 0.0336        | 19 712 107 798 | 107813.20         | 0.01%       |               |
| X-n429-k61             | 65 449         | 65 455         | 65471.30           | 0.03%       | 0.0601        | 65 452         | 65466.30          | 0.03%       | 0.0514        |
| X-n439-k37 X-n449-k29  | 36 391 55 233  | 36 394 55 273  | 36394.90 55317.80  | 0.01% 0.15% | 0.0170 0.2215 | 36 395 55 245  | 36395.50 55298.30 | 0.01% 0.12% | 0.0214 0.2383 |

10970037, 2024, 4, Downloaded from https://onlinelibrary.wiley.com/doi/10.1002/net.22213 by Technische UniversitGLYPH&lt;228&gt;t München, Wiley Online Library on [30/03/2025]. See the Terms and Conditions (https://onlinelibrary.wiley.com/terms-and-conditions) on Wiley Online Library for rules of use; OA articles are governed by the applicable Creative Commons License

TABLE B3 Comparison of HGS-CVRP and MDM-HGS over ten runs (3 of 3).

|                               | HGS-CVRP        | HGS-CVRP        | HGS-CVRP            | MDM-HGS     | MDM-HGS       | MDM-HGS         | MDM-HGS             | MDM-HGS     | MDM-HGS       |
|-------------------------------|-----------------|-----------------|---------------------|-------------|---------------|-----------------|---------------------|-------------|---------------|
| Instance                      | BKS             | Best cost       | Avg cost            | Gap         | Avg PI        | Best cost       | Avg cost            | Gap         | Avg PI        |
| X-n459-k26                    | 24 139          | 24 139          | 24144.00            | 0.02%       | 0.0662        | 24 139          | 24143.80            | 0.02%       | 0.0681        |
| X-n469-k138                   | 221 824         | 221 917         | 222019.70           | 0.09%       | 0.1312        | 222 011         | 222070.70           | 0.11%       | 0.1541        |
| X-n480-k70                    | 89 449          | 89 457          | 89474.50            | 0.03%       | 0.0653        | 89 457          | 89462.20            | 0.01%       | 0.0595        |
| X-n491-k59                    | 66 483          | 66 517          | 66573.30            | 0.14%       | 0.1964        | 66 490          | 66593.60            | 0.17%       | 0.2354        |
| X-n502-k39                    | 69 226          | 69 231          | 69240.60            | 0.02%       | 0.0326        | 69 230          | 69236.00            | 0.01%       | 0.0321        |
| X-n513-k21                    | 24 201          | 24 201          | 24201.00            | 0.00%       | 0.0191        | 24 201          | 24201.00            | 0.00%       | 0.0103        |
| X-n524-k153                   | 154 593         | 154 614         | 154658.60           | 0.04%       | 0.0796        | 154 646         | 154661.80           | 0.04%       | 0.0727        |
| X-n536-k96                    | 94 846          | 94 951          | 95022.90            | 0.19%       | 0.2370        | 94 947          | 94989.10            | 0.15%       | 0.2115        |
| X-n548-k50                    | 86 700          | 86 704          | 86723.40            | 0.03%       | 0.0589        | 86 700          | 86729.70            | 0.03%       | 0.0754        |
| X-n561-k42                    | 42 717          | 42 719          | 42727.10            | 0.02%       | 0.0597        | 42 719          | 42719.00            | 0.00%       | 0.0304        |
| X-n573-k30                    | 50 673          | 50 746          | 50758.60            | 0.17%       | 0.2328        | 50 744          | 50764.50            | 0.18%       | 0.2284        |
| X-n586-k159                   | 190 316         | 190 417         | 190490.80           | 0.09%       | 0.1299        | 190 350         | 190449.60           | 0.07%       | 0.1172        |
| X-n599-k92                    | 108 451         | 108 589         | 108635.90           | 0.17%       | 0.2176        | 108 458         | 108558.40           | 0.10%       | 0.1868        |
| X-n613-k62                    | 59 535          | 59 583          | 59623.50            | 0.15%       | 0.1979        | 59 559          | 59609.00            | 0.12%       | 0.1845        |
| X-n627-k43                    | 62 164          | 62 270          | 62315.20            | 0.24%       | 0.3758        | 62 226          | 62305.50            | 0.23%       | 0.3232        |
| X-n641-k35                    | 63 682          | 63 745          | 63811.00            | 0.20%       | 0.2959        | 63 706          | 63769.40            | 0.14%       | 0.2400        |
| X-n655-k131                   | 106 780         | 106 791         | 106801.30           | 0.02%       | 0.0311        | 106 786         | 106800.80           | 0.02%       | 0.0286        |
| X-n670-k130                   | 146 332         | 146 562         | 146755.90           | 0.29%       | 0.3748        | 146 604         | 146718.20           | 0.26%       | 0.3274        |
| X-n685-k75                    | 68 205          | 68 298          | 68337.40            | 0.19%       | 0.2656        | 68 250          | 68340.30            | 0.20%       | 0.2741        |
| X-n701-k44                    | 81 923          | 82 084          | 82155.10            | 0.28%       | 0.3772        | 82 047          | 82201.80            | 0.34%       | 0.5019        |
| X-n716-k35                    | 43 373          | 43 423          | 43459.00            | 0.20%       | 0.2963        | 43 441          | 43468.00            | 0.22%       | 0.3128        |
| X-n733-k159                   | 136 187         | 136 320         | 136364.10           | 0.13%       | 0.1871        | 136 317         | 136372.30           | 0.14%       | 0.1768        |
| X-n749-k98                    | 77 269          | 77 495          | 77575.50            | 0.40%       | 0.5321        | 77 467          | 77573.90            | 0.39%       | 0.5065        |
| X-n766-k71                    | 114 417         | 114 603         | 114714.40           | 0.26%       | 0.3870        | 114 578         | 114660.90           | 0.21%       | 0.2825        |
| X-n783-k48                    | 72 386          | 72 523          | 72676.50            | 0.40%       | 0.5657        | 72 529          | 72644.10            | 0.36%       | 0.4890        |
| X-n801-k40                    | 73 305          | 73 373          | 73425.80            | 0.16%       | 0.2657        | 73 315          | 73434.30            | 0.18%       | 0.2796        |
| X-n819-k171                   | 158 121         | 158 307         | 158442.40           | 0.20%       | 0.2833        | 158 254         | 158386.60           | 0.17%       | 0.2581        |
| X-n837-k142                   | 193 737         | 194 102         | 194185.90           | 0.23%       | 0.3366        | 193 982         | 194126.30           | 0.20%       | 0.3122        |
| X-n856-k95                    | 88 965          | 88 975          | 89001.70            | 0.04%       | 0.0737        | 89 003          | 89020.50            | 0.06%       | 0.0915        |
| X-n876-k59                    | 99 299          | 99 536          | 99619.20            | 0.32%       | 0.4252        | 99 528          | 99620.30            | 0.32%       | 0.4442        |
| X-n895-k37                    | 53 860          | 53 983          | 54052.20            | 0.36%       | 0.4975        | 53 918          | 54031.40            | 0.32%       | 0.5010        |
| X-n916-k207                   | 329 179         | 329 568         | 329774.30           | 0.18%       | 0.3071        | 329 555         | 329711.30           | 0.16%       | 0.2933        |
| X-n936-k151                   | 132 715         | 133 093         | 133268.40           | 0.42%       | 0.4964        | 133 111         | 133283.60           | 0.43%       | 0.5402        |
| X-n957-k87                    | 85 465          | 85 496          | 85523.20            | 0.07%       | 0.1254        | 85 487          | 85528.30            | 0.07%       | 0.1335        |
| X-n979-k58                    | 118 976         | 119 125         | 119210.70           | 0.20%       | 0.3617        | 119 128         | 119193.20           | 0.18%       | 0.3553        |
| X-n1001-k43                   | 72 355          | 72 541          | 72643.50            | 0.40%       | 0.6588        | 72 578          | 72641.30            | 0.40%       | 0.6888        |
| Loggi-n401-k23                | 336 903         | 337 022         | 337096.90           | 0.06%       | 0.0949        | 337 015         | 337060.60           | 0.05%       | 0.0953        |
| Loggi-n501-k24                | 177 078         | 177 466         | 177491.10           |             | 0.2420        | 177 209         | 177471.70           | 0.22%       | 0.2411        |
| Loggi-n601-k19                | 113 155         | 113 214         |                     | 0.23%       | 0.1369        | 113 220         | 113270.80           | 0.10%       | 0.1365        |
| Loggi-n601-k42                | 347 059         | 347 059         | 113262.10 347064.40 | 0.09% 0.00% | 0.0206        | 347 046         | 347053.90           | 0.00%       | 0.0088        |
| Loggi-n901-k42                | 246 301         | 246 478         | 246597.20           | 0.12%       | 0.2299        | 246 326         | 246553.20           | 0.10%       | 0.2207        |
| Loggi-n1001-k31               | 284 356         | 285 263         | 285677.90           | 0.46%       | 0.6623        | 285 451         | 285631.00           | 0.45%       | 0.6271        |
| ORTEC-n242-k12                | 123 750         | 123 750         | 123752.80           | 0.00%       | 0.0190        | 123 750         | 123755.00           | 0.00%       | 0.0204        |
| ORTEC-n323-k21                | 214 071         | 214 071         | 214111.20           | 0.02%       | 0.0405        | 214 071         | 214085.60           | 0.01%       | 0.0316        |
| ORTEC-n405-k18 ORTEC-n455-k41 | 200 986 292 485 | 200 986 292 516 | 200986.00 292519.90 | 0.00% 0.01% | 0.0045 0.0405 | 200 986 292 516 | 200986.00 292541.20 | 0.00% 0.02% | 0.0030 0.0628 |
| ORTEC-n510-k23                | 184 529         | 184 529         | 184594.20           | 0.04%       | 0.1019        | 184 529         | 184542.60           | 0.01%       | 0.1292        |
| ORTEC-n701-k64                | 445 543         | 445 585         | 445672.20           | 0.03%       | 0.0680        | 445 595         | 445719.20           | 0.04%       | 0.0879        |

10970037, 2024, 4, Downloaded from https://onlinelibrary.wiley.com/doi/10.1002/net.22213 by Technische UniversitGLYPH&lt;228&gt;t München, Wiley Online Library on [30/03/2025]. See the Terms and Conditions (https://onlinelibrary.wiley.com/terms-and-conditions) on Wiley Online Library for rules of use; OA articles are governed by the applicable Creative Commons License