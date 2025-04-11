Published in: Transportation Research Part C 137 (2022) 103560.

This preprint is originally revised and extended from its previous version (https://arxiv.org/abs/2011.10915v1), so there may exist some overlaps. In the previous version, we proposed a framework of Markov routing games (MRG) and introduced the bi-level network design problems. In the latest one, we first modify the MRG framework by considering the linkage between MRG and dynamic traffic assignment (DTA). We then incorporate various DTA models into MRG and investigate game equilibria for agents. We remove the bi-level network design problems from the previous version. Due to significant differences by major revision, it is recommended to read and make use of this latest version. The previous version shall be removed.

Please cite this Paper as: Shou, Z., Chen, X., Fu, Y., Di, X., 2022. Multi-Agent Reinforcement Learning for Markov Routing Games: A New Modeling Paradigm For Dynamic Traffic Assignment. Transportation Research Part C: Emerging Technologies 137, 103560. doi:https://doi.org/10.1016/j.trc.2022.103560

## Multi-Agent Reinforcement Learning for Markov Routing Games: A New Modeling Paradigm For Dynamic Traffic Assignment

Zhenyu Shou , Xu Chen , Yongjie Fu , Xuan Di a a a a,b, ∗

a Department of Civil Engineering and Engineering Mechanics, Columbia University b Data Science Institute, Columbia University

## Abstract

This paper aims to develop a paradigm that models the learning behavior of intelligent agents (including but not limited to autonomous vehicles, connected and automated vehicles, or human-driven vehicles with intelligent navigation systems where human drivers follow the navigation instructions completely) with a utility-optimizing goal and the system's equilibrating processes in a routing game among atomic selfish agents. Such a paradigm can assist policymakers in devising optimal operational and planning countermeasures under both normal and abnormal circumstances. To this end, we develop a Markov routing game (MRG) in which each agent learns and updates her own en-route path choice policy while interacting with others in transportation networks. To efficiently solve MRG, we formulate it as multiagent reinforcement learning (MARL) and devise a mean field multi-agent deep Q learning (MF-MADQL) approach that captures the competition among agents. The linkage between the classical DUE paradigm and our proposed Markov routing game (MRG) is discussed. We show that the routing behavior of intelligent agents is shown to converge to the classical notion of predictive dynamic user equilibrium (DUE) when traffic environments are simulated using dynamic loading models (DNL). In other words, the MRG depicts DUEs assuming perfect information and deterministic environments propagated by DNL models. Four examples are solved to illustrate the algorithm efficiency and consistency between DUE and the MRG equilibrium, on a simple network without and with spillback, the Ortuzar Willumsen (OW) Network, and a real-world network near Columbia University's campus in Manhattan of New York City.

Keywords: Markov Game, Multi-Agent Reinforcement Learning (MARL), Atomic Agents, Dynamic User Equilibrium (DUE)

## 1. Introduction

When one chooses a path dynamically while navigating a road network, others may also do so to compete for limited road resources. Therefore it is crucial to model the dynamic routing choices of

∗ Corresponding author. Tel.: +1 212 853 0435; Email address: sharon.di@columbia.edu (Xuan Di)

many drivers simultaneously. This paper aims to develop a game-theoretic paradigm that models one's learning behavior and the system's equilibrating processes in a dynamic routing game among atomic selfish agents, while accounting for competition among drivers.

While traveling from an origin to a destination, one needs to select a sequence of road segments. Thus, en-route path choice is inherently a sequential decision-making process, in which at each junction one decides what the next link to take. Markov decision processes (MDP) (Puterman, 1994) and reinforcement learning (RL) (Sutton and Barto, 1998) have become popular tools to model such a sequential process, in which travelers are rational agents to optimize a prescribed objective function (or reward) (Seongmoon Kim et al., 2005; Kim et al., 2016). In literature, recently there is a growing body of research using RL based approaches on route choice behavior. We will first review the literature on route choice of single-agent and then move to that of multi-agent.

## 1.1. Literature on reinforcement learning based route choices

The existing literature on dynamic routing games using RL-based approaches is listed in Table 1. Based on how to define the action of an agent, we broadly categorize RL-based route choice models into two groups. In the first group, the action of an agent is a route (Zhou et al., 2020; Ramos et al., 2018; Stefanello et al., 2016). For example, in Ramos et al. (2018); Stefanello et al. (2016), for an agent aiming to go to her destination node from her origin node, the action space for her is the k shortest paths from her origin to her destination. These studies assume that every driver does follow her chosen route until she reaches her destination. In reality, however, drivers could deviate from their chosen route when they realize that traffic condition on some alternative routes might be better, especially when they get stuck in traffic on the current route. To capture this en-route adaptive behavior of drivers, the second group of studies focus on en-route traffic assignment, or route choice from the perspective of a driver. In these studies, the action space of an agent currently at a node is the outbound links from the node (Grunitzki et al., 2014; Bazzan and Grunitzki, 2016; Mao and Shen, 2018). In other words, every agent needs to decide which outbound link to choose whenever the agent arrives at a node, until the agent reaches some terminal node. This en-route decision-making process captures the adaptive behavior of real drivers.

To study the en-route decision-making process of drivers, both single-agent RL and multi-agent RL are used in the literature. A single-agent Q-learning approach is developed in Mao and Shen (2018) with the use of some global information such as congestion status in the definition of state. The authors bootstrapped two traffic profiles from the PeMS dataset and derived an optimal policy for the agent. Unfortunately, single-agent RL fails to capture the competition among adaptive agents. Multi-agent RL is recently used to tackle the multi-driver route choice task (Grunitzki et al., 2014; Bazzan and Grunitzki, 2016). Despite of modeling multi-driver interactions, these studies use independent multi-agent tabular Q-learning where every agent is treated as an independent learner who has no information of other agents. The assumption of independent learners suffers from two issues. First, it is impractical to assume that agents select routes independently without accounting for interactions with other agents and congestion effects on transportation networks. Second, theoretical convergence guarantee for Q-learning does not hold, because the environment is no longer Markovian and stationary (Matignon et al., 2012; Nguyen et al., 2020).

To fill the aforementioned research gaps, in this paper, we will model en-routing decision-making processes in a multi-agent system as a Markov game in which one's payoff function depends on all others' routing strategies.

## 1.2. MARL and multi-agent Markov game

As a game-theoretic framework for MARL, Markov game (Littman, 1994; Solan and Vieille, 2015) is a generalization of MDP-like environments to multiple interacting agents with competing goals, in which the environment makes transitions probabilistically in response to the agents' actions. Markov game is typically defined as a non-cooperative game where self-interested agents aiming to maximize their own payoffs. In a Markov game, the solution concept of Nash equilibrium is generally adopted (Hu and Wellman, 1998). At a Nash equilibrium, one agent's strategy (i.e., policy) is the best response to other agents' strategy. MARL is an efficient and versatile tool to solve for the optimal policy of each agent. Among the pioneering studies, Littman (1994) proposes a minimax Q-learning algorithm to solve for the optimal policies of two agents with opposed goals in a zero-sum game, and Hu and Wellman (1998) extends it to a general-sum game and provided a multi-agent Q-learning algorithm.

Recent literature has witnessed various applications of MARL to high-dimensional and complicated tasks such as playing the game of Go (Silver et al., 2016, 2017), Poker (Brown and Sandholm, 2018, 2019),

Dota 2 (OpenAI, 2018), and StarCraft II (Vinyals et al., 2019). Outside the computer game domain, MARL has also attracted significant attention and been used in energy sharing (Prasad and Dusparic, 2019), federated control (Kumar et al., 2017), and sequential social dilemma (Leibo et al., 2017), to name a few. Interested readers are referred to Nguyen et al. (2020) for a comprehensive review of applications and challenges of MARL. In the transportation domain, we have also seen a growing trend of applying MARL to vehicle routing (Bazzan and Kl¨gl, 2008; Bazzan and Grunitzki, 2016), traffic signal control u (Bakker et al., 2010; Chen et al., 2020), fleet management (Lin et al., 2018; Shou et al., 2020; Shou and Di, 2020; Di et al., 2018; Di and Ban, 2019; Chen and Di, 2021), order dispatching (Li et al., 2019), and autonomous driving (Palanisamy, 2019; Bhalla et al., 2020). However, its application to dynamic routing game is still at its nascent stage.

## 1.3. Contributions of this paper

Here we will highlight the main contributions of this study as follows.

- 1. Multi-agent dynamic routing is modeled as a Markov routing game (MRG) that accounts for the interactions among adaptive agents and traffic congestion. It is capable of modeling individual agents' routing behavior with a partial or unknown information structure and a model-free environment, in which agents need to learn optimal policies through repeated interactions with other agents and the traffic environment.
- 2. An efficient multi-agent reinforcement learning algorithm is developed via a mean action, which is defined as the traffic flow on the chosen link right after an agent enters the link. The use of the mean action not only captures the competition among agents, but also enables value sharing and policy sharing, which is computationally favorable, especially in a large-sized problem with a large number of agents.
- 3. A variety of dynamic network loading (DNL) models are used as the simulation environment of the proposed Markov routing game. We then demonstrate the consistency of our computed Markov equilibrium and its associated dynamic user equilibrium (DUE). The linkage between the classical dynamic traffic assginment (DTA) paradigm and our proposed game is also demonstrated. We illustrate that the developed Markov game for dynamic routing depicts analytical DTA assuming complete information of environment transition dynamics and travel or delay time functional forms.
- 4. Computational efficiency of the developed Markov routing game is demonstrated on mid- and large-sized networks.

The remainder of the paper is organized as follows. Section 2 introduces the developed Markov game for dynamic routing and devises a mean field multi-agent deep Q-learning algorithm. Section 3 demonstrates the linkage between the classical DUE paradigm and our proposed MARL paradigm. Section 4 presents four examples to demonstrate the consistency between DUE and the equilibrium learned from our model as well as algorithm efficiency, on a simple network without and with spillback, the OW network, and a real-world network near Columbia University's campus in Manhattan of New York City. Section 5 concludes this study.

## 2. From singe- to multi-agent routing models

In this section, we first state formally the problem of dynamic en-route routing in the multi-agent setting. Then we explain how one's sequential decision-making process can be formulated as a singleagent RL. Building on this, we generalize it to multi-agent RL and then propose an efficient algorithm.

Definition 2.1. (MRG Problem Statement): There are N intelligent agents (autonomous vehicles (AV) for example) indexed by i ∈ { 1 2 , , · · · , N } moving from a set of origins, denoted as O , to a set of destinations D . Each agent aims to select its optimal next-go-to edge at every intermediate node by minimizing a travel cost functional over a predefined planning horizon [0 , T ]. The traffic system evolves according to all agents' actions and some transition probability at each time step. With the system state updated, those agents at intermediate nodes select the next-go-to edge. The process continues till either all agents reach the destination or the planning time horizon is terminated.

4

Table 1: Existing research on dynamic routing using RL based approaches

When one solves its own optimal control problem while others are doing so simultaneously, a congestion (routing) game forms. We assume that the system evolution dynamic remains unknown to these agents. Thus they have to learn the game and select their individual routing strategy online via their interaction with other agents and the traffic environment. The outcome is a Nash equilibrium that represents a scalable decentralized online routing strategy for each controllable agent.

- Remark. 1. In this paper, we assume travelers are perfectly rational agents in the context of selfish routing. In other words, every traveler is a self-interest agent who aims to maximize individual accumulative rewards or minimize individual cost or maximize payoffs, including but not limited to autonomous vehicles, connected and automated vehicles, or human-driven vehicles with intelligent navigation systems and human drivers follow the navigation instructions completely. The device or mechanism that executes routing algorithms can be smart computers residing in autonomous vehicles or connected and automated vehicles, intelligent navigation aids or phone apps, and even human drivers who are perfectly rational in dynamic routing.
- 2. We use agents, players, travelers, and controllable vehicles interchangeably, which all represent those intelligent vehicles that select routes based on utility-optimizing behavior.

## 2.1. Single-agent reinforcement learning

As a stepping stone, we first introduce a single-agent RL approach. Within the single-agent scope, there is only one agent interacting with the stochastic environment. The goal of the deep Q-learning approach is to derive an optimal policy so that the agent could get the maximum expected cumulative reward by following the policy in the environment.

To be specific to the route choice task, we introduce the single-agent deep Q learning approach on a Braess network, as presented in Figure (1). In the Braess network, there are four nodes, namely { n , n 0 1 , n 2 , n 3 } . n 0 is the origin node and n 3 the destination/terminal node. There are five directed links connecting these nodes. We denote the link connecting n i and n j as l ij . Link travel time on l ij is denoted by ∆ t ij . In this study, ∆ t 01 = 45, ∆ 23 = 45, ∆ t 02 = k 02 · x , ∆ t 13 = k 13 · x , and ∆ t 21 = α , where x denotes the travel flow (i.e., number of vehicles) on the link, k 02 and k 13 are two multipliers, and α is a control parameter.

Figure 1: Braess network

![Image](Papers_Converted/0_Artifacts/[shou2022]_Multi-agent_reinforcement_learning_for_Markov_routing_games-A_new_modeling_paradigm_for_dynamic_traffic_assignment_artifacts/image_000000_7ebbb9b9ecec3605196f49f9abcd62f461076346ae40f5026b017f21ec479018.png)

First and foremost, the interaction between the agent and the environment is typically characterized by a Markov decision process or an MDP for short (Puterman, 1994). An MDP is specified by a tuple ( S, A, R, P, γ ), where S denotes the state space, A the action space, R the reward function, P the state transition probability matrix, and γ ∈ [0 , 1] the discount factor. An MDP goes as follows. At any state s ∈ S but some terminal state, the agent chooses action a ∈ A and executes the action in the environment. It observes a state transition from s to a new state s ′ ∈ S with probability P s ( ′ | s, a ) and receives a reward r s, a, s ( ′ ) ∈ R . In the context of route choice,

- · s ∈ S . State s consists of two components, namely node and time, i.e., s = ( n, t ).

- · a ∈ A . Action a for the agent currently in state s = ( n, t ) is simply one of the outbound links from node n . For example, a = l n,n ′ , where l n,n ′ is the link connecting node n and node n ′ . In this study, we assume action is deterministic, meaning that the agent will enter the chosen link. Note that the route choice for each agent is a discrete action space and the number of outbound links from a node varies from node to node in the network, indicating that the number of allowable actions is dependent on the node where the agent is currently located.
- · P . After taking action a in state s , the agent arrives at a new state s ′ = ( n , t ′ ′ ) with probability P s ( ′ | s, a ), where n ′ is the end node of the chosen link (i.e., l n,n ′ ), and t ′ is the time right after the state transition. P is typically unknown to the agent. Therefore, the agent needs to repeatedly interact with the environment to gain state transition experiences, i.e., ( s, a, s ′ ).
- · r ∈ R . In addition to the observed state transition s → s ′ , the agent receives a reward r t ′ ( s, a, s ′ ) after executing action a , where the subscript t ′ explicitly denotes that the reward is received by the agent at time t ′ . In the context of route choice, the reward function r t ′ ( s, a, s ′ ) is typically chosen as some negative travel cost related to the state transition s → s ′ . For example, r t ′ ( s, a, s ′ ) = -( t ′ -t ), i.e., negative travel time, and r t ′ ( s, a, s ′ ) = -dist( n, n ′ ), where dist( n, n ′ ) measures the distance between node n and n ′ , i.e., negative travel distance.
- · γ . The discount factor γ is used to discount the future reward. When γ = 1, the agent does not differentiate future rewards from immediate rewards. As γ get smaller, the agent cares less about rewards received in the distant future, therefore her decision-making gets more myopic. In this study, we take γ = 1 because usually drivers aim to minimize their cumulative travel cost for a trip. In other words, they value a future travel cost and an immediate cost similarly.
- · ρ = ∑ T t =0 γ r t t . ρ is the discounted cumulative reward. The agent aims to maximize ρ by deriving an optimal policy.

A policy µ is a mapping from state s to action a , i.e., µ s ( ) = a . Following policy µ , value function V µ ( s ) is defined as the expected discounted cumulative reward from state s . Mathematically, it is given in a recursive form, namely V µ ( s ) = E a ∼ µ s ,s ( ) ′ ∼ P ( ·| s,a ) [ r s, a, s ( ′ ) + γV µ ( s ′ )].

An optimal policy , denoted as µ ∗ , is the policy that maximizes the value function, i.e., µ ∗ = argmax V µ µ ( s , ) ∀ s ∈ S . The optimal value function, denoted as V ( s ), is given as (Sutton and Barto, 1998)

$$V ( s ) = m a x _ { a } \mathbb { E } _ { s ^ { \prime } \sim P ( \cdot | s, a ) } [ r ( s, a, s ^ { \prime } ) + \gamma V ( s ^ { \prime } ) ].$$

While the value function V ( s ) captures the optimal expected cumulative reward that an agent can earn from state s , the state-action value Q s, a ( ) forces the agent to take action a and therefore captures the optimal expected cumulative reward from state s and action a . Mathematically, V ( s ) = max Q s,a a ( ). Substituting the relation between V and Q into Equation (1) yields

$$Q ( s, a ) = \mathbb { E } _ { s ^ { \prime } \sim P ( \cdot | s, a ) } [ r ( s, a, s ^ { \prime } ) + \gamma m a x _ { a ^ { \prime } } Q ( s ^ { \prime }, a ^ { \prime } ) ].$$

With the optimal action-value function Q , optimal policy can be explicitly derived as µ ∗ ( s ) = argmax Q s,a a ( ). With the collected experience tuple ( s, a, s , r ′ ), the agent can update the Q value by the widely used tabular Q-learning algorithm

$$Q ( s, a ) \leftarrow Q ( s, a ) + \eta [ r + \gamma m a x _ { a ^ { \prime } } Q ( s ^ { \prime }, a ^ { \prime } ) - Q ( s, a ) ],$$

where η ∈ (0 , 1] is the learning rate. Convergence is guaranteed for the Q-learning algorithm when the learning rate is decayed properly over time (Sutton and Barto, 1998). Thanks to the emergence of deep Q networks (DQNs) (Mnih et al., 2015), we could resort to neural networks as a functional approximator of the Q function. Denoting the functional approximator parameterized by θ as Q θ , DQN updates its parameter θ by minimizing the following loss

$$\mathcal { L } ( \theta ) = \mathbb { E } _ { s, a, s ^ { \prime } } \left [ \left ( r ( s, a, s ^ { \prime } ) + \gamma m a x _ { a ^ { \prime } } Q _ { \theta ^ { - } } ( s ^ { \prime }, a ^ { \prime } ) - Q _ { \theta } ( s, a ) \right ) ^ { 2 } \right ],$$

where Q θ -is called a target network copied from Q θ every τ episodes to ensure training stability, where τ is a hyperparameter.

## Example 2.1. (Single-agent route choice).

To be more concrete, we demonstrate the aforementioned notations in the Braess network. Supposing an agent is initially placed at n 0 in the Braess network, the initial state of the agent is s = ( n , 0 0). There are two allowable actions for the agent, namely l 01 and l 02 . Assuming the agent chooses action l 02 in the initial state, the agent arrives at s ′ = ( n , 2 1) with probability P s ( ′ = ( n , 2 1) | s = ( n , 0 0) , a = l 02 ) = 100%. The time component in s ′ is 1 because the link travel time ∆ t 02 = x = 1 (i.e., there is one agent on this link). The state transition probability is 100% because both the action and the state transition are deterministic in this case. Assuming the reward function is the negative travel time, the agent receives a reward of -∆ t 02 = -1 along with the state transition s → s ′ .

We now apply the single-agent Q-learning to the Braess network with one agent initially at n 0 . For the purpose of demonstration, we assume α = 1 and k 02 = k 13 = 40 in this example. Due to the simplicity of this case, it could be solved analytically from the destination and moving backwards. Noticing the static nature of this example, we neglect the time component in state. n 3 is the terminal node, meaning that V ( n 3 ) = 0. There are two nodes connecting n 3 , namely n 1 and n 2 . Note that the agent could arrive at n 1 from n 2 , meaning that V ( n 2 ) is dependent on V ( n 1 ) and V ( n 1 ) is independent on V ( n 2 ), we thus discuss n 1 first. At node n 1 , the only allowable action is l 13 leading the agent to terminal node n 3 , yielding Q n , l ( 1 13 ) = -∆ t 13 + γV ( n 3 ) = -40 and V ( n 1 ) = -40. At node n 2 , there are two actions, namely l 21 and l 24 . Action l 21 leads the agent to n 1 , yielding Q n , l ( 2 21 ) = -∆ t 21 + γV ( n 1 ) = -( α +40) = -41. Action l 24 leads the agent to the terminal node n 3 , yielding Q n , l ( 2 24 ) = -∆ t 24 + γV ( n 3 ) = -45. Therefore, V ( n 2 ) = max Q n ,l ( ( 2 21 ) , Q n ( 2 , l 24 )) = -41 and µ ∗ ( n 2 ) = l 21 . At node n 0 , there are two actions, namely l 01 and l 02 . Action l 01 leads the agent to n 1 , yielding Q n , l ( 0 01 ) = -∆ t 01 + γV ( n 1 ) = -85. Similarly, Q n , l ( 0 02 ) = -∆ t 02 + γV ( n 2 ) = -81. Therefore, V ( n 0 ) = max Q n ,l ( ( 0 01 ) , Q n ( 0 , l 02 )) = -81 and µ ∗ ( n 0 ) = l 02 . Following optimal policy µ ∗ , the agent takes route n 0 → n 2 → n 1 → n 3 , which is the shortest path in this example.

## 2.2. Multi-agent reinforcement learning and Markov games

Although the single-agent Q learning efficiently finds the shortest path, it fails to capture the competition among agents in a multi-agent system (MAS) (Di and Shi, 2021). In a single-agent RL problem, only one agent is placed in the environment to learn the optimal policy, which maximizes the expected cumulative reward of the agent. Unfortunately, when multiple agents follow this optimal policy, their expected cumulative reward might be low. For example, if there are 50 agents at node n 0 initially in the Braess network, their expected cumulative reward is -110 by following the optimal policy (i.e., the shortest path n 0 → n 2 → n 1 → n 3 ). This reward (i.e., -110) is lower than that of following another route (e.g., n 0 → n 1 → n 3 yields an expected cumulative reward of -95). Therefore, to tackle a multidriver route choice task, we develop a multi-agent deep Q-learning approach in this section to capture the competition among agents.

## 2.2.1. Problem formulation

Similar to the previous single-agent case, we introduce how to formulate the multi-agent routing problem on the Braess network shown in Figure (1).

Due to the existence of multiple agents, the multi-agent routing problem is formulated as a Markov game (Littman, 1994), which is a generalization of Markov decision processes to multiple interacting agents with competing goals, in which the environment makes transitions probabilistically in response to the agents' actions. We denote the Markov game by a partially observable Markov decision process (Littman, 1994), defined by a tuple ( S, O , O 1 2 , · · · , O N , A 1 , A 2 , · · · , A N , P, R 1 , R 2 , · · · , R N , N, γ ), where N is the number of agents and S is the environment state space. Environment state s ∈ S is not fully observable. Instead, agent i draws a private observation o i ∈ O i which is correlated with s . O i is the observation space of agent i , yielding a joint observation space O = O 1 × O , 2 ×···× O N , A i is the action space of agent i ∈ { 1 2 , , · · · , N } , yielding a joint action space A = A 1 × A 2 ×···× A N , P : S × A × S → [0 , 1] is the state transition probability, R i : S × A × S → R is the reward function for agent i , and γ is the discount factor. Agent i ∈ { 1 2 , , · · · , N } uses a policy π i : O i × A i → [0 , 1] to choose actions after drawing observation o i . After all agents taking actions, the joint action a triggers a state transition s → s ′ based on the state transition probability P ( s ′ | s a , ). Agent i draws a private observation o ′ i corresponding to s ′ and receives a reward r i ( s a s , , ′ ). Agent i aims to maximize its discounted expected cumulative reward by deriving an optimal policy µ ∗ i . This process repeats until agents reach their own terminal state.

Due to the existence of other agents, the Q-value function for agent i , i.e., Q i , is now dependent on the environment state s ∈ S and the joint action a ∈ A of all agents, i.e,

$$Q _ { i } = Q _ { i } ( \mathbf s, \mathbf a ).$$

Similarly, the value function of agent i , i.e., V i = V i ( s ), is dependent on the environment state s .

In the multi-agent system, optimal policy of agent i , denoted as µ ∗ i , is defined as the i th agent's best response to others' policies when others hold their policies constant. Below we introduce the formal definition of optimal policy in a multi-agent system using the concept of Nash equilibrium (Littman, 2001).

Definition 2.2. 1. A set of policies µ , i · · · , µ N is a Nash equilibrium if each is a best response to the others. In other words, nobody can improve her value function by unilaterally switching her policy, while all others hold their policies fixed. That is, ∀ i ∈ { 1 , · · · , N } , the value attained by agent i from any state s ,

$$V _ { i } ( s ) = m a x _ { a _ { i } } \mathbb { E } _ { \mathfrak { a } ^ { - i } \sim A } \mathbb { E } _ { s ^ { \prime } \sim P ( \cdot | s, \mathfrak { a } ) } [ r _ { i } ( s, \mathfrak { a }, s ^ { \prime } ) + \gamma V _ { i } ( s ^ { \prime } ) ].$$

At a Nash equilibrium, each agent maximizes its value function given that all other agents remain fixed. Every Markov game has a Nash equilibrium in stationary policies (Filar and Vrieze, 1997).

- 2. Markov strategy is the policy that depends only on the current state. If all players other than i play Markov strategies, then player i has a best response that is a Markov strategy (Solan and Vieille, 2015). In this paper, we mainly focus on Markov strategy that is a mapping from the current state s to action a .

Multi-agent RL is a computational tool for Markov games. We further specify each component using the language of MARL below.

- · N . There are N controllable adaptive agents, denoted by { 1 2 , , · · · , N } . Note that in a traffic network, there are also other traffic participants who are not controlled by the learning algorithm developed in this study. We regard those uncontrollable traffic participants as background traffic. The background traffic affects the behavior of controllable agents by its impact on link travel cost. For example, if the background traffic has caused a traffic jam on a link, it is very likely that controllable agents will avoid this jammed link.
- · s ∈ S . Environmental state s consists of some global information such as distribution of agents and traffic condition on each link. s is not fully accessible to agents.
- · o ∈ O . Although the environmental state s is not observable by agent i , the agent is able to draw a private observation o i ∈ O i , which is correlated with s . The Cartesian product of private observation spaces of all agents forms the joint observation space, i.e., O = O 1 × O 2 ×···× O N . In this paper, o i consists of two components, namely node and time, i.e., o i = ( n, t ). Joint observation o = ( o , o 1 2 , · · · , o N ).
- · a ∈ A . Based on o i = ( n, t ), the allowable action set for agent i ∈ { 1 2 , , · · · , N } consists of all outbound links from node n . For example, a i = l n,n ′ , where l n,n ′ is the link connecting n and n ′ . Joint action a = ( a , a 1 2 , · · · , a N ).
- · P . Joint action a triggers a state transition s → s ′ with probability P ( s ′ | s a , ). Agent i ∈ { 1 2 , , , · · · , N } draws a new private observations, namely o ′ i = ( n , t ′ ′ ), where n ′ is the end node of the chosen link l n,n ′ and t ′ is the time when agent i arrives at n ′ . The private observation o ′ i is correlated with s ′ .
- · r ∈ R . In addition to o ′ i , agent i ∈ { 1 2 , , · · · , N } receives a reward r i,t ′ ( s a s , , ′ ), where the subscript i, t ′ explicitly denotes that the reward is received by agent i at time t ′ . Similar to the single-agent RL, reward is typically some negative travel cost such as travel time and travel distance in the context of route choice.
- · γ . Similar to the single-agent RL, γ = 1.

- · ρ i = ∑ T t =0 γ r t i,t . ρ i is the discounted cumulative reward received by agent i . If the goal of agent i is to maximize ρ i , agents are non-cooperative and selfish; if the goal of agent i is to maximize the average discounted cumulative rewards of all agents, i.e., 1 N ∑ N i =1 ρ i , agents are cooperative.

Due to the coexistence of other agents, Q function of agent i , i.e., Q i is now a function of environmental state s and joint action a , namely

$$Q _ { i } = Q _ { i } ( \mathbf s, \mathbf a ).$$

Similarly, value function of agent i , i.e., V i is a function of environmental state s , namely V i = V i ( s ). Note that each agent i has her own Q function, i.e., Q i , which may be different from Q functions of other agents, and therefore has an optimal policy µ ∗ i different from policies of other agents. This distinguishes multi-agent RL from its single-agent counterpart, because agents may behave differently even in the same state with multi-agent RL while they choose the same action in the same state with single-agent RL.

Considering that environmental state s is not fully observable by agents, Q function shown in Equation (7) is not tractable from the perspective of agent i . Actually, it is private observation o i based on which agent i chooses next action. We thus rewrite Q function for agent i as

$$Q _ { i } = Q _ { i } ( o _ { i }, o _ { - i }, a _ { i }, a _ { - i } ),$$

where o -i and a -i denote the joint observation and joint action of all agents except agent i , respectively. In a non-coordinated environment (i.e., agents are not sharing their private observations or actions), o i and a i are but o -i and a -i are not accessible to agent i . Unfortunately, removing the dependency of Q i on a -i or o -i may introduce large instability in Q-learning. In addition, convergence guarantee in single-agent Q-learning is no longer valid due to the adaptive behavior of other agents if o -i and a -i are not included (Matignon et al., 2012).

For a real-world multi-agent problem, there are commonly more than tens of thousands of agents. Maintaining one Q function, i.e., Q i for each agent is thus computationally infeasible. Leveraging some homogeneity among agents (e.g., some agents share the same utility function), we broadly categorize agents into C groups and enable Q function sharing within each group. We denote the shared Q function for agents in group c ∈ { 1 2 , , · · · , C } as

$$Q ^ { c } ( o _ { i }, o _ { - i }, a _ { i }, a _ { - i } ).$$

The dimension of the input to the shared Q function easily becomes prohibitively large when the number of agents in the environment increases. For example, the joint action space for 1,000 agents is 1 000 , × d dimensional if each action is a d dimensional vector. We take d as a generic representation of the dimension of the action space of each agent. For example, d could be 1 action of agents denoted as a scalar (e.g., 1,2,..., i.e., label encoding) or 3 when the agent has three actions to take and these actions are represented by one hot encodings. To tackle this issue, we first notice that joint observation o -i and joint action a -i record full information of other agents, which may contain redundant information from the perspective of agent i . Actually, in the task of route choice, observations and actions of agents who are currently far away from agent i have a very limited influence on agent i . It is nearby agents who exert an impact on the traffic condition around agent i . For example, after agent i chooses a link at a node, an immediate influential quantity is the traffic flow on the chosen link. If the flow is high, agent i may experience a high travel cost on the link; if the flow is low, agent i may have a smooth transition to the end of the link. Therefore, high-dimensional o -i and a -i in Equation (9) could be approximated by some low-dimensional aggregate information of nearby agents. Similar to the mean field approximation in Yang et al. (2018), we call this aggregate information as the mean action of nearby agents and denote it by ¯ . a i Q function in Equation (9) thus becomes

$$Q ^ { c } ( o _ { i }, o _ { - i }, a _ { i }, a _ { - i } ) & \approx Q ^ { c } ( o _ { i }, a _ { i }, \bar { a } _ { i } ).$$

We now formally define the mean action in the context of route choice.

Definition 2.3. ( Mean action ). Mean action ¯ a i is defined as the traffic flow on the link that is chosen by agent i . Note that the traffic flow is calculated right after agent i enters the link.

## 2.2.2. Mean field multi-agent deep Q-learning

Different from single-agent RL, agent i in group c ∈ { 1 2 , , · · · , C } in an MAS interacts with not only the environment but also other agents and collects experience tuples in the form of ( o , a , o , r i i ′ i i , a ¯ ). i Considering that mean action ¯ a i is typically in a large discrete or even continuous space, a DQN parameterized by θ c , denoted by Q c ( o , a , a i i ¯ i | θ c ), is used to approximate Q c ( o , a , a i i ¯ ). i Similar to Equation (4), DQN updates its parameter θ c by minimizing the following loss

$$\mathcal { L } ( \theta _ { c } ) = \mathbb { E } _ { o _ { i }, a _ { i }, \bar { a } _ { i }, \theta _ { i } ^ { \prime } } \left [ ( r _ { i } + \gamma m a x _ { a _ { i } ^ { \prime } } \mathbb { E } _ { a _ { i } ^ { \prime } \sum _ { a _ { i } ^ { \prime } } } [ Q ^ { c } ( o _ { i } ^ { \prime }, a _ { i } ^ { \prime }, \bar { a } _ { i } ^ { \prime } | \theta _ { c } ^ { - } ) ] - Q ^ { c } ( o _ { i }, a _ { i }, \bar { a } _ { i } | \theta _ { c } ) ) ^ { 2 } \right ]$$

where Q c ( ·| θ -c ) is a target network copied from Q c ( ·| θ c ) every τ episodes to stabilize training. After updating Q function, optimal policy

$$\mu _ { i } ^ { * } ( o _ { i } ) = a r g m a x _ { a _ { i } } \mathbb { E } _ { \bar { a } _ { i } \sim \mu _ { = i } ^ { * } } [ Q ^ { c } ( o _ { i }, a _ { i }, \bar { a } _ { i } | \theta _ { c } ) ], \ \forall o _ { i } \in O _ { i }.$$

## Algorithm 1 Mean field multi-agent deep Q-learning (MF-MA-DQL)

- 1: Input: exploration parameter glyph[epsilon1] = glyph[epsilon1] 0 , learning rate η = η 0 , target network update period τ
- 2: Initialize one DQN Q c ( o, a, a θ ¯ | ), parameterized by θ c , and one target network Q c ( o, a, a θ ¯ | -c ) for each group c ∈ { 1 2 , , · · · , C }
- 3: Initialize N dictionaries to store the optimal policy for agents, i.e., µ , ∗ i ∀ i ∈ { 1 2 , , · · · , N }
- 4: Initialize one experience replay buffer B c for each group c ∈ { 1 2 , , · · · , C }
- 5: Set episode = 0

## 6: repeat

- 7: From the initial environmental state s 0 , each agent i draws observation o i
- 8: Set t = 0

## 9: repeat

- 10: For each agent i , select action a i according to the glyph[epsilon1] -greedy method, i.e., randomly select an allowable action with probability glyph[epsilon1] and greedily according to policy µ ∗ i with probability 1 -glyph[epsilon1]
- 13: Store experience tuple ( o , a , o , r i i ′ i i , a ¯ ) into replay buffer i B c if agent i belongs to group c
- 11: Execute joint action a = ( a , a 1 2 , · · · , a N ) in the environment to trigger state transition s → s ′ 12: Each agent i draws new observation o ′ i , receives reward r i , and observes mean action ¯ a i

14:

t

←

t

+1

15:

16:

17:

18:

19:

20:

21:

22:

23:

24:

25:

26:

27:

28:

29:

until

t

for

=

T

c

= 1 to for

C

do

j

= 1 to

J

c

do

Sample a batch of size

K

Update parameter

θ

c

of

Update optimal policy

c

Q

µ

end for end for

episode

=

episode

+1

Decrease the exploration parameter glyph[epsilon1]

Decrease the learning rate

η

if episode

mod

τ

= 0

then for

c

= 1 to

C

do

Update parameter

θ

end for

30:

end if

- 31: until the algorithm converges
- 32: Return optimal policies µ , ∗ i ∀ i ∈ { 1 2 , , · · · , N }

The mean field multi-agent deep Q-learning (MF-MA-DQL) algorithm is summarized in Algorithm (1). We first initialize a centralized deep Q network, parameterized by θ c , and a target Q network, parameterized by θ -c for each group c ∈ { 1 2 , , · · · , C } . Although Q function is shared among agents in the same group, each agent i in group c has her own optimal policy µ ∗ i , which is a dictionary mapping from observation o i to action a i . From initial environmental state s 0 , each agent i ∈ { 1 2 , , , · · · , N } keeps

-

c

∗

i

from replay buffer

B

c

c

by minimizing the loss defined in Equation (11)

of agent

i

by Equation (12)

of the target network, i.e.,

θ

-

c

←

θ

c

drawing private observation o i and taking action a i according to the widely used glyph[epsilon1] -greedy method (i.e., agent i chooses action randomly with probability glyph[epsilon1] and greedily from optimal policy µ ∗ i with probability 1 -glyph[epsilon1] ), until reaching some terminal state. Joint action a = ( a , a 1 2 , · · · , a N ) is executed in the environment to trigger the state transition s → s ′ . Each agent i draws new private observation o ′ i , receives reward r i , and observes mean action ¯ . a i An experience replay buffer B c for each group c is used to store experience tuple ( o , a , r , a i i i ¯ i , o ′ i ) generated by agent i in group c . Batch training is then used to update the centralized Q function by sampling a batch of size K c from buffer B c and minimizing the loss shown in Equation (11) over this batch. With the updated centralized Q function, each agent updates her optimal policy by Equation (12).

Remark. As a value based approach, the developed MF-MA-DQL algorithm does not suffer the problem of a variable action set. A variable action set means that the number of allowable actions might vary from state to state. For example, there are two outbound links from node n 0 and one outbound link from node n 1 in the Braess network shown in Figure (1), causing the problem of variable action set. A policy based approach, e.g., the widely used actor-critic method, does not fit naturally well into a RL problem with variable action set. The main reason is that the policy neural network typically takes as input a state and outputs a probability distribution on all allowable actions (i.e., the action set). Consequently, a variable action set might result in some challenges to maintain a good structure of the policy network. Fortunately, value based approaches automatically handle the problem of variable action set, because a value based approach calculates the expected Q value of all possible actions at a state and selects the action with the highest Q value as the optimal policy.

Example 2.2. (Multi-agent route choice). Now we apply the MF-MA-DQL algorithm to a multi-agent route choice problem. In the Braess network shown in Figure (1), now two agents are initially placed at node n 0 . The goal of both agents is to travel to the destination node n 3 as soon as possible. Consequently, these two agents are considered to be homogeneous, because they share the same reward function (i.e., the negative travel time) and go to the same destination. We first demonstrate the notations of Markov game in this example.

There are N = 2 agents in the Braess network. Environmental state s is not observable, Correlated with s , the joint observation of agents o = (( n , 0 0) , ( n , 0 0)). There are two allowable actions for both agents, namely l 01 and l 02 . The joint action a = ( a , a 1 2 ). a triggers state transition s → s ′ with probability P ( s ′ | s a , ). Each agent i ∈ { 1 2 , } then draws new private observation o ′ i , receives reward r i , and observes mean action ¯ . To be precise, we illustrate a i o ′ i , r i , and ¯ a i under two joint actions.

- · With a = ( l 01 , l 02 ), o ′ 1 = ( n , 1 ∆ t 01 = 45) and o ′ 2 = ( n , 2 ∆ t 02 = k 02 ). r 1 = -∆ t 01 = -45 and r 2 = -∆ t 02 = -k 02 . ¯ a 1 = 1 and ¯ a 2 = 1.
- · With a = ( l 02 , l 02 ), o ′ 1 = ( n , 2 ∆ t 02 = 2 · k 02 ) and o ′ 2 = ( n , 2 ∆ t 02 = 2 · k 02 ). r 1 = -∆ t 02 = - · 2 k 02 and r 2 = -∆ t 02 = - · 2 k 02 . ¯ a 1 = ¯ a 2 = 2.

Note that although agent 2 takes action l 02 in both cases, the agent arrives at different observations, i.e., ( n , k 2 02 ) and ( n , 2 2 · k 02 ) due to the changing behavior of agent 1 (i.e., taking action l 01 in the first case and l 02 in the second).

Due to the simplicity of this case, we now analyze optimal value at each node in the network. For the purpose of demonstration, we assume α = 10 and k 02 = k 13 = 40 in this example. Similar to Example 2.1, we neglect the time component in private observation due to the static nature of this problem.

- 1. Noticing that node n 3 is the terminal node, V ( n 3 ) = 0.
- 2. At node n 1 , the only action that agents can take is l 13 . After one or two agents, depending on whether both of them reach node n 1 , taking action l 13 , there are two scenarios: 1) there is only one agent (i.e., either agent 1 or agent 2) on link l 13 , meaning that traffic flow on l 13 is 1 and it takes the agent ∆ t 13 = k 13 · 1 = 40 to reach n 3 . Therefore, Q o ( = n , a 1 = l 13 , a ¯ = 1) = -40. Note that here we drop the subscript index i for the agent for notation simplicity, because agents are homogeneous in this case and Q o ( 1 = n , a 1 1 = l 13 , a ¯ 1 = 1) = Q o ( 2 = n , a 1 2 = l 13 , a ¯ 2 = 1) = -40. In other words, no matter who the agent is, it takes the agent 40 time steps to reach n 3 starting from n 1 , taking action l 13 , and observing ¯ = 1 ; 2) there are two agents on link a l 13 , meaning that traffic flow on l 13 is 2 and it takes the agent ∆ t 13 = k 13 · 2 = 80 to reach n 3 . Therefore, Q o ( = n , a 1 = l 13 , a ¯ = 2) = -80. Note that in scenario 2), it could be either both agents choose action l 13 at node n 1 simultaneously or one agent chooses action l 13 first because she arrives at n 1

