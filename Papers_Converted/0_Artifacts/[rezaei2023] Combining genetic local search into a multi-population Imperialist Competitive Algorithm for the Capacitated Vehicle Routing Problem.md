![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[rezaei2023] Combining genetic local search into a multi-population Imperialist Competitive Algorithm for the Capacitated Vehicle Routing Problem_artifacts/image_000000_5241994b1bbf689643e0422471c8539787249972725c6dd6051f1cc4e9c88824.png)

Contents lists available at ScienceDirect

## Applied Soft Computing

journal homepage: www.elsevier.com/locate/asoc

## Combining genetic local search into a multi-population Imperialist Competitive Algorithm for the Capacitated Vehicle Routing Problem

Babak Rezaei a , Frederico Gadelha Guimaraes a, ∗ , Rasul Enayatifar b , Pauline C. Haddow c

- a Department of Electrical Engineering, Universidade Federal de Minas Gerais (UFMG), Brazil
- b Department of Computer Engineering, Firoozkooh Branch, Islamic Azad University, Iran
- c Department of Computer Science, Norwegian University of Science and Technology (NTNU), Norway

## a r t i c l e i n f o

## a b s t r a c t

Article history:

Received 23 September 2022

Received in revised form 10 March 2023 Accepted 8 April 2023 Available online 25 April 2023

Keywords:

Vehicle Routing Problem Evolutionary computation Imperialist Competitive Algorithm Hybrid Genetic Search

The Vehicle Routing Problem (VRP) is one of the most significant problems in operational research today. VRP has a vast range of application fields such as transportation, logistics, manufacturing, relief systems and communication. To suit the needs of different real-world VRP scenarios, many models of VRP have been developed - CVRP (Capacitated VRP) being the classical form. In this article, a hybrid metaheuristic algorithm, ICAHGS, is proposed for solving CVRP. The present study proposes a refined Imperialist Competitive Algorithm (ICA) as the primary evolutionary and multipopulation method for addressing the Capacitated Vehicle Routing Problem (CVRP). In order to further optimize the search process, a Hybrid Genetic Search (HGS-CVRP) algorithm is applied as an enhanced local search and population management strategy within the ICA framework. Additionally, the internal restart step of the HGS-CVRP algorithm is replaced with a multi-step restart mechanism for intensification improvement. One notable aspect of the proposed method is its ability to facilitate parallel processing, with each empire able to be processed on a separate processor. This structure allows for increased computational efficiency in addressing the CVRP. To assess the effectiveness of the proposed algorithm, it has been compared to several state-of-the-art algorithms from the literature. The results of this comparison, which include both classical benchmark instances and real-world applications, demonstrate the competitive performance of the proposed algorithm.

© 2023Elsevier B.V. All rights reserved.

## 1. Introduction

The rapid increase in online shopping in our daily lives, requiring a large number of package deliveries, have increased the demand for express transportation and a reduction of transportation costs. Similar demands are placed on transportation systems for emergency healthcare deliveries. Efficient routing is needed to meet such demands, by improving transport allocation and scheduling. Thus, one of the most important problems and success stories of operational research to date, is the vehicle routing problem (VRP), first introduced by Dantzig and Ramser in 1959 [1].

The Vehicle Routing Problem (VRP) is a combinatorial optimization problem that aims to find the most efficient routes for a fleet of vehicles to deliver goods to geographically dispersed customers. VRP has a broad range of applications in logistics, transportation, and supply chain management. In logistics, it is

∗ Corresponding author.

E-mail addresses:

bbkrze@gmail.com (B. Rezaei), fredericoguimaraes@ufmg.br (F. Guimaraes), r.enayatifar@gmail.com

(R.

Enayatifar), pauline@ntnu.no (P.C. Haddow).

used to optimize delivery routes from a warehouse or distribution center to customers, taking into account factors such as vehicle capacity, road network constraints, and delivery time windows. In transportation, it is applied to optimize the routes of buses, taxis, and other public transportation vehicles to minimize travel time and costs. VRP is also relevant to the design of efficient and cost-effective supply chain networks, determining the most cost-effective mode of transportation, routing and inventory management decisions. Additionally, it can be applied to disaster relief operations and to optimize the routes of mobile sales or service teams [2].

In this paper, a hybrid algorithm termed ICAHGS is proposed for solving CVRP. The algorithm combines the Imperialist Competitive Algorithm (ICA) and Hybrid Genetic Search specialized to CVRP (HGS-CVRP). ICA provides advantages in global search exploiting sub-populations whose size decreases or increases depending on a measure of power of each sub-population. On the other hand, HGS-CVRP provides improved local search - route improvement (RI) and new neighborhood SWAP* [3]; as well as providing population management and diversity control. As such, the proposed hybrid takes advantage of the optimization strengths of each of these algorithms.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[rezaei2023] Combining genetic local search into a multi-population Imperialist Competitive Algorithm for the Capacitated Vehicle Routing Problem_artifacts/image_000001_213e0bfc472c25156fe3566a27cc4540bcbb5fe7ed52ff80a3172433cf62b6c1.png)

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[rezaei2023] Combining genetic local search into a multi-population Imperialist Competitive Algorithm for the Capacitated Vehicle Routing Problem_artifacts/image_000002_e7a190285367a25499ea5c58e3f6777b84d6b96b4873242979895b32b216fb95.png)

A beneficial action is to exploit the best solutions found so far intensification; which can guide the search to promising areas of the search space. One of the common methods to intensify the search around the best solutions includes restarting the search with best solutions and then fixing generated solutions [4]. HGS-CVRP has such a restart process. However, since the total population is regenerated there is no improvement in term of the intensification.

A further potential improvement is the introduction of randomization that increases exploration of the search space [5]. Such exploration is equivalent to diversification [4].

In comparison to the proposed ICAHGS, the Island model (IGA) is the most closely related evolutionary model. In the Island model, multiple independent islands exist, each running its own evolutionary algorithm. One notable difference is that the ICA includes an Assimilation step, in which colonies are moved towards their imperialists. This step can improve intensification - not present in IGA; where each imperialist is a local-optima for its colonies. Additionally, in the ICA, dynamic colonies are present, unlike in the IGA where the islands are fixed. In the ICA, empires that are unable to improve their cost prior to the next competition will gradually lose their colonies and ultimately be removed from the list. The removal of less successful empires frees computational resources, particularly when parallel processing is applied, unlike in IGA.

The main contributions of this paper include:

- · refinement of the original ICA to better address the CVRP problem
- · incorporation of a state-of-the-art algorithm, HGS-CVRP, as a local search step within a multi-population approach
- · replacement of the internal restart step of HGS-CVRP with a multi-step restart mechanism to increase intensification

The performance of the proposed ICAHGS was evaluated through experiments on 100 classical benchmark instances from Uchoa et al. [6] - provide a comprehensive challenge for evaluating solution quality; as well as well-known classical benchmarks from the literature; CMT [7] and Golden [8]. In order to assess the algorithm's performance on real-world applications, it was also tested on 6 instances from the LoggiBUD benchmark [9].

The reminder of the paper is structured as follows: A review of related works is provided in Section 2. In Section 3, a brief description of the background theory is provided. In Section 4, the proposed algorithm is described. Details of the experimental results are reported in Section 5 and the conclusion of the study is given in Section 6.

## 2. Related works

There are two major categories of optimization algorithms: exact methods and heuristic algorithms (or approximate algorithms) [2]. The most successful exact methods are the Branch and Bound [10] and Branch-and-cut [11]. Since CVRP is an NPhard problem, it needs exponential time to be solved optimally. If the number of customers exceeds 200, exact methods are unable to solve CVRP consistently [6]. Consequently, to obtain optimal results within a reasonable time, heuristic and metaheuristic solutions have been proposed. Heuristic algorithms are designed to find a good, but not necessarily optimal, solution to the problem in a reasonable amount of time. One well-known CVRP heuristic is the Clarke and Wright Savings Algorithm [12]. Gillett and Miller [13] proposed the Sweeping Algorithm, based on the idea of constructing routes by adding customers to a route one at a time. Further, Prins and Bouchenoua [14] introduced a Memetic Algorithm (MA) that utilizes three heuristics (Nearest Neighbor, Merge, and Tour Splitting) to generate the initial population. The algorithm then employs a simple order crossover (OX) operator and a local search procedure, which includes replace, swap, and 2-opt moves. However, the extensive use of these local search moves can significantly increase the overall running time of the MA. Thus Subramanian et al. [15] proposed a hybrid heuristic algorithm which combines an Iterated Local Search (ILS) heuristic and a Set Partitioning (SP) approach. However, due to the challenges inherent in CVRP, more robust, efficient, and stable algorithms are needed.

In recent years researchers have looked to hybrid solutions [16] - SA (Simulated Annealing) and TS (Tabu Search) [17]; discrete particle swarm optimization and SA [18] and LNS (Large Neighborhood Search) with ant colony optimization algorithm [19]. Prins proposed hybrid GA (HGA) [20], with a giant-tour solution representation and local search enhancement, included a repair procedure to achieve feasible solutions after crossover. Gokalp and Ugur proposed a multi-start metaheuristic method [21]. The hybrid combines a basic GA, ILS (Iterated Local Search) and Variable Neighborhood Descent (RVND). The Unified Hybrid Genetic Search (UHGA), proposed by Vidal et al. [22] involves a Hybrid GA, addressed diversity through the introduction of Adaptive Diversity Control to solve the Multi-Depot VRP, the Periodic VRP and Multi-Depot Periodic VRP.

The Knowledge guided local search (KGLS) is proposed by Arnold et al. [23] which is a methodology based on data mining techniques to find characteristics that distinguish good VRP solutions from not-so-good ones. Christiaens et al. [24] proposed the slack induction by string removals (SISR) as a fast optimization heuristic for solving VRP instances. It is included 3 methods and procedures; a ruin method, a recreate method, and a fleet minimization procedure. Recently, Vidal proposed HGS-CVRP [3] with the same methodology as UHGA but it includes SWAP* Neighborhood. There has been a growing interest in machine learning techniques e.g. applying reinforcement learning algorithms to learn routing policies that adapt to changing environments [25]. Imperialist Competitive Algorithm (ICA) [26] is a social inspired evolutionary algorithm, achieving success in many applications areas: image encryption [27], stock market forecasting [28] and scheduling problem [29]. Yousefikhoshbakht et al. [30] solved the Traveling Salesman Problem based on a refined ICA, applying order crossover in the Assimilation step and two exchange moves in Revolution step.

## 3. Background theory

## 3.1. Capacitated VRP

VRP can be considered as a generalization of TSP (TravelingSalesman Problem), which is one of the well-known classic problems in operational research.

In the original VRP [1], known as the Capacitated VRP (CVRP), the goal is to find a set of routes for a set of identical vehicles while all customers are served and the overall route cost is minimized. All customers must be served exactly once using only one route. All vehicles must start and finish at the same central depot. Each customer has one delivery that cannot be split between different vehicles. For each vehicle, the loading and traveling distance cannot be greater than the maximum allowable loading capacity and traveling distance, respectively [19].

## 3.2. Capacitated VRP formulation

CVRP is defined as a complete undirected graph G = ( V , E ). Set V = { 0 1 2 , , , . . . , n } is a set of nodes where the single central depot is represented by node 0 and N = { 1 2 , , . . . , n } is set of customers. The depot is a base for a fleet K = { 1 2 , , . . . , | K |} .

Fig. 1. Initializing the empires (inspired by [26]).

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[rezaei2023] Combining genetic local search into a multi-population Imperialist Competitive Algorithm for the Capacitated Vehicle Routing Problem_artifacts/image_000003_70198c6c8686162a3d7ec66465420bb616dfdbefcbc4392ac35c7fedb8409a60.png)

Fig. 2. Colony movement towards its imperialist (inspired by [26]).

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[rezaei2023] Combining genetic local search into a multi-population Imperialist Competitive Algorithm for the Capacitated Vehicle Routing Problem_artifacts/image_000004_e635c6bd7ebfb72e064bcb749ecd5bbb6236717b4c88e7b78bcb66869abc71dc.png)

Set E = { e = { i , j } = { j , i } : i , j ∈ V , i ̸ = j } is the edge set with nonnegative traveling cost cij between nodes i and j for each { i , j } ∈ E . The fleet is assumed to be homogeneous i.e. all | K | vehicles are available at the central depot , have the identical capacity Q &gt; 0 and all operate at equal cost. The objective is to determine the | K | vehicle routes in such a way that [2]:

