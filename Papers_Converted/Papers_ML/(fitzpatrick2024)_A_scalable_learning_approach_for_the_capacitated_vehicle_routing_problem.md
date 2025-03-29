![Image](image_000000_aeba0662fe9bddfc3b818c92196ddd0658e33af1bca143bee5482a6ee902225e.png)

Contents lists available at ScienceDirect

## Computers and Operations Research

Analytics and Machine Learning in Vehicle Routing Research
Ruibin Bai a and Xinan Chen a and Zhi-Long Chen b and Tianxiang Cui a and Shuhui Gong c and Wentao He a and Xiaoping Jiang d and Huan Jin a and Jiahuan Jin a and Graham Kendall e,f and Jiawei Li a and Zheng Lu a and Jianfeng Ren a and Paul Weng g,h and Ning Xue i and Huayan Zhang a

a School of Computer Science, University of Nottingham Ningbo China, Ningbo, China.
b Robert H. Smith School of Business, University of Maryland, MD 20742, USA.
c China University of Geosciences, Beijing, China.
d National University of Defence Technology, Hefei, China
e School of Computer Science, University of Nottingham, UK.
f School of Computer Science, University of Nottingham Malaysia, Malaysia
g UM-SJTU Joint Institute, Shanghai Jiao Tong University, Shanghai, China.
h Department of Automation, Shanghai Jiao Tong University, Shanghai, China.
i Faculty of Medicine and Health Sciences, University of Nottingham, UK.
ARTICLE HISTORY
Compiled February 22, 2021

ABSTRACT
The Vehicle Routing Problem (VRP) is one of the most intensively studied combinatorial optimisation problems for which numerous models and algorithms have been proposed. To tackle the complexities, uncertainties and dynamics involved in real-world VRP applications, Machine Learning (ML) methods have been used in combination with analytical approaches to enhance problem formulations and algorithmic performance across different problem solving scenarios. However, the relevant papers are scattered in several traditional research fields with very different, sometimes confusing, terminologies. This paper presents a first, comprehensive review of hybrid methods that combine analytical techniques with ML tools in addressing VRP problems. Specifically, we review the emerging research streams on ML-assisted VRP modelling and ML-assisted VRP optimisation. We conclude that ML can be beneficial in enhancing VRP modelling, and improving the performance of algorithms for both online and offline VRP optimisations. Finally, challenges and future opportunities of VRP research are discussed.

KEYWORDS
vehicle routing; machine learning; data driven methods; uncertainties

1. Background and Motivation
The Vehicle Routing Problem (VRP) is one of the most studied problems in the field of operations research. A search using keyword 'vehicle routing' on Clarivate's Web of Science returns more than 8,000 papers, including 131 review papers. One reason for this significant research attention is due to the booming e-commerce industry that leads to exponential growth in transportation and logistics. With the advances in computing

power and progresses in modelling and solution methodologies, it is now possible to solve VRPs of much larger sizes in less time than we could in the past. There have been a number of survey papers related to VRP. Vidal, Laporte, and Matl (2020) provided a good overview of different VRP variants, including the emerging variants characterised by different objectives and performance metrics. Braysy and Gendreau (2005a,b) conducted comprehensive reviews on the heuristic methods for different VRPs. Most of the papers they reviewed focus on deterministic VRPs, in which the problem parameters are assumed to be deterministic and known prior to the problem solving. Gendreau, Laporte, and Seguin (1996) provided a review on stochastic vehicle routing in which some of the problem parameters are assumed stochastic, while Pillac et al. (2013) surveyed all the dynamic vehicle routing problems in which the problem parameters are revealed dynamically over time. Given their close relevancy between stochastic VRP and dynamic VRP, Ritzinger, Puchinger, and Hartl (2016) provided a combined review for both the dynamic and stochastic vehicle routing problems.

Although a tremendous amount of research has been devoted to VRP problems, it is still very difficult to tackle some practical VRP applications for the following reasons. Firstly, the majority of existing VRP research focuses on the analytical properties of different VRP variants and the corresponding solution methods. This type of research is often dominated by the use of mathematical models to define key objectives and constraints (Vidal, Laporte, and Matl 2020). However, for the convenience of theoretical analyses and problem solving, almost all mathematical models are associated with a number of assumptions, some of which may not be practical for real-life applications. It can also be challenging to estimate relevant problem parameters. For practitioners, it becomes a hurdle in translating the existing models and algorithms into successful real-life applications.

Secondly, numerous VRP models have been developed to mathematically formulate uncertainties pertaining to the VRPs. However, most of them are limited to theoretical or small-scale empirical studies. Implementation of these models and the proposed solution methods in real-world applications is rare and still faces considerable challenges. There is a growing demand for making these models more practically applicable.

To tackle some of these issues, such as unrealistic model assumptions, difficulties in parameter estimation and the practicality of solution algorithms, there is an emerging VRP research direction of using hybrid methods that combine data analytics and machine learning tools with conventional optimisation based techniques. With the assistance of analytics and ML, conventional VRP modelling and solution techniques can be significantly strengthened. This paper aims to provide a comprehensive review of such hybrid methods for VRP applications.

VRP research is traditionally limited to the Operations Research (OR) community. However, with the advances in machine learning methodologies, researchers in this community have recently made attempts to tackle combinatorial optimisation problems (including VRPs) solely using machine learning methods (i.e. without explicitly exploiting the structures of the mathematical models). These methods often, despite some progress, suffer from issues such as the lack of generalisation across different scenarios, inefficiency in data use, and the inability to discover insights and interpret solution structures. There seems to be very little interaction between the two communities. Related papers are scattered in journals of both research communities and cross-community paper citations are fewer than you would expect. This leads to the lagged acknowledgement of progress made across research communities. Furthermore, each community uses its own set of terminologies such that similar ideas are defined with different terms. This often causes considerable confusion for researchers and prac-

titioners. We believe that a thorough review of the VRP research that uses tools from these two research communities would be useful to both communities.

Industrial partners also require a holistic review of existing VRP modelling techniques that explain the practicability of these formulations in terms of quality, availability of data and how parameters can be estimated with required precision in order to satisfy the assumptions and other engineering requirements. In this review, we include a section to existing research studies on how machine learning has been used in achieving more practical VRP modelling and parameter estimation. We will also review existing studies that use ML to help build more efficient algorithms for VRP problems.

This review paper is not intended to give another comprehensive review of all VRPrelated papers. Instead, the focus is on a new, fast-growing VRP research direction that investigates novel ways to integrate existing analytical methods based on mathematical models with advanced ML methodologies.

We recognise the long history of such research efforts in addressing VRP problems, but it has drawn particular research attention in the past three years, partly due to the growing popularity of analytics and ML research in the OR community, and partly due to a shift of focus in the ML community from single node intelligence to complex system intelligence. We believe that, to achieve major breakthroughs to realise full system intelligence, researchers from both the OR community and the ML community need to collaborate at a much deeper level to tackle the significant challenges that VRP presents. This review paper aims to bridge the gap and promote more interactions and collaborations between the two communities. We expect that our review will inspire more researchers to tackle complex VRP problems and make a positive impact on our daily life.

Figure 1 illustrates the overall classification of related research papers that hybridise ML with analytical approaches in the VRP. We broadly classify three major types of integration efforts. They are ML-assisted VRP modelling, ML-assisted offline and ML-assisted online optimisations, respectively. Most machine learning methodologies require historical data of some problem parameters. Some of the methods will also require meta-data generated by the optimisation methodologies.

The remainder of this review is organised as follows: In Section 2, we provide an introduction to VRP problems, its main variants and the main algorithms that have been utilized. Section 3 reviews modelling methodologies for VRPs with uncertainties, including stochastic programming, robust optimisation, chance constrained programming, and data analytics and forecast. In Section 4, ML-assisted VRP algorithms are reviewed according to how the machine learning is utilised in the solution methods, including decomposition based methods, adaptive neighbourhood search and machine learning trainable constructive methodologiess. Section 5 provides some concluding remarks and discusses the challenges, prospects and opportunities for the future research.

2. Introduction
For the benefit of those new to VRP, in this section, we give a general introduction of VRP problem, its main variants, and commonly used algorithms.

Figure 1. The proposed classifications of machine learning assisted VRP research.

Image

2.1. Basic Vehicle Routing Problem
The basic VRP, proposed by Dantzig and Ramser (1959), can be defined as an optimisation problem comprising of a set of distributed customers, each with a freight demand, and a fleet of vehicles starting from the central depot. The objective is to find minimal travel cost (e.g. distance or time), such that each customer is visited and served by a vehicle exactly once. The VRP is related to one of the most extensively studied combinatorial optimisation problems, the Travelling Salesman Problem (TSP), which was first considered by Menger (1932). While TSP aims to find a circular shortest path to traverse all customers without consideration of several practical constraints related to capacity and time, the basic VRP problem also takes into account the capacity constraints related to vehicles and hence requires to find multiple vehicle routes with the minimum total cost. The basic VRP problem is also called the Capacitated VRP (CVRP). For a given set of customers V and vehicle depot node 0, a commonly used mathematical model for CVRP is the following vehicle flow formulation:

  
 

subject to

 

 

 

 

glyph[negationslash]

  
 

where x ij is a binary decision variable that indicates whether the arc ( i, j ) is part of the solution and c ij is the cost of using arc ( i, j ) . K is the number of vehicles being used and r S ( ) is the minimum number of vehicles required to serve customer set S . Constraints (2a,2b) make sure that each customer is visited exactly once and constraints (2c,2d) ensure the satisfaction of the number of vehicle routes. Finally constraint (2e) makes sure that the demands from all customers are fully satisfied. VRP is a classical NP-hard problem, thus most solution algorithms are heuristic-based.

2.2. Main VRP Variants
Due to the complexities in real-world VRP problems, the basic VRP problem has been extended into a number of variants. The review work by Braekers, Ramaekers, and Van Nieuwenhuyse (2016) refers to a taxonomy from Eksioglu, Vural, and Reisman (2009a), which provides a detailed categorisation of various VRP problems and their models. They classified VRPs with different constraints and objectives as 'scenario characteristics', and VRP for different real-world applications as 'problem physical characteristics'. They also defined an 'information characteristics' category and a 'data characteristics' dimension, based on the nature of the problem data.

The focus of this paper is mainly on the potential benefits of using data analytics and machine learning for better VRP formulations and problem solving. Specifically, for some conventional VRP variants, machine learning models can potentially be applied as probabilistic modelling technique for customer arrivals, demand quantities, waiting times, etc. For example, when vehicles are constrained to deliver services to customers within certain time intervals, it is known as the Vehicle Routing Problem with Time Windows (VRPTW). If a number of goods are transported from certain pickup locations to other delivery locations, it is known as the Pickup and Delivery Problem (PDP). If part of the problem components remains uncertain and follows a probability distribution, it is defined as Stochastic Vehicle Routing Problem (SVRP). For different VRP modelling, machine learning methodologies can be implemented in historical data analysis to get the prior knowledge. We adopt the taxonomy proposed in Eksioglu, Vural, and Reisman (2009a) to highlight the VRP variants for which machine learning techniques could be used to enhance the practicality and quality of the problem modelling.

(1) Scenario Characteristics
· Number of stops/customers on a route is partially known or partially probabilistic (Albareda-Sambola, Fernández, and Laporte 2014; Snoeck, Merchán, and Winkenbach 2020).
· Customer service demand quantity is stochastic or unknown. (Zhang et al. 2013; Dinh, Fukasawa, and Luedtke 2018; Ghosal and Wiesemann 2020; Markovic, Cavar, and Caric 2005).
· Request times of new customers are stochastic or unknown.
· Onsite service/waiting times are stochastic or unknown. (Li, Tian, and Leung 2010; Zhang, Lam, and Chen 2013).
(2) Problem Physical Characteristics
· Time window restrictions (on customers, roads, and at facilities) are stochastic or unknown (Chuah and Yingling 2005).
· Travel time is stochastic or unknown. (Musolino et al. 2013; Han, Lee, and Park 2014; Li, Tian, and Leung 2010; Taş et al. 2013, 2014).
(3) Information Characteristics
· Evolution of information is partially dynamic. (Sabar et al. 2014).
· Quality of information is stochastic or unknown. (Balaji et al. 2019).
· Availability of information is local to only a subset of participants concerned in the problem.
(4) Data Characteristics
· Data used is from the real world which can be noisy and messy. (Markovic, Cavar, and Caric 2005; Bent and Van Hentenryck 2005; Li et al. 2018; Žunić, Ðonko, and Buza 2020)
Another VRP variant is the automated guided vehicle (AGV) routing, which is essentially a driver-less transport system used in logistics warehouses and marine container terminals (Vis 2006). Because of the availability of advanced communication and control mechanisms in real-time, AGV routing problems are often modelled as a dynamic problem (Qiu et al. 2002). As a result, data analytical methods and machine learning play a major role in tackling these type of problems as an improved version over simple heuristic based methods (e.g. (Grunow, Günther, and Lehmann 2006; Wang et al. 2015; Zhang et al. 2019)). Readers can refer to detailed surveys (Qiu et al. 2002; Vis 2006; Hasan, Abidin, and MFMS 2019) for better understanding of AGV routing.

2.3. Types of VRP algorithms
Considering that the VRP and its variants belong to the NP-hard class of problems, exact algorithms are only applicable under certain circumstances and for small-scale problems. Exact methods treat VRP as integer or mixed-integer programs, and try to find a (near-)optimal solution. Since real-life VRPs are usually of large sizes, heuristicbased methods are considered more suitable. Elshaer and Awad (2020) states that more than 70% of solution methods in the literature are based on metaheuristics, which have various 'meta' strategies capable of escaping from poor local optima but cannot guarantee optimality (Boussaïd, Lepagnot, and Siarry 2013). Representative exact algorithms for VRP include branch-and-price algorithm (Christiansen and Lysgaard 2007) and branch-and-cut algorithms (Augerat et al. 1995; Ralphs et al. 2003; Baldacci, Hadjiconstantinou, and Mingozzi 2004). Metaheuristics can be divided into two categories: single-point based heuristics, and population-based heuristics. The former includes classical heuristic methods, such as simulated annealing (SA) (Kirkpatrick, Gelatt, and Vecchi 1983), tabu search (TS) (Glover and Laguna 1997), GRASP (for greedy

randomized adaptive search procedure ) (Feo and Resende 1995), variable neighborhood search (VNS) (Mladenović and Hansen 1997), guided local search (GLS) (Voudouris and Tsang 2003), iterated Local Search (ILS) (Stützle 1999), large neighborhood search (LNS) and adaptive large neighborhood search (ALNS) Pisinger and Ropke (2010). The latter includes two main types, evolutionary algorithms and swarm intelligence, which are mainly inspired by some natural phenomena. The readers may refer to Elshaer and Awad (2020) for a comprehensive review on metaheuristic for VRP. This review focuses on the machine learning assisted VRP algorithms and Section 4 discusses all the relevant papers.

3. ML-Assisted VRP Modelling
This section provides an overview of various VRP modelling methodologies that are supported by data analytics and machine learning. In particular, we focus on the modelling techniques for handling uncertain, incomplete, imprecise or ambiguous data in VRPs, including stochastic programming, robust optimisation, chance constrained programming and data forecast.

3.1. Stochastic Programming for VRP Modelling
Machine learning has long been used to assist the modelling of stochastic VRPs, where the uncertain data is represented by random variables with known probability distributions. A number of studies have modelled stochastic VRPs as a Markov Decision Process (MDP), which can be tackled by Neuro-Dynamic Programming (NDP). NDP is also referred to as reinforcement learning in the artificial intelligence literature (Bertsekas and Tsitsiklis 1996). The value-function approximation method and/or the policy-function approximation method of NDP is tailored in each of those studies. Secomandi (2000) compared two different NDP algorithms on solving VRPs with stochastic demands and showed that the NDP with rollout policy performs better than an approximate policy iteration. Godfrey and Powell (2002) formulated the dynamic fleet management problem as a multistage dynamic program and use gradient-based information to obtain nonlinear approximations of value functions. Zhang et al. (2013) used tabular value approximation and policy approximation to solve VRPs with stochastic demands. Ulmer et al. (2020) presented a route-based Markov decision process modelling framework for dynamic VRP that extends the conventional MDP model for dynamic and stochastic optimisation problems by redefining the conventional action space to operate on route plans. The proposed modelling framework makes it easier to integrate machine learning approaches with the existing route-based analytical methods.

Machine learning has also been used to estimate the probability distributions of uncertain data in stochastic VRPs. Ritzinger and Puchinger (2013) state that hybrid metaheuristics are often used to solve complex and large real-world optimisation problems, combining advantages from machine learning techniques and mathematical optimisation. The authors examined the hybrid metaheuristics for dynamic stochastic VRP, where not all information is available in advance. Bent and Van Hentenryck (2005) proposed a machine learning approach by sampling from historical data to get partial knowledge of stochastic distribution in online scheduling problems. They claim that their approach could help widen the application area of stochastic algorithms. Experiments are conducted on several problems including dynamic VRPTW from pre-

vious work Bent and Van Hentenryck (2004). Defourny (2010) generated and selected scenario trees with random branching structures for multi-stage stochastic programming over large planning horizons. This work combines the perturb-and-combine estimation methods from machine learning with stochastic programming techniques for sequential decision making under uncertainty. The performance has been shown to be competitive. Bai et al. (2014) studied a two-stage model for stochastic transportation network that supports vehicle rerouting in the second stage.

3.2. Robust Optimisation for VRP Modelling
Stochastic programming assumes that the probability distributions of uncertain parameters are known, and aims for the best average performance over all possible future scenarios. By contrast, robust optimisation assumes that the probability distributions of uncertain parameters can be unknown, but the range of the uncertain data is known, and aims for a solution that could work even in the worst-case scenario. In general, robust optimisation is considered as a risk-averse method.

Robust CVRPs require the vehicle routes to be feasible for all customer demands within a pre-specified uncertainty set (e.g., a box, polyhedron, or ellipsoid). Branchand-cut schemes for the exact solution of the robust CVRPs have been proposed by (Sungur, Ordóñez, and Dessouky 2008) and (Gounaris, Wiesemann, and Floudas 2013). Robust CVRPs are amenable to solution schemes that appear to scale better than those for the chance constrained CVRPs (see Section 3.3). However, the solutions obtained from the robust CVRPs can be overly conservative since all demand scenarios within the uncertainty set are treated as equally likely, and the routes are selected solely in view of the worst demand scenario from the uncertainty set(Ghosal and Wiesemann (2020)).

