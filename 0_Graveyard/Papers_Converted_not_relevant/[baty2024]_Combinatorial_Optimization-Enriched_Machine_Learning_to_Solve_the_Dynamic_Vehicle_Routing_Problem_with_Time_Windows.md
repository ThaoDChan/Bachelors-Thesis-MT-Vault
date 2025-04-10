## Combinatorial Optimization enriched Machine Learning to solve the Dynamic Vehicle Routing Problem with Time Windows

Léo Baty ∗1 , Kai Jungel ∗2 , Patrick S. Klein ∗2 , Axel Parmentier , and Maximilian Schiffer 1 2,3

1 CERMICS, Ecole des Ponts, Marne-la-Vallée, France

2 School of Management, Technical University of Munich, Munich, Germany 3 Munich Data Science Institute, Technical University of Munich, Munich, Germany

## Abstract

With the rise of e-commerce and increasing customer requirements, logistics service providers face a new complexity in their daily planning, mainly due to efficiently handling same day deliveries. Existing multi-stage stochastic optimization approaches that allow to solve the underlying dynamic vehicle routing problem are either computationally too expensive for an application in online settings, or - in the case of reinforcement learning - struggle to perform well on high-dimensional combinatorial problems. To mitigate these drawbacks, we propose a novel machine learning pipeline that incorporates a combinatorial optimization layer. We apply this general pipeline to a dynamic vehicle routing problem with dispatching waves, which was recently promoted in the EURO Meets NeurIPS Vehicle Routing Competition at NeurIPS 2022. Our methodology ranked first in this competition, outperforming all other approaches in solving the proposed dynamic vehicle routing problem. With this work, we provide a comprehensive numerical study that further highlights the efficacy and benefits of the proposed pipeline beyond the results achieved in the competition, e.g., by showcasing the robustness of the encoded policy against unseen instances and scenarios.

Keywords: vehicle routing, structured learning, multi-stage stochastic optimization, combinatorial optimization, machine learning

∗ The first three authors contributed equally to this work.

## 1 Introduction

With the rise of e-commerce during the last decade, logistics service providers (LSPs) were exposed to increasing customer requirements, particularly with respect to (fast) delivery times. Accordingly, the concept of same-day deliveries, where LSPs guarantee to fulfill an order on the day on which they receive it, became a key element in the B2C sector. In fact, the share of same-day deliveries grew by 18.5% in 2021 (Maida 2022). As e-commerce and B2C deliveries remain competitive markets with several major players, retailers and LSPs continuously aim to outbid each other, which led to continuously shortened lead times, offering delivery within as little as two hours for certain product types in selected cities (Nicolai 2016).

Realizing last-mile deliveries within such short planning horizons remains inherently challenging from an efficiency perspective as LSPs generally trade off short lead times against oversized resources, e.g., by maintaining an oversized fleet to always be able to immediately react to incoming orders. In fact, the concept of same-day deliveries leads the concept of day-ahead planning, which has been the status quo in last-mile logistics for decades, ad absurdum. Instead of solving a combinatorially complex but static planning problem to determine cost-efficient delivery routes, LSPs have to dynamically dispatch orders to vehicles and route these vehicles in an online problem setting. Taking decisions in such a setting requires anticipating the benefit of dispatching or delaying an order while still inheriting the combinatorial complexity of the corresponding static planning problem and handling uncertainty with respect to future incoming orders.

The challenges that arise in such planning problems in practice invigorate the interest in dynamic vehicle routing problem (VRP) variants from a scientific perspective. State-of-the-art methodologies to solve such problems model the underlying planning task as a multi-stage stochastic optimization problem solved either directly (Pillac et al. 2013; Soeffker, Ulmer, and Mattfeld 2022) or via reinforcement learning (Nazari et al. 2018; Hildebrandt, Thomas, and Ulmer 2023; Basso et al. 2022). However, both of these approaches bear a major drawback. Reinforcement learning based algorithms succeed in taking anticipating decisions but often struggle when being applied to high-dimensional combinatorial problems. Contrarily, stochastic optimization techniques are generally amenable to combinatorial problem settings but struggle with respect to computational efficiency in high-dimensional problems, which makes them impracticable to use in a dynamic setting (Pflug and Pichler 2014; Carpentier et al. 2015).

Against this background, we propose a new methodological approach that mitigates the aforementioned shortcomings. Specifically, we develop a machine learning (ML) pipeline with an integrated combinatorial optimization (CO)-layer that allows to efficiently solve the dynamic VRP. This pipeline mitigates the challenges of multi-stage stochastic optimization problems by design: its ML-layer allows to incorporate uncertainty by adequately parameterizing an instance of the underlying deterministic CO problem, which can then be efficiently solved within the CO-layer. We used this pipeline in the EURO Meets NeurIPS Vehicle Routing Competition at NeurIPS 2022 (Kool, Bliek, et al. 2022), where it outperformed all other approaches.

## 1.1 Related work

Our work contributes to two different streams of research: from an application perspective it relates to dynamic VRPs and from a methodological perspective it relates to CO-enriched ML. We provide an overview of both related research streams in the following. For a general overview of VRPs we refer to Vidal, Laporte, and Matl 2020 and for a general overview of CO-enriched ML we refer to Bengio, Lodi, and Prouvost 2021, and Kotary et al. 2021.

Dynamic Vehicle Routing Problems Dynamic VRPs account for the dynamic nature of real-world processes where some problem data, such as the customers to serve, their demand, or the travel time between them, is not known in advance but revealed over time. Dynamic VRPs hence have a diverse field of applications ranging from ride-hailing (Jungel et al. 2023) over grocery delivery (Fikar 2018) to emergency services (Alinaghian, Aghaie, and Sabbagh 2019). To keep this literature review concise, we focus on dynamic VRPs in the context of dynamic dispatching problems in the following and refer to Pillac et al. (2013) and Ojeda Rios et al. (2021) , and Ulmer, Soeffker, and Mattfeld (2018) for comprehensive reviews of the field. Approaches to solve dynamic VRPs can be broadly categorized into CO-based approaches,

which leverage the combinatorial structure of the underlying problem, and ML-based approaches, which learn prescient policies accounting for the uncertainty of future customers.

Pure CO approaches generally amend solution methods developed for static VRPs to the dynamic case. Specifically, these CO approaches embed (meta-)heuristics into rolling horizon frameworks, i.e., solve a static variant of the considered problem each time new information enters the system (see, e.g., Ritzinger, Puchinger, and Hartl 2016; Ojeda Rios et al. 2021). Here, myopic approaches (see, e.g., Gendreau et al. 1999; Steever, Karwan, and Murray 2019) utilize only the current problem state, while look-ahead approaches (e.g., Flatberg et al. 2007; Bent and Van Hentenryck 2004) take into account potential realizations of future periods, e.g., via sampling.

ML approaches learn policies which account for the uncertainty of future observations. Accordingly, they model the underlying dynamic problem as a markov decision process solved with either approximate dynamic programming methods (see, e.g., Ulmer, Soeffker, and Mattfeld 2018), i.e., policy- or value function approximation, or reinforcement learning (see, e.g., Nazari et al. 2018; Kool, Hoof, and Welling 2019; Joe and Lau 2020). We refer to Raza, Sajid, and Singh 2022 and Hildebrandt, Thomas, and Ulmer 2023 for a review on reinforcement learning applied to dynamic VRPs.

As can be seen, various works exist that solve dynamic VRPs. Here, most approaches either utilize classical CO algorithms by sampling future scenarios or apply ML to approximate decision values which account for future expected rewards. All of these approaches contain at minimum one of the following shortcomings: classical CO-based algorithms struggle to amend to real-time requirements of dynamic problem settings, while ML-based approaches often lack solution quality as they do not take the problem's combinatorial structure into account. A truly integrated approach which combines CO and ML, thus leveraging the advantage of ML in dynamic settings without disregarding the problem's combinatorial structure, has so far not been proposed for dynamic VRPs.

Combinatorial Optimization enriched Machine Learning Many real-world combinatorial dispatching problems are subject to uncertain future events. To find combinatorial solutions which account for these uncertain events, one can integrate CO-layers into ML-based pipelines. We refer to such pipelines as COenriched ML pipelines. The main obstacle for using CO-layers in ML pipelines is their piecewise constant nature. Specifically, gradients are zero almost everywhere, and thus uninformative in such settings, rendering straightforward backpropagation ineffective. State-of-the-art methods address this issue and introduce regularization techniques that smoothen CO-layers to enable meaningful gradient computation, allowing their usage in ML-based pipelines. Both additive (Berthet et al. 2020) and multiplicative (Dalle et al. 2022) regularization approaches have been applied successfully to CO-enriched ML pipelines in supervised learning settings with Fenchel-Young losses (Blondel, Martins, and Niculae 2020).