- · each route is started and finished at the central depot
- · each customer n ∈ N is visited exactly once
- · the vehicle capacity Q is not exceeded by the total demand of customers in each single route
- · the sum of the distances traveled by the vehicles to serve all the customers is minimized.

Assuming such constraints, the objective function may be defined as Eq. (1).

$$\text{objective function} \equiv \minimize ( \sum _ { 1 } ^ { | K | } \sum _ { ( i, j \in E ) } c _ { i j } x _ { i j } ) & & ( 1 ) & & 3. 3. 4. \ S \underset { \text{imperial} } { \text{$S$\dots$} }$$

where for all { i , j } ∈ E , xij = 1 if a vehicle travels from i to j , otherwise xij = 0.

## 3.3. Imperialist Competitive Algorithm (ICA)

The original ICA, [26] and [31], is presented in brief in this section and the refinements to the algorithm are presented in Section 4.1.

## 3.3.1. Step 1: Create initial empires

The initial population of size Npop provides an initial collection of countries. Nimp of the most powerful countries (based on fitness value) are selected to form the initial empires. The remaining Ncol of the countries ( Ncol = Npop -Nimp ) are regarding as colonies that are divided amongst the Nimp empires such that the size of each empire - the number of colonies assigned; is relative to its imperialist power (relative fitness) - see Fig. 1. As the color coding highlights, a larger star (higher imperialist power/ fitness) has more colonies.

## 3.3.2. Step 2: Assimilation: Moving colonies toward the imperialists

The imperialist in each empire has the best fitness value in its corresponding empire. Therefore, each one is acting as a ''Local Optima'' in their empires. In this step, the imperialists start improving their corresponding colonies through crossover. The aim of this step is to search for points in the solution space near the imperialist (local optima), providing improved fitness. Fig. 2 illustrates the update process: where d is the distance between the colony and its imperialist. The new position of the colony depends on x (number of units to move) and θ (deviation from the direction to the empire). x is a random variable with uniform distribution where x ∼ U (0 , β × d ) and β is greater than 1. θ , is a random number with uniform distribution (or any suitable distribution that provides a random amount of deviation). Therefore, θ ∼ U ( -γ, γ ) where γ is a parameter that adjusts the deviation from the original direction.

## 3.3.3. Step 3: Revolution: Revolve the colonies

Revolution is equivalent mutation in a GA. The '' Revolution Rate '' is thus a parameter that determines the percentage of the colonies that need to be revolved. First, the number of the revolved colonies are calculated by Eq. (2), then the Revolution operator is applied to all the revolving colonies which are selected randomly.

$$\begin{smallmatrix} \text{:fined} & & \text{NumOf RevolvedColonies} & & \text{$=RevolutionRate} \\ & & & \text{* NumberOf ColoniesOf Empire} \end{smallmatrix}$$

$$( 2 )$$

3.3.4. Step 4: Possess empires: Swapping the position of colony and imperialist

Moving the colonies towards their corresponding imperialist causes the colonies to reach new positions. Therefore, their new fitness value will be calculated. If one of the colonies has better fitness value than its corresponding imperialist, their positions will be exchanged as highlighted in Fig. 3(a) (before) and Fig. 3(b) (after) i.e. the colony with the best fitness will become the new Imperialist.

## 3.3.5. Step 5: Total cost calculation

The Total cost of each empire indicates the power of an empire, as shown in Eq. (3):

Fig. 3. (a) Best colony and imperialist. (b) After swap (inspired by [26]).

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[rezaei2023] Combining genetic local search into a multi-population Imperialist Competitive Algorithm for the Capacitated Vehicle Routing Problem_artifacts/image_000005_96e4eb4fd03aaac6a020fe1987a9dabbc3f25f894ee1b03556ef55d74bcc22f8.png)

where T C . . n is the total cost of the n th empire, and 0 &lt; ξ &lt; 1 is a parameter that defines the contribution factor of colonies to the power of an empire. The Cost represents the fitness function (or objective function).

- (2) A controlled exploration of infeasible solutions: which provides more exploration in the areas close to the feasibility boundaries.

## 3.3.6. Step 6: Imperialist competition

In this competition between empires, the weakest colony of the weakest empire is selected for possession. An empire with more power has more likelihood to seize the colony. If an empire has no colonies left, the imperialist itself is selected as a colony for possession and the empire ceases to exist. Fig. 4 illustrates the imperialist competition where the size of stars again reflects the power of the imperialists and where the yellow colony represents the selected weakest colony. P1 , P2 and P3 are the possession probabilities for empires 1, 2 and 3 calculated based on total cost of each empire as follows:

$$N. T. C. _ { n } & = T. C. _ { n } - \max _ { i } \{ T. C. _ { i } \} & ( 4 ) \quad \text{Step} _ { \text{plied } t }$$

$$P _ { P _ { n } } = \left | \frac { N. T. C. _ { n } } { \sum _ { i = 1 } ^ { N _ { i m p } } N. T. C. _ { i } } } \right | & \begin{array} { c } \begin{array} { c } \begin{array} { c } \end{array} { c } \begin{array} { c } \begin{array} { c } \end{array} { c } \end{array} \\ \end{array} \end{array} \begin{array} { c } \begin{array} { c } \begin{array} { c } \begin{array} { c } \begin{array} { c } \end{array} { c } \end{array} \\ \end{array} \begin{array} { c } \begin{array} { c } \begin{array} { c } \begin{array} { c } \end{array} { c } \end{array} \\ \end{array} \begin{array} { c } \begin{array} { c } \begin{array} { c } \begin{array} { c } \end{array} { c } \end{array} \end{array}$$

where N T C . . . n is the normalized total cost of the n th empire and T C . . n is the total cost of the n th empire (see Eq. (3)). If P = { P 1 , P 2 , P 3 } and R = { r 1 , r 2 , r 3 } (uniformly distributed random numbers) then D = P -R = { P 1 -r 1 , P 2 -r 2 , P 3 -r 3 } . The empire whose relevant index in D is maximum, will be the winner of this competition.

## 3.3.7. Stopping conditions

The above-mentioned steps (2 to 6) are repeated iteratively. The weaker empires will collapse gradually by losing their colonies. Finally, only the most powerful empire will remain and its imperialist that is the solution's ''Local Optima'' i.e. it will be the best individual.

## 3.4. Hybrid genetic search for CVRP (HGS-CVRP)

HGS-CVRP is a specialized version of the UHGA (Unified Hybrid Genetic Search) for solving CVRP, through the introduction of the new neighborhood search, SWAP* [3]. Its search schema is the same as its original method (UHGA - [22]) which is a combination of 3 strategies:

## (1) Combining crossover and a neighborhood-based search:

Crossover improves diversification through more exploration in the solution space and the neighborhood-based search provides aggressive solution improvement.

- (3) Advanced population diversity management strategies: which allows the population to be diversified with high quality solutions.

As depicted in Fig. 5, HGS-CVRP performs following the steps until a stopping condition is met:

Step 1 Parents Selection: To select each parent, binary tournament selection is applied, returning an individual (solution) with the best fitness value of the tournament. To calculate the fitness value, two parameters are considered: objective value and diversity. Therefore, each individual has two ranking characteristics: in terms of solution quality and in terms of diversity contribution. The fitness value is calculated based on a weighted sum of these ranks.

Step 2 Recombination: An ordered crossover (OX) is applied to the two selected parents., Trip delimiters are then inserted by an efficient linear-time Split so as to make a complete CVRP solution. Ordered crossover (OX - [32]), includes 2 steps: (1) selecting a random slice of the first individual and then (2) completing missing values by starting from the second cutting point and adding the sequence from the second individual.

Step 3 Local Search: The proposed local search is a neighborhood search including a route improvement (RI) which is a combination of classical RI (Relocate, Swap, 2-Opt and 2-Opt ) ∗ and Swap . Further, a Repair operation with 50% probability is ∗ applied to infeasible solutions to recover feasible solutions.

In the classical form of the SWAP neighborhood, 2 customers are exchanged, aiming to make any intra-route or inter-route improvements. However, SWAP* neighborhood exchanges 2 customers from 2 different routes without an insertion in place. It means that one customer from route 1 can be inserted into any position of route 2 and one customer from route 2 can be inserted into any position of route 1.

Step 4 Population management: Each new individual produced is inserted in the appropriate subpopulation - feasible or infeasible. If a subpopulation reaches the maximum individuals, a Survivor selection is triggered. In addition, penalty parameters for solution infeasibility are adapted in this step.

For further details, see [3].

## 4. The proposed algorithm

In this study, a novel algorithm is proposed which aims to incorporate the benefits of both ICA and HGS-CVRP approaches. Additionally, the algorithm incorporates various modifications to both ICA and HGS-CVRP to further enhance its performance. Prior

Fig. 5. General structure of HGS-CVRP.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[rezaei2023] Combining genetic local search into a multi-population Imperialist Competitive Algorithm for the Capacitated Vehicle Routing Problem_artifacts/image_000006_dd321d4b61c5e4243b4bc8b558f82ef3bb980917de59c943d1e6ba824a72eafb.png)

to the detailed description of the proposed algorithm, the applied refinements will be discussed in the following subsections.

## 4.1. Refined ICA

In order to enhance the adaptability and suitability of the ICA method for addressing the CVRP problem, refinements have been applied to the original ICA.

generated individuals. For CVRP such individuals may be either feasible or infeasible solutions. Given that imperialists play ''Local Optima'' roles in their corresponding empires, initialization needs to ensure that the solutions assigned are in fact feasible solutions i.e. Nimp feasible solutions are generated.

## 4.1.1. Modified ''Create initial empires''

Following the original ICA (see Section 3.3.1), initialization of countries and their assignment to colonies are based on randomly

Eqs. (6) and (7) are applied to calculate the number of colonies for each empire based on their power (or fitness value). These two equations form a normalization equation to ensure that more colonies belong to more powerful empires. It should be noted that the value ''1.3'' is not part of the original ICA, However, it appears in the first implementation of the algorithm in MATLAB [33].

Finally, the Total cost of each empire is calculated, as before, by Eq. (3).

## 4.1.3. Removing the step ''Possess Empires''

maxPo w er = 1 3 .

$$\ast \text{MAX} \left ( \text{Cost} \left ( \text{imperiallist} _ { 1 } \right ), \dots, \text{Cost} \left ( \text{imperiallist} _ { N _ { i m p } } \right ) \right ) \quad ( 6 ) \quad \text{sentati} \quad \\ \text{are a} \, \leq \, \leq$$

$$N. C _ { \cdot n } = \text{round} \left ( \frac { \max P o w e r - Cost \left ( \text{imperialist} _ { n } \right ) } { \sum _ { i = 1 } ^ { N _ { i m p } } ( \max P o w e r - Cost \left ( \text{imperialist} _ { i } \right ) ) } \ast N _ { c o l } \right ) ^ { \quad \text{ are a s} } \\ N. C _ { \cdot n } = \text{round} \left ( \frac { \max P o w e r - Cost \left ( \text{imperialist} _ { n } \right ) } { \sum _ { i = 1 } ^ { N _ { i m p } } ( \max P o w e r - Cost \left ( \text{imperialist} _ { i } \right ) ) } \ast N _ { c o l } \right ) ^ { \quad \text{ are a s} }$$

where N C . . n is the number of colonies of the n th empire and the Cost represents the fitness function (or objective function). Algorithm 1 represents the pseudo code for this step.

## 4.1.2. Defining ''AssimRevol'' as a new step

The 'Assimilation' and 'Revolution' steps of the original ICA are combined and refined to form a new step, '' AssimRevol '' as described in Algorithm 2 . The crossover between the imperialist and a colony (see Section 3.3.2), is adjusted to be performed between two random colonies from the same empire - selected by two Binary Tournament Selections . Such increased randomness in the colony selection supports diversification - see Section 1; avoiding premature convergence of the empires and thus aiming to have a positive effect on the quality of solutions achieved.

Further, when triggered by multi-step restart mechanism - see Section 4.2.1; crossover is applied to the Best Solution Overall and one randomly selected colony so as to increase intensification - see Section 1.