For a comprehensive review of robust optimisation, we refer interested readers to (Bertsimas, Brown, and Caramanis 2010). More recent contributions are some simulation based approaches, which transform the complex stochastic VRPs into a set of deterministic CVRPs, for which fast and extensively tested meta-heuristics exist (Bernardo, Du, and Pannek 2020). To our knowledge, very little, if any, research has been conducted so far to apply machine learning techniques to robust VRPs, which offers many research opportunities.

3.3. Chance-constrained Programming for VRP Modelling
Chance-constrained programs are different from the stochastic programming and robust optimisation. It assumes that constraints are satisfied to some degree, measured by probabilities, rather than fully met. In VRPs, the chance-constrained CVRPs consider vehicle routes that satisfy the customer demands along each route with a pre-specified probability.

Dinh, Fukasawa, and Luedtke (2018) modelled the stochastic demand of customers as a joint normal distribution or a given discrete distribution. In particular, such modelling allows correlation between customer demands rather than independent demands. A branch-and-cut-and-price (BCP) algorithm is used to tackle the problem. Route relaxation is proposed in order to price through dynamic programming. The experiments demonstrated this approach's ability to solve the distributionally robust chance constrained vehicle routing problems (CCVRPs).

Ghosal and Wiesemann (2020) focused on the distributionally robust chanceconstrained CVRP, which assumes that the probability distribution of the customer demand is partially known. The customer demands are modelled as the distribution based on ambiguity sets. Several first-order and second-order moment ambiguity sets are investigated and branch-and-cut schemes are used to solve it. The experiments demonstrated the good scalability of distributionally robust CVRP.

Li, Tian, and Leung (2010) studied a version of VRP where the service and travel times are stochastic and time window constraints are involved. Such problem is then formulated as a chance constrained programming model and a stochastic programming model with recourse. The author assigned a confidence level for both time window and driving duration of each route. Tabu search is utilised as the algorithmic solver. Similarly, Gutierrez et al. (2018) also focused on VRP with stochastic service and travel times and time windows. The arrival times are estimated by a log-normal approximation. A multi-population memetic algorithm (MPMA) is then proposed as the algorithm to address the problem. Wu et al. (2020a) studied VRP for application of wet waste collections. In this work, chance constrained programming is used to model the uncertainty of waste generation rate and make it deterministic. A hybrid algorithm consisting of Particle Swarm Optimisation (PSO) and Simulated Annealing (SA) are used for the optimisation.

Similar to the robust optimisation methods, very little research has been conducted to explicitly combine machine learning with chance-constrained programming in solving VRP problems. However, one obvious way to hybridise them is to use ML to estimate probabilities or the parameters of the distributions. Another hybridisation opportunity can be using ML to speed up the various branch and pricing methods developed for chance-constrained programming.

3.4. Data Analytics and Forecast in VRP Modelling
In this subsection, we discuss a relatively loose connection between machine learning and the VRP. Research works in this area are not likely to use machine learning to address the related problem, but use it as a tool to reflect and analyse data that is related to problem solutions. A rough classification by Talbi (2016) divides such combination into the low-level integration, which is highly related to hyperparameter tuning; and the high-level integration, where machine learning is applied to help the modelling and provide certain contexts for metaheuristics. In this section, we mainly focus on machine learning assisted predictive models for handling uncertainties in VRP, and machine learning based model fine-tuning methods.

3.4.1. Predictive Models for Uncertainties in VRP
So far, a great number of methods have been proposed to solve classic VRP and many of them can generate high-quality solutions. However, most of these methods fail to work in realistic scenarios because of the over-simplification of the real-world constraints and uncertainties. For example, the elements such as customer occurrences and demands, and travel time may not be deterministic. Therefore, more sophisticated approaches are required to model and solve these problems.

Ce and Hui (2010) made a basic classification by analysing customer's demands and introduced corresponding data mining techniques for each case. Markovic, Cavar, and Caric (2005) used a neural network to predict customers' stochastic demand based on historical data, while Snoeck, Merchán, and Winkenbach (2020) applied probabilistic

graphical model to predict constrained customers for routing problems. Musolino et al. (2013) developed a simulation-based method to forecast the travel time for an emergency vehicles routing problem. Li et al. (2019) investigated various ensemble based machine learning methods to predict the travel time in vehicle routing applications.

Li et al. (2018) applied a data-driven search algorithm to improve traditional metaheuristics methods for large-scale manufacturing logistic problems in real-world industry practice. In the VRP part, they learned a multinomial transition matrix by updating from historical expert data and generated valid solutions by sampling from the matrix.

Calvet et al. (2016a) considered a realistic VRP setting where customers have different demands to different assigned depots. The matching between heterogeneous depots and customers with stochastic demand could significantly affect the profitability of the company. Thus, several regression models which estimate the customer demands based on their assigned depot are used to support the market segmentation strategy.

In dynamic VRP, a customer's availability indicates the available service time window of each customer, which is one of the important aspects that cause the dynamism in Dynamic VRP (DVRP). Kucharska (2019) focused on this dynamism and used algebraic-logical meta-model (ALMM) to solve dynamic VRP with predicted customer availability.

Another commonly used modelling approach addressing uncertainties in VRP is rolling horizon planning. Problems are solved iteratively over a planning horizon that shifts forward a constant or variable time span at each step. At each iteration, some of the uncertainties of the problem are estimated with historical data. Cordeau et al. (2015) used this mechanism for a dynamic multi-period auto-carrier transportation problem which considers balancing vehicle usage and demand in dynamic settings. Wen et al. (2010) studied dynamic multi-period vehicle routing problem with multiobjective of minimising customer waiting time and total travel costs as well as balancing the daily workload. Historical data are used to estimate the average daily workload over the rolling horizon.

Machine learning has also been applied to a wide variety of problems in transportation and logistics. Zhang et al. (2011) reviewed various data-driven traffic management systems, in which computer vision and other learning methods for environment sense and prediction are adopted. Zantalis et al. (2019) presented a review on machine learning and IoT in smart transportation, while Anuar et al. (2019) gave a review on machine learning and VRP methods in humanitarian logistics.

3.4.2. Learning to Configure Algorithms
There are a great number of hyper-parameters in VRP algorithms such as acceptance criteria of simulated annealing algorithm or population size of family of nature-inspired algorithms. Usually, these hyper-parameters need to be carefully adjusted manually. Good default parameters could significantly affect the performance of algorithms (Bengio, Lodi, and Prouvost 2020). Finding such suitable parameters is generally called the parameter setting problem (PSP). Machine learning methodologies could also be used to tackle the PSP problem as data-driven approaches. The general idea is to train regression models that map the specific problem instance features to the algorithm parameters where the training data are sets of pre-adjusted parameters and the corresponding performance.

Calvet et al. (2016b) proposed a novel statistical learning-based methodology for solving the PSP and used it to fine-tune the existing algorithm for multi-depot vehicle

routing problem. Kadaba, Nygard, and Juell (1991) proposed a hybrid framework including machine learning and knowledge-based systems to solve routing and scheduling applications. Neural networks are used for model selection and GA hyper-parameters tuning. Cooray and Rupasinghe (2017) used simple clustering techniques to improve the parameter selection for GA. Žunić, Ðonko, and Buza (2020) built a framework where the predictive models, such as support vector machine, were used to adaptively control the hyper-parameters for each instance based on the historical data. The performance of the framework was demonstrated by testing on heterogeneous fleet vehicle routing problem with time windows (HVRPTW) along with a set of realistic logistics constrains.

Clustering methodologies are also used in VRP modelling. Hu, Huang, and Zeng (2007) proposed a novel framework for solving CVRPTW. In the first stage of this framework, customers are clustered into different groups based on their features. The experiments demonstrated that such clustering could efficiently support the subsequent stages to generate feasible solutions. Costa, Mei, and Zhang (2020) used a Clusterbased Hyper-Heuristic (CbHH) that adaptively cluster the customers to determine the neighborhood searching space at each step. The genetic algorithm is used to evolve the sequence of low-level perturbative operator to improve the initial solution. The experiment showed the effectiveness of the clustering technique and GA-based heuristic search. Similarly, Göçmen and Erol (2019) used clustering method and upstream 3Dbin packing to provide context for VRP problem.

4. ML-Assisted VRP Algorithms
Machine learning can help address combinatorial optimisation problems in various ways (see Bengio, Lodi, and Prouvost (2020) for a more general discussion or Smith (1999) for a survey of older work). It can assist traditional solvers that utilise heuristics to make decisions. Those heuristics are created by experts, are generally hard to design, and may not transfer well from one problem (instance) to another. Machine learning can help learn such heuristics automatically. For instance, He, Daume III, and Eisner (2014) and Khalil et al. (2016) independently proposed learning searching in a branch-and-bound tree for mixed-integer linear programs (MILP). Li, Chen, and Koltun (2018) trained a graph convolutional network to guide a tree search, notably for the Boolean satisfiability (SAT) problem. Such approaches are useful as they can benefit those aiming to solve problems that can be formulated as MILP or SAT. See Lodi and Zarpellon (2017) for a recent survey on machine learning applied to branch and bound. In this section, we review various strategies that combine machine learning with traditional VRP solution methods.

4.1. ML-Guided VRP Decomposition Strategies
As a classic NP-hard combinatorial optimisation problem, VRP's search space grows exponentially with respect to the problem size, causing significant challenges to solve to optimality as the problem size increases. A common strategy is to decompose the original large scale problem into a number of smaller sub-problems. However, how the problem is decomposed becomes another difficult problem. In this section, we review decomposition based VRP studies that adopt machine learning to guide the decomposition.

In general, in a decomposition approach, the VRP problem is divided into smaller

and simpler sub-problems and a specific solution method is applied to each sub-problem (Archetti and Speranza 2014). We classify the ML-guided VRP decomposition into hierarchical and integrated approaches. Hierarchical approaches are high-level combinations of ML and OR algorithms (e.g. either exact methods or (meta-)heuristics), while the integrated approaches are low-level combinations because one algorithm is embedded within another.

4.1.1. Hierarchical approaches
The ML-guided hierarchical decomposition in VRP can be classified into two types: Route First and Cluster Second (RFCS) and Cluster First and Route Second (CFRS). RFCS decomposes a VRP problem into two sub-problems: 1) Generate a TSP tour from the depot around all the customers and back to the depot; 2) Partition the TSP tour into a set of vehicle routes. Conversely, the CFRS starting from assigning customers to vehicles, then vehicle tours are constructed.

The OR approaches of solving RFCS (e.g. Beasley (1983), Montoya et al. (2014)) and CFRS (e.g. Fisher and Jaikumar (1981), Dondo and Cerdá (2007)) have been applied to many VRP problems.

ML-guided CFRS Erdoğan and Miller-Hooks (2012) solved the Green Vehicle Routing Problem (G-VRP) using Modified Clarke and Wright Savings (MCWS) heuristic and an unsupervised clustering algorithm, which is built on the concepts from the Density-Based Spatial Clustering of Applications with Noise (DBSCAN) algorithm for exploiting the spatial properties of the G-VRP. The VRP is decomposed into two subproblems: 1) Cluster customers using DBSCAN; 2) run MCWS to construct vehicle tours.

Comert et al. (2017) proposed a two-stage solution method for the Vehicle Routing Problem with Time Windows (VRPTW). In the first stage, customers were assigned to vehicles using the best-performing clustering algorithms among K-means, K-medoids, and DBSCAN. In the second stage, a VRPTW was solved using Linear Programming (LP) to construct vehicle tours.

Göçmen and Erol (2019) studied intermodal network problems combining pick-up routing problems with three-dimensional loading constraints, clustered backhauls at the operational level, and train loading at a tactical level. The authors first used the clustering of backhauls, then packed using K-means for a feasible loading pattern with four loading dimensions, and finally utilised capacitated VRP formulation which can be solved to optimally for small problem instances. The K-means clustering ensures the assignments of tasks into several clusters, where the initial cluster number is defined before the solution procedure starts. The task assignment to which of the clusters is not part of the decision variables but is an input to the intermodal network problem.

The scientific literature shows that the ML-guided CFRS follows a similar solution procedure where an unsupervised cluster algorithm is first adopted to cluster customers to vehicles, followed by vehicle tours construction using (meta)-heuristics or LP methods. Readers are referred to He et al. (2009), Nallusamy et al. (2010), Korayem, Khorsid, and Kassem (2015), Reed, Yiannakou, and Evering (2014), Comert et al. (2018), Xu, Pu, and Duan (2018), Geetha, Poonthalir, and Vanathi (2013), Rautela, Sharma, and Bhardwaj (2019), Geetha, Vanathi, and Poonthalir (2012), Yücenur and Demirel (2011), Qi et al. (2011), Luo and Chen (2014), Gao et al. (2016), Miranda-Bront et al. (2017) for similar and more complete references.

ML guided RFCS We found only one study using ML in RFCS. Kubra, Muhammet, and Ozcan (2019) investigated the problem of determining service routes for the

staff of a company. The authors first implemented a GA to create a TSP tour, and then partitioned and compared the tours by considering different scenarios using clustering methods including K-means, K-medoids and K-modes.

4.1.2. Integrated approaches
Integrated approaches of the decomposition have been applied to enhance the performance of LP algorithms. For example, the approximation of strong branching (Alvarez, Louveaux, and Wehenkel 2017), learning techniques introduced in the branchand-bound algorithm (Lodi and Zarpellon 2017). A recent survey of utilising ML to solve combinatorial optimisation problems can be found in Bengio, Lodi, and Prouvost (2020). However, the integrated approaches tailored to the VRP are somewhat limited.

Desaulniers, Lodi, and Morabit (2020) employed a Graph Neural Networks (GNN) to select a subset of the columns generated at each iteration of the column generation process. The input of the GNN is a set of promising columns generated by solving an LP model to minimise the number of columns and ensure a maximal decrease of the objective value. The learned GNN model is to reduce the computational time spent re-optimising the restricted master problem (RMP) at each iteration by selecting the most promising columns. Computational results on two types of VRP indicate that average computational time reductions of 20 to 30% are achievable when solving the RMP.

Kruber, Lübbecke, and Parmentier (2017) applied a K-nearest-neighbour classifier to decide whether or not a Dantzig-Wolfe decomposition should be applied to a given problem, and which decomposition to choose. This method is not limited to VRP though.

Yao et al. (2019) proposed a decomposition framework based on Alternating Direction Method of Multipliers (ADMM) to iteratively improve the primal and dual solution quality simultaneously. ADMM has been utilised in distributed convex optimisation problems arising in statistics and machine learning (Boyd et al. (2010)). The authors first constructed a multi-dimensional commodity flow formulation for the VRP. Then, ADMM was applied to develop a decomposition framework, in which the original model was decomposed into a series of least-cost path problems which can be solved by the dynamic programming.

4.2. ML-Guided Perturbative VRP Algorithms
When VRPs are modelled as offline optimisation problems (i.e. the problem related parameters are known and given prior to problem solving), perturbation based metaheuristics and evolutionary algorithms are among the most popular solution methods. The simplest perturbation search method is the basic local search. It searches a predefined neighbourhood of candidate solutions to find solutions which are superior to the current ones according to the objective function. The neighbourhood of a solution is defined as the set of solutions that can be derived by perturbing the incumbent solution according to certain transition rules. For example, 2-opt neighbourhood function for VRP operates in the neighbourhood of solutions solutions that only differ in the order of two connecting arcs. Since global search is generally impractical for NP-hard problems, local search can provide satisfactory, although not optimal, solutions within a reasonable time when well designed. See Gendreau and Potvin (2010) for more detailed discussions of meta-heuristics and local search algorithms.

Local search starts from an initial solution (either randomly generated or heuris-

tically constructed), and moves to a neighbour solution better than the current one continuously until no improvement can be made or the stopping criteria are met. Local search may be trapped in local optima. To overcome this shortcoming, a number of meta-heuristics approaches are introduced to improve the neighbourhood search strategies, for example simulated annealing, tabu search, guided neighbourhood search, variable neighbourhood search and adaptive large neighbourhood search. The idea of getting away from local optimum is to introduce a perturbation so that the search process can accept inferior solutions and 'jump' out poor local optima by following some guiding strategies.

So far, ML has been used in different parts of perturbation based search approaches to improve its performance: initial solution generation, adaptive selection of neighbourhoods at different search stages, generation of neighbourhood functions and solution evaluations. We review ML guided initial solution generation in Section 4.3. In the following we focus on the intelligent neighbourhood selection, neighbourhood function generation and solution evaluations assisted by machine learning.

4.2.1. Learning to Select Perturbation Heuristics
The concept of using machine learning techniques to guide the neighbourhood search is well documented. Initially, various adaptive mechanisms in the principle of basic reinforcement learning are used in the form of perturbative hyper-heuristics (Burke et al. 2019) and adaptive large neighbourhood search (ALNS) (Ropke and Pisinger 2006). The main idea of perturbative hyper-heuristic is to intelligently select a set of perturbative low-level heuristics to adapt to different problem solving scenarios. As such, the learning based components are crucial parts of this type of hyper-heuristic methods. The literature of applying learning assisted perturbative hyper-heuristics for various combinatorial optimisation problems is rich. Here, we shall focus mainly on the papers that are developed for vehicle routing.

Bai et al. (2007) addressed a VRPTW problem with a hyper-heuristic method that uses the concept of reinforcement learning to guide the selection of low-level heuristics and simulated annealing as the perturbation acceptance criteria. The study investigated how the memory length of the adaptive learning mechanism affects the performance of the algorithm. Sabar et al. (2015) proposed a new hyper-heuristic method for two VRP problems by using an improved reward scheme (dynamic multiarmed banditextreme value-based reward) in the learning mechanism, coupled with gene expression programming for acceptance criteria generation. Soria-Alcaraz et al. (2017) extended this learning reward scheme with additional information from non-parametric statistics and fitness landscape measurements.

Garrido and Cristina Riff (2010) investigated evolutionary-based hyper-heuristic approaches for a dynamic vehicle routing problem. The results showed that evolutionary based learning mechanisms improve the algorithm performance by adapting to different dynamic environments. Asta and Ozcan (2014) proposed to use an apprenticeship learning in the hyper-heuristics for vehicle routing. The trained algorithm is able to produce high quality solutions for test instances which are not seen during the hyperheuristic training stage.

The learning assisted perturbative hyper-heuristics have also been used in other VRP variants, including railway maintenance service routing (Pour, Drake, and Burke 2018), multi-depot m-TSP problem (Pandiri and Singh 2018), multi-objective routing planning (Yao, Peng, and Xiao 2018), mixed-shift full truckload routing (Chen et al. 2018, 2020a), energy-aware routing (Leng et al. 2019), urban transport routing (Ahmed,

Mumford, and Kheiri 2019; Heyken Soares et al. 2020).