first and the other agent chooses action l 13 later due to her later arrival at node n 1 (e.g., one agent chooses route n 0 → n 1 and arrives at n 1 at t = 45, and the other agent chooses route n 0 → n 2 → n 1 and arrives at n 1 at t = k 02 + α = 50). In the former case, both agents observe ¯ = 2 and spend a time 80 on their way to n 3 ; in the latter case, the agent who arrives at n 1 first observes ¯ = 1 and a spends time 40 on her way to n 3 and the other agent observes ¯ = 2 and spends time 80. a With Q n , l ( 1 13 , 1) = -40 and Q n , l ( 1 13 , 2) = -80, optimal value at n 1 , i.e., V ( n 1 ), is within the range of [ -40 , -80] and depends on the distribution of ¯. a

- 3. At node n 2 , there are two outbound links, namely l 21 and l 23 . If one or two agents at n 2 choose action l 21 , Q n , l ( 2 21 , a ¯) = -α + γ × V ( n 1 ) = -10+ V ( n 1 ), which is lower than -50, regardless of ¯. a If one or two agents at n 2 choose action l 23 , Q n , l ( 2 23 , a ¯) = -∆ t 23 = -45. Therefore, action l 21 is strictly dominated by action l 23 , indicating that optimal policy at n 2 for both agents is to choose action l 23 and V ( n 2 ) = -45.
- 4. Finally, at node n 0 , there are two possible actions, namely l 01 and l 02 , and both agents choose action simultaneously. There are 3 possible scenarios: 1) when both agents choose action l 01 , Q n , l ( 0 01 , a ¯ = 2) = -∆ t 01 + γV ( n 1 ) = -45+( -80) = -125. Note that V ( n 1 ) = -80 because both agents arrive at node n 1 and choose action l 1 3 at the same time. 2) when both agents choose action l 02 , Q n , l ( 0 02 , a ¯ = 2) = -∆ t 02 + γV ( n 2 ) = -80 + ( -45) = -125. 3) when one agent chooses action l 01 and the other chooses action l 02 , Q n , l ( 0 01 , a ¯ = 1) = -∆ t 01 + γV ( n 1 ) = -45 + ( -40) = -85 and Q n , l ( 0 02 , a ¯ = 1) = -∆ t 02 + γV ( n 2 ) = -40 + ( -45) = -85. Note that V ( n 1 ) = -40 because only one agent uses link l 13 . Therefore, the optimal policy of one agent is to choose action l 01 and that of the other agent is to choose action l 02 at n 0 .