As indicated in line 1 of Algorithm 2 , AssimRevol is performed for each empire. The number of colonies to participate in the AssimRevol is calculated by Eq. (2) see line 2. In lines 3 to 9 the adjustments to the selected colonies are described. First, crossover is performed (lines 4 and 5) either between randomly selected colonies or between the BestSolutionOverall and randomly selected colonies, controlled by the parameter AssimType , line 4. Each colony generated from the crossover operator is passed to the LocalSearch function, HGS-CVRP (lines 6 and 7). Revolution (mutation), population management and penalty management are applied. In line 8, the RestartMechanism is triggered in the case of no improvement in the BestSolutionOverall after nbIter iteartions.

'' Possess Empires '' is step 4 in the original ICA where a colony and its imperialist are exchanged when the colony has a better fitness value (see Section 3.3.4). To avoid this step, a simple representation change is needed where the imperialist and its colonies are a sorted list, with the fittest colony being the imperialist.

## 4.1.4. Modified ''Imperialistic Competition''

Randomized selection is applied to refine step 6 in the original ICA (see Section 3.3.6) to increase diversification (see Section 1). A random colony (rather than the weakest) is selected from the weakest empire for addition to a randomly selected empire (rather than the most powerful one). Further, when the weakest empire loses all its colonies, its imperialist will be seized by a randomly selected empire (rather than the most powerful one).

## 4.2. Refined HGS-CVRP

In order to enhance the performance of the HGS-CVRP method, the single-step restart mechanism is replaced with a proposed multi-step restart mechanism.

## 4.2.1. Proposed multi-step restart mechanism

HGS-CVRP involves a complete restart after a predefined number of generations given no improvement in the BestSolutionOverall . Such a complete restart requires time to regain the fitness level at the point of restart and thus, a need for a more efficient solution was recognized.

The multi-step restart mechanism proposed herein provides two types: Partially and Totally (original). As shown in Fig. 6, in RestartPartially , 25% of the colonies (orange) will be kept and 75% of the colonies (blue) will be regenerated randomly. Further, 25% of the kept colonies (orange) will be selected randomly from Feasible solutions and 75% from Infeasible ones. Infeasible solutions have more potential to become new Feasible solutions, while most of the Feasible solutions, at this stage, have similar fitness values. As described in Algorithm 3 (lines 1-5), the number of resets and last reset value will be stored. The parameter number of resets defines whether RestartPartially will be performed (line 6) or RestartTotally (lines 7 to 9). Line 8 selects how crossover should be applied.

Fig. 6. The process of partially restart.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[rezaei2023] Combining genetic local search into a multi-population Imperialist Competitive Algorithm for the Capacitated Vehicle Routing Problem_artifacts/image_000007_f0438d225347ad636ab540a674c64682110ece89ffcdd6903973ae869b746fda.png)

## ICA Workspace

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[rezaei2023] Combining genetic local search into a multi-population Imperialist Competitive Algorithm for the Capacitated Vehicle Routing Problem_artifacts/image_000008_a6d4f676515b5c9707c3ef19e307ff9c9b65c28de2a72a9523e419f8b4efa60d.png)

Fig. 7. An overview of the proposed algorithm, ICAHGS.

## 4.3. ICAHGS approach

The proposed ICAHGS, seeks to exploit the advantages of both HGS-CVRP and ICA in a single method. As stated, ICA shows strength in global search in its multi-population structure whilst HGS-CVRP benefits from its population management, diversity control and improved local search.

Fig. 7 provides an overview of the proposed ICAHGS algorithm. As shown, ICA is the main algorithm - ICA Workspace; based on a refined ICA - see Section 4.1. Further, within each empire of the ICA Workspace, a refined HGC-CVRP manages the colonies see

Section 4.2. Algorithm 4 presents the ICAHGS algorithm, where the main algorithm is the refined ICA and lines 7 to 10 apply the refined HGS-CVRP to the colonies.

## 5. Experimental results

## 5.1. Experimental setup

The simulator was developed in C++ and compiled with g++ 7.5.0 on Linux Ubuntu 18.04. All experiments were performed on an Intel(R) Core(TM) i5-4590T CPU @ 2.00 GHz with 12 GB RAM.

Table 1 Sweep of ICA parameter.

To enable comparison of experimental results with Vidal's [3], the Tmax applied is scaled up by R = 2186 1646 where / R is the ratio of the single thread rating of CPUs [34]. Therefore, the new time limit will be Tmax = T maxV × R seconds, where n is the number of customers and T maxV = n × 240 100 is the suggested / Tmax by Vidal.

- 4. LoggiBUD [9]: The Loggi Benchmark for Urban Deliveries provides a real-world dataset - simulating the challenges of a large delivery company in the last-mile step of its supply chain in Brazil's largest cities. The 6 instances of the DIMACS VRP challenge 2021 [36] were selected.

To increase statistical significance, 10 independent runs with different seed numbers are performed for each instance. Further, to measure the quality of solutions, the Gap of each algorithm was calculated by Gap = ( Value -BKS ) / BKS where Value is the solution Value provided by the ICAHGS and BKS is the BestKnown Solution value for instance, extracted from the CVRPLIB website [35]. Further, statistical tests - t-tests and the Wilcoxon test, were applied where possible, provided in Section 5.10.

## 5.2. Benchmarks

In this study, 140 benchmark instances, selected from the following benchmarks are applied:

- 1. Uchoa [6]: This benchmark contains 100 instances from 100 to 1000 clients providing a wide range of CVRP characteristics. In this study, the main experiments are performed on this benchmark, and are referred to X-instances.
- 2. CMT [7]: CMT is another well-known benchmark in the literature, containing 14 instances from 50 to 199 customers.
- 3. Golden [8]: Golden is composed of 20 large-scale instances using from 200 to 480 customers where some instances have restrictions on the maximum length of any route.

## 5.3. Tuning the ICA parameters

Two subsets of the parameters ''Initial Imperialists'' ( subset 1 = [ 2 3 5 ) and ''Revolution Rate'' ( , , ] subset 2 = [ 0 4 . , 0 5 . , 0 6 ) were . ] experimentally defined. 5 random instances from different client ranges of X-instances were selected and the ICAHGS was performed for 10 independent runs with different seed numbers. The ξ (zeta) from the original ICA and the HGS-CVRP parameters were kept unchanged from [3,33], respectively. The average performance (Value) and average efficiency (time) - in term of consumed CPU time; are presented in Table 1. The resulting best values for ''Initial Imperialists'' (II) and ''Revolution Rate'' (RR) are 3 and 0.5, respectively. The overview of the ICAHGS parameters are provided in Table 2.

## 5.4. Refined ICA vs. original ICA

In order to assess the performance and efficiency of the proposed refined ICA within ICAHGS, two approaches are considered: ICAHGS with the original ICA (see Section 3.3) and with the refined ICA (see Section 4.1).

Table 2 The parameters of ICAHGS.

|               | Parameter name                                                  | Additional comment                                                                                                                                                                                                                                                    | Value            |
|---------------|-----------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|
| ICA part      | Initial countries Initial imperialists Revolution rate ξ (zeta) | From the original HGS-CVRP From parameter tuning From parameter tuning From the original ICA                                                                                                                                                                          | 100 3 0.5 0.3    |
| HGS-CVRP part | µ (miu) λ (lambda) n Elite n Closest Γ ξ Ref                    | Minimum population size Generation size Number of elite solutions considered in the fitness calculation Number of close solutions considered in the diversity-contribution Granular search parameter Target proportion of feasible individuals for penalty adaptation | 25 40 4 5 20 0.2 |

Table 3

Comparative results to analyze the refined ICA.

Table 4

| Instances   | ICAHGS with original ICA   | ICAHGS with original ICA   | ICAHGS with original ICA   | ICAHGS with refined ICA   | ICAHGS with refined ICA   | ICAHGS with refined ICA   | Improvement   | Improvement   |
|-------------|----------------------------|----------------------------|----------------------------|---------------------------|---------------------------|---------------------------|---------------|---------------|
|             | Best                       | Gap                        | Time                       | Best                      | Gap                       | Time                      | Type          | Percent       |
| X-n106-k14  | 26362                      | 0.00                       | 162.45                     | 26362                     | 0.00                      | 158.10                    | Time          | 2.68          |
| X-n125-k30  | 55539                      | 0.00                       | 241.02                     | 55539                     | 0.00                      | 20.70                     | Time          | 91.41         |
| X-n261-k13  | 26558                      | 0.00                       | 537.42                     | 26558                     | 0.00                      | 336.20                    | Time          | 37.44         |
| X-n284-k15  | 20244                      | 0.14                       | 597.25                     | 20215                     | 0.00                      | 853.60                    | BKS           | -             |
| X-n317-k53  | 78361                      | 0.01                       | 468.53                     | 78355                     | 0.00                      | 153.33                    | BKS           | -             |
| X-n411-k19  | 19739                      | 0.14                       | 857.05                     | 19712                     | 0.00                      | 1100.40                   | BKS           | -             |
| X-n429-k61  | 65458                      | 0.01                       | 1303.23                    | 65449                     | 0.00                      | 571.20                    | BKS           | -             |
| X-n459-k26  | 24145                      | 0.02                       | 811.56                     | 24139                     | 0.00                      | 1358.11                   | BKS           | -             |
| X-n513-k21  | 24201                      | 0.00                       | 864.97                     | 24201                     | 0.00                      | 128.30                    | Time          | 85.17         |
| X-n856-k95  | 89016                      | 0.06                       | 2584.16                    | 88990                     | 0.03                      | 2150.60                   | Gap           | 50.98         |

Comparative results to analyze the multi-step restart mechanism.

| Instances   | ICAHGS (without MSRM)   | ICAHGS (without MSRM)   | ICAHGS (without MSRM)   | ICAHGS (with MSRM)   | ICAHGS (with MSRM)   | ICAHGS (with MSRM)   | Improvement   | Improvement   |
|-------------|-------------------------|-------------------------|-------------------------|----------------------|----------------------|----------------------|---------------|---------------|
|             | Best                    | Gap                     | Time                    | Best                 | Gap                  | Time                 | Type          | Percent       |
| X-n106-k14  | 26386                   | 0.09                    | 58.10                   | 26362                | 0.00                 | 158.10               | BKS           | -             |
| X-n125-k30  | 55539                   | 0.00                    | 21.20                   | 55539                | 0.00                 | 20.70                | Time          | 2.4           |
| X-n261-k13  | 26558                   | 0.00                    | 800.40                  | 26558                | 0.00                 | 336.20               | Time          | 58.0          |
| X-n284-k15  | 20244                   | 0.14                    | 181.10                  | 20215                | 0.00                 | 853.60               | BKS           | -             |
| X-n317-k53  | 78355                   | 0.00                    | 251.80                  | 78355                | 0.00                 | 153.33               | Time          | 39.1          |
| X-n411-k19  | 19718                   | 0.03                    | 277.42                  | 19712                | 0.00                 | 1100.40              | BKS           | -             |
| X-n429-k61  | 65487                   | 0.06                    | 818.20                  | 65449                | 0.00                 | 571.20               | BKS           | -             |
| X-n459-k26  | 24143                   | 0.02                    | 1464.40                 | 24139                | 0.00                 | 1358.11              | BKS           | -             |
| X-n513-k21  | 24201                   | 0.00                    | 127.23                  | 24201                | 0.00                 | 128.30               | Time          | - 0.8         |
| X-n856-k95  | 89006                   | 0.05                    | 408.16                  | 88990                | 0.03                 | 2150.60              | Gap           | 40.0          |

The comparison is performed on 10 random benchmark instances and the results are listed in Table 3. In the table, if BKS is reached, BKS is listed as the improvement; otherwise, the % improvement is listed either with respect to the value achieved or to the gap. If both versions achieved BKS i.e., no improvement is needed, the effect on time is given.

The ICAHGS with refined ICA found 5 BKS. In 4 instances, both algorithms achieved the BKS, but ICAHGS with refined ICA showed better efficiency. Moreover, for X-n856-k95, the performance of ICAHGS with respect to the gap, has improved by more than 50%.

## 5.5. Analysis of multi-step restart mechanism (MSRM)