One emerging direction for perturbative heuristic hyper-heuristics is the use of deep reinforcement learning (DRL) for heuristics selection. A DRL combines deep learning with aforementioned reinforcement learning. Tyasnurita, Ozcan, and John (2017) used a time delay neural network (TDNN) as a classifier to select the low-level heuristics to solve open vehicle routing problem. Parameters of TNDD were trained through the experience replay from an 'expert' hyper-heuristic algorithm called MCF-AM (Modified Choice Function - All Moves) from Drake, Özcan, and Burke (2012). Chen and Tian (2019) proposed a general deep reinforcement learning based hyper-heuristic framework for combinatorial optimisation problems called local rewriting framework (Neuwriter) which generates a solution iteratively. In this work, a region picker and a rule picker were defined and trained separately. At each rewrite step, two pickers selected a subarea of current solution and its rewrite rule one behind the other. Then the modified sub-area will be put back to generate a new solution. The model repeated this process until convergence. The online job scheduling problem, expression simplification problem, and capacitated vehicle routing problem were used to test this method. A bi-directional LSTM layer was used to embed the input nodes in CVRP problem, and a similar pointer network mechanism was used to select a node through a probability distribution. Wu et al. (2020b) extended the Neuwriter framework by integrating the region picker and rule picker policy networks into one. Specifically, the compatibility computation was adopted in the model to produce a probability matrix of node pairs whose element specified the two corresponding nodes. In order to capture the node position information, sinusoidal positional encoding was introduced to the embedding layer. The results showed that this method outperformed Neuwriter.

Lu, Zhang, and Yang (2020) proposed an iterative improvement method called 'Learn to Improve (L2I)' based on that. Notably, this method outperformed LKH3 (Helsgaun 2017) which was the state-of-the-art method of VRP on both speed and solution quality. The model started with a random initial solution and used two classes of predefined low-level heuristics, namely, improvement operators and perturbation operators which were used to local search the solution and destroy part of the solution respectively. A double layer controller guided by reinforcement learning was used to select the corresponding heuristics. When generating the action, the model would also consider the history actions and their effects. A policy ensemble mechanism was used to improve the generalisation.

4.2.2. Learning to Adapt Neighbourhood Choices
Most real-life combinatorial optimisation problems involve complex objectives and constraints which often lead to very different solution space landscapes. For example, some solution spaces are very rugged with a lot of local optima while some other solution spaces contain plateaus that are insensitive to neighbourhood moves. These challenges lead to considerable research efforts in embedding learning mechanisms for more efficient neighbourhood search through adaptive neighbourhood selection. Since the early work on adaptive large neighbourhood search (ALNS) from (Ropke and Pisinger 2006), a number of follow-up research efforts have been made in either directly applying or extending the methods to solve different VRP variants. Papers that used similar algorithms for different VRP variants include Laporte, Musmanno, and Vocaturo (2010); Ribeiro and Laporte (2012); Kovacs et al. (2012); Hemmelmayr, Cordeau, and Crainic (2012); Demir, Bektas, and Laporte (2012); Masson, Lehuede, and Peton (2013); Azi, Gendreau, and Potvin (2014); Aksen et al. (2014); Belo-Filho, Amorim, and Almada-

Lobo (2015).

There have been some research to further enhance ALNS method by hybridising with other methods. Qu and Bard (2012) combined ALNS with a GRASP approach in solving a pickup and delivery problem with transshipment. Parragh and Cordeau (2017) combined a branch and price method with the ALNS method for a truck and trailer routing problem considering the time window constraints. Zulj, Kramer, and Schneider (2018) integrated tabu search in a ALNS framework for solving an orderbatching problem. Lahyani, Gouguenheim, and Coelho (2019) used a hybrid ALNS method to successfully address a multi-depot open vehicle routing problem. Ha et al. (2020) combined constraint programming with ALNS to solve VRP with synchronisation constraints.

Another learning-based ALNS algorithm proposed by Hottung and Tierney (2019) was called neural large neighborhood search (NLNS). The method was specifically adapted to support parallel computing, which is one of the contributions of this method. Such features could support two implementation patterns: batch search which solved a set of instances simultaneously and the single instance search which solved only one instance concurrently. The potential of the method was demonstrated through experiments for capacitated vehicle routing problem (CVRP) and the split delivery vehicle routing problem (SDVRP).

4.2.3. Learning to generate perturbation heuristics
A new exciting direction of using machine learning in solving VRP problems is that machine-learning models could be used to generate new heuristics to perturb solutions as apposed to using manually-crafted perturbation heuristics. For example, a neural network model takes an incumbent solution as inputs and outputs the indices of nodes that are modified. In this case, the neural network model itself acts as perturbative heuristic in the traditional optimisation algorithm.

da Costa et al. (2020) focused on improvement (or perturbative) heuristics that could refine a given solution iteratively until reaching a near-optimal solution. In his work, a method that could learn a policy to generate the 2-opt heuristic for TSP was proposed. A similar encoder-decoder framework was used to encode the graph and generate a sequence of action distribution but the mechanism was modified to make it easy to extend to k-opt operations. The difference to the initial pointer network is that at each decoder step, the model outputs an action (two nodes for 2-opt) rather than one specific node to construct the final solution.

Gao et al. (2020) proposed a method for learning the local search heuristics. Similarly, an encoder-decoder was used and trained by actor-critic algorithm. Inspired by the ALNS algorithm for vehicle routing problems, the authors defined the local search heuristic as a destroy operator and a repair operator. The destroy operator removed a subset node of the current solution and the repair operator was used to generate a permutation of the selected elements and insert them back. Motivated by the graph attention network (GAT) mechanism (Veličković et al. 2018) which is an effective method to represent the graph topology by propagating the neighbour node information through the attention mechanism, the authors proposed a modified version called Element-wise GAT with Edge-embedding (EGATE) which not only considered the information of nodes but also the arc between the nodes. The attention mask was generated by softmax the concatenation of embedding of arc and two nodes it connected with. The decoder acted as destroy and repair operation.

Chen et al. (2020b) were also motivated by ALNS algorithm, and proposed a similar method called dynamic particle removal (DPR) using Hierarchical Recurrent Graph Convolutional Network (HRGCN). Similar to the method of Gao et al. (2020), the authors also defined the local search as a destroy and repair operator. The degree (size and the allocation of the sub-nodes) of the destroy operator was dynamically determined. The HRGCN is able to be aware of spatial (graph topology) and temporal (embedding in previous iterations) context information.

4.2.4. Learning to Speedup Solution Evaluations
For many real world scheduling and routing problems, computing evaluation functions is expensive. ML has been used in evaluating solutions to reduce computational complexity. Boyan and Moore (2000) developed a general algorithm (which was called STAGE) to learn an evaluation function that predicted the outcome of a local search. The learned evaluation function was then used to guide the future search trajectories toward better optima on the same problem. Moll et al. (1998) introduced an offline reinforcement learning phase to STAGE and compared using of learned evaluation function with the original evaluation function. The proposed algorithm was applied to the DialA-Ride Problem, a variant of TSP, to show how well learning an instance-independent evaluation function could guide local search for additional instances.

4.3. Learning to Construct VRP Solutions
In this section we discuss some representative works that exploit machine learning in the constructive approach for vehicle routing related problems. Recall that such an approach consists of building iteratively a complete solution from scratch (e.g., in TSP, it sequentially selects unvisited cities until a tour is formed) in contrast to approaches based on iterative perturbative search.

The most common approach is to learn a probability distribution over solutions, which can then guide a tree search (e.g., greedy search, beam search) to generate a full solution. In contrast, another possibility is to learn directly a constructive solver. Indeed, such a solver can be seen as a sequential decision-maker, which can be trained via an evolutionary or a reinforcement learning process.

Using machine learning to solve routing problems has a long history (e.g., Hopfield and Tank (1985); Potvin, Lapalme, and Rousseau (1990)). We refer the reader to other surveys on historical developments (e.g., Smith (1999)). We will focus mostly on more recent applications of machine learning to these routing problems. We discuss first the works that focus on (mostly planar) TSP or variants, and then turn to those on VRP and its variants. We mention work on online VRP and variants at the end.

For TSP, Vinyals, Fortunato, and Jaitly (2015) proposed Pointer Networks, which is a novel neural network architecture with two components. First, an encoder was implemented as a recurrent neural network sequentially reading the positions of the cities. Second, a decoder was realized as a recurrent neural network iteratively outputting a probability distribution over remaining cities, using an attention mechanism (Bahdanau, Cho, and Bengio 2015). The whole model was trained in a supervised fashion. For testing, beam search was used to ensure that only valid tours are output. The technique was applied to TSP instances with up to 50 nodes.

Bello et al. (2017) extended the previous approach to train the network using reinforcement learning with an actor-critic scheme. They used the cost length of a generated tour as an unbiased estimate of the value of a policy. The authors found out that pre-

training on a set of instances in addition to training on the particular instance to be solved yielded the best performance on TSP instances with up to 100 nodes. Some more recent investigation (Joshi, Laurent, and Bresson 2019b) suggests that RL may lead to better generalization capability compared to supervised learning.

Khalil et al. (2017) proposed a general method to solve graph-based combinatorial optimization problems based on graph embedding to encode a partial solution and reinforcement learning to learn a greedy policy. In contrast to the previous approaches, they used a value-based RL method, fitted Q-learning.

Deudon et al. (2018) proposed a graph attention network architecture to improve over Pointer Networks to obtain invariance over input order. Also, the authors introduced the idea of preprocessing the inputs with PCA to obtain rotation invariance. With a critic using a similar architecture, the policy was trained in an actor-critic architecture. They used a mask to remove already visited cities. In their recent hybrid method, the authors proposed further to improve the solutions sampled from the trained policy using the 2-OPT heuristics.

In contrast to other approaches mentioned here, Nowak et al. (2017) proposed a nonautoregressive model based on graph neural networks, i.e., their model does not select cities sequentially. Instead, the model trained in a supervised way outputs an adjacency matrix representing a distribution over tours, from which they extract a full solution via beam search. Although the method is not competitive with other deep learning methods, Joshi, Laurent, and Bresson (2019a) improved this approach by using notably graph convolutional networks. They showed that their approach compares favourably with other previous autoregressive approaches.

Yang et al. (2018) proposed to perform the dynamic programming update of the Bellman-Held-Karp algorithm (Bellman 1962; Held and Karp 1962) for solving TSP in an approximate way using neural networks. Doing so allows their proposition to tackle much larger TSP instances.

Ma et al. (2020) proposed Graph Pointer Network (GPN), an extension of Pointer Network, with a graph embedding of the input. They demonstrated promising generalisability of GPN, which can be trained on small TSP instances (up to 100 cities) and then solve larger instances up to 1000 cities. Besides, they suggest using a hierarchy of GPNs to take into account additional constraints on the TSP problem. They tried their ideas on TSP with time windows and showed that their approach performs well.

Some works considered the multiple TSP (mTSP), where several traveling salesmen need to visit all the cities exactly once in a cooperative manner. This problem can be seen as a relaxed VRP problem. Kaempfer and Wolf (2019) adapted PointNet (Qi et al. 2017), which deals with sets of points, to this mTSP problem. Hu, Yao, and Lee (2020) solved it by first using a cooperative multi-agent deep reinforcement learning for agent-to-city assignment, and then computing the tour of each agent using a classic TSP solver.

While the previous works only focus on TSP, recent works started to consider the harder problems of VRPs and variants. Nazari et al. (2018) proposed a simplified version of Pointer Networks to solve capacitated VRPs, split-delivery VRPs (SDVRP) and stochastic VRPs. In order to make the input invariant to sequence order (e.g., order of customers), they replaced the RNN encoder of Pointer Networks by simple embedding maps. The resulting model can then handle changes in the input (e.g., customer demand after being visited). An actor-critic scheme was used for training and beam search, which tracks the most probable paths, was used to generate the final best solution.

Kool, van Hoof, and Welling (2019) proposed a transformer-based model to solve

routing problems, such as TSP, CVRP, or split delivery VRP. The model is similar to that of Deudon et al. (2018) with a few simplifications and improvements. In particular, in contrast to that work, Kool, van Hoof, and Welling (2019) did not use 2-OPT. Another novelty is that policy gradient with a self-critic baseline (estimated with greedy policy rollouts) was used for training. Peng, Wang, and Zhang (2020) generalized this approach to use a dynamic attention model so that state features can be updated during the construction of a solution.

Sheng, Ma, and Xia (2020) proposed a variation of Pointer Network to solve VRP with Task Priority and Limited Resources. The model was trained in the RL setting. They showed that this approach is comparable to Genetic Algorithm (GA) for mediumsized instances ( ≈ 50 cities), but its performance becomes better than GA for largersized instances ( > 100 cities) while taking much less computational time.

Duan et al. (2020) proposed a technique that combines training with reinforcement learning and supervised learning. The method was based on graph convolutional network to encode a problem instance with node and edge features. Node features were used as input of an RNN policy to output a solution, which was used to train a classifier taking edge features as inputs to predict the probability of selecting an edge in a solution. The method was evaluated on real-world data sets and shown to generalize well.

Apart from the work by Nazari et al. (2018), who considered SVRP, there is scarce literature on dynamic and stochastic VRP using modern machine learning techniques. One exception is Balaji et al. (2019) that considers stochastic and dynamic capacitated VRP problems with pickup and delivery, time windows and service guarantee. The authors showed that deep RL algorithms can directly be trained to solve them and they are competitive or superior to classic baselines.

On the other hand, traditional evolutionary algorithms have advantages in dynamic capacitated VRP problems, especially with the assistance of heuristic-based methods. Sabar et al. (2013) investigated a hyper-heuristic method assisted by a grammatical evolutionary method for capacitated VRP problem.

Jacobsen-Grocott et al. (2017) attempted to use a hyper-heuristic method to solve Dynamic Vehicle Routing with Time Windows (DVRPTW). Such problems require the acceptance or rejection decisions for dynamically arriving customer requests. The genetic programming evolved heuristics were used to determine whether or not to accept new requests and add them to the current routes. The results showed that with the dynamism degree increasing, the GP-evolved heuristics significantly outperformed the handcrafted heuristics.

Liu et al. (2017) developed a new Genetic Programming-based Hyper-Heuristic (GPHH) for automated heuristic design for Uncertain Capacitated Arc Routing Problem (UCARP), and designed a novel effective meta-algorithm. Their experimental results showed that the proposed GPHH significantly outperforms the existing GPHH methods and manually designed heuristics.

To gain more interpretable routing policies, Wang et al. (2019) used three ensemble genetic programming methods, namely, Bagging GP, Boosting GP, and Cooperative Co-evolution GP, to solve uncertain capacitated arc routing problem. Evolved depthlimited tree expressions correspond to an ensemble of these methods to represent the priorities of each task in an instance. The results showed that an ensemble of simple policies is able to compete with the complex policies while maintaining their high interpretability.

MacLachlan et al. (2020) proposed to evolve collaborative routing policies within a data-driven genetic programming hyper-heuristic algorithm for a capacitated arc

routing problem with uncertainties.

On the application side, Chen et al. (2020c) introduced genetic programming based hyper-heuristic method to solve a realistic VRP in marine container port with various uncertainty. The results confirmed the effectiveness of GP for this kind of dynamic uncertain problem.

5. Concluding Remarks and Future Directions
The vehicle routing problem and its variants are one of the most studied combinatorial optimisation problems in the research community because of its close relevance to transportation problems in industrial and societal activities, and yet few known algorithms have solved them to a satisfactory level. The main challenges lie in the scale of real-world problems, complexity in objectives and constraints (non-linearity, dynamic nature) and uncertainties. This literature review focuses on research efforts in integrating data analytics and machine learning in addressing these challenging VRP problems. A number of observations can be made.

5.1. Problem diversity and dynamics
One of most challenging aspects of VRP is its diverse and dynamic nature. As indicated in Eksioglu, Vural, and Reisman (2009b), the varieties of VRP can be attributed to the diverse problem scenario characteristics, its physical characteristics, as well as its information characteristics. This leads to numerous VRP models that have been developed to capture the main structures of various real-world problems. Although some research efforts have been made to generalise these models, big challenges still exist in terms of solution methods because these generalised models may not be able to take advantage of the underlying special structures that can be exploited for the development of more efficient algorithms. Therefore, a growing research direction is to automate (at least partially) the modelling process of real-life VRP problems by integrating data analytics and machine learning methods so that the key patterns and structures of the problems can be automatically identified and the most suitable VRP variants can be matched to the problem at hand. For practical applications, it, therefore, makes sense to develop a VRP expert system with a repository of parameterized VRP models and their corresponding algorithms so that the real-world problems can be automatically analysed, clustered and matched with one of existing model-algorithm pairs and readily solved.

Nevertheless, one should also recognise the dynamic nature of the VRP problems in real life. The problem can diverge from one variant or type to another over time. Therefore, such an expert system must be adaptive to the dynamic changes of the environments, leading to the requirements of using more advanced AI methods so that the system can evolve and improve automatically. Therefore, the ability to self-learn and self-evolve over time will be a key feature of the next generation VRP expert systems.

5.2. Perturbative improvements vs. generative approaches
As a classic NP-hard problem, VRP problems are often solved heuristically via iterative procedures, which can be broadly categorised in two different ways, namely perturbative and generative. A perturbative method is a kind of create-improve pro-

cess that assumes a deterministic (i.e. offline) model and aims to improve the quality of the solution incrementally while having ability to escaping from poor local optima. Various learning mechanisms can be used to exploit the structures of the solution landscapes. Machine learning has proved to be beneficial in helping select among a set of pre-defined perturbative heuristics or neighbourhoods in the most appropriate way. It can also be used for the automation at lower level. For example, ML can generate perturbation heuristics automatically. It can also be used to approximate the expensive solution evaluations to speed up the local search process.

Generative VRP approaches seek to build a high quality solution from scratch. Because the training process is often done offline, this type of methods has advantages compared with perturbative algorithms in terms of solution time and ability to handle uncertainties and problem related dynamics. Its main weakness is the quality of the resulting solutions which are often inferior compared with those from perturbative methods. However, with the advances in the computing power and deep learning (in particular the deep reinforcement learning), generative VRP algorithms have gained increasing popularity in recent years and have achieved more successes in solving practical VRP problems.

5.3. Model driven vs. data driven
Although in the early stage of the VRP research, significant research attentions have been paid on the perturbative methods that strive to search for the global optimality to a given problem formulation. However, it is increasingly recognised that, due to the problem uncertainties and dynamics, the perceived optimality is very rarely realised in practice. It is often impractical (if ever possible) to formulate the real-life VRP problems exactly because the resulting models will most likely be intractable. A compromise has to be made between the accuracy of the models and their tractability. This can be very challenging for practitioners.