In summary, optimal values of interest are listed in Table 2. Link l 21 is not used by agents because it is strictly dominated by l 23 . From n 0 , the payoff for agents are ( -85 , -85) if they choose actions differently and ( -125 , -125) if they choose the same action (i.e., either l 01 or l 02 ). Therefore, optimal policy of one agent is to choose action l 01 and that of the other is to choose l 02 , and this is a Nash equilibrium because no agent can get better payoff by a unilateral deviation.

Table 2: Optimal values of interest

| observation o   | action a       | mean action ¯ a   | Q ( o, a, ¯) a   | V ( o )                                                                                                              |
|-----------------|----------------|-------------------|------------------|----------------------------------------------------------------------------------------------------------------------|
| n 1 n 1         | l 13 l 13      | 1 2               | - 40 - 80        | V ( n 1 ) = E ¯ a [ Q ( n 1 , l 13 , ¯)] a is within the range of [ - 80 , - 40].                                    |
| n 2 n 2         | l 21 l 23      | 1 or 2 1 or 2     | ≤ - 50 - 45      | Action l 23 strictly dominates l 21 . V ( n 2 ) = Q ( n 2 , l 23 , ¯) a = - 45.                                      |
| n 0 n 0 n 0 n 0 | l 01 l 01 l 02 | 1 2 1             | - 85 - - 85      | V ( n 0 ) = - 85 if agents choose actions differently at n 0 and V ( n 0 ) = - 125 if agents choose the same action. |
|                 | l 02           | 2                 | 125 - 125        |                                                                                                                      |

