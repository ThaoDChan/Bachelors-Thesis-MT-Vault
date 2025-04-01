![Image](image_000000_d3e2a25c74157d02240004153c4dc8b879721d15051f97be27565ce2c12fda5b.png)

Contents lists available at ScienceDirect

## Information Sciences

j o urnal homepage: www.elsevier.com/locate/ins

## QMOEA: A Q-learning-based multiobjective evolutionary algorithm for solving time-dependent green vehicle routing problems with time windows

Rui Qi a , Jun-qing Li a,b, ⇑ , Juan Wang a , Hui Jin a , Yu-yan Han b a b

- School of Information Science and Engineering, Shandong Normal University, Jinan 250014, China
- School of Computer, Liaocheng University, Liaocheng 252059, China

## a r t i c l e i n f o

## a b s t r a c t

Article history:

Received 24 October 2021 Received in revised form 11 June 2022 Accepted 13 June 2022 Available online 17 June 2022

Keywords:

multiobjective optimization vehicle routing problem with time windows customer satisfaction time dependent Q-learning

The vehicle routing problem with time windows (VRPTW) is critical in the fields of operations research and combinatorial optimization. To promote research on the multiobjective VRPTW, a time-dependent green VRPTW (TDGVRPTW) is introduced in this study. Subsequently, a Q-learning-based multiobjective evolutionary algorithm (QMOEA) is proposed to solve the TDGVRPTW, where three objectives are simultaneously considered: total duration of vehicles, energy consumption, and customer satisfaction. In QMOEA, a hybrid initial method is devised that includes four problem-specific heuristics, to generate initial solutions with a high level of quality and diversity. Then, considering the problem features, two Pareto-front-based crossover strategies are designed to learn from the approximate Pareto front to explore the search space and accelerate the convergence process. Moreover, five local search operators are selected by a Q-learning agent at the decision point, to enhance local search abilities. Finally, a set of instances based on a realistic logistic system is presented to verify the effectiveness and superiority of QMOEA.

/C211 2022 Elsevier Inc. All rights reserved.

## 1. Introduction

In the modern transportation industry, vehicle routing problems (VRPs) have been intensively applied and investigated, such as perishable product transportation, cold chain transportation [1], autonomous mobile robots [2,3] and first-mile transportation [4]. The green VRP (GVRP) has become an important global research topic to improve energy efficiency and reduce carbon emissions. However, conflicting objectives, such as economic, environmental, and social benefits, should be considered simultaneously to reflect a realistic scenario. In this study, we investigated an extension of the multiobjective GVRP, denoted as TDGVRPTW, which simultaneously considers three classical optimization problems: GVPR, VRPTW, and time-dependent VRP (TDVRP). The change in vehicle speed over time, the road slope and a vehicle's current load were all considered to calculate a vehicle's fuel consumption and carbon emissions. Then, three objectives were optimized, namely, the total duration of vehicles, energy consumption, and customer satisfaction.

Usually, the transportation times between customers are considered critical issues for VRPs. In most studies [5,6], the transportation time was simplified as a constant value, thereby neglecting the impact of transportation time on a practical problem, especially urban traffic congestion. Notably, time-dependent factors should require an explicit treatment in appli-

![Image](image_000001_527d78c3f346d72ac2325f58190532adbd96ca3a11321de3366dcf2333f4617a.png)

![Image](image_000002_08c60ee4290a3c7f55c116c3805ead036fade5a3158e1c65710e3b1a5b98e45c.png)

cations. Not considering these factors will affect a product's freshness and will reduce customer satisfaction, thereby decreasing revenue. However, research on this stringent constraint is inadequate, and the studies [7-10] are limited to the single-objective TDVRPTW concept.

Moreover, various multiobjective evolutionary algorithms (MOEAs) [11] have been employed to solve continuous and discrete optimization problems. Compared with a single-objective algorithm, multiobjective optimization algorithms can balance conflicting objectives. A dominance-based MOEA can place multiple objectives at the same priority and can use the dominance relationship to determine whether the current solution should be retained. As such, a decision-maker can be presented with a greater number of reasonable schemes from which to select according to the actual situation. In previous studies [6,12-15], the equal probability method has been employed to select local search strategies, but this method ignores the knowledge generated in the evolutionary process. In this study, we introduce agents trained by Q-learning to learn knowledge from the environment. A strategy that maximizes long-term cumulative rewards is selected at the decision point, making the selection of each strategy knowledge-based but not random.

Based on the analysis, the considered time-dependent green VRPTW is close to reality, and many challenging issues, such as vehicle fuel consumption, carbon emissions and customer satisfaction, should be considered simultaneously. As such, the TDGVRPTW is modeled as a complicated multiple objective problem (MOP). However, few studies have considered this problem. Therefore, we propose a new algorithm, a Q-learning-based MOEA (QMOEA), for TDGVRPTW.

The main contributions of this study are as follows. (1) A three-objective version of the TDGVRPTW was formulated considering the constraints of customer satisfaction, total duration of vehicles, and energy consumption. (2) A hybrid initialization strategy was employed to produce high-quality and diverse solutions. (3) Considering the TDGVRPTW's features, two Pareto-front-based crossover strategies were designed to learn location information between customers and information regarding the order of visits by customers. (4) An adaptive local search method based on Q-learning was embedded to enhance local search abilities, where a reward update method was designed to balance multiple objectives. (5) The proposed algorithm, QMOEA, was employed to solve the TDGVRPTW.

The remainder of this article is organized as follows. In Section 2, we briefly present the related work. The formula description of the TDGVRPTW is given in Section 3. Section 4 elaborates the implementation details of QMOEA along with a novel encoding approach. In Section 5, various case studies and comparisons are presented to verify the effectiveness and superiority of QMOEA. Finally, the conclusions and future research directions are given along with pertinent observations in Section 6.

## 2. Related work

In this section, we briefly review the relevant literature on three typical optimization problems, namely, GVPR, VRPTW, and TDVRP, and the notion of multiobjective optimization.

## 2.1. GVRP

One of the fastest growing research topics under the VRP variant is the GVRP, which attempts to reduce greenhouse gas emissions. Sawik et al. [16] analyzed the influence of multiple factors on green vehicle scheduling, including environmental cost, total distance, fuel cost, noise, greenhouse gas emission, and fuel consumption. They indicated that distance and driving times were related to environmental objectives. In addition, Wang et al. [13] considered solving capacitated GVRP using a memetic algorithm with competition. Mehlawat et al. [7] studied a GVRP considering fuzzy travel time, multiple depots, split delivery, and heterogeneous vehicles and proposed a hybrid GA to solve it. Yu et al. [17] introduced an improved branchand-price algorithm, and utilized it to solve the heterogeneous fleet GVRP with time windows. Furthermore, an alternative fuel vehicle (AFV) was presented to replace the traditional fuel-powered vehicle [13]. Ren et al. [18] developed a variable neighborhood search for GVRP under hybrid energy vehicles with both gasoline and diesel vehicles and electric vehicles. However, the drawbacks of AFVs are obvious. Their usage on a massive scale is limited because battery charging infrastructure is scarce. Moreover, the multiobjective GVRP has been investigated. In [19], four objectives-total travel cost, traveling time of vehicles, carbon emissions, and revenue-were studied with different priorities. An improved ant colony optimization algorithm was adopted to solve the multidepot GVRP. In [20], three objectives-environmental cost, total cost and customer satisfaction-were considered. In [21], three sustainability dimensions, including economic, environmental and social, were investigated using an iterated greedy local search algorithm for the GVRP. However, most multiobjective methods are based on the single objective manner, which obtains only the final solution by fixing the weight of each objective and cannot truly reflect an actual situation.

## 2.2. VRPTW

VRPTW is a prominent VRP variant. In the VRPTW, in addition to the demand, each customer has a time window, that is, a specified period within which the customer is expected to arrive. Solomon [22] proposed construction methods for the initial population and constructed benchmark instances based on actual scenarios. Solomon provided opportunities and information for future research development. Afterward, various novel constraints were added based on the standard instances.

Moradi [23] proposed a heuristic technique and an operator for the VRPTW using a novel multiobjective discrete learnable evolution model. Chen et al. [24] developed an adaptive large neighborhood search heuristic for solving the VRPTW with delivery robots. Li et al. [25] considered a VRPTW with the synchronous transport of prefabricated components and developed an improved artificial bee colony to solve it. Song et al. [1] employed an improved artificial fish swarm algorithm for the VRPTW with energy consumption. The VRPTW has always been considered a multiobjective problem [26]. Recently, many researchers have tried solving the multiobjective VRPTW using Pareto-based methods. Wang et al. [5] developed a two-stage MOEAfor multidepot VRPTW. A periodic VRPTW considering five objectives was solved using a hybrid multiobjective memetic algorithm [11]. Zhang et al. [12] proposed multiobjective local search algorithms for the VRPTW to minimize the total traveling cost and balance the profit. Jiang et al. [27] presented a three-objective optimization model for a heterogeneous VRPTW and solved it using a variable neighborhood search-based hybrid MOEA. The above studies assumed a constant vehicle speed during transportation. In a realistic scenario, the vehicle speed is not constant.

## 2.3. TDVRP

TDVRP, another variant of the VRP, is suitable for the actual industrial environment. In a realistic logistic system, traffic congestion is inevitable. In particular, vehicle speed, load weight, and road slope significantly influence carbon emissions and fuel consumption. Therefore, the assumption that the speed for all road sections is constant is invalid. Recently, TDVRPTW has been studied extensively. Gmira et al. [28] studied TDVRPTW in which travel speed was related to road segments in a road network and developed a tabu search algorithm to solve it. Mehlawat et al. [7] employed a general functional form to replace the travel time with triangular fuzzy numbers. Ichoua et al. [29] proposed that the time of the day will affect the travel time. Further analysis showed that the change in vehicle speed due to traffic congestion is the main factor affecting vehicle transportation time. Recently, Fan et al. [8] solved a time-dependent multidepot GVRP using a hybrid genetic algorithm (GA). Pan et al.[9,10] studied TDVRP with time windows (TDVRPTW) and multitrip TDVRPTW to minimize the total route duration and travel distance (TD) of trips, respectively. Jie et al. [30] combined the sweep algorithm and an improved particle swarm optimization to solve the TDVRP with soft time windows and stochastic factors. Allahyari et al. [31] introduced a secure TDVRP with uncertain demands. In [32], a model of four objectives, namely, operational cost, deterioration cost, carbon emissions, and customer satisfaction, was considered, and a many-objective gradient evolution algorithm was studied.

## 2.4. Multiobjective optimization

MOEAs can successfully solve MOPs [33] that comprise two or three objectives to be optimized simultaneously. As an example of an MOP, consider the following M -objective minimization problem:

$$M i n i m i z e \, F ( x ) = \, \{ f _ { 1 } ( x ), f _ { 2 } ( x ), \, \dots, f _ { M } ( x ) \},$$

$$\ f i n i m i z e F ( x ) = \{ f _ { 1 } ( x ), f _ { 2 } ( x ), \, \dots, f _ { M } ( x ) \},$$

where thedecisionvectorisdenotedas x ¼ f x1 ; x2 ; x3 ; x4 ; :::; xN g 2 X , an M-dimensional search space. Notably, MOPs are referred to as many-objective optimization problems when M is greater than three. The basic concepts of MOPs are defined as follows.

Definition 1. : (Pareto Domination) Let two solutions x y ; 2 X ; y is dominated by x (denoted as x /C30 y ), if and only if 8 2 f i 1 ; /C1 /C1 /C1 ; M g ; f i x ð Þ /C20 f i y ð Þ and 9 j 2 f 1 ; /C1 /C1 /C1 ; N g ; f j x ð Þ &lt; f j y ð Þ :

Definition 2. : (Pareto Set) If :9 y 2 X ; y /C30 x , then x is a Pareto set (PS).

Definition 3. : (Pareto Front) When the set of all PS is mapped to objective spaces, the image is called a Pareto front (PF).

Fig. 1. One example in 2D objective space

![Image](image_000003_16d47c4a6155df549f0fb83f87d2f2ffb6ef51e6ea0319377eb22a68c2b0a876.png)