As an alternative, data driven methods have been proposed as end-to-end solvers to VRP problems to reduce (or remove) the requirement of mathematical models. These methods train deep neural networks to produce solutions directly without the mathematical formulations, potentially making them easier to be applied in the real world. Therefore, these data driven methods often serve as black-box solvers and are often criticised for their poor interpretation abilities. Another stream of data-driven methods are based on the principles of genetic programming that evolves decision trees for solution constructions for VRP problems based on historical data. When suitable constraints and requirements are enforced during the evolution process, the resulting GP trees tend to have better interpretation abilities than the solutions from the deep neural networks.

5.4. Challenges and Prospects
Despite the fast growing popularity of integrating ML in VRP research, the research community still faces a number of challenges.

Firstly, the VRP is often part of a larger complex system that requires a good level of reliability or robustness across all possible scenarios to ensure the system does not reach certain undesirable (or disastrous) states. In addition, a good level of interpretability of the adopted algorithms is also required. Although there has been some research efforts in the area of explainable AI and verification methods, more theoretic research must

be conducted to lay solid foundations for the future research and broader applications. Secondly, there lacks a high-quality ML-assisted VRP research platform for training and testing purposes. The required resources and libraries are very scattered in two different research communities in machine learning and operations research, respectively. To help grow the ML-assisted VRP research, the two research communities must work together towards an integrated platform with libraries and tools for both ML and optimisation. Of course, this would also lead to opportunities for multidisciplinary research.

Thirdly, many of the current ML-assisted VRP research requires huge amount of data which is normally not available directly. The trained models do not generalise well across different instances, scenarios and problem domains. This lack of data and model generalisation is largely caused by the trial-and-error nature of the traditional deep reinforcement learning. Important information regarding the objective function(s) and constraints is not fully exploited. More novel approaches are required to provide a better fusion of the current analytical methods based on mathematical models and neural network methods driven by data.

Last but not least, more real-world applications must be encouraged to address the criticisms of the existing VRP research. One must balance the adoption costs and the performance of the solution methods. In particular, a much more powerful VRP simulation software is required to help practitioners to build customised VRP environment with high performance in speed at a low cost. In addition, the research community should also consider building pre-trained VRP models/libraries to reduce the training costs further.
journal homepage: www.elsevier.com/locate/cor

## Ascalable learning approach for the capacitated vehicle routing problem

James Fitzpatrick a , Deepak Ajwani b, ∗ , Paula Carroll a

- a School of Business, University College Dublin, Dublin, D04 V1W8, Ireland
- b School of Computer Science, University College Dublin, Dublin, D04 V1W8, Ireland

## A R T I C L E I N F O

## A B S T R A C T

Keywords:

Capacitated vehicle routing problem

Green vehicle routing problem

Reinforcement learning

Set partitioning problem

Designing efficient heuristics for the different variants of vehicle routing problems and customising the heuristics to various input distributions is a time-consuming and expensive task. In recent years, end-to-end machine learning techniques have been developed because they are easy to modify for different problem variants, thereby saving on the design time to develop new efficient heuristics. These learning techniques, such as the transformer-based constructive methods, struggle to provide high quality solutions on problem instances with hundreds to thousands of customers in a reasonable time. Furthermore, many of the end-to-end heuristics also do not guarantee that solutions obey fleet-size constraints. We propose a heuristic for solving large capacitated vehicle routing problem (CVRP) that carefully integrates a machine learning heuristic with Integer Linear Programming techniques. To address the issue of solutions with poor objective function values generated by end-to-end machine learning approaches on larger instances, we dynamically partition the CVRP problem instance into smaller sub-problems and apply a machine heuristic on the smaller sub-problems. This allows the machine learning heuristic to always operate on smaller problems similar in size to those for which it was trained. The machine learning heuristic generates many solutions for each sub-problem which are then combined using a set partitioning approach based on a ILP formulation. The set partitioning ILP also guarantees that solutions obey fleet-size constraints.

We evaluate the performance of our heuristic on a difficult set of benchmark instances with hundreds to thousands of nodes, achieving small gaps (less than 3% on average) with respect to best known solutions, significantly improving upon the solution quality of the existing learning heuristics. Furthermore, we demonstrate that our results generalise well to other vehicle routing problems, such as green vehicle routing problem.

## 1. Introduction

Routing-type problems (RPs) are fundamental combinatorial optimisation problems (COPs) with a wide range of applications in logistics and in networking industries. Many variants of this problem incorporate diverse real-world constraints. RPs have attracted a lot of attention from the algorithm design and optimisation community and many algorithms have been developed for the different variants. This, however, is a time-consuming process as new mathematical models and algorithms are developed for each new variant and customised for different distributions of the input datasets.

customises it to the given input distribution. Thus, ML approaches have the potential to significantly cut down on the time required to develop efficient algorithms for the different variants and customise algorithms to different distributions of the input datasets.

In recent years, machine learning (ML) approaches have been developed for many RP variants (see e.g., Vinyals et al., 2015; Nazari et al., 2018; Kool et al., 2018; Kwon et al., 2020; Hottung et al., 2021). These approaches aspire to use a common framework: for each problem variant the ML model is modified slightly, or not at all. The training datasets are swapped and the training process implicitly adapts the learning framework to the RP variant being considered and

Received 31 October 2023; Received in revised form 31 May 2024; Accepted 24 July 2024

Available online 8 August 2024

One of the most promising learning-based approaches is the endto-end learning approach in which a neural network (NN) is used as a solver to produce a solution to a given problem instance. NNs are attractive because they are easy to modify for different problem variants and can produce high-quality solutions in a small amount of time. A vexing challenge associated with learning techniques is to ensure that they both generalise and scale. While many architectural, training, inference and learning-paradigm changes have been proposed for NN solvers (Vinyals et al., 2015; Nazari et al., 2018; Kool et al., 2018; Kwon et al., 2020; Hottung et al., 2021), there is still a significant research gap to achieve good performance of NN solvers on problem instances different in size and characteristics to those on which the NN was trained. To date, trained neural network models perform well only on

![Image](image_000001_df1bc056e5a470ae08fb2536cf18d9cf28181db28f2fb552fb2e38a5c564572f.png)

![Image](image_000002_e2461357d2bc3b5460075b08d1fe2ca2a0883b107ea416740c6fc6a962ccbf4d.png)

a narrow range of problem sizes and only for problems that are similar in structure to those encountered in the training sets. Furthermore, the solutions obtained by these learning heuristics often violate the fleet-size constraint. This limits the applicability of the end-to-end NN models to academic analysis in most cases.

In this paper, we develop a learning-based heuristic with a NN solver at the core that scales well and can solve problem sizes from hundreds to thousands of customers. Our approach also obeys fleet-size constraints.

To ensure that our learning heuristic can provide high-quality solutions on large problem instances, we dynamically decompose the RP instance into a set of sub-problems. For this, we adopt a decomposition scheme similar to the POPMUSIC metaheuristic (Ribeiro et al., 2002; Queiroga et al., 2021; Li et al., 2021). Our key novel insight is that applying a reinforcement learning (RL) heuristic on the smaller subproblems allows the RL heuristic to always operate on smaller problems similar in size to those for which it was trained. Thus, RL heuristic can be used to generate many high-quality solutions for each sub-problem. These solutions can then be combined using a set partitioning approach based on a MILP formulation. The set partitioning MILP also guarantees that solutions obey fleet-size constraints.

We demonstrate that this decomposition enables learning-based approaches to leverage their success at a large scale by solving the smaller sub-problems effectively. We show that small changes in the training scheme improves generalisation performance of the neural network solver. In particular, we demonstrate the efficacy of our leaning heuristic approach for the capacitated vehicle routing problem (CVRP) with an extensive set of computational experiments. We test a benchmark set of difficult problem instances with between 300 and 1000 customers (Uchoa et al., 2017), and achieve small gaps (less than 3% on average) to the best known solutions in seconds to minutes.

Further, explore, with careful feature engineering and constraint masking, the ability of these approaches to be extended to other, more difficult vehicle routing problem variants. In particular, we demonstrate that these approaches can be used to solve the green vehicle routing problem, even for large problem instances. We solve problem instances from the difficult problem set of Erdoğan and Miller-Hooks (2012).

## 1.1. Contributions

This work presents an integration of learning-based optimisation problem solving with the traditional techniques of optimisation and operational research, namely decomposition. Using the cluster first, route second approach along with set partitioning, we leverage this marriage to solve difficult capacitated vehicle routing problem instances. In particular, we:

- · Show that neural construction heuristics can perform well, consistently, even on large problem instances.
- · We show that introducing set partitioning approaches can improve the performance.
- · We demonstrate on a difficult benchmark problem set instead of contrived problem sets.
- · We demonstrate that this can generalise to other problem variants, such as the green vehicle routing problem.

We note that our learning heuristic is the first to achieve such good results on the larger benchmark datasets by Uchoa et al. (2017). This has been possible because instead of using NNs as an end-to-end machine learning approach to solve RPs, we use a RL based heuristic to generate many good solutions for sub-problems and combine those solutions using an optimisation technique.

The rest of the paper is structured as follows: Section 2 presents background literature on optimisation techniques and machine learning heuristics for solving RPs. The necessary preliminaries on CVRP and set partitioning problem are described in Section 3. Section 4 describes our methodology in detail. Section 5 shows the computational experiments and the comparison results. Section 7 presents a discussion on the strengths and limitations of the proposed approach.

## 2. Related literature

We first provide a brief review of the exact and heuristic techniques used for solving RPs and then focus on the ML heuristics developed for this purpose.

## 2.1. Exact and heuristic techniques for RPs

While we can obtain optimal solutions for very large instances of the travelling salesman problem (TSP) (Applegate et al., 2009), these successes have yet to be replicated for any of the vehicle routing problems (VRPs). Many of the VRPs are NP-hard optimisation problems (Cordeau et al., 2007). Unsurprisingly, even for CVRP, the state-of-the-art optimisation techniques struggle to find optimal solutions once the instance size grows to a few hundred nodes. Thus, researchers have focused on developing increasingly sophisticated solution techniques using branchand-cut-and price (BCP) methods (Pessoa et al., 2020). Using BCP methods, problems with fewer than one hundred nodes can be solved to optimality in less than a minute. The CVRP, however, is considered to be mostly of academic significance or as a testing ground for other problem variants with different objective functions and additional constraints more reflective of real-world problems (Pessoa et al., 2020; Queiroga et al., 2021). Adapting these techniques to different problem variants is difficult and time-intensive, requiring expert knowledge of these methods (Pessoa et al., 2020).

For this reason, many of the state of the art solution techniques for VRPs are heuristics, some of which can construct reasonable solutions for very large problem instances in a short time frame (Vidal et al., 2012; Helsgaun, 2017). One of the most well-known effective heuristics for solving the CVRP and many related variants is the Lin-Kernighan heuristic. The LKH-3 implementation can obtain optimal solutions for many small problem instances and near-optimal solutions for many larger problem instances (Helsgaun, 2017). Also, Hybrid Genetic Search (HGS) has been shown to be extremely effective for the CVRP. It can obtain optimal solutions for many problem instances with one hundred nodes in seconds, and solutions with small optimality gaps for many larger instances with known optimal solutions (Vidal et al., 2012).

## 2.2. ML heuristics for solving VRPs

Learning approaches for solving optimisation problems has grown in popularity in recent years. Many learning-based techniques have been proposed to solve routing problems, with ML-methods augmenting existing heuristics or exact approaches. Lu et al. (2019) develop an improvement heuristic that makes use of traditional perturbation and improvement operators, but use a trained neural network instead of a hand-crafted heuristic to select these operators. In a related work, Hottung et al. (2021) propose an improvement heuristic that uses traditional destroy operators but leverages a neural network repair operator to fix destroyed routes. da Costa et al. (2021) develop an approach that depends on a neural network to select 2-opt moves in an existing solution. In some cases, the neural network is itself a construction heuristic, an idea prompted by the work of Vinyals et al. (2015) and Nazari et al. (2018). Such models coupled with reinforcement learning techniques have enabled fast and easy-to-modify heuristics to become effective at solving relatively small VRPs (Nazari et al., 2018; Kool et al., 2018). Recent advances have enabled these approaches to become competitive for solving instances with around one-hundred nodes, achieving a small gap with respect to best known solutions (Kwon et al., 2020; Hottung et al., 2021). However, they have largely defied generalisation to much larger problem instances,

with poor and unpredictable performance or excessive computation required. Despite these shortcomings, a lot of interest remains in developing ML techniques because these techniques have the potential that they can be easily adapted to different VRP variants. This can significantly reduce the time to develop new heuristics for the different VRP variants and customise it to different input distributions.

## 2.2.1. The attention model

A number of construction heuristics for RPs involve node insertions. One type of insertion heuristic proceeds with the approach that nodes are inserted one-at-a-time into an initially empty solution until a feasible solution is obtained. At any step of the process, a node is selected from 𝑉 and inserted somewhere into a partial solution. The space of possible node insertions can be quite large because it may be necessary to select both the next node to be inserted and the location in which to insert it. One way to reduce the size of the space of such decisions is to restrict the placement of inserted nodes to the end of a solution: they may only be appended to the current partial solution. This can be effective if there exists an effective mechanism 𝑠𝑒𝑙𝑒𝑐𝑡𝑁𝑜𝑑𝑒 for selecting the node at each step, given the state of the partial solution. Nodes must be selected such that no insertion renders the solution infeasible. This can be achieved by restricting the nodes under consideration for insertion to some set 𝑊 ⊂ 𝑉 with a method 𝑟𝑒𝑠𝑡𝑟𝑖𝑐𝑡𝑁𝑜𝑑𝑒𝑠 . This will depend both on the variant of the RP under consideration and the current state of the partial solution 𝑆 . A general pseudo-code is given for these approaches in Algorithm 1.

## Algorithm 1 Appending Node-Insertion Heuristic

1: Given: 𝑉

2: 𝑆 ← ()

3: 𝑊 ← 𝑉

4: while 𝑊 ≠ {} do

5: 𝑊 ← 𝑟𝑒𝑠𝑡𝑟𝑖𝑐𝑡𝑁𝑜𝑑𝑒𝑠 𝑉 ( | 𝑆 )

6: 𝑢 = 𝑠𝑒𝑙𝑒𝑐𝑡𝑁𝑜𝑑𝑒 𝑊 , 𝑆 ( )

7: 𝑆 ← append ( 𝑆, 𝑢 )

8: end while

9: return 𝑆

Starting at the depot, a partial solution may visit a sequence of customer nodes until it returns to a depot, completing a route. This process is continued until the heuristic constructs a set of routes that visits each customer exactly once.

The main idea of end-to-end learning techniques is that the node insertion can be thought of as a Markov Decision Process. The action is the node-selection and the current state is the partial solution. Each action depends only on the current state. Kool et al. (2018) describe a neural network that estimates a score 𝑝 𝑢 for each node u in Algorithm 2. A neural network 𝑓 requires a feature set representing each node 𝑢 ∈ 𝑉 , a representation of the partial solution 𝑆 and the restricted set of nodes 𝑊 . The scores can be interpreted as probabilities, since they can be made to sum to unity. The NN is trained to produce a higher score for nodes more likely to be beneficial if inserted next. Nodes can be selected greedily, select node 𝑢 with maximum probability max 𝑢 𝑝 𝑢 at each step, or randomly by sampling, where node 𝑢 is selected with probability 𝑝 𝑢 . We refer to the former as the greedy variant and the latter as the sampling variant of the AM heuristic.

The node-selection procedure is outlined in Algorithm 2.

## Algorithm 2 selectNode

1: Given: 𝑊,𝑆

2: for each 𝑣 ∈ 𝑊 : do

3: 𝑝 𝑣 = 𝑓 𝑣 𝑊 , 𝑆 ( | )

4: end for

5: 𝑢 = max 𝑣 ∈ 𝑊 𝑝 𝑣 ∕∕ Or 𝑢 = 𝑟𝑎𝑛𝑑 𝑣 ∈ 𝑊 ( 𝑝 𝑣 )

6: return 𝑢

A restriction on V ensures that any node not in 𝑊 has zero-valued 𝑝 𝑢 . The mechanism for this restriction is referred to as a mask in the machine learning literature. In the case of the CVRP, 𝑟𝑒𝑠𝑡𝑟𝑖𝑐𝑡𝑁𝑜𝑑𝑒𝑠 ensures that any customer node already in 𝑆 has a zero-valued 𝑝 𝑢 and that the previously-visited node which could be a depot also has a zerovalued 𝑝 𝑢 . This mask can be easily modified for a wide range of RP variants.

The relative simplicity of the concept of predicting a node to append to the partial solution is attractive, but it scales poorly to larger problem instances. As the problem size increases, the computation required for the NN encoding grows quadratically, which is prohibitive.

Gaps with respect to best known solutions become significant for problems with one hundred nodes or more, unless a sampling approach is taken (Kool et al., 2018). Even the sampling approach may require a large number of samples to reduce the gap enough. The POMO method, described in Section 2.2.2 illustrates a much faster approach that achieves comparable results to sampling thousands of solutions.

Several alternative architectural modifications have been made to improve the quality of the solutions produced by these attention-based end-to-end models. The work of Falkner and Schmidt-Thieme (2020) uses joint attention to construct several routes simultaneously in the hope that this will increase the likelihood of good node selections. Xin et al. (2021) use multiple decoders and dynamic embeddings to provide better information to the model that makes the node selection decisions. Duan et al. (2020) implement a combined reinforcement and supervised learning approach, using node-selection techniques that inject edge-based features to improve solution construction. These approaches, while successful to some extent, lack a key quality that we need for a general heuristic: simplicity of idea and implementation. This, according to Cordeau et al. (2002) is an essential element of any good heuristic. This is why, despite the existence of many other promising solution techniques that take advantage of ML methods (Kool et al., 2022; Lu et al., 2019, among others), we proceed with the POMO approach. This is supported by further work that has extended the POMO model successfully to problems with difficult time windows constraints (Rabecq and Chevrier, 2022).

## 2.2.2. Improved solution construction