Markov games to model en-route choice are independent of data-generating mechanism. Here data refers to background traffic in which vehicles move and by which travel choices are influenced, including but not limited to travel time or delay and/or travel speed on a link, queuing delay at a node, as well as transition flows between links. Routing flows can arise from models like DNL, or model-free simulation. In the next two sections, we will demonstrate the proposed routing game using data first generated from existing DNL models and then from a traffic simulator where its data generating mechanism remains unknown.

## 3. Linkage between Dynamic traffic assignment (DTA) and Markov routing games (MRG)

DTA is the descriptive modeling of dynamic traffic flows on networks that is consistent with traffic flow theory, travel behaviors, and established travel demands. It has been widely used to model route choice behavior of travelers. A general DTA problem typically consists of two parts, namely a dynamic network loading (DNL) module and a route choice model.

DNL . Given traffic demand and route choices, the DNL module propagates traffic flow dynamics and/or traffic congestion through the network. There are two components in the DNL module, namely a

link model and a junction model. A link model is used to propagate inflow traffic demand to exit and is usually described by either a delay/latency function or an exit-flow function (Nie and Zhang, 2005). As for the former, both linear delay functions and nonlinear functions (e.g., the widely used BPR function) are used in the literature. As for the latter, there are a large body of literature proposing or using various exit-flow functions such as the M-N model (Merchant and Nemhauser, 1978a,b), the point-queue model (Kuwahara and Akamatsu, 1997; Ban et al., 2012), and the cell transmission model (Daganzo, 1994, 1995). A junction model is used to determine the flow distribution at an intersection.

Route choice . Route choice models guide travelers to properly choose next go-to links right before they arrive at a node/intersection. According to the objective of travelers, there are typically two types of research problems, namely dynamic system optimum (DSO) and dynamic user equilibrium (DUE). While DSO aims to minimize total or average travel cost of all travelers (Ziliaskopoulos, 2000), DUE describes the equilibrium state in which no individual traveler could decrease her travel cost by unilaterally deviating from her current route choices (Friesz and Han, 2019; Friesz et al., 2013). Huang et al. (2021) applies mean-field game to model both velocity and route choices of a large number of intelligent agents on a road network.

Figure 2: Connection between DTA and Markov routing game (MRG)