To study the effect of the multi-step restart mechanism (see Section 4.2.1), the performance and efficiency of ICAHGS without MSRM - allows total restart; and with MSRM - allows both partial and total restart; were compared. The results for the same 10 random instances as in Section 5.4, are reported in Table 4. In 5 instances, MSRM has pushed the algorithm to find BKS. Furthermore, for X-n856-k95, a 40% performance improvement is achieved. For 3 cases, where BKS was achieved in both approaches, the efficiency has improved when applying MSRM.

5.6. ICAHGS versus HGS-CVRP on CMT and golden benchmarks

To study the generality of the results, two further benchmarks are studied: CMT [7] and Golden [8]. The average and the best obtained results at Tmax from 10 independent runs are provided in Appendices B (CMT) and C (Golden).

In Table 5, a summary of the gaps in the average values of solutions among all instances of CMT and Golden benchmarks is presented, along with the number of BKS instances discovered.

Both ICAHGS and HGS-CVRP found 13 BKS out of 14 instances of CMT and the gap for the remaining instance (CMT5) is 0.01% negligible for most applications.

For the golden dataset, ICAHGS achieved 8 BKS, compared to the 7 of HGS-CVRP. Furthermore, considering Average and Best found solutions, ICAHGS performed better for 10 and 8 instances, respectively see Appendix C. In addition, ICAHGS obtained 62% improvement for the Max. Gap of Best solutions and 39.1% for the Max. Gap of Average solutions.

5.7. Evaluating ICAHGS against state-of-the-art algorithms on Uchoa benchmark

ICAHGS is compared to four state-of-the-art methods from the literature: Google OR-Tools [37], KGLS (knowledge guided local

Table 5 Comparative results between HGS-CVRP and ICAHGS on CMT and Golden benchmarks.

Table 6

|               | CMT      | CMT    | Golden   | Golden   |
|---------------|----------|--------|----------|----------|
|               | HGS-CVRP | ICAHGS | HGS-CVRP | ICAHGS   |
| Min Gap       | 0.00     | 0.00   | 0.00     | 0.00     |
| Avg Gap       | 0.0009   | 0.0009 | 0.18     | 0.17     |
| Max Gap       | 0.0123   | 0.0123 | 0.92     | 0.56     |
| Number of BKS | 13       | 13     | 7        | 8        |

Comparing Gaps of average solution values for HGS-CVRP and ICAHGS.

Table 7

| Gaps             | Included instances   |   HGS-CVRP |   ICAHGS |   Improvement (%) |
|------------------|----------------------|------------|----------|-------------------|
| Max Gap Avg. Gap | 100                  |      0.56  |    0.48  |              14.3 |
|                  | 100                  |      0.11  |    0.08  |              27.3 |
| Max Gap          | First 50             |      0.16  |    0.11  |              31.3 |
| Avg. Gap         | First 50             |      0.023 |    0.012 |              47.8 |
| Max Gap          | Last 50              |      0.56  |    0.48  |              14.3 |
| Avg. Gap         | Last 50              |      0.198 |    0.151 |              23.7 |

Comparative results between different algorithms.

|               |   OR-Tools (2020) |   KGLS (2018) |   SISR (2020) |   HGS-CVRP (2020) |   ICAHGS |
|---------------|-------------------|---------------|---------------|-------------------|----------|
| Min Gap       |              0.38 |          0    |          0    |              0    |     0    |
| Avg Gap       |              4.01 |          0.53 |          0.19 |              0.11 |     0.08 |
| Max Gap       |              8.62 |          1.24 |          1.63 |              0.56 |     0.48 |
| Number of BKS |              0    |          6    |         21    |             44    |    51    |

search) [23], SISR (slack induction by string removals) [24], and HGS-CVRP [3]. Google OR-Tools was selected as a baseline due to its reputation as a reliable general-purpose tool in the field of operations research. The remaining algorithms, were chosen for their relevance and timeliness as representative examples of current techniques in the field.

ICAHGS experiments were conducted on 100 benchmark instances from the Uchoa benchmark. The average and best results reported in Vidal' research [3], for each state-of-the-art algorithm on X-instances, are provided for comparison.

Table 6 presents a detailed comparison of the obtained gaps in average solution values for HGS-CVRP and ICAHGS whilst Table 7 summarizes the gaps in average solution values across all instances, as well as the number of BKS instances found. A more comprehensive presentation of the comparative results can be found in Appendix A.

It is clear that ICAHGS achieved higher quality solutions in comparison with its original algorithm (HGS-CVRP) and the remaining of the state-of-the-art algorithms.

The following improvements, compared to HGS-CVRP, may be noted:

- · ICAHGS achieves 51 BKS, whilst HGS-CVRP achieves 44 i.e. a 15.9% comparison (see Table 7).
- · ICAHGS gains 14.3% and 27.3% improvement respectively for Max. and Avg. Gap of Average solutions (see Table 6).
- · In the case of small instances (first 50 instances), the ICAHGS obtains optimal or near-optimal solutions, with 31.3% and 47.8% improvement over HGS-CVRP respectively for Max. and Avg. Gap of Average solutions (see Table 6).
- · In the case of large instances (last 50 instances), the ICAHGS achieves 14.3% improvement for the Max. Gap and 23.7% for the Avg. Gap of average solutions (Table 6)

When comparing the HGS based algorithms to other state of the art algorithms, the following improvements may be noted:

Fig. 8. Performance of ICAHGS and HGS-CVRP over time.

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[rezaei2023] Combining genetic local search into a multi-population Imperialist Competitive Algorithm for the Capacitated Vehicle Routing Problem_artifacts/image_000009_e4d3821c25f8a49bb29c05acd89bf5f824180575cebea0e11c5f547610784d61.png)

- · Both the HGS algorithms achieve more BKS than SISR (21 BKS) and KGLS (6) - see Table 7. Further, it should be noted that OR-Tools did not achieve any BKS.
- · They HGS algorithms achieve much lower Avg. Gap followed by SISR (0.19%), KGLS (0.53%) and OR-Tools (4.01%).

## 5.8. Evolutionary performance of ICAHGS and HGS-CVRP

Fig. 8 compares the Best performance of ICAHGS and HGSCVRP over time - the same compiler and seed numbers are applied to each algorithm and Vidal's source code [38] is applied for HGS-CVRP. As shown, ICAHGS quickly shows superior performance to HGS-CVRP.

Figs. 9 to 12 highlight such performance over time for (a) 2 cases of small instances and (b) 2 cases of large instances. The route of each vehicle in the best-found solution by ICAHGS is displayed to the right of the evolutionary performance graphs. As shown in Fig. 9, both algorithms obtained the BKS at the same time given the small size of the instance. However, as highlighted in Figs. 10 and 11, ICAHGS achieved the best solutions in approximately half the time consumed by the HGS-CVRP i.e. for both a small instance and also a large instance. However, in Fig. 12, neither algorithm could find the BKS. HGS-CVRP has obtained the Gap 0.06 after 1484 s and then has stagnated. On the other hand, ICAHGS reached the Gap 0.06 in 332 s i.e., four times faster; and improvement continues until a superior Gap of 0.03 is achieved.

## 5.9. Real-world application

Solving a real-world problem is the ultimate goal for VRP algorithms. Thus ICAHGS is compared to HGS-CVRP on 6 instances of the LoggiBUD benchmark [9]. The average and the best obtained results at Tmax from 10 independent runs are provided in Appendix D. Table 8 provides a summary of the gaps in the average solution values for both the HGS-CVRP and ICAHGS algorithms as well as the number of BKS achieved. As shown, ICAHGS outperforms HGS-CVRP, achieving better performance in 5 out of the 6 instances for average solutions and 4 out of 6 instances for best solutions.

## 5.10. Statistical analysis

A t-test and the Wilcoxon test are both statistical tests that are used to compare the means of two groups of data. A t-test,

## Performance of best solution for X-nl25-k3o

## Best solution for X-nl25-k3o Cost 55539

Fig. 9. Performance and best-found solution for X-n125-k30 (small).

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[rezaei2023] Combining genetic local search into a multi-population Imperialist Competitive Algorithm for the Capacitated Vehicle Routing Problem_artifacts/image_000010_1387322d2f9759fa46e3ea959cd7409edf6f04e184e84e92e679e6915f66ad6c.png)

Fig. 10. Performance and best-found solution for X-n261-k13 (small).

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[rezaei2023] Combining genetic local search into a multi-population Imperialist Competitive Algorithm for the Capacitated Vehicle Routing Problem_artifacts/image_000011_807e744eaf1f816c9429ca0fce99709a07f97e9055d5b05622afccf874723565.png)

## Performance of best solution for X-n513-k21

## Best solution for X-n513-k21 Cost 24201

Fig. 11. Performance and best-found solution for X-n513-k21 (large).

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[rezaei2023] Combining genetic local search into a multi-population Imperialist Competitive Algorithm for the Capacitated Vehicle Routing Problem_artifacts/image_000012_b39569617ea2056820dd76b9eb52e72ee429d320dff6c46f95fc42d70df7a6c2.png)

also known as Student's t-test, is used to determine whether there is a significant difference between the means of two groups of data [39]. On the other hand, the Wilcoxon test is a nonparametric test that is used when the data is not normally distributed or when the variances of the two groups are not equal [40].

In order to evaluate the performance of the ICAHGS algorithm in comparison to state-of-the-art algorithms (OR-Tools,

## Performance of best solution for X-n856-k95

## Best solution for X-n856-k95 Cost 88990

Fig. 12. Performance and best-found solution for X-n856-k95 (large).

![Image](/Users/dennis/Documents/Studium/Bachelor's Thesis M&T/4_Notes/Bachelor's Thesis M&T Vault/Papers_Converted/0_Artifacts/[rezaei2023] Combining genetic local search into a multi-population Imperialist Competitive Algorithm for the Capacitated Vehicle Routing Problem_artifacts/image_000013_d2fdc6a4a25a8e9a1e5acaa08b9b917f63f7af446023accb8146031cf1170795.png)

Table 8

Comparative results between HGS-CVRP and ICAHGS on 6 instances of LoggiBUD benchmark.

pop and RR refer to the initial population size and Revolution Rate, respectively.

|               |   HGS-CVRP |   ICAHGS |
|---------------|------------|----------|
| Min Gap       |       0.01 |     0.01 |
| Avg Gap       |       0.16 |     0.14 |
| Max Gap       |       0.6  |     0.54 |
| Number of BKS |       1    |     1    |

KGLS, SISR, and HGS-CVRP), both t-test and Wilcoxon test were conducted on the average of solutions obtained from the Uchoa benchmark (as presented in Appendix A) - see Table 9. As shown, with p &lt; 0 05, . the ICAHGS algorithm demonstrates significantly superior performance in comparison to the state-of-the-art algorithms.

Table 10 presents the statistical analysis of ICAHGS performance compared to HGS-CVRP on the Golden, LoggiBUD and CMT benchmarks. ICAHGS shows a significant performance improvement over HGS-CVRP on the Golden and LoggiBUD benchmarks. However, for the CMT benchmark, both ICAHGS and HGS-CVRP were able to find almost all the BKS, resulting in no significant performance difference. It should be noted that, due to the lack of data behind the results for the state-of-the-art algorithms on these benchmarks, it was not possible to make any statistical comparison.

## 5.11. Complexity analysis

Time complexity analysis is a fundamental concept in computer science and is used to understand the performance of algorithms. It refers to the study of how the running time of an algorithm scales with the size of the input data. In this section, the time complexity of the proposed ICAHGS is compared to HGS-CVRP and the original ICA.

As outlined in Algorithm 4 , the AssimRevol step of ICAHGS involves two loops for traversing empires and their colonies, during which OX Crossover, local search, and population management (if necessary) are applied to each colony. On the other hand, HGS-CVRP applies the same operations to each individual in the population, leading to the same time complexity as ICAHGS given the same initial population size. Additionally, ICAHGS includes the ''imperialistic competition'' process, which occurs frequently but does not contain any loops that would impact its time complexity calculations. It is worth noting that each iteration of ICAHGS is equivalent to pop × RR iterations of HGS-CVRP, where