Solutions generated with AM models have a similar structure, limiting their diversity (Kwon et al., 2020). Diversity in structure is desirable because it increases the chance of avoiding getting trapped at local optima and that a good solution will be constructed. POMO greedy selection proposes a new greedy sampling technique which constructs 𝑁 greedy solutions similar to the manner outlined in Section 2.2.1. However, a different initial customer is fixed for each solution artificially. Each solution starts at the depot and must then visit a designated customer. This approach is naive in principle, since some nodes may not be desirable to visit immediately before or after the depot, but it works extremely well in practice. This approach yields comparable results with the sampling variant of AM heuristic at a small fraction of the computational effort (Kwon et al., 2020).

Employing instance augmentation techniques yields even better solutions (see, for example, Kwon et al. (2020)). A problem instance can be augmented by applying a uniform affine transformation (rotations, reflections, translations) to the positions of the nodes. The ordering of the nodes in an optimal solution will not be affected. This augmented problem instance will yield a different NN feature representation. The AM models have been shown to construct different solutions to these augmented problems than to the original, un-augmented problems (Kwon et al., 2020). Using POMO greedy selection on the same problem augmented with 𝑑 different transforms allows one to greedily construct 𝑑 × 𝑛 solutions to the same problem. In practice, this works significantly better than the sampling variant of the AM heuristic and still at much smaller computational cost. The normalisation of the transformations can be kept consistent if they are restricted to reflections and ninety-degree rotations. Fig. 1 shows sample transformations to yield augmented problem instances.

Fig. 1. Example of a toy routing problem with POMO augmentation. The blue square represents the depot and the red circles the customers of arbitrary demand. The top row contains only rotations (about the geometric centre) of angles 𝑖𝜋 2 , with 𝑖 increasing from left to right with 𝑖 ∈ {0 1 2 3} , , , . The second row is similar, but each problem has also been flipped vertically around the centre line.

![Image](image_000003_344e1a6b32b1aeaf035242ce4bef0f1841eec812e7b77cac701b49ad4650c2eb.png)

## 3. Preliminaries

$$\ s. t. \sum _ { r \in \mathcal { R } } a _ { i r } x _ { r } = 1 \quad \quad \forall \ i \in I \quad \quad \ \ ( 3 )$$

We demonstrate the efficacy of our method on CVRP and we use the set partitioning formulation to combine the solutions of our subproblems. In this section, we describe these optimisation problems.

## 3.1. The capacitated vehicle routing problem

A CVRP instance 𝑃 can be defined on a graph 𝐺 = ( 𝑉 , 𝐸 ) , where 𝑉 is the set of nodes of the graph and 𝐸 the set of edges. The set of nodes 𝑉 is a disjoint set 𝑉 = 𝐷 ∪ 𝐼 , where 𝐷 is the set of depot nodes (usually 𝐷 = {0} ) and 𝐼 is the set of customer nodes 𝐼 = {1 … , , 𝑁 } . For each customer 𝑖 ∈ 𝐼 there is an associated nonzero demand 𝑑 𝑖 that must be satisfied. The customers are served by a homogeneous fleet of exactly 𝑘 vehicles, each with a capacity 𝑄 . Vehicles serving the customers must begin and complete their route at a depot node.

A route 𝑟 is an ordered set of customer nodes. Let | 𝑟 | be the number of customers in route 𝑟 . No route may contain the same customer twice. If a customer is visited in some route 𝑟 , it may not be visited in any other route in the solution. The sum of the demands of the customers served along a route may not exceed the total capacity of a vehicle. Let 𝑑 𝑖 be the demand of customer 𝑖 ( 𝑑 0 = 0 ) and  be the set of all possible routes, we have:

$$\sum _ { i \in r } d _ { i } & \leq Q \quad \forall r \in \mathcal { R } & ( 1 ) & \text{A v} \\ & \text{green v}$$

Each edge ( 𝑖, 𝑗 ) has an associated cost 𝑐 𝑖𝑗 . An optimal solution satisfies the constraints and minimises the sum of the costs of the edges in the solution routes.

## 3.2. The set partitioning problem

A feasible CVRP solution 𝑆 is a collectively exhaustive and mutually exclusive set of routes 𝑅 = { 𝑟 1 , … , 𝑟 | 𝑆 | } , where | 𝑆 | is the number of routes in the solution 𝑆 . Note that 𝑅 is a small subset of  , | 𝑅 ≪ | |  | , where  is the set of all feasible routes. A solution of a routing problem 𝑃 can therefore be represented as a partition of the set 𝐼 , where each element (subset of I) of the partition can be mapped to a feasible route. This allows any CVRP to be transformed to a set partitioning problem (SPP). Let 𝑥 𝑟 be a binary decision variable taking unit value if a route 𝑟 is selected and zero otherwise. Let 𝑎 be matrix with a unit entry at element 𝑎 𝑖𝑟 if the customer 𝑖 is in route 𝑟 and zero otherwise. We can formulate the SPP-CVRP as an integer linear programme (ILP) as follows:

$$\minimise$$

$$\inf \lim i s e \sum _ { r \in R } c _ { r } x _ { r } & & ( 2 ) & & \inf _ { \text{the} }$$

$$\sum _ { r \in R } x _ { r } \leq k$$

$$x _ { r } \in \{ 0, 1 \} \quad \quad \ \forall \ r \in \mathcal { R }. \quad \quad \ \ ( 5 )$$

Constraint (3) ensures that each customer is contained in exactly one selected route, while constraint (4) demands that at most 𝑘 routes are selected.

The SPP is also an NP-hard optimisation problem. Furthermore, the number of feasible routes can grow exponentially with the number of customers. However for most problem instances, the vast majority of the routes of  are not going to be in an optimal solution. It may be sufficient to solve the SPP-CVRP by identifying a small subset  𝑠 of  that is likely to contain a high-quality solution (Subramanian et al., 2013). For larger problem instances, this subset must be carefully constructed to ensure that the resulting SPP is tractable and this is exactly what we aim to do in our learning heuristic. Note that the fleetsize constraint is guaranteed to be satisfied by the SPP-CVRP solution as long as there is at least one feasible solution in  𝑠 , a condition that is ensured in our learning heuristic.

## 3.3. The green vehicle routing problem

A variety of solution techniques have been proposed to solve the green vehicle routing problem and its variants. These have been largely derived from the previously existing literature for traditional vehicle routing problem variants. Although exact approaches have been proposed to solve variants of both the green vehicle routing problem (see, for example, Koç and Karaoglan (2016) and Bruglieri et al. (2019)) and the electric vehicle routing problem (see Desaulniers et al. (2016)), they have been limited to solving relatively small problem instances. Metaheuristics are by far the most popular research direction because of their ability to quickly and effectively search the solution space (Moghdani et al., 2021). These solution methodologies generally require extensive algorithm engineering efforts and are typically used to solve problem instances that consider relatively few real-world constraints.

In industrial settings, expediency is often required not only in the run-time of a solution method, but also in the development of that solution method. One approach that has promise to solve both of these problems is the recently emerging solution method known as end-to-end learning, pioneered by Kool et al. (2018). This approach uses attention-based neural networks (see Vaswani et al. (2017)) to learn how to solve combinatorial optimisation problems under a reinforcement learning regime. Recent works in this direction, such as those of Kwon et al. (2020) and Hottung et al. (2021), have become

increasingly effective for larger and more highly-constrained problem variants. However, previous attempts to scale these approaches to larger problem sizes have largely failed.

## 4. Methodology

We first outline the heuristic at a high level and then explain each element in detail in the following sections.

## 4.1. A scalable learning-based heuristic for the CVRP

The key idea behind our learning heuristic is to dynamically decompose the RP instance into a set of roughly equal sized sub-problems. We then train a RL heuristic to solve the RP instances of size equal to the size of the sub-problems. Our RL heuristic does not just generate one solution, but many different solutions for each sub-problem. For each sub-problem, we then combine the solutions generated by the RL heuristic using a set partitioning ILP formulation. The union of the combined solutions for sub-problems gives us a high-quality solution of the original RP instance.

This heuristic can enable scaling to large graphs with little generalisation error as the RL heuristic is only used on the sub-problems that are of small size and for which it is trained to deal with. Similarly, the SPP instances also do not grow too big as they are only dealing with small sub-problems.

Algorithm 3 shows the high-level outline for the heuristic  . This heuristic takes as input a routing problem 𝑃 , a maximum sub-problem size 𝑙 , and a maximum number of iterations 𝑇 . We assume that the size 𝑁 of problem 𝑃 obeys 𝑁 ≥ 𝑙 . Otherwise, no decomposition is required. An initial solution 𝑆 is constructed for 𝑃 using some fast initial construction heuristic 𝑖𝑛𝑖𝑡𝑖𝑎𝑙𝐶𝑜𝑛𝑠𝑡𝑟𝑢𝑐𝑡𝑖𝑜𝑛𝐻𝑒𝑢𝑟𝑖𝑠𝑡𝑖𝑐 .

At each iteration, 𝑖 a sub-problem 𝑃 ′ is created from the routing problem 𝑃 using the solution 𝑆 . A subset of routes, 𝑅 ′ , of 𝑆 , are selected to form 𝑃 ′ by the subroutine 𝑠𝑒𝑙𝑒𝑐𝑡𝑆𝑢𝑏𝑃𝑟𝑜𝑏𝑙𝑒𝑚 . This subproblem contains all of the customers of those routes, as well as the depot. It may not contain more than 𝑙 nodes of the master problem. The routes 𝑅 ′ form an initial solution 𝑆 ′ to the sub-problem. The heuristic 𝑙𝑒𝑎𝑟𝑛𝑖𝑛𝑔𝐻𝑒𝑢𝑟𝑖𝑠𝑡𝑖𝑐 is then used to solve the sub-problem, giving a set of solutions, composed of a set of routes  (including the original solution 𝑆 ′ ). This is passed to an SPP solver to produce a solution 𝐾 ′ . This solution must obey the fleet-size constraint. If the objective function value 𝑐 𝐾 ( ′ ) is less than that of 𝑐 𝑆 ( ′ ) , we accept the new routes and substitute them in 𝑆 in place of 𝑅 ′ to form an improved solution using the subroutine 𝑢𝑝𝑑𝑎𝑡𝑒𝑆𝑜𝑙𝑢𝑡𝑖𝑜𝑛 .

## Algorithm 3 The General Heuristic

Require: 𝑃, 𝑙, 𝑇 𝑆 = 𝑖𝑛𝑖𝑡𝑖𝑎𝑙𝐶𝑜𝑛𝑠𝑡𝑟𝑢𝑐𝑡𝑖𝑜𝑛𝐻𝑒𝑢𝑟𝑖𝑠𝑡𝑖𝑐 𝑃 ( ) for 𝑖 ∈ {1 , ..., 𝑇 } do 𝑃 , 𝑆 , ′ ′ = 𝑠𝑒𝑙𝑒𝑐𝑡𝑆𝑢𝑏𝑃𝑟𝑜𝑏𝑙𝑒𝑚 𝑃, 𝑆, 𝑙 ( )  = 𝑙𝑒𝑎𝑟𝑛𝑒𝑑𝐻𝑒𝑢𝑟𝑖𝑠𝑡𝑖𝑐 𝑃 ( ′ ) 𝐾 ′ = 𝑠𝑒𝑡𝑃 𝑎𝑟𝑡𝑖𝑡𝑖𝑜𝑛𝑖𝑛𝑔𝑃 𝑟𝑜𝑏𝑙𝑒𝑚 (  , 𝑆 ′ ) 𝑆 = 𝑢𝑝𝑑𝑎𝑡𝑒𝑆𝑜𝑙𝑢𝑡𝑖𝑜𝑛 𝑆, 𝑆 , 𝐾 ( ′ ′ ) end for return S

Our approach is very general. We may make use of any starting heuristic we like, as long as it can generate an initial feasible solution. The sub-problem selection heuristic can be replaced for a more intricate approach, which may yield better candidates to resolve. Improved learning-based heuristics for the sub-problem may be used, and they may take the same form as those of Kool et al. (2018). We may also use alternative recombination heuristics instead of the set-partitioning approach. Section 4.2 describes our choice of initial construction heuristics for CVRP, Section 4.3 describes how we dynamically decompose the RP instance using the initial solution, Section 4.4 describes our choice of learning heuristic to solve the subproblems and Section 4.5 describes how we use CVRP-SPP to combine the RL solutions for each sub-problem.

## 4.2. The initial construction heuristic

The initial heuristic provides a starting point for the decomposition strategy. The choice of the initial heuristic is thus, crucial, for the effectiveness and easy adaptability of our learning framework. We considered the following candidates for the choice of initial heuristic in our framework:

- · Hybrid genetic search (HGS): HGS (Vidal et al., 2012) is an extremely fast and effective heuristic for the CVRP, and can yield very good solutions for large instances in seconds. Queiroga et al. (2021) have used HGS to obtain an initial solution.
- · Clarke-Wright (CW) parallel-savings heuristic (Clarke and Wright, 1964) is a relatively simple heuristic that can be easily adapted to a range of vehicle routing problems.
- · Lin-Kernighan heuristic LKH3: LKH3 (Helsgaun, 2017) is a wellknown powerful solver for the CVRP and other routing problem variants. Li et al. (2021) ran the Lin-Kernighan heuristic for 30,000 iterations to obtain an initial solution, though it took two to three hours of computation.

Even though HGS and LKH3 would have resulted in significantly better initial solutions compared to CW (see Section 5.5 for a detailed comparison), we decided to use CW as our starting heuristic. This is because our primary interest is to design a learning heuristic that can be easily adapted to a large number of VRP variants and CW heuristic is the easiest to adapt to the different variants. In contrast, we note that HGS is quite specific to CVRP and it is not easy to adapt it to other VRP variants. For many variants of the VRP, there exist neither powerful, fast heuristics, nor readily-accessible implementations. In this case we allow the CW heuristic to make disimproving ''savings'' to ensure the fleet-size constraint is obeyed from the beginning.

## 4.3. Selecting sub-problems

Next, we describe how we use the initial feasible solution 𝑆 generated by the initialConstructionHeuristic to dynamically decompose the RP instance into a set of sub-problems. To generate a sub-problem, we first select a subset 𝑆 ′ of routes in 𝑆 and then define a sub-problem 𝑃 ′ based on that. We define the support graph 𝐺 ′ to contain all of the nodes selected in routes of 𝑆 ′ and the depot, as well as the edges 𝐸 ′ between those nodes. Let the number of vehicles 𝑘 ′ required to service the customers of the sub-problem be equal to the number of routes in 𝑆 ′ . Then, the problem 𝑃 ′ is simply to solve a CVRP on 𝐺 ′ with at most 𝑘 ′ vehicles

Consider that we can view the solution 𝑆 as set of routes 𝑆 = { 𝑟 1 , … , 𝑟 𝑘 } . The subset 𝑆 ′ is initialised with a route 𝑟 ∈ 𝑆 at random. We define some distance measure 𝑑 𝑟, 𝑟 ( 𝑖 ) between the centroids of the routes 𝑟 and 𝑟 𝑖 . We insert routes into 𝑆 ′ in the increasing order of their distance 𝑑 𝑟, 𝑟 ( 𝑖 ) from the initial route 𝑟 . We keep adding routes to 𝑆 ′ as long as the total number of nodes in all routes in 𝑆 ′ is less than a predefined threshold 𝑙 . This ensures that the number of nodes in 𝑆 ′ is always less than or equal to 𝑙 .

As the solution 𝑆 gets updated during the course of the heuristic and the initial route 𝑟 is randomly selected, we obtain different sub-problems. Fig. 2 shows an overview of our decomposition approach.

This decomposition allows a RL heuristic trained on graphs of around 𝑙 nodes to be able to provide many good solutions for each of these sub-problems. In our experiments, we set the default value of 𝑙 to be 125.

An important assumption that we are making in this decomposition strategy is that in 𝑃 ′ , we ask for nodes in 𝐺 ′ to be covered by 𝑘 ′ routes. It is conceivable that the optimal solution does not obey the decomposition boundaries and even when it does obey the decomposition boundary, it covers the nodes with fewer or more than 𝑘 ′ routes. However, as we show in Section 5, our heuristic still finds solutions with small gaps to the best known solutions in a reasonable time.

Fig. 2. An illustration of the problem decomposition and sub-problem construction. In (a), we have the original (master) problem. In (b), we start with an initial solution. Each route is given a different colour and depot-customer edges are depicted as dashed for clarity. In (c) we select a subset of the nodes to form a new problem by selecting a subset of the routes. The non-selected nodes are depicted in grey. In (d) we use the learning-based heuristic to solve the sub-problem consisting of the selected nodes. The new sub-solution must have the same number of routes as the original solution to the sub-problem.

![Image](image_000004_094ec2ea1914782e72317f596c672123c1e467c75a5edc7e9b09ea9fda9068f2.png)

Fig. 3. The context vector may now include information relating to the capacity or fuel remaining in the vehicle.

![Image](image_000005_4ed3cc3e6902b601c848ae530b474675a6a4876ca54b047f547a400553e552aa.png)

## 4.4. The heuristic for solving sub-problems

Next, we describe the learning heuristic 𝑙𝑒𝑎𝑟𝑛𝑒𝑑𝐻𝑒𝑢𝑟𝑖𝑠𝑡𝑖𝑐 that we use to solve the sub-problems with up to 𝑙 customers. We base our heuristic on the AM model of Kool et al. (2018) and we use the NN to construct a set  of 8 𝑁 solutions of the sub-problem 𝑃 ′ using POMO (Kwon et al., 2020) construction with augmentation. We refer the reader to Sections 2.2.1 and 2.2.2 for more details of these techniques and refer the reader to Kool et al. (2018) for an in-depth explanation of the neural network architecture and Kwon et al. (2020) for details of the training and inference.

As noted in Section 2.2.1, the existing literature on end-to-end learning techniques has mostly focused on training NN solvers to solve VRP problems of a fixed size. Our approach constructs sub-problems with a range of sizes (bounded above by our threshold 𝑙 ), depending upon the structure of the initial feasible solution and the random initial routes selected. This implies that our learning heuristic has to be effective on graphs of different structures and varying sizes (smaller than 𝑙 ). We achieve this by training the neural network on a range of problem sizes, exposing it to problems with different values of 𝑁 before it is expected to solve the varying size sub-problems. These modifications are described below.

Although the architecture of the neural network remains largely the same as that of Kwon et al. (2020), we make some important modifications in the training and masking to adapt the approach. The architectural change is reflected in the modification of the context vector, which includes the usual graph embedding vector, 𝑣 𝑔 , initial and final node embedding vectors 𝑣 𝑖 and 𝑣 𝑓 . We may now insert representations of the current capacity 𝑄𝑡 or fuel 𝐵𝑡 of the vehicle (see Fig. 3). In particular, 𝑄𝑡 is the fraction of capacity remaining in the vehicle at step 𝑡 and 𝐵𝑡 is the fraction of fuel remaining at step 𝑡 . These are concatenated into a vector of 386 elements if both 𝑄𝑡 and 𝐵𝑡 are included, and 384 otherwise (as it is in the original works).