![Image](Papers_Converted/0_Artifacts/[shou2022]_Multi-agent_reinforcement_learning_for_Markov_routing_games-A_new_modeling_paradigm_for_dynamic_traffic_assignment_artifacts/image_000001_4e9f5f6e2d23b1ca14c6a6c0db467652a930fc00455894ffc8636ec4fa693fb5.png)

We now discuss the difference and connection between DTA and the MRG developed in this paper. Deriving a Nash equilibrium in general requires a perfect knowledge of the system evolution dynamics and the rewards of the game (P´ erolat et al., 2017). However, the proposed MRG models individual agents' routing behavior with a partial or unknown information structure and a stochastic model-free environment. In other words, in MRG, agents lack the knowledge of the transition probability of system dynamics or travel time functions of road segments. Thus, agents need to learn through repeated interactions with other agents and the traffic environment. This setting mimics the reality that we usually have no knowledge of how traffic evolves nor a mathematical relation between travel time and congestion. This is the key difference between DTA models and MRG presented in this paper. DTA models stipulate the system evolution dynamics and the travel cost of using each link or route, while MRG does not contain such information and thus requires agents to learn and adapt their strategy according to historical episodes generated from interactions.

Other than the difference between DTA and MRG, we would like to draw their linkage. Markov game is the generalization of MDP-like environment to a multi-agent system. The environment of an agent is comprised of other agents and unknown objects. In other words, stochasticity of the environment arises from the actions of other agents and unknown sources. To generate such an MDP-like environment, we not only need to model all the agents' Markov strategies, but also their interactions with one another. Various DNL models of DTA can provide such an environment. Specifically, there are two components in a DTA, namely a DNL module and a route choice model, and two components in a Markov game, namely an environment and agents. The connection between DTA and Markov game is componentwise, as schematically presented in Figure (2). The DNL in DTA can be used as the environment engine in Markov

game, while the route choices in DTA are made by agents in Markov game. Further, the interaction between the DNL and route choice in DTA is similar to that between the environment and agents in Markov game. In DTA, a route choice model outputs route choices of travelers whenever they arrive at a node or an intersection, and the DNL module propagates traffic flow dynamics. In MRG, agents (i.e., travelers) take actions (i.e., route choices) whenever they arrive at a node, and the environment experiences a state transition determined by the DNL module in DTA. To be precise, the actions taken by agents in Markov game are route choices in DTA, and the state transition and rewards in Markov game are determined by the DNL module in DTA. Moreover, DUE and MRG have non-overlapped regimes that delineate the differences of these two model types: DTA models encompass those with reactive DUE (Li et al., 2000; Lam and Huang, 1995) and are primarily solved using aggregate traffic flow variables; while MRG is focused on en-route dynamic choice of individuals within traffic environments of which transition dynamics and travel time functions remain unknown to agents.

In summary, the MRG coupled with DNL models depicts one type of DTA models, of which the decision-making processes of discrete vehicles (in other words, atomic agents (Roughgarden, 2007)) instead of aggregate traffic flows are modeled, traffic dynamic evolution is modeled as a stochastic transition probability without any need of model specification and in discrete time steps, and the equilibrium outcome is predictive DUE. The detailed comparisons of DTA and MRG are summarized in Table 3.

Table 3: DTA versus MRG

| Model   | Game                  | Traffic dynamics        | Traffic dynamics              | Traffic dynamics   | Traffic dynamics         | Travel Cost         | Travel Cost               | Equilibrium              |
|---------|-----------------------|-------------------------|-------------------------------|--------------------|--------------------------|---------------------|---------------------------|--------------------------|
| Model   | Atomic or non- atomic | Knowledge of dy- namics | State transi- tion            | Functional form    | Temporal resolu- tion    | Knowledge of reward | Form                      | Equilibrium              |
| DTA     | Non- atomic           | Perfect                 | Stochastic or deter- ministic | Model- based       | Discrete or con- tinuous | Known               | Actual or instan- taneous | Predictive or reac- tive |
| MRG     | Atomic                | Imperfect               | Stochastic                    | Model- free        | Discrete                 | Unknown             | Actual                    | Predictive               |

Atomic - Discrete vehicles; Non-atomic - Aggregate traffic flow.

We further specify two components, namely, dynamic network loading and route choice, in DTA and MRG, respectively, using mathematical representations.

DTA: find optimal path flow h t ∗ k , k ∈ K , ∀ t ∈ { 0 , T } .

- · Dynamic network Loading: { N ↑ l ( t +1) = N ↑ l ( ) + t ∑ l ′ ∈ Γ -1 ( ) l y l ′ l ( ) t , N ↓ l ( t +1) = N ↓ l ( ) + t ∑ l ′′ ∈ Γ( ) l y ll ′′ ( ) t , l ∈ L D , 0 ≤ t ≤ N t -1 ,

where N ↑ l and N ↓ l are the cumulative counts of vehicles at the upstream and downstream of link l , respectively. ∑ l ′ ∈ Γ -1 ( ) l y l ′ l and ∑ l ′′ ∈ Γ( ) l y ll ′′ are the inflow and outflow of link l , respectively.

- · Route choice for aggregate vehicle flows:

$$\forall k \in \mathcal { K }, \begin{cases} h _ { k } ^ { t } > 0 \to \pi _ { k } ^ { t } = m i n _ { k } \mathcal { T } ( H ), \\ h _ { k } ^ { t } = 0 \to \pi _ { k } ^ { t } > m i n _ { k } \mathcal { T } ( H ), \end{cases}$$

where, h t k is the traffic flow on path k ∈ K , H is the path flow matrix, T ( H ) is the travel time matrix regarding each path and π t k is the minimum travel time.

Remark. 1. y l ′ l is the transit flow from link l ′ to link l , depending on sending and receiving flow in DNL models, which will be introduced in Section 4.

- 2. Each element τ t k in T ( H ) is the travel time on path k and τ t k = T ↓ k ( N ↑ k ) -T ↑ k ( N ↑ k ). T ↓ k and T ↑ k represent the time when vehicles leave and enter path k , respectively.

MRG: find optimal policies µ , i ∗ i = 1 , · · · , N , ∀ t ∈ { 0 , T } .

- · Environment transition: P ( s ′ | s a , ), where s represents the environment state, i.e., the agent distribution on the network, which determines the cumulative counts of vehicles N ,N ,l ↑ l ↓ l ∈ L D .
- · Route choice for each agent:

$$\left | \quad \forall i \in N, \begin{cases} a _ { i, l } ^ { t } > 0 \to V _ { i } ( s ) = m a x _ { a _ { i } } \mathbb { E } _ { a ^ { - i } \sim A } \mathbb { E } _ { s ^ { \prime } \sim P ( \cdot | s, a ) } [ r _ { i } ( s, \mathbf a, s ^ { \prime } ) + \gamma V _ { i } ( s ^ { \prime } ) ], \\ a _ { i, l } ^ { t } = 0 \to V _ { i } ( s ) < m a x _ { a _ { i } } \mathbb { E } _ { a ^ { - i } \sim A } \mathbb { E } _ { s ^ { \prime } \sim P ( \cdot | s, a ) } [ r _ { i } ( s, \mathbf a, s ^ { \prime } ) + \gamma V _ { i } ( s ^ { \prime } ) ]. \end{cases}$$

where, for agent i , a t i,l is her route choice, indicating whether agent i chooses link l or not. The reward r i ( s, a , s ′ ) is the negative realized travel time after agent i choosing action a i .

## 4. Numerical examples

In this section, we demonstrate the effectiveness of our MF-MA-DQL algorithm to solve optimal routing policies. In Example 4.1, we apply the developed algorithm to a simple network based on a variety of DNL models and compare it with a traditional iterative method. In Example 4.2, we specify one of the DNL models - LTM to explore the spillback phenomenon. In Example 4.3, we apply the developed algorithm to the OW Network (Ortuzar and Willumsen, 2011) with LTM as its data-generation environment. In Example 4.4, our algorithm is applied to a real-world network with traffic simulated in SUMO (i.e., model-free environment). To reiterate, the first three examples contain model-based traffic environments using DNL models, while the last one has a model-free environment in which travel time function and traffic state update are unknown and thus requires agents to learn from their experiences.

Table 4: Numerical examples

|             |   Example | Network            | Dynamic Loading Environment   |
|-------------|-----------|--------------------|-------------------------------|
| Model-based |       4.1 | Simple             | PQ, SQ, CTM, LTM              |
| Model-based |       4.2 | Simple (spillback) | LTM                           |
| Model-based |       4.3 | OW                 | LTM                           |
| Model-free  |       4.4 | Real-world network | SUMO                          |

Example 4.1. In this example, we apply the developed MF-MA-DQL algorithm to a simple network. We first briefly introduce several DNL models used as the environment of MARL.

- 1. PQ: In the point queue (PQ) model, vehicles have no physical length (no spillback) and move at free flow speed. The link traversal time consists of free flow travel time and queuing delay (Zhang et al., 2013).
- 2. M-N: In the M-N model, exit flow is a generalized function of road density (Merchant and Nemhauser, 1978a). When the function satisfies flow capacity constraints, the M-N model becomes a special case of PQ (Nie, 2011). For simplicity, we use the PQ model to demonstrate our algorithm.
- 3. SQ: The different between spatial queue (SQ) and PQ models is that the exit flow in the SQ model depends on the flow capacity and the remaining capacity of downstream links (Zhang et al., 2013).
- 4. CTM: The cell transmission model (CTM) approximately solves the LWR system by discretizing space and time (Daganzo, 1994, 1995). CTM considers not only flow capacity and jam density, but also backward speed into the fundamental diagram.
- 5. LTM: The link transmission (LTM) model exactly solves the LWR system using the NewellDaganzo method (Yperman, 2007). Compared with CTM, LTM reduces computational complexity.
- 6. DQ: The deterministic double queue (DQ) model coincides with LTM (Osorio et al., 2011). For simplicity, we use LTM in this paper.

We then show how to construct the environment of MARL based on DNL models. Note that a link can be discretized into cells such that the length of each cell is exactly the distance traveled by a vehicle within a time interval ∆ t at free flow speed. We assume that the time interval ∆ t is also the time step in the environment of our MARL. To better understand this, we use LTM to illustrate the environment. Environments based on other DNL models are in Appendix A.1.

LTM Environment: LTM is applied to only to the upstream and downstream ends of a link (Yperman, 2007; Osorio et al., 2011). It is a discrete model, calculating the upstream and downstream flow at each time step. At time t , the cumulative counts of vehicles at the upstream and downstream ends of the link are denoted by N ↑ ( ) and t N ↓ ( ), respectively. t The sending flow S t ( ) at the downstream end of the link is calculated based on the free flow speed v , representing how many vehicles can leave during the time interval ( t, t +∆ ), which is calculated as: t

$$S ( t ) = m i n \{ N ^ { \uparrow } ( t - \frac { L } { v } + \Delta t ) - N ^ { \downarrow } ( t ), \ q _ { m a x } \Delta t \},$$

where L represents the link length and N ↑ ( t -L v + ∆ ) represents the upstream cumulative count of t vehicles at time t -L v +∆ . t q max represents the flow rate capacity and q max ∆ is the maximum number t of vehicles that can leave at each time step. The receiving flow R t ( ) at the upstream end of the link is calculated based on the congested wave speed (backward speed) w , representing how many vehicles enter the link during the time interval ( t, t +∆ ), which is calculated as: t

$$R ( t ) = m i n \{ N ^ { \downarrow } ( t - \frac { L } { w } + \Delta t ) + k _ { j } L - N ^ { \uparrow } ( t ), \ q _ { m a x } \Delta t \},$$

where N ↓ ( t -L w + ∆ ) is the downstream cumulative count of vehicles at time t t -L w + ∆ , t k j is the jam density and k L j is the link capacity, representing the maximum number of vehicles that can be accommodated into the link.

Note that the receiving and sending flows only capture the upstream and downstream ends of a link. To determine the flow propagation in the environment, we also need node models to calculate how many vehicles can move from an upstream link to a downstream link. There are some simple node models capturing merging and diverging cases (Daganzo, 1994). However, general intersection with more than one inflow link and one outflow link is not standardized as merge and diverge models. It can be signalcontrolled or priority-controlled. In this paper, we use discharging priorities (Ma et al., 2017) based on agents' route choice at a general intersection node where the inflow links of the node are ranked in some order, representing different priorities. Vehicles on each inflow link may leave based on the link priority and enter outflow links based on their route choice (actions).

With the aforementioned dynamic loading environment, now we apply the developed MF-MA-DQL algorithm to solve DUE where travel cost on all used routes for any given origin-destination pair is supposed to be the same according to Wardrop's first principle. At equilibrium, no individual selfish vehicle could decrease her travel cost by unilaterally deviating from her current route choice strategy. Therefore, the reward for each is simply the negative of her own travel time. In other words, each selfish agent aims to minimize her own travel time. Simple Network

Figure 3: Simple Network

![Image](Papers_Converted/0_Artifacts/[shou2022]_Multi-agent_reinforcement_learning_for_Markov_routing_games-A_new_modeling_paradigm_for_dynamic_traffic_assignment_artifacts/image_000002_8f79e103b85e3cb969a1eebd3a4a07e85a5126eef873c7dea0667a8aa9348057.png)

The simple network used to test the proposed algorithm is shown in Figure 3. The origin and destination are node 1 and 4, respectively. When vehicles reach node 3, they have to make a route choice

between link 3 → A → 4 and 3 → B → 4. ˜ O is the dummy node of origin. We assume that at the beginning, vehicles are on the dummy link ˜ O → 1, representing actual travel demand at the origin node (Ma et al., 2014). ˜ D is the dummy node of destination. All vehicles are on the dummy link 4 → ˜ D after reaching destination. The travel demand at the origin node d t ( ) is d (0) = 50, which means there are 50 vehicles on the dummy link ˜ O → 1 at time 0. The free flow speed is v = 0 2. . Other parameters of this network is shown in Table 5 where link length, flow rate capacity, jam density and backward speed are denoted as L q , max , k j , and w , respectively. Note that SQ, LTM and CTM share the same link length and flow capacity with PQ. LTM and CTM share the same link capacity with SQ.

Table 5: Parameter in Example 4.1

| Link      | PQ   | PQ    | SQ   | LTM/CTM   |
|-----------|------|-------|------|-----------|
| Link      | L    | q max | k j  | w         |
| ˜ O → 1   | ∞    | ∞     | -    | -         |
| 1 → 2     | 0.4  | 20    | 300  | 0.1       |
| 2 → 3     | 0.4  | 20    | 300  | 0.1       |
| 3 → A → 4 | 0.8  | 2     | 60   | 0.1       |
| 3 → B → 4 | 0.4  | 2     | 30   | 0.1       |
| 4 → ˜ D   | ∞    | ∞     | -    | -         |

We make a comparison of the developed algorithm and a traditional iterative method (See Appendix A.2). We plot the convergent performance based on environments of PQ, SQ, LTM and CTM. Convergence plots of both methods for the DUE scenario are presented in Figure 4. The x-axis represents the number of iterations/episodes and the y-axis is the average travel time. In Figure 4a, the iterative method reaches convergence when the average travel times on both links become the same. After 40 iterations, the average travel times on both links 3 -A -4 and 3 -B -4 converge. The converged average travel time for d (0) = 50 is 13 7. . At the equilibrium, there are 20 and 30 vehicles choosing link 3 -A -4 and 3 -B -4, respectively.

For the MF-MA-DQL algorithm, after bouncing back and forth in Figure (4b), average travel travel time gradually reaches its converged value 13 7, which is consistent with the iterative method. . In this case, no spillback happens and travel times in all DNL environments converge to the same value.

Figure 4: Convergence plots for DUE

![Image](Papers_Converted/0_Artifacts/[shou2022]_Multi-agent_reinforcement_learning_for_Markov_routing_games-A_new_modeling_paradigm_for_dynamic_traffic_assignment_artifacts/image_000003_a959b1bae135edf5727c21e8496b7a59b756fbc881d4bed08d5e2e90881e5cf1.png)

Example 4.2. In this example, we investigate a special case on the simple network where queue spillback may happen. The dynamic loading environment we use is LTM. The travel demand at the origin node d t ( ) is d (0) = 90, which means there are 90 vehicles on the dummy link ˜ O → 1 at time 0. The free flow speed is v = 0 2 and the backward speed is . w = 0 1. . The length of link 1 → 2, 2 → 3, 3 → A → 4, 3 → B → 4 are 0.2, 0.2, 0.4 and 0.4, respectively. The flow rate capacity of link 1 → 2 and 2 → 3 is 8. The flow rate capacity of dummy link ˜ O → 1 is 4, which means at each time step, four vehicles can leave

the origin node. The flow rate capacity of link 3 → A → 4 and 3 → B → 4 is:

$$q _ { m a x } = \begin{cases} 2, \, t \leqslant 5 \\ 1, \, t > 5 \end{cases}$$

Convergence plots of both methods for the DUE scenario are presented in Figure 5. In Figure 5a, the iterative method reaches convergence when the average travel times on both links become the same. After 80 iterations, the average travel times on both links 3 -A -4 and 3 -B -4 converge. The converged average travel time for d (0) = 90 is 26.

For the MF-MA-DQL algorithm, after bouncing back and forth during the first 200 episodes in Figure (4b), average travel travel time gradually reaches its converged value 26, which is consistent with the iterative method. At the equilibrium, there are 45 and 45 vehicles choosing link 3 -A -4 and 3 -B -4, respectively.

Figure 6 plots the queue spillback on the network for DUE. The x-axis represents the time and the y-axis represents the queue length. The queue length q ↓ ( ) at each time step is the number of vehicles t in the downstream queue of each link (Ma et al., 2014; Osorio et al., 2011; Yperman, 2007) in the LTM environment, which is calculated as:

$$q ^ { \downarrow } ( t ) = N ^ { \uparrow } ( t - \frac { L } { v } + \Delta t ) - N ^ { \downarrow } ( t ).$$

It is shown that the queue forms on the link 2 -3 when t = 12 due to the limited link capacity of its downstream links 3 -A -4 and 3 -B -4. After that, the queue moves backward and forms on the link 1 -2 when t = 20 due to the limited link capacity of its downstream link 2 -3.





˜√]√}̂˜√