A common application of these pipelines are hard CO problems, e.g., single machine scheduling with release dates (Parmentier and T'Kindt 2021), for which classical CO approaches are often intractable. Here, the statistical model learns to parameterize an embedded, tractable CO problem that shares the feasible solution space with the intractable hard problem, such that it produces a solution that is valid for the original problem. This approach can be adapted to learn the parameterization of multi-stage optimization problems such as the two-stage stochastic minimum spanning tree problem (Dalle et al. 2022) or the stochastic vehicle scheduling problem (Parmentier 2021; Parmentier 2022). Jungel et al. 2023 used a CO-enriched ML pipeline to learn a dispatching policy in a dynamic autonomous mobility-on-demand system. While the work of Jungel et al. 2023 may seem similar to this work at first sight, it differs in several fundamental aspects: the dispatching problem studied in Jungel et al. 2023 is solvable in polynominal time and hence does not rely on a heuristics in the CO-layer. Moreover the statistical model of Jungel et al. 2023 takes the form of a generalized linear model whereas we rely on deep learning in this work.

While CO-enriched ML pipelines are widely used to solve real-world problems, existing work in this field relies on exact algorithms in the CO-layer so far. However, this approach is not tractable in settings where the CO problem is difficult to solve, as is the case in most dynamic VRPs. Moreover, existing work assumes CO-layers with linear objective functions where the predicted objective costs have the same dimension as the decision variables, which is not the case in our dynamic VRP setting.

## 1.2 Contributions

To close the research gap outlined above, we propose a novel ML-based pipeline enriched with a COlayer, and apply it to a novel class of dynamic VRPs introduced in the EURO Meets NeurIPS Vehicle Routing Competition that is highly interesting for academia and practice. Specifically, our work contains several contributions. From a methodological perspective, we generalize the CO-layer of CO-enriched ML pipelines to non-linear objective functions. Note that in this context, we also extend the open source library InferOpt.jl to such non-linear settings. Moreover, we present the first CO-enriched ML pipeline that utilizes a metaheuristic component to solve the CO-layer. By so doing, we show that our general ML-CO paradigm, which formally requires optimal solutions to derive true gradients, can work well with heuristic solutions in practice. In this context, we detail how to carefully design a metaheuristic that allows to derive heuristic solutions, i.e., approximate gradients, which enable convergence in the ML-layer of our pipeline. We show how to train the ML-layer of this pipeline in a supervised learning setting, i.e., based on a training set derived from an anticipative strategy. We present a comprehensive numerical study to show the efficacy of our methodology in a benchmark against state-of-the-art approaches. Beyond this, our study validates that our learning approach generalizes well to unseen scenarios. Interestingly, our results point at the fact that, counterintuitive to common practice, imitating an anticipative strategy can work well for high-dimensional multi-stage stochastic optimization problems.

We refer to our git repository ( https://github com/tumBAIS/euro-meets-neurips-2022 . ) for instructions and all material necessary to reproduce the results outlined in this paper.

## 1.3 Outline

The remainder of this paper is organized as follows. Section 2 provides a formal definition of our problem setting before Section 3 introduces our CO-enriched ML pipeline. We then detail the individual layers of our pipeline. Specifically, Section 4 details the algorithmic framework used in the CO-layer, while Section 5 details the learning methodology for the ML-layer. Section 6 details the design of our computational study and it's results. Finally, Section 7 concludes the paper.

## 2 Problem setting

Our problem setting focuses on a variant of the dynamic vehicle routing problem with time windows (VRPTW) introduced in the EURO Meets NeurIPS Vehicle Routing Competition (see Kool, Bliek, et al. 2022). In this dynamic VRPTW, we aim to find a cost-minimal set of routes that start and end at a central depot d and allow a fleet of vehicles to serve a set of requests R within a finite planning horizon [0 , T max ] . We focus on an online problem setting in which the request set R is initially unknown and requests continuously arrive over [0 , T max ] . Within this planning horizon, the fleet operator makes dispatching decisions at (equidistant) time steps τ ∈ [0 , T max ] and needs to serve all requests that arrive during the planning horizon. Accordingly, we discretize the planning horizon into a set of n epochs E = { [ τ 0 , τ 1 ] , [ τ 1 , τ 2 ] , . . . , [ τ n -1 , τ n ] } and denote the start time of an epoch e as τ e .

In each epoch, the fleet operator solves a dispatching and vehicle routing problem for the epoch dependent request set R e . Each request r ∈ R e has a certain demand q r , and vehicles have a homogeneous vehicle capacity Q , which limits the maximum number of requests serviceable on a single route. Serving a request r takes s r time units, and a request must be served within a request-specific time window [ glyph[lscript] r , u r ] . Traveling from the delivery location of request i to the delivery location of another request j takes t ij time units and incurs cost c ij . We can straightforwardly encode an instance of an epoch's planning problem on a fully-connected digraph D e = ( V e , A e ) with a vertex set V e = R ∪{ } e d where d is the depot, and an arc set A e . With this notation, our problem representation unfolds as follows.

System state We describe a system state at decision time τ e as x e = R e , where e is the epoch starting at time τ e . Here, R e contains all requests revealed but not yet dispatched. In our specific setting no further

information is required to describe the system state as there is no fleet limit. Note that we use redundant notation x e = R e to adhere to conventions commonly used in the domains of ML and vehicle routing, respectively. We can distinguish requests contained in R e into two disjoint categories: must-dispatch requests need to be dispatched in e as dispatching these in a later epoch would violate the requests' time window upon delivery; postponable requests can but do not have to be dispatched in e .

Feasible decisions Given the current state of the system x e , the fleet operator chooses a subset of R e that will be served by vehicles leaving the depot in this epoch, and computes the respective routes that allow for vehicle dispatching. Vehicles dispatched in epoch e leave the depot at time τ e +∆ τ and their routes cannot be modified once they have been dispatched, i.e., vehicles dispatched in epoch e cannot serve requests revealed in epoch e ′ , with τ e ′ &gt; τ e . In this context, a feasible decision y e ∈ Y ( x e ) in state x e corresponds to a set of routes that

- (i) contains all must-dispatch requests,
- (ii) allows each route to visit all contained requests within their respective time windows, and
- (iii) the cumulative customer demand on each route does not exceed the vehicle capacity Q .

We can encode a feasible decision y e ∈ Y ( x e ) with a vector ( y e i,j ) ( i,j ) ∈A e where

$$y _ { i, j } ^ { e } = \begin{cases} 1 & \text{if $(i,j)$ is in a route of the solution} \\ 0 & \text{otherwise.} \end{cases}$$

System evolution The system transitions into the next epoch e ′ once the fleet operator decides on y e ∈ Y ( x e ) . To describe x e ′ , we derive R e ′ by removing all requests contained in y e from R e , and adding all requests that enter the system between τ e and τ e ′ .

Policy Let X denote the set of potential system states. Then, a (deterministic) policy π : X → Y is a mapping that assigns a decision y e ∈ Y ( x e ) to any system state x e ∈ X .

Objective We aim to find a policy that minimizes the expected cost of serving all requests R over the planning horizon [0 , T max ] . Formally,

$$\underset { \pi } { \min } \mathbb { E } \left [ \sum _ { e \in E } c ( \pi ( x ^ { e } ) ) \right ],$$

where c : Y → R gives the cost of routes y ∈ Y .

Discussion The problem setting defined and formalized above contains various assumptions that might be questioned from a practitioner's perspective. In particular, assuming an unlimited fleet size and full knowledge of the request distribution appears to be rather unrealistic. As this paper focuses on the methodology used to win the EURO Meets NeurIPS Vehicle Routing Competition, we decided to keep these assumptions without further questioning for the sake of consistency and reproducibility. However, we like to emphasize that the methodological pipeline presented in this paper is readily applicable to problem settings with limited fleet sizes and incomplete knowledge of the underlying request distribution as long as some historical data is available.

## 3 ML pipeline with CO-layer

To explain the rationale of our CO-enriched ML pipeline, we recall that a policy π maps a system state x e to a feasible decision y e in Y ( x e ) . The state x e is a set of requests R e , and the solution y e is a set of feasible

Figure 1: Our CO-enriched ML pipeline.

![Image](image_000000_4b3961fc6bac1900c897ba58c8aae9aac6c0364a5cc6b60ca38edda4b06c0fb7.png)

✡

✠

decisions which encode the routes covering a subset of R e . The set of feasible decisions Y ( x e ) hence coincides with the set of feasible solutions of a prize-collecting VRPTW, a variant of the (static) VRPTW where it is not mandatory to serve all requests, but a prize θ e j is collected if request j is served. In this section, we show that any decision y e derived from an optimal policy in a dynamic problem setting corresponds to an optimal solution of a prize-collecting VRPTW for a well chosen prize vector θ e = ( θ e j ) j ∈R e . However, finding prizes θ e j is non-trivial, such that we resort to ML for this purpose. Specifically, we introduce a family of policies ( π w w ) encoded by the CO-enriched ML pipeline illustrated in Figure 1: in the ML-layer, a statistical model ϕ w predicts θ e based on the given system state x e . This yields a prize-collecting VRPTW instance ( x , θ e e ) which we solve in the CO-layer with a dedicated algorithm f . The algorithm's output y e then corresponds to our dispatching and routing decision.

In what follows, we first formally introduce the prize-collecting VRPTW and proof that we can represent every optimal decision in epoch e as a solution of a specially constructed prize-collecting VRPTW instance. We then detail how we design our pipeline to leverage this observation for finding dispatching and routing decisions.

Prize-collecting VRPTW Recall that D e = ( V e , A e ) is a fully-connected digraph with vertex set V e = R ∪{ } e d , comprising epoch requests R e and the depot d , and that a feasible solution of the prize-collecting VRPTW y e ∈ Y ( x e ) can be encoded by the vector ( y e i,j ) ( i,j ) ∈A e where

$$y _ { i, j } ^ { e } = \begin{cases} 1 & \text{if $(i,j)$ is in a route of the solution} \\ 0 & \text{otherwise.} \end{cases}$$

We further consider costs ( c i,j ) ( i,j ) ∈A e on each arc, and prizes ( θ e j ) j ∈R e on each vertex. Then, we can state the objective of the prize-collecting VRPTW as follows, glyph[negationslash]

$$\max _ { y \in \mathcal { Y } ( x ^ { e } ) } & \sum _ { \substack { ( i, j ) \in \mathcal { A } ^ { e } \\ ( i, j \neq d \\ ( i + \text{total profit} ) } } \theta _ { j \neq d } ^ { e } y _ { i, j } - \sum _ { \substack { ( i, j ) \in \mathcal { A } ^ { e } \\ ( i, j \neq d \\ ( i + \text{total routing cost} ) } } c _ { i, j } y _ { i, j } \,.$$

Proposition 3.1. For any x e , there exists a θ ∈ R |R | e such that any optimal solution of (2) is an optimal decision with respect to (1) .

Proof. Since the horizon is finite and the set of feasible decisions at each step is also finite, there exists an optimal decision y glyph[star] for x e . Let ¯ R e be the subset of requests of R e that are dispatched in y glyph[star] . Then any solution y which has lower or equally low routing costs and covers ¯ R e exactly is also optimal. This follows

from the Bellman equation since the routes have no impact on the evolution of the state. We can construct y by solving a prize-collecting VRPTW on R e with request prizes

$$\bar { \theta } _ { j } = \begin{cases} M & \text{if $j\in \bar{ \mathcal{R} }^{e}$} \\ - M & \text{otherwise}, \end{cases}$$

where M = ( |R | · e max ( i,j ) ∈A e c i,j ) is a large constant. The corresponding prize-collecting VRPTW solution y clearly covers ¯ R e exactly and has at most the routing cost of y glyph[star] . glyph[square]

CO-layer We embed the prize-collecting VRPTW as a layer in our CO-enriched ML pipeline. Hence, this CO-layer must support forward and backward passes to assure compatibility with the ML-layer.

The forward pass simply solves the prize-collecting VRPTW instance defined by ( x , θ e e ) using a metaheuristic algorithm f detailed in Section 4. The backward pass backpropagates the gradient of the loss used in the learning algorithm through this layer. Section 5 introduces this loss and its gradient. To make their statement easier, we reformulate (2) as

↦

$$f \colon \theta ^ { e } \mapsto \underset { y \in \mathcal { Y } ^ { e } } { \arg \max } \, \theta ^ { e ^ { \top } } g ( y ) + h ( y ) \, \text{ where } \, \ g ( y ) = \left ( \sum _ { i \in \mathcal { V } ^ { e } } y _ { i, j } \right ) _ { j \in \mathcal { R } ^ { e } } \, \text{ and } h ( y ) = \sum _ { ( i, j ) \in \mathcal { A } ^ { e } } c _ { i, j } y _ { i, j }. \quad ( 4 )$$

ML-layer Finding the optimal prizes θ e of Proposition 3.1 is non-trivial. We therefore use a statistical model ϕ w to predict a vector of prizes θ e = ϕ w ( x e ) ∈ R |R | e given the system state x e . The only technical aspect from the ML perspective is that the dimensions of the input and the output are not fixed. Indeed, the number of requests may change from one state to another, and also differs across instances. We benchmark different choices for commonly used statistical models ϕ w in our computational study (cf. Section 6).

Discussion Alternative ML-based solution approaches presented in the EURO Meets NeurIPS Vehicle Routing Competition (e.g., Doorn et al. 2022) generally proceed in two steps. They first apply a binary classifier, to decide on which requests to dispatch and postpone, respectively. They then construct routes covering these dispatched requests in a second step, essentially decoupling request dispatching from route construction. A major difficulty in this approach is to learn a statistical model which implicitly balances the current route costs and future route costs to find optimal dispatching decisions. We bypass this difficulty by taking dispatching and routing decisions simultaneously in our CO-layer (4), which allows us to train our statistical model based on the routing rather than the dispatching decision.

## 4 Combinatorial optimization algorithm

To derive solutions of the prize-collecting VRPTW within our CO-layer, we propose a metaheuristic algorithm based on hybrid genetic search (HGS). Our algorithm extends the implementation of the HGS algorithm introduced by Kool, Juninck, et al. 2022, which adapts the original HGS of Vidal 2022 to the VRPTW.

Tailoring the work of Kool, Juninck, et al. 2022 to our problem setting requires various modifications to support optional requests, e.g., adaptions of the local search, initialization, and crossover procedures. We further introduce new mutation mechanisms, implement prize-collecting VRPTW specific neighborhoods, and allow to warm-start the population. In what follows, we summarize the core concepts of the HGS algorithm before detailing our prize-collecting VRPTW-specific modifications.

Hybrid Genetic Search for the Vehicle Routing Problem With Time Windows The HGS algorithm is an evolutionary algorithm that maintains a population of solutions, organized in two disjoint sub-populations that contain feasible and infeasible solutions, respectively. The algorithm makes this population evolve over time, generating offspring solutions by combining promising parent solutions selected from

the population in a randomized binary tournament. The algorithm then improves the generated offspring solution in a local search procedure. Note that it uses a penalty-based approach to explore infeasible regions of the solution space. The local search procedure yields a locally optimal solution, which is then added to the population. This may trigger a survivor selection procedure if the population size exceeds a certain threshold. This procedure eliminates a subset of solutions from the population. Survivor and parent selection are based on the fitness of a solution, a metric which captures the quality, i.e., objective value, of a solution and it's contribution to the population's diversity. This ensures a sufficiently diverse population, balancing diversification and intensification in the genetic algorithm. To utilize this algorithmic structure for our problem setting, i.e., the prize-collecting VRPTW, we applied the following adaptions and extensions, leading to the algorithm outlined in Figure 2.

Initialization

Figure 2: General structure of our metaheuristic algorithm. Round nodes indicate populations, square nodes indicate algorithmic components.

![Image](image_000001_756b1a027842e9a9421532b0322ccd05be8c7c2f59749fc01d50c569521957ea.png)

Solution representation We represent decisions on which requests to serve and which to ignore in our solution representation implicitly. Specifically, we use the same giant-tour representation as in Vidal 2022, but allow incomplete giant-tours. Here, requests deemed unprofitable are absent from the giant-tour and thus not considered by traditional local search operators.

Accounting for optional requests during crossover The HGS proposed in Kool, Juninck, et al. 2022 generates offspring solutions using two crossover operators: Ordered Crossover ( OX ) (Oliver, Smith, and Holland 2013) and Selective Route Exchange ( SREX ) (Nagata and Kobayashi 2010). As a first modification, we remove the OX operator as our benchmarking experiments indicate that it has no substantial impact on the algorithm's performance for our problem variant. Our second modification amends the SREX operator to the prize-collecting VRPTW. Specifically, we preserve the set of requests served by the first parent in any generated offspring by re-inserting requests that are currently not served as in regular SREX , i.e., sequentially.

Additional diversification and intensification mechanisms We further introduce two new mutation operators that diversify and intensify solutions based on the set of served requests. The first operator ( random remove/insert ), based on the result of a coin toss, either removes ν · | R σ | of the requests currently served,

or inserts η · | C \ R σ | of the currently unserved requests. This mutation occurs with a probability of ρ right after offspring generation.

The second operator ( optimize\_request\_set ) optimizes the set of requests served by a given solution. Specifically, this operator first removes any requests from the solution that cause a detour whose cost is higher than the request's profit, and re-inserts any profitable requests that are not part of the current solution. The operator perturbs insertion and removal costs with a random factor drawn uniformly from the interval [ ζ -, ζ + ] . In contrast to the first operator, we delay evaluating the second operator until the LS converges to avoid removing an excessive amount of requests due to poor solution quality. We run this operator with probability µ . To intensify our search in regions around promising solutions, we further run this operator (without perturbation) on any solution that improves on the current best solution.

Local Search Table 1 shows the local search operators used in our algorithm. We consider traditional and prize-collecting VRPTW specific operators. Specifically, traditional operators refer to those defined for the VRPTW (see Kool, Juninck, et al. 2022; Vidal 2022). These traditionally explore neighborhoods by exchanging arcs within a solution, i.e., by changing the position of one or several requests in the current solution. Hence, they preserve the set of requests that receive service in a given solution. Prize-collecting VRPTW specific operators on the other hand work with the request set exclusively, that is, they remove or insert a request from a given solution. We evaluate these after evaluating traditional VRPTW operators to avoid removing profitable requests prematurely, i.e., due to bad routing decisions. As in Vidal 2022, we further distinguish between small and large neighborhood operators: small neighborhoods consider only those moves that involve requests which are geographically close and compatible w.r.t. their time windows. Large neighborhoods on the other hand remain unrestricted.

Table 1: Local search operators

| Type                | Name                         | Description                                                                                                                                 |
|---------------------|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Traditional (Small) | relocate                     | removes a single request from it's route and re-inserts it at a different position in the solution                                          |
|                     | relocate pair                | removes a pair of consecutive requests from their route and re-inserts them at a different position in the solution                         |
|                     | relocate reversed pair       | removes a pair of consecutive requests from their route and re-inserts them in reverse order at a different position in the solution        |
|                     | swap                         | exchanges the position of two requests in the solution                                                                                      |
|                     | swap pair                    | exchanges the positions of two pairs of consecutive requests in the solution                                                                |
|                     | swap pair with single        | exchanges the positions of a pair of consecutive requests with a single request in the solution                                             |
|                     | 2-opt                        | reverses a route segment                                                                                                                    |
|                     | 2-opt*                       | splits two routes into two segments each, swapping a segment from the first with a segment from the second route                            |
| Traditional (Large) | relocate*                    | removes a request from it's route and inserts it at the best possible position in any spatially overlapping route                           |
|                     | swap*                        | removes requests a and b from routes r a and r b with spatial overlap, inserting a and b at the best position in r b and r a , respectively |
| PC-VRPTW            | serve request remove request | inserts a currently unserved request into the solution removes a request from the solution                                                  |

Initialization We apply a pre-processing technique to account for extreme weights encountered during the learning procedure. Specifically, we maintain a set of certainly profitable and certainly unprofitable requests based on the maximum and minimum detour required, respectively. We determine certainly unprofitable requests based on the following observation: if min j ∈R ∪{ } e d c jr +min j ∈R ∪{ } e d c rj -θ r ≥ max i,j ∈ R ∪{ } ( e d ) 2 c i,j holds for some request r ∈ R e , then for any route σ that includes r , a cheaper route σ ′ exists. To see this, let u, v ∈ R e ∪ { d } such that ( u, r ) , ( r, v ) ∈ σ . Then c u,r + c r,v -θ r ≥ max i,j ∈ R ∪{ } ( e d ) 2 c i,j ≥ c u,v , such that removing r from σ yields a cheaper route. Analogously, we force the inclusion of a request r ∈ R e if c d,r + c r,d &lt; θ r holds. This procedure further allows to account for must-dispatch customers during the evaluation phase by setting θ r accordingly.

The magnitude of changes in customer prices decreases as the learning procedure converges. We exploit this behaviour in our algorithm and seed the initial population, generated as in Kool, Juninck, et al. 2022, with solutions of previous learning epochs. Specifically, we generate promising solutions in a new construction heuristic which first applies our optimize\_customers operator to the solution to account for changed request prizes, and then optimizes the routing decisions using the local search procedure. Note that this is only possible while training the pipeline. When evaluating the pipeline, we use starting solutions generated as in Kool, Juninck, et al. 2022 exclusively.

Discussion Focusing on our algorithmic design decisions, we note that we have tailored our algorithm to the unique challenges of our learning methodology and training environment. Specifically, successfully computing approximate gradients that ensure convergence during training requires solutions with low variance. Here, the algorithm needs to generalize well to the different prize distributions observed during training. Beyond this, it must be capable of providing such solutions within tight time constraints to allow training within a reasonable amount of time.

We have tackled these challenges with a design that focuses on guiding the population towards promising regions of the search space through aggressive diversification (e.g., randomized insertion and deletion of requests) and intensification ( optimize\_request\_set ) operators, explicit request insertion and deletion neighborhoods, and warmstarting. The resulting algorithm behaves more greedily than implicit approaches (e.g. Vidal, Maculan, et al. 2016), but converges reliably within the tight time constraints imposed during training. We further cope with extreme prizes observed during training in a preprocessing step.

## 5 Learning approach

The objective of our learning problem is to find parameters w such that the statistical model ϕ w predicts a prize vector θ that leads to 'good' decisions in the CO-layer. To reach this objective, we train our CO-enriched ML pipeline to imitate a good policy. To do so, we follow a supervised learning setting and therefore build a dataset D = ( { x , y 1 ¯ ) 1 , . . . , ( x , y n ¯ n ) } of state instances x i with the decisions ¯ y i taken by the imitated policy. In this supervised learning setting, we define a loss L ( θ, y ¯) which quantifies the error when we predict f ( θ ) instead of ¯ y . Then, we formulate the learning problem as finding the parameter ˆ w D that minimizes the empirical risk, i.e., the average loss on the training dataset,

$$\hat { w } _ { \mathcal { D } } = \arg \min _ { w } \sum _ { i = 1 } ^ { n } \mathcal { L } ( \varphi _ { w } ( x ^ { i } ), \bar { y } ^ { i } ).$$

The rest of this section introduces the imitated policy, the training set, the loss, and our algorithm to solve learning problem (5).

## 5.1 Anticipative decisions in the training set

The dynamic VRPTW is difficult because future requests are unknown when taking decisions. If we know the future, i.e., if we knew all the requests at the beginning of the horizon, we would just have to solve the corresponding static VRPTW at the beginning of the horizon, and then take the corresponding dispatching decisions at each epoch. This procedure would give us the optimal anticipative policy. However, such an anticipative policy can of course not be used in practice because it relies on unavailable information. Nonetheless, we can rebuild the decisions taken by the anticipative policy a posteriori, when all information has been revealed, i.e., on historical data. We therefore can learn to imitate an anticipative policy, which we gained from historical data, in our learning problem (5).

Practically, we rebuild the decision of the anticipative policy as follows. We know all the requests of an historical VRPTW instance. Then, we associate a release time to each request i , which is equal to the time τ i at which the request would be revealed in a dynamic setting, so the earliest point in time at which request i can be served. We then seek routes to serve all these requests at minimum cost while respecting the release

times and the time windows. We use the adapted HGS of Kool, Juninck, et al. 2022 to solve this variant of the static VRPTW. This yields a set of routes P from which we reconstruct the decisions taken in each epoch as follows. For route p ∈ P , let τ p = max r ∈ p τ r be the latest release time of any request route p serves. The anticipative policy dispatches route p in the first epoch e where τ e ≥ τ p , i.e., the first epoch where all served requests have been revealed.

## 5.2 Loss function

We recall that the objective of our learning problem is to find parameter values w of a statistical model ϕ w such that for any state x , the CO-layer predicts a good decision y = f ( θ ) for the prize-collecting VRPTW instance ( x, θ = ϕ w ( x )) . More precisely, for each state-decision pair ( x, y ¯) in the data set, we want the target decision ¯ y to be as close as possible to the optimal solution of the prize-collecting VRPTW (2). Hence, it is natural to take the non-optimality of ¯ y as a solution of f ( θ ) as loss function:

$$\mathcal { L } ( \theta, \bar { y } ) = \max _ { y \in \mathcal { Y } ( x ) } \{ \theta ^ { \top } g ( y ) + h ( y ) \} - ( \theta ^ { \top } g ( \bar { y } ) + h ( \bar { y } ) ).$$

We clearly have L ( θ, y ¯) ≥ 0 in general, and L ( θ, y ¯) = 0 only if y is an optimal solution of (4). Unfortunately, the polyhedron P (¯) = y { θ ∈ R | x | | θ glyph[latticetop] g y (¯) + h y (¯) ≥ θ glyph[latticetop] g y ( ) + h y , ( ) ∀ y ∈ Y ( x ) } is generally highly degenerate. For instance, if h = 0 , then θ = 0 belongs to P ( y ) for all y in Y . This implies that θ = 0 would be an optimum of our learning problem, which is a problem because such a θ allows our CO-layer to return any solution in Y ( x ) . This degeneracy can be removed by considering the perturbed loss

$$\mathcal { L } _ { \varepsilon } ( \theta, \bar { y } ) = \mathbb { E } \left [ \max _ { y \in \mathcal { Y } ( x ) } \{ ( \theta + \varepsilon Z ) ^ { \top } g ( y ) + h ( y ) \} \right ] - ( \theta ^ { \top } g ( \bar { y } ) + h ( \bar { y } ) ).$$

where ε &gt; 0 and Z ∼ N (0 , I | x | ) is a Gaussian perturbation. The perturbed loss L ε has been considered in the literature only in the case where g y ( ) = y and h y ( ) = 0 (cf. Berthet et al. 2020). Due to non-zero h , the geometry of the loss changes. Notably, while the size of ε does not matter when h = 0 (cf. Parmentier and T'Kindt 2021), it does matter when h is non-zero: the larger ε , the smaller the impact of h on the prediction. Proposition 5.1 summarizes the geometry of these losses in our more general context.

Proposition 5.1. Let x ∈ X , ¯ y ∈ Y ( x ) . Let C (¯) = y { θ ∈ R | x | : θ glyph[latticetop] g y (¯) ≥ θ glyph[latticetop] g y , ( ) ∀ y ∈ Y ( x ) } be the normal cone associated to g y (¯) .

↦

1. θ →L ( θ, y ¯) is piecewise linear and convex, with subgradient

$$\underbrace { g \left ( \underset { y \in \mathcal { Y } ( x ) } { \arg \max } \theta ^ { \top } g ( y ) + h ( y ) \right ) } _ { = g ( f ( \theta ) ) } - g ( \bar { y } ) \in \partial _ { \theta } \mathcal { L } ( \theta, \bar { y } ).$$

↦

2. θ →L ε ( θ, y ¯) is C ∞ and convex with gradient

$$\nabla _ { \theta } \mathcal { L } _ { \varepsilon } ( \theta, \bar { y } ) = \mathbb { E } \left [ g \left ( \underset { y \in \mathcal { Y } ( x ) } { \arg \max } ( \theta + \varepsilon Z ) ^ { \top } g ( y ) + h ( y ) \right ) \right ] - g ( \bar { y } ) = \mathbb { E } [ g ( f ( \theta + \varepsilon Z ) ) ] - g ( \bar { y } ).$$

- 3. L ε ( θ, y ¯) ≥ L ( θ, y ¯) .
- 4. C (¯) y is the recession cone of P (¯) y .

↦

glyph[negationslash]

↦

- 5. Let θ ∈ R | x | . If η is in C (¯) y \{ 0 } , then λ →L ( θ + λη, y ¯) is non increasing. If in addition C (¯) = y R | x | , then λ →L ε ( θ + λη, y ¯) is decreasing.
- 6. Let θ ∈ R | x | . If η is in the interior ˚ (¯) C y of C (¯) y , then lim λ →∞ L ( θ + λη, y ¯) = lim λ →∞ L ε ( θ + λη, y ¯) = 0 .

↦

- Proof. 1. θ →L ( θ, y ¯) is linear and convex as it is the maximum of mappings that are linear in θ . Let ( θ, θ ˜ ) ∈ R | x | × R | x | . Let y ∗ be in argmax y ∈Y ( x ) θ glyph[latticetop] g y ( )+ h y ( ) and ˜ y in argmax y ∈Y ( x ) ˜ θ glyph[latticetop] g y ( )+ h y ( ) . By definition of ˜ y , we have ˜ θ glyph[latticetop] g y (˜) + h y (˜) ≥ ˜ θ glyph[latticetop] g y ( ∗ ) + h y ( ∗ ) , which gives L ( ˜ θ, y ¯) ≥ L ( θ ∗ , y ¯) + ( ˜ θ -θ ∗ ) glyph[latticetop] ( g y ( ∗ ) -g y (¯) ) and the subgradient in ∂ θ L ( θ, y ¯) .

↦

↦

- 2. Since L ε ( θ, y ) = E [ L ( θ + εZ, y )] , we obtain that θ → L ε ( θ, y ) is C ∞ as a convolution product of θ → L ( θ, y ¯) with a Gaussian density (which is C ∞ ), and is convex as an expectation of a convex function. The gradient follows from the subgradient of L ( θ, y ) .
- 3. Let θ ∈ R | x | . For all ˜ y in Y ( x ) , we have

$$\text{for all } z \in \mathbb { R } ^ { | x | }, \quad \underset { y } { \max } ( \theta + \varepsilon z ) ^ { \top } g ( y ) + h ( y ) \geq ( \theta + \varepsilon Z ) ^ { \top } g ( \tilde { y } ) + h ( \tilde { y } )$$

$$\text{hence}, \ \mathbb { E } [ \max _ { y } ( \theta + \varepsilon Z ) ^ { \top } g ( y ) + h ( y ) ] \geq \theta ^ { \top } g ( \tilde { y } ) + h ( \tilde { y } ) \ \ ( \since { \mathbb { E } } [ Z ] = 0 )$$

and , E [max( y θ + εZ ) glyph[latticetop] g y ( ) + h y ( )] -( θ glyph[latticetop] g y (¯) + h y (¯)) ≥ θ glyph[latticetop] g y (˜) + h y (˜) -( θ glyph[latticetop] g y (¯) + h y (¯)) .

Finally: E [max ( y θ + εZ ) glyph[latticetop] g y ( ) + h y ( )] -( θ glyph[latticetop] g y (¯) + h y (¯)) ≥ max y { θ glyph[latticetop] g y ( ) + h y ( ) } -( θ glyph[latticetop] g y (¯) + h y (¯)) , i.e. L ε ( θ, y ¯) ≥ L ( θ, y ¯) .

- 4. Let θ ∈ P (¯) y . We have ∀ y, ( θ + λη ) glyph[latticetop] g y (¯) + h y (¯) ≥ ( θ + λη ) glyph[latticetop] g y ( ) + h y ( ) ⇔ ∀ y, θ T g y (¯) + h y (¯) + λη glyph[latticetop] ( g y (¯) -g y ( ) ) ≥ θ glyph[latticetop] g y ( ) + h y ( ) . If η is in C (¯) y , then η glyph[latticetop] ( g y (¯) -g y ( ) ) ≥ 0 , and θ + λη is in P (¯) y for any λ &gt; 0 . If η is not in C (¯) y , then η glyph[latticetop] ( g y (¯) -g y ( ) ) &lt; 0 and there exists a λ &gt; 0 such that θ + λη is not in P (¯) y .
- 5. Let η ∈ C (¯) y . Let us denote by ˆ = argmax ( y y θ + λη ) glyph[latticetop] g y ( ) + h y ( ) . We have

$$\text{for all } \lambda, \quad \mathcal { L } ( \theta + \lambda \eta, \bar { y } ) = ( \theta + \lambda \eta ) ^ { \top } g ( \hat { y } ) + h ( \hat { y } ) - \left [ ( \theta + \lambda \eta ) ^ { \top } g ( \bar { y } ) + h ( \bar { y } ) \right ]$$

↦

By taking the derivative with respect to λ , we obtain: η glyph[latticetop] ( g y (ˆ) -g y (¯)) which is negative by definition of η . This gives us the non increasing-property of λ →L ( θ + λη, y ¯) .

↦

glyph[negationslash]

glyph[negationslash]

Similarly, by denoting ˆ ( y ε Z ) = argmax ( y θ + λη + εZ ) glyph[latticetop] g y ( ) + h y ( ) for all Z , we obtain the derivative of λ →L ε ( θ + λη ) : η glyph[latticetop] ( E [ g y (ˆ ( ε Z ))] -g y (¯)) . If C (¯) = y R | x | , then P ( θ + λη + εZ / ∈ C (¯)) y &gt; 0 . Hence, E [ g y (ˆ ( ε Z )] = g y (¯) , η glyph[latticetop] ( E [ g y (ˆ ( ε Z )] -g y (¯)) &lt; 0 , and therefore λ →L ε ( θ + λη ) is decreasing.

↦

glyph[negationslash]

glyph[negationslash]

- 6. Let η ∈ C ˚ (¯) y . By definition of ˚ (¯) C y , we have η glyph[latticetop] (¯ y -y ) &gt; 0 for all y = ¯ y . Hence, there exists M &gt; 0 such that for all λ ≥ M and y = ¯ y , we have ( θ + λη ) glyph[latticetop] ¯+ y ≥ ( θ + λη ) glyph[latticetop] y . That is, for all λ ≥ M we have θ + λη ∈ P (¯) y , i.e. L ( θ + λη, y ¯) = 0 . Hence lim λ →∞ L ( θ + λη, y ¯) = 0 .

glyph[negationslash]

↦

.

If C (¯) = y R | x | , from point 5 and the previous limit, we get that λ →L ( θ + εz + λη, y ¯) monotonically decreases to zero for any z . The monotone convergence theorem therefore gives lim λ →∞ L ε ( θ + λη, y ¯) = 0

If C (¯) = y R | x | , the result is immediate.

↦

The perturbed loss L ε has properties that make it very suitable for a learning problem. Indeed, it is convex and smooth, and while the expectation in its gradient is intractable, sampling Z gives a stochastic gradient. The learning problem (5) with loss L ε can therefore be solved using stochastic gradient descent. Furthermore, θ →L ε ( θ, y ¯) tends to 0 only if θ is far in the interior of C (¯) y , which means that there is no ambiguity on the fact that ¯ y is an optimal solution of (4) for θ , and removes the degeneracy issue of θ →L ( θ, y ¯) . This situation is illustrated in two dimensions in Figure 3. Figure 3a represents the polytope corresponding to the convex envelope conv( g ( Y ))) = conv( { g y , ( ) ∀ y ∈ Y} ) . The algorithm f necessarily outputs a vertex of this polytope (red square) as the objective function of the CO-layer is linear in g y ( ) . The dark blue hexagon is the

↦

output E [ g f ( ( θ + εZ ))] of the perturbed CO-layer, which can be seen as the expectation over a distribution (blue circles) on the vertices of the polytope. On Figure 3b, with h = 0 and no perturbation, we can see that the loss L ( θ, y ¯) is 0 for any θ in the normal cone of ¯ y (with g y (¯) being the red square), and that the origin belongs to the normal cone of any vertex y . On Figure 3c, the perturbation has been added and we can see that L ( θ, y ¯) goes to zero only when we are safely inside the cone, which means that the output of the combinatorial optimization layer is non-ambiguous. Finally, on Figure 3d and 3e, we can see that a non-zero h changes the geometry for small θ values but not for large θ values.

↦

↦

↦

![Image](image_000002_59c78ee632be22e15864d48c3f4114ab546bf0d1e18efdc1f461caff5575d038.png)

glyph[negationslash]

↦

glyph[negationslash]

Figure 3: Example with two-dimensional θ and its polytope 3a. Contour plots of the loss L and its perturbed version L for two different values of h . Thick lines on 3b and 3c represent the normal fan associated to the polytope. We can see the perturbed regularization 'pushing' the loss inside the normal cone C (¯) y .

Non-optimal CO-layer Note that our CO-layer f is a (meta)-heuristic algorithm and thus does not guarantee an optimal solution, which is not in line with the assumptions of the theory reviewed in this section. However, we observe that this is not a problem in practice as f outputs solutions close enough to optimal ones.

## 6 Computational study

The aim of our computational study is twofold. First, we validate the performance of our CO-enriched ML pipeline in a benchmark against several state-of-the-art approaches. Second, we conduct extensive numerical experiments to assess the impact of different training settings on the performance of our CO-enriched ML pipeline. Specifically, we investigate the impact of i) the feature set, ii) the size of instances in the training set, iii) the training set size, iv) the imitated target strategy, and v) the type of statistical model used.

For this purpose, we first detail the design of our computational study in Section 6.1, and the different benchmarks in Section 6.2. We then present the results of our benchmark study in Section 6.3, and discuss the results of our experiments on different training settings in Section 6.4.

## 6.1 Design of experiments

We design our computational study in line with the problem setting presented in the EURO Meets NeurIPS Vehicle Routing Competition (Kool, Bliek, et al. 2022). We consider a set of VRPTW instances derived from real-world data of a US-based grocery delivery service. We refer to these instances as static instances. A static instance contains a set of requests, each with a specific location, a service time, a demand, and a time window. Hence, each instance implicitly defines a distribution of request locations, service times, demands, and time windows. We generate an instance of the dynamic problem from a static instance as follows: we first discretize the planning horizon of the static instance into one hour epochs. Then, for each epoch e , we sample m requests randomly, constructed by drawing from the static instance's location, demand, service time, and time window distributions independently. We refer to m as the sample size in the remainder of this section. We discard requests that are infeasible with respect to the starting time of the current epoch and the chosen time window. The remaining requests form the set of requests revealed in epoch e . This sampling approach has two implications: First, the expected number of requests arriving in each epoch decreases with advancing epochs as the probability of sampling a feasible time window decreases. Second, time windows that end closer to the end of the planning horizon are more likely to appear, as these time windows have a higher probability to be feasible. In line with the challenge's problem setting, we minimize travel time only, such that the number of vehicles, service times, and waiting times are not part of the objective. Furthermore, we do not limit the number of vehicles and assume knowledge over the static instance's request distribution.

Finally, we note that we can generate several dynamic instances from one static instance by varying the seed used to sample from the request distribution. In what follows, we refer to this seed as the instance seed .

## 6.2 Benchmark policies

We evaluate the performance of our CO-enriched ML pipeline against a set of benchmark policies. These policies follow a two-stage approach by first deciding on the set of requests to dispatch and then form covering routes in a second step by solving a VRPTW on the dispatched requests using the HGS algorithm (cf. Kool, Juninck, et al. 2022; Vidal 2022). For each benchmark policy, we allow a time limit for finding decisions of 90 seconds per epoch, if not stated differently. In what follows, we briefly introduce the benchmark policies considered in this computational study.

Greedy policy The greedy policy dispatches all requests as soon as they enter the system.

Lazy policy The greedy policy dispatches all requests as soon as they enter the system.

Random policy The random policy dispatches postponable requests with a probability of 50%. It further dispatches all must-dispatch requests.

Rolling-horizon policy The rolling-horizon policy samples a scenario for the remaining epochs, and applies the HGS in a similar way as the anticipative strategy assuming that the sampled scenario represents the true scenario. This yields a set of routes based on which it decides which requests to dispatch. Specifically, it dispatches a request of the current epoch if and only if that request shares a route with a must dispatch request of the current epoch. We assign a time limit of 600 seconds to ensure convergence of the HGS since this policy entails solving a VRPTW on a complete scenario. Note that this extends the 90 second time

Monte-carlo policy The monte-carlo policy samples nine scenarios for the remaining epochs, solved individually as in the rolling-horizon policy. It dispatches a request based on a majority decision, i.e., if and only if the rolling-horizon policy dispatches the request in at least 50% of the sampled scenarios. We raise the time-limit accordingly, i.e., allow a total of 5400 seconds.

ML-CO policy The ML-CO policy is encoded in the CO-enriched ML pipeline introduced in Section 3. Unless specified otherwise, we train our ML-CO policy on a set of 15 training instances using a sample size of 50 requests per epoch, derived using the anticipative strategy as detailed in Section 5.1. We limit the runtime of the HGS used to derive the anticipative solutions to 3600 seconds. Our ML-layer uses a feedforward neural network with four hidden layers, each comprising ten neurons. For the ML-CO policy, we set the time limit for finding decisions to 90 seconds per epoch. Table 2 indicates that this time limit does not impact the performance of our ML-CO policy significantly.

Table 2: Performance of our ML-CO policy for different time limits that we allow for the PC-HGS during evaluation.

|                               | Decision time limit during evaluation [seconds]   | Decision time limit during evaluation [seconds]   | Decision time limit during evaluation [seconds]   | Decision time limit during evaluation [seconds]   | Decision time limit during evaluation [seconds]   | Decision time limit during evaluation [seconds]   |
|-------------------------------|---------------------------------------------------|---------------------------------------------------|---------------------------------------------------|---------------------------------------------------|---------------------------------------------------|---------------------------------------------------|
|                               | 30                                                | 60                                                | 90                                                | 120                                               | 180                                               | 240                                               |
| Relative distance to ant. b.: | 5.66%                                             | 5.18%                                             | 5.15%                                             | 5.01%                                             | 4.89%                                             | 4.87%                                             |

We note that the greedy , lazy , and random policies are the baseline heuristics used in the EURO Meets NeurIPS Vehicle Routing Competition and refer the interested reader to Kool, Bliek, et al. 2022 for more details.

We additionally include the anticipative strategy used during training (cf. Section 5.1) as a baseline. To derive the anticipative baseline, we solve the offline VRPTW using the HGS algorithm proposed in Kool, Juninck, et al. 2022 with a time limit of 3600 seconds. Note that the HGS algorithm is a heuristic which implies that it is possible to find a solution which outperforms the anticipative baseline.

We evaluate the performance of the benchmark policies on 25 dynamic test instances and report for each instance the average result over 20 different instance seeds. To create a dynamic test instance we consider a sample size of 100 requests per epoch.

## 6.3 Performance Analysis

Figure 4a compares the performances of the introduced benchmarks. The figure shows the gap of the policy's objective value relative to the anticipative baseline (ant. b.). In general, we can see that benchmark policies which consider information about the uncertain appearance of future requests, i.e., rolling-horizon, monte-carlo, ML-CO, and the anticipative baseline, outperform benchmarks which make dispatching decision based on the current epoch only, i.e., lazy, random, and greedy. Specifically, these perform on average 74.05%, 36.29%, and 20.99% worse than the anticipative baseline. The rolling-horizon policy performs 7.12% worse than the anticipative baseline. We see that considering the future impact of our dispatching decision based on only a single scenario already improves the performance in comparison to the greedy policy significantly. This

![Image](image_000003_6d6fc85ec050d877638923879f544d4b4fc33213f3aa399dcd5bad3141253c6f.png)

(a) Performance relative to anticipative baseline solution.

![Image](image_000004_dfe28fa868b0bc4668ad65a02fc9cfe4f2bd9b408a5ea7e835249b5221390dc8.png)

baseline

- (b) Variance of objective value with respect to instance seeds.

Figure 4: Performance of benchmark policies.

is surprising as the scenario drawn is often only a poor representation of the requests actually observed in later epochs due to the size of the sample space. However, the distance between the drawn scenario and the correct scenario might be outweighed by the benefit of combining future requests with actual requests in low-cost routes. The monte-carlo policy outperforms the rolling-horizon policy and only has a gap of 6.97% to the anticipative baseline. This shows that a better approximation of future uncertainties, achieved through a higher number of sampled scenarios, leads to better dispatching decisions, thus improving performance. Yet, the improvement with respect to the rolling-horizon benchmark is rather small in comparison to the improvement from the greedy policy to the rolling-horizon policy. Our ML-CO policy outperforms all other online policies and is only 5.15% worse than the anticipative baseline. This is surprising as the general consensus in literature on multi-stage stochastic problems indicates the contrary, i.e., that imitating an anticipative strategy does not generalize well.

Result 1. Our ML-CO policy performs best across all online benchmark policies and outperforms the montecarlo policy by 1.57%. This indicates that learning a policy by imitating an anticipative strategy yields good performances in this problem setting.

Figure 4b shows the variance of the objective value over several instance seeds for the considered benchmark policies. Comparing Figures 4a and 4b shows a clear trend: policies with low variance outperform policies with high variance. This trend results from finding more robust solutions that generalize well over uncertain future observations. Note, that, although performing worse, the monte-carlo policy has less variance than our ML-CO policy. This relates to the sampling approach the monte-carlo policy bases on, which yields a robust solution that performs well over all sampled scenarios and therefore focuses too much on variance minimization. Our ML-CO policy on the other hand balances the trade-off between robustness and solution quality.

Result 2. The monte-carlo policy and our ML-CO policy generalize well over uncertain future observations.

The learning component in the ML-CO policy balances the trade-off between robustness and solution quality, leading to a superior performance.

## 6.4 Extended analysis

This analysis assesses the impact of different training settings on the performance of our ML-CO policy. Specifically, we investigate the impact of i) the feature set, ii) the size of each training instance, iii) the training set size, iv) the imitated target strategy, and v) the type of statistical model used. We evaluate each model's performance as detailed in Section 6.2 and report the relative gap to the anticipative baseline.