ICAHGS with the original ICA and with the refined ICA, have the same time complexity. However, ICAHGS with the refined ICA exhibits a shorter run time. The run time for the original ICA can be calculated as O ( Iter × Emp × Pop × ( 1 + RR ) ), where Iter is the maximum number of iterations, Emp represents the number of empires, Pop denotes the number of colonies, and RR is the Revolution Rate. On the other hand, the run time for the proposed ICAHGS with refined ICA is calculated as O ( Iter × Emp × Pop × RR ). Given the same number of empires, colonies, and a Revolution Rate of 0.5 (as proposed in the ICAHGS), ICAHGS with refined ICA is 3 times faster. This improvement is achieved by combining the Assimilation and Revolution steps (see 4.1.2). In the original ICA, all colonies within each empire participate in the Assimilation step. In contrast, only pop × RR colonies participate in the refined ICA.

## 5.12. Discussion

The CVRP is an important problem in the operational level of supply chain management, dealing with transportation plans and routing decisions. At the strategic level, one has to deal with high level decisions related to the fleet size, operating with own or leased fleet, defining the number and location of factories, facilities or distribution centers, usually named as depots in the CVRP and multi-depot CVRP formulation.

In this study, a hybrid method, ICAHGS is developed to improve the quality of the solutions for the CVRP problem and the obtained results confirm this claim. However, CVRP is one of the basic cases in the VRP domain. Many constraints that are relaxed cannot be ignored in a real-world application i.e. fixed number of vehicles; distance between nodes (calculated based on Euclidian distance hence ignoring traffic congestions); fixed capacity and cost for all the vehicles and so on.

Nonetheless, despite the mentioned limitations, solving a standard CVRP optimally or near-optimally can have a great impact on providing solutions for real cases based on CVRP. Since applications based on CVRP are normally executed in operational levels, the present research can play a significant role in cost and time savings. Variants of the standard setting can incorporate problemspecific constraints to the problem, and the proposed method can be adapted to these situations simply by making changes in the fitness function. All the ICAHGS algorithm steps can be preserved with none or little modifications. Depending on the application, a

## Table 9

Comparative statistical results between ICAHGS and state-of-the-art algorithms on Uchoa benchmark.

Table 10 Statistical

|          | p-values ICAHGS   | OR-Tools ICAHGS   | ICAHGS & SISR   | ICAHGS & HGS-CVRP   |
|----------|-------------------|-------------------|-----------------|---------------------|
| t-test   | < .001            | < .001            | < .001          | < .001              |
| Wilcoxon | < .001            | < .001            | < .001          | < .001              |

results between ICAHGS and HGS-CVRP.

| Benchmarks   | p-values   |          |
|--------------|------------|----------|
|              | t-test     | Wilcoxon |
| Golden       | < .001     | < .001   |
| LoggiBUD     | .015       | .019     |
| CMT          | Nan        | Nan      |

achieved with ICAHGS compared to HGS-CVRP. Further, the time complexity analysis demonstrates that ICAHGS with the refined ICA, using the same algorithm parameters, is three times faster than the ICAHGS with the original ICA.

well suited VRP variant can be adopted while its implementation might be affected by the complexity of the sought solution. It is worth noting that most of the VRP variants are inherited from the CVRP by considering different constraints and initial conditions. In future work, such other variants of VRP will be investigated.

Additionally, the crossover operator and the problem representation are interdependent in these types of problems. Hence, the decision to retain the order crossover (OX) operator in the ICAHGS approach was based on its effectiveness in the original HGS-CVRP. Furthermore, exploring alternative crossover operators is a desirable and fascinating possibility. However, altering the crossover operator in the original HGS-CVRP could raise concerns regarding the ICAHGS algorithm's performance, thereby casting doubt on the degree to which the improved performance can be attributed to the crossover operator or the hybridization approach. To avoid any such confusion, maintaining the original HGS-CVRP with the OX crossover operator is opted, allowing for a more precise attribution of the ICAHGS's superior performance to the proposed hybridization approach. Evaluating different crossover operators in ICAHGS would necessitate testing them in HGS-CVRP as well, making it more appropriate for another research paper with a distinct research objective.

The current discourse highlights the significance of evaluating various crossover operators in addressing the traveling salesman problem (TSP) and vehicle routing problem (VRP). Prior literature has explored this topic, with several studies discussing the impact of different crossover operators on the aforementioned problems. One such study conducted by Puljić et al. [41] compared the performance of eight evolutionary crossover operators, including order crossover (OX), partially mapped crossover (PMX), and edge recombination crossover (ERX), on the VRP. The researchers concluded that although PMX and ERX are popular in the context of TSP, they are not the most effective for the VRP. According to their study, PMX has inferior performance compared to OX and ERX. Additionally, Wibisono et al. [42] conducted experiments that demonstrate the superiority of OX over PMX in terms of average results and solution diversity.

## 6. Conclusion

In this study, a hybrid algorithm (ICAHGS) for solving CVRP is proposed, combining a refined ICA together with a refined HGS-CVRP. Comparison with other state-of-the-art algorithms on the Uchoa benchmark confirm that the combination of these algorithms results in a powerful algorithm for CVRP, in terms of performance and efficiency. Further experiments on the CMT and Golden benchmarks provided increased generality, confirming performance improvement on other datasets with respect to the HGS-CVRP algorithm. Finally, real-world testing on the LoggiBUD benchmark further confirmed the enhanced performance

As stated, applying ICAHGS on other VRP variants such as Multi Depot VRP (MDVRP) and VRP with Time Window (VRPTW) is planned for future work. Further, Linear Programming (LP) combined with such a metaheuristic approach may be an interesting avenue for further research. The interdependence between the problem representation and the crossover operator highlights the potential for further improvement through the exploration of alternative crossover methods. To this end, assessing various crossover operators in the context of the ICAHGS approach is suggested as an interesting topic for future investigation.

## CRediT authorship contribution statement

Babak Rezaei: Conceptualization, Methodology, Software, Formal analysis, Investigation, Writing - original draft, Writing review &amp; editing, Visualization. Frederico Gadelha Guimaraes: Conceptualization, Methodology, Writing - review &amp; editing, Validation, Supervision, Project administration. Rasul Enayatifar: Conceptualization, Methodology, Writing - review &amp; editing, Validation, Supervision. Pauline C. Haddow: Conceptualization, Methodology, Writing - review &amp; editing, Validation, Supervision.

## Declaration of competing interest

The authors declare that they have no known competing financial interests or personal relationships that could have appeared to influence the work reported in this paper.

## Data availability

No data was used for the research described in the article.

## Acknowledgment

The author would like to acknowledge Thibaut Vidal for providing access to the source code for HGS-CVRP.

Appendix A. Comparative results between OR-Tools, KGLS, SISR, HGS-CVRP, ICAHGS and BKS

$$\text{See Table A.1.}$$

Appendix B. Comparative results HGS-CVRP, ICAHGS and BKS for CMT benchmark ( Christofides, Mingozzi and Toth , 1979)

See Table B.1.

Appendix C. Comparative results HGS-CVRP, ICAHGS and BKS for Golden benchmark ( Golden et al. 1998 )

$$\text{See Table C.1.}$$

Appendix D. Comparative results HGS-CVRP, ICAHGS and BKS for Loggi benchmark for urban deliveries ( Loggibud, 2021 )

See Table D.1.

Table A.1 Comparative results between OR-Tools, KGLS, SISR, HGS-CVRP, ICAHGS and BKS.

