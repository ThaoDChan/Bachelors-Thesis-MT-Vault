(0123456789().,-volV

(0123456789().volV

,-

## METHODOLOGIES AND APPLICATION

## The new optimization algorithm for the vehicle routing problem with time windows using multi-objective discrete learnable evolution model

Behzad Moradi 1

- /C211 Springer-Verlag GmbH Germany, part of Springer Nature 2019

## Abstract

This paper presents a new multi-objective discreet learnable evolution model (MODLEM) to address the vehicle routing problem with time windows (VRPTW). Learnable evolution model (LEM) includes a machine learning algorithm, like the decision trees, that can discover the correct directions of the evolution leading to significant improvements in the fitness of the individuals. We incorporate a robust strength Pareto evolutionary algorithm in the LEM presented here to govern the multi-objective property of this approach. A new priority-based encoding scheme for chromosome representation in the LEM as well as corresponding routing scheme is introduced. To improve the quality and the diversity of the initial population, we propose a novel heuristic manner which leads to a good approximation of the Pareto fronts within a reasonable computational time. Moreover, a new heuristic operator is employed in the instantiating process to confront incomplete chromosome formation. Our proposed MODLEM is tested on the problem instances of Solomon's VRPTW benchmark. The performance of this proposed MODLEM for the VRPTW is assessed against the state-of-the-art approaches in terms of both the quality of solutions and the computational time. Experimental results and comparisons indicate the effectiveness and efficiency of our proposed intelligent routing approach.

Keywords Vehicle routing problem with time windows (VRPTW) /C1 Learnable evolution model (LEM) /C1 Multi-objective combinatorial optimization (MOCO) /C1 Strength Pareto evolutionary algorithm (SPEA)

## 1 Introduction

Companies annually spend substantial amount of the transportation cost on distribution of goods and services. Hence, the efficient routing of vehicles to serve customers is an important goal. The vehicle routing problem with time windows (VRPTW) is one of the best-known and the most important combinatorial optimization problems in operational research, transportation science and computer science. It is concerned with determining the optimal set of routes for a fleet of identical vehicles with restricted capacity that depart from a central depot and serve a set of geographically dispersed customers with known demands and predefined time windows and finally return to the central depot within a specific time window. Each customer is visited once and only once by exactly one vehicle within related time window, i.e., the earliest and latest time for starting the service. The VRPTW, as an important extension of vehicle routing problem, is used in many real-world applications such as postal delivery, waste collection, school bus routing, food and beverage delivery, cash delivery to banks and ATM terminals, and maintaining operations. The graphical model of a simple VRPTW and a typical solution are shown in Fig. 1, where the central depot and customers are denoted as 0 and 1 through 9, respectively. The solution includes three routes: 0-3-5-96-0, 0-4-1-7-0 and 0-2-8-0.

Communicated by V. Loia.

&amp;

Behzad Moradi b.moradi@kut.ac.ir

1 Department of Computer Engineering, Kermanshah University of Technology, Kermanshah, Iran

Due to its complexities and usefulness, intensive research has focused on developing both exact and approximate (heuristic/meta-heuristic) approaches for solving the VRPTW (Desaulniers et al. 2014). Aside from their inability to support multi-objective optimization, exact approaches are inefficient with large-scale VRPTW

![Image](Papers_Converted/0_Artifacts/[moradi2020]_The_new_optimization_algorithm_for_the_vehicle_routing_problem_with_time_windows_using_multi-objective_discrete_learnable_evolution_model_artifacts/image_000000_9878e20f2e7273d916c37d14b5a54582caea7a7c412334e2ca297d12c0da92b4.png)

Fig. 1 A typical VRPTW

![Image](Papers_Converted/0_Artifacts/[moradi2020]_The_new_optimization_algorithm_for_the_vehicle_routing_problem_with_time_windows_using_multi-objective_discrete_learnable_evolution_model_artifacts/image_000001_36946710d562a515577557fa61f37b586cdc939ec89d13839515298a876e52fa.png)

instances due to their runtime. In some approximate approaches, two-phase algorithms are incorporated for optimizing two VRPTW objectives in lexicographical order, i.e., first the number of vehicles (NV) is minimized, and then, the total traveled distance (TD) is minimized with the minimal number of vehicles. Some other approximate approaches deal with the VRPTW as a multi-objective optimization problem, where both conflicting objectives are optimized in a simultaneous manner. Approximate approaches are capable of finding optimal or near-optimal solutions in a reduced runtime, especially in large-scale problem instances.

The VRPTW as a constrained multi-objective problem is included in the category of NP-hard problems. Hence, multi-objective evolutionary computation algorithms (MOECAs) and multi-objective swarm intelligence algorithms (MOSIAs) are appropriate alternatives to solve this problem. Much research effort has been expended on the development of MOECAs and MOSIAs to tackle the VRPTW in the literature. Despite the fact that the evolutionary computation algorithms (ECAs) including genetic algorithm, evolutionary strategy, differential evolution, etc. (which are Darwinian-type ECAs), and the swarm intelligence algorithms (SIAs), including particle swarm optimization, ant colony optimization, shuffled frog leaping algorithm, etc., are able to overcome the limitations of classical approaches and exhibit better performance, they have several drawbacks. The Darwinian-type ECAs perform a semi-blind search using usual genetic operators, which cause misspending of time. The semi-random operators applied in these algorithms make random modifications in the potential optimal solutions which sometimes mislead the search process, resulting in increasing the number of iterations. Although the SIAs outperform the Darwinian-type ECAs in solving the VRPTW, they endure such disadvantages as weak local search ability, high computational cost due to slow convergence rate and the principal role of randomness.

A multi-objective intelligent approach is proposed in this article based on learnable evolution model (LEM) to address the VRPTW. The LEM is a non-Darwinian-type evolutionary computation algorithm, where the evolutionary process is governed through a machine learning method. Unlike other proposed approaches, a robust multiobjective evolutionary algorithm (i.e., strength Pareto evolutionary algorithm) is applied to handle the conflicting objectives of the VRPTW. To the extent of the authors' knowledge here, this is the first work that solves the VRPTW through a multi-objective discrete learnable evolution model (MODLEM). The main contributions of this article are summarized as follows:

- · A multi-objective non-Darwinian-type evolutionary computation algorithm, namely learnable evolution model (LEM), is applied to solve the VRPTW where a machine learning method is adopted to avoid the semiblind search. The LEM reduces the number of iterations remarkably, leading to a shorter search process.
- · A new chromosome representation and corresponding routing scheme are proposed to encode the VRPTW solutions into chromosomes and to decode each chromosome to a set of vehicle routes.
- · The objectives of the VRPTW, i.e., the number of vehicles and the total traveled distance, are governed by the most efficient version of the strength Pareto evolutionary algorithm (i.e., SPEA2 ? ) incorporated into the LEM presented here.
- · Several novel heuristics are employed in the initialization phase and the instantiating process in order to generate populations more efficiently.

The remainder of this article is organized as follows. Section 2 presents the problem formulation of the VRPTW. Section 3 reviews the existing approaches for solving the VRPTW. The proposed chromosome representation is described in Sect. 4 followed by a presentation of our proposed MODLEM framework for the VRPTW being provided in Sect. 5. Experimental results are presented in Sect. 6. Finally, Sect. 7 concludes the article.

## 2 Vehicle routing problem with time windows

The VRPTW can be defined as follows: n customers are waiting to be served, each of which requires a quantity of services/goods. A fleet of identical vehicles is located in

the central depot to distribute the services/goods. The capacity of each vehicle must be greater than or equal to the total of all demands on the route traveled by the vehicle. Besides, each customer must be served once and only once by exactly one vehicle with in its time window, i.e., a predefined time interval described by the earliest and the latest arrival time. The vehicles have to arrive at the customers before the latest arrival time. A vehicle is allowed to arrive before the earliest arrival time, in which case it must wait until the earliest arrival time and then begin to serve the customer. Each customer imposes an additional service time to the route, i.e., the loading or unloading time of goods or the time required to deliver services. Each vehicle is supposed to depart from and return to the central depot within a specific time window. A solution of the VRPTW is a set of routes in which a vehicle starts from the central depot, serves customers and then returns to the initial point under capacity and time window constraints. The number of routes in the network is equal to the fleet size, and one vehicle is dedicated to one route (Ban ˜Os et al. 2013a, b; Qi et al. 2015).

The multi-objective vehicle routing problem with time windows (MO-VRPTW) consists of minimizing the fleet size and the total distance traveled by the vehicles to supply all customers. The mathematical formulation of the VRPTW is described as follows. There is a non-directed complete graph G = ( V E , ), where V = { v 0 , v 1 , … , vn } and E = {( vi , vj )| vi , vj [ V i , = j } are the node set and the arc set, respectively. v 0 represents the central depot and vi ( i = 1, 2, … , n ) represents customers. The customers are served using a fleet of identical vehicles with same capacity Q . Each node is associated with a demand quantity qi and a time window [ ei , bi ], among which node v 0 is associated with q 0 = 0 and [0, b 0 ].

The time window constraint indicates that a vehicle k should arrive at the location of customer i (at t k i ) before the latest arrival time bi . However, arriving earlier than the earliest arrival time ei results in a waiting time w k i (Fig. 2). Each customer i has a service time s i , the actual time that the delivery takes once a vehicle arrives at the customer's location. Each vehicle is supposed to complete its individual route within the time window of the central depot. Each arc in the network represents a connection between two nodes, associated with a traveling distance dij , i.e., Euclidean distance between nodes vi and vj , and a traveling time t ij , which is proportional to the traveling distance. Let N and K represent the number of customers and the fleet size, respectively.

The optimization model of the MO-VRPTW can be defined as follows:

8

$$\text{i. The } \quad \begin{cases} \min F ( x ) = ( f _ { 1 }, f _ { 2 } ) \\ f _ { 1 } = K \\ \varepsilon \text{ and } \quad \begin{cases} \min F ( x ) = ( f _ { 1 }, f _ { 2 } ) \\ f _ { 2 } = \sum _ { i = 1 } ^ { N } \sum _ { j = 0, j \neq i } ^ { K } \sum _ { k = 1 } ^ { K } d _ { i j } ^ { k } \\ \text{ndow}, \end{cases}$$

6

Subject to

$$\text{ice is} \quad \sum _ { j = 1 } ^ { N } x _ { 0 j } ^ { k } = \sum _ { i = 1 } ^ { N } x _ { i 0 } ^ { k } = 1, \, k \in \{ 1, 2, \dots, K \} \quad \quad ( 2 )$$

6

$$\underset { \text{ing or } } { \text{ then } } \underset { \text{$\sum_{j=0,j\neq i}^{N}$} } { \text{$\sum_{j=0,j\neq i}^{N}$} } x _ { j } ^ { k } = \underset { \text{$\sum_{j=0,j\neq i}^{N}$} } { \overset { N } { \text{$\sum_{j=0,j\neq i}^{N}$} } } x _ { j i } ^ { k } \leq 1, \ i \in \{ 1, 2, \dots, N \}, \ k \\ \underset { \text{$\sum_{j=0,j\neq i}^{N}$} } { \text{$\sum_{j=0,j\neq i}^{N}$} } }$$

6

$$\underset { \bullet } { \text{low. } A } \quad \underset { k = 1 } { \sum } \underset { i = 0, i \neq j } { \sum } ^ { K } x _ { i j } ^ { k } = 1, \, j \in \{ 1, 2, \dots, N \}$$

6

$$\underset { \text{$d$ to} } { \text{ then} } \underset { \text{$u$ to} } { \text{ $v$} } \sum _ { k = 1 } ^ { K } \sum _ { j = 0, j \neq i } ^ { N } x _ { i j } ^ { k } = 1, i \in \{ 1, 2, \dots, N \} \text{ \quad \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \ \ } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad } \text{ \quad }$$

6

$$\intertext { h \text{ time} _ { - a \dots } } \sum _ { i = 0 } ^ { N } q _ { i } \sum _ { j = 0, j \neq i } ^ { N } x _ { i j } ^ { k } \leq Q, \, k \in \{ 1, 2, \dots, K \}$$

6

6

$$\begin{smallmatrix} e \text{ meet} \\ \text{less to} \\ \text{of the} \end{smallmatrix} \quad t _ { i } ^ { k } + w _ { i } ^ { k } + s _ { i } + t _ { i j } & = t _ { j } ^ { k }, \, i, j \in \{ 1, 2, \dots, N \}, \, i \neq j, \, k \\ \in \{ 1, 2, \dots, K \} \quad \quad ( 7 )$$

/C0

/C1

$$\text{irected} _ { \mu } \quad e _ { i } \leq \left ( t _ { i } ^ { k } + w _ { i } ^ { k } \right ) \leq b _ { i }, \ i \in \{ 0, 1, \dots, N \}, \ k \in \{ 1, 2, \dots, K \} \\ \text{irc set},$$

where

/C26

$$\underset { \colon \text{ and } a } { \ s a v a } ^ { s a v a } = \begin{cases} 1 & \text{if vehicle $k$ travels from $v_{i}$ to $v_{j}$} \\ 0 & \text{otherwise} \end{cases}$$

Formula (1) refers to minimizing two objective functions, f 1 and f 2 , representing the number of used vehicles and the total traveled distance, respectively. Constraints (2) and (3) guarantee that vehicles start the delivery task from the central depot, serve customers one by one and then return to the central depot. Constraints (4) and (5) state that a customer is visited only once by exactly one vehicle. Constraint (6) denotes that the quantity of services/goods delivered by each vehicle could not exceed its capacity Q . Equation (7) describes how the travel time is computed. Constraint (8) ensures that time windows are observed. Expression (9) indicates that x k ij is equal to 1 if vehicle k goes from node vi to node vj , and is equal to 0 otherwise.

Fig. 2 Examples of the time windows in the VRPTW: a visiting customer i before its time window starts; b visiting customer i within its time window; c visiting customer i after closing its time window

![Image](Papers_Converted/0_Artifacts/[moradi2020]_The_new_optimization_algorithm_for_the_vehicle_routing_problem_with_time_windows_using_multi-objective_discrete_learnable_evolution_model_artifacts/image_000002_4b6d54e7d7878c26f9c1c50b774fb324f053f0d1ed4c87fd6ff23587903bb2a3.png)

## 3 Literature review

## 3.1 Vehicle routing problem with time windows

Due to the NP-hardness of the VRPTW and its widespread real-life applications, intensive research has focused on developing different approaches for solving the problem and its variants over the past years. The approaches introduced cope with the VRPTW are classified in three main groups, that is, exact, heuristic and meta-heuristic.

- relaxations are solved through column generation and cutting planes are generated to strengthen the linear relaxations. Column generation is an iterative process for solving linear programs involving a large number of variables.

## 3.1.1 Exact approaches

Numerous exact approaches are proposed to handle the VRPTW by the following groups (Kallehauge 2008; Baldacci et al. 2012; Desaulniers et al. 2014; Pecin et al. 2017):

- (1) Agra et al. (2013), Chabrier (2006), Desaulniers et al. (2011), Desrochers et al. (1992), Feillet et al. (2004, 2007), Fisher et al. (1997), Irnich and Villeneuve (2006), Irnich et al. (2010), Jepsen et al. (2006, 2008), Kallehauge et al. (2006), Kolen et al. (1987), Kohl et al. (1999), Larsen (2004), Lozano et al. (2015), Petersen et al. (2008), PrescottGagnon et al. (2009), Righini and Salani (2006) and Rousseau et al. (2007) who follow the ' 'branch and --cut and price'' --(BCP) approach. The BCP is a branch-and-bound algorithm in which the linear
- (2) Bard et al. (2002), Cook and Rich (1999), Kallehauge et al. (2007), Letchford and Salazar-Gonza ´lez (2006) and Lysgaard (2006) who follow the ' 'branch and cut'' --(BC) approach. The BC involves a cutting plane method incorporated into a branchand-bound algorithm. The cutting planes are used to tighten the linear relaxations.
- (3) Baldacci et al. (2010, 2011) who follow the ' 'reduced set partitioning'' (RSP) approach. The RSP consists of a procedure to reduce the size of the VRPTW set partitioning model through assigning a value of zero to a very large number of the problem variables and a mixed-integer programming solver to deal with the reduced set partitioning model.

The classical approaches might not be able to deal with the large-scale VRPTW instances because the required runtime rises in an exponential manner as the number of customers increases, leading to the development of approximate approaches to tackle the VRPTW where acceptable solutions can be obtained in reasonable computational time (Gong et al. 2012).

## 3.1.2 Heuristic approaches

The problem-specific heuristic approaches introduced for solving the VRPTW are categorized into ' 'route -constructing'' (Solomon 1986, 1987; Potvin and Rousseau 1993; Antes and Derigs 1995; Kontoravdis and Bard 1995; Russell 1995; Liu and Shen 1999; Ioannou et al. 2001; Dullaert and Bra ¨ysy 2003; Repoussis et al. 2006; Pang 2011) and ' 'route -improving'' methods (Russell 1995, 1977; Savelsbergh 1985, 1990, 1992; Baker and Schaffer 1986; Solomon and Desrosiers 1988; Van Landeghem 1988; Potvin and Rousseau 1995; Rochat and Taillard 1995; Bra ¨ysy 2001; Lau et al. 2001; Rousseau et al. 2002; Lim and Zhang 2007; Nagata and Bra ¨ysy 2009).

Route-constructing heuristic methods select customers (or connecting arcs) in a sequential manner until a feasible rout is created. Customers are chosen based on some cost minimization criteria so that the problem constraints (i.e., vehicle capacity and time windows) are not violated. In sequential route-constructing methods, one route at a time is generated, while parallel route-constructing methods build several routes at the same time.

Route-improving heuristic methods seek to generate an improved route on the basis of an already available route through a local search. First an initial feasible route is generated in a random or heuristic manner. Then an iterative process is performed to improve the current route until the termination condition is met as follows: A movegeneration mechanism is applied to create a candidate set of the neighboring routes through changing one attribute or a combination of attributes of the current route. A neighboring route is selected from the candidate set considering acceptance strategy (i.e., first-accept or best-accept). Once a neighboring route is identified, it is compared against the current route. If the selected neighboring route is better than the current route, it is replaced by the current route, and the search continues.

## 3.1.3 Meta-heuristic approaches

The family of meta-heuristic approaches proposed for solving the VRPTW includes, but is not limited to, tabu search, simulated annealing, evolutionary computation and swarm intelligence algorithms.

Some researchers like Badeau et al. (1997), Beham (2007), Chiang and Russell (1997), Cordeau et al. (2001), Cordeau and Maischberger (2012), Jiang et al. (2014), Lau et al. (2003), Lee et al. (2003), Paraskevopoulos et al. (2008) and Qi et al. (2008) have proposed a tabu search algorithm to deal with the VRPTW (Badeau et al. 1997; Chiang and Russell 1997; Cordeau et al. 2001; Lau et al. 2003; Lee et al. 2003; Beham 2007; Paraskevopoulos et al.

2008; Qi et al. 2008; Cordeau and Maischberger 2012; Jiang et al. 2014).

Ban ˜Os et al. (2013a, b), Chiang and Russell (1996), Czech and Czarnas (2002), Tavakkoli-Moghaddam et al. (2011) and Woch and Łebkowski (2009) have presented a simulated annealing algorithm to tackle the VRPTW (Chiang and Russell 1996; Czech and Czarnas 2002; Woch and Łebkowski 2009; Tavakkoli-Moghaddam et al. 2011; Ban ˜Os et al. 2013a, b).

Many researchers have addressed the VRPTW through an evolutionary computation algorithm. Thangiah et al. (1991) presented a genetic algorithm system, called GIDEON, to heuristically solve the VRPTW. Potvin and Bengio (1996) proposed GENEROUS, the GENEtic ROUting System, to cope with the VRPTW which is based on the natural evolution paradigm. Berger et al. (2003) and Berger and Barkaoui (2004) introduced a hybrid genetic algorithm to address the VRPTW. Ombuki et al. (2006) represented the VRPTW through as a multi-objective problem in which the problem is solved through the Pareto ranking technique incorporated into the genetic algorithm. Garcia-Najera and Bullinaria (2009, 2011) developed a novel multi-objective evolutionary algorithm to solve the VRPTW which incorporated a method for measuring the similarity of solutions in order to maintain population diversity. Wang et al. (2008a, b) provided an improved genetic algorithm consists of new chromosome representation, new crossover operator and new crossover strategy. Nazif and Lee (2010) solved the VRPTW through a genetic algorithm where an optimized crossover operator is applied. Błocho and Czech (2011) proposed a parallel genetic algorithm for minimizing fleet size in the VRPTW. Blocho and Czech (2012) extended their previous work by introducing the usage of the edge assembly crossover (EAX) operator during exchanging the best solutions between the components. Ursani et al. (2011) proposed a localized optimization framework which is an iterative procedure between two phases, optimization and de-optimization, to handle the VRPTW. Hsu and Chiang (2012) and Chiang and Hsu (2014) developed a VRPTW solution procedure based on an evolutionary algorithm in which problem-specific knowledge incorporated into the genetic operators. Kumar et al. (2014) introduced a genetic algorithm with fitness aggregation approach and specialized genetic operators called fitness aggregated genetic algorithm (FAGA) to solve the VRPTW. Pierre and Zakaria (2014, 2017) provided a stochastic partially optimized cyclic shift crossover operator incorporated into a genetic algorithm for the optimization of the multi-objective VRPTW.

Labadi et al. (2008) solved the VRPTW through a memetic algorithm, i.e., a hybridization of the genetic algorithm and local refinement procedure. Nagata et al.

(2010) proposed an effective memetic algorithm to tackle the VRPTW. Blocho and Czech (2013) presented a parallel memetic algorithm for solving the VRPTW; that is, components of the algorithm are executed as parallel processes. Nalepa and Czech (2012) provided a two-phase approach, including parallel memetic algorithms, to solve the VRPTW. Nalepa and Czech (2013) proposed a memetic algorithm including new selection schemes in order to avoid the premature convergence of the search and also to keep the balance between the exploration and exploitation during the search. Nalepa (2014) and Nalepa and Blocho (2016) proposed an adaptive memetic algorithm in which the parameters of the algorithm are adjusted dynamically during the search. Qi et al. (2015) developed a memetic algorithm following the framework of multi-objective evolutionary algorithm based on decomposition (MMOEA/D) for dealing with multi-objective VRPTW (MOVRPTW).

Le Bouthillier and Crainic (2005) presented a parallel cooperative method where several search threads communicate through asynchronous exchanges of information using a common solution warehouse. Le Bouthillier et al. (2005) extended the approach of their previous work to a guided parallel cooperative search method. Barbucha (2013, 2014) provided an agent-based cooperative population learning algorithm for the VRPTW. Dan et al. (2009) presented a multi-agent model system for the VRPTW based on the internal behavior of agents and coordination among them.

Yassen et al. (2015) proposed a meta-harmony search algorithm (meta-HSA) to cope with the VRPTW where two HSA algorithms, HSA-optimizer and HSA-solver, are adopted. Melia ´n-Batista et al. (2014) described the VRPTW through a bi-objective mixed-integer linear model and developed a solution approach based on the scatter search meta-heuristic.

Homberger and Gehring (1999) solved the VRPTW through two different evolutionary strategies. Mester et al. (2007) developed an efficient evolution strategies algorithm for vehicle routing problem with time window constraints. Repoussis et al. (2009) proposed an arc-guided evolutionary algorithm for solving the VRPTW.

Calvete et al. (2007) formulated the VRPTW through a goal programming approach. Ghoseiri and Ghannadpour (2010) modeled the VRPTW through goal programming and applied a multi-objective genetic algorithm to address it.

Swarm intelligence algorithms have been used by many researchers for solving the VRPTW. Gambardella et al. (1999) presented an ant colony optimization-based approach to solve the VRPTW. In the same manner, Bara ´n and Schaerer (2003) proposed another ant colony optimization algorithm to solve the VRPTW. Gong et al.

(2007) provided a two-generation ant colony optimization to deal with the VRPTW. Castro et al. (2009) provided a VRPTW solver based on multi-objective discrete particle swarm optimization in which infeasible solutions are evolved to feasible ones through a dynamic trade-off between objectives. Mun ˜oz-Zavala et al. (2009) proposed a particle swarm optimization algorithm to address the VRPTW. Amini et al. (2010) provided a particle swarm optimization algorithm to cope with a real-world case study VRPTW. Niu et al. (2012) and Tan et al. (2015) developed a novel bacterial foraging optimization (BFO) approach with adaptive chemotaxis step to solve the VRPTW.

It is worth mentioning that the basic approaches are combined to introduce hybrid approaches for solving the VRPTW. Two or more exact, heuristic and meta-heuristic algorithms are simultaneously incorporated into hybrid methods. The hybrid approaches employed in the recent works on the VRPTW are tabulated in Table 1. For more details on swarm intelligence hybrid approaches for solving the VRPTW and its variants, readers are encouraged to refer to Dhanya and Kanmani (2016).

## 3.2 Learnable evolution model

Darwinian-type EC is inspired from the principles of Darwin's theory of evolution. The Darwinian-type EC applies usual genetic operators like mutation, crossover and selection to generate new populations. The mutation is a random modification of the current individual, the crossover is a semi-random recombination of two or more individuals, and the selection is a parallel hill climbing. These semi-random operators, which govern the evolution process, do not consider the experiences of individuals or the experience of an entire population or a collection of populations. Therefore, new individuals are generated through a parallel trial-and-error process, so the lessons learned from the past generations are not used in this type of evolution (Michalski 2000; Wojtusiak 2012).

The Darwinian-type operators are domain-independent and can be simply applied without any knowledge of the problem area. The Darwinian-type EC algorithms are applied to various problems in different fields. However, the Darwinian-type EC is semi-blind and exhibits low efficiency in solving highly complex problems in terms of the evolution time caused by the basal role of randomness (Michalski 2000; Wojtusiak 2012).

Learnable evolution model (Michalski 2000) is a nonDarwinian-type EC algorithm where the evolutionary process is governed through a machine learning method. LEM seeks to determine why certain individuals in a population have better fitness in relation to others. The revealed reasons are expressed as inductive hypotheses and then used in generating new populations. In fact, LEM

| Table 1 VRPTW hybrid solution approach   | Hybrid approach                                                                   | Heuristic algorithm                             | Meta-heuristic algorithm                                                                |
|------------------------------------------|-----------------------------------------------------------------------------------|-------------------------------------------------|-----------------------------------------------------------------------------------------|
|                                          | Thangiah et al. (1994)                                                            | -                                               | Genetic algorithm Simulated annealing Tabu search                                       |
|                                          | Sessomboon et al. (1998)                                                          | Route-improving                                 | Genetic algorithm                                                                       |
|                                          | Gehring and Homberger (1999)                                                      | -                                               | Evolution strategy Tabu search                                                          |
|                                          | Tan et al. (2001)                                                                 | -                                               | Genetic algorithm Simulated annealing Tabu search                                       |
|                                          | Braysy ¨ and Dullaert (2003)                                                      | Route-improving                                 | Genetic algorithm Simulated annealing                                                   |
|                                          | Li and Lim (2003)                                                                 | Route-improving                                 | Simulated annealing                                                                     |
|                                          | Bent and Van Hentenryck (2004)                                                    | Route-improving                                 | Simulated annealing                                                                     |
|                                          | Zhao et al. (2004)                                                                | Route-constructing                              | Particle swarm optimization                                                             |
|                                          | Chen and Ting (2005)                                                              | -                                               | Ant colony system Simulated annealing                                                   |
|                                          | Mester and Braysy ¨ (2005) Tan et al. (2006) Xu et al. (2008) Jiang et al. (2009) | Route-improving Route-improving Route-improving | Evolution strategy Evolutionary algorithm Genetic algorithm Particle swarm optimization |
|                                          | Liu et al. (2009)                                                                 | - -                                             | Genetic algorithm Particle swarm optimization Genetic algorithm                         |
|                                          | Wang and Li (2011) Yu et al. (2011)                                               | Route-constructing -                            | Genetic algorithm Ant colony optimization Tabu search                                   |
|                                          | Gong et al. (2012) BanOs ˜ et al. (2013b)                                         | Route-constructing -                            | Particle swarm optimization Evolutionary computation Simulated annealing                |
|                                          | Hu et al. (2013)                                                                  | Route-constructing                              | Particle swarm optimization Chaos algorithm                                             |
|                                          | Barbucha (2014)                                                                   | -                                               | Cooperative search Reactive search                                                      |
|                                          | Hifi and Wu (2014) Luo et al. (2015)                                              | Route-improving Route-improving                 | Ant colony optimization Shuffled frog leaping                                           |
|                                          |                                                                                   |                                                 | algorithm                                                                               |
|                                          | Koc ¸ et al. (2015)                                                               | Route-constructing                              | Genetic algorithm Artificial bee colon                                                  |
|                                          | Zhang et al. (2017)                                                               | -                                               | Tabu search                                                                             |

avoids semi-blind search and uses the experiences of the past generations in order to create new populations. An intelligent evolution is conducted through LEM, leading to the detection of the right directions for evolution, hence making large improvements in the individuals' fitness.

Two versions of LEM are introduced in the literature: the uniLEM and duoLEM. In the uniLEM version, the evolution process is conducted solely through the machine learning mode, while in the duoLEM version two separate modes are applied. Algorithm 1 summarizes the duoLEM.

The LEM is successfully applied in a wide variety of problems in computer science (Farzi 2012; Wojtusiak et al. 2012a, b, Shehab et al. 2013a, b, Moradi 2018a, b) and other related technological fields (Domanski et al. 2004; Jourdan et al. 2005; Michalski and Kaufman 2006; Wojtusiak et al. 2012a, b; Moradi and Mirzaei 2016) and considerably surpasses the conventional EC and SI algorithms. The LEM is able to speed up an evolution process by two or more orders of magnitude compared to Darwinian-type EC algorithms (Michalski 2010).

## 4 Solution representation for the VRPTW

To apply a meta-heuristic algorithm, specifically learnable evolution model, to a real-life problem, the key issue is how to encode a candidate solution into a chromosome.

This solution encoding approach in return affects the success/failure of the LEM for the problem of interest. In the literature on the VRPTW, chromosome representation methods include permutation representation (Labadi et al. 2008), sector representation (Mun ˜oz-Zavala et al. 2009), set representation (Gong et al. 2012) and direct representation (Garcia-Najera and Bullinaria 2011).

In the permutation representation, which is more compatible with our proposed MODLEM for the VRPTW, a chromosome representing a set of vehicle routes is given by an integer string of length n, where n is the number of customers in a particular problem instance. Each gene's allele, the value the gene takes, indicates the number assigned to a customer, while each gene's locus, the position of the gene located within the structure of chromosome, dictates the order that the corresponding customer is served. A chromosome string includes a sequence of routes, but no delimiter is applied to denote the start or end of the routes in a given chromosome. However, a decoding scheme is deployed to transform an arbitrary chromosome into a VRPTW solution, i.e., a set of vehicle routes. The vehicle routes are constructed by sequential customer appending procedure with repeatedly starting from and ending at the central depot. A vehicle must depart from the central depot, and the first gene of a chromosome indicates the first customer that the vehicle serves. The customers are appended one by one to the currently constructed route in the order that they appear on the chromosome. The decoding scheme takes into consideration that the vehicle capacity and the time window constraints are not violated before adding a customer to the current route. A new route is initiated every time that a visited customer cannot be appended to the current route due to constraints violation. This process is continued until each customer is assigned to exactly one route.

In the permutation representation and respective decoding scheme, the current route is ended and a new route is started when appending a customer in the sequence to the current route violates one or more constraints, i.e., the vehicle capacity, the customer time window or the central depot time window. Consequently, a vehicle must return to the central depot as soon as the first constraint is violated, while there may be a quantity of services/goods to serve another customer within corresponding time windows.

We propose a new chromosome representation, motivated by the idea of path representation in Mohemmed et al. (2008), to address the concern raised earlier, i.e., returning vehicles to the central depot due to the first constrain violation.

A fixed-length indirect representation for encoding a VRPTW solution into a chromosome is proposed, where instead of customer numbers, directly appearing on the

chromosome representation, some guiding information about the customers constituting the vehicle routes is used to represent the VRPTW solution. The guiding information is the priority of customers in the problem instance. The locus of a gene is used to represent customer number and its allele is used to indicate the priority of the customer for constructing vehicle routes. This chromosome representation is unique, and one chromosome string can only be decoded to one VRPTW solution.

Along with this chromosome representation, a decoding scheme is employed, responsible for transforming a chromosome string into a set of vehicle routes in a parallel manner. A vehicle departs from the central depot, visits all or some customers and returns to the central depot. At each step of the vehicle routes construction, the customer with highest priority is selected among unserved customers and will be appended to a vehicle route only if the constraints are satisfied. When a candidate customer cannot be appended to the previously constructed routes without violation of any constraints, a new vehicle is employed. A selected customer is visited by the vehicles in the order that they leave the central depot. This process is continued until all customers are assigned to the vehicles. A dynamic customer adjacency matrix is maintained and continuously updated after selection of a customer to be assigned to a vehicle route so that the served customer will not be a candidate for future selection.

Figure 3 shows an illustrative example of the prioritybased chromosome representations. At each step of the encoding scheme, the customer of the highest priority is selected and appended to the current (or a new) vehicle route. The dynamic customer adjacency matrix is updated through deleting the relevant row and column. The solution includes three routes, that is, R1: 0 ? 6 ? 2 ? 8 ? 0, R2: 0 ? 4 ? 1 ? 7 ? 0 and R3: 0 ? 5 ? 3 ? 9 ? 0 of length 23, 11 and 22, respectively. Therefore, the total distance traveled by the used vehicle is 56.

In this chromosome representation, each vehicle returns to the central depot only when the capacity constraint of the vehicle is met or when all unserved customers are visited. This decoding scheme reduces the number of applied vehicles and the total traveled distance remarkably, leading to a fair fitness assignment to chromosomes. Consequently, this appropriate fitness assignment guides the evolution in the right directions and enhances the performance of the search process.

## 5 Multi-objective discrete learnable evolution model for the VRPTW

Figure 4 shows the general trend of the intelligent routing approach based on multi-objective discrete learnable evolution model, the MODLEM, proposed in this work to cope with the VRPTW formulated in Eqs. (1-9). Our proposed evolutionary optimization scheme consists of three major components: (1) search engine, (2) solution evaluator and (3) Pareto-optimal set generator.

The search engine guides evolutionary process through a machine learning algorithm without trial-and-error process and various heuristics incorporated in the DLEM. The Weka machine learning software is applied to run the C4.5 decision tree learning algorithm (J48) to generate the inductive hypotheses. The knowledge base includes manifold heuristics. New heuristic knowledge can be added to the knowledge base.

A chromosome string is transformed to the corresponding vehicle routes through our proposed decoding scheme, and the number of used vehicles and the total traveled distance are calculated in the solution evaluator.

In the Pareto-optimal set generator, first the SPEA2 ? is employed to assign a fitness value to each individual in the population and the archive sets based on calculated objectives, and then, the archive sets are updated.

A visual representation of our proposed MODLEM for the VRPTW is shown in Fig. 5. The steps involved in this proposed approach are discussed in detail where we put emphasis on our new heuristics introduced to improve capability of this proposed approach.

## 5.1 Initialization

The initial population determines where the search process is started at the beginning of an ECA. An efficient procedure to distribute all candidate solutions in the initial population throughout the search space can significantly reduce the MOECAs required computational time to obtain a good approximation of the Pareto fronts. The performance of the MOECAs is highly dependent on this procedure because a failure to provide a distributed initial population may lead the convergence of the population toward the wrong basins of the search space or possibly to the same point. Moreover, starting the search process from totally random locations could lead to a long computational time to find the promising regions. Adding a few highquality individuals to the initial population can improve the convergence of the population.

We apply an effective method to create initial population where both raised concerns are addressed. To ensure the diversity of the initial population, we adopt a dissimilarity criterion to approximate the difference of the chromosomes. The dissimilarity between two chromosomes is measured through Hamming distance, i.e., the number of loci at which the corresponding alleles are different. Two chromosomes are considered to be dissimilar if they have a dissimilarity value more than DSVthr. A new generated

![Image](Papers_Converted/0_Artifacts/[moradi2020]_The_new_optimization_algorithm_for_the_vehicle_routing_problem_with_time_windows_using_multi-objective_discrete_learnable_evolution_model_artifacts/image_000003_4dcc5a3cc4ff19c5c25b5de636e8b843a9bbff06b1d51dd92baf053d00054135.png)

![Image](Papers_Converted/0_Artifacts/[moradi2020]_The_new_optimization_algorithm_for_the_vehicle_routing_problem_with_time_windows_using_multi-objective_discrete_learnable_evolution_model_artifacts/image_000004_511a3bf10e7deea765187e14cd9ce229852c7c85ca846f6d52b3cce159a325b6.png)

Fig. 3 continued

![Image](Papers_Converted/0_Artifacts/[moradi2020]_The_new_optimization_algorithm_for_the_vehicle_routing_problem_with_time_windows_using_multi-objective_discrete_learnable_evolution_model_artifacts/image_000005_05a4e3590179286752c8430ec7d23b77870fb407c5e4c67c3ef855cc8cfa997b.png)

chromosome is inserted to the initial population if it is dissimilar to created subpopulation up to now.

A combined scheme is applied to initialize the population, where a % of the individuals are generated by assigning a random permutation of the priority values, i.e., integer set {1, 2, … , n }, to each chromosome and the remaining ones are generated in a heuristic manner, motivated by the Solomon's push forward insertion heuristic (PFIH; Solomon 1987). In the original Solomon's approach, process starts by selecting from the unserved customers, the one that is farthest from the central depot. Then, the cost is calculated for each unserved customer at each feasible insertion position in the current vehicle route. The customer of the smallest cost is inserted into the corresponding best position. The cost to insert a customer u between customers i and j is defined by Eq. (10). When no more customer insertions without violating any constraint are possible, a new vehicle route is set up. The insertion procedures are repeated until all customers are assigned to the vehicle routes.

/C16

/C17

/C0

/C1

$$C o s t ( i, u, j ) = \alpha _ { 1 } \left ( d _ { i u } + d _ { u j } - \mu d _ { i j } \right ) + \alpha _ { 2 } \left ( t _ { j } ^ { t ^ { \prime } } - t _ { j } ^ { s } \right ) \quad ( 1 0 )$$

where a 1 C 0, a 2 C 0 and l C 0, a 1 þ a 2 ¼ 1. The symbols t s j and t s 0 j denote the service start time at customer j before and after the insertion of customer u , respectively.

The original deterministic PFIH results in constructing only one heuristic VRPTW solution, and it requires expensive computational time. We propose a stochastic PFIH to generate multiple unique high-quality VRPTW solutions in a reasonable computational time. Starting with an empty vehicle route, the farthest unserved customer to the central depot is selected and it is inserted at the beginning of the vehicle route. Unserved customers are selected one by one in a random manner and inserted at the position where the increase in cost is minimal provided that the capacity and the time windows constraints are satisfied.

Fig. 4 General trend of MODLEM

![Image](Papers_Converted/0_Artifacts/[moradi2020]_The_new_optimization_algorithm_for_the_vehicle_routing_problem_with_time_windows_using_multi-objective_discrete_learnable_evolution_model_artifacts/image_000006_30042c85449c7b4ccf7534fec1d2a4909b999b9c6fa667e8bd801090cfac71be.png)

Fig. 5 MODLEM flowchart

![Image](Papers_Converted/0_Artifacts/[moradi2020]_The_new_optimization_algorithm_for_the_vehicle_routing_problem_with_time_windows_using_multi-objective_discrete_learnable_evolution_model_artifacts/image_000007_a0beb8fd38d074a42d09e5227b97175a7b9debe8f34419df3981969bfe02363d.png)

If none of the constraints are met, recently selected customer is returned to the set of the unserved customers and a new vehicle route is constructed. This process is continued until all customers are assigned to the vehicle routes. The set of the routes in each created VRPTW solution is transferred to the corresponding chromosome string through an encoding scheme; that is, priority values from integer set {1, 2, … , n } are assigned to the customers in the order that they are inserted in the vehicle routes.

Two initially empty archive sets are created to allow the individuals of the initial population to become copied in archive sets. The archive sets would maintain Pareto-optimal set in the evolutionary process.

## 5.2 Evaluating the initial population and the archive sets

Fitness values of all individuals in the population and archive sets are calculated through fitness assignment method presented in the SPEA2 ? , proposed by Kim et al. (2004) to eliminate the potential drawbacks of its predecessors (i.e., the SPEA Zitzler and Thiele 1998a, b and the

Fig. 6 A typical hypothesis: a output of the J48 decision tree algorithm; b interpreted hypothesis

![Image](Papers_Converted/0_Artifacts/[moradi2020]_The_new_optimization_algorithm_for_the_vehicle_routing_problem_with_time_windows_using_multi-objective_discrete_learnable_evolution_model_artifacts/image_000008_3dd07a80d12446ebff7fd529ac0d8dd1320238f4a6be93497752500f75008d0c.png)

SPEA2 Zitzler et al. 2001). The SPEA2 ? outperforms its counterparts.

Each individual in the population and archive sets is assigned a strength value representing the number of individuals it dominates. Raw fitness of each individual is calculated by adding the strength values of its dominators in the population and archive sets. In order to distinguish individuals having identical raw fitness, additional density information is applied. The inverse of the distance to the k th nearest neighbor is taken as the density estimate where k is ffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffiffi ffi 3 /C2 Population Size. p It is worth mentioning that fitness is minimized here.

routes. The H-group description (the positive rule set) is updated through incremental specialization mechanism where the priority value ranges described by the positive rules are continuously shrunk leading to small ranges containing the optimal VRPTW solutions.

## 5.3 Generating the hypothesis

The high-performance group (H-group) and low-performance group (L-group) are selected from the archive sets as the positive and negative training samples for a machine learning method. The population-based method is applied where the archive sets are sorted according to the assigned fitness values, and the higher H % of the individuals and the lower L % of the individuals are inserted into the H-group and the L-group, respectively. The middle (100H-L) % of the archive sets is discarded in order to make a distinction between good and bad individuals.

The C4.5 decision tree learning algorithm is applied to create a hypothesis, considering H-group and L-group as the positive and negative training samples, respectively. The generated hypothesis consists of several rules that determine two kinds of priority values ranges: The positive rules determine ranges which may lead to constructing low-cost vehicle routes and the negative rules determine ranges which may lead to constructing high-cost vehicle

Figure 6 indicates the output of the J48 decision tree learning algorithm for a typical VRPTW comprising 10 customers. Initial population is created through allocating values (priorities) from initial interval [1,10] to the chromosomes. The positive and negative training samples are determined and considered as inputs of the learning algorithm. The output of the algorithm, the generated hypothesis in the form of positive and negative rules, is interpreted as condensed intervals. Updated intervals are highlighted in bold.

Note that, in this article, several well-known algorithms are assessed for the learning phase of the LEM. The findings point to the superiority of the C4.5 decision tree learning algorithm versus other classification methods. Consequently, the C4.5 decision tree learning algorithm is applied in our proposed MODLEM to generate the hypothesis. To save space and time, the results related to the comparison of the classification methods are not expressed here.

## 5.4 Instantiating the hypothesis

The H-group description determines the priority values ranges based on the lessons learned from the past populations. Higher priority values are assigned to the customers that visiting them sooner than others is most likely to lead to an optimal VRPTW solution. Thus, they were served sooner than others in the low-cost vehicle routes during the

history of the evolution. Lower priority values are assigned to the customers that inserting them in the vehicle routes sooner than others led to generating the invalid or high-cost vehicle routes in the past generations.

A new population is created through instantiating the hypothesis; that is, a predetermined number of chromosomes are generated in such a way that each of them satisfies at least one rule in the positive rule set. Given a positive rule, instantiation is done by assigning a unique priority value to each customer from its corresponding range.

In the simple instantiation, the customers are selected one by one according to their number, i.e., 1, 2, … , n , and random priority values from corresponding ranges are assigned to them. However, this strategy mostly fails to complete the instantiation because of the conflict raised from the priority values ranges. It is obvious that such wasteful computational attempts decrease the efficiency of the VRPTW solving approach.

We propose a heuristic operator to address the problem of incomplete chromosome formation in the instantiating process where two criteria are considered in the mechanism in which the customers are selected and the priority values are assigned to the customers, i.e., (1) freedom degree : the length of a customer's priority values range and (2) presence degree : the number of priority values ranges of a positive rule in which a priority value presents.

In our proposed heuristic instantiation, first the customers are selected based on their freedom degree. That is, the lesser the freedom degree of a customer, the higher the chance that the customer to be selected. The minimum freedom degree simply addresses the issue of customer selection order in the instantiating process in such a way that the customer with the most limitation is most likely to be selected before others. Then the priority value of minimum presence degree is chosen from the selected customer's corresponding priority values range and assigned to it. One of two or more priority values of equal minimum presence degree is selected in a random manner. This heuristic instantiation significantly reduces the possibility of incomplete chromosome formation and therefore the computational time of our proposed MODLEM for the VRPTW.

## 5.5 Evaluating the population and the archive sets

A fitness value is assigned to each individual in the population and archive sets which would determine the superiority of this individual to others. Evaluation of individual is made through fitness assignment method presented in the SPEA2 ? .

## 5.6 Updating the archive sets

The Pareto-optimal sets should be iteratively revised to survive the fittest individuals and obtain good spread of non-dominated individuals. To accomplish this, two archive sets are maintained and updated through the evolutionary process as follows.

All non-dominated individuals in the population and archive sets are copied into two new archive sets. If the number of individuals of the new archive sets is less than a certain size (i.e., population size), the best dominated individuals in the population and previous archive sets are copied to them. On the contrary, if the number of individuals of the new archive sets exceeds a certain size, an archive truncation procedure is invoked which iteratively removes individuals from the new archive sets until the certain size is obtained. At each iteration, the individual having the minimum Euclidean distance to another individual is chosen for removal. Archive truncation in variable space is applied to one of the archive sets, and an objective space is applied to the other archive set.

## 5.7 Checking the termination conditions

Here, if a termination condition is met, the process ends; otherwise, the process continues to run again. The following criteria are considered as termination conditions: (1) the optimal VRPTW solution (if known) is found, (2) the best fitness value is not improved for certain number of iterations and (3) the predetermined number of iterations is reached.

## 6 Experimental results

Extensive experiments were conducted to verify the performance of our proposed MODLEM in solving the VRPTW. First effectiveness of our proposed heuristics in the MODLEM is investigated. Then we compare the VRPTW solutions obtained through the MODLEM with the best-known records. Finally efficiency of our proposed MODLEM for the VRPTW is compared to the state-of-theart methods.

The proposed approach was implemented in MATLAB 2016a and run on a PC equipped with an Intel Core i5 3.3 GHz processor and 4 GB RAM. In this experimental study, the appropriate parameter settings are found through an off-line tuning method.

## 6.1 Data sets and parameter tuning

Our proposed MODLEM is tested on a standard benchmark involving 56 problem instances provided by Solomon (1987) reflecting various real-life scheduling circumstances. These problem instances are split into six different classes, containing customers clustered around the central depot having small time windows (C1) or having large time windows (C2), distributed randomly on the map having small time windows (R1) or having large time windows (R2), and distributed both random and clustered having small time windows (RC1) or having large time windows (RC2). In each Solomon's problem instance, there are 100 customers to serve. Recent analyses show that classes C1 and C2 have positively correlating objectives; that is, the TD of a solution increases with the NV, whereas many of the problem instances in classes R1, R2, RC1 and RC2 were found to have conflicting objectives.

Two classes of parameters should be set in our proposed MODLEM for the VRPTW. We apply the WEKA machine learning software to run the decision tree C4.5 algorithm (J48) with the default value of the parameters to generate the inductive hypothesis. The tenfold cross-validation is used as evaluation approach. Moreover, the values of 0.3 and 0.7 are assigned to a 1 and a 2 in Eq. (10), respectively, in order to emphasize time insertion.

Other parameters, tabulated in Table 2, are tuned through an off-line manner where initially the best suggested values in the literature are assigned to the parameters, and then, several experiments are governed to further fine-tune these parameters on six problem instances of the Solomon's data set.

To analysis the effect of the parameters in our proposed approach, we tested various values as tabulated in Table 2. For each parameter, the performance of the MODLEM variants is calculated, averaged over the six chosen problem instances. The value leading to highest performance is assigned to the corresponding parameter. The best settings obtained through this parameter tuning process are tabulated in the last column in Table 2.

## 6.2 Contributions of the heuristic operators in the MODLEM

In the MODLEM, we applied some new heuristic operators to address the relevant issues. This experiment is provided to analyze how these heuristic operators contribute to our proposed intelligent approach performance. To validate the impact of each heuristic operator, the following three variants are considered:

- -Variant-I: the MODLEM without the heuristic initial population generator
- -Variant-II: the MODLEM without the heuristic instantiation operator
- -Variant-III: the MODLEM without any of the heuristic operators

In Variant-I, the initial population is created in a totally random manner. The simple instantiation is adopted in Variant-II where new population of individuals is generated through random instantiation of the hypothesis. Variant-III includes none of our proposed heuristic operators.

In order to assess the effectiveness of the heuristic operators incorporated in our proposed MODLEM, two complementary quality metrics are used, that is, the hypervolume (HV) (Zitzler and Thiele 1998a, b) and the set coverage (SC) (Zitzler and Thiele 1999).

The HV measures the volume covered by solutions of a non-dominated set in the objective space. The HV includes three criteria: convergence, distribution and extent of the obtained Pareto-optimal set (Zitzler et al. 2000). In a multiobjective optimization problem with d objective functions to minimize, the size of the region of the objective space of a non-dominated set of solutions A = { a 1 , a 2 , … , an } bounded by a reference point r = { r 1 , r 2 , … , rd } is calculated. The corresponding hypercube for each element ai of the set A is calculated as follows: h (i) = [ ai 1 , r 1 ] 9 [ ai 2 , r 2 ] 9 … [ aid , rd ]. Finally, the hypervolume of A is determined by the union of these | A | hypercubes as shown in Eq. (11).

## Table 2 MODLEM configuration parameters

| Parameter          | Initial value   | Interval of values   |   Step size | Best value   |
|--------------------|-----------------|----------------------|-------------|--------------|
| Population size    | 100             | [50, 250]            |        50   | 150          |
| No. of generations | 100             | [50, 250]            |        50   | 200          |
| H-group size       | 30%             | [20, 40] %           |         5   | 40%          |
| L-Group size       | 30%             | [20, 40] %           |         5   | 40%          |
| DSV thr            | 0.3 n *         | [0.1, 0.5] n*        |         0.1 | 0.3 n*       |
| a                  | 20%             | [5, 25]              |         5   | 15%          |

- n* determines the number of customers

Table 3 Average of median and interquartile range of the hypervolume

| Instances   | MODLEM      | Variant-I   | Variant-II   | Variant-III   | SSD   |
|-------------|-------------|-------------|--------------|---------------|-------|
| C1          | 0.723 0.001 | 0.628 0.012 | 0.523 0.007  | 0.392 0.012   | OK    |
| C2          | 0.801 0.000 | 0.713 0.008 | 0.581 0.004  | 0.414 0.009   | OK    |
| R1          | 0.891 0.002 | 0.801 0.014 | 0.761 0.009  | 0.655 0.015   | OK    |
| R2          | 0.870 0.001 | 0.841 0.013 | 0.799 0.008  | 0.611 0.015   | OK    |
| RC1         | 0.852 0.002 | 0.812 0.011 | 0.787 0.009  | 0.652 0.013   | OK    |
| RC2         | 0.898 0.001 | 0.862 0.010 | 0.801 0.009  | 0.703 0.013   | OK    |
| Avg.        | 0.839 0.001 | 0.776 0.011 | 0.708 0.007  | 0.571 0.012   |       |

Table 4 Set coverage results (MODLEM, Variant-I)

| Instances   |   SC [Variant-I, MODLEM] |   SC [MODLEM, Variant-I] |
|-------------|--------------------------|--------------------------|
| C1          |                    0.378 |                    0.709 |
| C2          |                    0.311 |                    0.714 |
| R1          |                    0.298 |                    0.767 |
| R2          |                    0.301 |                    0.752 |
| RC1         |                    0.324 |                    0.722 |
| RC2         |                    0.295 |                    0.758 |
| Avg.        |                    0.302 |                    0.737 |

Table 5 Set coverage results (MODLEM, Variant-II)

| Instances   |   SC [Variant-II, MODLEM] |   SC [MODLEM, Variant-II] |
|-------------|---------------------------|---------------------------|
| C1          |                     0.173 |                     0.79  |
| C2          |                     0.155 |                     0.811 |
| R1          |                     0.143 |                     0.826 |
| R2          |                     0.141 |                     0.807 |
| RC1         |                     0.152 |                     0.64  |
| RC2         |                     0.138 |                     0.711 |
| Avg.        |                     0.15  |                     0.764 |

!

$$H V ( A, r ) & = L \left ( \bigcup _ { i = 1 } ^ { | A | } h ( a _ { i } ) | a _ { i } \in A \right ) & ( 1 1 ) & \quad \text{$main$} \quad \text{define} \\ & \quad \text{values}$$

where L refers to the Lebesgue measure.The SC indicates the fraction of non-dominated solutions in a Pareto-

Optimal set covered by the non-dominated solutions in another one. The SC can be applied in determining whether the outcomes of an approach dominate those of another one or not (Zitzler et al. 2000). The fraction of non-dominated solutions in a Pareto-optimal set B = { b 1 , b 2 , … , bm }, which are covered by the non-dominated solutions in A = { a 1 , a 2 , … , an }, is calculated through Eq. (12).

$$\text{OK} \quad \text{SC} ( A, B ) = \frac { | \{ b \epsilon B ; \exists a \epsilon A \, \colon a \preceq b \} | } { | B | }$$

where the symbol ^ is the dominance operator.

In this experiment, identical parameter settings are used for the MODLEM and its three variants (See Sect. 6.1). All four considered MODLEM variants are run 31 times for each Solomon's problem instance for a certain CPU time independently. The reference points are formed by the worst and the best values of the considered objectives of all found non-dominated VRPTW solutions for corresponding problem instance. The ideal and nadir points for each problem instance are calculated by adding an offset equal to -25% and ? 25% to the minimum and the maximum values of the obtained NV and TD, respectively. Furthermore, the statistically significant differences (SSD) of the obtained results are determined through the statistical analysis. In principle, a statistically significant result refers to a result that is not attributed to chance. It is noted that, in the experiments here, the confidence level in the statistical test is 95% ( p value less than 0.05) which means the differences are unlikely to have happened by chance with a probability of 95%. For a detailed explanation of the tests, refer to Sheskin (2011).

The median hypervolume and the interquartile range of the hypervolume values, averaged for each class of the Solomon's problem instances, for our proposed MODLEM and its three variants are tabulated in Table 3, where the first column refers to the class of the Solomon's problem instances and the other columns indicate the results obtained through the corresponding approaches for the different classes of the Solomon's problem instances. The values corresponding with these columns have the format XY , where the X value refers to the averaged median

Table 6 Set coverage results (MODLEM, Variant-III)

| Instances   |   SC [Variant-III, MODLEM] |   SC [MODLEM, Variant-III] |
|-------------|----------------------------|----------------------------|
| C1          |                      0.059 |                      0.898 |
| C2          |                      0.059 |                      0.902 |
| R1          |                      0.055 |                      0.941 |
| R2          |                      0.056 |                      0.923 |
| RC1         |                      0.058 |                      0.912 |
| RC2         |                      0.051 |                      0.989 |
| Avg.        |                      0.056 |                      0.912 |

Table 7 Comparison between the best-known results and the best results obtained through the MODLEM for C1, R1 and RC1

| Approaches   | Best-known   | Best-known   | Best-known                      | MODLEM   | MODLEM   | MODLEM   | MODLEM   |
|--------------|--------------|--------------|---------------------------------|----------|----------|----------|----------|
| Instances    | NV           | TD           | References                      | NV       | TD       | %NV      | %TD      |
| C101         | 10           | 827.30       | Desrochers et al. (1992)        | 10       | 828.94   | ? 0.00   | ? 0.20   |
| C102         | 10           | 827.30       | Desrochers et al. (1992)        | 10       | 828.94   | ? 0.00   | ? 0.20   |
| C103         | 10           | 826.30       | Tavares et al. (2003)           | 10       | 828.07   | ? 0.00   | ? 0.21   |
| C104         | 10           | 822.90       | Tavares et al. (2003)           | 10       | 824.78   | ? 0.00   | ? 0.23   |
| C105         | 10           | 827.30       | Tavares et al. (2003)           | 10       | 828.94   | ? 0.00   | ? 0.20   |
| C106         | 10           | 827.30       | Desrochers et al. (1992)        | 10       | 828.94   | ? 0.00   | ? 0.20   |
| C107         | 10           | 827.30       | Tavares et al. (2003)           | 10       | 828.94   | ? 0.00   | ? 0.20   |
| C108         | 10           | 827.30       | Tavares et al. (2003)           | 10       | 828.94   | ? 0.00   | ? 0.20   |
| C109         | 10           | 827.30       | Tavares et al. (2003)           | 10       | 828.94   | ? 0.00   | ? 0.20   |
| Avg.         |              |              |                                 |          |          | ? 0.000  | ? 0.204  |
| R101         | 18           | 1607.70      | Desrochers et al. (1992)        | 19       | 1650.80  | ? 5.56   | ? 2.68   |
| R102         | 17           | 1434.00      | Desrochers et al. (1992)        | 17       | 1486.86  | ? 0.00   | ? 3.67   |
| R103         | 13           | 1175.67      | Lau et al. (2001)               | 13       | 1292.68  | ? 0.00   | ? 9.95   |
| R104         | 10           | 974.20       | Tan et al. (2006)               | 9        | 1007.31  | - 10.00  | ? 3.40   |
|              | 9            | 1007.24      | Mester (2002)                   |          |          | ? 0.00   | ? 0.01   |
| R105         | 15           | 1346.12      | Kallehauge et al. (2006)        | 14       | 1377.11  | - 6.67   | ? 0.95   |
|              | 14           | 1377.11      | Rochat and Taillard (1995)      |          |          | ? 0.00   | ? 0.00   |
| R106         | 13           | 1234.6       | Cook and Rich (1999)            | 12       | 1252.03  | - 7.69   | ? 1.41   |
|              | 12           | 1251.98      | Mester (2002)                   |          |          | ? 0.00   | ? 0.00   |
| R107         | 11           | 1051.84      | Kallehauge et al. (2006)        | 10       | 1104.66  | - 9.09   | ? 5.02   |
|              | 10           | 1104.66      | Shaw (1997)                     |          |          | ? 0.00   | ? 0.00   |
| R108         | 10           | 947.55       | Jung and Moon (2002)            | 9        | 960.88   | - 10.00  | ? 1.41   |
|              | 9            | 960.88       | Berger et al. (2003)            |          |          | ? 0.00   | ? 0.00   |
| R109         | 12           | 1013.20      | Chiang and Russell (1997)       | 11       | 1194.73  | - 8.33   | ? 17.92  |
|              | 11           | 1194.73      | Homberger and Gehring (1999)    |          |          | ? 0.00   | ? 0.00   |
| R110         | 12           | 1068.00      | Cook and Rich (1999)            | 10       | 1118.84  | - 16.67  | ? 4.76   |
|              | 10           | 1118.59      | Mester (2002)                   |          |          | ? 0.00   | ? 0.02   |
| R111         | 12           | 1048.70      | Cook and Rich (1999)            | 10       | 1096.73  | - 16.67  | ? 4.58   |
|              | 10           | 1096.72      | Rousseau et al. (2002)          |          |          | ? 0.00   | ? 0.00   |
| R112         | 10           | 953.63       | Rochat and Taillard (1995)      | 9        | 982.14   | - 10.00  | ? 2.99   |
|              | 9            | 982.14       | Gambardella et al. (1999)       |          |          | ? 0.00   | ? 0.00   |
| Avg.         |              |              |                                 |          |          | - 4.693  | ? 2.799  |
| RC101        | 15           | 1619.80      | Kohl et al. (1999)              | 14       | 1696.95  | - 6.67   | ? 4.76   |
|              | 14           | 1696.94      | Taillard et al. (1997)          |          |          | ? 0.00   | ? 0.00   |
| RC102        | 13           | 1470.26      | Tan et al. (2006)               | 12       | 1554.75  | - 7.69   | ? 5.75   |
|              | 14           | 1461.23      | Jung and Moon (2002)            |          |          | - 14.29  | ? 4.60   |
|              | 12           | 1554.75      | Taillard et al. (1997)          |          |          | ? 0.00   | ? 0.00   |
| RC103        | 12           | 1196.12      | Tan et al. (2006)               | 11       | 1261.67  | - 8.33   | ? 5.48   |
|              | 11           | 1261.67      | Shaw (1997)                     |          |          | ? 0.00   | ? 0.00   |
| RC104        | 10           | 1135.48      | Cordeau et al. (2001)           | 10       | 1135.48  | ? 0.00   | ? 0.00   |
| RC105        | 14           | 1542.29      | Barbucha (2014)                 | 13       | 1629.44  | - 7.14   | ? 5.65   |
|              | 16           | 1518.58      | Jung and Moon (2002)            |          |          | - 18.75  | ? 7.30   |
|              | 13           | 1629.44      | Berger et al. (2003)            |          |          | ? 0.00   | ? 0.00   |
| RC106        | 13           | 1371.69      | Tan et al. (2006)               | 11       | 1424.73  | - 15.39  | ? 3.87   |
|              | 11           | 1424.73      | Berger et al. (2003)            |          |          | ? 0.00   | ? 0.00   |
| RC107        | 11           | 1222.10      | Ghoseiri and Ghannadpour (2010) | 11       |          | ? 0.00   | ? 0.69   |
|              | 12           | 1212.83      |                                 |          |          | - 8.33   | ?        |
|              |              |              | Jung and Moon (2002)            |          | 1230.48  |          | 1.46     |

Table 7 (continued)

| Approaches   | Best-known   | Best-known   | Best-known             | MODLEM   | MODLEM   | MODLEM   | MODLEM   |
|--------------|--------------|--------------|------------------------|----------|----------|----------|----------|
| Instances    | NV           | TD           | References             | NV       | TD       | %NV      | %TD      |
| RC108        | 11           | 1117.53      | Jung and Moon (2002)   | 10       | 1139.82  | - 9.09   | ? 1.20   |
|              | 10           | 1139.82      | Taillard et al. (1997) |          |          | ? 0.00   | ? 0.00   |
| Avg.         |              |              |                        |          |          | - 5.628  | ? 2.398  |

hypervolume and Y to the averaged interquartile range of hypervolume.

## 6.3 Comparison with the best-known solutions

The experimental results, tabulated in Tables 3, show the effect of applying our proposed heuristic operators on the MODLEM performance in terms of the HV. These hypervolume values indicate that our proposed MODLEM covers a greater portion of the solution space compared to the variants.

Regarding the associated interquartile range values, the results obtained through our proposed MODLEM are always better than other variants. These interquartile range values clearly confirm that the results obtained by applying our proposed MODLEM are generally less sparse than when other variants are run. Accordingly, we can assert that our proposed MODLEM is generally more stable and reliable than other variants in terms of the results.

The results of the statistical analysis, in the sixth column in Table 3, indicate that the results obtained through our proposed MODLEM and other variants are significantly different in all problem instances.

In order to make a more reliable assertion about the best alternative to solve the VRPTW, we also used the set coverage metric. The average of the SC results is tabulated in Tables 4, 5 and 6 for each class of the Solomon's problem instances. These set coverage results indicate that non-dominated solutions in the Pareto-optimal set obtained through our proposed MODLEM dominate many nondominated solutions found through any of other variants, but not vice versa. The set coverage results confirm that our proposed MODLEM is a better alternative for solving the VRPTW.

The reason for such better median HV and SC values is the vital role of the heuristic operators in enhancing the performance of the MODLEM. On the one hand, a distributed population including a few high-quality individuals is created through the first heuristic operator which leads to obtaining a more convergent, distributed and extensive Pareto-optimal set. On the other hand, unfruitful attempts in the heuristic instantiating process are reduced by applying the second heuristic operator, and accordingly, more computational time is spent on the evolution.

In order to assess the performance of our proposed MODLEM, we compare our obtained solutions with the best-known ones, tabulated in Tables 7 and 8. In Tables 7 and 8, the first column shows the Solomon's problem instances. The number of used vehicles, the total traveled distance of each best-known solution and the authors of the corresponding best-known solutions are listed at the second, the third and the fourth columns of these tables, respectively. The NV and the TD of the best solution obtained through our proposed MODLEM within 31 independent runs for each problem instances are tabulated at the fifth and sixth columns in Tables 7 and 8. The total traveled distance is computed with double precision and rounded off to two decimals. %NV and %TD are the percentage difference for our proposed MODLEM in the number of used vehicles and the total traveled distance with the best solutions identified till now through approaches proposed by other authors for solving the VRPTW.

Ninety-seven best-known solutions are tabulated in Tables 7 and 8; that is, more than one best-known solution is reported for some problem instances. Tables 7 and 8 show that our proposed intelligent routing approach obtains the best-known solutions in 22 cases. The results of our proposed MODLEM which are equal to the best-known ones for each problem instance are emphasized in boldface. In 46 cases, our proposed MODLEM achieves new nondominated solutions. Our proposed MODLEM is also capable finding solutions similar to the best-known ones in other cases.

The average of the percentage difference of NV and TD on different classes of the Solomon's problem instances are summarized in Table 9. The average of %NV on both C1 and C2 problem instances is 0 on the best-known solutions. On R1, RC1 and RC2 problem instances, this is less than zero that means an improvement compared to the bestknown solutions. Slightly worse results are observed on problem instances belonging to class R2 in comparison with the best-known solutions in terms of the average of %NV. Moreover, the average of %TD on all problem instances is no more than 2.8 on the best-known solutions.

Table 8 Comparison between the best-known results and the best results obtained through the MODLEM for C2, R2 and RC2

| Approaches   | Best-known   | Best-known   | Best-known                     | MODLEM   | MODLEM   | MODLEM   | MODLEM   |
|--------------|--------------|--------------|--------------------------------|----------|----------|----------|----------|
| Instances    | NV           | TD           | References                     | NV       | TD       | %NV      | %TD      |
| C201         | 3            | 589.10       | Cook and Rich (1999)           | 3        | 591.56   | ? 0.00   | ? 0.42   |
| C202         | 3            | 589.10       | Cook and Rich (1999)           | 3        | 591.56   | ? 0.00   | ? 0.42   |
| C203         | 3            | 591.17       | Li and Lim (2003)              | 3        | 591.17   | ? 0.00   | ? 0.00   |
| C204         | 3            | 590.60       | Potvin and Bengio (1996)       | 3        | 590.60   | ? 0.00   | ? 0.00   |
| C205         | 3            | 588.88       | Potvin and Bengio (1996)       | 3        | 588.88   | ? 0.00   | ? 0.00   |
| C206         | 3            | 588.49       | Lau et al. (2003)              | 3        | 588.49   | ? 0.00   | ? 0.00   |
| C207         | 3            | 588.29       | Rochat and Taillard (1995)     | 3        | 588.29   | ? 0.00   | ? 0.00   |
| C208         | 3            | 588.03       | Tan et al. (2006)              | 3        | 588.32   | ? 0.00   | ? 0.05   |
| Avg.         |              |              |                                |          |          | ? 0.000  | ? 0.111  |
| R201         | 8            | 1198.15      | Tan et al. (2001)              | 4        | 1252.37  | - 50.00  | ? 4.53   |
|              | 9            | 1144.48      | Alvarenga et al. (2007)        |          |          | - 55.56  | ? 9.43   |
|              | 4            | 1252.37      | Homberger and Gehring (1999)   |          |          | ? 0.00   | ? 0.00   |
| R202         | 6            | 1077.66      | Tan et al. (2001)              | 3        | 1191.70  | - 50.00  | ? 10.58  |
|              | 8            | 1034.35      | Jung and Moon (2002)           |          |          | - 62.50  | ? 15.21  |
|              | 3            | 1191.70      | Rousseau et al. (2002)         |          |          | ? 0.00   | ? 0.00   |
| R203         | 5            | 933.29       | Tan et al. (2001)              | 3        | 939.50   | - 40.00  | ? 0.67   |
|              | 6            | 874.87       | Jung and Moon (2002)           |          |          | - 50.00  | ? 7.39   |
|              | 3            | 939.54       | Mester (2002)                  |          |          | ? 0.00   | ? 0.00   |
| R204         | 3            | 752.13       | Barbucha (2014)                | 5        | 731.30   | ? 40.00  | - 2.77   |
|              | 4            | 736.52       | Jung and Moon (2002)           |          |          | ? 25.00  | - 0.71   |
|              | 2            | 825.52       | Bent and Van Hentenryck (2004) |          |          | ? 150.0  | - 11.41  |
| R205         | 5            | 954.16       | Ombuki et al. (2006)           | 7        | 965.10   | ? 40.00  | ? 1.15   |
| R206         | 3            | 833.00       | Thangiah et al. (1994)         | 5        | 887.60   | ? 66.67  | ? 6.56   |
| R207         | 3            | 814.78       | Rochat and Taillard (1995)     | 5        | 807.00   | ? 66.67  | - 0.96   |
|              | 4            | 799.86       | Jung and Moon (2002)           |          |          | ? 25.00  | ? 0.89   |
| R208         | 2            | 726.75       | Mester (2002)                  | 4        | 703.40   | ? 100.00 | - 3.21   |
|              | 4            | 705.45       | Jung and Moon (2002)           |          |          | ? 0.00   | - 0.29   |
| R209         | 3            | 855.00       | Thangiah et al. (1994)         | 3        | 909.16   | ? 0.00   | ? 6.33   |
| R210         | 5            | 910.70       | Jung and Moon (2002)           | 6        | 944.70   | ? 20.00  | ? 3.73   |
| R211         | 4            | 755.96       | Jung and Moon (2002)           | 5        | 754.60   | ? 25.00  | - 0.18   |
| Avg.         |              |              |                                |          |          | ? 11.918 | ? 2.266  |
| RC201        | 6            | 1134.91      | Tan et al. (2006)              | 4        | 1406.9   | - 33.33  | ? 23.97  |
|              | 4            | 1406.91      | Mester (2002)                  |          | 4        | ? 0.00   | ? 0.00   |
| RC202        | 5            | 1130.53      | Tan et al. (2006)              | 3        | 1365.65  | - 40.00  | ? 20.78  |
|              | 3            | 1365.65      | Repoussis et al. (2009)        |          |          | ? 0.00   | ? 0.00   |
|              | 8            | 1095.64      | Jung and Moon (2002)           |          |          | - 62.5   | ? 24.64  |
| RC203        | 4            | 1026.61      | Tan et al. (2006)              | 3        | 1049.62  | - 25.00  | ? 2.24   |
|              | 5            | 928.51       | Czech and Czarnas (2002)       |          |          | - 40.00  | ? 13.04  |
|              | 3            | 1049.62      | Jung and Moon (2002)           |          |          | ? 0.00   | ? 0.00   |
| RC204        | 3            | 798.41       | Mester (2002)                  | 4        | 788.30   | ? 33.33  | - 1.27   |
|              | 4            | 786.38       | Jung and Moon (2002)           |          |          | ? 0.00   | ? 0.24   |
| RC205        | 5            | 1295.46      | Tan et al. (2006)              | 4        | 1297.65  | - 20.00  | ? 0.17   |
|              | 7            | 1157.55      | Jung and Moon (2002)           |          |          | - 42.86  | ? 12.10  |
|              | 4            | 1297.19      | Mester (2002)                  |          |          | ? 0.00   | ? 0.04   |
| RC206        | 4            | 1112.20      | Yu et al. (2011)               | 3        | 1146.32  | - 25.00  | ? 3.07   |
|              | 7            | 1054.61      | Jung and Moon (2002)           |          |          | - 57.14  | ? 8.69   |
|              | 3            | 1146.32      | Homberger and Gehring (1999)   |          |          | ? 0.00   | ? 0.00   |

Table 8 (continued)

| Approaches   | Best-known   | Best-known   | Best-known                     | MODLEM   | MODLEM   | MODLEM   | MODLEM   |
|--------------|--------------|--------------|--------------------------------|----------|----------|----------|----------|
| Instances    | NV           | TD           | References                     | NV       | TD       | %NV      | %TD      |
| RC207        | 4            | 1040.67      | Tan et al. (2006)              | 6        | 763.30   | ? 50.00  | - 26.65  |
|              | 6            | 966.08       | Jung and Moon (2002)           |          |          | ? 0.00   | - 20.99  |
|              | 3            | 1061.14      | Bent and Van Hentenryck (2004) |          |          | ? 100.00 | - 28.07  |
| RC208        | 3            | 828.14       | Ibaraki et al. (2005)          | 5        | 779.60   | ? 66.67  | - 5.86   |
|              | 4            | 799.31       | Jung and Moon (2002)           |          |          | ? 25.00  | - 2.47   |
| Avg.         |              |              |                                |          |          | - 3.373  | ? 1.127  |

Table 9 Average of percentage difference of NV and TD for different classes

Taking into account both the average of %NV and the average of %TD, the performance of our proposed MODLEM on all six classes of Solomon's problem instances, except for class R2, is comparable to the approaches obtaining the best-known solutions in the literature. On C1, C2, R1, RC1 and RC2, for which the average of %TD is a positive value, the respective average of %NV is a zero or negative value.

## 6.4 Comparison with the state of the art

| Instances   | %NV      | %TD     |
|-------------|----------|---------|
| C1          | ? 0.000  | ? 0.204 |
| C2          | ? 0.000  | ? 0.111 |
| R1          | - 4.693  | ? 2.799 |
| R2          | ? 11.918 | ? 2.266 |
| RC1         | - 5.628  | ? 2.398 |
| RC2         | - 3.373  | ? 1.127 |
| Avg.        | - 0.296  | ? 1.484 |

Accordingly, most of the solutions obtained through our proposed MODLEM on the Solomon's problem instances are compromising compared with the best-known ones.

We compare the performance of this newly proposed approach, for the VRPTW, with four other impressive approaches, that is, the Tabu-ABC proposed by Zhang

Table 10 Comparison among results obtained through the MODLEM and the state-of-the-art approaches for C1 and C2

| Approaches   | MODLEM   | MODLEM   | MODLEM   | MODLEM   | Tabu-ABC   | Tabu-ABC   | Tabu-ABC   | M-MOEA/D   | M-MOEA/D   | HSFLA   | HSFLA   | ESS-PSO   | ESS-PSO   |
|--------------|----------|----------|----------|----------|------------|------------|------------|------------|------------|---------|---------|-----------|-----------|
| Instances    | NV       | TD       | FE       | Time     | NV         | TD         | Time       | NV         | TD         | NV      | TD      | NV        | TD        |
| C101         | 10       | 828.94   | 12,204   | 113.05   | 10         | 828.94     | 1070.42    | 10         | 828.94     | 10      | 828.94  | 10        | 828.94    |
| C102         | 10       | 828.94   | 12,312   | 114.38   | 10         | 828.94     | 280.49     | 10         | 828.94     | 10      | 828.94  | 10        | 828.94    |
| C103         | 10       | 828.07   | 12,636   | 117.04   | 10         | 824.07     | 116.66     | 10         | 828.06     | 10      | 824.06  | 10        | 828.06    |
| C104         | 10       | 824.78   | 11,880   | 110.39   | 10         | 824.78     | 60.94      | 10         | 824.78     | 10      | 824.78  | 10        | 824.78    |
| C105         | 10       | 828.94   | 11,988   | 111.72   | 10         | 828.94     | 478.01     | 10         | 828.94     | 10      | 828.94  | 10        | 828.94    |
| C106         | 10       | 828.94   | 13,068   | 121.03   | 10         | 828.94     | 389.41     | 10         | 828.94     | 10      | 828.94  | 10        | 828.94    |
| C107         | 10       | 828.94   | 11,448   | 106.4    | 10         | 828.94     | 322.39     | 10         | 828.94     | 10      | 828.94  | 10        | 828.94    |
| C108         | 10       | 828.94   | 10,908   | 101.08   | 10         | 828.94     | 185.52     | 10         | 828.94     | 10      | 828.94  | 10        | 828.94    |
| C109         | 10       | 828.94   | 11,124   | 103.74   | 10         | 828.94     | 83.06      | 10         | 828.94     | 10      | 828.94  | 10        | 828.94    |
| Avg.         | 10       | 828.38   | 11,952   | 110.39   | 10         | 827.94     | 331.88     | 10         | 828.38     | 10      | 827.38  | 10        | 828.38    |
| C201         | 3        | 591.56   | 18,792   | 174.75   | 3          | 591.56     | 1337.71    | 3          | 591.56     | 3       | 591.56  | 3         | 591.56    |
| C202         | 3        | 591.56   | 21,600   | 200.38   | 3          | 591.56     | 348.48     | 3          | 591.56     | 3       | 591.56  | 3         | 591.56    |
| C203         | 3        | 591.17   | 19,872   | 184.07   | 3          | 591.17     | 132.09     | 3          | 591.17     | 3       | 591.17  | 3         | 591.17    |
| C204         | 3        | 590.60   | 20,628   | 191.06   | 3          | 594.89     | 53.45      | 3          | 590.60     | 3       | 590.60  | 3         | 590.60    |
| C205         | 3        | 588.88   | 19,872   | 184.07   | 3          | 588.88     | 619.37     | 3          | 588.88     | 3       | 588.88  | 3         | 588.88    |
| C206         | 3        | 588.49   | 16,308   | 151.45   | 3          | 588.49     | 473.96     | 3          | 588.49     | 3       | 588.49  | 3         | 588.49    |
| C207         | 3        | 588.29   | 21,060   | 195.72   | 3          | 588.29     | 322.58     | 3          | 588.29     | 3       | 588.29  | 3         | 588.29    |
| C208         | 3        | 588.32   | 23,112   | 214.36   | 3          | 588.32     | 234.39     | 3          | 588.32     | 3       | 588.32  | 3         | 588.32    |
| Avg.         | 3        | 589.86   | 20,155   | 186.40   | 3          | 590.40     | 440.25     | 3          | 589.86     | 3       | 589.86  | 3         | 589.86    |

et al. (2017), the M-MOEA/D proposed by Qi et al. (2015), the HSFLA proposed by Luo et al. (2015) and the ESSPSO proposed by Zhang et al. (2018) in terms of both the quality of solutions and the computational time. The best results obtained for each instance are highlighted in bold.

The comparison results between our proposed MODLEM and other approaches on C, R and RC problem instances are presented in Tables 10, 11 and 12, respectively. The results consist of the number used vehicles and the total traveled distance over all 56 problem instances. Moreover, the average CPU time required to run our proposed MODLEM and the Tabu-ABC approach to obtain best solutions are included in the tables. The results of the state-of-the-art approaches are provided from the corresponding literatures. The non-dominated solutions obtained by applying our proposed MODLEM within 31 independent runs for each problem instance, and corresponding average computational times and total number of function evaluations (FE) are reported based on our experiment.

Table 10 shows that our proposed MOLEM performed as well as the M-MOEA/D, the HSFLA, the Tabu-ABC and the ESS-PSO approaches on both classes C1 and C2. That is to say, all approaches obtained solutions with the same number of used vehicles and total traveled distance on almost all problem instances in which all customers are located in geographical locations close together. The only exceptions are C103 where HSFLA succeeded in finding a better solution and C204 where Tabu-ABC performed slightly worse than others approaches.

Table 11 shows that our proposed MODLEM and the M-MOEA/D approach yielded solutions of the same quality on R102. The quality of solutions obtained through our proposed MODLEM is better than that of solutions found through the M-MOEA/D approach on 9 problem instances. Our proposed MODLEM achieved alternative non-dominated solutions on 13 problem instances. Furthermore, our proposed MODLEM and the HSFLA approach found solutions having the same total traveled

Table 11 Comparison among results obtained through the MODLEM and the state-of-the-art approaches for R1 and R2

| Approaches Instances   | MODLEM   | MODLEM   | MODLEM   | MODLEM   | Tabu-ABC   | Tabu-ABC   | Tabu-ABC   | M-MOEA/D   | M-MOEA/D   | HSFLA   | HSFLA   | ESS-PSO   | ESS-PSO   |
|------------------------|----------|----------|----------|----------|------------|------------|------------|------------|------------|---------|---------|-----------|-----------|
| Approaches Instances   | NV       | TD       | FE       | Time     | NV         | TD         | Time       | NV         | TD         | NV      | TD      | NV        | TD        |
| R101                   | 19       | 1650.80  | 13,824   | 128.75   | 20         | 1643.18    | 521.88     | 19         | 1652.17    | 19      | 1650.80 | 20        | 1642.88   |
| R102                   | 17       | 1486.86  | 14,472   | 134.93   | 18         | 1460.26    | 187.46     | 17         | 1486.12    | 17      | 1486.12 | 18        | 1472.92   |
| R103                   | 13       | 1292.68  | 12,204   | 113.30   | 15         | 1217.39    | 141.13     | 13         | 1354.22    | 13      | 1292.67 | 14        | 1213.73   |
| R104                   | 9        | 1007.31  | 11,556   | 107.12   | 11         | 987.61     | 85.29      | 10         | 999.31     | 9       | 1007.30 | 11        | 976.61    |
| R105                   | 14       | 1377.11  | 11,988   | 111.24   | 15         | 1363.91    | 235.14     | 14         | 1414.64    | 14      | 1377.11 | 15        | 1360.76   |
| R106                   | 12       | 1252.03  | 12,960   | 120.51   | 13         | 1247.90    | 174.85     | 12         | 1265.99    | 12      | 1252.03 | 13        | 1239.37   |
| R107                   | 10       | 1104.66  | 15,120   | 140.08   | 12         | 1087.50    | 130.82     | 10         | 1139.47    | 10      | 1104.66 | 11        | 1073.34   |
| R108                   | 9        | 960.88   | 11,340   | 105.06   | 11         | 961.85     | 87.48      | 10         | 965.52     | 9       | 960.88  | 10        | 950.59    |
| R109                   | 11       | 1194.73  | 12,420   | 115.36   | 13         | 1152.99    | 162.67     | 12         | 1157.44    | 11      | 1194.73 | 13        | 1151.84   |
| R110                   | 10       | 1118.84  | 14,148   | 131.84   | 12         | 1091.50    | 154.76     | 11         | 1110.68    | 10      | 1118.84 | 12        | 1073.46   |
| R111                   | 10       | 1096.73  | 12,096   | 112.27   | 12         | 1067.46    | 131.07     | 11         | 1073.82    | 10      | 1096.73 | 12        | 1053.50   |
| R112                   | 9        | 982.14   | 11,664   | 108.15   | 10         | 973.25     | 118.97     | 10         | 981.43     | 9       | 982.14  | 10        | 953.62    |
| Avg.                   | 11.9     | 1210.40  | 12,816   | 119.48   | 13.5       | 1187.90    | 177.63     | 12.4       | 1216.73    | 11.9    | 1210.34 | 13.3      | 1180.22   |
| R201                   | 4        | 1252.37  | 8856     | 82.35    | 6          | 1174.69    | 196.05     | 4          | 1253.23    | 4       | 1252.37 | 8         | 1152.63   |
| R202                   | 3        | 1191.70  | 8424     | 78.08    | 5          | 1046.10    | 119.36     | 4          | 1081.82    | 3       | 1191.70 | 6         | 1036.30   |
| R203                   | 3        | 939.50   | 8640     | 80.52    | 5          | 884.02     | 85.53      | 3          | 955.70     | 3       | 939.50  | 6         | 875.21    |
| R204                   | 5        | 731.30   | 8856     | 82.35    | 4          | 750.40     | 69.77      | 3          | 753.32     | 2       | 825.52  | 4         | 737.43    |
| R205                   | 7        | 965.10   | 9180     | 85.40    | 5          | 960.75     | 116.61     | 3          | 1017.96    | 3       | 994.43  | 5         | 954.16    |
| R206                   | 5        | 887.60   | 8100     | 75.64    | 4          | 900.97     | 78.55      | 3          | 915.49     | 3       | 906.14  | 5         | 884.25    |
| R207                   | 5        | 807.00   | 7128     | 66.49    | 4          | 809.72     | 73.13      | 3          | 813.47     | 2       | 890.61  | 4         | 801.15    |
| R208                   | 4        | 703.40   | 9396     | 87.84    | 5          | 723.14     | 65.18      | 2          | 728.63     | 2       | 726.82  | 3         | 706.86    |
| R209                   | 3        | 909.16   | 7884     | 73.20    | 5          | 863.12     | 90.78      | 3          | 918.82     | 3       | 909.16  | 5         | 860.11    |
| R210                   | 6        | 944.70   | 7236     | 67.10    | 5          | 763.22     | 95.55      | 3          | 952.91     | 3       | 939.37  | 5         | 912.80    |
| R211                   | 5        | 754.60   | 8532     | 79.91    | 4          | 1646.17    | 77.49      | 3          | 774.68     | 2       | 885.71  | 4         | 757.60    |
| Avg.                   | 4.55     | 916.95   | 8384     | 78.08    | 4.73       | 956.57     | 97.09      | 3.09       | 924.18     | 2.73    | 951.03  | 5         | 879.86    |

Table 12 Comparison among results obtained through the MODLEM and the state-of-the-art approaches for RC1 and RC2

| Approaches   | MODLEM   | MODLEM   | MODLEM   | MODLEM   | Tabu-ABC   | Tabu-ABC   | Tabu-ABC   | M-MOEA/D   | M-MOEA/D   | HSFLA   | HSFLA   | ESS-PSO   | ESS-PSO   |
|--------------|----------|----------|----------|----------|------------|------------|------------|------------|------------|---------|---------|-----------|-----------|
| Instances    | NV       | TD       | FE       | Time     | NV         | TD         | Time       | NV         | TD         | NV      | TD      | NV        | TD        |
| RC101        | 14       | 1696.95  | 12,636   | 117.42   | 16         | 1646.17    | 271.72     | 14         | 1758.17    | 14      | 1696.95 | 16        | 1639.75   |
| RC102        | 12       | 1554.75  | 13,284   | 123.60   | 14         | 1481.61    | 193.72     | 13         | 1509.18    | 12      | 1554.75 | 14        | 1461.33   |
| RC103        | 11       | 1261.67  | 11,880   | 110.21   | 12         | 1280.76    | 143.69     | 11         | 1274.85    | 11      | 1261.67 | 12        | 1277.55   |
| RC104        | 10       | 1135.48  | 11,232   | 104.03   | 11         | 1162.03    | 92.34      | 10         | 1145.79    | 10      | 1135.48 | 10        | 1138.13   |
| RC105        | 13       | 1629.44  | 13,608   | 126.69   | 16         | 1545.30    | 195.66     | 14         | 1548.43    | 13      | 1629.44 | 15        | 1519.46   |
| RC106        | 11       | 1424.73  | 13,824   | 128.75   | 14         | 1401.17    | 163.77     | 12         | 1447.84    | 11      | 1424.73 | 13        | 1378.62   |
| RC107        | 11       | 1230.48  | 14,580   | 135.96   | 12         | 1235.28    | 137.43     | 11         | 1254.67    | 11      | 1230.48 | 12        | 1212.83   |
| RC108        | 10       | 1139.82  | 11,554   | 107.12   | 11         | 1136.35    | 132.30     | 10         | 1183.85    | 10      | 1139.82 | 11        | 1118.57   |
| Avg.         | 11.5     | 1384.17  | 12,824   | 119.48   | 13.3       | 1361.08    | 166.33     | 11.9       | 1390.35    | 11.5    | 1384.17 | 12.9      | 1343.28   |
| RC201        | 4        | 1406.94  | 9504     | 88.83    | 7          | 1271.78    | 174.15     | 4          | 1421.88    | 4       | 1406.94 | 9         | 1265.56   |
| RC202        | 3        | 1365.65  | 9072     | 84.42    | 6          | 1116.21    | 122.95     | 4          | 1161.29    | 3       | 1365.64 | 8         | 1096.53   |
| RC203        | 3        | 1049.62  | 8428     | 78.12    | 5          | 941.81     | 82.79      | 3          | 1097.29    | 3       | 1049.62 | 5         | 926.82    |
| RC204        | 4        | 788.30   | 9936     | 92.61    | 4          | 801.87     | 56.04      | 3          | 801.90     | 3       | 798.46  | 4         | 786.38    |
| RC205        | 4        | 1297.65  | 8748     | 81.90    | 7          | 1165.82    | 115.08     | 4          | 1327.09    | 4       | 1297.65 | 7         | 1157.55   |
| RC206        | 3        | 1146.32  | 8316     | 77.49    | 5          | 1072.85    | 98.90      | 3          | 1200.92    | 3       | 1146.32 | 6         | 1057.83   |
| RC207        | 6        | 763.30   | 8640     | 80.64    | 5          | 977.11     | 85.60      | 3          | 1107.71    | 3       | 1061.14 | 6         | 966.37    |
| RC208        | 5        | 779.60   | 8748     | 81.27    | 5          | 792.33     | 81.55      | 3          | 841.37     | 3       | 828.14  | 4         | 779.31    |
| Avg.         | 4        | 1074.67  | 8924     | 83.16    | 5.5        | 1017.47    | 102.13     | 3.38       | 1119.93    | 3.25    | 1119.24 | 6.13      | 1004.54   |

distance and number of used vehicles on 13 problem instance. Our proposed MODLEM obtained non-dominated solutions different from those found through the HSFLA approach on 6 problem instances. The HSFLA approach introduced solutions of slightly better quality than our proposed MODLEM on R102, R103 and R104 where the total traveled distance is reduced from 1486.86 to 1486.12, from 1292.68 to 1292.67 and from 1007.31 to 1007.30, respectively, and on R210 where the total traveled distance reduced from 944.7 to 939.37 (improved by %0.56) and the number of used vehicles declined from 6 to 3.

Our proposed MODLEM yielded alternative solutions different from ones obtained through the Tabu-ABC approach on 19 problem instances. On R108 and R208, our proposed MODLEM achieved solutions of higher quality compared to the Tabu-ABC approach taking into account the number of used vehicles and the total traveled distance. Our proposed MODLEM performed slightly worse than the Tabu-ABC approach on R205 and R210. Moreover, our proposed MODLEM and the ESS-PSO approach found alternative solutions on almost all R1 and R2 problem instances. R205, R206, R207 and R210 are the exceptions where the ESS-PSO outperforms our proposed MODLEM.

Considering experimental results on RC1 and RC2 problem instance tabulated in Table 12, on can conclude that our proposed MODLEM was able to find alternative or better solutions compared to all other approaches with the exception of R204 and R208 where our proposed MODLEM is slightly inferior to the ESS-PSO approach.

Regarding the quality of solutions in terms of the number of used vehicles and the total traveled distance, the fact that our proposed MODLEM is a better alternative compared to cutting edge methodologies is obvious.

Computational time is used to further compare the performance of the approaches. The average computational times (in seconds) of our proposed MODLEM and the Tabu-ABC approach are tabulated at the forth and the seventh columns, respectively, in Tables 10, 11 and 12 for each Solomon's problem instances.

Average computational times, averaged over each class of the Solomon's problem instances, are graphically illustrated in Fig. 7. The superiority of this proposed MODLEM to the Tabu-ABC approach in terms of computational time is observed in Fig. 7.

Figure 7 shows that problem instances with customers clustered around the central depot, which belong to classes C1 and C2, require a larger computational time, for both our proposed MODLEM and the Tabu-ABC approach. However, when comparing the averages running time of both approaches on Solomon's problem instances, we note that our proposed MODLEM requires less computational time than the Tabu-ABC approach.

Fig. 7 Comparison of the average computational time for each class of Solomon's data set

![Image](Papers_Converted/0_Artifacts/[moradi2020]_The_new_optimization_algorithm_for_the_vehicle_routing_problem_with_time_windows_using_multi-objective_discrete_learnable_evolution_model_artifacts/image_000009_7790406af62707f90e9d138b78b82cfff3cb07b3c1aa9e3cd83dfaf74cf8260b.png)

Table 13 Advantages and disadvantages of different approaches in solving the VRPTW

| Approach       | Merits                                                                                            | Demerits                                                         |
|----------------|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
| Exact          | Reliable High-quality solutions                                                                   | Slow Low scalability                                             |
| Heuristic      | Trivial role of randomness Low computational time                                                 | Problem-specific Local search                                    |
| Meta-heuristic | Domain-independent Fairly high-quality solutions                                                  | Semi-blind search Weak local search Principal role of randomness |
| MODLEM         | Low computational time High convergence rate Utilizing past experience through a learning process | Problem-specific heuristic operators Knowledge of the domain     |

The presented results show that the proposed MODLEM obtained very competitive results for the VRPTW in terms of the quality of solutions as well as the computational time. This could be due to the ability of the multi-objective learnable evolution model as well as our proposed heuristic operators incorporated in this proposed vehicle routing problem with time windows solving approach to discover the right directions of the evolution in the search process which leads to finding the optimal routes.

The positive and negative points of the proposed methods are summarized in Table 13.

through using experiences learned from current and past populations. The most effective version of the strength Pareto evolutionary algorithm (SPEA) (i.e., SPEA2 ? ) is adopted to govern the objectives of the VRPTW simultaneously, leading to a good approximation of the Pareto fronts. The performance of the MODLEM is enhanced by incorporating two heuristic operators to generate initial population and to instantiate new population of individuals. To encode VRPTW solutions into chromosomes, a new priority-based representation scheme is applied in this proposed MODLEM.

## 7 Conclusion and future work

A new multi-objective discrete learnable evolution model is proposed to handle the vehicle routing problem with time windows. The LEM includes a learning process which detects the promising sub-areas of the solution space that most likely contain optimal routes in an intelligent manner

We examine the performance of the proposed MODLEM on Solomon's benchmark problem instances. The contribution of the heuristic operators included into our proposed MODLEM is evaluated. Experimental results verify that both heuristic operators play a critical role in enhancing the performance. By comparing the obtained results to the best-known solutions identified till now, we notice that the performance of our proposed MODLEM is comparable to the approaches achieving the best-known

solutions in the literature. Through extensive computational experiments and comparisons with the state-of-theart approaches, the impressive performance of our proposed approach is observed in terms of both the quality of solutions and the CPU time.

Regarding the limitations of our proposed MODLEM approach, the default value of the parameters is used in the decision tree C4.5 algorithm and other parameters are tuned off-line. Moreover, the proposed heuristic operators are problem-specific, that is, vehicle routing problem with time windows.

As for future work, our proposed MODLEM approach can be enhanced or extended. On the one hand, an online strategy should be employed for parameter configuration, making the approach more efficient in terms of both quality of solutions and computational time. On the other hand, some modifications should be made to our proposed MODLEM to tackle variant vehicle routing problems. It is expected that our modified approach will be able to confront complex real-world problems in transportation and logistics. These prospective versions are considered as future research directions.

## Compliance with ethical standards

Conflict of interest The author declares that he has no conflict of interest.

Ethical approval This article does not contain any studies with human participants or animals performed by the author.

## References

Agra A, Christiansen M, Figueiredo R, Hvattum LM, Poss M, Requejo C (2013) The robust vehicle routing problem with time windows. Comput Oper Res 40(3):856-866

Alvarenga GB, Mateus GR, De Tomi G (2007) A genetic and set partitioning two-phase approach for the vehicle routing problem with time windows. Comput Oper Res 34(6):1561-1584. https://doi.org/10.1016/j.cor.2005.07.025

Amini S, Javanshir H, Tavakkoli-Moghaddam R (2010) A PSO approach for solving VRPTW with real case study. Int J Res Rev Appl Sci 4(3):118-126

Antes J, Derigs U (1995) A new parallel tour construction algorithm for the vehicle routing problem with time windows

Badeau P, Guertin F, Gendreau M, Potvin J-Y, Taillard E (1997) A parallel tabu search heuristic for the vehicle routing problem with time windows. Transp Res C Emerg Technol 5(2):109-122

Baker EK, Schaffer JR (1986) Solution improvement heuristics for the vehicle routing and scheduling problem with time window constraints. Am J Math Manag Sci 6(3-4):261-300

Baldacci R, Bartolini E, Mingozzi A, Roberti R (2010) An exact solution framework for a broad class of vehicle routing problems. CMS 7(3):229-268

Baldacci R, Mingozzi A, Roberti R (2011) New route relaxation and pricing strategies for the vehicle routing problem. Oper Res 59(5):1269-1283

Baldacci R, Mingozzi A, Roberti R (2012) Recent exact algorithms for solving the vehicle routing problem under capacity and time window constraints. Eur J Oper Res 218(1):1-6

- Ban ˜Os R, Ortega J, Gil C, Ferna ´Ndez A, De Toro F (2013a) A simulated annealing-based parallel multi-objective approach to vehicle routing problems with time windows. Expert Syst Appl 40(5):1696-1707

Ban ˜Os R, Ortega J, Gil C, Ma ´Rquez AL, De Toro F (2013b) A hybrid meta-heuristic for multi-objective vehicle routing problems with time windows. Comput Ind Eng 65(2):286-296

Bara ´n B, Schaerer M (2003) A multiobjective ant colony system for vehicle routing problem with time windows. In: The 21st IASTED International Multi-Conference on Applied Informatics (AI 2003), February 10-13, 2003, Innsbruck, Austria

Barbucha D (2013) An agent-based cooperative population learning algorithm for vehicle routing problem with time windows. In: KES-AMSTA

Barbucha D (2014) A cooperative population learning algorithm for vehicle routing problem with time windows. Neurocomputing 146:210-229

Bard JF, Kontoravdis G, Yu G (2002) A branch-and-cut procedure for the vehicle routing problem with time windows. Transp Sci 36(2):250-269

Beham A (2007). Parallel tabu search and the multiobjective vehicle routing problem with time windows. In: Parallel and Distributed Processing Symposium. IPDPS 2007. IEEE International, IEEE Bent R, Van Hentenryck P (2004) A two-stage hybrid local search for the vehicle routing problem with time windows. Transp Sci 38(4):515-530

Berger J, Barkaoui M (2004) A parallel hybrid genetic algorithm for the vehicle routing problem with time windows. Comput Oper Res 31(12):2037-2053

Berger J, Barkaoui M, Bra ¨ysy O (2003) A route-directed hybrid genetic approach for the vehicle routing problem with time windows. INFOR Inf Syst Oper Res 41(2):179-194

Błocho M, Czech ZJ (2011). A parallel algorithm for minimizing the number of routes in the vehicle routing problem with time windows. In: International conference on parallel processing and applied mathematics. Springer

Blocho M, Czech ZJ (2012) A parallel EAX-based algorithm for minimizing the number of routes in the vehicle routing problem with time windows. In: 2012 IEEE 14th international conference on high performance computing and communication &amp; 2012 IEEE 9th international conference on embedded software and systems (HPCC-ICESS). IEEE

Blocho M, Czech ZJ (2013) A parallel memetic algorithm for the vehicle routing problem with time windows. In: 2013 8th international conference on P2P, parallel, grid, cloud and internet computing (3PGCIC). IEEE

Bra ¨ysy O (2001) Local search and variable neighborhood search algorithms for the vehicle routing problem with time windows. Universitas Wasaensis, Vaasa

Bra ¨ysy O, Dullaert W (2003) A fast evolutionary metaheuristic for the vehicle routing problem with time windows. Int J Artif Intell Tools 12(02):153-172. https://doi.org/10.1142/S0218213003001162

Calvete HI, Gale ´ C, Oliveros M-J, Sa ´nchez-Valverde B (2007) A goal programming approach to vehicle routing problems with soft time windows. Eur J Oper Res 177(3):1720-1733

Castro J, Landa-Silva D, Pe ´rez J (2009) Exploring feasible and infeasible regions in the vehicle routing problem with time windows using a multi-objective particle swarm optimization approach. In: Nature inspired cooperative strategies for optimization (NICSO 2008), pp 103-114

Chabrier A (2006) Vehicle routing problem with elementary shortest path based column generation. Comput Oper Res 33(10):2972-2990

- Chen C-H, Ting C-J (2005) A hybrid ant colony system for vehicle routing problem with time windows. J East Asia Soc Transp Stud 6:2822-2836. https://doi.org/10.11175/easts.6.2822
- Chiang T-C, Hsu W-H (2014) A knowledge-based evolutionary algorithm for the multiobjective vehicle routing problem with time windows. Comput Oper Res 45:25-37
- Chiang W-C, Russell RA (1996) Simulated annealing metaheuristics for the vehicle routing problem with time windows. Ann Oper Res 63(1):3-27
- Chiang W-C, Russell RA (1997) A reactive tabu search metaheuristic for the vehicle routing problem with time windows. INFORMS J Comput 9(4):417-430
- Cook W, Rich JL (1999) A parallel cutting-plane algorithm for the vehicle routing problem with time windows. Technical Report TR99-04, Computational and Applied Mathematics, Rice University, Housten, USA

Cordeau J-F, Maischberger M (2012) A parallel iterated tabu search heuristic for vehicle routing problems. Comput Oper Res 39(9):2033-2050

Cordeau J-F, Laporte G, Mercier A (2001) A unified tabu search heuristic for vehicle routing problems with time windows. J Oper Res Soc 52(8):928-936

Czech ZJ, Czarnas P (2002) Parallel simulated annealing for the vehicle routing problem with time windows. In: 10th Euromicro workshop on parallel, distributed and network-based processing. Proceedings. IEEE

Dan Z, Cai L, Zheng L (2009) Improved multi-agent system for the vehicle routing problem with time windows. Tsinghua Sci Technol 14(3):407-412

Desaulniers G, Desrosiers J, Spoorendonk S (2011) Cutting planes for branch-and-price algorithms. Networks 58(4):301-310

Desaulniers G, Madsen OB, Ropke S (2014) The vehicle routing problem with time windows. Veh Routing Probl Methods Appl 18:119-159

Desrochers M, Desrosiers J, Solomon M (1992) A new optimization algorithm for the vehicle routing problem with time windows. Oper Res 40(2):342-354

Dhanya K, Kanmani S (2016) Solving vehicle routing problem using hybrid swarm intelligent methods. In: 2016 international conference on communication and signal processing (ICCSP). IEEE

Domanski PA, Yashar D, Kaufman KA, Michalski RS (2004) An optimized design of finned-tube evaporators using the learnable evolution model. HVAC&amp;R Res 10(2):201-211

Dullaert W, Bra ¨ysy O (2003) Routing relatively few customers per route. Top 11(2):325-336

Farzi S (2012) The design of self-organizing evolved polynomial neural networks based on learnable evolution model 3. Int Arab J Inf Technol 9(2):124-132

Feillet D, Dejax P, Gendreau M, Gueguen C (2004) An exact algorithm for the elementary shortest path problem with resource constraints: application to some vehicle routing problems. Networks 44(3):216-229

Feillet D, Gendreau M, Rousseau L-M (2007) New refinements for the solution of vehicle routing problems with branch and price. INFOR Inf Syst Oper Res 45(4):239-256

Fisher ML, Jo ¨rnsten KO, Madsen OB (1997) Vehicle routing with time windows: two optimization algorithms. Oper Res 45(3):488-492

Gambardella LM, Taillard E, Agazzi G (1999) MACS-VRPTW: a ´ multiple ant colony system for vehicle routing problems with time windows. Technical Report

Garcia-Najera A, Bullinaria JA (2009) Bi-objective optimization for the vehicle routing problem with time windows: using route similarity to enhance performance. In: EMO. Springer

- Garcia-Najera A, Bullinaria JA (2011) An improved multi-objective evolutionary algorithm for the vehicle routing problem with time windows. Comput Oper Res 38(1):287-300
- Gehring H, Homberger J (1999) A parallel hybrid evolutionary metaheuristic for the vehicle routing problem with time windows. In: Paper presented at the Proceedings of EUROGEN99
- Ghoseiri K, Ghannadpour SF (2010) Multi-objective vehicle routing problem with time windows using goal programming and genetic algorithm. Appl Soft Comput 10(4):1096-1107
- Gong W, Liu X, Zhang J, Fu Z (2007) Two-generation ant colony system for vehicle routing problem with time windows. In: International conference on wireless communications, networking and mobile computing. WiCom 2007. IEEE

Gong Y-J, Zhang J, Liu O, Huang R-Z, Chung HS-H, Shi Y-H (2012) Optimizing the vehicle routing problem with time windows: a discrete particle swarm optimization approach. IEEE Trans Syst Man Cybern C (Appl Rev) 42(2):254-267

Hifi M, Wu L (2014) A hybrid metaheuristic for the vehicle routing problem with time windows. In: International conference on control, decision and information technologies (CoDI'14). https://doi.org/10.1109/CoDIT.2014.6996891

Homberger J, Gehring H (1999) Two evolutionary metaheuristics for the vehicle routing problem with time windows. INFOR Inf Syst Oper Res 37(3):297-318

- Hsu W-H, Chiang T-C (2012) A multiobjective evolutionary algorithm with enhanced reproduction operators for the vehicle routing problem with time windows. In: 2012 IEEE congress on evolutionary computation (CEC), IEEE
- Hu W, Liang H, Peng C, Du B, Hu Q (2013) A hybrid chaos-particle swarm optimization algorithm for the vehicle routing problem with time window. Entropy 15(4):1247-1270. https://doi.org/10. 3390/e15041247

Ioannou G, Kritikos M, Prastacos G (2001) A greedy look-ahead heuristic for the vehicle routing problem with time windows. J Oper Res Soc 52(5):523-537

Irnich S, Villeneuve D (2006) The shortest-path problem with resource constraints and k-cycle elimination for k C 3. INFORMS J Comput 18(3):391-406

Irnich S, Desaulniers G, Desrosiers J, Hadjar A (2010) Path-reduced costs for eliminating arcs in routing and scheduling. INFORMS J Comput 22(2):297-313

Jepsen M, Petersen B, Spoorendonk S, Pisinger D (2006) A nonrobust branch-and-cut-and-price algorithm for the vehicle routing problem with time windows. Oper Res (forthcoming)

Jepsen M, Petersen B, Spoorendonk S, Pisinger D (2008) Subset-row inequalities applied to the vehicle-routing problem with time windows. Oper Res 56(2):497-511

Jiang J, Ng KM, Poh KL, Teo KM (2014) Vehicle routing problem with a heterogeneous fleet and time windows. Expert Syst Appl 41(8):3748-3760

Jiang W, Zhang Y, Xie J (2009) A particle swarm optimization algorithm with crossover for vehicle routing problem with time windows. In: IEEE symposium on computational intelligence in scheduling, CI-Sched 2009, pp 103-106. https://doi.org/10.1109/ SCIS.2009.4927022

Jourdan L, Corne D, Savic D, Walters G (2005) Preliminary investigation of the 'learnable evolution model' for faster/better multiobjective water systems design. In: Evolutionary multicriterion optimization. Springer

Jung S, Moon B-R (2002) A hybrid genetic algorithm for the vehicle routing problem with time windows. In: Paper presented at the proceedings of the 4th annual conference on genetic and evolutionary computation

Kallehauge B (2008) Formulations and exact algorithms for the vehicle routing problem with time windows. Comput Oper Res 35(7):2307-2330

Kallehauge B, Larsen J, Madsen OB (2006) Lagrangian duality applied to the vehicle routing problem with time windows. Comput Oper Res 33(5):1464-1487

Kallehauge B, Boland N, Madsen OB (2007) Path inequalities for the vehicle routing problem with time windows. Networks 49(4):273-293

Kim M, Hiroyasu T, Miki M, Watanabe S (2004) SPEA2 ? : improving the performance of the strength Pareto evolutionary algorithm 2. In: International conference on parallel problem solving from nature. Springer

Koc ¸ C ¸ , Bektas ¸ T, Jabali O, Laporte G (2015) A hybrid evolutionary algorithm for heterogeneous fleet vehicle routing problems with time windows. Comput Oper Res 64:11-27. https://doi.org/10. 1016/j.cor.2015.05.004

Kohl N, Desrosiers J, Madsen OB, Solomon MM, Soumis F (1999) 2-path cuts for the vehicle routing problem with time windows. Transp Sci 33(1):101-116

Kolen AW, Rinnooy Kan A, Trienekens HW (1987) Vehicle routing with time windows. Oper Res 35(2):266-273

Kontoravdis G, Bard JF (1995) A GRASP for the vehicle routing problem with time windows. ORSA J Comput 7(1):10-23

Kumar VS, Thansekhar M, Saravanan R, Amali SMJ (2014) Solving multi-objective vehicle routing problem with time windows by FAGA. Procedia Eng 97:2176-2185

Labadi N, Prins C, Reghioui M (2008) A memetic algorithm for the vehicle routing problem with time windows. RAIRO Oper Res 42(3):415-431

Larsen J (2004) Refinements of the column generation process for the vehicle routing problem with time windows. J Syst Sci Syst Eng 13(3):326-341

Lau HC, Lim YF, Liu Q (2001) Diversification of search neighborhood via constraint-based local search and its applications to VRPTW. In: Paper presented at the 3rd international workshop on integration of AI and OR techniques (CP-AI-OR), Kent, UK Lau HC, Sim M, Teo KM (2003) Vehicle routing problem with time windows and a limited number of vehicles. Eur J Oper Res 148(3):559-569

Le Bouthillier A, Crainic TG (2005) A cooperative parallel metaheuristic for the vehicle routing problem with time windows. Comput Oper Res 32(7):1685-1708

Le Bouthillier A, Crainic TG, Kropf P (2005) A guided cooperative search for the vehicle routing problem with time windows. IEEE Intell Syst 20(4):36-42

Lee LH, Tan KC, Ou K, Chew YH (2003) Vehicle capacity planning system: a case study on vehicle routing problem with time windows. IEEE Trans Syst Man Cybern A Syst Hum 33(2):169-178

Letchford AN, Salazar-Gonza ´lez J-J (2006) Projection results for vehicle routing. Math Program 105(2):251-274

Li H, Lim A (2003) Local search with annealing-like restarts to solve the VRPTW. Eur J Oper Res 150(1):115-127. https://doi.org/10. 1016/S0377-2217(02)00486-1

Lim A, Zhang X (2007) A two-stage heuristic with ejection pools and generalized ejection chains for the vehicle routing problem with time windows. INFORMS J Comput 19(3):443-457

Liu F-HF, Shen S-Y (1999) A route-neighborhood-based metaheuristic for vehicle routing problem with time windows. Eur J Oper Res 118(3):485-504

Liu X, Jiang W, Xie J (2009) Vehicle routing problem with time windows: a hybrid particle swarm optimization approach. In: 5th international conference on natural computation, ICNC 2009, pp 502-506. https://doi.org/10.1109/ICNC.2009.353

Lozano L, Duque D, Medaglia AL (2015) An exact algorithm for the elementary shortest path problem with resource constraints. Transp Sci 50(1):348-357

Luo J, Li X, Chen M-R, Liu H (2015) A novel hybrid shuffled frog leaping algorithm for vehicle routing problem with time windows. Inf Sci 316:266-292

Lysgaard J (2006) Reachability cuts for the vehicle routing problem with time windows. Eur J Oper Res 175(1):210-223

Melia ´n-Batista B, De Santiago A, AngelBello F, Alvarez A (2014) A bi-objective vehicle routing problem with time windows: a real case in Tenerife. Appl Soft Comput 17:140-152

Mester D (2002) An evolutionary strategies algorithm for large scale vehicle routing problem with capacitate and time windows restrictions. In: Paper presented at the proceedings of the conference on mathematical and population genetics, University of Haifa, Israel

Mester D, Bra ¨ysy O (2005) Active guided evolution strategies for large-scale vehicle routing problems with time windows. Comput Oper Res 32(6):1593-1614. https://doi.org/10.1016/j.cor. 2003.11.017

Mester D, Bra ¨ysy O, Dullaert W (2007) A multi-parametric evolution strategies algorithm for vehicle routing problems. Expert Syst Appl 32(2):508-517

Michalski RS (2000) Learnable evolution model: evolutionary processes guided by machine learning. Mach Learn 38(1-2):9-40

Michalski R (2010) Learning and evolution: an introduction to nondarwinian evolutionary computation. In: Foundations of intelligent systems, pp 21-30

Michalski RS, Kaufman KA (2006) Intelligent evolutionary design: a new approach to optimizing complex engineering systems and its application to designing heat exchangers. Int J Intell Syst 21(12):1217-1248

Mohemmed AW, Sahoo NC, Geok TK (2008) Solving shortest path problem using particle swarm optimization. Appl Soft Comput 8(4):1643-1653

Moradi B (2018a) An intelligent evolutionary computation approach for solving the shortest path problem. J Mult Valued Log Soft Comput 30(4-6):359-377

Moradi B (2018b) Multi-objective mobile robot path planning problem through learnable evolution model. J Exp Theor Artif Intell 31(2):1-24

Moradi B, Mirzaei A (2016) A new automated design method based on machine learning for CMOS analog circuits. Int J Electron 103(11):1868-1881

Mun ˜oz-Zavala A, Herna ´ndez-Aguirre A, Villa-Diharce E (2009) Particle evolutionary swarm multi-objective optimization for vehicle routing problem with time windows. In: Swarm intelligence for multi-objective problems in data mining, pp 233-257

Nagata Y, Bra ¨ysy O (2009) A powerful route minimization heuristic for the vehicle routing problem with time windows. Oper Res Lett 37(5):333-338

Nagata Y, Bra ¨ysy O, Dullaert W (2010) A penalty-based edge assembly memetic algorithm for the vehicle routing problem with time windows. Comput Oper Res 37(4):724-737

Nalepa J (2014) Adaptive memetic algorithm for the vehicle routing problem with time windows. In: Proceedings of the companion publication of the 2014 annual conference on genetic and evolutionary computation. ACM

Nalepa J, Blocho M (2016) Adaptive memetic algorithm for minimizing distance in the vehicle routing problem with time windows. Soft Comput 20(6):2309-2327

Nalepa J, Czech ZJ (2012) A parallel heuristic algorithm to solve the vehicle routing problem with time windows. Stud Inform 33(1):91-106

- Nalepa J, Czech ZJ (2013). New selection schemes in a memetic algorithm for the vehicle routing problem with time windows. In: International conference on adaptive and natural computing algorithms. Springer

Nazif H, Lee LS (2010) Optimized crossover genetic algorithm for vehicle routing problem with time windows. Am J Appl Sci 7(1):95

- Niu B, Wang H, Tan L, Li L, Wang J-W (2012) Vehicle routing problem with time windows based on adaptive bacterial foraging optimization. In: ICIC, vol 2. Springer
- Ombuki B, Ross BJ, Hanshar F (2006) Multi-objective genetic algorithms for vehicle routing problem with time windows. Appl Intell 24(1):17-30

Pang K-W (2011) An adaptive parallel route construction heuristic for the vehicle routing problem with time windows constraints. Expert Syst Appl 38(9):11939-11946

Paraskevopoulos DC, Repoussis PP, Tarantilis CD, Ioannou G, Prastacos GP (2008) A reactive variable neighborhood tabu search for the heterogeneous fleet vehicle routing problem with time windows. J Heuristics 14(5):425-455

Pecin D, Contardo C, Desaulniers G, Uchoa E (2017) New enhancements for the exact solution of the vehicle routing problem with time windows. INFORMS J Comput 29(3):489-502

Petersen B, Pisinger D, Spoorendonk S (2008) Chva ´tal-Gomory rank1 cuts used in a Dantzig-Wolfe decomposition of the vehicle routing problem with time windows. Vehicle Routing Probl Latest Adv New Chall 43:397-419

Pierre DM, Zakaria N (2014) Partially optimized cyclic shift crossover for multi-objective genetic algorithms for the multiobjective vehicle routing problem with time-windows. In: 2014 IEEE symposium on computational intelligence in multi-criteria decision-making (MCDM). IEEE

Pierre DM, Zakaria N (2017) Stochastic partially optimized cyclic shift crossover for multi-objective genetic algorithms for the vehicle routing problem with time-windows. Appl Soft Comput 52:863-876

Potvin J-Y, Bengio S (1996) The vehicle routing problem with time windows part II: genetic search. INFORMS J Comput 8(2):165-172

Potvin J-Y, Rousseau J-M (1993) A parallel route building algorithm for the vehicle routing and scheduling problem with time windows. Eur J Oper Res 66(3):331-340

Potvin J-Y, Rousseau J-M (1995) An exchange heuristic for routeing problems with time windows. J Oper Res Soc 46(12):1433-1446 Prescott-Gagnon E, Desaulniers G, Rousseau LM (2009) A branchand-price-based large neighborhood search algorithm for the vehicle routing problem with time windows. Networks 54(4):190-204

- Qi M, Miao L, Zhang L, Xu H (2008) A new tabu search heuristic algorithm for the vehicle routing problem with time windows. In: International conference on management science and engineering. ICMSE 2008. 15th annual conference proceedings. IEEE
- Qi Y, Hou Z, Li H, Huang J, Li X (2015) A decomposition based memetic algorithm for multi-objective vehicle routing problem with time windows. Comput Oper Res 62:61-77

Repoussis P, Paraskevopoulos D, Tarantilis C, Ioannou G (2006) A reactive greedy randomized variable neighborhood tabu search for the vehicle routing problem with time windows. In: Hybrid metaheuristics, pp 124-138

Repoussis PP, Tarantilis CD, Ioannou G (2009) Arc-guided evolutionary algorithm for the vehicle routing problem with time windows. IEEE Trans Evol Comput 13(3):624-647

- Righini G, Salani M (2006) Symmetry helps: bounded bi-directional dynamic programming for the elementary shortest path problem with resource constraints. Discrete Optim 3(3):255-273
- Rochat Y, Taillard E ´ D (1995) Probabilistic diversification and intensification in local search for vehicle routing. J Heuristics 1(1):147-167
- Rousseau L-M, Gendreau M, Pesant G (2002) Using constraint-based operators to solve the vehicle routing problem with time windows. J Heuristics 8(1):43-58
- Rousseau L-M, Gendreau M, Feillet D (2007) Interior point stabilization for column generation. Oper Res Lett 35(5):660-668
- Russell RA (1977) An effective heuristic for the m-tour traveling salesman problem with some side conditions. Oper Res 25(3):517-524
- Russell RA (1995) Hybrid heuristics for the vehicle routing problem with time windows. Transp Sci 29(2):156-166
- Savelsbergh MW (1985) Local search in routing problems with time windows. Ann Oper Res 4(1):285-305
- Savelsbergh M (1990) An efficient implementation of local search algorithms for constrained routing problems. Eur J Oper Res 47(1):75-85
- Savelsbergh MW (1992) The vehicle routing problem with time windows: minimizing route duration. ORSA J Comput 4(2):146-154
- Sessomboon W, Watanabe K, Irohara T, Yoshimoto K (1998) A study on multi-objective vehicle routing problem considering customer satisfaction with due-time (the creation of Pareto optimal solutions by hybrid genetic algorithm). Trans Jpn Soc Mech Eng. https://doi.org/10.1299/kikaic.64.1108
- Shaw P (1997) A new local search algorithm providing high quality solutions to vehicle routing problems. Dept of Computer Science, University of Strathclyde, Glasgow, Scotland, UK, APES Group

Shehab ME, Badran K, Salama GI (2013a) A generic feature extraction model using learnable evolution models (LEM ? ID3). Int J Comput Appl 64(11):27-32

- Shehab ME, Badran K, Salama GI, Mgeed MZA (2013b) A novel feature extraction model using learnable evolution model. Int J Comput Sci Telecommun 4(4):5-9

Sheskin DJ (2011) Handbook of parametric and nonparametric statistical procedures. CRC Press, Boca Raton

- Solomon MM (1986) On the worst-case performance of some heuristics for the vehicle routing and scheduling problem with time window constraints. Networks 16(2):161-174
- Solomon MM (1987) Algorithms for the vehicle routing and scheduling problems with time window constraints. Oper Res 35(2):254-265
- Solomon MM, Desrosiers J (1988) Survey paper-time window constrained routing and scheduling problems. Transp Sci 22(1):1-13
- Taillard E, Badeau P, Gendreau M, Guertin F, Potvin J-Y (1997) A ´ tabu search heuristic for the vehicle routing problem with soft time windows. Transp Sci 31(2):170-186. https://doi.org/10. 1287/trsc.31.2.170
- Tan K, Lee L, Ou K (2001) Artificial intelligence heuristics in solving vehicle routing problems with time window constraints. Eng Appl Artif Intell 14(6):825-837. https://doi.org/10.1016/S09521976(02)00011-8
- Tan KC, Chew YH, Lee L (2006) A hybrid multiobjective evolutionary algorithm for solving vehicle routing problem with time windows. Comput Optim Appl 34(1):115. https://doi.org/ 10.1007/s10589-005-3070-3
- Tan L, Lin F, Wang H (2015) Adaptive comprehensive learning bacterial foraging optimization and its application on vehicle routing problem with time windows. Neurocomputing 151:1208-1215
- Tavakkoli-Moghaddam R, Gazanfari M, Alinaghian M, Salamatbakhsh A, Norouzi N (2011) A new mathematical model for a

competitive vehicle routing problem with time windows solved by simulated annealing. J Manuf Syst 30(2):83-92

Tavares J, Machado P, Pereira FB, Costa E (2003) On the influence of GVR in vehicle routing. In: Proceedings of the ACM symposium on applied computing, pp 753-758. https://doi.org/10.1145/ 952532.952679

Thangiah SR, Nygard KE, Juell PL (1991) Gideon: a genetic algorithm system for vehicle routing with time windows. In: Seventh IEEE conference on artificial intelligence applications. Proceedings. IEEE

Thangiah SR, Osman IH, Sun T (1994) Hybrid genetic algorithm, simulated annealing and tabu search methods for vehicle routing problems with time windows. Computer Science Department, Slippery Rock University, Technical Report SRU CpSc-TR-9427, 69

Ursani Z, Essam D, Cornforth D, Stocker R (2011) Localized genetic algorithm for vehicle routing problem with time windows. Appl Soft Comput 11(8):5375-5390

Van Landeghem H (1988) A bi-criteria heuristic for the vehicle routing problem with time windows. Eur J Oper Res 36(2):217-226

Wang C-H, Li C-H (2011) Optimization of an established multiobjective delivering problem by an improved hybrid algorithm. Expert Syst Appl 38(4):4361-4367. https://doi.org/10.1016/j. eswa.2010.09.105

Wang W, Wang Z, Qiao F (2008a) An improved genetic algorithm for vehicle routing problem with time-window. In: International symposium on computer science and computational technology. ISCSCT'08. IEEE

Wang X, Xu C, Hu X (2008b) Genetic algorithm for vehicle routing problem with time windows and a limited number of vehicles. In: International conference on management science and engineering. ICMSE 2008. 15th annual conference proceedings. IEEE

Woch M, Łebkowski P (2009) Sequential simulated annealing for the vehicle routing problem with time windows. Decis Mak Manuf Serv 3(1-2):87-100

Wojtusiak J (2012) The LEM3 system for multitype evolutionary optimization. Comput Inform 28(2):225-236

- Wojtusiak J, Warden T, Herzog O (2012a) The learnable evolution model in agent-based delivery optimization. Memet Comput 4(3):165-181

Wojtusiak J, Warden T, Herzog O (2012b) Machine learning in agentbased stochastic simulation: inferential theory and evaluation in transportation logistics. Comput Math Appl 64(12):3658-3665

Xu H, Fan W, Wei T, Yu L (2008) An Or-opt NSGA-II algorithm for multi-objective vehicle routing problem with time windows. In: IEEE conference on automation science and engineering, CASE 2008, pp 309-314. https://doi.org/10.1109/COASE.2008. 4626505

Yassen ET, Ayob M, Nazri MZA, Sabar NR (2015) Meta-harmony search algorithm for the vehicle routing problem with time windows. Inf Sci 325:140-158

Yu B, Yang Z, Yao B (2011) A hybrid algorithm for vehicle routing problem with time windows. Expert Syst Appl 38(1):435-441. https://doi.org/10.1016/j.eswa.2010.06.082

Zhang D, Cai S, Ye F, Si Y-W, Nguyen TT (2017) A hybrid algorithm for a vehicle routing problem with realistic constraints. Inf Sci 394:167-182

Zhang J, Yang F, Weng X (2018) An evolutionary scatter search particle swarm optimization algorithm for the vehicle routing problem with time windows. IEEE Access 6:63468-63485

Zhao YW, Wu B, Wang W, Ma YL, Wang W, Sun H (2004) Particle swarm optimization for vehicle routing problem with time windows. Mater Sci Forum 471:801-805

Zitzler E, Thiele L (1998a) An evolutionary algorithm for multiobjective optimization: the strength pareto approach. Citeseer

Zitzler E, Thiele L (1998b) Multiobjective optimization using evolutionary algorithms-a comparative case study. In: International conference on parallel problem solving from nature. Springer

Zitzler E, Thiele L (1999) Multiobjective evolutionary algorithms: a comparative case study and the strength Pareto approach. IEEE Trans Evol Comput 3(4):257-271

Zitzler E, Deb K, Thiele L (2000) Comparison of multiobjective evolutionary algorithms: empirical results. Evol Comput 8(2):173-195

Zitzler E, Laumanns M, Thiele L (2001) SPEA2: improving the strength Pareto evolutionary algorithm. In: Eurogen

Publisher's Note Springer Nature remains neutral with regard to jurisdictional claims in published maps and institutional affiliations.