## 4.4.1. Training

In order to provide high-quality solutions on our sub-problems, we need to ensure that our training data is diverse in terms of the instance sizes and the problem structure. Ideally, the training dataset should have the same distribution as the test dataset. Unfortunately in the literature, there has been little focus on the role of diversity in training. Most previous learning heuristics for solving RPs that have focused the training on a narrow set of simplistic problem instances with low variability in structure and no variability in size. For instance, all problem instances used for training the AM models in the works

of Kool et al. (2018) and Kwon et al. (2020) have the same value of 𝑁 and each problem instance in each batch and epoch in a training run is generated by placing nodes randomly on a unit square with a small range of uniform random demands. Furthermore, each vehicle has an identical capacity 𝑄 . As a result, the vast majority of the optimal solutions to these problems have very similar structure: several short routes with few customers and similar length. In contrast, real-life VRP instances exhibit a much greater variation in structure. Customer nodes can be closely clustered, depots can be far away from these clusters and some routes can be extremely long.

Since we want to generate many different high-quality solutions for our sub-problems, we use more diverse problem instances for training. The work of Bdeir et al. (2022) demonstrates the efficacy of this process. Training problem instances vary in size throughout an epoch. Each batch contains problem instances of the same size, but two subsequent batches may have different values of 𝑁 . For each batch, we sample from the discrete uniform distribution 𝑁 ∼  (75 125) , when 𝑙 = 125 . Training instances are also generated using the approach of Uchoa et al. (2017) to ensure a wide variety of problem structures are encountered during training. For the purpose of this training, we do not fix 𝑘 for these instances, since we cannot guarantee that a solution produced by the neural network will have 𝑘 routes. As in previous works, the neural network is trained using reinforcement learning techniques with the reward being greatest when the length of a solution is smallest. The remaining details of our training methodology are in line with Kool et al. (2018) and Kwon et al. (2020).

## 4.5. CVRP-Set partitioning problems

A major issue with end-to-end learning heuristics for solving RPs is that when the RPs have a constraint on the number of routes 𝑘 (fleet-size constraint), the learning solutions frequently violate it and either produce solutions with too few or too many routes. This is because learning approaches minimise objective function value (solution length) without any consideration of the constraint on the number of routes. To guarantee the fleet-size constraint and improve the solution quality further, we formulate a CVRP set partitioning problem.

The routes considered in the CVRP set partitioning problem include all the routes in 𝑆 ′ . In addition, we consider all the routes in up to 8 𝑁 different solutions produced by the POMO construction augmentation (refer to Sections 2.2.2 and 4.4 for how we get up to 8 𝑁 solutions). We then use the ILP formulation described in Section 3.2 to find the minimum cost subset of atmost 𝑘 ′ (equal to number of routes in 𝑆 ′ ) routes such that all customers in 𝐺 ′ are covered.

Note that even if all the solutions returned by the 𝑙𝑒𝑎𝑟𝑛𝑒𝑑𝐻𝑒𝑢𝑟𝑖𝑠𝑡𝑖𝑐 have more than 𝑘 ′ routes, the CVRP-SPP ILP will still return a guaranteed solution with at most 𝑘 ′ routes. This is because 𝑆 ′ is a feasible solution to the ILP. In fact, the solution 𝐾 ′ will at least be as good as 𝑆 ′ in terms of solution quality.

We then update the solution 𝑆 by replacing 𝑆 ′ in 𝑆 by 𝐾 ′ . The modified 𝑆 is still a feasible solution covering all customers and satisfying fleet-size constraint while improving the overall objective. The modified 𝑆 is then used to generate the next sub-problem, if needed. We make use of the formulation in Section 3.2 for this work.

## 5. Results and analysis

In this section, we present the results of our computational experiments comparing the different learning heuristics for solving CVRP. Our focus is on large and complex instances of Uchoa et al. (2017) and hence, we primarily focus on comparing our learning heuristic  with the AM (Kool et al., 2018) and POMO (Kwon et al., 2020) techniques. We note that the other learning heuristics for solving CVRP take considerably longer to solve these larger instances.

To understand the gains resulting from the different components, we provide detailed ablation studies. We first compare our results with the

Clarke-Wright heuristic that is used as the initial construction heuristic. Next, we examine the gain from the CVRP-SPP step and the use of the POMO model as the sub-problem solver.

We also present the solution quality of the state-of-the-art handtuned, carefully-crafted HGS heuristic for CVRP. This is to understand how far are the learning heuristics from the state-of-the-art optimisation heuristic for CVRP. As noted earlier, our focus is primarily on comparing learning heuristics that can be easily adapted to solve different variants of the VRP problems and HGS cannot be easily adapted for other variants of VRP.

## 5.1. Datasets

We test the performance of our approach on the dataset of Uchoa et al. (2017). We consider only the larger problem instances with more than three hundred customers, for which the question of scalability is relevant. We consider two partitions of this benchmark set: medium-sized instances with between three and seven hundred customer (denoted X 𝑀 ) and larger instances with between seven hundred and one thousand customers (denoted X 𝐿 ).

## 5.2. Experimental setup

All experiments were carried out using Python. The neural networks were developed and trained using the pyTorch package and the SPPs were formulated and solved using the python interface for the Xpress solver. 1 The pyHygese package was used as an interface for the HGS solver. Neural network training was carried out on a machine with four Nvidia GeForce GTX 1080 GPUs and 72 Intel Xeon E5-2697 v4 @ 2.30 GHz CPUs. Inference and problem solving using the proposed method was carried out on a machine with twelve Intel Core i7-9750H @ 2.60 GHz CPUs and a single Nvidia GeForce GTX 1080 GPU.

## 5.3. Evaluation metric

Wecompare solutions in terms of gap with respect to the best known solutions (BKSs) at the time of writing. This gap is defined in Eq. (6) in terms of the objective function value 𝑧 of the incumbent solution and the objective function value 𝑧 𝐵𝐾𝑆 of the best known solution:

$$\begin{smallmatrix} \cdot \cdot \cdot \\ \text{:nation} \\ \text{ups}. \end{smallmatrix} \quad 1 0 0 \times \frac { z - z _ { B K S } } { z _ { B K S } }. \quad \quad \quad$$

We report the mean gap of five separate runs for our proposed approach  and for HGS. HGS runtimes were restricted to match the mean runtime of the five runs of our proposed approach. For the AM model, the solution was obtained by generating 𝑁 solutions. For POMO, we perform greedy node selection inference with eight augmentations of the original problem instances, as described in the original work (Kwon et al., 2020) and Section 2.2.2.

5.4. Comparison with other ML heuristics on medium-sized problem instances

The results for the medium problem instances are presented in Table 1. Comparing the various learning heuristics, we observe that performance varies considerably for the different problem instances and solution approaches. The gaps with respect to the best known solution are particularly large for the AM model, which only produces one solution at test time. POMO yields relatively better solutions in some cases because of its improved node-selection technique. However, it still yields solutions with very large gaps for most instances in this set. A moderate positive correlation exists between the gap observed for these approaches (0.449 and 0.294 respectively for AM and POMO) and the size of the problem. A stronger correlation exists (0.505 and

Table 1 Performance on the 𝑋𝑀 problem instances.

| Problem     | BKS      | Gap (%)   | Gap (%)   | Gap (%)   | Gap (%)   | Gap (%)   | Time (s)   | Time (s)   | Time (s)   | Time (s)   |
|-------------|----------|-----------|-----------|-----------|-----------|-----------|------------|------------|------------|------------|
| Problem     | BKS      | HGS       | POMO      | AM        | CW        |          | AM         | POMO       | CW         |           |
| X-n303-k21  | 21736    | 0.00      | 22.8      | 33 59 .   | 13 70 .   | 3.80      | < 1        | < 1        | < 1        | 91         |
| X-n308-k13  | 25859    | 0.01      | 15.96     | 16 91 .   | 3 57 .    | 0.90      | < 1        | < 1        | < 1        | 95         |
| X-n313-k71  | 94043    | 0.00      | 3.57      | 9 16 .    | 2 21 .    | 1.50      | < 1        | < 1        | < 1        | 94         |
| X-n317-k53  | 78355 a  | 0.00      | 2.53      | 19 83 .   | 3 85 .    | 2.30      | < 1        | < 1        | < 1        | 101        |
| X-n322-k28  | 29834 a  | 0.00      | 80.01     | 76 05 .   | 8 93 .    | 3.10      | < 1        | < 1        | < 1        | 104        |
| X-n327-k20  | 27532    | 0.00      | 36.44     | 19 39 .   | 9 31 .    | 1.20      | < 1        | < 1        | < 1        | 98         |
| X-n331-k15  | 31102 a  | 0.00      | 25.06     | 305 83 .  | 7 11 .    | 3.40      | < 1        | < 1        | < 1        | 108        |
| X-n336-k84  | 139111   | 0.01      | 3.01      | 28 77 .   | 12 67 .   | 2.60      | < 1        | 1          | < 1        | 112        |
| X-n344-k43  | 42050    | 0.01      | 21.23     | 209 37 .  | 14 75 .   | 2.00      | < 1        | 1          | < 1        | 114        |
| X-n351-k40  | 25896    | 0.02      | 14.18     | 43 47 .   | 5 93 .    | 1.30      | < 1        | 1          | < 1        | 111        |
| X-n359-k29  | 51505    | 0.01      | 10.03     | 9 85 .    | 3 30 .    | 2.10      | < 1        | 1          | < 1        | 119        |
| X-n367-k17  | 22814    | 0.01      | 16.98     | 208 44 .  | 8 02 .    | 3.30      | < 1        | 1          | < 1        | 120        |
| X-n376-k94  | 147713 a | 0.02      | 1.91      | 27 62 .   | 1 68 .    | 3.30      | < 1        | 1          | < 1        | 124        |
| X-n384-k52  | 65928    | 0.02      | 11.39     | 10 26 .   | 2 37 .    | 2.00      | < 1        | 1          | < 1        | 126        |
| X-n393-k38  | 38620 a  | 0.01      | 258.14    | 51 52 .   | 6 20 .    | 3.40      | < 1        | 1          | < 1        | 128        |
| X-n401-k29  | 66154    | 0.01      | 6.35      | 51 73 .   | 2 57 .    | 2.90      | < 1        | 1          | < 1        | 134        |
| X-n411-k19  | 19712    | 0.02      | 37.22     | 213 78 .  | 2 20 .    | 2.90      | < 1        | 1          | < 1        | 141        |
| X-n420-k130 | 107798 a | 0.02      | 4.83      | 31 57 .   | 4 71 .    | 2.30      | < 1        | 2          | < 1        | 146        |
| X-n429-k61  | 19712    | 0.03      | 26.60     | 18 04 .   | 4 46 .    | 3.60      | < 1        | 1          | < 1        | 149        |
| X-n439-k37  | 36391 a  | 0.01      | 372.30    | 298 23 .  | 9 48 .    | 3.00      | < 1        | 1          | < 1        | 158        |
| X-n449-k29  | 55233    | 0.02      | 28.07     | 24 47 .   | 5 39 .    | 2.40      | < 1        | 1          | < 1        | 161        |
| X-n459-k26  | 24139    | 0.03      | 487.25    | 94 27 .   | 15 40 .   | 1.60      | < 1        | 1          | < 1        | 170        |
| X-n469-k138 | 221824 a | 0.00      | 3.36      | 59 57 .   | 15 86 .   | 1.20      | < 1        | 2          | < 1        | 177        |
| X-n480-k70  | 89449    | 0.02      | 9.31      | 11 31 .   | 3 34 .    | 1.90      | < 1        | 2          | < 1        | 182        |
| X-n491-k59  | 66483    | 0.02      | 28.94     | 18 43 .   | 3 13 .    | 2.40      | < 1        | 2          | < 1        | 189        |
| X-n502-k39  | 69226    | 0.00      | 23.35     | 287 18 .  | 5 97 .    | 2.70      | < 1        | 2          | < 1        | 193        |
| X-n513-k21  | 24201    | 0.02      | 727.86    | 312 55 .  | 8 90 .    | 2.10      | < 1        | 2          | < 1        | 195        |
| X-n524-k153 | 154593   | 0.03      | 8.41      | 97 39 .   | 6 40 .    | 3.10      | < 1        | 4          | < 1        | 198        |
| X-n536-k96  | 94896    | 0.03      | 12.91     | 27 14 .   | 7 11 .    | 3.30      | < 1        | 3          | < 1        | 210        |
| X-n548-k50  | 86700    | 0.02      | 70.15     | 12 89 .   | 3 87 .    | 1.60      | < 1        | 3          | < 1        | 204        |
| X-n561-k42  | 42717    | 0.02      | 479.50    | 302 29 .  | 3 66 .    | 1.50      | < 1        | 3          | < 1        | 203        |
| X-n573-k30  | 50673    | 0.02      | 19.47     | 224 79 .  | 7 88 .    | 3.00      | < 1        | 3          | < 1        | 207        |
| X-n586-k159 | 190316   | 0.03      | 6.31      | 78 32 .   | 8 62 .    | 2.00      | < 1        | 5          | < 1        | 218        |
| X-n599-k92  | 108451   | 0.03      | 70.91     | 27 56 .   | 3 66 .    | 1.40      | < 1        | 4          | < 1        | 211        |
| X-n613-k62  | 59535    | 0.04      | 288.40    | 314 07 .  | 4 77 .    | 2.30      | < 1        | 4          | < 1        | 233        |
| X-n627-k43  | 62164    | 0.03      | 229.13    | 241 97 .  | 3 59 .    | 1.80      | < 1        | 4          | < 1        | 231        |
| X-n641-k35  | 63682    | 0.03      | 209.78    | 402 12 .  | 9 43 .    | 1.80      | < 1        | 5          | < 1        | 243        |
| X-n655-k131 | 106780 a | 0.01      | 47.51     | 236 15 .  | 7 88 .    | 1.70      | < 1        | 7          | < 1        | 245        |
| X-n670-k130 | 146332   | 0.03      | 25.42     | 299 54 .  | 4 00 .    | 1.60      | < 1        | 7          | < 1        | 267        |
| X-n685-k75  | 68205    | 0.04      | 232.13    | 292 48 .  | 7 24 .    | 1.90      | < 1        | 7          | < 1        | 256        |

0.450 respectively for AM and POMO) between the ratio 𝑁 𝑘 ∕ and the observed gap. This suggests that for these approaches, performance is worse for larger problem instances and for solutions with more customers in a route. In contrast, our learning heuristic  produces solutions with gap no greater than 3.8%, which is a significant improvement over AM and POMO. In fact,  outperforms AM and POMO on almost all instances in this set. For  , the correlation with problem size and mean gap is 0.097 which is very weak and suggests no association. The correlation between the ratio 𝑁 𝑘 ∕ and the observed gap is -0.349. The maximum value of 𝑁 𝑘 ∕ for this problem set is 24 . This suggests that partitioning the problem actually yields better performance on instances where the sub-problems have fewer routes (smaller 𝑘 ) with many customers (larger 𝑁 ) .

The Clarke-Wright heuristic yields much smaller gaps, in less time, than the AM and POMO heuristics, for all but one problem instance in this set. Our heuristic  can produce significant improvements in the quality of the CW initial solution with no gap greater than 3.8%.

HGS performs excellently, with gaps smaller than 1% in most cases and achieving optimal solutions in some of cases. However as noted earlier, the focus of our work is on comparing the learning heuristics that can easily adapt to other VRP variants while HGS is very specific to CVRP and cannot easily be modified for other VRP variants. The mean gap of HGS solutions is provided to understand how far are the solutions of our learning heuristic  from those of the state-of-the-art manually designed heuristic. Although there is still a good bit of gap between the solutions of  and HGS (see Table 1), we expect that as the learning techniques evolve further, this gap will reduce even for the simpler variants of VRP that have received significant research attention over many decades and where careful, hand-crafted heuristics have been developed.

The results in Table 1 are for the case where HGS is allowed to run as much time as our learning heuristic. In contrast, if we restrict HGS to run for only as much time as CW heuristic, we found that the HGS metaheuristic achieved a mean gap of 3.89% on the instances that it could obtain a solution for (84 of 100 instances) while the mean gap between the solution obtained with the CW heuristic and the best known solution across all instances of the Uchoa problem set, Uchoa et al. (2017), was 5.99%. This implies that the lower gaps of HGS come at the cost of additional running time.

## 5.5. Comparison on larger problem instances

Table 2 shows the performance results for the larger problem instances of the benchmark set. We note that none of these instances have proven optimal solutions. Similar to the case of medium sized problem instances, we observe the our learning heuristic achieves a mean gap of less than 3.3% with respect to the best known solution. This significantly outperforms the AM and POMO heuristics on all instances in this category. Furthermore, the mean gap of the solutions produced by our heuristic is still uncorrelated with the increase in the size of the problem instances (with an R-value of -0.07). The runtime of our heuristic does increase steadily with the instance size. However,

Table 2 Performance on the 𝑋𝐿 problem instances.