![Image](Papers_Converted/0_Artifacts/[shou2022]_Multi-agent_reinforcement_learning_for_Markov_routing_games-A_new_modeling_paradigm_for_dynamic_traffic_assignment_artifacts/image_000004_09326ea6072afd411a435ecd18e5db6317b9930a354e7b3e57eb76b891da5431.png)

(b) MF-MA-DQL algorithm for DUE

Figure 5: Convergence plots for DUE

![Image](Papers_Converted/0_Artifacts/[shou2022]_Multi-agent_reinforcement_learning_for_Markov_routing_games-A_new_modeling_paradigm_for_dynamic_traffic_assignment_artifacts/image_000005_5f98ab0b5da7f828837e458a77e11c0c10403751b29cdab2206c7cf6e1a03e2d.png)

Figure 6: Queue spillback on the network for DUE

![Image](Papers_Converted/0_Artifacts/[shou2022]_Multi-agent_reinforcement_learning_for_Markov_routing_games-A_new_modeling_paradigm_for_dynamic_traffic_assignment_artifacts/image_000006_db89109bf9746d2cc9641343e83cf85c70cca1a7e4e044f002099119bea4db8b.png)

Example 4.3. In this example, we apply the developed algorithm to the OW network (13 nodes and 24 links) based on the LTM environment. The free flow speed is v = 0 2 and the backward speed is . w = 0 1. . Other parameters of the OW network is shown in Figure 7. The origin is 1 and the destination is 13. The travel demand is d (0) = 20.

Convergence plots of our algorithm and the traditional iterative method are presented in Figure 8. In Figure 8a, the average travel time converges to 5 5 after 40 episodes. . At the equilibrium, there are 9, 1, 4 and 6 vehicles choosing paths 1 -2 -8 -9 -13, 1 -7 -8 -9 -13, 1 -7 -8 -12 -13 and 1 -7 -11 -12 -13, respectively. For the MF-MA-DQL algorithm, after bouncing back and forth during the first 300 episodes in Figure (8b), average travel travel time gradually reaches its converged value around 5 5, which is consistent with the iterative method. .

Figure 8: Convergence plots for DUE

![Image](Papers_Converted/0_Artifacts/[shou2022]_Multi-agent_reinforcement_learning_for_Markov_routing_games-A_new_modeling_paradigm_for_dynamic_traffic_assignment_artifacts/image_000007_c9ab2c4f752c8e271292ba1a450367beaf4e701937908c135619feffd218c9b3.png)

![Image](Papers_Converted/0_Artifacts/[shou2022]_Multi-agent_reinforcement_learning_for_Markov_routing_games-A_new_modeling_paradigm_for_dynamic_traffic_assignment_artifacts/image_000008_56db9935fb4f2ccebe38ab20f8734bb57afe7efce91f5879c18289773bc42f66.png)

Example 4.4. In this example, we apply the developed algorithm to a real-world road network with 69 nodes and 166 links, as presented in Figure (9). This road network covers the area from 133th Street (south) to 146th Street (north) and from Riverside Drive (west) to Convent Avenue (east) in upper Manhattan. In addition to Riverside Drive and Convent Avenue, there are Broadway and Amsterdam Avenue in the north-south direction. The road network is imported into SUMO.

Figure 9: Road network in SUMO

![Image](Papers_Converted/0_Artifacts/[shou2022]_Multi-agent_reinforcement_learning_for_Markov_routing_games-A_new_modeling_paradigm_for_dynamic_traffic_assignment_artifacts/image_000009_955fe61b847766a59ebe3deb4a6fc84c252bdbf9ea8ad81bff2fb79ee5913776.png)

We first briefly introduce the environment. There are two types of vehicles in the road network, namely controllable agents and background traffic. The former actively adapts her route choices while the latter constantly follows some prescribed travel pattern. There are four groups of controllable agents in this case study, namely the first group travelling from node 14 to node 60, the second group from node 15 to node 60, third group from node 48 to node 1, and the fourth group from node 69 to node 1. In total there are 120 controllable agents who enter the road network according to their starting time. On average, there are 12 controllable agents entering the road network every 50 seconds. With respect to the background traffic, there are in total around 1,600 vehicles in the south-north direction and 500 vehicles in the east-west direction within the simulation time period (i.e., 1000 seconds).

The goal of each controllable agent is to minimize her travel cost from origin to destination. Noticing that the first two groups of controllable agents share the same destination, i.e., node 60, these agents thus share the same Q function. We denote this Q function as Q 60 , where the superscript 60 indicates that this Q function is used by agents whose destination is node 60. Obviously, Q value at destination node is 0, i.e., Q 60 ( o i = (60 , t ) , a i , a ¯ ) = 0, regardless of time i t , action a i , and mean action ¯ . a i Similarly, the remaining controllable agents whose destination is node 1 share the same Q function, which is denoted by Q 1 . Q 1 ( o j = (1 , t ) , a j , a ¯ ) = 0. j

Figure 10: MF-MA-DQL algorithm (accident occurs)

![Image](Papers_Converted/0_Artifacts/[shou2022]_Multi-agent_reinforcement_learning_for_Markov_routing_games-A_new_modeling_paradigm_for_dynamic_traffic_assignment_artifacts/image_000010_cd82ee3b75e8cc29616e6aaf11cdbbf18d0a88d34ed7d7f6504783a8bd22ab5d.png)

To demonstrate the effectiveness of the proposed MF-MA-DQL algorithm, we now apply it to the aforementioned setup (i.e., 120 controllable agents and thousands of vehicles as background) with an accident blocking the link connecting node 26 and node 27, starting at t = 180 s and ending at t = 490 . s The convergence plot is presented in Figure (10). The y-axis is the average travel time of all controllable agents and the x-axis is the index of episodes in training. Initially, agents spend a very long time (i.e., more than 900 seconds) on traveling to their destinations. Actually, some agents may not be able to reach their destinations and could record a total travel time of 1,000 seconds (i.e., the maximum allowed time in one episode). During the first 50 episodes, agents explore the road network and learn towards a better policy pretty fast. Therefore, their average travel time is substantially decreased from above 900 seconds to around 600 seconds. Despite some bouncing back and forth between 50 episodes and 300 episodes, the average travel time of all controllable agents stabilizes around 570 seconds after 100 episodes.