Several tradeoff solutions, known as Pareto optimal solutions, are provided by MOEAs as a consequence of the dominance strategy. As shown in Fig. 1, each decision vector x 2 X can be mapped to two objectives: f 1 and f 2 . As shown in Fig. 1, z /C30 w because the solution w is within the dominant range of the solution z (gray rectangular block). Notably, all red points are not dominated by other solutions, which means that the set of red points is a part of PF and the decision vector corresponding to the red points is a part of PS .

To solve MOPs, Zhang et al. [34] conducted a study based on decomposition, where a multiobjective problem was converted into a single-objective problem using linear and nonlinear methods and designed MOEA based on decomposition (MOEA/D). Recently, Liu et al. [35] modified MOEA/D. Zhou et al. [36] introduced a multiobjective state transition algorithm based on a modified decomposition method. For dominance-based MOEAs, Srinivas and Deb proposed a nondominated sorting GA (NSGA). Subsequently, NSGA-II [37] and NSGA-III [38] were developed by Deb et al. Recently, He et al. [39] proposed MOEAs based on coordinated selection (MaOEA-CSS) to solve high-dimensional MOPs. Santiago et al. [40] introduced a novel MOEAwith a fuzzy logic-based adaptive selection of operators. In addition, indicator-based MOEAs have attracted increasing research attention. Using indicators to guide the evolutionary direction of a population, the population can reach an actual PF. Elarbi et al. [41] designed reference point dominance-based NSGA-II (RPDNSGAII). Hen et al. [42] used a hyperplane formed by neighboring solutions to obtain nondominated solutions and proposed a hyperplane-assisted evolutionary algorithm (hpaEA).

Many methods have been applied to VRP in various contexts, as evidenced by the studies above. For the modern transportation industry, the impact of many realistic factors on vehicle scheduling, such as time variations, greenhouse gas emissions, and time windows, needs to be considered simultaneously. Notably, only a few studies have considered multiple factors simultaneously. When two objectives, i.e., the number of vehicles (NV) and TD, are considered, NV is usually taken as the primary objective. Furthermore, to meet practical needs, multiple objectives are simultaneously considered to obtain multiple sets of solutions from which decision-makers may choose. Accordingly, in this study, we consider TDGVRPTW focusing on three objectives, namely, total duration of vehicles, energy consumption, and customer satisfaction.

## 3. Problem description and formulation

In this section, the considered TDGVRPTW is described. The calculation methods of the three objectives involved in this problem-time-dependent travel time function, energy consumption function, and customer satisfaction function-are also described in detail.

## 3.1. Problem description

The TDGVRPTW is defined as a complete directed graph G C 0 ; E /C0 /C1 : C 0 ¼ C [ f c 0 g is the vertex set, where C ¼ f c 1 ; c 2 ; /C1 /C1 /C1 ; cn g contains n customers and c 0 denotes the starting and ending vertices of the vehicle. Each customer ci 2 C is associated with a service time si , demand for goods qi , and time windows eei ; ll i ½ /C138 . Each vehicle has a maximum capacity Q to service customers. A route R is defined as a sequence of nodes that starts from the depot, visits a sequence of customers, and returns to the depot. Without loss of generality, we consider a soft time window. Thus, a higher customer satisfaction cs is received when vehicles arrive within the time window of a customer. E ¼ f ð e i ; j Þj i ; j 2 C i ; -j g is the set of edges. Each edge has a slope wij and category g ; namely, fast, medium, and slow speeds. t ij is defined as the duration time between nodes i and j , whose value is associated with the distance dij and speed sp within time zone p . The speed-time function C /C1Þ ð is depicted in Fig. 2. We will describe this in detail in Subsection 3.2.

To serve all customers in C , the following constraints ( C 1 /C0 C 3 ) and assumptions ( S 1 /C0 S 3 ) should be considered.

- 1) Return time constraint C 1 : the vehicle must start and return to the depot before the closing time.

Fig. 2. The relationship between speed and time zones

![Image](image_000004_6a9f249b01b610c72c5df39d13efcc628657be9baac4c760bdce9f1b00d97516.png)

- 2) Vehicle capacity constraint C 2 : the vehicle capacity cannot be exceeded.
- 3) Single-visit constraint C 3 : each customer is served only once.
- 4) Soft time window assumption S 1 : early arrival ( &lt; eei ) and later arrival ( &gt; ll i ) are allowed for customer ci , which will affect customer satisfaction.
- 5) Vehicle type assumption S 2 : a homogeneous vehicle fleet is employed.
- 6) Departure time assumption S 3 : for the depot, the vehicle's departure time is zero.

The goal of TDGVRPTW is to determine the vehicle routes to simultaneously minimize the total duration of vehicles f 1 , energy consumption f 2 , and customer satisfaction f 3 . The detailed definition of objectives is introduced separately in later sections.

## 3.2. Time-dependent travel time function

Ichoua et al. [29] assumed that the vehicle speed can be regarded as a fixed value in a short period. Thus, workday T is partitioned into p time zones, i.e., T ¼ z 1 ; z 2 ; /C1 /C1 /C1 ; zr ; /C1 /C1 /C1 ; z p /C8 /C9 . In addition, the speed-time function C /C1Þ ð is modeled as a stepwise function (Fig. 2). For a given edge e i ; j ð Þ , the travel time t ij may need to span multiple time zones and the distance dij can be obtained [Fig. 3(a)]. If vehicle k departs node i to j withintimezone zr ¼ ½ tt r /C0 1 ; tt r /C138 , denoted as t kr ij , the maximum duration time ls kr ij can be calculated using ls kr ij ¼ tt r /C0 t kr ij : Moreover, let the continuous variables F kr ij , e kr ij , and J r ij represent the maximum TD, the actual TD, and remaining distance respectively : Fig. 3(b) illustrates the relationship between the three distances, i.e., F kr ij ¼ ls kr ij /C1 sr ; J r ij ¼ dij /C0 e kr ij .

Proposition 1. If the speed time function is a stepwise function, then the travel time function s /C1Þ ð for e i ; j ð Þ ; 8 i ; j 2 C 0 ; i -j is a piecewise-linear function ( Fig. 4 ).

Proposition 1 was stated by Ichoua et al. [29]. The proof process was provided by Pan et al. [10].

According to Proposition 1, the travel time function s /C1Þ ð can be modeled as a piecewise-linear function. To facilitate the understanding of the problem, it is assumed that each e i ; j ð Þ spans at most two time zones. As depicted in Fig. 4, the time zone zr comprises two parts T 2 r /C0 1 ij ¼ w r 2 /C0 1 ij ; w r 2 ij h i and T 2 r ij ¼ w r 2 ij ; w r 2 þ 1 ij h i . Notably, wij 1 ; wij 2 ; wij 3 , . . . are breakpoints. When the vehicle departs from node i within T 2 r /C0 1 ij , the vehicle needs only one time period, so the travel time is a fixed value. Conversely, when the vehicle departs within T 2 r ij , the travel time will change because the vehicle spans multiple periods. Thus, for T 2 r ij , the slope of the function h 2 r ij and its intersection g 2 r ij on the y-axis are, calculated as follows:

$$o _ { i j } ^ { 2 r } = \frac { \tau \left ( w _ { i j } ^ { 2 r + 1 } \right ) - \tau \left ( w _ { i j } ^ { 2 r } \right ) } { w _ { i j } ^ { 2 r + 1 } - w _ { i j } ^ { 2 r } }$$

$$\eta _ { j } ^ { 2 r } = \frac { w _ { j } ^ { 2 r + 1 } \tau \left ( w _ { j j } ^ { 2 r } \right ) - w _ { j j } ^ { 2 r } \tau \left ( w _ { j j } ^ { 2 r + 1 } \right ) } { w _ { j j } ^ { 2 r + 1 } - w _ { j j } ^ { 2 r } }$$

Consequently, if the departure time t kr ij occurs within zr , the travel time is calculated as follows:

$$\tau ( t _ { i j } ^ { \tau } ) & = \sigma _ { i j }, t _ { i j } ^ { \tau } + \eta _ { i j } ^ { \tau }$$

Let x kr ij be 1 if vehicle k departs from node i to j within time zone zr ; otherwise, set it to 0. Therefore, the travel time of e i ; j ð Þ can be computed as follows:

$$t _ { \hat { v } } = \sum _ { k \in V } \sum _ { r = 1 } ^ { p } \tau ( t _ { \hat { v } } ^ { k } ) ^ { k r } _ { \hat { v } }$$

The total duration of vehicles f 1 is given by

$$f _ { 1 } = \min \sum _ { i e c ^ { \prime } } \sum _ { j e c ^ { \prime } i e c ^ { \prime } i e c ^ { \prime } i ( i ) } t _ { i j }$$

## 3.3. Energy consumption function

The MEET model is widely applicable to calculate vehicle carbon emissions [43]. In time zone zr , the carbon emission rate of vehicle k in e i ; j ð Þ is

Fig. 3. The calculation method of node i to j

![Image](image_000005_1cbea9bdffad064042bbbe404f2f153ae7b9b139548e27b76cb1a06ef781205a.png)

![Image](image_000006_787f1871ae0c1f0a446dfe68ab18a98dc676057d04911493806297c958ad89d8.png)

Fig. 4. The duration time function for an arc i ; j ð Þ at any departure time t

![Image](image_000007_80e5f263115eda1f75662c9f0b330c9c4f5446d1fe8d4a1727bd51e3a5021632.png)

$$c _ { i j } ^ { k r } = e \cdot G _ { i j } \cdot \psi _ { i j } ^ { k } / 100 0$$

where e Gij ; ; and w k ij are the carbon emission rate in an environment of no-load, driving speed v and driving slope 0, slope correction factor, and load correction factor, respectively.

$$e = ( 1 1 0 + 0.000375 \nu ^ { 3 } + 8 7 0 2 / \nu )$$

$$G _ { \vartheta } = e ^ { ( 0. 0 0 5 9 \vartheta ^ { 2 } - 0. 0 7 7 5 \vartheta + 1 1. 9 3 6 ) \vartheta _ { \vartheta } }$$

$$\psi _ { i } ^ { k } = 0. 2 7 \cdot r _ { i j } ^ { k } + 1 + 0. 0 6 1 4 \cdot v \cdot r _ { i j } ^ { k } - 0. 0 0 1 1 \cdot v ^ { 3 } \cdot r _ { i j } ^ { k } - 0. 0 0 2 3 5 \cdot r _ { i j } ^ { k } \cdot v - \left ( 1. 3 3 \cdot r _ { i j } ^ { k } \right ) / v$$

wij is the slope of the route i ; j ð Þ ; r k ij is the ratio between the load and capacity on e i ; j ð Þ for vehicle k , and x and # are the perunit cost of fuel consumption and carbon emissions, respectively. The fuel consumption rate of vehicle k on e i ; j ð Þ is calculated as follows:

$$f _ { i j } ^ { ( x ) } = \frac { c _ { i j } ^ { ( x ) } } { 2. 3 2 }$$

Thus, the energy consumption f 2 is given by

$$f _ { 2 } = \sum _ { i \in c ^ { c } } \sum _ { j \in c ^ { c }, \{ i \} } \sum _ { k \in \mathcal { Y } } \sum _ { r \in \{ 1,.. \rho \} } x _ { i j } ^ { k r } e _ { i j } ^ { k r } \left ( \omega \cdot \cdot J _ { i j } ^ { k r } + \vartheta \cdot c _ { i j } ^ { k r } \right )$$

## 3.4. Customer satisfaction function

In a realistic logistic system, customer satisfaction is a critical indicator of dispatch efficiency. To accurately measure customer satisfaction, a rating method is employed to model customer satisfaction as a piecewise function (Fig. 5). Here, time windows eei ; ll i ½ /C138 are further divided into two parts: the preferred time window ei ; l i ½ /C138 and allowable time window eei ; ll i ½ /C138 . In addition, customer satisfaction is partitioned into five levels, le 2 f 0 0 25 0 5 0 75 1 ; : ; : ; : ; g . If a customer is served at his or her

Fig. 5. Customer satisfaction function cs t ð Þ

![Image](image_000008_b73826fb5e6f7d575933d61e3191d63468c8e1a36b8943b96a96bfd0d0301aa1.png)

desired time, the grade of his or her satisfaction is 1; otherwise, the satisfaction level gradually decreases with an increase in the difference between the vehicle's arrival time and the desired time.