| Problem     | BKS    | Gap (%)   | Gap (%)   | Gap (%)   | Gap (%)   | Gap (%)   | Time (s)   | Time (s)   | Time (s)   | Time (s)   |
|-------------|--------|-----------|-----------|-----------|-----------|-----------|------------|------------|------------|------------|
|             |        | HGS       | POMO      | AM        | CW        |          | AM         | POMO       | CW         |           |
| X-n701-k44  | 81923  | 0.03      | 85 96 .   | 354 24 .  | 4 62 .    | 2.80      | < 1        | 6          | < 1        | 278        |
| X-n716-k35  | 43373  | 0.04      | 85 68 .   | 300 73 .  | 5 57 .    | 3.30      | < 1        | 7          | < 1        | 303        |
| X-n733-k159 | 136187 | 0.03      | 23 39 .   | 221 93 .  | 2 58 .    | 2.20      | 1          | 8          | < 1        | 322        |
| X-n749-k98  | 77269  | 0.03      | 210 64 .  | 173 51 .  | 3 02 .    | 1.30      | < 1        | 7          | < 1        | 335        |
| X-n766-k71  | 114417 | 0.03      | 15 38 .   | 142 82 .  | 4 34 .    | 2.30      | < 1        | 7          | < 1        | 367        |
| X-n783-k48  | 72386  | 0.03      | 312 11 .  | 259 99 .  | 6 26 .    | 2.00      | < 1        | 9          | < 1        | 378        |
| X-n801-k40  | 73305  | 0.04      | 404 58 .  | 514 36 .  | 5 31 .    | 1.80      | 1          | 10         | < 1        | 414        |
| X-n819-k171 | 158121 | 0.03      | 25 08 .   | 46 38 .   | 5 02 .    | 2.40      | 1          | 11         | < 1        | 433        |
| X-n837-k142 | 193737 | 0.04      | 38 66 .   | 248 90 .  | 3 49 .    | 2.70      | 1          | 13         | < 1        | 467        |
| X-n856-k95  | 88965  | 0.03      | 415 89 .  | 270 32 .  | 3 84 .    | 3.30      | 1          | 12         | < 1        | 471        |
| X-n876-k59  | 99299  | 0.02      | 29 35 .   | 211 38 .  | 3 10 .    | 1.40      | 1          | 12         | < 1        | 481        |
| X-n895-k37  | 53860  | 0.04      | 389 04 .  | 630 38 .  | 8 81 .    | 2.50      | 1          | 15         | < 1        | 502        |
| X-n916-k207 | 329179 | 0.03      | 12 74 .   | 25 52 .   | 4 60 .    | 1.89      | 1          | 15         | < 1        | 525        |
| X-n936-k151 | 132715 | 0.05      | 93 08 .   | 86 12 .   | 11 39 .   | 2.80      | 1          | 15         | < 1        | 544        |
| X-n957-k87  | 85465  | 0.04      | 309 89 .  | 228 99 .  | 4 27 .    | 2.40      | 2          | 19         | < 1        | 564        |
| X-n979-k58  | 118796 | 0.05      | 23 02 .   | 87 12 .   | 3 88 .    | 2.90      | 2          | 16         | < 1        | 578        |
| X-n1001-k43 | 72355  | 0.04      | 420 38 .  | 303 93 .  | 7 05 .    | 2.10      | 2          | 16         | < 1        | 601        |

Table 3

## Table 4

Comparing performance with and without the set partitioning step. Forgoing the recombination of solutions to sub-problems by solving an SPP improves run-times, but significantly impacts performance.

| Benchmark set   | Mean gap (%)   | Mean gap (%)   | Mean time (s)   | Mean time (s)   |
|-----------------|----------------|----------------|-----------------|-----------------|
|                 |  (No SPP)     |               |  (No SPP)      |                |
| X 𝑀             | 4.56           | 2.31           | 95              | 167             |
| X 𝐿             | 4.66           | 2.35           | 341             | 435             |

even the largest problem of 1000 customers has a mean solve time of just over ten minutes.

Again, we observe that the mean gap of our heuristic solutions does improve considerably over the CW heuristic. The HGS heuristic does achieve a gap of lower than 1% in almost all cases, but as stated earlier, this heuristic is not easy to adapt to other variants of VRP problems and is thus, not the focus of our comparison.

## 5.6. Ablation study: Set partitioning

Worst-case solve-times for CVRP-SPPs can be quite long even for small instances. The re-combination of many solutions to form an improved sub-solution is useful, but it is worth investigating the benefit it brings to the overall solution quality in our learning-based heuristic. If an initial solution obeys the fleet-size constraints, then it is sufficient to simply replace sub-solutions with any solution that has the same number of routes and has a better objective function value. If this is not the case, then the SPP step can help to combine known solutions into a solution that obeys the fleet-size constraints.

Table 3 shows the effects of removing the SPP step from our proposed approach. We report the mean gap over each partition of the benchmark problem set, as well as the mean solve time. Removing the SPP step leads to a significant decrease in the solve time (reducing it by almost half on average in the case of the medium-sized problem instances) but it leads to a steep deterioration in the solution quality. For both the medium and the large problem instances, the mean gap with respect to the best known solutions doubles.

## 5.7. Ablation study: AM v POMO as the internal heuristic

In this section, we study if our choice of POMO (with modified features and training data) as the learnedHeuristic to solve sub-problems in our learning heuristic is justified. We explore this by testing the AM model as the internal sub-problem solver for the proposed approach. We test both the greedy solution construction approach of the AM model (AMG) and the sampling variant of the AM model (AMS). We

Performance comparison for the smallest of the medium size problem instances. We compare timings and solution quality when the sub-solver is replaced with the original AM model.

| Problems   | Mean gap (%)   | Mean gap (%)   | Mean gap (%)   | Mean time (s)   | Mean time (s)   | Mean time (s)   |
|------------|----------------|----------------|----------------|-----------------|-----------------|-----------------|
|            |  (AMG)        |  (AMS)        |               |  (AMG)         |  (AMS)         |                |
| X 𝑆        | 6.53           | 6.21           | 2.41           | 33              | 1653            | 104             |

perform these tests for the medium-sized problems with four hundred or fewer customers (denoted by X 𝑆 ). We report performance based on the average gap from the best-known solutions and the average solution time across all instances.

From Table 4, we observe that the performance of our heuristic with modified POMO as the internal heuristic is considerably better than with AMG or AMS as an internal heuristic. The mean computation time of our heuristic with POMO as the internal heuristic is less than two minutes. Thus, we conclude that our choice of modified POMO as the sub-problem solver is well justified.

We note that the advantage of the POMO approach is that it achieves similar performance in terms of solution quality to the sampling variant of the AM model while taking approximately the same time (Kwon et al., 2020). With augmentation, solve-time is increased somewhat, but this yields significantly better solutions than the AM approach. Our results show that these advantages of POMO also result in it being a better heuristic for solving our sub-problems.

## 5.8. Using the ML heuristic as the initial heuristic

In our learning heuristic, we have used CW as our initial construction heuristic to generate the first feasible solution. CW is a simple heuristic that can be adapted for many VRP variants.

Ideally, our goal is to replace the CW heuristic with a more adaptable learning heuristic. However, there are several challenges to achieving that. The results from Tables 1 and 2 indicate that the solution quality of AM and POMO heuristics varies a lot over the benchmark problem instances. Some of the gaps with respect to the best known solutions reach hundreds of percent. Furthermore, in 49 of the 57 problem instances, the POMO greedy sampling approach produces solutions that do not obey the fleet-size constraint. In each case, more routes are produced than required. In some cases, there are hundreds more routes than in the best solution found.

To explore if a learning heuristic can be used as an initial construction heuristic, we relax the constraint that the initial solution contains the correct number of routes. This enables us to use the AM or POMO method to generate an initial solution. We allow a

## J. Fitzpatrick et al.

## Table 5

Using the learning-based heuristics as the initial heuristic. Allowing candidate subsolutions with fewer routes to replace existing sub-solutions allows the proposed heuristic to obey fleet-size constraints for more of the problem instances. This yields a comparable gap to the best known solutions.

| Problem   | Mean gap   | Mean gap         | Solutions obeying fleet-size constraints   | Solutions obeying fleet-size constraints   |
|-----------|------------|------------------|--------------------------------------------|--------------------------------------------|
|           | POMO       |  (POMO initial) | POMO                                       |  (POMO initial)                           |
| X 𝑀       | 76.97      | 2.68             | 8                                          | 21                                         |
| X 𝐿       | 229.11     | 2.89             | 0                                          | 3                                          |

candidate sub-solution to replace an existing solution if it improves the objective function value or if it reduces the number of routes required to service the customers in the sub-problem. We allow candidate subsolutions with fewer routes to replace existing sub-solutions until the fleet-size constraint is respected. After this, candidate sub-solutions may only replace existing sub-solutions if they contain the same number of routes as the existing sub-solution and the objective function value is lower. Doing this allows us to effectively match the performance of the proposed heuristic, even without SPP. Unlike  , this is not guaranteed to satisfy the fleet-size constraint. Nonetheless, Table 5 shows that our learning heuristic with POMO as the initial construction heuristic (  (POMO initial)) obtains solutions that respect the fleet-size constraints in 24 of the 57 problem instances.

## 6. Extending to other vehicle routing problem variants

Next, we show how our approach can be easily extended to other vehicle routing problem variants with a particular focus on green vehicle routing problems and electric vehicle routing problems. This extension need not require any changes to the architecture of the neural network under consideration nor in the training scheme (though small changes may greatly improve performance). It is necessary, however, to consider additional constraints, which affects the masking scheme that is adopted.

Many variants of the vehicle routing problem can be considered with simple modifications of the existing masking scheme. In some cases, we also need to modify the features, embedder and decoder context. In the remainder of this section, we describe these modifications and demonstrate the effectiveness of the extension of our approach on a green vehicle routing problem benchmark.

## 6.1. Constraints in VRP problems

We first review the constraints that are often found in the various vehicle routing problem variants.

## 6.1.1. Capacity constraints

Vehicle capacity constraints are often ignored in formulations of the green and electric vehicle routing problems. We allow for the possibility that vehicles may have a limited cargo-carrying capacity 𝑄 . The vehicles may service a demand 𝑑 𝑖 at each customer in 𝐼 . The sum of the demands of a customer in a given route 𝑟 may not exceed 𝑄 .

## 6.1.2. Time windows constraints

Time windows constraints restrict or influence the decision to service customers at particular times. A vehicle arrives at a node 𝑖 at time 𝑡 𝑖 . In the case of hard time windows, the customer 𝑖 may only be serviced between the beginning of the time window 𝑙 𝑖 and the close of the time window 𝑢 𝑖 . In the case of soft time windows, a customer may be serviced outside of this range of times, but a penalty may be incurred. It is always ensured that the time window beginning starts after the opening time of the problem ( 𝑡 = 0 ) and that the time window finish occurs before the close of the problem closing time ( 𝑡 = 𝑇 ). To ensure feasibility in the case that 𝑇 is finite, there must be enough time to return from customer 𝑖 to the depot, that is: 𝑇 - ( 𝑢 𝑖 + 𝑡 𝑖𝛿 ) ≥ 0 . For hard time windows, a vehicle must wait at the customer if it arrives before the opening of the time window 𝑢 𝑖 and can only begin servicing the customer once the time window opens.

## 6.1.3. Route duration and length constraints

In the case of route length constraints, the cumulative length of the edges used to form a complete route 𝑟 may not exceed some threshold 𝐿 . In the case of route duration constraints, the total time taken to traverse all edges, service each customer and refuel or recharge the vehicle cannot exceed the total allotted routing time 𝑇 .

## 6.1.4. Battery and fuel constraints

Battery constraints and fuel constraints concern the prevention of the condition in which the vehicle may become stranded. The maximum battery capacity or fuel level of a vehicle is 𝐵 . Traversing each arc or edge ( 𝑖, 𝑗 ) expends a certain amount of fuel or energy 𝜖 𝑖𝑗 . The total fuel level, or battery energy level, 𝑏 𝑖 , upon arrival at any node 𝑖 may never be less than zero. To prevent the vehicle from becoming stranded, it must either return to the depot before running out of fuel or battery energy, or it must visit a charging station node. Depending on the charging strategy, a family of constraints may exist to restrict the charging profile once the vehicle arrives.

The heuristic used to identify the initial solution is the end-toend model, and improvements are permitted by accepting improving solutions with fewer routes at each stage of the decomposition. The architectural differences are minor for each problem variant to the end-to-end neural network sub-solver are minor and are limited to the handling of the initial features through an embedding, the decoder context and the creation of masks to ensure the feasibility of each node selection.

## 6.2. Constraint masks

Masks are generated in the same manner as described in the original works of Kool et al. (2018) and Kwon et al. (2020) and others. If visiting a customer would violate the capacity constraint, then the visit should not be considered. If a customer has already been visited, it may not be visited again. If a node has just been visited, then it cannot be selected next for insertion. A customer may not be visited if, when starting from a node 𝑖 , there is not enough time remaining to reach 𝑗 (see Fig. 4). Similarly, a customer may not be visited if there is not enough time to both visit (and service) that customer and return to the depot. A customer may not be visited if there is not enough fuel remaining to reach node 𝑗 from node 𝑖 . A customer node may not be visited, if doing so would render the vehicle stranded. A customer may only be visited if the vehicle has enough fuel or charge to either visit that customer and return directly to the depot or if it has enough fuel or energy to visit that customer, reach a refuelling or recharging station and refuel or recharge and still have enough time to return to the depot. This set of mask becomes a tree of decisions, much like the one depicted in Fig. 5. A node may only be visited if the conjunction of all the logical conditions for visiting that node are satisfied.

During training, it is necessary to ensure the convergence of a solution by mandating that at least one customer is selected in any route. This prevents behaviour observed at the beginning stages of training where multiple routes are constructed in which only refuelling or recharging stations are visited. Constructing many routes of this kind does not render the solutions for some of the models of these problems infeasible, but it does significantly slow training or possibly make training impossible. This is because the training updates occur only once all solutions for a batch of problem instances have been completed. If any one solution does not converge, then no updates to the weights of the neural network will be made. This mask can be removed at testing time if required, or retained to guarantee the feasibility of a solution. This may come at a cost in restricting the solution space that the end-to-end model can access.

Fig. 4. Masking scheme. Considering the vehicle starts at node 𝑖 , which could be any node, it must consider if it can now visit customer 𝑗 . This is possible if it has both enough energy and time to do so and to return to the depot. Otherwise, a check can be performed to evaluate if subsequently visiting a charging station could make such a visit feasible.

![Image](image_000006_cc945550e69c9fb8e26494387caa623985a2dacc65dbeeec47bffea3991e9f1b.png)

Fig. 5. A depiction of a decision tree used to generate the mask that determines whether or not a customer may be visited, considering the vehicle is an electric vehicle. If a node is feasible, the probability 𝑝 𝑡 𝑗 that node 𝑗 is selected at time step 𝑡 is nonzero. It is zero-valued otherwise.

![Image](image_000007_84f02d9fa93aaed15efcaf9edbc1fdd37b8ff7afcabe3f06b4e064da31476f42.png)

## 6.3. Feature vectors

Next, we consider the feature vectors that can be used for these problem variants. The feature vectors describing each node represent the input information for the neural network for that node. Depending on the constraints that have been considered, the constructed feature vector may have a different size.

## 6.3.1. Depot features

The depot features are always represented by a two-dimensional feature vector. These are the coordinates of the depot in the first quadrant of the Cartesian plane.

## 6.3.2. Customer features

As with the depot features, the absolute coordinates on the Cartesian plane are always considered. In addition, the polar coordinates with respect to the depot are considered because they are found to be useful in many classical heuristics that have been developed in the literature. The normalised length of the vector between the depot node 𝛿 and the customer node 𝑖 , 𝜌 𝑖 = 𝓁 2 ( 𝑥 𝛿 , 𝑥 𝑖 )∕ √ 2 is the first of these features. The normalised angle 𝜔𝑖 = 𝜃 𝑖 ∕2 𝜋 (in radians) is computed as the angle subtended by the positive side of the horizontal axis and the line pointing from the depot to the node 𝑖 , assuming the depot to be at the origin of that coordinate system. The customer feature 𝑧 𝑖 may contain a number of elements, depending on the constraints that are considered. These include the normalised customer demand, 𝑑 𝑖 ∕ 𝑄 , where 𝑑 𝑖 is the demand of customer 𝑖 and 𝑄 is the vehicle capacity. This may also include the service time 𝑠 𝑖 , which we assume to be always less than half an hour (following the convention of Montoya et al. (2017)), but it may be normalised by the maximum service time 𝑆 to become 𝑠 𝑖 ∕ 𝑆 . It may also contain the normalised time window bounds 𝑙 𝑖 ∕ 𝑇 and 𝑢 𝑖 ∕ 𝑇 , if they are considered.

## 6.3.3. Refuelling or recharging station features

The features for a refuelling station or a recharging station should indicate the likelihood that a given station is a suitable choice, given its location and its technology. As with the other node types, the Cartesian coordinates of the nodes are considered and, as with the customer nodes, the normalised vector length 𝜌 𝑖 and angle 𝜔𝑖 with respect to the depot. If the refuelling or recharging function is modelled as a constant then an additional feature is simply the time required to perform a full charge divided by the total time allotted for routing, 𝛥 𝑖 (0 , 𝐵 )∕ 𝑇 . If the refuelling or recharging function is modelled as a linear function of the current amount of fuel or state of charge upon arrival at the station, then this feature also suffices to differentiate charging stations. If the charging function is modelled as a nonlinear function (or piecewise linear) function of the state of charge, then we may represent the charging station with the normalised time taken to complete a full refuelling or recharging and the normalised time required to perform a refuelling or recharging up to 80%, 𝛥 𝑖 (0 , 8 𝐵 10 ) . This is suggested by the fact that up until this point, the charging function of most electric vehicles can be accurately approximated as a linear function of the state of charge (Montoya et al., 2017).

## 6.4. Node feature embedder

For each node type, we have a different set of weights in the encoding. Among nodes of the same node type, the weights used to perform the embedding are shared. For depot nodes, we use the following:

$$q _ { i } = W _ { D } x _ { i } + b _ { D } \ \forall i \in D$$

For customer nodes, we use the following embedder:

$$q _ { i } & = W _ { I } x _ { i } + b _ { I } \quad \forall i \in I & ( 8 ) & \quad \text{The}$$

For refuelling stations, we use the following embedder:

$$q _ { i } & = W _ { F } x _ { i } + b _ { F } \quad \forall i \in F & ( 9 ) \quad \text{sources}$$

The rest of the encoder is as described in the work of Kwon et al. (2020).

## 6.5. Decoder context

The context for each problem variant contains different elements, depending on whether or not each element of that variant is considered. Each context vector, as with the original paper of Kool et al. (2018) and follow-up works such as Falkner and Schmidt-Thieme (2020) contain the vectors representing the embedding of the previously visited node and the graph embedding. The context must represent the current state of the solution. It must therefore contain information about the current amount of fuel, the time remaining for travel, the distance remaining for travel and the current cumulative variance in the time expended and the current cumulative variance in the fuel or battery energy used. If the current route contains a set of edges 𝑟 and the last-visited node is node 𝑗 , then we have the potential elements of the context vector as follows:

$$\underset { \ e \text{ useful} } { e \text{ useful} } \quad \bar { q } _ { i } = 1 - \frac { 1 } { T } \sum _ { e \in r } ( t _ { e } + \sigma _ { e } ^ { t } ) & & ( 1 0 )$$

$$\ e a u c {. } _ { \ } \delta \text{ and } \quad \bar { q } _ { i } = 1 - \frac { 1 } { B } \sum _ { e \in r } ^ { \mu } ( \epsilon _ { e } + \sigma _ { e } ^ { \epsilon } )$$

Here, 𝜎 𝑡 𝑒 is the variance in time taken to traverse edge 𝑒 and 𝜎 𝜖 𝑒 is the variance in energy.