Given that there is an accident happening from t = 180 s and lasting for more than 300 seconds, one natural question arises, i.e., whether agents' route choice behavior is changed by this accident. Therefore, we visualize agents' route choices before, during, and around the end of the accident in Figure (11). At t = 80 , i.e., s before the accident, controllable agents going to node 1 mainly choose Broadway instead of other north-south roads (i.e., Riverside drive, Amsterdam avenue, and Convent avenue) due to a comparatively light traffic condition on Broadway. At t = 190 , an accident occurs on s the link connecting node 25 and node 27, and thus no vehicle could reach node 27 from node 25. In other words, no vehicle could go south by entirely staying on Broadway. As one may see in the middle plot of Figure (11), controllable agents have learned the accident and are able to avoid being stuck in traffic by using the adjacent link as a slight detour. The route choice of agents at t = 440 s (i.e., around the end of the accident) show that some controllable agents are now willing to wait in traffic until the traffic accident is resolved, indicating that from the perspective of these agents, detouring may be more costly than waiting in traffic when they have learned that the accident is about to end.

Figure 11: Route choice of controllable agents at different time steps

![Image](Papers_Converted/0_Artifacts/[shou2022]_Multi-agent_reinforcement_learning_for_Markov_routing_games-A_new_modeling_paradigm_for_dynamic_traffic_assignment_artifacts/image_000011_01ba00908db53e6853039598428e9a3b616589a395caf286f1c83aa17a55ad95.png)

We list the computation time of all numerical examples in Table 6. In Example 4.1-4.3, the computation time of the MARL approach is the running time of the training process until agents stabilize their behaviors. The computation time of the fixed point method is the running time of the iterative process until traffic flow reaches a stationary point. It is shown that the computation time of solving the Markov game is longer than that of the fixed point method. The explanation is in the Markov game, each individual agent needs to learn from the traffic environment and update their own policies. In Example 4.4, the computation time is the running time of the training process with traffic simulated in SUMO. Note that the computation time in this example is much longer than others. This is because the simulation is time-consuming. For each episode, the running time of the simulator is set as 20 min. The total time of simulation is 300 · 20 = 10 h .

Table 6: Computation time

|                                                            | MARL                  | Fixed point   |
|------------------------------------------------------------|-----------------------|---------------|
| 4.1 Simple network (50 agents, no spillback)               | 1 min 29s             | 11s           |
| 4.2 Simple network (90 agents, spillback)                  | 6 min 37s             | 57s           |
| 4.3 OW network (20 agents)                                 | 2 min 7s              | 19s           |
| 4.4 SUMO simulation (120 agents, 2100 background vehicles) | 12h (Simulation: 10h) | -             |

## 5. Conclusion

This paper develops a Markov game and multi-agent reinforcement learning that models intelligent agents' learning behavior and the system's equilibrating processes in a routing game among atomic selfish agents. A multi-agent mean field deep Q-learning algorithm is developed to tackle the route choice task of multiple self-interested agents. The proposed mean action, which is defined as the traffic flow on the chosen link, carries partial but not full information of nearby agents and is thus able to partially capture the competition among agents. In addition, the use of the mean action enables Q-value sharing and policy sharing, which is computationally efficient. The developed algorithm is demonstrated in four numerical examples, namely a simple network with various DNL models as its traffic propagation

mechanism, the OW network with LTM, and a large-sized real-world road network with 69 nodes and 166 links implemented in SUMO.

The linkage between the classic DUE paradigm and our proposed MARL paradigm is demonstrated schematically. The difference is that, DTA models stipulate the system evolution dynamics and the travel cost of using each link or route, while the routing game does not contain such information and thus requires agents to learn and adapt their strategy according to historical episodes generated from interactions. Specifically, the developed Markov routing game depicts DUE assuming perfect information and deterministic environments propagated by DNL models. On the simple network without and with spillback, we further show that the numerical solution from the developed MARL algorithm agrees well with the DUE from the iterative method.

Nevertheless, there are several future directions we would like to explore. First, departure time choice and velocity control (Huang et al., 2021) are two travel choices of intelligent agents. The proposed modelfree MARL paradigm will be extended to accommodate departure time choice and velocity control for discrete travelers. Second, in this paper, we assume drivers make decisions when they reach a node. In reality, drivers may make routing choices before they arrive at a node. We would like to incorporate such behavior in future work. Last but not the least, we will develop a bi-level network design problem that optimizes planning or policy countermeasures in the upper level and the adaptive routing behavior of discrete travelers in the lower level.

## Acknowledgements