There are four situations when the vehicle arrives at customer i at time t . Customer satisfaction csi t ð Þ can be calculated as follows:

$$c s _ { i } ( t ) = \begin{cases} 0, t < e e _ { i } | | t > l l _ { i } \\ 1, e _ { i } \leq t \leq l _ { i } \\ 1 - \frac { 1 } { 4 } \cdot \xi, e _ { i } - l e \cdot \xi < t < e _ { i } - l e \cdot ( \xi - 1 ) \\ 1 - \frac { 1 } { 4 } \cdot \xi, l _ { i } - l e \cdot \xi < t < l _ { i } - l e \cdot ( \xi - 1 ) \end{cases} \\ \intertext { where } l e = \frac { e _ { i } - e e _ { i } } { 3 } \, \text{and } \xi \in \{ 1, 2, 3 \}. \quad. \quad. \ e \. \cdot \ \.$$

where le ¼ e i /C0 ee i 3 and n 2 f 1 2 3 . ; ; g

Therefore, customer satisfaction f 3 is given by

$$f _ { 3 } = \min \frac { 1 } { \sum _ { k \ell V } \sum _ { i \ell C } \sum _ { j \ell C } \sum _ { r \ell \left ( 1, \dots, p \right ) } \text{cs} _ { i } \left ( t _ { j j } ^ { k k } \right ) }$$

## 4. Algorithm description

QMOEA is based on the NSGA-II but it has many advanced features, e.g., two-dimensional (2D) vector encoding, efficient hybrid initialization methods, problem-specific crossover strategies, and Q-learning-based adaptive local search methods, which contribute to its originality and effective performance for solving TDGVRPTW. The framework of QMOEA is presented in Algorithm 1, and all components of this algorithm are introduced in the following subsections.

## 4.1. Framework of QMOEA

QMOEA is proposed using the NSGA-II's algorithm as the main framework. In QMOEA, the environment selection and sorting strategies of NSGAII are directly adopted. The novelties of QMOEA are as follows: (1) the initial strategy is replaced with a hybrid initial strategy to obtain optimal performance solutions, (2) four crossover strategies-two novel Pareto-based heuristics and two effective heuristics-are employed to solve the considered problem, and (3) a Q-learning-based adaptive local search method is proposed to improve local search abilities.

Algorithm 1 describes the framework of QMOEA. The implementation details are as follows: (1) the number of evaluations is set as the termination condition; (2) Ps , Cr , and LSr are the population size, crossover rate, and local search rate,

Fig. 6. A feasible solution

![Image](image_000009_e4c5acd64fc787d69cd7aac6942e59bb5d53850bca7782a6296041157dbae5ce.png)

respectively; (3) the crossover and mutation phase randomly select a strategy; and (4) the approximate PF is generated and updated using a nondominated strategy.

## Algorithm 1

The framework of QMOEA

Input: Ps , Cr ; LSr and the problem instances Otput: PF and PS 1. P Initialization Ps ð Þ (c.f. Subsection 4.3) 2. PF PS ; ½ /C138 UpdatePF P ð Þ 3. While termination conditions not satisfied do 4. O TournamentSelection P ð Þ 5. O Crosso v er O Cr ; ð Þ (c.f. Subsection 4.4) 6. O Qlearning based adapti v e LocalSearch O LSr ; ð Þ (c.f. Subsection 4.5) 7. P En v ironmentalSelection P O ; ½ /C138Þ ð (c.f. [37]) 8. PF PS ; ½ /C138 UpdatePF P ð Þ 9. End while 10. Return PF and PS

## 4.2. Encoding and decoding

Each feasible solution can be encoded into a 2D vector. One vector L 1 , containing N integers, and where N is the number of customers, represents the customer service sequence. Notably, the first vector is divided according to the criteria of the vehicle capacity constraint and the closing time of the depot constraint. The scheduling sequence of each vehicle is also specified in the encoding representation in the second vector. The other vector L 2 , which has the same length as the first, shows the ownership of the customer and vehicle. More explicitly, start and stop nodes are not added to the chromosome to accelerate the crossover and mutation operation. Fig. 6 represents a solution representation example, where there are nine customers and three vehicles. Clearly, three routes are constructed, denoted as r 1 ¼ f 3 5 9 , ; ; g r 2 ¼ f 7 1 6 , and ; ; g r 3 ¼ f 8 2 4 . ; ; g

## Algorithm 2

The calculation process of travel time

| Input: The departure time t kr ij and the distance dij   | Input: The departure time t kr ij and the distance dij                                              |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Output: The duration time t ij                           | Output: The duration time t ij                                                                      |
| 1                                                        | If F ij kr /C21 dij do/* edge i ; j ð Þ just needs one time zone */                                 |
| 2                                                        | J r ij ¼ 0 ; e kr ij ¼ dij ; t ij ¼ dij sr ; /* update travel time */                               |
| 3                                                        | Else                                                                                                |
| 4                                                        | J r ij ¼ dij /C0 F ij kr ; e kr ij ¼ dij ; t ij þ ¼ ls kr ij /* calculate the remaining distance */ |
| 5                                                        | n ¼ 1                                                                                               |
| 6                                                        | While 1 do/* edge i ; j ð Þ needs to span multiple time zones */                                    |
| 7                                                        | F k r ; þ n ij ¼ s r þ n s                                                                          |
| 8                                                        | If F k r ; þ n ij /C20 J r þ /C0 n 1 do                                                             |
| 9                                                        | J r þ n ij ¼ J r ij /C0 F k r ; þ n ij ; e k r ; þ n ij ¼ F k r ; þ n ij ; t ij þ ¼ s n ; ¼ n þ 1;  |
| 10                                                       | else                                                                                                |
| 11                                                       | J r þ n ij ¼ 0; e k r ; þ n ij ¼ J r þ /C0 n 1 ij ; t ij þ ¼ J r þ /C0 n 1 ij = s r þ n ;break;     |
| 12                                                       | End if                                                                                              |
| 13                                                       | End while                                                                                           |
| 14                                                       | End if                                                                                              |
| 15                                                       | Return t ij                                                                                         |

The decoding process is common, where each objective is calculated according to the scheduling sequence of each vehicle. The transportation time t ij from node i to j can be calculated using Algorithm 2.

## 4.3. Initialization phase

In a combinatorial optimization problem, an initial population with a high level of quality and diversity always gains a satisfying result in the competitive levels of performance. To generate initial solutions with optimal performance, a hybrid initial strategy, including four problem-specific heuristics-random method, k-nearest neighbor heuristic (k-NNH), improved push-forward insertion heuristic (IPFIH), and earliest preferred time heuristic (EPH)-is embedded. Let m denote the initial population size; the details are given as follows:

- 1) Generate m = 4 feasible solutions using the random method.
- 2) Generate m = 4 feasible solutions using the k-NNH.

Considering the situation of customer clustering, k-NNH is employed. When customer i is served by vehicle k , the next customer is randomly selected from the k-nearest neighbors of the current customer i . Then, running k-NNH several times will obtain several different initial solutions. Clearly, the value of k determines the degree of diverseness. In this study, k is set to 1/5 of the total number of customers. The time complexity of the k-NNH is O kN 2 /C16 /C17 :

## 3) Generate m = 4 feasible solutions by the IPFIH.

The push-forward insertion heuristic was proposed by Solomon [22], and it is an efficient constructive heuristic for the VRPTW. As such, a customer's time window and geographical location are considered simultaneously. The IPFIH procedure is illustrated in Algorithm 3. The first customer, customer i , is selected by Eq. (14), where d i 0 denotes the distance between the depot and customer i , ll i is the latest allowable time window of customer i , and pi denotes the polar coordinate angle of customer i for the depot. Notably, the larger the value of h is, the higher the probability that the customer will be selected. Motivated by k-NNH, the next customer to be served is randomly selected from the k -largest h obtained from Eq. (15), where p 1 is the polar coordinate angle of the first customer. Running this procedure several times will yield different initial solutions. The time complexity of IPFIH is O kn 2 /C0 /C1 :

$$h _ { i } = - \alpha d _ { 0 } + \beta u _ { i } + \gamma \left ( \left ( \frac { | p _ { i } | } { 3 6 0 } \right ) \right )$$

$$h _ { j } = - \alpha d _ { j } + \beta \lambda _ { j } + \gamma \left ( \left ( \frac { | p _ { j } - p _ { 1 } | } { 3 6 0 } \right ) \right )$$

According to the extensive computational tests of Solomon, a b , , and c are set to 0.7, 0.1, and 0.2, respectively, which indicates better performance.

## Algorithm 3

The procedure of improved PFIH

| Input: Customer information   |                                                                              |
|-------------------------------|------------------------------------------------------------------------------|
| Output: Initial solution s    |                                                                              |
| 1                             | Select the first customer i from C by Eq. (14)                               |
| 2                             | Update the load of vehicle v                                                 |
| 3                             | Get the set of customers F ¼ C = i f g that are not served                   |
| 4                             | While length F ð Þ > 0 do                                                    |
| 5                             | Calculate evaluation values H ¼ f h 1 ; h 2 ; /C1 /C1 /C1g for F by Eq. (15) |
| 6                             | Find the k -largest value in H , denoted as P k                              |
| 7                             | Randomly select customer j from P k                                          |
| 8                             | If constraint C 1or C 2 not satisfied do                                     |
| 9                             | Update 2D vector with the customer visited by vehicle s v                    |
| 10                            | Increase new vehicle v                                                       |
| 11                            | Select the first customer i from F by Eq. (14) and update F ¼ F = i f g      |
| 12                            | Update the load of vehicle v                                                 |
| 13                            | Else                                                                         |
| 14                            | Update F ¼ F = j f g                                                         |
| 15                            | Update the load and travel time of vehicle v                                 |
| 16                            | End if                                                                       |
| 17                            | End while                                                                    |
| 18                            | Return s                                                                     |

## Algorithm 4

## Procedure of EPH

| Input: Customer information               | Input: Customer information                                                    |
|-------------------------------------------|--------------------------------------------------------------------------------|
| Output: Initial solution s                | Output: Initial solution s                                                     |
| 1. Get a random sequence F of N customers | 1. Get a random sequence F of N customers                                      |
| 2. For i = 1to N                          | 2. For i = 1to N                                                               |
| 3                                         | Insert the customer F i ð Þ into P by order of earliest preferred time windows |
| 4                                         | If constraints C 1and C 2 are satisfied do                                     |
| 5                                         | Update the load and travel time of the vehicle v                               |
| 6                                         | Else                                                                           |
| 7                                         | Update 2D vector with the customer s P visited by vehicle v                    |
| 8                                         | Increase new vehicle v and empty P                                             |
| 9                                         | End                                                                            |
| 10                                        | End For                                                                        |
| 11                                        | Return s                                                                       |

## 4) Generate m = 4 feasible solutions using EPH.

To solve the problem of customers with a narrow time window, EPH is designed. The details are as follows. First, a random sequence F , including all customers, is generated. Then, customer i in F is successively inserted into current vehicle v . In particular, the customers P in current vehicle v are sorted in the nonincreasing order of the earliest preferred time window. Finally, repeat until all customers in F are visited. The EPH procedure is represented in Algorithm 4, and the time complexity is O N 2 /C16 /C17 .

## 4.4. Problem-specific crossover phase

For multiobjective optimization, two novel crossovers-similar customer order crossover (SCOX) and customer block order crossover (CBOX) -are proposed to enhance the ability of the algorithm in terms of exploiting the information on the nondominated solution set. In addition, the best cost-best route crossover (BCBRC) [44] and improved PTL (IPTL) [45] are adopted to explore the solution space and to prevent the algorithm from searching in poor regions of the solution space. The procedure of the four crossover methods is described as follows:

SCOX : First, a temporary route is constructed by customers who appear the most frequently in each position of each route from PS . Then, two parents are randomly selected from the population, and the position of the customers in each route of each parent is compared with the temporary route. If they are the same, the customer is put in the same locus as its offspring. Finally, the empty location of offspring is filled by the order of customers in the other parent. The entire procedure of SCOX is depicted in Fig. 7.