## 6.6. Demonstrating effectiveness on a Green vehicle routing problem

We extended our learning approach for a green vehicle routing problem variant by adapting the mask, feature vector, embedders and decoding context as described above. Next, we demonstrate that this simple extension of our approach is already quite effective for this problem.

## 6.6.1. Generating problem instances

For the training dataset of our learning algorithm, all problem instances are generated with customers and demands distributed according to the approach of Uchoa et al. (2017). Customers can be placed at random in the unit square uniformly, in clusters or a mixture of both. The demand of a customer is also random, but are integervalued and may be generated uniformly in a random range, which may be a small range, a large range or a medium range. The demand may also be some fixed value that depends on the quadrant. Service times are generated similarly, but are not integer values, they are realvalued and may not exceed half an hour and may not be less than one twentieth of an hour.

The distance 𝑑 𝑖𝑗 between each pair of nodes is determined by computing the Euclidean distance 𝓁 2 ( 𝑥 𝑖 , 𝑥 𝑗 ) between them, multiplying it by one thousand, and rounding it (using the ceiling function) to the closest integer:

$$\text{perform} \quad d _ { i j } = \left [ 1 0 0 \ast \ell _ { 2 } ( x _ { i }, x _ { j } ) \right ]$$

The base time 𝑡 𝑖𝑗 taken to travel between each node is computed in the manner of the instances generated by Montoya et al. (2017). The base time is assumed to be the time taken for a vehicle travelling at 40 km/h. If the travel time is stochastic, then traffic conditions along an edge are presumed to be a function of the centrality of its nodes. A node that is closer on average to other nodes is assumed to have a higher degree of traffic. The centrality of a node is computed by determining the average Euclidean distance 𝑐 𝑖 between a given node and all the other nodes and subtracting it from the maximum possible distance between edges in the unit square, √ 2 :

$$c _ { i } & = \sqrt { 2 } - \frac { 1 } { n } \sum _ { j = 1, j \neq i } ^ { | V | } \ell _ { 2 } ( x _ { i }, x _ { j } ) & ( 1 3 )$$

The fuel consumption along an edge is a linear function of the time taken to traverse an edge. If this time taken to traverse an edge is stochastic, then it is a linear function of the stochastic time. That is, the fuel consumption is linear in the realised value of the time taken to traverse the given edge. Fleet size constraints are not considered to ensure that each problem instance is feasible.

## J. Fitzpatrick et al.

approach to that of the best known solution on each instance.

| Instance   |      BKS |   Our solution |   Ratio |   Our time (s) |
|------------|----------|----------------|---------|----------------|
| 20c2sC1    |  1235.21 |        1331.06 |    1.08 |              0 |
| 20c3sC2    |  1539.94 |        1610.44 |    1.05 |              0 |
| 20c3sC3    |   985.41 |        1094.53 |    1.11 |              0 |
| 20c3sC4    |  1080.16 |        1131.97 |    1.05 |              0 |
| 20c3sC5    |  2190.68 |        2210.11 |    1.01 |              0 |
| 20c3sC6    |  2785.86 |        2965.38 |    1.06 |              0 |
| 20c3sC7    |  1393.98 |        1455.74 |    1.04 |              0 |
| 20c3sC8    |  3319.71 |        3741.65 |    1.13 |              0 |
| 20c3sC9    |  1799.95 |        1810.29 |    1.01 |              0 |
| 20c3sC10   |  2583.42 |        2858.83 |    1.11 |              0 |
| 111c21s    |  5626.64 |        5914.43 |    1.05 |              1 |
| 111c22s    |  5610.57 |        5840.33 |    1.04 |              1 |
| 111c24s    |  5412.48 |        5903.1  |    1.09 |              1 |
| 111c26s    |  5408.38 |        5641.18 |    1.04 |              1 |
| 111c28s    |  5331.93 |        5491.93 |    1.03 |              1 |
| 200c21s    | 10428.6  |       11478.1  |    1.1  |             32 |
| 250c21s    | 11886.6  |       13976.3  |    1.18 |             36 |
| 300c21s    | 14242.6  |       15162.7  |    1.06 |             49 |
| 350c21s    | 16471.7  |       17651.5  |    1.07 |             54 |
| 400c21s    | 21952.5  |       24103.1  |    1.1  |             76 |
| 450c21s    | 21854.6  |       24634.1  |    1.13 |             75 |
| 500c21s    | 24527.5  |       26988.3  |    1.1  |            102 |

6.6.2. Results on the green vehicle routing problem benchmark

We trained our learning model on the generated instances described earlier with between 20 and 50 customer nodes. For testing, we use the green vehicle routing problem benchmark set of Erdoğan and Miller-Hooks (2012). We summarise the results in Table 6.

First, we note that even on instances with 400-500 customers, we are able to compute good solutions in a small time (75-102 s). To the best of our knowledge, our approach is the first machine learning technique to find such good solutions on these benchmark instances. This shows that our decomposition strategy is effective in dealing with large instances in reasonable time. We believe that with further algorithm engineering tricks, this approach can be scaled to even solve problem instances with thousands of nodes.

In terms of generalisation, we note that our approach is able to achieve consistently good results for large problem instances, even though it was trained on significantly smaller problem instances. Compared to the best known solution on each instance, the solutions produced by our approach have a ratio of less than equal to 1.18 on all instances and less than equal to 1.1 on most instances. Also, from the results shown in Table 6, we observe that the optimality ratio with respect to the best known solutions shows moderate correlation with the size of the problem instances ( 𝑅 = 0 408 . ). In terms of generalisation across instance distributions, we note that the training instances are synthetically generated and the test instances come from the benchmark set of Erdoğan and Miller-Hooks (2012).

These results indicate that by simply constructing a new masking scheme and slightly adapting the features, embedder and decoder, our approach can be used to solve other vehicle routing problem variants such as the green vehicle routing problem.

## 7. Discussion

Partitioning a large problem instance into smaller sub-problems enables machine-learning based solvers, which can solve smaller problems effectively, to scale. This does not involve adapting the architecture of the machine-learning solver, but uses it effectively out-of-the box. The benefit of this is that existing effective solvers can be immediately incorporated in this way. Since many of these approaches can be made to solve a large variety of problem variants, this approach can be adapted to solve large problems for those variants also. This should minimise engineering effort for developing effective solvers for more difficult problem instances with a large variety of instances sizes using machine-learning techniques.

Our learning heuristic uses the ML models only on problems of a similar size for which they are trained. This means that we do not have to engineer the ML models themselves for scalability. Our decomposition is a route-first type of decomposition, instead of, say, a geometric decomposition based solely on the angular position of the customer nodes. This is to ensure that we can satisfy the fleet-size constraint.

Compared to the AM and POMO approaches, the main gain of our heuristic comes from the decomposition of the problem and using the learning models on sub-problems of roughly similar size. Running the CVRP-SPP improves the solution quality of sub-problems even further. In contrast, the gains from changing the features and updating the training procedure are relatively modest.

## The Set Partitioning Problem

Our ablation study indicates that the set partitioning step can increase the quality of the solutions obtained by reducing the gap with respect to the best known solution, as shown in Table 3. However, the time required to yield such an improvement is significant. Should the time be available for such methods to be used, the set partitioning approach is a general one that can work for any variant of a routing problem. All that is required is an existing set of feasible routes, with a feasible solution guaranteed if these routes are obtained from solutions to the original problem. In some cases performance of the machine learning heuristic may be poor enough on the sub-problems that improving solutions are rarely generated. The set partitioning step may be necessary for any significant improvements to be made after the first few iterations. Without this step, the solution quality may align closely to that of the initial solution. An alternative approach might be to make use of the retraining mode discussed in the work of Kwon et al. (2020). Instead of using the solution construction scheme using the trained machine learning heuristic, a training mode is used instead. In essence, the trained heuristic acts as a strong basis for finding good solutions for a particular problem variant. At test time, however, training can be restarted using only the problems encountered at test time. By re-training the machine learning heuristic on only these problems, generalisation performance is traded for improved solution quality for only these test problems. By starting with an already-trained machine learning heuristic, we can yield improved solutions to these problems at test time. This may allow this approach to yield better solutions without the need of the set partitioning approach. Future work could focus on the development of this approach, extending the retraining scheme to other problem variants and implementing it as the core machine learning heuristic of this technique.

## Selecting Sub-Problems

In this work, we select sub-problems on which to attempt improvement with a hand-crafted heuristic, relying on distances of route centroids. The initial route is selected randomly. Although this can yield improvements, it remains to develop this further. One approach may involve introducing a tabu list, for example, to ensure focus is placed on regions of the problem where improvement has not yet occurred. Another approach may be to develop a learning-based approach for sub-problem selection. This could encourage improvement on regions that are most likely to yield improvements and reduce the time spent on sub-problems where no improvement occurs.

Dealing with Routes that have Many Customers The performance of our approach depends on the ability of the internal sub-solver to quickly and effectively solve the sub-problems. In our approach, we limit the sub-problem size to 𝑙 and assume that the sub-problem sizes are in a limited range below 𝑙 . Thus, a learning model trained on instances of sizes in that limited range would be able to deal with the sub-problems effectively. However, if the number of vehicles 𝑘 serving the customers is small and the number of customers is large, it is possible that 𝑁 𝑘 ∕ is close to 𝑙 or perhaps larger than 𝑙 . If it is close to 𝑙 ,

then only one route will generally be used to solve a sub-problem. This effectively reduces each sub-problem to a travelling salesman problem over the customers contained in that route. If it is larger than 𝑙 , then we will have to increase 𝑙 or to abandon the solve. If we increase 𝑙 , then the trained learning models may not be effective on the larger size subproblems and as a result, there may be fewer (if any) improvements in sub-problem solutions.

The recent approach of Efficient Active Search (Hottung et al., 2021) offers an approach to yield improved solution quality even for problem instances much different than those encountered at training time. This is achieved by adding an instance-specific layer to the architecture of the neural network solver. This layer only is retrained at test time for a test instance to improve the solution quality for that specific instance. This allows learning-based solvers to obtain highquality solutions even for problem instances larger than those that the neural network was intended to solve. This comes at the cost of increased solve times as a result of retraining, but solution quality gains can be significant. Classical techniques find it considerably harder to obtain good solutions for such problems than for problems with many shorter routes (Pecin et al., 2017). Initial experiments replacing the sub-solver with the EAS approach indicate that this may be a promising approach in future works, since it can be used to solve problem instances that have routes with many customers.

## 8. Conclusions

We proposed a learning heuristic for solving VRP problems that decomposes a problem instance into smaller sub-problems and solves the sub-problems using a neural network based machine learning heuristic. We leverage the decomposition to ensure that the neural network solver is only ever called to solve sub-problems of the master problem that it has been trained to solve effectively. This allows us to obtain good solutions for large problems without significantly changing the neural network architecture. The solutions for the sub-problems are further improved by formulating and solving a set-partitioning problem. We show that the solutions from our heuristic have small gaps with respect to the best known solutions on the large and complex CVRP problem instances from the dataset of Uchoa et al. (2017). We provide an ablation study that quantifies the gains resulting from our choice of heuristics for the different components.

Our learning heuristic provides a general framework for solving a wide range of other, more highly-constrained routing problem variants with little modification. This has the potential to significantly reduce the time to design new learning heuristics for the different variants and for customising them to specific input distributions. Initial experimentation shows that this can be used to solve problem instances of the GVRP of Erdoğan and Miller-Hooks (2012), even at scale.

## CRediT authorship contribution statement

James Fitzpatrick: Conceptualization, Methodology, Software, Investigation, Writing - original draft. Deepak Ajwani: Conceptualization, Methodology, Writing - original draft. Paula Carroll: Conceptualization, Methodology, Writing - review &amp; editing.

## Data availability

We have used publicly available datasets in our work.

## Acknowledgements

This publication has emanated from research conducted with the financial support of Science Foundation Ireland under Grant number 18/CRT/6183. For the purpose of Open Access, the authors have applied a CC BY public copyright licence to any Author Accepted Manuscript version arising from this submission.

## References

Applegate, D.L., Bixby, R.E., Chvátal, V., Cook, W., Espinoza, D.G., Goycoolea, M., Helsgaun, K., 2009. Certification of an optimal TSP tour through 85,900 cities. Oper. Res. Lett. 37 (1), 11-15.

- Bdeir, A., Falkner, J.K., Schmidt-Thieme, L., 2022. Attention, filling in the gaps for generalization in routing problems. In: Joint European Conference on Machine Learning and Knowledge Discovery in Databases (ECML PKDD). Springer, pp. 505-520.

Bruglieri, M., Mancini, S., Pezzella, F., Pisacane, O., 2019. A path-based solution approach for the green vehicle routing problem. Comput. Oper. Res. 103, 109-122. Clarke, G., Wright, J.W., 1964. Scheduling of vehicles from a central depot to a number of delivery points. Oper. Res. 12 (4), 568-581.

- Cordeau, J.-F., Gendreau, M., Laporte, G., Potvin, J.-Y., Semet, F., 2002. A guide to vehicle routing heuristics. J. Oper. Res. Soc. 53, 512-522.
- Cordeau, J.-F., Laporte, G., Savelsbergh, M.W., Vigo, D., 2007. Vehicle routing. In: Handbooks in Operations Research and Management Science. Vol. 14, Elsevier, pp. 367-428.
- da Costa, P., Rhuggenaath, J., Zhang, Y., Akcay, A., Kaymak, U., 2021. Learning 2-opt heuristics for routing problems via deep reinforcement learning. SN Comput. Sci. 2, 1-16.
- Desaulniers, G., Errico, F., Irnich, S., Schneider, M., 2016. Exact algorithms for electric vehicle-routing problems with time windows. Oper. Res. 64 (6), 1388-1405.
- Duan, L., Zhan, Y., Hu, H., Gong, Y., Wei, J., Zhang, X., Xu, Y., 2020. Efficiently solving the practical vehicle routing problem: A novel joint learning approach. In: Proceedings of the 26th ACM SIGKDD International Conference on Knowledge Discovery &amp; Data Mining. pp. 3054-3063.
- Erdoğan, S., Miller-Hooks, E., 2012. A green vehicle routing problem. Transportation research part E: Logistics and Transportation Review 48 (1), 100-114.
- Falkner, J.K., Schmidt-Thieme, L., 2020. Learning to solve vehicle routing problems with time windows through joint attention. arXiv preprint arXiv:2006.09100.
- Helsgaun, K., 2017. An Extension of the Lin-Kernighan-Helsgaun TSP Solver for Constrained Traveling Salesman and Vehicle Routing Problems. Vol. 12, Roskilde: Roskilde University.
- Hottung, A., Kwon, Y.-D., Tierney, K., 2021. Efficient active search for combinatorial optimization problems. arXiv preprint arXiv:2106.05126.
- Koç, Ç., Karaoglan, I., 2016. The green vehicle routing problem: A heuristic based exact solution approach. Appl. Soft Comput. 39, 154-164.
- Kool, W., van Hoof, H., Gromicho, J., Welling, M., 2022. Deep policy dynamic programming for vehicle routing problems. In: International Conference on Integration of Constraint Programming, Artificial Intelligence, and Operations Research (CPAIOR). Springer, pp. 190-213.

Kool, W., Van Hoof, H., Welling, M., 2018. Attention, learn to solve routing problems!. arXiv preprint arXiv:1803.08475.

- Kwon, Y.-D., Choo, J., Kim, B., Yoon, I., Gwon, Y., Min, S., 2020. Pomo: Policy optimization with multiple optima for reinforcement learning. Adv. Neural Inf. Process. Syst. 33, 21188-21198.
- Li, S., Yan, Z., Wu, C., 2021. Learning to delegate for large-scale vehicle routing. Adv. Neural Inf. Process. Syst. 34, 26198-26211.
- Lu, H., Zhang, X., Yang, S., 2019. A learning-based iterative method for solving vehicle routing problems. In: International Conference on Learning Representations.
- Moghdani, R., Salimifard, K., Demir, E., Benyettou, A., 2021. The green vehicle routing problem: A systematic literature review. J. Clean. Prod. 279, 123691.
- Montoya, A., Guéret, C., Mendoza, J.E., Villegas, J.G., 2017. The electric vehicle routing problem with nonlinear charging function. Transp. Res. B 103, 87-110.
- Nazari, M., Oroojlooy, A., Snyder, L., Takác, M., 2018. Reinforcement learning for solving the vehicle routing problem. Adv. Neural Inf. Process. Syst. 31.
- Pecin, D., Pessoa, A., Poggi, M., Uchoa, E., 2017. Improved branch-cut-and-price for capacitated vehicle routing. Math. Program. Comput. 9, 61-100.
- Pessoa, A., Sadykov, R., Uchoa, E., Vanderbeck, F., 2020. A generic exact solver for vehicle routing and related problems. Math. Program. 183, 483-523.
- Queiroga, E., Sadykov, R., Uchoa, E., 2021. A POPMUSIC matheuristic for the capacitated vehicle routing problem. Comput. Oper. Res. 136, 105475.
- Rabecq, B., Chevrier, R., 2022. A deep learning attention model to solve the vehicle routing problem and the pick-up and delivery problem with time windows. arXiv preprint arXiv:2212.10399.
- Ribeiro, C.C., Hansen, P., Taillard, É.D., Voss, S., 2002. POPMUSIC-Partial optimization metaheuristic under special intensification conditions. Essays Surv. Metaheuristics 613-629.
- Subramanian, A., Uchoa, E., Ochi, L.S., 2013. A hybrid algorithm for a class of vehicle routing problems. Comput. Oper. Res. 40 (10), 2519-2531.
- Uchoa, E., Pecin, D., Pessoa, A., Poggi, M., Vidal, T., Subramanian, A., 2017. New benchmark instances for the capacitated vehicle routing problem. European J. Oper. Res. 257 (3), 845-858.
- Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A.N., Kaiser, Ł., Polosukhin, I., 2017. Attention is all you need. Adv. Neural Inf. Process. Syst. 30.

## J. Fitzpatrick et al.

Vidal, T., Crainic, T.G., Gendreau, M., Lahrichi, N., Rei, W., 2012. A hybrid genetic algorithm for multidepot and periodic vehicle routing problems. Oper. Res. 60 (3), 611-624.

Vinyals, O., Fortunato, M., Jaitly, N., 2015. Pointer networks. Adv. Neural Inf. Process. Syst. 28.

Xin, L., Song, W., Cao, Z., Zhang, J., 2021. Multi-decoder attention model with embedding glimpse for solving vehicle routing problems. In: Proceedings of the AAAI Conference on Artificial Intelligence. Vol. 35, pp. 12042-12049.