| Instance                                                | OR-Tools                                          | (2020)                        |                          | KGLS (2018)                               |                                    |                                            | SISR (2020)                      |                                                     | HGS-CVRP                                 | (2020)                        |                            | ICAHGS                                                   | BKS                                        |                             |
|---------------------------------------------------------|---------------------------------------------------|-------------------------------|--------------------------|-------------------------------------------|------------------------------------|--------------------------------------------|----------------------------------|-----------------------------------------------------|------------------------------------------|-------------------------------|----------------------------|----------------------------------------------------------|--------------------------------------------|-----------------------------|
| X-n101-k25                                              | Avg 27977.2                                       | Gap Best 1.40 27865           | Avg 27631.9              |                                           | Gap 0.15                           | Best                                       | Avg                              | Gap Best 0.01 27591                                 | Avg                                      |                               | Gap Best 0.00 27591        | Avg Gap 27591.0                                          | Best                                       |                             |
| X-n106-k14 X-n110-k13                                   | 26757.5                                           | 1.50                          | 26747 14986              | 26413.2                                   | 0.19                               | 27595 26375                                | 27593.3 26380.9                  | 0.07 26368                                          |                                          | 27591.0 26381.4               | 0.07 26364 14971           | 0.00 26362.0 0.00 14971.0                                | 27591 26362 14971                          | 27591 26362                 |
| X-n115-k10                                              | 15099.8 12808.3                                   | 0.86 0.48                     | 12768                    | 14971.0 12747.1                           | 0.00 0.00                          | 14971 12747                                | 14972.1 12747.0                  | 0.01 0.00                                           | 14971 12747                              | 14971.0 12747.0               | 0.00 0.00 12747            | 0.00 12747.0 0.00                                        | 12747 13332                                | 14971 12747                 |
| X-n120-k6 X-n125-k30                                    | 13501.9 56853.4                                   | 1.27 2.37                     | 13458 56601              | 13332.0 55740.8                           | 0.00 0.36                          | 13332 55670                                | 13332.0 55559.8                  | 0.00 13332 0.04 55539                               | 13332.0 55539.0                          | 0.00 0.00                     | 13332 55539                | 13332.0 0.00 55539.0 0.00                                | 55539                                      | 13332 55539                 |
| X-n129-k18                                              | 29722.3                                           | 2.70                          | 29668                    | 28971.6                                   | 0.11                               | 28954                                      | 28948.9                          | 0.03 28940 10918                                    | 28940.0 10916.0                          | 0.00                          | 28940 10916                | 28940.0 0.00                                             | 28940 10916                                | 28940                       |
| X-n134-k13 X-n139-k10                                   | 11171.0 13741.2                                   | 2.34 1.11                     | 11096 13693              | 10940.5 13590.0                           | 0.22 0.00                          | 10930 13590                                | 10937.7 13590.4                  | 0.20 0.00 13590                                     | 13590.0                                  |                               | 0.00 0.00 13590 0.00       | 10916.0 0.00 13590.0 0.00                                | 13590 15700                                | 10916 13590                 |
| X-n143-k7 X-n148-k46                                    | 16135.6 44598.5                                   | 2.77 2.65                     | 16019 44334              | 15730.6 43588.3                           | 0.19 0.32                          | 15726 43507                                | 15727.8 43464.1                  | 0.18 15700 0.04 43448 0.04                          | 15700.0 43448.0 21225.0                  | 0.00                          | 15700 43448                | 15700.0 0.00 43448.0 0.00 0.02                           | 43448 21220                                | 15700 43448                 |
| X-n153-k22 X-n157-k13                                   | 21789.3 17137.7                                   | 2.68 1.55                     | 21605 17086              | 21386.0 16877.5                           | 0.78 0.01                          | 21375 16876                                | 21228.6 16878.2                  | 21225 0.01 16876                                    | 16876.0                                  | 0.02 0.00                     | 21225 16876                | 21225.0 16876.0 0.00                                     | 16876                                      | 21220 16876                 |
| X-n162-k11 X-n167-k10                                   | 14262.2 21176.4                                   | 0.88 3.01                     | 14238 21158              | 14147.0 20586.9                           | 0.06                               | 14147                                      | 14159.0 20558.6                  | 0.15 14138 0.01 20557                               | 14138.0 20557.0                          | 0.00 0.00 0.00                | 14138 20557 45607          | 14138.0 0.00 20557.0 0.00                                | 14138 20557                                | 14138 20557                 |
| X-n172-k51                                              | 46874.9                                           | 2.78                          | 46695                    | 45802.8                                   | 0.15 0.43                          | 20557                                      |                                  | 0.03                                                |                                          |                               |                            | 45607.0 0.00                                             | 45607                                      | 45607                       |
| X-n176-k26                                              | 49260.2                                           | 3.03                          | 48986 25787              | 47991.6                                   | 0.38                               | 45763 47958                                | 45622.6 47823.7                  | 45607 0.02 47812                                    | 45607.0 47812.0                          | 25569.0                       | 0.00 47812 25569           | 47812.0 0.00                                             | 47812                                      | 47812                       |
| X-n181-k23 X-n186-k15                                   | 25935.6 24908.0                                   | 1.43 3.16                     | 24908 17380              | 25602.3 24178.3                           | 0.13 0.14                          | 25594 24156                                | 25575.1 24166.2                  | 0.02 25569 0.09 24151 0.02                          | 24145.0                                  | 0.00                          | 0.00 24145 16980           | 25569.0 0.00 24145.0 0.00 0.01                           | 25569 24145 16980                          | 25569 24145                 |
| X-n190-k8                                               | 17421.9                                           | 2.60                          |                          | 17033.5                                   | 0.32                               | 17001                                      | 16982.8                          | 16980 44241                                         | 16983.3                                  | 0.02 0.00                     | 44225                      | 16981.7 0.00                                             |                                            | 16980                       |
| X-n195-k51 X-n200-k36                                   | 46151.1 60447.9                                   | 4.36 3.19                     | 45757 60338              | 44427.2 58828.0                           | 0.46 0.43                          | 44396 58756                                | 44292.0 58635.6                  | 0.15 0.10 0.45                                      | 44225.0 58578.0                          | 0.00                          |                            | 44225.0 58578.0 19565.0                                  | 44225 58578                                | 44225 58578                 |
| X-n204-k19 X-n209-k16                                   | 20348.4 31775.5                                   | 4.00                          | 20212 31740              | 19621.0                                   | 0.29                               | 19581                                      | 19653.2                          | 58587 19565                                         | 19565.0 30656.0                          | 0.00 0.00                     | 58578 19565 30656          | 0.00 0.00 0.00                                           | 19565                                      | 19565                       |
| X-n214-k11                                              | 11374.0                                           | 3.65 4.77                     | 11228                    | 30709.7 10944.3                           | 0.18                               | 30685                                      | 30661.7 10894.4                  | 0.02 30656 0.35 0.02                                | 10860.5                                  | 0.04                          | 10856                      | 30656.0 10860.0 117595.0                                 | 30656 10856                                | 30656 10856                 |
| X-n219-k73 X-n223-k34                                   | 118038.0 42046.6                                  | 0.38 3.98                     | 117924 41794             | 117689.1                                  | 0.81 0.08                          | 10913 117651 40686                         | 117623.7 40535.5                 | 10874 117596 0.24                                   | 117596.1                                 | 0.00                          | 117595                     | 0.04 0.00 40437.0                                        | 117595                                     | 117595                      |
| X-n228-k23 X-n233-k16                                   | 26613.4 19883.9                                   | 3.39                          | 26396 19682              | 40714.4 25836.8 19328.6                   | 0.69 0.37                          | 25808 19268                                | 25814.3 19285.7                  | 40504 0.28 25782 0.29                               | 40437.0 25742.8                          | 0.00 0.00                     | 40437 25742                | 0.00 25742.0 0.00 19230.0 0.00                           | 40437 25742 19230                          | 40437 25742                 |
| X-n237-k14 X-n242-k48                                   | 27927.5 85518.0 38282.8                           | 3.40 3.27 3.34                | 27809 85518              | 27095.9 83209.2                           | 0.51 0.20                          | 27044 83136                                | 27081.1 82885.6                  | 19232 0.14 27043 0.16 82805                         | 19230.0 27042.0 82806.0                  | 0.00 0.00 0.07 0.01           | 19230 27042 82771          | 27042.0 0.00 82771.8                                     | 27042 82764 37274                          | 19230 27042 82751           |
| X-n247-k50 X-n251-k28                                   | 40087.6                                           |                               | 37853 40007              |                                           | 0.55                               |                                            |                                  |                                                     | 37277.1                                  |                               |                            | 37275.0                                                  | 38684                                      | 37274 38684                 |
| X-n256-k16                                              |                                                   | 2.71                          |                          | 37388.4 38893.3 18891.6                   | 0.31                               | 37317 38847                                | 37379.6                          | 37274 38687                                         |                                          |                               | 37274                      | 0.03 0.00 38684.0                                        | 18839                                      |                             |
| X-n261-k13                                              | 19294.5 27920.6                                   | 3.63 2.42 5.13                | 19067 27760              | 26717.5                                   | 0.54 0.28                          | 18888                                      | 38765.2 18887.3                  | 0.28 0.21 0.26 18880 0.14                           | 38689.9 18839.6                          | 0.02 0.00 0.00                | 38684 18839 26558          | 0.00 18839.4 0.00 26558.1                                | 26558 75525                                | 18839 26558 75478           |
| X-n266-k58 X-n270-k35                                   | 77660.8 36700.5                                   |                               | 77275 36401              | 75954.6 35462.1                           | 0.60                               | 26671                                      | 26595.8                          | 26558 75549 35325                                   | 26558.2 75564.7 35303.0                  |                               | 75478 35303                | 0.00 75545.9 0.09 0.03                                   |                                            |                             |
| X-n275-k28 X-n280-k17                                   | 22087.3                                           | 2.89 3.99 3.96                | 21918                    | 21299.4                                   | 0.63 0.48 0.26                     | 75793 35447 21265                          | 75609.2 35364.4 21250.5          | 0.17 0.21 0.03 21245                                |                                          | 0.11 0.03 0.00                | 21245 33506                | 35303.0 21245.0                                          | 35303                                      | 35291 21245                 |
| X-n284-k15 X-n289-k60                                   | 35055.6 21137.9                                   | 4.63 4.57                     | 34859 20872              |                                           |                                    |                                            | 33648.6                          | 33545                                               | 21245.0 33543.2 20245.5                  | 0.12                          | 20231 95242                | 0.00 33513.9 0.03 20236.0 0.10 0.11                      | 21245 33503 20215 95204                    | 33503                       |
| X-n294-k50                                              | 98560.9                                           | 3.58                          | 97868                    | 33670.1 20360.0 95882.8 47454.1           | 0.50                               | 33598 20323 95770                          | 20287.6                          | 0.43 0.36 20261 0.20 95245 0.19                     |                                          | 0.15 0.16                     | 47167                      | 95251.7 0.01                                             | 47161                                      | 20215 95151                 |
| X-n298-k31                                              | 49301.8 36970.5 22573.7                           | 4.54 8.00 3.85                | 49010 36296              | 34377.4                                   | 0.72 0.77                          |                                            | 95345.8                          | 0.11                                                | 95300.9                                  |                               | 34231 21739                | 47167.6 34231.0 0.00                                     |                                            | 47161 34231 21736           |
| X-n303-k21 X-n308-k13                                   |                                                   |                               | 22376                    |                                           | 0.62 0.43 0.77                     | 47413 34359 21845                          | 47251.9 34267.8 21772.9          |                                                     |                                          | 0.05 0.01 0.06 0.05           |                            | 21741.3 0.02 0.02                                        |                                            | 25859                       |
| X-n313-k71                                              | 27141.4                                           | 4.96                          | 26934                    |                                           | 0.84                               | 25999                                      | 26281.0                          | 47199 34234 21753 26224                             | 47184.1 34234.8 21748.5 25870.8          |                               | 25862 94045                | 25864.4 0.02                                             | 34231 21738 25861 94044 78355              | 94043                       |
| X-n317-k53 X-n322-k28                                   | 97497.4 79211.0                                   | 3.67 1.09                     | 96958 78863              | 21903.4 26076.4 94763.8 78413.5           | 0.77 0.07                          | 94652                                      | 94155.7                          | 0.17 1.63 0.12 94098 0.04 0.20                      | 94112.2 78355.4                          | 0.07 0.00 0.05                | 78355 29834                | 94060.9 78355.0 0.00 29848.0 0.05 0.03                   | 29834 27532                                | 78355 29834                 |
| X-n327-k20 X-n331-k15 X-n336-k84                        | 31488.5 28777.6 32648.2                           | 5.55 4.52                     | 30932 28592 32493 142905 | 30038.0 27646.8 31200.1                   | 0.68 0.42 0.32                     | 78391 30010 27613 31111                    | 78386.1 29892.5                  | 78361 29861 27611 31122 139272                      | 29848.7 27540.8                          | 0.03 0.00 0.12                | 27532                      | 27539.2 31103.0                                          |                                            | 27532                       |
| X-n344-k43 X-n351-k40                                   | 143294.8 44036.4 27433.6                          | 4.97 3.01                     | 43560 27093              | 140831.3 42350.5 26190.7                  | 1.24 0.71 1.14                     |                                            | 27644.7 31124.5                  | 0.41 0.07 0.23 0.17 0.31                            |                                          | 31103.0 139273.5 42075.6      | 31102 42061 25924          | 0.00 139195.8 0.06 42058.4 0.02 0.14                     | 31102 139139                               | 31102 139111                |
| X-n359-k29 X-n367-k17                                   | 53858.4 23874.0                                   | 4.72                          | 53541                    |                                           | 0.77                               | 140716 42229 26150 51662                   | 139429.8 42122.7 25976.5 51549.8 | 42081 0.09 0.10                                     |                                          | 0.06 0.18                     | 139205 51566               | 25933.2 51554.1 0.10 0.00                                | 42055                                      | 42050 25896 51505 22814     |
| X-n376-k94 X-n384-k52                                   | 148775.7                                          | 5.94 4.57                     | 23597 148630             |                                           | 0.57                               | 22867                                      | 22836.1                          | 25965 51514 22821                                   | 25943.6 51620.0                          |                               |                            | 22814.0 0.00 0.07                                        | 25924 51515 22814                          | 65938 38260                 |
| X-n393-k38 X-n401-k29                                   | 69022.0 40785.6                                   | 4.65                          | 68550 40303              | 51901.3 22944.7 147854.1 66443.0          | 0.10 0.77                          | 66363                                      | 66113.6 38384.5                  | 0.03 0.27 0.33 0.13                                 | 22814.0 147714.5 66049.1                 | 0.22 0.00 0.00 0.17 0.00      | 22814 65997 38260          | 0.00 0.05                                                |                                            | 147713 66154                |
| X-n411-k19                                              | 68249.2 20810.6                                   | 0.72 4.68 6.60 3.17           | 67913                    | 38466.4 66501.9                           | 0.54 0.53                          | 147801 38433 66466                         | 147763.5 66239.5                 | 147736 66046                                        |                                          | 0.15 0.04                     | 147713 66209 19716         | 147715.5 65983.4 38260.0 66190.2 19718.7 107838.0        | 147713 107798                              | 19712                       |
|                                                         | 111594.0                                          | 5.57                          | 20571                    | 19924.8                                   | 1.08                               | 19782                                      | 19776.7                          | 0.33 0.05                                           | 38260.0 66252.5 19720.3                  |                               | 107810                     | 0.03 0.04                                                | 65968 38260 66166 19712 65449              | 107798 65449                |
| X-n420-k130 X-n429-k61 X-n439-k37                       | 68858.4 37655.3 58427.1                           | 3.52 5.21 3.47 5.78           | 110857 68113 37171 58066 | 108295.3 65857.5 36483.8                  | 0.46 0.62 0.26                     | 108175 65795 36445                         | 107853.4 65539.3 36457.7         | 38338 66222 19757 107809 0.14 65494 0.18 36402 0.28 | 107839.8 65502.7 36395.5                 | 0.04 0.08 0.01 0.25           | 65484 36395 55306          | 65457.3 0.01 36397.8 0.02 0.16 0.10 0.11                 | 36395 55280 24139                          | 36391 55233                 |
| X-n449-k29 X-n459-k26 X-n469-k138 X-n480-k70 X-n491-k59 |                                                   |                               | 25435 230460 92457 69944 | 55770.7                                   | 0.97 0.46 0.74 0.60 1.00           | 55675                                      | 55388.8 24228.3 89515.1 66606.9  | 55296 0.37 0.19 0.07                                | 55368.5                                  | 0.10 0.16 0.08                | 24139 221916 89498 66569   | 55322.0 24162.4 222073.6 89498.5 66558.3 69237.3 24201.0 | 221987 89464                               | 24139 89449 66483 69226     |
| X-n502-k39                                              | 25834.9                                           | 7.03                          |                          | 24251.0                                   |                                    | 24234 89926 67034                          | 69271.4                          | 24187 222090 89458 0.19                             | 24163.8 89524.4 66641.5 69239.5          |                               | 69230                      |                                                          |                                            | 221824                      |
|                                                         | 230963.3 92923.0                                  | 4.12 3.88 6.52 1.36 6.80      | 223468.0 89986.3         |                                           | 0.16                               | 223086                                     | 24293.9                          | 222253.9 0.07                                       | 222170.1 24201.0                         |                               |                            | 0.06 0.11 0.02 0.00 0.05                                 |                                            | 24201                       |
| X-n513-k21 X-n524-k153 X-n536-k96                       | 70817.2 70166.5 25845.9 156897.0                  | 1.49 4.99                     | 70032 25295 156322 98815 | 67145.6 69333.9                           | 0.66 0.72                          | 69307 24293 155422                         | 154894.6 95145.9                 | 66502 69238 0.38 24237 154758 95071                 | 154747.6 95091.9                         | 0.24 0.02 0.00 0.10 0.26      | 24201 154646 95040         | 154666.4 0.22 0.06                                       | 66520 69228 24201 154610 95014 86727 42723 | 94846 86700                 |
| X-n548-k50                                              |                                                   | 3.09 7.12                     | 89066 45330              | 24360.7 155699.6 95864.7 86938.6 43031.7  |                                    | 95781 86901                                | 86789.1                          | 86710 42799                                         | 86778.4 42742.7                          | 0.09 0.06 0.28                | 86710 42726                | 0.06 0.24                                                |                                            | 42717                       |
| X-n561-k42 X-n573-k30                                   | 99575.6 89382.6 45758.6 52436.5 198347.2          | 3.48 4.22                     | 52080 197853             | 50957.2 191411.4                          | 1.07 0.28 0.74 0.56 0.58           | 42989 50849                                | 42875.0 50842.6                  | 0.20 0.32 0.10 0.37 0.33 0.17 0.22                  |                                          | 0.14 0.19                     | 50757                      | 95052.0 86755.7 42742.3                                  |                                            | 154593 50673                |
| X-n586-k159 X-n599-k92 X-n613-k62                       | 113380.7 64073.6 64897.9                          | 4.55 7.62 4.40                | 112831 63561 64590       | 109356.1 60201.2                          | 0.83 1.12                          | 191260 109125 60111                        | 190640.1 59705.6 62291.8         | 50777 190454                                        | 50813.0 190588.1 59696.3 62371.6 63874.2 | 0.27 0.33 0.30                | 190470 59636 62238         | 50796.2 190484.1 108652.1 59660.3 62271.1                | 50741 190405 108586 59627 62242 63741      | 59535 62164                 |
| X-n627-k43 X-n641-k35 X-n655-k131                       | 66862.3 107815.9                                  | 4.99 0.97                     | 66652 107710             |                                           | 62486 63952                        |                                            | 108684.8 63851.8 106841.6        | 108598 59609 62221 63802 106808                     | 108656.0                                 | 0.03 0.30                     | 108605 63782 106785        | 0.09 0.19 0.21 0.17 0.23 0.03                            |                                            | 190316 108451 63684         |
| X-n670-k130 X-n685-k75                                  |                                                   |                               | 151071 73090             | 62568.1 64094.3 106956.8 147654.2 68854.7 | 0.65 0.64 0.17 0.90 0.95           |                                            | 68379.6                          | 0.29 0.21 0.26 0.06 0.43 0.26 0.16                  | 68343.1 82237.3                          |                               |                            | 63830.3                                                  |                                            | 106780 68205                |
| X-n701-k44 X-n716-k35                                   |                                                   |                               | 86604 45704              |                                           | 0.72 0.82 0.82                     | 106936 147477 68628 82447                  | 146961.5 82053.9 43492.0         | 146676                                              | 106808.8 146777.7 43505.8                |                               | 146640 68288 82075 43459   | 106808.1 146732.1 68320.8 82121.0 43481.1                | 106802 146624 68273 82032 43451 136314     | 81923 43373                 |
| X-n733-k159 X-n749-k98                                  | 151874.1 74085.5 87060.3 46012.9 143829.1 82813.4 | 3.79 8.62 6.27 6.09 5.61 7.18 | 142650 82083             | 82513.5 43730.4 137299.3 78211.9          | 0.27 0.48                          | 77350 72619 73393 88990 99521 53997 329671 | 136445.2 77534.9 114836.0        | 68271 82007 43449 136344 77399 114751               | 136426.9 77655.4                         | 0.20 0.38 0.31 0.18 0.50 0.30 | 136323 77563               | 0.17 0.24 0.25 0.15 0.34 0.26                            |                                            | 146332 77269 72386          |
| X-n766-k71 X-n783-k48                                   | 123106.2 77518.9                                  | 7.59 7.09                     | 121645 76764             | 1.22                                      | 43627 78109 72974                  | 137185 115011                              | 72637.3                          | 0.27 72544                                          | 72790.7 73500.4                          | 0.56 0.27                     |                            | 136389.9 77532.1 114719.0 72736.2                        |                                            |                             |
| X-n801-k40 X-n819-k171                                  | 76428.2 165074.0                                  | 4.26 4.40                     | 76262 164377             | 115186.0 73043.8 73590.5 159572.5         |                                    |                                            |                                  | 0.19 0.34 0.37 0.35 0.15 0.19 0.11                  |                                          | 0.25 0.26                     | 72704 73396                | 73450.1 158503.1                                         |                                            | 136187 114417 73305         |
| X-n837-k142 X-n856-k95                                  |                                                   | 201518                        | 91109                    |                                           | 0.67 0.91 0.39 0.92 0.72 0.41 0.82 |                                            |                                  | 0.16 0.19 0.39                                      | 114764.5 158511.6 194231.3 89037.5       | 0.08                          | 114679 88986               | 194252.2 89024.7 99675.2                                 | 114646 158368 194087                       | 88965 99299                 |
| X-n876-k59 X-n895-k37                                   | 201836.8 91613.9 103576.1 58191.7                 | 4.18 2.98 4.31                | 103017                   | 195135.0 89333.5 100115.7 54306.0         | 0.83                               | 73500 159396 89218 54240                   | 73412.0 89111.1 99484.5 54072.3  | 73362                                               |                                          | 99682.7 54070.6               |                            | 54070.5 329830.3 133085.7                                | 0.20                                       |                             |
| X-n916-k207                                             |                                                   | 57607                         | 340947                   |                                           | 0.59                               |                                            | 158424.5 193946.6 329584.3       | 158344 193868 89042 99405 53982 329418 133190       | 329852.0                                 | 0.39 0.39 0.20                | 158391 194103 99596 54023  | 0.24 0.27 0.07 0.38 0.39 0.20 0.28                       | 85465                                      |                             |
| X-n936-k151                                             | 342127.2                                          | 8.04 3.93                     |                          | 331111.0 133831.4                         | 0.84                               | 194988 100048 331006 133713                | 133497.1                         | 85493                                               | 133369.9                                 | 0.49                          |                            |                                                          | 133047 85513 119179 118976                 | 158121 193737 329179 132715 |
| X-n957-k87 X-n979-k58                                   | 140479.3 88603.0 123885.2                         | 5.85 3.67 4.13 123379         | 139456 88222             | 85746.6 119600.1                          | 0.33 0.52 119559                   | 85656 119247.5                             | 85559.8 119108.2                 | 0.12 0.59 0.11 0.11 119065                          | 85550.1                                  | 0.10 0.23                     | 329572 133121 85506 119180 | 85545.3 0.09 119244.2 0.23                               |                                            |                             |