In the following, an example is illustrated to explain the procedure of SCOX. Let p i ; i ¼ 1 2 3 be nondominated solutions ; ; in PS , with each containing nine customers. The solutions are listed as follows:

Fig. 7. The SCOX procedure

![Image](image_000010_0ee4eefd793abeee03d106bc39926537ec94f617cf4e685afaa8f72fb20f604d.png)

p 1 /C138g p 2 ¼ f½ 3 1 7 ; ; /C138 ; 5 8 2 ; ; ½ /C138 ; 6 4 9 ; ; ½ /C138g

¼ f½ 9 6 7 ; ; /C138 ; 2 4 ; ½ /C138 ; 3 5 1 8 ; ; ; ½

p 3 ¼ f½ 1 4 2 7 ; ; ; /C138 ; 3 6 5 8 ; ; ; ½ /C138 ; 9 ½ /C138g :

Step 1: Count the maximum route length among PSs as the size of the temporary route. In this case, the length of the temporary route is 4.

Step 2: Find the customer with the maximum occurrence number at each position k k ¼ 1 2 ; ; /C1 /C1 /C1 ; 4 . For example, the ð Þ times that each customer i i ¼ 1 2 ; ; /C1 /C1 /C1 ; 9 ð Þ appears at position 1 are 1, 1, 3, 0, 1, 1, 0, 0, and 2, respectively. Similarly, the times that each customer i appears at positions 2, 3, and 4 are, respectively, as follows:

Position 2: 1, 0, 0, 3, 1, 2, 0, 1, and 0

Position 3: 1, 2, 0, 0, 1, 0, 2, 0, and 1

Position 4: 0, 0, 0, 0, 0, 0, 1, 2, and 0

Then, customers with the highest frequency in each location are selected to form a temporary route 3 4 2 8 . The time ; ; ; ð Þ complexity of such an operation is O PS j j /C3 N 2 /C16 /C17 .

Subsequently, two parents are randomly selected from the population, i.e., p 1 ¼ f½ 3 1 9 8 ; ; ; /C138 ; 2 4 7 ; ; ½ /C138 ; 5 6 ; ½ /C138g and p 2 ¼ f½ 5 1 2 ; ; /C138 ; 3 6 ; ½ /C138 ; 8 4 7 9 ; ; ; ½ /C138g . Then, each route of the parents is compared with the temporary route to find customers in the same location and keep them there. Thus, the location of customers 3, 4, and 8 in parent 1 is reserved for offspring 1. Similarly, the location of customers 3, 4, and 2 in parent 2 is reserved for offspring 2.

Finally, the blank positions of offspring 1 are filled, using the customers in parent 2 who are not present in offspring 1. Thus, customers 5 1 2 6 7 9 ; ; ; ; ; ð Þ are placed in offspring 1 in order. Similarly, offspring 2 1 9 2 3 8 7 4 5 6 ; ; ; ; ; ; ; ; ð Þ is generated.

CBOX : First, a temporary set is constructed by customer pairs who appear the most frequently in PS . Then, two parents are randomly selected from the population, and the same customer pairs, comparing the parent with the temporary set, are reserved for the offspring. Finally, the blank positions of the offspring are filled with customers who are not duplicated. The entire procedure is illustrated in Fig. 8.

In the following, the same example p i ; i 2 f 1 2 3 ; ; g is employed to explain the procedure of CBOX.

Step 1: Build a temporary set. First, the times of customer pairs are calculated in each subroute of PS . For instance, the times that customer 1 is served before other customers are 0, 0, 0, 1, 0, 0, 1, 1 and 0, respectively. Thus, (1, 4) is the first customer pair in the temporary set. Similarly, the times that customers 2-9 are served before other customers can be calculated as follows:

Customer 2: 0, 0, 0, 1, 0, 0, 1, 0, and 0

Customer 3: 1, 0, 0, 0, 1, 1, 0, 0, and 0

Customer 4: 0, 1, 0, 0, 0, 0, 0, 0, and 1

Customer 5: 1, 0, 0, 0, 0, 0, 0, 2, and 0

Customer 6: 0, 0, 0, 1, 1, 0, 0, 0, and 0

Customer 7: 0, 0, 0, 0, 0, 0, 0, 0, and 0

Customer 8: 0, 1, 0, 0, 0, 0, 0, 0, and 0

Customer 9: 0, 0, 0, 0, 0, 1, 0, 0, and 0

Finally, the temporary set {(1, 4), (2, 7), (3, 1), (4, 2), (5, 8), (6, 5), (8, 2), (9, 6)} is constructed. The time complexity of such an operation is O PS j j /C3 N 2 /C16 /C17 .

Step 2: Two parents are selected randomly, i.e., P 1 ¼ f½ 3 1 9 6 ; ; ; /C138 ; 2 4 7 ; ; ½ /C138 ; 5 8 ; ½ /C138g and P 2 ¼ f½ 5 6 4 ; ; /C138 ; 3 1 ; ½ /C138 ; 8 2 7 9 ; ; ; ½ /C138g . Compared with the temporary set, the same customer pairs are found in the parent and put into the offspring. Therefore, cus-

Fig. 8. The CBOX procedure

![Image](image_000011_04815a49258dae6d6d2773f987e61202164737b17dbb488c63547654cf1ec73e.png)

Fig. 10. The BCBRC procedure

![Image](image_000012_9a557613171674e89d9242a1bd9e008a12263a384d685780329cf254f3106c9f.png)

tomer pairs (3, 1) and (5, 8) in parent P1 are retained in offspring 1. Similarly, customer pairs (3, 1) and (8, 2) in parent P2 are retained in offspring 2. Then, similar to SCOX, the other customers are filled in the empty position in the offspring.

IPTL : The IPTL method preserves a part of the parent information. First, offspring 1 and 2 have all the sequences of parents 1 and 2, respectively. Next, two points are randomly selected from parents 1 and 2, and then, parts of the two points are intercepted and copied to the front of the offspring. Finally, duplicated customers are deleted to maintain customer uniqueness. Fig. 9 shows the whole procedure of IPTL.

BCBRC : The BCBRC can reduce the total duration time and NV simultaneously. The minimum duration time route in each parent is reserved as C 1 and C 2 . Then, the reserved customers are removed from another parent. As shown in Fig. 10, the

189

![Image](image_000013_94298680c8f58f8386d3b3c620027f6bc4bf4ca2b92bca9f109665904b7bf0a0.png)

customers C 1 ¼ f 2 4 7 ; ; g are removed from P 2 . Finally, the removed customers are inserted in the position with the minimum duration.

## 4.5. Local intensification phase

The selection of heuristics in the local intensification phase determines the algorithm's performance. In this study, the Qlearning algorithm acts as an agent to make transitions from one heuristic to another in a rational and guided manner. In addition, the state and action are defined as the selection of local search operators.

## 4.5.1. Q-learning-based adaptive local search method

The Q-learning algorithm is a type of reinforcement learning (RL) [46] that uses a Q table to record the relationship between the state and action. As such, heuristics can be used collaboratively, and the number of actions is constant and independent of the problem's size. The Q-learning-based local search strategy is dedicated to selecting the optimal state-action pair based on knowledge during the iterative process. In the iterative process, the current action at is selected according to Algorithm 5 in state st . Then, the reward r t þ 1 is obtained by Eq. (17). Meanwhile, the environment changes to state st þ 1 . Finally, for the new state, the agent updates Q st ; at ð Þ by selecting the maximum value of Q st ; a ð Þ using the following:

$$Q ( s _ { t }, a _ { t } ) = ( 1 - \alpha ) \cdot Q ( s _ { t }, a _ { t } ) + \alpha \cdot ( r _ { t + 1 } + \gamma \cdot \max Q ( s _ { t + 1 }, a ) )$$

where t ; a c ; ; r ; s ; and a are the current time, learning rate, discount factor, reward, state, and action, respectively.

Motivated by the Markov decision process, each local search operator is considered an action, which is described in Subsection 4.6.2. In addition, a novel method, considering the relative reduction between objectives, is designed to calculate the reward, which is described as follows:

$$r = \sum _ { i = 1 } ^ { n } \frac { p _ { i } - o _ { i } } { p _ { i } }$$

where pi and oi are the ith objective values of the parent and offspring, respectively. Clearly, the larger the reward is, the better the strategy being selected. As such, multiple objectives are considered simultaneously in the same priority. If the reward is negative, the contribution is small, and the Q value will decline; otherwise, the Q value will increase, which means that the probability of the strategy being selected next time will increase. As shown in Fig. 11, for the minimization problem with three objectives, P ¼ ð 6 8 5 ; ; Þ and O ¼ ð 4 5 8 ; ; Þ are the objective value sets of the parent and offspring, respectively. Then, the reward is estimated to be 0.11, using Eq. (17).

In addition, the e -greedy strategy is employed in action selection to provide a certain degree of randomness and prevent falling into a local optimum, as depicted in Algorithm 5. The method has an e probability of randomly selecting an action and an 1 /C0 e probability of adopting the Q table to obtain the optimal action in the current state (Line 4 of Algorithm 5).

## Algorithm 5

e -greedy function

```
    Input: Q table Q, currentState s
    Output: action a
    1. n = length(Q)/* get the number of local search operators */
    2. If rand() < & do
    3.     action = ceil(rand() · n)/* randomly select a local search operator*/
    4. Else
    5.     action = getActionbyQ(s)
    6. End if
    7. Return action
```

As described in Algorithm 6, a novel local search method integrates Q-learning, the multiobjective reward, and the e -greedy method, which is called the Q-learning-based adaptive local search. The details are as follows: (1) the Q table is initialized with a random number between 0 and 0.1; (2) four neighborhood operators are selected via Q-learning, which are described in Subsection 4.5.2; (3) Eq. (17) is adopted to judge whether the current solution is adopted, considering constraints ( C 1 /C0 C 2 ); (4) as the number of iterations increases, e is decreased by decayrate times.

## Algorithm 6

The Q-learning-based adaptive local search

| Input : population P , population size Ps ; and local search rate LSr   | Input : population P , population size Ps ; and local search rate LSr                          |
|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| Output : population O                                                   | Output : population O                                                                          |
| 1                                                                       | Q Global QTable : /* get Q value from global variable*/                                        |
| 2                                                                       | state Global currentState : /* get the state of the current environment*/                      |
| 3                                                                       | For i ¼ 1 to PS do                                                                             |
| 4                                                                       | If rand ðÞ < LSr do                                                                            |
| 5                                                                       | action   /C0 e greedy Q ; state ð Þ                                                            |
| 6                                                                       | Switch( action )                                                                               |
| 7                                                                       | case 1: O i ð Þ   Internal 2 /C0 opt P i ð ÞÞ ð                                                |
| 8                                                                       | case 2: O i ð Þ   Internal or /C0 opt P i ð ÞÞ ð                                               |
| 9                                                                       | case 3: O i ð Þ   External exchange P i ð ÞÞ ð                                                 |
| 10                                                                      | case 4: O i ð Þ   External 2 /C0 opt /C3 P i ð ÞÞ ð                                            |
| 11                                                                      | case 5: O i ð Þ   External relocate P i ð ÞÞ ð                                                 |
| 12                                                                      | End                                                                                            |
| 13                                                                      | reward ¼ getReward Oi ; Pi ð Þ /* the reward is calculated by Formula (17) */                  |
| 14                                                                      | updateQ v alue Q ; state ; action reward ; ð Þ /* the value of Q is updated by Formula (16) */ |
| 15                                                                      | state ¼ action /* update the current environment state */                                      |
| 16                                                                      | End if                                                                                         |
| 17                                                                      | End for                                                                                        |
| 18                                                                      | e ¼ e /C3 decayrate                                                                            |
| 19                                                                      | Global state : ¼ state                                                                         |
| 20                                                                      | Global QTable : ¼ Q                                                                            |
| 21                                                                      | Return O                                                                                       |

## 4.5.2. Local search operators

To explore the solution space and accelerate convergence, multiple well-known local search operators are employed to increase the opportunity of jumping out of local optima, which are presented as follows. Among them, the exploration depth of each local search operator is set to I , to control the intensity of the local search and reduce the dependence on time. The functions of I are as follows: 1) to control the number of route combinations that need to be adjusted and 2) to control the

Fig. 12. Neighborhood functions