Table 3: Performance of ML-CO policy using different feature sets.

|                               | complete (baseline)   | Feature sets model-aware   | model-free   |
|-------------------------------|-----------------------|----------------------------|--------------|
| Relative distance to ant. b.: | 5.15%                 | 13.83%                     | 6.78%        |

Different feature sets Table 3 compares the performance of our ML-CO policy on three feature sets (i.e., complete , model-aware , and model-free ). Table 4 details the features each set comprises. Specifically, the model-free feature set contains features computable from the current state x e , while the model-aware feature set only contains features which include distributional information from the static instance. We further include the complete feature set which combines the features from the model-aware and model-free feature sets. The complete feature set was used in the model submitted to the challenge, i.e., our baseline. Our results show that the performance of our ML-CO policy does not rely solely on model-aware information not available in real-world scenarios. Specifically, considering model-aware features derived from the static instance decreases the gap of our ML-CO policy to the anticipative baseline by only 1 63 . percentage points on average.

Result 3. Our ML-CO policy performs well without considering model-aware features derived from the static instance.

| model-free                                                                                                                                                                                                                                   | model-free                                                                                                          | model-aware                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x coordinate of location y coordinate of location demand service time time window start time window end time from depot to request relative time depot to request time window start / rem. time time window end / rem. time is must dispatch | x r y r q r s r l r u r t d,r t d,r / ( u r - s r ) l r / ( T max - τ e ) u r / ( T max - τ e ✶ τ e +∆ τ + t d,r >u | Quantiles from distribution of travel time to all locations: 1% quantile Pr [ X < x ] ≤ 0 . 01 , X ∼ t r, : 5% quantile Pr [ X < x ] ≤ 0 . 05 , X ∼ t r, : 10% quantile Pr [ X < x ] ≤ 0 . 1 , X ∼ t r, : 50% quantile Pr [ X < x ] ≤ 0 . 5 , X ∼ t r, : Quantiles from distribution of slack time to all time windows: 0% quantile Pr [ X < x ] ≤ 0 , X ∼ u : - ( l r + s r + t r, : ) 1% quantile Pr [ X < x ] ≤ 0 . 01 , X ∼ u : - ( l r + s r + t r, : ) 5% quantile Pr [ X < x ] ≤ 0 . 05 , X ∼ u : - ( l r + s r + t r, : ) 10% quantile Pr [ X < x ] ≤ 0 . 1 , X ∼ u : - ( l r + s r + t r, : ) 50% quantile Pr [ X < x ] ≤ 0 . 5 , X ∼ u : - ( l r + s r + t r, : ) |

Table 4: Different feature sets.

Different sample size of training instances Table 5 shows the performance of the ML-CO policy relative to the performance of the anticipative baseline when training the ML-CO policy on training instances derived from different sample sizes. Note that we evaluate the trained models on instances generated with a sample size of 100 regardless of the sample size used during training. Our results, indicate that there exists a trade-off between different sample sizes. Specifically, our pipeline performs best when training on instances with a sample size of 50, reaching an average gap of 5 15% . to the anticipative baseline. Increasing

Table 5: Performance of ML-CO policy when trained on different sized training instances.

|                               | Sample size of training instances   | Sample size of training instances   | Sample size of training instances   | Sample size of training instances   | Sample size of training instances   |
|-------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
|                               | 10                                  | 25                                  | 50                                  | 75                                  | 100                                 |
| Relative distance to ant. b.: | 8.71%                               | 6.31%                               | 5.15%                               | 7.41%                               | 13.08%                              |

or decreasing the sample size reduces the performance. We attribute this to the bias-variance trade-off: small instances allow to train a statistical model that is highly accurate on the training set but fails to generalize to the larger instances used during evaluation. Training on large instances on the other hand fails to extract enough structural information to yield an accurate statistical model.

Result 4. It is crucial to balance learning accuracy and model generalization when training the ML-CO policy.

|                               | Num. of training instances   | Num. of training instances   | Num. of training instances   | Num. of training instances   | Num. of training instances   | Num. of training instances   | Num. of training instances   | Num. of training instances   |
|-------------------------------|------------------------------|------------------------------|------------------------------|------------------------------|------------------------------|------------------------------|------------------------------|------------------------------|
|                               | 1                            | 2                            | 5                            | 10                           | 15                           | 20                           | 25                           | 30                           |
| Relative distance to ant. b.: | 10.19%                       | 7.77%                        | 7.26%                        | 5.89%                        | 5.15%                        | 5.23%                        | 4.50%                        | 4.94%                        |

Table 6: Performance of ML-CO policy when training on different numbers of training instances.

Different numbers of training instances Table 6 shows the performance of the ML-CO policy relative to the performance of the anticipative baseline when training the ML-CO policy on different training set sizes. Our results convey that the performance of our ML-CO policy improves with the training set size. However, the marginal improvement decreases with the training set size. Specifically, performance saturates at a training set size of 10 instances, which shows that our ML-CO policy learns to generalize well even from small training sets. This indicates that our approach requires only a few instances to extract most of the structural information contained in the problem setting, which is a stark contrast to findings from classic supervised learning, which generally requires significantly larger training sets.

Result 5. The ML-CO policy only needs few training instances to learn a general policy.

|                                     | Imitated anticipative strategies   | Imitated anticipative strategies   | Imitated anticipative strategies   | Imitated anticipative strategies   | Imitated anticipative strategies   | Imitated anticipative strategies   | Imitated anticipative strategies   | Imitated anticipative strategies   |
|-------------------------------------|------------------------------------|------------------------------------|------------------------------------|------------------------------------|------------------------------------|------------------------------------|------------------------------------|------------------------------------|
|                                     | best seed                          | 3600 sec                           | 900 sec                            | 300 sec                            | 240 sec                            | 180 sec                            | 120 sec                            | 60 sec                             |
| Relative distance to ant. b.:       | 6.18%                              | 5.15%                              | 4.79%                              | 7.61%                              | 6.01%                              | 5.25%                              | 5.33%                              | 5.81%                              |
| Average objective value [ × 10000]: | 20.49                              | 20.56                              | 20.70                              | 20.78                              | 20.80                              | 20.87                              | 20.97                              | 21.21                              |

Table 7: Performance of ML-CO policy for different target strategies.

Varying target strategies To investigate the impact of the quality of the solutions imitated by our ML-CO policy, we vary the time limit allocated to the HGS used to derive the underlying anticipative solutions. Here the intuition is as follows: solutions derived with a low time limit should be of lower quality than solutions derived with a high time limit. As the HGS algorithm is subject to random decisions, we further include a training set derived from the best solutions found across 10 runs with a time limit of 3600 seconds each. Table 7 shows the performance of the ML-CO policy relative to the performance of the anticipative baseline when training the ML-CO policy on different target solutions with different solution qualities. Surprisingly, there is no clear trend between the solution quality of the respective training set and the performance of the ML-CO policy. This leads to the assumption that an improved quality of the anticipative target solution does not increase the performance of the trained ML-CO policy and therefore the anticipative strategy might not be the best policy to imitate.

Result 6. The anticipative strategy might not be the best policy to imitate and there might exist a target solution which yields better performances.

Figure 5: Performance of ML-CO policy using different statistical models.

![Image](image_000005_696fecb386d7af6c8ad1cb709359ad76e5c7b653b8e40e0964cdd7064352d247.png)

↦

Different statistical models Figure 5 compares the performance of our ML-CO policy when using different statistical models ϕ w . All statistical models rely on a feature mapping φ that maps a state x e and a request i of this state to a feature vector φ i, x ( e ) in R | φ | . Our linear model ϕ w ( x e ) = ( w φ i, x glyph[latticetop] ( )) i manages to handle variable size inputs and outputs by applying the same linear model φ → w φ glyph[latticetop] independently to each dimension i . Similarly, our neural network ϕ w ( x e ) = ( g w ( φ i, x ( )) ) i applies an auxiliary neural network g w independently for each dimension i . Finally, we propose two graph neural networks . Both graph neural networks consider requests as nodes and the connection between requests as edges. We include a regular and a sparsified graph neural network, the latter contains only those edges which are feasible with respect to request time windows and travel times. Both graph neural networks receive an input vector φ i, x ( ) for each node i , such that ϕ w ( x e ) = ( h w ( φ i, x ( ) i ∈ I ) ) .

The linear model performs worst with an average gap of 6 85 . % to the anticipative baseline while the sparsified graph neural network performs best with a 5 04 . % gap to the anticipative baseline. Comparing the performance of the linear model to the neural networks' performance indicates the importance of a feature generator. In fact, using a simple neural network already lowers the gap to the anticipative baseline to 5 15 . %. As expected, using a graph neural network that calculates structural features further improves the performance of the ML-CO policy. However, the improvement is rather small in comparison to the performance of the neural network. This suggests that most of the structural information is already included in the structured learning approach.

Result 7. Feature-generating statistical models yield the best performing ML-CO policies.

## 7 Conclusion

We presented a novel CO-enriched ML pipeline for a dynamic VRP that was introduced in the EURO Meets NeurIPS Vehicle Routing Competition. Specifically, our work contains several methodological contributions and an extensive computational study. From a methodological perspective, we extend ML-based pipelines to objective functions where the dimension of the predicted objective costs does not match the dimension of the decision variables. These objective functions amend to other problem settings such that we have made them available in the open source library InferOpt.jl . Moreover, we presented the first pipeline that utilizes a metaheuristic component to solve the CO-layer and showed how to carefully design a metaheuristic that finds heuristic solutions which allow to compute approximate gradients. We showed how to train the ML-layer of this pipeline in a supervised learning fashion, i.e., based on a training set derived from an anticipative strategy. We presented a comprehensive numerical study and show that our policy encoded via the CO-enriched ML pipeline outperforms greedy policies by 13.18% and even monte-carlo policies, which were granted a longer runtime, by 1.57% in terms of travel time on average. Interestingly, our results point at the fact that counterintuitive to common practice, imitating anticipative strategies can work well for high-dimensional multi-stage stochastic optimization problems, even if the anticipative strategy might not be the best strategy to imitate.

## Acknowledgments

We thank Wouter Kool and the organizational committee of the EURO Meets NeurIPS 2022 Vehicle Routing Competition for providing the problem setting studied in this paper.

## References

- Alinaghian, M., M. Aghaie, and M. S. Sabbagh (2019). 'A mathematical model for location of temporary relief centers and dynamic routing of aerial rescue vehicles'. In: Computers &amp; Industrial Engineering 131, pp. 227-241 (cit. on p. 2).
- Basso, R., B. Kulcsár, I. Sanchez-Diaz, and X. Qu (2022). 'Dynamic stochastic electric vehicle routing with safe reinforcement learning'. In: Transportation Research Part E: Logistics and Transportation Review 157, p. 102496 (cit. on p. 2).
- Bengio, Y., A. Lodi, and A. Prouvost (2021). 'Machine learning for combinatorial optimization: A methodological tour d'horizon'. en. In: European Journal of Operational Research 290.2, pp. 405-421 (cit. on p. 2).
- Bent, R. W. and P. Van Hentenryck (2004). 'Scenario-Based Planning for Partially Dynamic Vehicle Routing with Stochastic Customers'. In: Operations Research 52.6, pp. 977-987 (cit. on p. 3).
- Berthet, Q., M. Blondel, O. Teboul, M. Cuturi, J.-P. Vert, and F. Bach (2020). 'Learning with differentiable pertubed optimizers'. In: Advances in neural information processing systems 33, pp. 9508-9519 (cit. on pp. 3, 11).
- Blondel, M., A. F. Martins, and V. Niculae (2020). 'Learning with fenchel-young losses'. In: The Journal of Machine Learning Research 21.1, pp. 1314-1382 (cit. on p. 3).
- Carpentier, P., J.-P. Chancelier, G. Cohen, and M. De Lara (2015). 'Stochastic multi-stage optimization'. In: Probability Theory and Stochastic Modelling 75 (cit. on p. 2).
- Dalle, G., L. Baty, L. Bouvier, and A. Parmentier (2022). Learning with Combinatorial Optimization Layers: a Probabilistic Approach . arXiv:2207.13513 (cit. on p. 3).
- Doorn, J. van, L. Lan, L. Pentinga, and N. Wouda (2022). 'Solving a static and dynamic VRP with time windows using hybrid genetic search and simulation'. In: EURO Meets NeurIPS Vehicle Routing Competition . url : https://github.com/ortec/euro-neurips-vrp-2022-quickstart/blob/main/ papers/OptiML.pdf (cit. on p. 7).
- Fikar, C. (2018). 'A decision support system to investigate food losses in e-grocery deliveries'. In: Computers &amp; Industrial Engineering 117, pp. 282-290 (cit. on p. 2).
- Flatberg, T., G. Hasle, O. Kloster, E. J. Nilssen, and A. Riise (2007). 'Dynamic And Stochastic Vehicle Routing In Practice'. In: Dynamic Fleet Management: Concepts, Systems, Algorithms &amp; Case Studies . Ed. by V. Zeimpekis, C. D. Tarantilis, G. M. Giaglis, and I. Minis. Boston, MA: Springer US, pp. 41-63 (cit. on p. 3).
- Gendreau, M., F. Guertin, J.-Y. Potvin, and É. Taillard (1999). 'Parallel Tabu Search for Real-Time Vehicle Routing and Dispatching'. In: Transportation Science 33.4, pp. 381-390 (cit. on p. 3).
- Hildebrandt, F. D., B. W. Thomas, and M. W. Ulmer (2023). 'Opportunities for reinforcement learning in stochastic dynamic vehicle routing'. In: Computers &amp; Operations Research 150, p. 106071 (cit. on pp. 2, 3).
- Joe, W. and H. C. Lau (2020). 'Deep Reinforcement Learning Approach to Solve Dynamic Vehicle Routing Problem with Stochastic Customers'. In: Proceedings of the International Conference on Automated Planning and Scheduling 30.1, pp. 394-402 (cit. on p. 3).
- Jungel, K., A. Parmentier, M. Schiffer, and T. Vidal (2023). Learning-based Online Optimization for Autonomous Mobility-on-Demand Fleet Control . arXiv:2302.03963 (cit. on pp. 2, 3).
- Kool, W., L. Bliek, D. Numeroso, R. Reijnen, R. R. Afshar, Y. Zhang, T. Catshoek, K. Tierney, E. Uchoa, T. Vidal, and J. Gromicho (2022). The EURO Meets NeurIPS 2022 Vehicle Routing Competition (cit. on pp. 2, 4, 14, 15).

- Kool, W., H. van Hoof, and M. Welling (2019). 'Attention, Learn to Solve Routing Problems!' In: International Conference on Learning Representations (cit. on p. 3).
- Kool, W., J. O. Juninck, E. Roos, K. Cornelissen, P. Agterberg, J. van Hoorn, and T. Visser (2022). Hybrid Genetic Search for the Vehicle Routing Problem with Time Windows: a High-Performance Implementation (cit. on pp. 7-11, 14, 15).
- Kotary, J., F. Fioretto, P. Van Hentenryck, and B. Wilder (2021). End-to-End Constrained Optimization Learning: A Survey . arXiv:2103.16378 (cit. on p. 2).
- Maida, J. (2022). Same-day delivery market in the US: 18.50 Y-o-y growth rate in 2021: Market size from 2020 to 2025 . url : %7Bhttps://www.prnewswire.com/news-releases/same-day-deliverymarket-in-the-us-18-50-y-o-y-growth-rate-in-2021--market-size-from-2020-to-2025-301510886.html%7D (cit. on p. 2).
- Nagata, Y. and S. Kobayashi (2010). 'A Memetic Algorithm for the Pickup and Delivery Problem with Time Windows Using Selective Route Exchange Crossover'. In: Parallel Problem Solving from Nature, PPSN XI . Springer Berlin Heidelberg, pp. 536-545 (cit. on p. 8).
- Nazari, M., A. Oroojlooy, L. Snyder, and M. Takac (2018). 'Reinforcement Learning for Solving the Vehicle Routing Problem'. In: Advances in Neural Information Processing Systems . Ed. by S. Bengio, H. Wallach, H. Larochelle, K. Grauman, N. Cesa-Bianchi, and R. Garnett. Vol. 31. Curran Associates, Inc. (cit. on pp. 2, 3).
- Nicolai, B. (2016). Amazon liefert Pakete innerhalb von zwei Stunden . url : https://www.welt.de/ wirtschaft/article153690106/Amazon-liefert-Pakete-innerhalb-von-zwei-Stunden.html (cit. on p. 2).
- Ojeda Rios, B. H., E. C. Xavier, F. K. Miyazawa, P. Amorim, E. Curcio, and M. J. Santos (2021). 'Recent dynamic vehicle routing problems: A survey'. In: Computers &amp; Industrial Engineering 160, p. 107604 (cit. on pp. 2, 3).
- Oliver, I., D. Smith, and J. R. Holland (2013). 'A study of permutation crossover operators on the traveling salesman problem'. In: Genetic Algorithms and their Applications . Psychology Press, pp. 224-230 (cit. on p. 8).
- Parmentier, A. (2021). Learning structured approximations of operations research problems . arXiv:2107.04323 (cit. on p. 3).
- - (2022). 'Learning to Approximate Industrial Problems by Operations Research Classic Problems'. In: Operations Research 70.1, pp. 606-623 (cit. on p. 3).
- Parmentier, A. and V. T'Kindt (2021). Learning to solve the single machine scheduling problem with release times and sum of completion times . arXiv:2101.01082 (cit. on pp. 3, 11).
- Pflug, G. C. and A. Pichler (2014). Multistage stochastic optimization . Vol. 1104. Springer (cit. on p. 2).
- Pillac, V., M. Gendreau, C. Guéret, and A. L. Medaglia (2013). 'A review of dynamic vehicle routing problems'. In: European Journal of Operational Research 225.1, pp. 1-11 (cit. on p. 2).

Raza, S. M., M. Sajid, and J. Singh (2022). 'Vehicle Routing Problem Using Reinforcement Learning: Recent Advancements'. In: Advanced Machine Intelligence and Signal Processing . Ed. by D. Gupta, K. Sambyo, M. Prasad, and S. Agarwal. Singapore: Springer Nature Singapore, pp. 269-280 (cit. on p. 3).

Ritzinger, U., J. Puchinger, and R. F. Hartl (2016). 'A survey on dynamic and stochastic vehicle routing problems'. In: International Journal of Production Research 54.1, pp. 215-231 (cit. on p. 3).

- Soeffker, N., M. W. Ulmer, and D. C. Mattfeld (2022). 'Stochastic dynamic vehicle routing in the light of prescriptive analytics: A review'. In: European Journal of Operational Research 298.3, pp. 801-820 (cit. on p. 2).
- Steever, Z., M. Karwan, and C. Murray (2019). 'Dynamic courier routing for a food delivery service'. In: Computers &amp; Operations Research 107, pp. 173-188 (cit. on p. 3).
- Ulmer, M. W., N. Soeffker, and D. C. Mattfeld (2018). 'Value function approximation for dynamic multi-period vehicle routing'. In: European Journal of Operational Research 269.3, pp. 883-899 (cit. on pp. 2, 3).
- Vidal, T. (2022). 'Hybrid genetic search for the CVRP: Open-source implementation and SWAP* neighborhood'. In: Computers &amp; Operations Research 140, p. 105643 (cit. on pp. 7-9, 14).

- Vidal, T., G. Laporte, and P. Matl (2020). 'A concise guide to existing and emerging vehicle routing problem variants'. In: European Journal of Operational Research 286.2, pp. 401-416 (cit. on p. 2).
- Vidal, T., N. Maculan, L. S. Ochi, and P. H. Vaz Penna (2016). 'Large Neighborhoods with Implicit Customer Selection for Vehicle Routing Problems with Profits'. In: Transportation Science 50.2, pp. 720-734 (cit. on p. 10).