Table B.1 Comparative results HGS-CVRP, ICAHGS and BKS for CMT benchmark.

| Instance   | HGS-CVRP (2020)   | HGS-CVRP (2020)   | HGS-CVRP (2020)   | HGS-CVRP (2020)   | ICAHGS   | ICAHGS   | ICAHGS   | ICAHGS   | BKS     |
|------------|-------------------|-------------------|-------------------|-------------------|----------|----------|----------|----------|---------|
| Instance   | Avg               | Gap Avg.          | Best              | Gap Best          | Avg      | Gap Avg. | Best     | Gap Best | BKS     |
| CMT1       | 524.61            | 0.00              | 524.61            | 0.00              | 524.61   | 0.00     | 524.61   | 0.00     | 524.61  |
| CMT2       | 835.26            | 0.00              | 835.26            | 0.00              | 835.26   | 0.00     | 835.26   | 0.00     | 835.26  |
| CMT3       | 826.14            | 0.00              | 826.14            | 0.00              | 826.14   | 0.00     | 826.14   | 0.00     | 826.14  |
| CMT4       | 1028.42           | 0.00              | 1028.42           | 0.00              | 1028.42  | 0.00     | 1028.42  | 0.00     | 1028.42 |
| CMT5       | 1291.45           | 0.01              | 1291.45           | 0.01              | 1291.45  | 0.01     | 1291.45  | 0.01     | 1291.29 |
| CMT6       | 555.43            | 0.00              | 555.43            | 0.00              | 555.43   | 0.00     | 555.43   | 0.00     | 555.43  |
| CMT7       | 909.68            | 0.00              | 909.68            | 0.00              | 909.68   | 0.00     | 909.68   | 0.00     | 909.68  |
| CMT8       | 865.95            | 0.00              | 865.95            | 0.00              | 865.95   | 0.00     | 865.95   | 0.00     | 865.95  |
| CMT9       | 1162.55           | 0.00              | 1162.55           | 0.00              | 1162.55  | 0.00     | 1162.55  | 0.00     | 1162.55 |
| CMT10      | 1395.85           | 0.00              | 1395.85           | 0.00              | 1395.85  | 0.00     | 1395.85  | 0.00     | 1395.85 |
| CMT11      | 1042.12           | 0.00              | 1042.12           | 0.00              | 1042.12  | 0.00     | 1042.12  | 0.00     | 1042.12 |
| CMT12      | 819.56            | 0.00              | 819.56            | 0.00              | 819.56   | 0.00     | 819.56   | 0.00     | 819.56  |
| CMT13      | 1541.14           | 0.00              | 1541.14           | 0.00              | 1541.14  | 0.00     | 1541.14  | 0.00     | 1541.14 |
| CMT14      | 866.37            | 0.00              | 866.37            | 0.00              | 866.37   | 0.00     | 866.37   | 0.00     | 866.37  |

## Table C.1

Comparative results HGS-CVRP, ICAHGS and BKS for Golden benchmark.

| Instance   | HGS-CVRP (2020)   | HGS-CVRP (2020)   | HGS-CVRP (2020)   | HGS-CVRP (2020)   | ICAHGS   | ICAHGS   | ICAHGS   | ICAHGS   | BKS      |
|------------|-------------------|-------------------|-------------------|-------------------|----------|----------|----------|----------|----------|
| Instance   | Avg               | Gap Avg.          | Best              | Gap Best          | Avg      | Gap Avg. | Best     | Gap Best | BKS      |
| Golden_1   | 5623.47           | 0.00              | 5623.47           | 0.00              | 5623.47  | 0.00     | 5623.47  | 0.00     | 5623.47  |
| Golden_2   | 8436.24           | 0.38              | 8412.83           | 0.10              | 8417.20  | 0.15     | 8406.33  | 0.02     | 8404.61  |
| Golden_3   | 11036.20          | 0.35              | 11036.20          | 0.35              | 11036.20 | 0.35     | 11036.20 | 0.35     | 10997.80 |
| Golden_4   | 13624.50          | 0.26              | 13624.50          | 0.26              | 13624.50 | 0.26     | 13624.50 | 0.26     | 13588.60 |
| Golden_5   | 6460.98           | 0.00              | 6460.98           | 0.00              | 6460.98  | 0.00     | 6460.98  | 0.00     | 6460.98  |
| Golden_6   | 8412.90           | 0.15              | 8412.90           | 0.15              | 8412.90  | 0.15     | 8412.90  | 0.15     | 8400.33  |
| Golden_7   | 10195.60          | 0.92              | 10195.60          | 0.92              | 10159.55 | 0.56     | 10132.90 | 0.30     | 10102.70 |
| Golden_8   | 11635.30          | 0.00              | 11635.30          | 0.00              | 11635.30 | 0.00     | 11635.30 | 0.00     | 11635.30 |
| Golden_9   | 580.39            | 0.12              | 579.71            | 0.00              | 580.30   | 0.10     | 579.71   | 0.00     | 579.70   |
| Golden_10  | 737.83            | 0.33              | 736.60            | 0.16              | 737.39   | 0.27     | 736.00   | 0.08     | 735.43   |
| Golden_11  | 913.80            | 0.20              | 913.14            | 0.13              | 913.60   | 0.18     | 913.06   | 0.12     | 911.98   |
| Golden_12  | 1105.86           | 0.42              | 1104.95           | 0.34              | 1104.74  | 0.32     | 1103.34  | 0.19     | 1101.24  |
| Golden_13  | 857.19            | 0.00              | 857.19            | 0.00              | 857.19   | 0.00     | 857.19   | 0.00     | 857.19   |
| Golden_14  | 1080.55           | 0.00              | 1080.55           | 0.00              | 1080.55  | 0.00     | 1080.55  | 0.00     | 1080.55  |
| Golden_15  | 1339.47           | 0.16              | 1339.07           | 0.13              | 1339.36  | 0.16     | 1337.84  | 0.04     | 1337.27  |
| Golden_16  | 1614.80           | 0.22              | 1613.25           | 0.12              | 1614.50  | 0.20     | 1612.57  | 0.08     | 1611.28  |
| Golden_17  | 707.76            | 0.00              | 707.76            | 0.00              | 707.76   | 0.00     | 707.76   | 0.00     | 707.76   |
| Golden_18  | 995.13            | 0.00              | 995.13            | 0.00              | 995.13   | 0.00     | 995.13   | 0.00     | 995.13   |
| Golden_19  | 1365.90           | 0.02              | 1365.63           | 0.00              | 1365.65  | 0.00     | 1365.60  | 0.00     | 1365.60  |
| Golden_20  | 1818.68           | 0.06              | 1817.89           | 0.02              | 1818.37  | 0.04     | 1817.89  | 0.02     | 1817.59  |