![Image](image_000014_7f5c77c6baa2d8bb6a9909d7a7bcf4d42e6e50a080541f35d03a5527cb050a08.png)

number of relocations that each customer needs to adjust. Therefore, the time complexity of each search operation does not exceed O I /C3 N 2 /C16 /C17 .

Internal 2-opt aims to tackle a situation in which the vehicle routing distance is longer because of the intersection of vehicle routes. As shown in Fig. 12(a), r ¼ ½ 0 ; 2 ; 5 ; 4 ; 9 ; 0 /C138 is the route of a vehicle. Then, the edges (2, 5) and (4, 9) are replaced by edges (2, 4) and (5, 9). Thus, r 0 ¼ ½ 0 ; 2 ; 5 ; 4 ; 9 ; 0 /C138 is obtained.

Internal or-opt changes the connection of three edges to obtain an optimal solution. As shown in Fig.12(b), the edges (2, 8), (3, 4), and (5, 9) are replaced by edges (2, 4), (5, 8), and (3, 9); thus r 0 1 ¼ ½ 0 ; 2 ; 4 ; 5 ; 8 ; 3 ; 9 ; 0 /C138 is obtained. Notably, the service order of customers 3 and 8 is always the same.

External exchange focuses on different routes and then swaps the customer's location. As shown in Fig. 12(c), two routes in a solution are selected randomly, i.e., r 1 ¼ ½ 0 ; 2 ; 6 ; 7 ; 0 and /C138 r 2 ¼ ½ 0 ; 9 ; 4 ; 5 ; 0 , and then, customers 4 and 6 are selected to /C138 exchange. Thus, new routes r 0 1 ¼ ½ 0 ; 2 ; 4 ; 7 ; 0 /C138 and r 0 2 ¼ ½ 0 ; 9 ; 6 ; 5 ; 0 /C138 are obtained.

External 2-opt* originates from internal 2-opt, which is introduced to modify two edges in different routes. As shown in Fig. 12(d), edges (2, 5) and (9, 4) from different routes are replaced by edges (2,4) and (9,5). Then, new routes r 0 1 ¼ ½ 0 ; 2 ; 4 ; 0 /C138 and r 0 2 ¼ ½ 0 ; 9 ; 5 ; 0 /C138 are obtained.

External relocate selects a customer and then moves the customer from one route to another. As shown in Fig. 12(e), r 1 ¼ ½ 0 ; 2 ; 6 ; 7 ; 0 /C138 and r 2 ¼ ½ 0 ; 9 ; 5 ; 0 /C138 are two randomly selected routes. Customer 6 is moved from r 1 to r 2 ; thus, r 0 1 ¼ ½ 0 ; 2 ; 7 ; 0 /C138 and r 0 2 ¼ ½ 0 ; 9 ; 6 ; 5 ; 0 /C138 are obtained.

## 5. Experimental design

In this section, standard benchmark instances and specific problem instances are described to verify the proposed algorithm. Parameter tuning is then performed to improve the algorithm's performance. Next, various associated experiments are designed to prove the effectiveness of the proposed strategy. Afterward, the algorithm is compared with other MOEAs.

All algorithms were implemented using MATLAB R2019a on an evolutionary multiobjective optimization platform [47], and all tests were performed on a laptop computer with an Intel (R) Core (TM) i5-6300HQ CPU @2.30 GHz with 16-GB RAM.

## 5.1. Experimental instances

Solomon [22] classified 56 instances into three categories according to the distribution of customers, including C-the clustering of customers, R-the random distribution of customers, and RC-the combination of C and R. Furthermore, according to the time window, each category was subdivided into short and long scheduling horizons, which were labeled as 1 and 2, respectively.

The TDGVRPTW instances were generated based on Solomon instances and the Dabia dataset[48]. Each instance was denoted as t k s , where t is the three-category distribution, i.e., t 2 f C ; R ; RC g , k is the type of window k 2 f 1 ; 2 , and g s is the number of customers, i.e., s 2 f 25 ; 50 ; 100 , yielding 168 instances in total. The number of customers, planning horizon, g vehicle capacity, and service time for the categories were obtained from the literature (Table 1) [9]. Table 2 shows the speed profiles of each time zone. The planning horizon l 0 was divided into five time zones. Moreover, roads comprised three different characteristic speeds of fast, medium, and slow. The slope value was generated by a uniform U 0 1 ; ½ /C138 distribution. In addition, the coefficients x and # of the carbon emission and fuel consumption model were set to 7.5 and 0.06, respectively. For the computational tests shown here, a c e ; ; , and decayrate are set to 0.1, 0.9, 0.8 and 0.999, respectively, in Eq. (16). In the local search, the depth I of the local search was set to 10 following the literature [11].

## 5.2. Evaluation criteria

In this study, real and approximate PFs were employed to refer to the global optimal solution and the solution obtained by an algorithm satisfying the termination condition, respectively. The final PF is defined as an approximation to the global optimal solution. However, the acquisition of the global optimal solution requires a rigorous mathematical proof. Since, this study is the first attempt to solve the TDGVRPTW, there is no existing real PF that can be used. To address this issue, all algorithms are replicated 10 times independently, with the maximum number of evaluations set to 20,000 for each instance.

Table 1 Instance information.

| Instances class   | Customer number   | Planning horizon   |   Capacity |   Service time |
|-------------------|-------------------|--------------------|------------|----------------|
| C1                | 25/50/100         | [0, 12360]         |        200 |            900 |
| C2                | 25/50/100         | [0, 33900]         |        700 |            900 |
| R1                | 25/50/100         | [0, 2300]          |        200 |            100 |
| R2                | 25/50/100         | [0, 10000]         |       1000 |            100 |
| RC1               | 25/50/100         | [0, 2400]          |        200 |            100 |
| RC2               | 25/50/100         | [0, 9600]          |       1000 |            100 |

The speed profiles of each time zones.

(a) C101\_100