This work is partially sponsored by the Region 2 University Transportation Research Center (UTRC) (subcontract RUTGER PO 966112/PID#824227) and the National Science Foundation under CAREER award number CMMI-1943998.

## References

Bakker, B., Whiteson, S., Kester, L., Groen, F.C.A., 2010. Traffic Light Control by Multiagent Reinforcement Learning Systems, in: Babuˇ ska, R., Groen, F.C.A. (Eds.), Interactive Collaborative Information Systems. Springer, Berlin, Heidelberg. Studies in Computational Intelligence, pp. 475-510. doi: 10.1007/978-3-642-11688-9\_18 .

- Ban, X.J., Pang, J.S., Liu, H.X., Ma, R., 2012. Continuous-time point-queue models in dynamic network loading. Transportation Research Part B: Methodological 46, 360-380.
- Bazzan, A.L.C., Grunitzki, R., 2016. A multiagent reinforcement learning approach to en-route trip building, in: 2016 International Joint Conference on Neural Networks (IJCNN), pp. 5288-5295. ISSN: 2161-4407.
- Bazzan, A.L.C., Kl¨gl, F., 2008. u Re-routing Agents in an Abstract Traffic Scenario, in: Zaverucha, G., da Costa, A.L. (Eds.), Advances in Artificial Intelligence - SBIA 2008, Springer, Berlin, Heidelberg. pp. 63-72.
- Bhalla, S., Ganapathi Subramanian, S., Crowley, M., 2020. Deep Multi Agent Reinforcement Learning for Autonomous Driving, in: Goutte, C., Zhu, X. (Eds.), Advances in Artificial Intelligence, Springer International Publishing, Cham. pp. 67-78.
- Brown, N., Sandholm, T., 2018. Superhuman AI for heads-up no-limit poker: Libratus beats top professionals. Science 359, 418-424.
- Brown, N., Sandholm, T., 2019. Superhuman AI for multiplayer poker. Science 365, 885-890.
- Chen, C., Wei, H., Xu, N., Zheng, G., Yang, M., Xiong, Y., Xu, K., Li, Z., 2020. Toward A Thousand Lights: Decentralized Deep Reinforcement Learning for Large-Scale Traffic Signal Control. Proceedings of the AAAI Conference on Artificial Intelligence 34, 3414-3421. Number: 04.
- Chen, X., Di, X., 2021. Ridesharing user equilibrium with nodal matching cost and its implications for congestion tolling and platform pricing. Transportation Research Part C: Emerging Technologies 129, 103233.
- Daganzo, C.F., 1994. The cell transmission model: A dynamic representation of highway traffic consistent with the hydrodynamic theory. Transportation Research Part B: Methodological 28, 269-287.
- Daganzo, C.F., 1995. The cell transmission model, part II: Network traffic. Transportation Research Part B: Methodological 29, 79-93.
- Di, X., Ban, X.J., 2019. A unified equilibrium framework of new shared mobility systems. Transportation Research Part B: Methodological 129, 50-78.
- Di, X., Ma, R., Liu, H.X., Ban, X.J., 2018. A link-node reformulation of ridesharing user equilibrium with network design. Transportation Research Part B: Methodological 112, 230-255.
- Di, X., Shi, R., 2021. A survey on autonomous vehicle control in the era of mixed-autonomy: From physics-based to AI-guided driving policy learning. Transportation Research Part C: Emerging Technologies 125, 103008.
- Filar, J., Vrieze, K., 1997. Applications and special classes of stochastic games, in: Competitive Markov Decision Processes. Springer, pp. 301-341.
- Friesz, T.L., Han, K., 2019. The mathematical foundations of dynamic user equilibrium. Transportation research part B: methodological 126, 309-328.
- Friesz, T.L., Han, K., Neto, P.A., Meimand, A., Yao, T., 2013. Dynamic user equilibrium based on a hydrodynamic model. Transportation Research Part B: Methodological 47, 102-126.

- Gawron, C., 1998. An Iterative Algorithm to Determine the Dynamic User Equilibrium in a Traffic Simulation Model. International Journal of Modern Physics C 09, 393-407. Publisher: World Scientific Publishing Co.
- Grunitzki, R., Ramos, G.d.O., Bazzan, A.L.C., 2014. Individual versus Difference Rewards on Reinforcement Learning for Route Choice, in: 2014 Brazilian Conference on Intelligent Systems, pp. 253-258.

Hu, J., Wellman, M.P., 1998. Multiagent Reinforcement Learning: Theoretical Framework and an Algorithm, in: Proceedings of the Fifteenth International Conference on Machine Learning, Morgan Kaufmann Publishers Inc., San Francisco, CA, USA. pp. 242-250.

Huang, K., Chen, X., Di, X., Du, Q., 2021. Dynamic driving and routing games for autonomous vehicles on networks: A mean field game approach. Transportation Research Part C: Emerging Technologies 128, 103189.

- Kim, G., Ong, Y.S., Cheong, T., Tan, P.S., 2016. Solving the Dynamic Vehicle Routing Problem Under Traffic Congestion. IEEE Transactions on Intelligent Transportation Systems 17, 2367-2380. Conference Name: IEEE Transactions on Intelligent Transportation Systems.

Kumar, S., Shah, P., Hakkani-Tur, D., Heck, L., 2017. Federated Control with Hierarchical Multi-Agent Deep Reinforcement Learning. arXiv:1712.08266 [cs] ArXiv: 1712.08266.

- Kuwahara, M., Akamatsu, T., 1997. Decomposition of the reactive dynamic assignments with queues for a many-to-many
- origin-destination pattern. Transportation Research Part B: Methodological 31, 1-10.
- Lam, W.H., Huang, H.J., 1995. Dynamic user optimal traffic assignment model for many to one travel demand. Transportation Research Part B: Methodological 29, 243-259.
- Leibo, J.Z., Zambaldi, V., Lanctot, M., Marecki, J., Graepel, T., 2017. Multi-agent Reinforcement Learning in Sequential Social Dilemmas. arXiv:1702.03037 [cs] ArXiv: 1702.03037.
- Li, J., Fujiwara, O., Kawakami, S., 2000. A reactive dynamic user equilibrium model in network with queues. Transportation Research Part B: Methodological 34, 605-624.
- Li, M., Qin, Z., Jiao, Y., Yang, Y., Wang, J., Wang, C., Wu, G., Ye, J., 2019. Efficient Ridesharing Order Dispatching with Mean Field Multi-Agent Reinforcement Learning, in: The World Wide Web Conference, ACM, New York, NY, USA. pp. 983-994. Event-place: San Francisco, CA, USA.

Lin, K., Zhao, R., Xu, Z., Zhou, J., 2018. Efficient Large-Scale Fleet Management via Multi-Agent Deep Reinforcement Learning, in: Proceedings of the 24th ACM SIGKDD International Conference on Knowledge Discovery &amp; Data Mining, ACM, New York, NY, USA. pp. 1774-1783.

Littman, M.L., 1994. Markov Games As a Framework for Multi-agent Reinforcement Learning, in: Proceedings of the Eleventh International Conference on International Conference on Machine Learning, Morgan Kaufmann Publishers Inc., San Francisco, CA, USA. pp. 157-163. Event-place: New Brunswick, NJ, USA.

- Littman, M.L., 2001. Value-function reinforcement learning in markov games. Cognitive systems research 2, 55-66.

Ma, R., Ban, X.J., Pang, J.S., 2014. Continuous-time dynamic system optimum for single-destination traffic networks with queue spillbacks. Transportation Research Part B: Methodological 68, 98-122.

- Ma, R., Ban, X.J., Pang, J.S., 2017. A link-based differential complementarity system formulation for continuous-time dynamic user equilibria with queue spillbacks. Transportation Science 52.

Mao, C., Shen, Z., 2018. A reinforcement learning framework for the adaptive routing problem in stochastic time-dependent network. Transportation Research Part C: Emerging Technologies 93, 179-197.

- Matignon, L., Laurent, G.J., Fort-Piat, N.L., 2012. Independent reinforcement learners in cooperative Markov games: a survey regarding coordination problems. The Knowledge Engineering Review 27, 1-31.
- Merchant, D.K., Nemhauser, G.L., 1978a. A Model and an Algorithm for the Dynamic Traffic Assignment Problems. Transportation Science 12, 183-199. Publisher: INFORMS.
- Merchant, D.K., Nemhauser, G.L., 1978b. Optimality Conditions for a Dynamic Traffic Assignment Model. Transportation Science 12, 200-207. Publisher: INFORMS.
- Mnih, V., Kavukcuoglu, K., Silver, D., Rusu, A.A., Veness, J., Bellemare, M.G., Graves, A., Riedmiller, M., Fidjeland, A.K., Ostrovski, G., Petersen, S., Beattie, C., Sadik, A., Antonoglou, I., King, H., Kumaran, D., Wierstra, D., Legg, S., Hassabis, D., 2015. Human-level control through deep reinforcement learning. Nature 518, 529-533.

Nguyen, T.T., Nguyen, N.D., Nahavandi, S., 2020. Deep Reinforcement Learning for Multiagent Systems: A Review of Challenges, Solutions, and Applications. IEEE Transactions on Cybernetics 50, 3826-3839. doi: 10.1109/TCYB.2020. 2977374 . conference Name: IEEE Transactions on Cybernetics.

Nie, X., Zhang, H.M., 2005. A comparative study of some macroscopic link models used in dynamic traffic assignment. Networks and Spatial Economics 5, 89-115.

- Nie, Y.M., 2011. A cell-based merchant-nemhauser model for the system optimum dynamic traffic assignment problem. Transportation Research Part B: Methodological 45, 329-342.
- OpenAI, 2018. Openai five. https://blog.openai.com/openai-five/ .
- Ortuzar, J.d.D., Willumsen, L., 2011. Modelling Transport. doi: 10.1002/9781119993308 .
- Osorio, C., Fl¨tter¨d, G., Bierlaire, M., 2011. Dynamic network loading: a stochastic differentiable model that derives link o o state distributions. Procedia-Social and Behavioral Sciences 17, 364-381.
- Palanisamy, P., 2019. Multi-Agent Connected Autonomous Driving using Deep Reinforcement Learning. arXiv:1911.04175 [cs, stat] ArXiv: 1911.04175.

P´ erolat, J., Strub, F., Piot, B., Pietquin, O., 2017. Learning nash equilibrium for general-sum markov games from batch data, in: Artificial Intelligence and Statistics, PMLR. pp. 232-241.

Prasad, A., Dusparic, I., 2019. Multi-agent Deep Reinforcement Learning for Zero Energy Communities, in: 2019 IEEE PES Innovative Smart Grid Technologies Europe (ISGT-Europe), pp. 1-5.

- Puterman, M.L., 1994. Markov Decision Processes: Discrete Stochastic Dynamic Programming. 1st ed., John Wiley &amp; Sons, Inc., New York, NY, USA.
- Ramos, G.d.O., Bazzan, A.L.C., da Silva, B.C., 2018. Analysing the impact of travel information for minimising the regret of route choice. Transportation Research Part C: Emerging Technologies 88, 257-271.
- Roughgarden, T., 2007. Routing games. Algorithmic game theory 18, 459-484.

Seongmoon Kim, Lewis, M.E., White, C.C., 2005. Optimal vehicle routing with real-time traffic information. IEEE Transactions on Intelligent Transportation Systems 6, 178-188. Conference Name: IEEE Transactions on Intelligent

Transportation Systems.

Shou, Z., Di, X., 2020. Reward design for driver repositioning using multi-agent reinforcement learning. Transportation research part C: emerging technologies 119, 102738.

Shou, Z., Di, X., Ye, J., Zhu, H., Zhang, H., Hampshire, R., 2020. Optimal passenger-seeking policies on E-hailing platforms using Markov decision process and imitation learning. Transportation Research Part C: Emerging Technologies 111, 91-113.

Silver, D., Huang, A., Maddison, C.J., Guez, A., Sifre, L., van den Driessche, G., Schrittwieser, J., Antonoglou, I., Panneershelvam, V., Lanctot, M., Dieleman, S., Grewe, D., Nham, J., Kalchbrenner, N., Sutskever, I., Lillicrap, T., Leach, M., Kavukcuoglu, K., Graepel, T., Hassabis, D., 2016. Mastering the game of Go with deep neural networks and tree search. Nature 529, 484-489.

Silver, D., Schrittwieser, J., Simonyan, K., Antonoglou, I., Huang, A., Guez, A., Hubert, T., Baker, L., Lai, M., Bolton, A., Chen, Y., Lillicrap, T., Hui, F., Sifre, L., van den Driessche, G., Graepel, T., Hassabis, D., 2017. Mastering the game of Go without human knowledge. Nature 550, 354-359.

Solan, E., Vieille, N., 2015.

Stochastic games. Proceedings of the National Academy of Sciences 112, 13743-13746.

Stefanello, F., Silva, B.C.d., Bazzan, A.L.C., 2016. Using Topological Statistics to Bias and Accelerate Route Choice: Preliminary Findings in Synthetic and Real-World Road Networks, in: ATT@IJCAI.

Sutton, R.S., Barto, A.G., 1998.

Introduction to Reinforcement Learning. 1st ed., MIT Press, Cambridge, MA, USA.

Vinyals, O., Babuschkin, I., Czarnecki, W.M., Mathieu, M., Dudzik, A., Chung, J., Choi, D.H., Powell, R., Ewalds, T., Georgiev, P., Oh, J., Horgan, D., Kroiss, M., Danihelka, I., Huang, A., Sifre, L., Cai, T., Agapiou, J.P., Jaderberg, M., Vezhnevets, A.S., Leblond, R., Pohlen, T., Dalibard, V., Budden, D., Sulsky, Y., Molloy, J., Paine, T.L., Gulcehre, C., Wang, Z., Pfaff, T., Wu, Y., Ring, R., Yogatama, D., W¨nsch, D., McKinney, K., Smith, O., Schaul, T., Lillicrap, u T., Kavukcuoglu, K., Hassabis, D., Apps, C., Silver, D., 2019. Grandmaster level in StarCraft II using multi-agent reinforcement learning. Nature 575, 350-354.

Yang, Y., Luo, R., Li, M., Zhou, M., Zhang, W., Wang, J., 2018. Mean Field Multi-Agent Reinforcement Learning, in: International Conference on Machine Learning, pp. 5571-5580.

Yperman, I., 2007. The link transmission model for dynamic network loading URL: https://www.researchgate.net/ publication/28360292\_The\_Link\_Transmission\_Model\_for\_dynamic\_network\_loading .

Zhang, H., Nie, Y., Qian, S., 2013. Modelling network flow with and without link interactions: The cases of point queue, spatial queue and cell transmission model. Transportmetrica B: Transport Dynamics 1, 33-51.

Zhou, B., Song, Q., Zhao, Z., Liu, T., 2020. A reinforcement learning scheme for the equilibrium of the in-vehicle route choice problem based on congestion game. Applied Mathematics and Computation 371, 124895.

Ziliaskopoulos, A.K., 2000. A Linear Programming Model for the Single Destination System Optimum Dynamic Traffic Assignment Problem. Transportation Science 34, 37-49. Publisher: INFORMS.

## Appendix A. Appendices

## Appendix A.1.

PQ Environment: The sending flow in the PQ environment is calculated in the same way as LTM. Different from the LTM environment, PQ does not capture the link capacity. Therefore, the receiving flow is only determined by the flow rate capacity:

$$R ( t ) = q _ { m a x } \Delta t.$$

SQ Environment: The SQ model takes the link capacity into consideration. Different from the LTM environment, it does not capture the backward speed. The receiving flow is calculated as:

$$R ( t ) = m i n \{ N ^ { \downarrow } ( t ) + k _ { j } L - N ^ { \uparrow } ( t ), \ q _ { m a x } \Delta t \},$$

CTM Environment: In the CTM environment, each link is divided into cells. The length of each cell is supposed to be the distance traveled by a vehicle at free flow speed v . The sending flow S t ( ) is the number of vehicles which can leave a cell at each time step. It is calculated as:

$$S ( t ) = m i n \{ n ( t ), q _ { m a x } \Delta t \},$$

where n t ( ) is the number of vehicles on the cell at time t . It means the number of vehicles which can leave may not exceed the number of vehicles on the cell. The receiving flow R t ( ) is the number of vehicles which can be accommodated on the cell at time t . It is calculated as:

$$R ( t ) = m i n \{ \frac { w } { v } ( N - n ( t ) ), q _ { m a x } \Delta t \},$$

where N is the cell capacity, representing the maximum number of vehicles on a cell.

## Appendix A.2.

Iterative Method: We employ the iterative method proposed in Gawron (1998) to solve DUE. The basic idea of the iterative method is to let vehicles choose routes according to some policy and then calculate the network loading and travel cost by simulation so that vehicles gradually learn towards better route choices. By iterating this process, a stationary fixed point solution is supposed to be derived so that no vehicle could get better off by switching her route choices. To make this paper self-explanatory, we now briefly introduce the iterative method adapted to the two-node two-link network. Interested readers could refer to Gawron (1998) for more details.

## Algorithm 2 An iterative method to solve DUE

- 1: Initialize a proportion p t (0) indicating the proportion of the demand at t choosing link 0. Consequently, p t (1) = 1 -p t (0)
- 2: Initialize a learning step size η ∈ (0 1] ,
- 3: Initialize function g x ( ) = exp ( ax 1 -x 2 ), where a is a parameter, and function f ( x ) = p t (0) g x ( ) p t (0) g x ( ) + p t (1)
- 4: repeat
- 5: Calculate the time-dependent travel cost of both links, denoted by τ t (0) and τ t (1), from simulation
- τ t (1) -τ t (0)
- 6: Calculate the relative cost difference δ 01 = τ t (1) + τ t (0)
- 7: Update the proportion by p t (0) = (1 -η p ) t (0) + ηf δ ( 01 )
- 8: Calculate p t (1) = 1 -p t (0)
- 9: Update function f ( x ) = p t (0) g x ( ) p t (0) g x ( ) + p t (1)
- 10: until the iterative method reaches a stationary fixed point
- 11: Return p t (0) and p t (1)

The iterative method is listed in Algorithm (2). With demand d t ( ) emerging from node O at time t , we randomly initialize p t (0) and p t (1) to denote the proportion of the demand choosing link 0 and link 1, respectively. We also initialize functions g x ( ) and f ( x ) which will be used to update p t (0) and p t (1). Note that f : [ -1 1] , → [0 , 1] is a monotonic increasing function. With p t (0) and p t (1), we run simulation to calculate the average travel cost on both links for the demand starting at time t and denote these time-dependent travel cost by τ t (0) and τ t (1). We then calculate the relative cost difference τ (1) -τ (0)

δ 01 = t t τ t (1) + τ t (0) and use this cost difference to update the proportion p t (0) towards f ( δ 01 ) by step size η . The rationale of this updating rule is as follows. When δ 01 is large (e.g., close to 1), meaning that the cost on link 0 is much less than that on link 1, we have f ( δ 01 ) ≈ 1, indicating that p t (0) is updated towards a larger value; when δ 01 is small (e.g., close to -1), meaning that the cost on link 0 is much larger than that on link 1, we have f ( δ 01 ) ≈ 0, indicating that p t (0) is updated towards a smaller value. After updating p t (0) and p t (1), we repeat the iterative process until reaching a stationary fixed point.