## Table D.1

Comparative results HGS-CVRP, ICAHGS and BKS for Loggi Benchmark for Urban Deliveries.

| Instance        | HGS-CVRP (2020)   | HGS-CVRP (2020)   | HGS-CVRP (2020)   | HGS-CVRP (2020)   | ICAHGS    | ICAHGS   | ICAHGS   | ICAHGS   | BKS    |
|-----------------|-------------------|-------------------|-------------------|-------------------|-----------|----------|----------|----------|--------|
|                 | Avg               | Gap Avg.          | Best              | Gap Best          | Avg       | Gap Avg. | Best     | Gap Best |        |
| Loggi-n401-k23  | 337231.80         | 0.08              | 337130            | 0.05              | 337153.20 | 0.06     | 337006   | 0.02     | 336946 |
| Loggi-n501-k24  | 177506.20         | 0.02              | 177495            | 0.02              | 177505.00 | 0.02     | 177469   | 0.00     | 177466 |
| Loggi-n601-k19  | 113309.40         | 0.14              | 113257            | 0.09              | 113270.20 | 0.10     | 113220   | 0.06     | 113155 |
| Loggi-n601-k42  | 347092.80         | 0.01              | 347059            | 0.00              | 347081.20 | 0.01     | 347059   | 0.00     | 347059 |
| Loggi-n901-k42  | 246614.80         | 0.08              | 246485            | 0.03              | 246661.80 | 0.10     | 246523   | 0.04     | 246418 |
| Loggi-n1001-k31 | 286069.20         | 0.60              | 285743            | 0.49              | 285889.60 | 0.54     | 285649   | 0.45     | 284356 |

## References

- [1] G.B. Dantzig, J.H. Ramser, The truck dispatching problem, Manage. Sci. 6 (1) (1959) 80-91, http://dx.doi.org/10.1287/mnsc.6.1.80.
- [2] P. Toth, D. Vigo, Vehicle Routing: Problems, Methods, and Applications, SIAM, 2014.
- [3] T. Vidal, Hybrid genetic search for the CVRP: Open-source implementation and SWAP* neighborhood, Comput. Oper. Res. 140 (2022) 105643, http: //dx.doi.org/10.1016/j.cor.2021.105643.
- [4] E.-G. Talbi, Metaheuristics: From Design to Implementation. Vol. 74, John Wiley &amp; Sons, 2009.
- [5] X.-S. Yang, Chapter 4 Random walks and optimization, in: X.-S. Yang (Ed.), Nature-Inspired Optimization Algorithms (Second Edition), Academic Press, 2021, pp. 63-81.
- [6] E. Uchoa, D. Pecin, A. Pessoa, M. Poggi, T. Vidal, A. Subramanian, New benchmark instances for the capacitated vehicle routing problem, European J. Oper. Res. 257 (3) (2017) 845-858, http://dx.doi.org/10.1016/j.ejor. 2016.08.012.
- [7] N. Christofides, The vehicle routing problem, Comb. Optim. (1979) 315-318.
- [8] B.L. Golden, E.A. Wasil, J.P. Kelly, I.M. Chao, in: T.G. Crainic, G. Laporte (Eds.), Metaheuristics in Vehicle Routing, Fleet Management and Logistics, Kluwer, Boston, 1998.
- [9] Loggi Benchmark for Urban Deliveries: https://github.com/loggi/loggibud.
- [10] N. Christofides, S. Eilon, An algorithm for the vehicle-dispatching problem, J. Oper. Res. Soc. 20 (3) (1969) 309-318, http://dx.doi.org/10.1057/jors. 1969.75.
- [11] J. Lysgaard, A.N. Letchford, R.W. Eglese, A new branch-and-cut algorithm for the capacitated vehicle routing problem, Math. Program. 100 (2) (2004) 423-445, http://dx.doi.org/10.1007/s10107-003-0481-8.
- [12] G. Clarke, J.W. Wright, Scheduling of vehicles from a central depot to a number of delivery points, Oper. Res. 12 (4) (1964) 568-581, http: //dx.doi.org/10.1287/opre.12.4.568.
- [13] B.E. Gillett, L.R. Miller, A heuristic algorithm for the vehicle-dispatch problem, Oper. Res. 22 (2) (1974) 340-349, http://dx.doi.org/10.1287/opre. 22.2.340.
- [14] C. Prins, S. Bouchenoua, A memetic algorithm solving the VRP, the CARP and general routing problems with nodes, edges and arcs, in: Recent Advances in Memetic Algorithms, 2005, pp. 65-85, http://dx.doi.org/10. 1007/3-540-32363-5\_4.

- [15] A. Subramanian, E. Uchoa, L.S. Ochi, A hybrid algorithm for a class of vehicle routing problems, Comput. Oper. Res. 40 (10) (2013) 2519-2531, http://dx.doi.org/10.1016/j.cor.2013.01.013.
- [16] I. Sbai, S. Krichen, O. Limam, Two meta-heuristics for solving the capacitated vehicle routing problem: the case of the Tunisian Post Office, Oper. Res. 22 (1) (2022) 507-549, http://dx.doi.org/10.1007/s12351-019-005438.
- [17] S.-W. Lin, Z.-J. Lee, K.-C. Ying, C.-Y. Lee, Applying hybrid meta-heuristics for capacitated vehicle routing problem, Expert Syst. Appl. 36 (2, Part 1) (2009) 1505-1512, http://dx.doi.org/10.1016/j.eswa.2007.11.060.
- [18] Y. Kao, M. Chen, Solving the CVRP problem using a hybrid PSO approach, in: Computational Intelligence: Revised and Selected Papers of the International Joint Conference, IJCCI 2011, Paris, France, October 24-26, 2011, Springer Berlin Heidelberg, Berlin, Heidelberg, 2013, http://dx.doi.org/10. 1007/978-3-642-35638-4\_5.
- [19] S. Akpinar, Hybrid large neighbourhood search algorithm for capacitated vehicle routing problem, Expert Syst. Appl. 61 (2016) 28-38, http://dx.doi. org/10.1016/j.eswa.2016.05.023.
- [20] C. Prins, A simple and effective evolutionary algorithm for the vehicle routing problem, Comput. Oper. Res. 31 (12) (2004) 1985-2002, http: //dx.doi.org/10.1016/S0305-0548(03)00158-8.
- [21] O. Gokalp, A. Ugur, A multi-start ILS-RVND algorithm with adaptive solution acceptance for the CVRP, Soft Comput. 24 (4) (2020) 2941-2953, http://dx.doi.org/10.1007/s00500-019-04072-6.
- [22] T. Vidal, T.G. Crainic, M. Gendreau, N. Lahrichi, W. Rei, A hybrid genetic algorithm for multidepot and periodic vehicle routing problems, Oper. Res. 60 (3) (2012) 611-624, http://dx.doi.org/10.1287/opre.1120.1048.
- [23] F. Arnold, K. Sörensen, What makes a VRP solution good? The generation of problem-specific knowledge for heuristics, Comput. Oper. Res. 106 (2019) 280-288, http://dx.doi.org/10.1016/j.cor.2018.02.007.
- [24] J. Christiaens, G. Vanden Berghe, Slack induction by string removals for vehicle routing problems, Transp. Sci. 54 (2) (2020) 417-433, http://dx. doi.org/10.1287/trsc.2019.0914.
- [25] L. Mingyang, W. Zheng, L. Juntao, A deep reinforcement learning algorithm for large-scale vehicle routing problems, in: Proc. SPIE, 2022, http://dx.doi. org/10.1117/12.2640015.
- [26] E. Atashpaz-Gargari, C. Lucas, Imperialist competitive algorithm: An algorithm for optimization inspired by imperialistic competition, in: 2007 IEEE Congress on Evolutionary Computation, 2007, http://dx.doi.org/10.1109/ CEC.2007.4425083.
- [27] R. Enayatifar, A.H. Abdullah, M. Lee, A weighted discrete imperialist competitive algorithm (WDICA) combined with chaotic map for image encryption, Opt. Lasers Eng. 51 (9) (2013) 1066-1077, http://dx.doi.org/ 10.1016/j.optlaseng.2013.03.010.
- [28] H.J. Sadaei, R. Enayatifar, M.H. Lee, M. Mahmud, A hybrid model based on differential fuzzy logic relationships and imperialist competitive algorithm for stock market forecasting, Appl. Soft Comput. 40 (2016) 132-149, http: //dx.doi.org/10.1016/j.asoc.2015.11.026.
- [29] A. Ayough, M. Zandieh, H. Farsijani, GA and ICA approaches to job rotation scheduling problem: considering employee's boredom, Int. J. Adv. Manuf. Technol. 60 (5) (2012) 651-666, http://dx.doi.org/10.1007/s00170-0113641-7.
- [30] M. Yousefikhoshbakht, M. Sedighpour, New imperialist competitive algorithm to solve the travelling salesman problem, Int. J. Comput. Math. 90 (7) (2013) 1495-1505, http://dx.doi.org/10.1080/00207160.2012.758362.
- [31] S. Hosseini, A. Al Khaled, A survey on the imperialist competitive algorithm metaheuristic: Implementation in engineering domain and directions for future research, Appl. Soft Comput. 24 (2014) 1078-1094, http://dx.doi. org/10.1016/j.asoc.2014.08.024.
- [32] I.M. Oliver, D. Smith, J.R.C. Holland, Study of Permutation Crossover Operators on the Traveling Salesman Problem, L. Erlhaum Associates, Hillsdale, NJ, 1987.
- [33] MATLAB implementation of ICA: https://www.mathworks.com/ matlabcentral/fileexchange/22046-imperialist-competitive-algorithm-ica.
- [34] CPU Benchmark: https://www.cpubenchmark.net/compare/Intel-i5-4590Tvs-Intel-Xeon-Gold-6148.
- [35] CVRPLIB website: http://vrp.atd-lab.inf.puc-rio.br/index.php/en/.
- [36] DIMACS: http://dimacs.rutgers.edu/programs/challenge/vrp/cvrp/.
- [37] Google OR-Tools: https://developers.google.com/optimization/routing.
- [38] Vidal's Github repository: https://github.com/vidalt/HGS-CVRP.
- [39] Student, The probable error of a mean, Biometrika (1908) 1-25, http: //dx.doi.org/10.2307/2331554.
- [40] F. Wilcoxon, Probability tables for individual comparisons by ranking methods, Biometrics 3 (3) (1947) 119-122, http://dx.doi.org/10.2307/ 3001946.
- [41] K. Puljić, R. Manger, Comparison of eight evolutionary crossover operators for the vehicle routing problem, Math. Commun. 18 (2) (2013) 359-375.
- [42] E. Wibisono, I. Martin, D.N. Prayogo, Comparison of crossover operators in genetic algorithm for vehicle routing problems, 2021.