|        | Zone1        | Zone2              | Zone3             | Zone4          | Zone5   |
|--------|--------------|--------------------|-------------------|----------------|---------|
| Period | [0,0.2 l 0 ] | [0.2 l 0, 0.3 l 0] | [0.3 l 0,0.7 l 0] | [0.7 l 0,0.8 l |         |
| Fast   | 1.5          | 1                  | 1.67              | 1.17           | 1.33    |
| Normal | 1.17         | 0.67               | 1.33              | 0.83           | 1       |
| Slow   | 1            | 0.33               | 0.67              | 0.5            | 0.83    |

![Image](image_000015_c45e8499408f499bd8a4d8f42a8d8d050e93db800c9b818e1c31f10ea5ab64d8.png)

![Image](image_000016_64c11b4ca122f46cf0358a3a36ba29ec990235be800d881cd8f7f147bec77954.png)

Parameter levels for Cr and LSr .

| Parameter   |   Values |   Values |   Values |   Values |   Values |   Values |
|-------------|----------|----------|----------|----------|----------|----------|
|             |     1    |      2   |     3    |      4   |     5    |      6   |
| Cr          |     0.05 |      0.1 |     0.15 |      0.2 |     0.25 |      0.3 |
| LSr         |     0    |      0.1 |     0.3  |      0.5 |     0.7  |      0.9 |

![Image](image_000017_9e18e0649ebe7406d803eb8b4ca7770d44c486a2e55cc77821c1cb094d0d9c83.png)

![Image](image_000018_1f8efa40eda8ab57db379a0d80de177c7e1e8d45af891aa39159957cfb0da038.png)

(b) 95% Confidence interval for LSr

![Image](image_000019_edd840096a7c4137c8d2065d9cae6b00be302581229051c6419d81c5353107f8.png)

Instances

Fig. 16. Initial PF of C102\_100 obtained using different algorithms

![Image](image_000020_930c0d63cced1d140add155da1fbec68a5c7b51cc4c8d3fab87aa0a0212278af.png)

Computational results of QMOEA-SC and QMOEA-NSC.

| Instances   | HV       | HV        | IGD      | IGD       |
|-------------|----------|-----------|----------|-----------|
| Instances   | QMOEA-SC | QMOEA-NSC | QMOEA-SC | QMOEA-NSC |
| C1_25       | 7        | 2         | 6        | 3         |
| C2_25       | 2        | 6         | 3        | 5         |
| R1_25       | 10       | 2         | 10       | 2         |
| R2_25       | 6        | 5         | 5        | 6         |
| RC1_25      | 7        | 1         | 7        | 1         |
| RC2_25      | 3        | 5         | 1        | 7         |
| C1_50       | 7        | 2         | 9        | 0         |
| C2_50       | 4        | 4         | 3        | 5         |
| R1_50       | 9        | 3         | 11       | 1         |
| R2_50       | 7        | 4         | 6        | 5         |
| RC1_50      | 5        | 3         | 6        | 2         |
| RC2_50      | 2        | 6         | 5        | 3         |
| C1_100      | 6        | 3         | 6        | 3         |
| C2_100      | 5        | 3         | 8        | 0         |
| R1_100      | 6        | 6         | 8        | 4         |
| R2_100      | 5        | 6         | 6        | 5         |
| RC1_100     | 8        | 0         | 7        | 1         |
| RC2_100     | 6        | 2         | 8        | 0         |
| TOTAL       | 105      | 63        | 115      | 53        |

Computational results of QMOEA-NQ and QMOEA.

HVcorrelation analysis

| Instances   | HV       | HV       | IGD      | IGD      |
|-------------|----------|----------|----------|----------|
|             | QMOEA-Q  | QMOEA-NQ | QMOEA-Q  | QMOEA-NQ |
| C101_25     | 7.62E-01 | 7.72E-01 | 1.39E-02 | 1.22E-02 |
| C201_25     | 8.37E-01 | 7.84E-01 | 3.11E-02 | 3.99E-02 |
| R107_25     | 7.25E-01 | 7.15E-01 | 1.04E-02 | 1.71E-02 |
| R207_25     | 7.27E-01 | 7.24E-01 | 7.59E-03 | 7.07E-03 |
| RC107_25    | 7.02E-01 | 6.81E-01 | 8.31E-03 | 2.67E-02 |
| RC207_25    | 7.72E-01 | 7.70E-01 | 8.15E-03 | 1.12E-02 |
| C101_50     | 7.48E-01 | 7.24E-01 | 8.12E-03 | 1.68E-02 |
| C202_50     | 8.02E-01 | 7.76E-01 | 8.11E-03 | 2.39E-02 |
| R107_50     | 7.37E-01 | 7.13E-01 | 6.11E-03 | 1.21E-02 |
| R210_50     | 6.94E-01 | 6.63E-01 | 5.37E-03 | 2.67E-02 |
| RC107_50    | 7.76E-01 | 7.67E-01 | 6.13E-03 | 9.72E-03 |
| RC207_50    | 7.42E-01 | 6.97E-01 | 5.70E-03 | 2.43E-02 |
| C101_100    | 6.92E-01 | 6.65E-01 | 1.69E-02 | 2.91E-02 |
| C205_100    | 7.43E-01 | 7.14E-01 | 1.42E-02 | 5.09E-02 |
| R107_100    | 6.93E-01 | 6.73E-01 | 1.93E-02 | 3.17E-02 |
| R210_100    | 7.75E-01 | 7.55E-01 | 1.00E-02 | 4.39E-02 |
| RC101_100   | 7.64E-01 | 7.47E-01 | 1.32E-02 | 2.72E-02 |
| RC207_100   | 7.69E-01 | 7.40E-01 | 1.09E-02 | 2.71E-02 |

![Image](image_000021_3640971f4e05be7585d90987c256a9b7702ac92b6c1eecea081a78b12ff467ed.png)

![Image](image_000022_be220a9e9844546f922ca3ca5e52d81052201de2849223b9f9db609b8c2db09e.png)

Then, the approximate PF of all algorithms is merged to obtain the final PF of each instance [11,40,49]. Fig. 13 depicts the final PF of C101\_100 and R101\_100. The experimental results can be obtained from the website: http://ischedulings.com/ TDGVRPTW.zip.

The performance indicators used in this study are as follows.

The hypervolume (HV) was employed as an indicator to evaluate an algorithm's performance, which was calculated based on the normalized objective value under the reference point 3 1 : ; 3 1 : ; 3 1 . Thus, the convergence and diversity of the solu-: ½ /C138 tion are measured by the area bounded by the reference point and PF . The higher the HV value is, the better the solution.

The inverted generational distance (IGD) indicator was introduced to measure the distance between the approximate and final PFs . A lower IGD value indicates that the approximate optimal solutions are closer to the final PF .

The relative percentage increase (RPI) was also designed to analyze all data of the compared algorithms in the same instance. The RPI value was calculated as follows:

$$R P l = \frac { D _ { c } - D _ { b } } { D _ { b } }$$

where Dc is the value of either 1-HV or IGD obtained from the compared algorithms and Db is the best value of either 1-HV or IGD. Similar to HV, the lower the RPI value is, the better the approximate optimal solution.

![Image](image_000023_f2dd19560abf931a8a1a223388d6c67f003c6376c2c609e8d099065cefba031a.png)

![Image](image_000024_78b1d7f8f74ce05ddc654844c306dad3ca51bcb51abd14f9dc02f536305f4efc.png)

![Image](image_000025_7a8309003e4d83e5088e41a0ff12c1811e3892ed4a6fc07b9d8f5b5dfa87a37f.png)

![Image](image_000026_d2bf67845e24919c977b2afab9fccf06643ef55fafd9b4aac820d195e49bb61d.png)

(c) MOEA/D

![Image](image_000027_82fbef7e498455d368c29f33dbdd36451d7712d15d29a29e4d66d06d526e6c37.png)

(d) MaOEACSS

![Image](image_000028_e2388d1cbfa677d09318806eae98d08fc9f30404368b4851f87da8b30c98c516.png)

(e) RPDNSGAII

![Image](image_000029_d23af87b85bf3f6fd973e90f94d4edbdb5296c130cf0b356913d65909825f047.png)

Fig. 18. The parallel coordinates of the final solution sets on the C106\_50 instance.

![Image](image_000030_de2e25c15378c4b242769b6c91f06e153b57f418f4de9682dc8588186a31dad7.png)

![Image](image_000031_089c5be10a548e84c5c01229b2212124a93423fec2ed77fb0e619f9d0f74c549.png)

(a The final PF

![Image](image_000032_0b9c143470f50c5f0507e2f3d40129daab64cacade37087603731932888e0d5e.png)

b) QMOEA

![Image](image_000033_3a0b5102b6f22a4bc4b8d7d82dfcdc4fcbdc148bc99446f88c626bd33c8a9ec8.png)

MaOEACSS

![Image](image_000034_15f1c3f7289e95cd7480c67b2db8e92c6788e83ee3a8e8b046cbc2ee3ca9611c.png)

RPDNSGAII

1-HV

![Image](image_000035_8f60fee8aea1cb8f5c44eeb392282a6682fd240d7ff9d32fc8042d729d151d54.png)

![Image](image_000036_8fdc3e58f78403cfa89ba1c34b888f5e3ac736d6ab346141aa216143ad96f802.png)

## IGD

Fig. 20. Box plots of IGD and HV with different customer groups in five different algorithms

![Image](image_000037_a6f555ff52bad226c76996f372c5624aa6f6e0580d931c1e5eeb32ec4b069668.png)

![Image](image_000038_b8598a7e58d0d694fbea121a49609db391e09e6acba6de6afd13368f0f3d8467.png)

![Image](image_000039_be825dcb6d42ff291ddcf973b8f82db7d2ef389ee1e27f3751b6fbc67ad4d8c5.png)

![Image](image_000040_a5572edacf6e4e0513250a788d1605aac73dc2e8b4cc346345fb87108c166193.png)

## 5.3. Parameter tuning

To investigate the effect of key parameters, i.e., crossover rate Cr and local search rate LSr , the parameters of QMOEA were calibrated via the design of experiments, which was performed with the maximum number of evaluations set to 10,000. For instance, with 25, 50, and 100 customers, population size PS is set to 30, 50, and 100, respectively. The levels for the two parameters are listed in Table 3.

A full factorial design was developed, where all possible combinations of the above factors resulted in a total of 6 /C2 6 ¼ 36 different configurations. Six instances were randomly selected from each of C1, C2, R1, R2, RC1, and RC2. A total of 36 instances were arranged to calibrate QMOEA. Subsequently, five independent replications were performed for each instance. Finally, the 95% confidence intervals of the HV values of parameters Cr and LSr were obtained (Fig. 14).

Fig. 14 (a) shows that the optimal HV value was reached with Cr ¼ 0 15. From this point, the performance of the algorithm : gradually decreased as Cr increased. Similarly, for the local search level [Fig. 14(b)], the optimal value can be obtained in terms of stability or average value when LSr ¼ 0 5. : Thus, the optimal parameter combination was obtained, i.e., Cr ¼ 0 15 : ; and LSr ¼ 0 5. :

Table 6 Comparisons of the HV and IGD values for 50 customers.

| Ins               | HV            | IGD           | IGD           | IGD           | IGD           | IGD                                 | IGD               | IGD               | IGD               |
|-------------------|---------------|---------------|---------------|---------------|---------------|-------------------------------------|-------------------|-------------------|-------------------|
| Ins               | MOEA/D        | MaOEACSS      | QMOEA         | RPDNSGAII     | hpaEA         | MOEA/D                              | MaOEACSS QMOEA    | RPDNSGAII         | hpaEA             |
| C101_50           | 0.7045        | 0.5501        | 0.7577        | 0.6962        | 0.6485        | 4.72E-02 1.76E-01                   | 1.12E-02          | 5.45E-02          | 9.77E-02          |
| C102_50           | 0.6603        | 0.593         | 0.7217        | 0.6676        | 0.6592        | 5.00E-02 9.40E-02                   | 4.13E-02          | 6.42E-02          | 7.63E-02          |
| C103_50           | 0.6335        | 0.5529        | 0.6725        | 0.6366        | 0.6055        | 4.25E-02 8.36E-02                   | 2.32E-02          | 4.72E-02          | 5.81E-02          |
| C104_50           | 0.5716        | 0.5569        | 0.6141        | 0.5897        | 0.5819        | 5.99E-02 5.66E-02                   | 1.79E-02          | 2.26E-02          | 3.87E-02          |
| C105_50           | 0.7383        | 0.604         | 0.8076        | 0.7284        | 0.7328        | 8.22E-02 1.18E-01                   | 6.30E-02          | 7.94E-02          | 6.84E-02          |
| C106_50           | 0.7638        | 0.5963        | 0.7949        | 0.7231        | 0.6989        | 4.03E-02 1.15E-01                   | 5.28E-02          | 5.46E-02          | 7.11E-02          |
| C107_50           | 0.743         | 0.5816        | 0.7938        | 0.7187        | 0.6862        | 5.62E-02 1.36E-01                   | 5.85E-02          | 9.08E-02          | 7.56E-02          |
| C108_50           | 0.6574        | 0.5514        | 0.7411        | 0.6584        | 0.6633        | 9.33E-02 1.28E-01                   | 4.14E-02          | 6.00E-02          | 7.75E-02          |
| C109_50           | 0.6779        | 0.5607        | 0.7304        | 0.6806        | 0.6578        | 5.75E-02 1.10E-01                   | 4.15E-02          | 4.94E-02          | 9.19E-02          |
| C201_50           | 0.7266        | 0.4804        | 0.815         | 0.6965        | 0.6232        | 8.49E-02 2.54E-01                   | 1.32E-02          | 5.77E-02          | 1.12E-01          |
| C202_50           | 0.7559        | 0.6289        | 0.8228        | 0.7422        | 0.7244        | 7.10E-02 1.50E-01                   | 3.15E-02          | 7.93E-02          | 8.53E-02          |
| C203_50           | 0.7243        | 0.5851        | 0.7735        | 0.7375        | 0.7013        | 4.54E-02 1.23E-01                   | 5.40E-02          | 3.55E-02          | 9.07E-02          |
| C204_50           | 0.6478        | 0.5305        | 0.6907        | 0.6532        | 0.6582        | 6.92E-02 1.26E-01                   | 2.56E-02          | 4.18E-02          | 6.98E-02          |
| C205_50           | 0.8273        | 0.6242        | 0.8581        | 0.7417        | 0.7236        | 4.76E-02 1.65E-01                   | 4.08E-02          | 1.06E-01          | 1.29E-01          |
| C206_50           | 0.7642        | 0.5902        | 0.8285        | 0.763         | 0.7287        | 6.08E-02 1.58E-01                   | 4.32E-02          | 7.66E-02          | 1.32E-01          |
| C207_50           | 0.7955        | 0.6813        | 0.8391        | 0.7676        | 0.7636        | 1.01E-01 1.38E-01                   | 2.07E-02          | 9.54E-02          | 9.37E-02          |
| C208_50           | 0.7621        | 0.5964        | 0.8227        | 0.7192        | 0.714         | 8.08E-02 1.67E-01                   | 3.78E-02          | 8.97E-02          | 1.22E-01          |
| R101_50           | 0.72          | 0.5943        | 0.7731        | 0.7056        | 0.6857        | 5.88E-02 1.25E-01                   | 3.15E-02          | 6.50E-02          | 9.44E-02          |
| R102_50           | 0.7028        | 0.5953        | 0.7688        | 0.7041        | 0.6888        | 7.21E-02 1.38E-01                   | 3.48E-02          | 7.14E-02          | 9.57E-02          |
| R103_50           | 0.6966        | 0.6111        | 0.7657        | 0.7174        | 0.7029        | 7.01E-02 1.16E-01                   | 3.47E-02          | 7.15E-02          | 7.42E-02          |
| R104_50           | 0.6334        | 0.5687        | 0.6975        | 0.6602        | 0.6402        | 7.70E-02 1.07E-01                   | 3.80E-02          | 5.87E-02          | 1.05E-01          |
| R105_50           | 0.7323 0.6972 | 0.6299 0.58   | 0.82 0.7824   | 0.7617        | 0.7394        | 6.88E-02 1.38E-01                   | 3.12E-02          | 6.09E-02          | 8.06E-02          |
| R106_50 R107_50   | 0.4909        | 0.412         | 0.6452        | 0.7054 0.5521 | 0.6943 0.5649 | 7.05E-02 1.41E-01 7.87E-02 2.26E-01 | 2.93E-02 2.52E-02 | 9.02E-02 9.53E-02 | 8.60E-02 9.09E-02 |
| R108_50           | 0.6149        | 0.5617        | 0.7113        | 0.6619        | 0.6516        | 7.04E-02 1.07E-01                   | 2.32E-02          | 8.02E-02          | 6.18E-02          |
| R109_50           | 0.6873        | 0.6019        | 0.7788        | 0.7091        | 0.6861        | 5.84E-02 1.17E-01                   | 2.89E-02          | 6.85E-02          | 7.14E-02          |
| R110_50           | 0.6489        | 0.5789        | 0.7455        | 0.7011        | 0.6616        | 6.43E-02 1.37E-01                   | 2.91E-02          | 4.98E-02          | 1.11E-01          |
| R111_50           | 0.6193        | 0.5997        | 0.7504        | 0.6892        | 0.6765        | 7.33E-02 1.21E-01                   | 3.29E-02          | 6.32E-02          | 7.92E-02          |
| R112_50           | 0.6531        | 0.6168        | 0.7485        | 0.6905        | 0.684         | 6.81E-02 9.83E-02                   | 2.39E-02          | 5.56E-02          | 7.16E-02          |
| R201_50           | 0.7116        | 0.4902        | 0.7944        | 0.6829        | 0.6806        | 4.77E-02 2.20E-01                   | 1.63E-02          | 5.42E-02          | 5.03E-02          |
| R202_50           | 0.7634        | 0.6185        | 0.811         | 0.748         | 0.718         | 7.36E-02 1.17E-01                   | 7.26E-02          | 6.31E-02          | 7.80E-02          |
| R203_50           | 0.7081        | 0.6064        | 0.7569        | 0.7399        | 0.7111        | 6.11E-02 9.88E-02                   | 5.94E-02          | 6.56E-02          | 8.77E-02          |
| R204_50           | 0.6365        | 0.5983        | 0.6988        | 0.6758        | 0.6605        | 6.32E-02 9.36E-02                   | 2.12E-02          | 3.23E-02          | 5.09E-02          |
| R205_50           | 0.7666        | 0.6038        | 0.8305        | 0.7869        | 0.7485        | 7.26E-02 1.29E-01                   | 4.90E-02          | 7.56E-02          | 1.29E-01          |
| R206_50           | 0.7227        | 0.5924        | 0.7826        | 0.7658        | 0.7366        | 6.43E-02 1.26E-01                   | 2.53E-02          | 5.66E-02          | 8.87E-02          |
| R207_50           | 0.6567        | 0.5663        | 0.7261        | 0.6793        | 0.6691        | 5.41E-02 1.13E-01 6.66E-02          | 2.52E-02          | 5.95E-02          | 7.85E-02          |
| R208_50           | 0.6455        | 0.5548        | 0.7056        | 0.6563        | 0.6408        | 1.08E-01                            | 1.53E-02          | 4.42E-02          | 6.44E-02          |
| R209_50           | 0.7125        | 0.5578        | 0.7823        | 0.7061        | 0.7238        | 6.91E-02 1.66E-01                   | 2.92E-02          | 6.67E-02          | 1.08E-01          |
| R210_50           | 0.6981        | 0.5814        | 0.7717        | 0.6987        | 0.7027        | 6.36E-02 1.18E-01                   | 4.19E-02          | 7.59E-02 6.79E-02 | 8.93E-02 1.28E-01 |
| R211_50 RC101_50  | 0.7296 0.7718 | 0.6229 0.6665 | 0.7911 0.8675 | 0.7437 0.8118 | 0.7138 0.7538 | 9.78E-02 1.07E-01 7.54E-02 1.23E-01 | 3.66E-02 2.25E-02 | 6.69E-02          | 9.04E-02          |
|                   |               | 0.4825        | 0.7527        |               | 0.6228        | 8.35E-02 2.15E-01                   | 2.40E-02          |                   |                   |
| RC102_50          | 0.5989        |               |               | 0.6673        |               |                                     |                   | 7.62E-02          | 1.15E-01          |
| RC103_50          | 0.6314        | 0.5917        | 0.7641        | 0.7159        | 0.6607        | 9.22E-02 1.33E-01                   | 3.20E-02 2.06E-02 | 6.52E-02          | 9.85E-02          |
| RC104_50          | 0.6213        | 0.5402        | 0.7042        | 0.6722        | 0.635         | 5.16E-02 1.41E-01                   |                   | 4.23E-02          | 7.97E-02          |
| RC105_50 RC106_50 | 0.7509 0.6616 | 0.6095 0.5548 | 0.8148 0.7771 | 0.7662        | 0.7155        | 6.70E-02 1.41E-01                   | 3.21E-02          | 5.37E-02          | 9.84E-02          |
|                   |               |               |               | 0.7016        | 0.6676        | 6.60E-02 1.82E-01                   | 2.59E-02          | 6.82E-02          | 1.13E-01          |
| RC107_50          | 0.6488        | 0.5302        | 0.7315        | 0.7002        | 0.6289        | 5.53E-02 1.91E-01                   | 2.87E-02          | 5.23E-02          | 1.18E-01          |
|                   |               |               |               |               |               | 1.30E-01                            | 1.97E-02          | 5.80E-02          | 8.31E-02          |
| RC108_50 RC201_50 | 0.6196 0.6767 | 0.5722 0.4216 | 0.7519 0.7581 | 0.7234 0.639  | 0.6737 0.5896 | 8.12E-02 2.81E-01                   |                   | 8.29E-02          | 1.40E-01          |
| RC202_50          | 0.7113        | 0.5884        | 0.7858        | 0.7243        | 0.7088        | 7.12E-02 4.98E-02 1.30E-01          | 1.46E-02 2.88E-02 | 6.42E-02          | 9.82E-02 1.01E-01 |
| RC203_50          | 0.7028        | 0.5892        | 0.7658        | 0.7145        | 0.685         | 5.06E-02 1.19E-01                   | 4.01E-02          | 3.92E-02 4.43E-02 | 9.70E-02          |
| RC204_50          | 0.6789        | 0.5903        | 0.7255        | 0.6928        | 0.6593        | 5.67E-02 1.13E-01                   | 2.93E-02 3.50E-02 | 5.72E-02          | 1.05E-01          |
| RC205_50          | 0.7481        | 0.6018        | 0.8313        | 0.7644        | 0.7274        | 7.52E-02 1.64E-01                   | 3.83E-02          | 7.38E-02          | 1.01E-01          |
| RC206_50 RC207_50 | 0.7324        | 0.5672        | 0.8236        | 0.7353 0.7507 | 0.7042 0.7157 | 1.04E-01 1.79E-01                   | 4.43E-02          |                   | 9.73E-02          |
| RC208_50          | 0.7747        | 0.6035        | 0.8131        | 0.7246        | 0.6842        | 4.29E-02 1.33E-01                   | 3.56E-02          | 6.39E-02          | 8.84E-02          |
|                   | 0.7187        | 0.6115        | 0.7889        |               |               | 7.00E-02 1.18E-01                   |                   | 8.12E-02          |                   |

## 5.4. Effectiveness of the proposed strategy

## 5.4.1. Effectiveness of the initial strategy

To demonstrate the effectiveness of the initial strategy, two types of algorithms were designed: QMOEA-R, which uses only the random strategy, and QMOEA-H, which uses a hybrid strategy (c.f. Subsection 4.3). Then, QMOEA-R and QMOEAH were independently run 30 times in all instances. Only the nondominated solution set of the first-generation population was reached. Subsequently, the HV value was obtained as a response variable (RV) for each run. The mean plot for RV is shown in Fig. 15. Clearly, Fig. 15 is divided into three parts according to the number of customers s 2 f 25 ; 50 ; 100 . g

As shown in Fig. 15, QMOEA-H outperformed QMOEA-R. For 147 of 168 instance sizes (87.5%), QMOEA-H resulted in significantly better HV than QMOEA-R, indicating that the hybrid initial strategy can provide the initial population with a high level of quality and diversity. Moreover, with an increase in the number of customers, the difference between QMOEA-H and QMOEA-R was more significant, and the effectiveness of the hybrid strategy became more profound. The main reason for this phenomenon is that the conflicts become more aggravated in terms of the convergence and diversity of solutions as the number of customers increases.

Furthermore, in the initialization phase, the initial solutions of C102\_100 were reached by QMOEA-H and QMOEA-R. Then, the initial PF was generated by merging the two initial solutions. As shown in Fig. 16, QMOEA-H significantly outperformed QMOEA-R in terms of distribution and convergence.

## 5.4.2. Effectiveness of the proposed crossover operator

To demonstrate the effectiveness of SCOX and CBOX, two different types of QMOEAs were designed: QMOEA-NSC, which does not integrate SCOX and SBOX, and QMOEA -SC, which integrates SCOX and SBOX. The HV and IGD computational results, averaged across the 10 replications and grouped for each subset, are summarized in Table 4. For example, for the C1-type with 25 customers, QMOEA-SC solved to optimality 7 of 9 problems (77.8%) for HV; for IGD, QMOEA-SC obtained 6 of 9 optimal values. In addition, the general trends of the two indicators were consistent, which verifies the effectiveness of the strategies. As shown in Table 4, (1) considering the HV values, compared with QMOEA-NSC, QMOEA-SC solved to optimality 105 of 168 problems; (2) for IGD, QMOEA-SC obtained 115 better results than QMOEA-NSC; (3) two crossover operations can learn knowledge and can guide the evolution process; and (4) the crossover method for C2-type instances will accelerate the convergence of the solution, and will affect the convergence or diversity of the solution to a certain extent.

## 5.4.3. Effectiveness of the adaptive local search method

To validate the performance of the proposed Q-learning-based adaptive local search method, QMOEA-NQ, which has randomly selected neighborhoods and QMOEA-Q, which contains the adaptive local search, were designed. The computational results of the two algorithms in terms of HV and IGD are illustrated in Table 5. The first column gives 18 randomly selected instances. Each instance was run five times by each of QMOEA-NQ and QMOEA-Q to obtain the average HV and IGD values. The results of the two algorithms for HV are given in Columns 2 and 3. Columns 4 and 5 provide the results for IGD, respectively.

As shown in Table 5, (1) for HV, QMOEA-Q obtained 17 better values than QMOEA-NQ, which means that QMOEA-Q is better than QMOEA-NQ in both convergence and diversity; (2) for IGD, QMOEA-Q obtained 16 better values than QMOEA-NQ, which further verifies the efficiency of the adaptive local search method.

In addition, for IGD and 1-HV of all instances, the unfactored analysis of variance (ANOVA) was performed for QMOEA-Q and QMOEA-NQ. The ANOVA results are illustrated in Fig.17. The correlation index p was less than 0.05, which means that the QMOEA-Q is significantly effective within 95% least-significant difference intervals.

## 5.5. Effectiveness of QMOEA

This study is the first attempt to solve the TDGVRPTW. Five algorithms were modified to solve the problem, i.e., MOEA/D (2007), MaOEA-CSS (2017), RPDNSGAII (2018), hpaEA (2019), and QMOEA. For a fair comparison, the strategies of the five algorithms were modified so that the problem could be solved. MOEA/D employed all proposed strategies, MaOEA-CSS added the proposed crossover method, RPDNSGAII took the initialization strategy, and hpaEA kept its original state. In addition, the same maximum number of evaluations and runs of 168 instances with 10 replications was set for each comparative algorithm. Then, the HV and IGD of each instance were obtained.

To intuitively understand the performances of the comparison algorithms, the parallel coordinate method was introduced for visualizing analytic three-dimensional data. Figs. 18 and 19 depict the final and approximate PFs from different algorithms for C106\_50 and R101\_100, respectively. For a clearer presentation, the ranges of the three objectives were normalized to [0,1]. (1) QMOEA obtains the solution sets closest to the final PF . (2) For the simple example C106\_50, RPDNSGAII and hpaEA had a good distribution. However, for the complex example R101\_100, the algorithms could easily fall into a local optimum, especially objective 1. (3) The population obtained by QMOEA had significantly better diversity and convergence than those of the other algorithms. Other algorithms were mostly concentrated in the middle of the PF .

The RPI was used to analyze all data for 168 instances, which was categorized into three groups according to the number of customers s ¼ f 25 50 100 . Fig. 20 reports box plots of IGD and 1-HV. Column 1 presents the number of customers. Col-; ; g umns 2 and 3 present the RPI values of HV and IGD with different customer groups for the five algorithms. For the HV value, QMOEA achieved the minimum RPI values and significantly outperformed all comparative algorithms. Comparing MOEA/D with QMOEA, the decomposition-based method was not as effective as the dominance-based method.

However, for the IGD value, QMOEA, MaOEA-CSS, RPDNSGAII, and RPDNSGAII had no significant differences. The above analysis demonstrates that QMOEA can obtain better performance in terms of the diversity of solutions. The comparison results of the HV and IGD values for the given instances of 50 customers are illustrated in Table 6.

## 6. Conclusions

In this study, a three-objective version of the TDGVRPTW was investigated. We proposed QMOEA to solve the problem in which three objectives-total duration of vehicles, energy consumption, and customer satisfaction-were simultaneously considered. First, a hybrid initial method, which included four different methods, could improve the quality of the solutions. Second, two PF-based crossover strategies were designed to explore the search space and accelerate convergence. Subsequently, the sequence of local search operators was determined by the Q-learning algorithm to improve the exploitation ability. Finally, QMOEA was verified in a set of realistic instances.

In future work, our method can be extended for the following tasks. (1) It might be worth studying a problem by considering more realistic constraints, such as heterogeneous vehicle transport, open vehicle transport, and warehouse capacity limitation. (2) Moreover, problem-specific knowledge for various real industrial environments should be employed to gain enhanced performance. (3) In addition, it might be interesting to study deep Q networks and other state-of-the-art politybased RL. (4) Furthermore, it is worthwhile to investigate the joint scheduling of production scheduling and outbound distribution with just-in-time philosophy gaining increased research attention. (5) Moreover, multiobjective optimization methods should be designed to solve the problem in dynamic or fuzzy environments.

## Data availability

I have shared the link to my data in this study.

## Declaration of competing interest

The authors declare that they have no known competing financial interests or personal relationships that could have appeared to influence the work reported in this paper.

## Acknowledgement

This research was financially supported by National Science Foundation of China (62173216).

## References

- [1] M. Song, J. Li, Y. Han, Y. Han, L. Liu, Q. Sun, Metaheuristics for solving the vehicle routing problem with the time windows and energy consumption in cold chain logistics, Applied Soft Computing 95 (2020), https://doi.org/10.1016/j.asoc.2020.106561.
- [2] U. Orozco-Rosas, O. Montiel, R. Sepulveda, Mobile robot path planning using membrane evolutionary artificial potential field, Applied Soft Computing 77 (2019) 236-251.
- [3] U. Orozco-Rosas, K. Picos, O. Ross, Hybrid path planning algorithm based on membrane pseudo-bacterial potential field for autonomous mobile robots, IEEE Access 7 (1) (2019) 156787-156803.
- [4] F. Ning, G. Jiang, S.-K. Lam, C. Ou, P. He, Y. Sun, Passenger-centric vehicle routing for first-mile transportation considering request uncertainty, Information Sciences 570 (2021) 241-261.
- [5] J. Wang, T. Weng, Q. Zhang, A Two-Stage Multiobjective Evolutionary Algorithm for Multiobjective Multidepot Vehicle Routing Problem With Time Windows, IEEE Transaction on Cybernetics 49 (7) (2019) 2467-2478.
- [6] X. Wang, T. Choi, Z. Li, S. Shao, An Effective Local Search Algorithm for the Multidepot Cumulative Capacitated Vehicle Routing Problem, IEEE Transactions on Systems, Man, and Cybernetics: Systems 50 (12) (2020) 4948-4958.
- [7] M.K. Mehlawat, P. Gupta, A. Khaitan, W. Pedrycz, A Hybrid Intelligent Approach to Integrated Fuzzy Multiple Depot Capacitated Green Vehicle Routing Problem With Split Delivery and Vehicle Selection, IEEE Transactions on Fuzzy Systems 28 (6) (2020) 1155-1166.
- [8] H. Fan, Y. Zhang, P. Tian, Y. Lv, H. Fan, Time-dependent multi-depot green vehicle routing problem with time windows considering temporal-spatial distance, Computers &amp; Operations Research 129 (1) (2021) 105-211.
- [9] B. Pan, Z. Zhang, A. Lim, A hybrid algorithm for time-dependent vehicle routing problem with time windows, Computers &amp; Operations Research 128 (2021), https://doi.org/10.1016/j.cor.2020.105193.
- [10] B. Pan, Z. Zhang, A. Lim, Multi-trip time-dependent vehicle routing problem with time windows, European Journal of Operational Research 291 (1) (2021) 218-231.
- [11] J. Wang, W. Ren, Z. Zhang, H. Huang, Y. Zhou, A Hybrid Multiobjective Memetic Algorithm for Multiobjective Periodic Vehicle Routing Problem With Time Windows, IEEE Transactions on Systems, Man, and Cybernetics: Systems 50 (11) (2020) 4732-4745.
- [12] Z. Zhang, H. Qin, Y. Li, Multi-Objective Optimization for the Vehicle Routing Problem With Outsourcing and Profit Balancing, IEEE Transactions on Intelligent Transportation Systems 21 (5) (2020) 1987-2001.
- [13] L. Wang, J. Lu, A memetic algorithm with competition for the capacitated green vehicle routing problem, IEEE/CAA Journal of Automatica Sinica 6 (2) (2019) 516-526.
- [14] J. Long, Z. Sun, P.M. Pardalos, Y. Hong, S. Zhang, C. Li, A hybrid multi-objective genetic local search algorithm for the prize-collecting vehicle routing problem, Information Sciences 478 (2019) 40-61.
- [15] J. Li, Y. Du, K. Gao, P. Duan, D. Gong, Q. Pan, P.N. Suganthan, A hybrid iterated greedy algorithm for a crane transportation flexible job shop problem, IEEE Transactions on Automation Science and Engineering (2021), https://doi.org/10.1109/TASE.2021.3062979.
- [16] B. Sawik, J. Faulin, E. Perez-Bernabeu, A Multicriteria Analysis for the Green VRP: A Case Discussion for the Distribution Problem of a Spanish Retailer, Transportation Research Procedia 22 (2017) 305-313.
- [17] Y. Yu, S. Wang, J. Wang, M. Huang, A branch-and-price algorithm for the heterogeneous fleet green vehicle routing problem with time windows, Transportation Research Part B Methodological 122 (2019) 511-527.
- [18] X. Ren, H. Huang, S. Feng, G. Liang, An improved variable neighborhood search for bi-objective mixed-energy fleet vehicle routing problem, Journal of Cleaner Production 275 (2020), https://doi.org/10.1016/j.jclepro.2020.124155.
- [19] Y. Li, H. Soleimani, M. Zohal, An improved ant colony optimization algorithm for the multi-depot green vehicle routing problem with multiple objectives, Journal of Cleaner Production 227 (2019) 1161-1172.

- [20] M. Afshar-Bakeshloo, A. Mehrabi, H. Safari, M. Maleki, F. Jolai, A green vehicle routing problem with customer satisfaction criteria, Journal of Industrial Engineering International 12 (4) (2016) 529-544.
- [21] H. Abdullahi, L. Reyes-Rubiano, D. Ouelhadj, J. Faulin, A.A. Juan, Modelling and multi-criteria analysis of the sustainability dimensions for the green vehicle routing problem, European Journal of Operational Research 292 (1) (2020) 143-154.
- [22] M.M. Solomon, Algorithms for the Vehicle Routing and Scheduling Problems with Time Window Constraints, Operations Research 35 (2) (1987) 254265.
- [23] B. Moradi, The new optimization algorithm for the vehicle routing problem with time windows using multi-objective discrete learnable evolution model, Soft Computing 24 (9) (2019) 6741-6769.
- [24] C. Chen, E. Demir, Y. Huang, An adaptive large neighborhood search heuristic for the vehicle routing problem with time windows and delivery robots, European Journal of Operational Research 294 (3) (2021) 1164-1180.
- [25] J. Li, Y. Han, P. Duan, Y. Han, B. Niu, C. Li, Z. Zheng, Y. Liu, Meta-heuristic algorithm for solving vehicle routing problems with time windows and synchronized visit constraints in prefabricated systems, Journal of Cleaner Production 250 (2020), https://doi.org/10.1016/j.jclepro.2019.119464.
- [26] H. Zhang, Q. Zhang, L. Ma, Z. Zhang, Y. Liu, A hybrid ant colony optimization algorithm for a multi-objective vehicle routing problem with flexible time windows, Information Sciences 490 (2019) 166-190.
- [27] P. Jiang, J. Men, H. Xu, S. Zheng, Y. Kong, L. Zhang, A Variable Neighborhood Search-Based Hybrid Multiobjective Evolutionary Algorithm for HazMat Heterogeneous Vehicle Routing Problem With Time Windows, IEEE Systems Journal 14 (3) (2020) 4344-4355.
- [28] M. Gmira, M. Gendreau, A. Lodi, J.Y. Potvin, Tabu search for the time-dependent vehicle routing problem with time windows on a road network, European Journal of Operational Research 288 (1) (2021) 129-140.
- [29] S. Ichoua, M. Gendreau, J. Potvin, Vehicle dispatching with time-dependent travel times, European Journal of Operational Research 144 (2) (2003) 379396.
- [30] K.-W. Jie, S.-Y. Liu, X.-J. Sun, A hybrid algorithm for time-dependent vehicle routing problem with soft time windows and stochastic factors, Engineering Applications of Artificial Intelligence 109 (2022), https://doi.org/10.1016/j.engappai.2021.104606.
- [31] S. Allahyari, S. Yaghoubi, T. Van Woensel, The secure time-dependent vehicle routing problem with uncertain demands, Computers &amp; Operations Research 131 (2021), https://doi.org/10.1016/j.cor.2021.105253.
- [32] F.E. Zulvia, R.J. Kuo, D.Y. Nugroho, A many-objective gradient evolution algorithm for solving a green vehicle routing problem with time windows and time dependency for perishable products, Journal of Cleaner Production 242 (2020), https://doi.org/10.1016/j.jclepro.2019.118428.
- [33] A. Pan, B. Shen, L. Wang, Ensemble of resource allocation strategies in decision and objective spaces for multiobjective optimization, Information Sciences 605 (2022) 393-412.
- [34] Q. Zhang, H. Li, MOEA/D: A multiobjective evolutionary algorithm based on decomposition, IEEE Transactions on Evolutionary Computation 11 (6) (2007) 712-731.
- [35] Y. Liu, Y. Hu, N. Zhu, K. Li, J. Zou, M. Li, A decomposition-based multiobjective evolutionary algorithm with weights updated adaptively, Information Sciences 572 (2021) 343-377.
- [36] X. Zhou, Y. Gao, S. Yang, C. Yang, J. Zhou, A multiobjective state transition algorithm based on modified decomposition method, Applied Soft Computing (2022), https://doi.org/10.1016/j.asoc.2022.108553.
- [37] K. Deb, A. Pratap, S. Agarwal, T. Meyarivan, A fast and elitist multiobjective genetic algorithm: NSGA-II, IEEE Transactions on Evolutionary Computation 6 (2) (2002) 182-197.
- [38] K. Deb, H. Jain, An Evolutionary Many-Objective Optimization Algorithm Using Reference-Point-Based Nondominated Sorting Approach, Part I: Solving Problems With Box Constraints, IEEE Transactions on Evolutionary Computation 18 (4) (2014) 577-601.
- [39] Z. He, G. Yen, Many-Objective Evolutionary Algorithms Based on Coordinated Selection Strategy, IEEE Transactions on Evolutionary Computation 21 (2) (2017) 220-233.
- [40] A. Santiago, B. Dorronsoro, A.J. Nebro, J.J. Durillo, O. Castillo, H.J. Fraire, A novel multi-objective evolutionary algorithm with fuzzy logic based adaptive selection of operators: FAME, Information Sciences 471 (2019) 233-251.
- [41] M. Elarbi, S. Bechikh, A. Gupta, L. Ben Said, Y.-S. Ong, A New Decomposition-Based NSGA-II for Many-Objective Optimization, IEEE Transactions on Systems Man and Cybernetics-Systems 48 (7) (2018) 1191-1210.
- [42] H. Chen, Y. Tian, W. Pedrycz, G. Wu, R. Wang, L. Wang, Hyperplane Assisted Evolutionary Algorithm for Many-Objective Optimization Problems, IEEE Transactions on Cybernetics 50 (7) (2019) 3367-3380.

[43]

E. Demir, T. Bekta, G. Laporte, A comparative analysis of several vehicle emission models for road freight transportation, Transportation Research Part D

Transport and Environment 16 (5) (2011) 347-357.

- [44] K. Ghoseiri, S.F. Ghannadpour, Multi-objective vehicle routing problem with time windows using goal programming and genetic algorithm, Applied Soft Computing 10 (4) (2010) 1096-1107.
- [45] Q. Pan, M.F. Tasgetiren, Y. Liang, A discrete particle swarm optimization algorithm for the no-wait flowshop scheduling problem, Computers &amp; Operations Research 35 (9) (2008) 2807-2839.
- [46] Z. Zhang, Z. Wu, H. Zhang, J. Wang, Meta-Learning-Based Deep Reinforcement Learning for Multiobjective Optimization Problems, IEEE Transactions on Neural Networks and Learning Systems (2022), https://doi.org/10.1109/TNNLS.2022.3148435.
- [47] Y. Tian, R. Cheng, X. Zhang, Y. Jin, PlatEMO: A MATLAB Platform for Evolutionary Multi-Objective Optimization, IEEE Computational Intelligence Magazine 12 (4) (2017) 73-87.
- [48] S. Dabia, S. Ropke, T. van Woensel, T. De Kok, Branch and Price for the Time-Dependent Vehicle Routing Problem with Time Windows, Transportation Science 47 (3) (2013) 380-396.
- [49] J. Li, X. Chen, P. Duan, J. Mou, KMOEA: A knowledge-based multi-objective algorithm for distributed hybrid flow shop in a prefabricated system, IEEE Transactions on Industrial Informatics (2021), https://doi.org/10.1109/TII.2021.3128405.