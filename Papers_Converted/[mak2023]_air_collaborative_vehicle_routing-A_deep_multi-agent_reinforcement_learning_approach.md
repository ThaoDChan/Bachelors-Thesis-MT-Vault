![Image](image_000000_926deef3cbe9e21be70a34c04ebc44984913898e970e7b1a3be19015be39c637.png)

Contents lists available at ScienceDirect

## Transportation Research Part C

journal homepage: www.elsevier.com/locate/trc

## Fair collaborative vehicle routing: A deep multi-agent reinforcement learning approach

Stephen Mak a, ∗ , Liming Xu a , Tim Pearce b,1 , Michael Ostroumov c ,

## Alexandra Brintrup a

- a Institute for Manufacturing, Department of Engineering, University of Cambridge, United Kingdom
- b Microsoft Research Cambridge, United Kingdom
- c Value Chain Lab, United Kingdom

## A R T I C L E I N F O

Keywords:

Collaborative vehicle routing

Deep multi-agent reinforcement learning Negotiation Gain sharing Multi-agent systems

Machine learning

## A B S T R A C T

Collaborative vehicle routing occurs when carriers collaborate through sharing their transportation requests and performing transportation requests on behalf of each other. This achieves economies of scale, thus reducing cost, greenhouse gas emissions and road congestion. But which carrier should partner with whom, and how much should each carrier be compensated? Traditional game theoretic solution concepts are expensive to calculate as the characteristic function scales exponentially with the number of agents. This would require solving the vehicle routing problem (NP-hard) an exponential number of times. We therefore propose to model this problem as a coalitional bargaining game solved using deep multi-agent reinforcement learning, where - crucially - agents are not given access to the characteristic function. Instead, we implicitly reason about the characteristic function; thus, when deployed in production, we only need to evaluate the expensive post-collaboration vehicle routing problem once. Our contribution is that we are the first to consider both the route allocation problem and gain sharing problem simultaneously - without access to the expensive characteristic function. Through decentralised machine learning, our agents bargain with each other and agree to outcomes that correlate well with the Shapley value - a fair profit allocation mechanism. Importantly, we are able to achieve a reduction in run-time of 88%.

## 1. Introduction

Heavy goods vehicles (HGVs) in the UK contributed 4.3% of the UK's total greenhouse gas emissions in 2019 (UK BEIS, 2021). HGVs are utilised inefficiently at 61% of their total weight capacity. Moreover, 30% of the distance travelled carries zero freight (UK DfT, 2020, RFS0125).

Collaborative vehicle routing (CVR) has been proposed to improve HGV utilisation. Here, carriers collaborate through sharing their delivery information in order to achieve economies of scale. If carriers agree to work together, they are said to be in a coalition . As a result of improved utilisation, total travel costs across collaborating carriers can be reduced, resulting in a collaboration gain . The remaining question then is how to allocate this collaboration gain in a fair manner such that carriers are incentivised to form coalitions. An example of CVR is given in Fig. 1.

∗ Correspondence to: 17 Charles Babbage Road, CB3 0FS, United Kingdom.

E-mail address: sm2410@cam.ac.uk (S. Mak).

1 Previously at Department of Computer Science and Technology, Tsinghua University.

## https://doi.org/10.1016/j.trc.2023.104376

Received 26 April 2023; Received in revised form 22 September 2023; Accepted 8 October 2023

Available online 13 October 2023

0968-090X/© 2023 The Authors. Published by Elsevier Ltd. This is an open access article under the CC BY license (http://creativecommons.org/licenses/by/4.0/).

![Image](image_000001_083e6c6ff7a2ea0333c73cabee526d67ede5fdd5b88fdab9a999b93f9e4dd887.png)

![Image](image_000002_8526d009494764269a049c09bc46d771134f45f7b2b855bab171c1245b79f342.png)

![Image](image_000003_cd5c37fcdc0d3ae1efd1033c4e56ff593cbb4365c1297885f3d1c4ab3b6c5cf2.png)

Fig. 1. Three agents (denoted by colours) before and after collaboration. Squares denote depots. Crosses denote customer locations. Node indices (arbitrary) are denoted in black, with costs given in their respective colours. The collaboration gain is defined as the difference in social welfare (or total cost) before and after collaboration. In Fig. 1(b), Agents 1, 2 and 3 all decide to collaborate which reduces the system's total cost by 0.88 (or 26%). This results in a collaboration gain per capita (assuming agents split the gain equally) of 0.29. For detailed calculations, see Section 3.1.

![Image](image_000004_96cb4b0c3bc7def454c9e1b5a775a4b6dfce263233bce09814072b33c10e2fd7.png)

Prior literature suggests that collaborative vehicle routing can reduce costs by around 4-46% and also reduce greenhouse gas emissions and road congestion (Cruijssen et al., 2007a; Zhang et al., 2017; Gansterer and Hartl, 2018b; Pan et al., 2019; Gansterer and Hartl, 2020; Cruijssen, 2020; Ferrell et al., 2020). Sharing resources may also lead to improved resilience to fluctuations in supply and/or demand. Despite these benefits, real-world adoption remains limited, with only a few companies participating (Cruijssen et al., 2007b; Guajardo and Rönnqvist, 2016; Cruijssen, 2020). Currently, a key barrier is the computational complexity of calculating a fair gain sharing mechanism that scales with a larger number of companies. Guajardo and Rönnqvist (2016) recommends that future work should investigate approximate gain sharing methods. Our paper follows this recommendation.

Our first contribution is modelling the collaborative routing problem as a coalitional bargaining game (Okada, 1996) with intelligent agents obtained through the use of deep multi-agent reinforcement learning (MARL). We provide the theoretical grounding in this paper, tying together the fields of collaborative vehicle routing, coalitional bargaining, and deep multi-agent reinforcement learning in order to obtain a theoretically grounded approach that significantly reduces run-time. Here, agents attempt to reach agreement on selecting the 'best' carrier(s) to partner with, and rationally share the collaboration gain amongst the coalition. This bargaining process takes place over multiple rounds of bargaining (see Section 3.3 for a formal definition). A benefit of this approach is that both the routing problem (who should deliver which requests?) and the gain sharing problem (who receives how much of the added value?) are considered simultaneously, whereas a key limitation of many previous methods consider these sub-problems in isolation from one another (Gansterer and Hartl, 2018b). Moreover, our approach is agnostic to the underlying routing problem the complexity of the vehicle routing problem (VRP) formulation can be increased with further constraints such as time windows, without further modification to the method.

Our second contribution is that agents do not need access to the full characteristic function explicitly. To obtain the full characteristic function, the collaboration gain for all possible coalitions must be calculated. In the three-player setting, there are four possible coalitions {1 2 3} {1 2} {1 3} , , , , , , and {2 3} , . Therefore, to obtain the full characteristic function requires solving 2 𝑛 -1 NP-hard post-collaboration VRPs (for a formal introduction, see Section 2.1.3). As a result, methods that require full access to the characteristic function are intractable for settings with more than 6 carriers (Cruijssen, 2020). Instead, our agents can implicitly reason about the characteristic function through only receiving a high-dimensional graph input of delivery information (for example, latitudes and longitudes), as well as other agents' actions. This eliminates the need to fully evaluate the characteristic function when deployed in production, which involves solving the expensive post-collaboration VRP an exponential number of times. Instead, we only need to solve the post-collaboration VRP once when deployed in real-world settings, thus allowing our approach to achieve a significant run-time reduction. In addition, our approach utilises Centralised Training with Decentralised Execution (CTDE) to obtain decentralised agent policies (Lowe et al., 2020). Decentralised policies are desirable in real-world applications as each agent does not necessarily require access to the global, underlying state. This helps ensure that companies' sensitive information will not be leaked to competitors. This also aids to stabilise training in multi-agent settings as well as reduce communication costs. Furthermore, our approach is inductive as opposed to transductive of prior methods. This enables our agents to generalise to agents never seen before during training and thus reduces computational cost.

The remainder of this paper is organised as follows. Section 2 positions our work within the wider context of both collaborative vehicle routing and deep multi-agent reinforcement learning. Section 3 provides a formal introduction to coalitional games, coalitional bargaining and reinforcement learning. Section 4 discusses and justifies various design decisions regarding our agents. Section 5 details our experimental setup, results, discussion and future work. Finally, Section 6 concludes our findings and provides broader managerial implications as a result of this work.

## 2. Related work

## 2.1. Collaborative vehicle routing

Prior collaborative routing literature tackles the partner selection sub-problem (i.e., who should each carrier work with?) by estimating the collaboration gain between different carriers using heuristics (Palhazi Cuervo et al., 2016; Adenso-Díaz et al., 2014). However, a limitation of this approach is that they do not consider how much each agent should be compensated, nor if agents even agree to join the same coalitions (i.e., if the coalitions are stable). Posing this problem as a coalitional bargaining game not only allows us to tackle the partner selection aspect, but we are also able to consider the gain sharing aspect simultaneously as well.

The majority of the collaborative routing literature is concerned with the exchange of individual transportation requests amongst the carriers. This can be divided into three types of planning approaches: centralised; decentralised without auctions; and decentralised with auctions (Gansterer and Hartl, 2018b, 2020).

## 2.1.1. Centralised planning

Centralised planning approaches desire to simply maximise social welfare (the sum of each company's profits). Typically, this goal is achieved by using a form of mixed integer linear programming or (meta)heuristics (Cruijssen et al., 2007a; Gansterer and Hartl, 2018b; Angelelli et al., 2022). This can be viewed as a common-payoff setting, i.e., where all agents are on the same team and receive the same reward. However, assuming a common-payoff setting in practice is unrealistic as companies are self-interested - they mostly care only about their own profits (Cruijssen et al., 2007b). Moreover, there exists fierce competition especially in horizontal collaborations. Therefore, the more realistic setting of decentralised control is needed where agents are modelled to be self-interested.

## 2.1.2. Decentralised planning

There have been few attempts to tackle CVR with decentralised approaches as well. One approach focuses on the problem of partner selection , i.e. ''who should work with whom?''. Adenso-Díaz et al. (2014) proposes an a priori index to estimate the collaboration gain between carriers based on their transportation requests. However, a key limitation is that they do not consider the gain sharing aspect and thus the coalitions formed may not be stable.

A key challenge in decentralised settings is managing the explosion in the number of bundles . Consider Fig. 1 where Agent 2 may desire to sell delivery node 10 to Agent 1. However, if Agent 2 offers both nodes 10 and 11 as a bundle , then Agent 2 may be able to command a higher price. Indeed, the number of possible bundles scales  (2 𝑚 ) where 𝑚 is the number of deliveries. To manage this explosion, a heuristic is typically implemented where agents can only submit or request a few bundles (sometimes only one) which would limit optimality (Bo Dai and Chen, 2009).

A second challenge is to also elicit other agents' preferences over all bundles. One approach is to invoke structure on the problem in the form of combinatorial auctions which aids optimality (Krajewska et al., 2008; Gansterer and Hartl, 2018b; Gansterer et al., 2019; Los et al., 2022). Auctions are where carriers submit requests they do not wish to fulfil to a common pool. Then, other carriers can submit bids on these requests with various methods of determining the ''winners'' of said bids. Combinatorial auctions in these settings allow carriers to bid on bundles of transportation requests instead of individual transportation requests which increases its expressivity and optimality. However, this additional structure comes at additional computational complexity. Moreover, in auction mechanism design, there are four desirable properties: efficiency; individual rationality; incentive compatibility; and budget balance. Gansterer et al. (2019) proposes two auction-based approaches which may be useful in practice, but would be unable to satisfy all four properties simultaneously: there exists a trade-off instead. Los et al. (2022) investigates large-scale carrier collaboration containing 1000 carriers with decentralised auctions. Whilst impressive in scale, their approach ignores the difficulty of large-scale gain sharing.

Both auction-based and non-auction-based approaches may also be exploited by strategic agent behaviour. Would agents intentionally misreport the costs associated with performing deliveries in order to maximise their own profits? Whilst we do not tackle this problem in our work, we believe MARL could be a useful tool to investigate this strategic behaviour in future work.

## 2.1.3. Gain sharing

Whilst gain sharing has been studied in collaborative routing using cooperative game theory (Guajardo and Rönnqvist, 2016), the solution concepts typically assumes that the characteristic function is given. For a set of 𝑛 agents, 𝑁 = {1 … , , 𝑛 } , the characteristic function 𝑣 ∶ 𝟐 𝑁 → R ≥ 0 assigns a value , or in our case collaboration gain , for every possible coalition that could be formed. Note that there exists  (2 𝑛 ) possible coalitions. This is intractable for settings with more than a few agents, because evaluating the collaboration gain of even a single coalition, involves solving a vehicle routing problem which is NP-hard. For detailed calculations of the collaboration gain, see Section 3.1. Guajardo and Rönnqvist (2016) reviews 55 papers from the collaborative transportation

Table 1 Characteristics of selected games studied in MARL.

| Game                                  | > 2-players   | Mixed-Motive   | Known optimum   | Partially observable   |
|---------------------------------------|---------------|----------------|-----------------|------------------------|
| Go (Silver et al., 2016)              | ✗             | ✗              | ✗               | ✗                      |
| StarCraft II (Vinyals et al., 2019)   | ✗             | ✗              | ✗               | ✓                      |
| SMAC a (Samvelyan et al., 2019)       | ✓             | ✗              | ✗               | ✓                      |
| Dota 2 (OpenAI et al., 2019)          | ✓             | ✗              | ✗               | ✓                      |
| Gran Turismo (Wurman et al., 2022)    | ✓             | ✗              | ✗               | ✓                      |
| Football (Kurach et al., 2020)        | ✓             | ✗              | ✗               | ✓ d                    |
| Hide and Seek (Baker et al., 2020)    | ✓             | ✗              | ✗               | ✓                      |
| Communication (Foerster et al., 2016) | ✓             | ✗              | ✓               | ✓                      |
| GCE b (Mordatch and Abbeel, 2018)     | ✓             | ✗              | ✓               | ✓                      |
| SSDs c (Leibo et al., 2017)           | ✓             | ✓              | ✗               | ✓                      |
| Coalitional Bargaining (ours)         | ✓             | ✓              | ✓               | ✗                      |

a StarCraft Multi-Agent Challenge.

b Grounded Communication Environment.

c Sequential Social Dilemmas.

d Both fully and partially observable settings supported.

literature concerning gain sharing. They recommend that a future research direction should focus on developing approximate gain sharing approaches based on cooperative game theory that scales with the number of agents.

In the wider algorithmic game theory literature, coalition formation has also been extensively studied (Chalkiadakis et al., 2011). However, much of the existing literature again assumes that the full characteristic function is given. Alternatively, they aim to find more succinct representations of the characteristic function, typically at a cost of increased computational complexity when computing solution concepts (Chalkiadakis et al., 2011). Examples include Induced Subgraph Games and Marginal Contribution Nets (Deng and Papadimitriou, 1994; Ieong and Shoham, 2005); however, even these succinct representation schemes require evaluating the value of multiple coalitions and thus solving multiple NP-hard VRPs. We argue that many real-world scenarios consist of the characteristic function being a function of the agents' assets or capabilities. In the collaborative routing setting, this is a function of the transportation requests an agent possesses. We therefore ask: ''Can agents form optimal coalitions from the delivery information alone instead of having full access to the characteristic function?'' . Therefore, our paper can be viewed as using an alternative, succinct representation scheme which approximates a rational outcome by using a function approximator.

## 2.2. Deep multi-agent reinforcement learning

Single agent reinforcement learning has seen increasing adoption in supply chain management. However, supply chains can be naturally modelled as a system comprising multiple self-interested agents (Fox et al., 2000; Xu et al., 2021; Brintrup, 2021). For a thorough review of reinforcement learning applied towards supply chain management, see Yan et al. (2022).

Recently, MARL has seen success in playing board and video games such as Go, StarCraft II and Dota 2 (Silver et al., 2016; Vinyals et al., 2019; OpenAI et al., 2019). Whilst these are tremendous feats in the AI space, the underlying games tend to be 2-player and zero-sum. However, most real-world applications, including supply chain management (Gabel and Riedmiller, 2012; Kosasih and Brintrup, 2021), are 𝑛 -player and mixed-motive (with potential 'sequential social dilemmas' [Leibo et al., 2017]). Whilst there is some research in this direction, the majority of MARL research focuses on pure coordination or pure competition settings (see Table 1). Our work is 3-player and mixed-motive which leads to a more challenging joint-policy space, allowing for complex behaviours such as collusion.

The most similar work to ours from a multi-agent learning perspective is that of Bachrach et al. (2020) and Chalkiadakis and Boutilier (2004). In Bachrach et al. (2020), they apply deep MARL to a spatial and non-spatial Weighted Voting Game, where agents are given full access to the characteristic function. In Chalkiadakis and Boutilier (2004), they apply a Bayesian MARL approach to coalition formation as their problem has uncertainty in the characteristic function. In their problem, each agent knows its own capability, but does not observe other agents' capabilities. As a result, they maintain a belief over other agents' capabilities. However, each agents' capabilities remains constant. In our work, each agents' 'capability' can be thought of as the transportation requests it possesses, which constantly changes between episodes. Thus, our agents must be able to generalise across differing agent capabilities.

## 3. Background

## 3.1. Collaborative vehicle routing

We denote the set of 𝑛 agents as 𝑁 = {1 … , , 𝑛 } . A coalition is a subset of 𝑁 , i.e. 𝐶 ⊆ 𝑁 . The grand coalition is where all agents are in the coalition, i.e. 𝐶 = 𝑁 .

Pre-collaboration profit and social welfare : The pre-collaboration profit of Agent 1 in Fig. 2 is calculated as follows: the Revenue is 3 (1 for each delivery); the Cost is 1.42 (sum of the edge distances); thus the Profit is 1.58 (Revenue subtract Cost). Similarly,

Fig. 2. Three agents, Agents 1, 2 and 3 are denoted by the colours green, orange and purple respectively. Squares denote depots. Crosses denote customer locations. Node indices (arbitrary) are denoted in black, with costs given in their respective colours. The collaboration gain is defined as the difference in social welfare before and after collaboration. Figs. 2(b) and 2(c) refer to two possible post-collaboration scenarios with collaboration gains per capita of 0.29 and 0.38 respectively. Thus, it would be rational for the coalition {1 2} , to form instead of the grand coalition {1 2 3} , , .

![Image](image_000005_4b332a409b855e4aa4bea648456ada47aafa0b3946ad7b43b42d53460ab90e17.png)

![Image](image_000006_f24a2b6c799698859150eb604b0b05ce6948619431b77effc2f724737acc761f.png)

![Image](image_000007_c8938ba411ec839c11408b28a3a799d5a994b0917579dc00bdf4b9c311fa65b5.png)

the pre-collaboration profit of Agents 2 and 3 is 2 and 2.07. The pre-collaboration social welfare is the sum of the pre-collaboration profits, thus 1 58 + 2 + 2 07 = 5 65 . . . .

Post-collaboration ''profit'' and social welfare : Assuming agents agree to form the grand coalition 𝐶 = {1 2 3} , , , the postcollaboration ''profit'' of Agent 1 can be calculated as 1 - (0 06 + 0 06) = 0 88 . . . . Note that the post-collaboration ''profit'' for Agent 1 appears to have decreased from 1.58 to 0.88 as a result of collaboration. This will be accounted for when discussing the characteristic function and thus Agent 1 will not lose out when we calculate its reward. For Agents 2 and 3, the post-collaboration ''profit'' is 2.19 and 3.46 respectively. Thus a post-collaboration social welfare of 0 88 + 2 19 + 3 46 = 6 53 . . . . .

Collaboration gain : The collaboration gain is defined as the difference in social welfare before and after collaboration for a given coalition, in this case 6 53 - 5 65 = 0 88 . . . for the grand coalition. Note that the collaboration gain is always greater than or equal to 0. The value per capita is 0 88 . 3 = 0 29 . . During the bargaining process, agents are able to choose how to divide this collaboration gain amongst themselves. In the unique case where agents agree to divide the collaboration gain equally, i.e. according to the value per capita, we refer to this as equal gain sharing . Note that if only Agents 1 and 2 form a coalition (and exclude Agent 3), then the collaboration gain (assuming equal gain sharing) is divided by 2 instead - thus making it rational to object and form the coalition {1 2} , (the value per capita of this coalition is 0.38).

Characteristic function : The characteristic function, 𝑣 ∶ 𝟐 𝑁 → R calculates for every possible coalition the collaboration gain. Importantly, to fully evaluate the characteristic function would require solving a variant of the Vehicle Routing Problem for every possible coalition which scales  (2 𝑛 ) .

Following the example in Fig. 2:

| 𝑣 ({1 , 2 , 3}) = 0 . 88   | Value per Capita = 0 . 88 3 = 0 . 29   |
|----------------------------|----------------------------------------|
| 𝑣 ({1 , 2}) = 0 . 76       | Value per Capita = 0 . 76 2 = 0 . 38   |
| 𝑣 ({1 , 3}) = 0 . 24       | Value per Capita = 0 . 24 2 = 0 . 12   |
| 𝑣 ({2 , 3}) = 0 . 01       | Value per Capita = 0 . 01 = 0 . 005    |

It is important to note that the characteristic function is 0-normalised , essential and super-additive (see Section 3.2 for a formal definition). This guarantees that agents will not lose profits as a result of collaboration. The final take-home profit that each agent (or carrier) receives can then be calculated as the sum of the pre-collaboration profit and its respective allocation of the collaboration gain. For Agents 1, 2 and 3, this would equate to 1 58+ . 0 88 . 3 = 1 87 . , 2+ 0 88 . 3 = 2 29 . and 2 07+ . 0 88 . 3 = 2 36 . respectively (assuming equal gain sharing). In reality, carriers will receive this take-home profit (which is always greater than or equal to the pre-collaboration profit) as an incentive to collaborate.

## 3.2. Coalitional games

We consider the 𝑛 -player coalitional game, also called a cooperative game, with a set of agents 𝑁 = {1 … , , 𝑛 } . A coalition is defined as a subset of N, i.e. 𝐶 ⊆ 𝑁 . The set of all coalitions is denoted 𝛴 . The grand coalition is where the coalition consists of

Fig. 3. Flowchart of the 𝑛 -player coalitional bargaining game (Okada, 1996). Our proposed approach is therefore to obtain a set of intelligent agents that can bargain with each other in a coalitional bargaining game. To achieve a suitable level of agent intelligence, we train our agents using deep multi-agent reinforcement learning.

![Image](image_000008_490ea68476bbc9c7e623d52ec9c698ffb28750e343a6854f03ce6adfdba6df05.png)

all agents in N, i.e. 𝐶 = 𝑁 . A singleton coalition is where the coalition consists of only one agent, i.e. | 𝐶 | = 1 . A coalition structure 𝐶𝑆 = { 𝐶 , 1 … , 𝐶 𝑘 } is a partition of 𝑁 into mutually disjoint coalitions, 𝐶 1 ∪ ⋯ ∪ 𝐶 𝑘 = 𝑁 and 𝐶 𝑖 ∩ 𝐶 𝑗 = ∅ ∀ , 𝑖 ≠ 𝑗 .

A (transferable utility) coalitional game is a pair 𝐺 = ⟨ 𝑁,𝑣 ⟩ . The characteristic function 𝑣 ∶ 𝟐 𝑁 → R ≥ 0 represents the value (or collaboration gain in our setting) that a given coalition 𝐶 receives. Like Okada (1996), we assume that the characteristic function is 0-normalised , essential and super-additive . The characteristic function is 0-normalised if the value of all singleton coalitions is 0, i.e. 𝑣 ({ }) 𝑖 = 0 ∀ , 𝑖 ∈ 𝑁 . It is essential if the value of the grand coalition is strictly positive, 𝑣 𝑁 ( ) &gt; 0 . It is super-additive if 𝑣 𝐶 ( ∪ 𝐷 ) ≥ 𝑣 𝐶 ( ) + 𝑣 𝐷 ( ) for all coalition pairs 𝐶, 𝐷 ∈ 𝛴 where 𝐶 ∩ 𝐷 = ∅ .

The payoff vector 𝐱 𝐶 = ( 𝑥 𝐶 𝑖 ) 𝑖 ∈ 𝐶 denotes the pay-off for player 𝑖 in the coalition 𝐶 . The payoff vector is feasible if ∑ 𝑖 ∈ 𝐶 𝑥 𝐶 𝑖 ≤ 𝑣 𝐶 ( ) . The set of all feasible payoff vectors for a given coalition C is 𝑋 𝐶 , and 𝑋 𝐶 + when all the elements of 𝑋 𝐶 is non-negative.

## 3.3. Coalitional bargaining

The purpose of this work is to find a partition of the 𝑁 carriers with an associated payoff vector, i.e. ( 𝐶𝑆, 𝐱 ) , which all selfinterested, rational carriers agree to. Notice how this does not imply any sequential decision making. However, it was found that certain cooperative solution concepts can be retrieved as the outcome of non-cooperative, extensive form games such as coalitional bargaining (Nash, 1953). Therefore, this necessitates sequential decision making in our problem where we propose to obtain intelligent agents through the use of MARL.

Okada (1996) presents the 𝑛 -player, random proposers, alternating offers coalitional bargaining game which we adopt. At every time-step 𝑡 = 1 2 … , , an agent from 𝑁 is selected uniformly at random to be the proposer . The proposer, player 𝑖 , has two actions the proposed coalition and proposed pay-off vector. The proposed coalition 𝐶 must contain player 𝑖 and the value of the coalition 𝑣 𝐶 ( ) must be greater than 0. Due to the characteristic function being 0-normalised this implies | 𝐶 | ≥ 2 . The payoff vector 𝐱 𝐶 must be in the set of all feasible, non-negative payoff vectors 𝑋 𝐶 + . After player 𝑖 has proposed, the remaining players called the responders are uniformly at random selected sequentially to either accept or reject the proposal. If all agents in the proposed coalition 𝐶 accepts, then those agents form a coalition with the agreed upon proposal. The remaining players outside of 𝐶 continue negotiating from the next time-step. If any responder in 𝐶 rejects the proposal, then all players receive an immediate reward of zero and negotiations go on to the next round of bargaining. Then, a new proposer is selected uniformly at random and the time-step incremented by 1. This continues until either agreement is reached, or the maximum time step is reached. When a proposal ( 𝐶, 𝑥 𝐶 ) is agreed upon at time 𝑡 , every agent 𝑖 in 𝐶 receives a reward of 𝛾 𝑡 -1 𝑥 𝐶 𝑖 , where 𝛾 ∈ [0 1] , is the discount factor. The discount factor decreases the reward received as time passes. This encourages agents to reach agreement within the first time-step in the three-player setting as shown in Okada (1996). The discount factor in this setting is analogous to the patience of an agent, or the urgency of the delivery decision. Any agent who is not in a coalition at the end of this process is assumed to have a reward of zero. In the three-player setting, note that if one proposal is accepted, then no more feasible coalitions can form; thus, this denotes the end of the bargaining process as seen in Fig. 3.

| Table 2                               |                                                                                                                          |
|---------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| Symbol                                | Definition                                                                                                               |
| Coalitional Bargaining Game:          | Coalitional Bargaining Game:                                                                                             |
| 𝑛                                     | Number of Agents                                                                                                         |
| 𝑁                                     | Set of all 𝑛 Agents (i.e., grand coalition)                                                                              |
| 𝑖                                     | Agent index                                                                                                              |
| 𝐶                                     | A Coalition                                                                                                              |
| 𝛴                                     | Set of all Coalitions                                                                                                    |
| 𝐶𝑆                                    | A Coalition Structure                                                                                                    |
| ∅                                     | Empty set                                                                                                                |
| 𝐺                                     | A coalitional game                                                                                                       |
| 𝑣 ( ⋅ )                               | Characteristic function                                                                                                  |
| 𝑣 ( 𝐶 )                               | Value of the coalition 𝐶 , or the collaboration gain of the coalition 𝐶 in the collaborative vehicle routing setting.    |
| 𝐱 𝐶                                   | Payoff vector for a given coalition 𝐶                                                                                    |
| 𝑋 𝐶                                   | Set of all feasible payoff vectors for a given coalition 𝐶                                                               |
| 𝑋                                     |                                                                                                                          |
| 𝐶 +                                   | Set of all feasible, non-negative payoff vectors for a given coalition 𝐶                                                 |
| (Multi-agent) Reinforcement Learning: | (Multi-agent) Reinforcement Learning:                                                                                    |
| 𝛾                                     | Discount factor                                                                                                          |
|                                      | A Markov decision process (MDP)                                                                                          |
|                                      | Set of states                                                                                                            |
| 𝑠 0                                   | Initial state of an episode                                                                                              |
|                                      | Set of (joint) actions                                                                                                   |
|                                      | Transition probability distribution                                                                                      |
| 𝜌 0                                   | Distribution of the initial state, 𝑠 0                                                                                   |
| 𝑎                                     | An action                                                                                                                |
| 𝐺 𝑡                                   | Time-step index Return following time 𝑡                                                                                  |
| 𝑇                                     | Maximum time-step (or the horizon length)                                                                                |
| 𝜋                                     | Agent's policy                                                                                                           |
| 𝑉 𝜋 ( 𝑠 )                             | State-value function of a state 𝑠 following a policy 𝜋                                                                   |
| ̂ 𝑉 ( 𝑠, 𝜃 )                          | Policy's (parameterised by 𝜃 ) estimate of the state-value function given the state 𝑠 .                                  |
| ̂ 𝑄 ( 𝑠, 𝑎, 𝜃 )                       | Policy's (parameterised by 𝜃 ) estimate of the action-value function given the state 𝑠 and taking the action 𝑎 .         |
| 𝑄 ( 𝑠, 𝑎 )                            | Action-value function of a state 𝑠 taking the action 𝑎 following a policy 𝜋                                              |
| 𝜋                                    | Set of all possible rewards                                                                                              |
| 𝑅 𝑖,𝑡                                 | Reward at time 𝑡 for agent 𝑖                                                                                             |
| 𝜃 𝑖                                   | Agent i's policy parameters, usually the parameters of a neural network                                                  |
| 𝐽 ( 𝜃 )                               | Performance measure for the policy 𝜋 𝜃                                                                                   |
| ∇ 𝐽 ( 𝜃 )                             | Column vector of partial derivatives of 𝜋 ( 𝑎 | 𝑠, 𝜃 ) with respect to 𝜃                                                 |
| ̂ 𝑔                                   | Estimate of the policy gradient                                                                                          |
| 𝑀                                     | Number of episodes played in parallel                                                                                    |
| 𝛼                                     | Learning rate for stochastic gradient descent                                                                            |
| 𝑏 ( 𝑠 )                               | A baseline function for policy gradient methods                                                                          |
| 𝑟 𝑡 ( 𝜃 ) 𝜀                           | PPO's probability ratio between the new policy (after gradient updates) and the old policy (before gradient updates) PPO |
|                                       | Threshold to clip the probability ratio in                                                                               |
|                                      | Entropy bonus                                                                                                            |
| Collaborative Vehicle Routing:        | Collaborative Vehicle Routing:                                                                                           |
| 𝐃                                     | Deliveries matrix                                                                                                        |
| 𝑥                                     | x-coordinate of the location                                                                                             |
| 𝑦                                     | y-coordinate of the location                                                                                             |
| 𝑜                                     | Agent index who owns the location                                                                                        |
| 𝑑                                     | Binary variable denoting whether the location is a depot or a customer                                                   |
|                                       | denoting which agents are in the                                                                                         |
| 𝐜 𝐱                                   | Multi-hot encoded vector proposed coalition Proposed pay-off vector                                                      |
| 𝐫                                     | Responses of the agents to the given proposal                                                                            |
| 𝑝                                     | Agent index who was selected to propose in the current round of bargaining                                               |
| 𝑎                                     | Binary variable denoting whether the current agent is proposing or responding                                            |
| Dir ( 𝜶 )                             | Dirichlet distribution with concentration parameters 𝜶                                                                   |

## 4. Methodology

In summary, analytically calculating cooperative game theory solution concepts is intractable for settings with more than 6 carriers (Cruijssen, 2020). Instead, we can recover these cooperative solution concepts through non-cooperative, extensive form games such as coalitional bargaining (Serrano, 2004). However, coalitional bargaining requires intelligent, rational agents and it is difficult to manually craft rule-based agents for collaborative routing due to its exponential and NP-hard nature. Instead, we propose to develop intelligent, rational agents through having agents learn through trial-and-error, learning to collaborate in the presence of multiple other self-interested, rational agents (i.e., multi-agent reinforcement learning). A holistic diagram to depict the whole pipeline can be found in Appendix E, Fig. E.1. The remainder of this section focuses on the reinforcement learning algorithm employed. Pseudo-code of the pipeline can be found in Appendix D. A notation table is provided in Table 2.

## 4.1. Single agent reinforcement learning

Reinforcement Learning (RL) is a subfield of machine learning. Here, the field studies an agent learning what actions to take for a given state in order to maximise a numerical reward . In supervised learning, the ground truth target labels are provided. In RL, we are not told the ''correct'' actions to take that will maximise (expected) cumulative reward. Instead, the agent must learn through trial-and-error. This leads to an exploration-exploitation dilemma. Should the agent try new actions (explore) in the hope that there is a better sequence of actions that leads to an even higher expected reward? Or, should the agent stick with its current best-known actions (exploit) since the agent believes it is unlikely there will be a better sequence of actions with higher expected reward? (Sutton and Barto, 2018). The agent selects actions according to its policy based on the current state. The action is sent to the environment which calculates the reward and next state which is then returned to the agent. Through the learning process, we aim to obtain a policy that maximises the expected cumulative reward.

In our setting of collaborative vehicle routing, the environment is the coalitional bargaining game as described in Section 3.3. Each carrier is represented as an individual agent. The state is the locations of depots and customers, as well as auxiliary features to describe the current state of the coalitional bargaining process - see Section 4.3 for further details. There are three actions that an agent can take depending on if it is proposing or responding. When proposing, the agent must decide (a) which other carriers should the agent propose to partner with, and (b) how much should each carrier in the proposal be paid. When responding, the agent must decide (c) if they accept or reject the proposal. The reward is the collaboration gain the agent is allocated as a result of the coalitional bargaining process. Throughout the training process, we train our agents' policies (or neural network) to maximise expected cumulative reward. See Section 4 for a formal definition of states, actions and rewards in our setting.

We can formalise the problem using Markov decision processes (MDPs) (Puterman, 1994). Formally, a finite-horizon, discounted Markov decision process  can be defined by the tuple  = ⟨   , , 𝑃 , 𝑟, 𝜌 0 , 𝛾 ⟩ where  is the set of states,  is the set of actions,  ∶  ×  →  is the transition probability distribution,  ∶  ×  ×  → R is the reward function, 𝜌 0 ∶  → R is the distribution of the initial state 𝑠 0 , and 𝛾 ∈ [0 1] , is the discount factor.

An episode begins by first sampling an initial state 𝑠 0 from 𝜌 0 . A trajectory ( 𝑠 , 𝑎 0 0 , 𝑠 1 , 𝑎 1 , …) is generated by sampling actions from the agent's policy 𝑎 𝑡 ∼ 𝜋 𝑎 𝑡 ( | 𝑠 𝑡 ) . The next states are obtained by sampling the transition dynamics function 𝑠 𝑡 +1 ∼  ( 𝑠 𝑡 +1 | 𝑠 𝑡 , 𝑎 𝑡 ) until reaching a terminal state. At each time step, a reward 𝑅𝑡 ∼  ( 𝑠 𝑡 , 𝑎 , 𝑠 𝑡 𝑡 +1 ) is received. At timestep 𝑡 , the discounted return, 𝐺𝑡 , is defined as:

$$G _ { t } \preceq R _ { t + 1 } + \gamma R _ { t + 2 } + \gamma ^ { 2 } R _ { t + 3 } + \dots + \gamma ^ { T } R _ { T + 1 } = \sum _ { k = 0 } ^ { T } \gamma ^ { k } R _ { t + k + 1 }$$

where 𝑇 is the maximum time-step and 𝛾 ∈ [0 1] , is the discount factor. As 𝛾 approaches 1, the agent will take into account rewards received far into the future. However, as 𝛾 approaches 0, the agent will only account for the immediate reward 𝑅𝑡 +1 , and the agent is often said to be myopic .

The state-value function of a state 𝑠 under a policy 𝜋 is denoted by 𝑉 𝜋 ( 𝑠 ) . This is the expected return when the agent starts in 𝑠 and continues following its policy 𝜋 . Formally:

$$V _ { \tau } ( s ) \equiv \mathbb { E } _ { \tau } \left [ G _ { 1 } \left | S _ { i } = s \right ] = \mathbb { E } _ { \tau } \left [ \sum _ { k = 0 } ^ { T } \gamma ^ { k } R _ { i * k + 1 } \left | S _ { i } = s \right ], \quad \forall s \in S$$

A similar notion is the action-value function which is denoted by 𝑄𝜋 𝑠, 𝑎 ( ) . This is the expected return when the agent starts from 𝑠 , but also takes the action 𝑎 , and follows its policy 𝜋 afterwards. Formally:

$$Q _ { x } ( s, a ) \pm \mathbb { E } _ { x } \left [ G _ { t } \, | \, S _ { t } = s, A _ { t } = a \right ] = \mathbb { E } _ { x } \left [ \sum _ { L = 0 } ^ { T } \gamma ^ { k } \, \mathcal { R } _ { t + k + 1 } \, | \, S _ { t } = s, A _ { t } = a \right ]$$

## 4.2. Multi-agent reinforcement learning

A stochastic game generalises MDPs to involve multiple agents. This can be defined as a tuple ⟨ 𝑁,𝑆,𝐴,  ,  , 𝛾, ⟩ where:

- · 𝑁 denotes the set of 𝑛 agents
- · 𝐴 = 𝐴𝑖 × ⋯ × 𝐴𝑛 = {( 𝑎 , 1 … 𝑎 𝑛 ) | 𝑎 𝑖 ∈ 𝐴𝑖 for every 𝑖 ∈ {1 … , , 𝑛 }} denotes the set of joint actions, where 𝐴𝑖 is player 𝑖 's set of actions and × denotes the Cartesian product.
- · 𝑆 denotes the set of states including the initial state 𝑠 0
- ·  ∶ 𝑆 × 𝐴 → 𝑆 denotes the transition dynamics
- · 𝛾 denotes the discount factor
- ·  ∶ 𝑆 × 𝐴 × 𝑆 × 𝑁 → R denotes the reward function

For every time-step 𝑡 , an agent 𝑖 ∈ 𝑁 receives an observation of the global state 𝑠 and outputs an action 𝑎 𝑖,𝑡 sampled from its policy 𝜋 𝑖 ( 𝑎 𝑖,𝑡 ∣ 𝑠 𝑡 ) . We update the state 𝑠 𝑡 to include agent 𝑖 's action before sending this new state to agent 𝑗 ∈ 𝑁,𝑗 ≠ 𝑖 . Note that the time-step is not yet incremented. We continue this process until all agents in 𝑁 have submitted their actions to the environment. This yields the joint action 𝐚 = ( 𝑎 , 1 … 𝑎 𝑛 ) . We calculate the reward 𝑅𝑖,𝑡 ∼  ( 𝑠 𝑡 , 𝐚 , 𝑠 𝑡 +1 , 𝑖 ) . We consider the sparse reward setting, i.e., all rewards are zero until the episode terminates. Upon termination, we calculate the reward for agent 𝑖 depending on if agent

𝑖 successfully joined a coalition or not. When a proposal ( 𝐶, 𝑥 𝐶 ) is agreed upon at time 𝑡 , every agent in 𝐶 receives a reward of 𝛾 𝑡 -1 𝑥 𝐶 𝑖 𝑣 𝐶 ( ) . Else, if the agent is not in a coalition 𝐶 , it is assumed to receive a reward of zero. The return 𝐺𝑖 is discounted by a factor 𝛾 ∈ [0 1] , , given by 𝐺𝑖 = ∑ 𝑇 𝑡 =1 𝛾 𝑡 -1 𝑟 𝑖,𝑡 .

Agent 𝑖 's objective is to find a policy 𝜋 𝜃 𝑖 which maximises its expected discounted sum of rewards E [ ∑ 𝑇 𝑡 =1 𝛾 𝑡 -1 𝑅𝑖,𝑡 ] . It is important to note that this maximisation assumes all opponents' policies 𝜋 𝜃 𝑗 ∀ 𝑗 ≠ 𝑖 to be fixed. Thus, one of the key challenges in MARL is the non-stationarity present due to multiple concurrently learning agents.

In our setting, we assume perfect information and thus agents have full access to the global state. We make this assumption as the aim of our paper is to provide the theoretical grounding between collaborative vehicle routing, coalitional bargaining, and multi-agent reinforcement learning. The imperfect information setting is also a promising research direction, e.g., to investigate the value of information. Future work could study the applicability of decentralised partially observable Markov decision processes (dec-POMDPs) (Oliehoek and Amato, 2016) to imperfect information settings in collaborative vehicle routing.

A challenge in reinforcement learning is handling the curses (plural) of dimensionality (Powell, 2022). With ''tabular'' methods, the policy is represented by a lookup table. One curse is that the size of the state space grows exponentially with the number of dimensions (even if the state space is discrete). In our setting, our state space is continuous thus further exacerbating the challenge. As a result, we must resort to function approximation methods (Sutton et al., 2000). Instead, we aim to replace the lookup table with a parameterised model, with parameters 𝜃 ∈ R 𝑑 to map from states to actions. Thus, we can write the policy for agent 𝑖 as 𝜋 𝜃 𝑖 ( 𝑎 𝑖,𝑡 | 𝑠 𝑡 ) instead. Respectively, the state-value function and action-value function can also be re-written ̂ 𝑉 𝑠, ( 𝜽 ) ≈ 𝑉 𝜋 ( 𝑠 ) and ̂ 𝑄 𝑠, 𝑎, ( 𝜽 ) ≈ 𝑄𝜋 𝑠, 𝑎 ( ) . Importantly, the dimensionality 𝑑 of the model is typically much less than the number of states. Changing one parameter will effect the estimated value of many other states. Thus, if we can generalise across states, this could greatly accelerate learning. Note that any parameterised model can be used: a linear function, multi-layer perceptron, decision trees etc. Historically, linear functions were favoured due to favourable convergence guarantees. However, deep neural networks have demonstrated significant success due to their high capacity and generalisability (Sutton and Barto, 2018; Vinyals et al., 2019; Mnih et al., 2015; OpenAI et al., 2019). Thus, we also opt for deep neural networks as well.

Policy gradient -based approaches are a common way to learn a parameterised policy 𝜋 𝜃 𝑖 which maximises an agent's expected discounted return. It is also performant, for example, it achieved great success in playing Dota 2 (OpenAI et al., 2019) amongst others. Typically, a scalar performance measure 𝐽 ( 𝜽 ) is defined and we maximise their performance using approximate gradient ascent: 𝜽 𝑡 +1 = 𝜽 𝑡 + 𝛼 ̂ 𝑡 ∇ ( 𝐽 𝜽 ) where ̂ 𝐽 ∇ ( 𝜽 𝑡 ) ∈ R 𝑑 is a stochastic estimate whose expectation approximates the gradient of 𝐽 𝜃 𝑡 ( ) with respect to 𝜃 𝑡 . However, a challenge is that the performance depends on both the policy's action selection and also the distribution of states where these actions are selected. Varying 𝜃 affects both of these distributions and we typically do not know the effect of our policy on the state distribution. The policy gradient theorem (Sutton et al., 2000; Sutton and Barto, 2018) shows that we can approximate the gradient of performance with respect to 𝜃 but without requiring the derivative of the state distribution. Formally:

$$\nabla J ( \theta ) \, \alpha \, \sum _ { s } \mu ( s ) \sum _ { a } Q _ { s } ( s, a ) \nabla \pi ( a \, | \, s, \theta )$$

The simplest approach is the REINFORCE algorithm (Williams, 1992). Here, an agent plays 𝑀 episodes in parallel until termination and remembers all states, actions and rewards it encountered (or trajectory). Next, it estimates the (undiscounted) policy gradient using:

$$\hat { g } = \frac { 1 } { M } \sum _ { m = 1 } ^ { M } \left [ \sum _ { t = 1 } ^ { T } \hat { A } _ { t } ^ { m } \mathbb { V } _ { o } \log \mathfrak { z } _ { o } ( \alpha _ { t } ^ { m } ) \left | s _ { t } ^ { m } \right ) \right ]$$

where, for REINFORCE ̂ 𝐴𝑡 = ∑ 𝑇 𝑡 ′ = 𝑡 𝛾 𝑡 ′ -𝑡 𝑟 𝑠 ( 𝑚 , 𝑎 𝑡 ′ 𝑚 𝑡 ′ ) . The agent updates its policy using stochastic gradient descent, i.e., 𝜃 ← 𝜃 + 𝛼 ̂ 𝑔 where 𝛼 is the learning rate. The intuition for this policy update is that for each action the agent took for a given state, it will increase or decrease the (log) probability of taking that same action proportional to the discounted return it received during that episode. However, policy gradient methods are notorious for having high variance in the policy gradient. As a result, we employ multiple variance reduction techniques to mitigate this problem, such as 𝑀 parallel environments.

Another variance reduction technique is to subtract a baseline . A baseline 𝑏 𝑠 ( ) can be any function that may or may not depend on the state 𝑠 . Importantly, it must not vary with the action 𝑎 . We can replace REINFORCE's estimate of ̂ 𝐴𝑡 by using ̂ 𝐴𝑡 = [( ∑ 𝑇 𝑡 ′ = 𝑡 𝛾 𝑡 ′ -𝑡 𝑟 𝑠 ( 𝑚 , 𝑎 𝑡 ′ 𝑚 𝑡 ′ ) ) -𝑏 𝑠 ( ) ] instead. It can be shown that introducing a baseline does not introduce bias into the policy gradient, but may significantly reduce variance (Williams, 1992; Greensmith et al., 2004; Sutton and Barto, 2018). An example baseline is the average return an agent received. The term [( ∑ 𝑇 𝑡 ′ = 𝑡 𝛾 𝑡 ′ -𝑡 𝑟 𝑠 ( 𝑚 , 𝑎 𝑡 ′ 𝑚 𝑡 ′ ) ) -𝑏 𝑠 ( ) ] can be thought of as how much better than the baseline an agent performed as a result of choosing its action. A common choice of 𝑏 𝑠 ( ) is to estimate the state-value ̂ 𝑉 𝜋 ( 𝑠 𝑚 𝑡 , 𝜃 ) = E 𝜋 [ ∑ 𝑇 𝑡 ′ = 𝑡 𝛾 𝑡 ′ -𝑡 𝑟 𝑠 ( 𝑚 , 𝑎 𝑡 ′ 𝑚 𝑡 ′ ) | 𝑆𝑡 = 𝑠 ] . Selecting a good baseline is crucial. We discuss our proposed baseline functions in Section 4.7.

In REINFORCE, typically only one gradient update is used per batch of trajectories. As a result, REINFORCE is typically said to be sample inefficient - it requires a lot of episodes to train a performant policy. In addition, REINFORCE can be unstable during training, and sometimes performance collapse may occur as a result of the data distribution changing too drastically.

Proximal Policy Optimisation (PPO) (Schulman et al., 2017) aims to improve the sample efficiency by performing multiple gradient updates to maximise the use of each gathered data point. However, this risks changing the data distribution too drastically and thus risks performance collapse. To rectify this, the intuition behind PPO is to constrain the policy from deviating too greatly. Let the current policy (before any gradient updates) be denoted 𝜋 𝜃 𝑜𝑙𝑑 ( 𝑎 𝑡 | 𝑠 𝑡 ) . After one round of gradient updates, this would yield

Fig. 4. Actors' neural network design. Grey boxes denote state inputs. Blue boxes denote MLP parameters which come from supervised pre-training (see Section 4.6). Note that the linear layer to produce coalition logits is learnt and not pre-trained. White boxes denote learnt parameters. Red boxes denote actions. Numbers in brackets denote the output shapes (ignoring batch size as it is shared by all).

![Image](image_000009_dbcfc84d85c7d8bba49a4e18dac13134edce7dd00ad0b8877d888eae81cb255c.png)

new policy parameters, denoted 𝜋 𝜃 ( 𝑎 𝑡 | 𝑠 𝑡 ) . PPO constrains that the probability ratio, 𝑟 𝑡 ( 𝜃 ) = 𝜋 𝜃 ( 𝑎 𝑡 | 𝑠 𝑡 ) 𝜋 𝜃 𝑜𝑙𝑑 ( 𝑎 𝑡 | 𝑠 𝑡 ) , of taking action 𝑎 𝑡 for the same state 𝑠 𝑡 under the old policy vs new policy to be no more than a certain percentage 𝜀 . This should prevent the risk of policy collapse if 𝜀 is chosen carefully. Moreover, PPO is then able to perform more gradient updates on the same data points, thus greatly improving its sample efficiency. In addition, it is also more stable during training and is less sensitive to chosen hyperparameters. As a result, PPO has been applied to wide range of domains, most notably in OpenAI Five (bots to play Dota 2) (OpenAI et al., 2019) and also in ChatGPT (OpenAI, 2022).

PPO adjusts the neural network parameters 𝜃 to increase or decrease the probability ratio 𝑟 𝑡 ( 𝜃 ) proportional to the advantage the agent received ̂ 𝐴𝑡 . PPO enforces the 𝜀 threshold by clipping the probability ratio, 𝑟 𝑡 ( 𝜃 ) , to remain within ± 𝜀 . We can further encourage exploration by adding an entropy bonus. Thus, the PPO policy gradient can be estimated as follows:

$$\hat { g } \approx \frac { 1 } { M } \sum _ { m = 1 } ^ { M } \sum _ { t = 1 } ^ { T } \nabla _ { \rho } \left [ \min ( r _ { i } ( \theta ) \hat { A } _ { t }, \text{clip} ( r _ { i } ( \theta ), 1 - \varepsilon, 1 + \varepsilon ) \, \hat { A } _ { t } ) + \beta \mathcal { H } [ \tau _ { \rho } ] ( s _ { i } ) \right ]$$

where ̂ 𝐴𝑡 is the baseline, 𝛽 is the entropy regularisation coefficient and  the entropy bonus. An entropy bonus encourages agents to explore rather than exploit. It is important to note that when the advantage is positive, we clip 𝑟 𝑡 ( 𝜃 ) only if it is greater than 1+ 𝜀 . If the advantage is negative, we clip 𝑟 𝑡 ( 𝜃 ) only if it is less than 1 𝜀 (see Figure 1 of Schulman et al., 2017 for further details). The clip function is a function that clips the first argument by the lower and upper bounds denoted by the second and third arguments respectively.

As a result, PPO has been widely used in a range of applications, most notably in OpenAI Five (for Dota 2) and in ChatGPT (OpenAI et al., 2019; OpenAI, 2022).

## 4.3. State space

The agents receive a variety of inputs from the environment as seen in Fig. 4. Let the state at time 𝑡 be denoted by 𝑠 𝑡 ∈ 𝑆 which can be represented by the tuple ⟨ 𝐃 𝐜 𝐱 𝐫 , , , , 𝑡, 𝑝, 𝑎 ⟩ . The deliveries matrix 𝐃 ∈ R 12×4 describes the features of each of the three depots and nine customers, yielding twelve rows where we refer to each row as a location . A location can be represented by the tuple ⟨ 𝑥, 𝑦, 𝑜, 𝑑 ⟩ where 𝑥 ∈ R is the x-coordinate; 𝑦 ∈ R is the y-coordinate; 𝑜 ∈ N denotes the agent who owns the location; and 𝑑 ∈ {0 1} , denotes whether the location is a depot or a customer. For instance, to represent Agent 2's depot located at ⟨ 𝑥 = 0 2 . , 𝑦 = 0 173 . ⟩ , its corresponding row in 𝐃 would be represented as ⟨ 0 2 . , 0 173 2 1 . , , ⟩ and the remaining rows in 𝐃 would be comprised of similar entries for the remaining depots and customers, yielding a shape of 12 × 4. The vector 𝐜 ∈ {0 1} , | 𝑁 | denotes which agents were selected to be in the proposed coalition. The vector 𝐱 ∈ R | 𝑁 | denotes the proposed pay-off vector, the vector 𝐫 ∈ {0 1} , | 𝑁 | denotes the responses of the agents. The vectors 𝐜 , 𝐱 and 𝐫 are initialised to zero if no agent has taken an action in the current round of bargaining. The scalar 𝑡 ∈ N 0 denotes the current round of bargaining, 𝑝 ∈ N denotes which agent was selected to propose in the current round of bargaining, and 𝑎 ∈ {0 1} , denotes whether the current agent is proposing or responding.

## 4.4. Action space

The agents have three action heads: coalitions , proposals and response .

The coalitions action is denoted by 𝐜 ∈ {0 1} , | 𝑁 | where | 𝑁 | is the total number of agents, in this case, 3. Note that in game theory, typically agents propose a coalition of size | 𝐶 | instead of | 𝑁 | . However, it is beneficial to output coalitions in this manner as it keeps the output size constant. The coalitions action denotes whether the respective agent index is part of the coalition 𝐶 . Note that this game assumes that player 𝑖 is in the coalition 𝐜 , i.e., 𝑐 𝑖 = 1 . The deliveries matrix, 𝐃 , is fed through two dense layers with 256 hidden neurons. These parameters come from a supervised pre-training step (see Section 4.6). The output is fed through a linear layer with

| 𝑁 | outputs. These outputs are passed into | 𝑁 | independent Bernoulli distributions to determine the probability that a given agent is in the coalition 𝐶 . A Bernoulli distribution is chosen as the number of outputs required scales linearly with the number of agents. Alternatively, this action can be output auto-regressively, but would be more computationally expensive. It may also be useful to introduce correlation in the agents' actions via more expressive probability distributions which may speed up learning.

The proposals action is denoted by 𝐱 ∈ R | 𝑁 | where ∑ 𝑖 𝑥 𝑖 = 1 , 𝑥 𝑖 ∈ [0 1] , . This vector denotes how much of the collaboration gain is assigned to each respective agent (as a percentage). Note that in game theory, the definition of a feasible pay-off vector is ∑ 𝑖 ∈ 𝐶 𝑥 𝐶 𝑖 ≤ 𝑣 𝐶 ( ) . However, agents will never know the value of 𝑣 𝐶 ( ) a priori (although it can implicitly reason about it). Thus, to practically implement our neural network, we output a vector that is interpreted as percentages as opposed to absolute values. These percentages are then multiplied by the value of a coalition 𝑣 𝐶 ( ) to obtain a feasible pay-off vector.

Note that this is a continuous action space, as opposed to the other actions which are discrete. To parameterise the proposals action head, we use the Dirichlet distribution which is a multivariate generalisation of the Beta distribution. The neural network will output three logits 𝜶 which are used as the concentration parameters of the Dirichlet distribution Dir ( 𝜶 ) . The Dirichlet distribution has support over the probability simplex 𝑆𝐾 = { 𝜽 ∶ 0 ≤ 𝜃 𝑘 ≤ 1 , ∑ 𝐾 𝑘 =1 𝜃 𝑘 = 1} (Murphy, 2021). Intuitively, agents will propose an equal gain share with high probability if the inputs to the Dirichlet are large and equal. Agents will make proposals uniformly at random within the probability simplex if the inputs to the Dirichlet are small and equal, but greater than 1. If Agent 1 wanted to collaborate with Agent 2 but not 3, the input to the Dirichlet could be ⟨ 10000 10000 1 001 , , . ⟩ . This would result in approximately a 50/50 split between Agents 1 and 2 with high probability.

The Dirichlet distribution is appealing due to two reasons. Firstly, the proposals vector requires that it sums to 1 which matches the form of the Dirichlet distribution. Secondly, the Dirichlet distribution has finite support. In continuous action spaces, a Gaussian distribution is typically used which has infinite support and can lead to bias (Chou et al., 2017). Chou et al. (2017) overcomes this issue by using a Beta distribution instead as it has finite support and find that their agents learn more efficiently.

To calculate the proposals, the state inputs are passed through a variety of dense layers (see Fig. 4) to produce an embedding. A linear layer with | 𝑁 | output neurons is applied to the embedding. As in Chou et al. (2017) we add 1.001 to the output logits to ensure the resultant Dirichlet distribution remains unimodal. As a result, during evaluation the agents can fully exploit by proposing the mode of the distribution, instead of having to sample from the Dirichlet which may involve exploration. The output logits are then masked by the coalitions vector, i.e. if a player 𝑖 is not in the coalition 𝑆 , its corresponding output logit will be 1.001. Finally, to calculate the pay-off vector, we sample from the Dirichlet distribution with the masked output logits.

The response action 𝑟 ∈ {0 1} , denotes whether an agent accepts or rejects a given proposal. It takes the resultant embedding followed by a single linear layer with one output neuron. The output is then fed through a Bernoulli distribution.

Whilst we have chosen to use Bernoulli and Dirichlet distributions to parameterise the three action spaces, it may be beneficial to experiment with more expressive probability distributions or e.g. output actions auto-regressively. This may speed up learning and would be an interesting line of future research.

## 4.5. Reward function

Our reward function is sparse, i.e., at timestep 𝑡 the agents will always receive an immediate reward 𝑅𝑡 of zero until the coalitional bargaining game terminates. Upon termination, we calculate a reward for each agent.

If agent 𝑖 successfully joins a coalition 𝐶 by having all agents in 𝐶 accept the proposal, then it receives a reward of 𝑟 𝑖,𝑡 = 𝑣 𝐶 ( ) ⋅ 𝑥 𝑖 where 𝑣 𝐶 ( ) is the collaboration gain obtained by coalition 𝐶 , and 𝑥 𝑖 is the 𝑖 th element of the pay-off vector 𝐱 . For clarity, if agent 𝑖 is the proposer and has its proposal rejected by the responder agents, it will receive an immediate reward of zero. However, there is potential for agent 𝑖 to obtain more than zero immediate reward in future rounds of bargaining and thus the discounted return can still be greater than zero.

Else, if agent 𝑖 does not successfully join a coalition 𝐶 by the end of the episode, then it will receive a terminal reward of zero.

## 4.6. Transfer learning

A key challenge with policy gradient approaches is its sample inefficiency, even in single agent settings. This is further exacerbated due to the non-stationary learning dynamics imposed by having multiple agents learn concurrently. In typical RL settings, agents learn ''tabula rasa'', i.e., without any prior knowledge. Whilst this is mathematically elegant, learning tasks tabula rasa for problems with high complexity, such as in real-world, multi-agent settings, is rare (Agarwal et al., 2022). Instead, it may be preferable to pre-train on some offline dataset in order to learn a good feature extractor. For example, Silver et al. (2016) and Vinyals et al. (2019) pre-train their networks on human gameplay data in a supervised learning setting before using RL. This idea of transfer learning, or recently, reincarnating RL (Agarwal et al., 2022) is well accepted in the RL literature and the reader is referred to Taylor and Stone (2009) and Agarwal et al. (2022) for a thorough review. Furthermore, transfer learning is well accepted in supervised learning, especially in the computer vision and natural language processing domains leading to the likes of ChatGPT (OpenAI, 2022). In our case, the pre-training process aids in efficiently initialising the agents' policies and facilitates faster convergence in the MARL framework.

We therefore pre-train our agents to learn a good feature extractor in a supervised learning fashion. We hypothesise that a good feature extractor should be able to predict whether a given coalition for a given state is productive or not. As a result, we create a dataset of one million instances and randomly select a feasible coalition per instance and calculate the social welfare obtained. Next, we train a neural network to predict the social welfare for a given state and coalition. We optimise the neural network to

Fig. 5. Pre-trained neural network design. Grey boxes denote state inputs. White boxes denote learnt parameters. Red box denotes the output, which predicts the collaboration gain for this given state and coalition. Numbers in brackets denote the output shapes (ignoring batch size as it is shared by all).

![Image](image_000010_9e736036e76e16dd983380472f38563ea4599701b64324c57b37af81f7c3528f.png)

minimise the mean squared error. We split the dataset using an 80/20 train/test split. The neural network design can be seen in Fig. 5. We experimented with different neural network architectures but found this architecture performed best. Whilst this is not the exact task agents must perform in the collaborative routing scenario, the intuition is that the neural network should still learn useful patterns which are transferable to the full collaborative routing problem.

## 4.7. Policy gradient baselines

As discussed in Section 4, a useful baseline helps reduce the variance in policy gradient methods. We use two types of baselines: one for the response action (when agents are responding); and one shared for both the coalitions and proposals actions (when agents are proposing). The neural network architectures for the baselines can be found in Fig. 6.

The response action is discrete and thus we can easily implement Counterfactual Multi-Agent Policy Gradients (COMA) (Foerster et al., 2017). They use the following baseline:

$$A ^ { i } ( s, \mathfrak { a } ) = Q ( s, \mathfrak { a }, i ) - \sum _ { d ^ { \prime } } \tilde { x } ^ { i } ( a ^ { i ^ { \prime } } | t ^ { \prime } ) \hat { Q } ( s, ( \mathfrak { a } ^ { - i }, a ^ { \prime } ), i, \phi )$$

$$a ^ { \prime _ { 1 } }$$

where 𝑄𝜋 𝑠, ( 𝐚 , 𝑖 ) = E 𝜋 [ ∑ 𝑇 𝑡 ′ = 𝑡 𝛾 𝑡 ′ -𝑡 𝑟 𝑠 ( 𝑚 , 𝑎 𝑡 ′ 𝑚 𝑡 ′ ) | 𝑠, 𝐚 ] is the discounted return received if all agents take the joint action 𝐚 in state 𝑠 . The estimate comes from a function approximator with parameters 𝜙 . 𝑎 𝑖 ′ is the other actions agent 𝑖 could have taken. 𝜏 𝑖 is the prior trajectory agent 𝑖 has observed. ̂ 𝑄 𝑠, ( ( 𝐚 -𝑖 , 𝑎 ′ 𝑖 ) , 𝑖, 𝜙 ) is the estimated discounted return agent 𝑖 would receive if it took a different action 𝑎 𝑖 ′ whilst keeping the other agents' actions 𝐚 -𝑖 constant. This is estimated through the use of a neural network with parameters 𝜙 . was relative to any other In our case, the question we ask is: if an agent has agreed to a given proposal, could it have

Intuitively, the COMA baseline can be thought of as how much better agent 𝑖 's decision to take action 𝑎 action agent 𝑖 could have taken, 𝑎 𝑖 ′ . done better by rejecting instead, assuming other agents' actions remain the same?

In the discrete setting, it is easy to sum over all other actions agent 𝑖 could have taken. However, with continuous actions using Dirichlet distributions in the proposals action, this can be difficult. Therefore, we instead estimate the state-value which estimates the expected discounted return conditioned on the state 𝑠 . We denote this baseline with ̂ 𝑉 𝜋 ( 𝑠, 𝑖, 𝐰 ) = E 𝜋 [ ∑ 𝑇 𝑡 ′ = 𝑡 𝛾 𝑡 ′ -𝑡 𝑟 𝑠 ( 𝑚 , 𝑎 𝑡 ′ 𝑚 𝑡 ′ ) | 𝑠 ] where 𝐰 is the parameters of a function approximator such as a neural network. Thus, our baseline for both the coalitions action and proposals action is given by:

$$A ^ { i } ( s, \mathfrak { a } ) = Q _ { g } ( s, \mathfrak { a }, i ) = \hat { \mathcal { V } } _ { x } ( s, i, \mathfrak { w } )$$

$$A ^ { i } ( s, \mathbf a ) = Q _ { \pi } ( s, \mathbf a, i ) - \hat { V } _ { \pi } ( s, i,$$

Finally, we normalise 𝐴 𝑖 ( 𝑠, 𝐚 ) by subtracting the mean and dividing by the standard deviation due to the small magnitude in rewards.

## 4.8. Time limits

It is crucial to deal with time limits properly in this setting. The full coalitional bargaining game presented in Okada (1996) is infinite horizon, i.e., negotiation could go on indefinitely. Clearly, this is impossible to simulate on a finite computer and we must set a maximum number of rounds. Nevertheless, it is still possible to optimise for the infinite horizon, but care must be taken as shown in Pardo et al. (2018). They argue that if an episode terminates only due to reaching the maximum number of rounds, we should bootstrap the discounted estimated value of the next state, ̂ 𝑣 𝜋 ( 𝑠 ′ ) . Thus, if agents reach agreement within the maximum number of rounds, they should receive a reward 𝑟 as expected. However, if they exceed the maximum number of rounds, they should receive

Fig. 6. Grey boxes denote state inputs. Blue boxes denote MLP parameters which come from supervised pre-training (see Section 4.6). White boxes denote learnt parameters. Red boxes denote outputs for the baseline. Numbers in brackets denote the output shapes (ignoring batch size as it is shared by all).

![Image](image_000011_a794342ee8c045ea93ffb7a9bf28158f2bf945a747f8b561bf0965b5c89cb704.png)

![Image](image_000012_e94f55ea7273e21b98380a91f1e11e2da6e7d8d22e8e895d20e58a76b1c52a5e.png)

a reward of 𝑟 + 𝛾 ̂ 𝑣 𝜋 ( 𝑠 ′ ) . In our setting with the maximum number of rounds equal to 10, if agents do not reach agreement within 10 rounds, we fictitiously step them into the next state 𝑠 ′ , at round 11 with proposers selected uniformly at random. If player 𝑖 is not selected as a proposer, then the selected proposer is asked to propose a coalition and pay-off vector ( 𝑆, 𝐱 ) in this fictitious round. We then use a critic to estimate the value of this state, ̂ 𝑣 𝜋 ( 𝑠 ′ ) .

## 4.9. Skill retention

Okada (1996) shows that agents should reach agreement with no delay in agreement. Therefore, as agents learn to collaborate better, they will reach agreement sooner, which is beneficial due to the environment's discount factor. However, this may lead to agents forgetting how to play the game at later time-steps. To enable retention of skills at later time-steps, we employ a targeted training design. During training, instead of starting all bargaining games at round 1, we uniformly at random start them between round 1 and the last round of bargaining, 𝑇 - 1 . Therefore, agents will always be exposed to a range of bargaining scenarios even if agents are collaborating optimally.

## 5. Experiments

## 5.1. Problem setting

We base our problem setting on a modified version of Gansterer and Hartl (2018a). We consider an environment with three companies, each represented by an agent. Each agent has one depot and three customers that it must deliver to. The depot ( 𝑥, 𝑦 ) locations for each agent are held fixed at {(-0 2 0 173) (0 2 0 173) (0 -0 173)} . , . , . , . , , . respectively. The depots' service radius for each instance is selected uniformly at random from the set {0 3 0 4 0 6} . , . , . . This is visualised in Fig. 7. The rationale by Gansterer and Hartl (2018a) is that, through varying the depots' service radius, this varies the degree of overlap and thus competition (or collaboration

Fig. 7. A plot of the distribution of depot and customer locations. Depots are denoted by squares. Each depot has three distinct service radii which are selected uniformly at random. Customers may be uniformly at random located within any of corresponding depot's service radius.

![Image](image_000013_73ae7c409a97293865163eaf7a399db60ba1417188d423ce4193e645bd4b5d5e.png)

opportunity) between carriers. A high degree of overlap using a radius of 0.6 creates high collaboration opportunity between carriers. A low degree of overlap using a radius of 0.3 has low collaboration opportunity between carriers. With a small radius of 0.3, this can analogously be seen as the scenario when depots do not lie in close proximity to each other. The customers locations are then generated uniformly at random with the depot's service radius.

To calculate the pre-collaboration and post-collaboration gains, the shortest paths are calculated exactly using Gurobi (Gurobi Optimization, LLC, 2021). The pre-collaboration shortest paths can be calculated by solving three (un)Capacitated Vehicle Routing Problems (one for each agent). The post-collaboration shortest paths are calculated by solving a single multi-depot vehicle routing problem. Problem formulations for the capacitated VRP and multi-depot VRP can be found in Appendices A and B respectively. Capacity is effectively removed by setting the capacity of each vehicle to an arbitrarily large number and the weight of each delivery to 1.

Whilst this problem setting is rather simplistic, this is important as it allows us to evaluate our agents rigorously. To calculate optimal solutions (for evaluation purposes only), we must brute force the characteristic function. This is expensive and only possible for small, simple VRPs and 3 agents.

## 5.2. Experimental design

We perform 10 independent runs with different random seeds to train our agents. Agents are trained for 10,000 epochs and evaluated every 100 epochs. Agents are evaluated on instances it has never seen before in training. We train using a batch size of 2048 and evaluate with a batch size of 2048. All agents use a discount factor 𝛾 of 0.95. All agents' observations are normalised with a running estimate of the mean and standard deviation. The maximum number of bargaining rounds 𝑇 is set to 10. The learning rate was held constant at 3×10 -4 and we use Adam optimisation. We clip the global norm of gradient updates if they exceed 1. We use 𝜀 = 0 05 . to clip the probability ratios in PPO as it seems to help stability in Yu et al. (2021). All code to generate results is run on the Wilkes 3 high performance computing cluster with AMD EPYC ™ 7763 64-Core Processors and NVIDIA A100 GPUs. Note we only use a supercomputer to perform runs in parallel. Training takes approximately 8 h per run.

## 5.3. Evaluation

## 5.3.1. Correlation with the Shapley value

The objective of our work is to find a partition of the 𝑁 carriers with an associated fair pay-off vector. We emphasise that certain cooperative solution concepts (e.g. Shapley values) can be retrieved as the outcome of non-cooperative, extensive form games (e.g. coalitional bargaining as in our work). The Shapley value is the most common gain sharing mechanism used in the collaborative vehicle routing setting (Guajardo and Rönnqvist, 2016) as it is widely accepted in game theory to be fair - each agent gets paid proportional to their marginal contribution. In addition, it is also guaranteed to be unique. We believe that both of these arguments would help transportation planners to reach agreements better, in line with Krajewska et al. (2008). Thus, we compare the outcomes that our MARL agents agree to with the Shapley value for each instance by measuring the correlation, mean absolute error, and mean squared error.

## 5.3.2. Baseline bots

We compare our MARL agents against two rule-based bots as a baseline. The heuristic bot always proposes the grand coalition with equal gain share and always accepts every proposal. The random bot proposes coalitions and gain shares as well as responses all uniformly at random. These two bots help us to understand that (a) our MARL agents are learning interesting, complex behaviours, and (b) our experimental setup is not too easy in design and that simple, intuitive policies are not sufficient for this setting.

## 5.3.3. Accuracy

A simple evaluation metric is to measure how often the agents propose the correct coalition. For player 𝑖 , the correct coalition 𝐶 𝑖 ∗ is defined to be the coalition 𝐶 which would maximise player 𝑖 's reward. This involves brute forcing the characteristic function to evaluate the value of each possible coalition which is only possible since we consider 3 agents. We emphasise that brute force is only required to evaluate our agents - brute force is not required to train the agents. The reward 𝑅 is the collaboration gain from agreeing to coalition 𝐶 , 𝑣 𝐶 ( ) , multiplied by the 𝑖 th element of the pay-off vector, 𝑥 𝑖 .

## 5.3.4. Optimality gap

We denote the absolute and relative optimality gap of player 𝑖 by 𝜙𝑖 and 𝜂 𝑖 respectively. The absolute optimality gap 𝜙𝑖 for player 𝑖 is defined as 𝜙𝑖 = 𝑣 𝐶 ( ∗ 𝑖 ) -𝑣 𝐶 ( ) , where 𝐶 𝑖 ∗ is the correct coalition, 𝐶𝑖 is player 𝑖 's proposed coalition, and 𝑣 ( ) ⋅ is the characteristic function (i.e. the collaboration gain of a given coalition). The relative optimality gap 𝜂 𝑖 , is calculated as:

$$\eta _ { i } = \frac { v ( C _ { i } ^ { \prime } ) - v ( C _ { i } ) } { v ( C _ { i } ^ { \prime } ) }$$

$$\frac { \stackrel { \ast } { \gamma _ { i } } \right ) - v ( \cdot ) } { v ( C _ { i } ^ { * } ) }$$

Since the data is randomly generated, there could be scenarios where there is no value in collaborating, i.e. even the value of the grand coalition is 0, 𝑣 𝑁 ( ) = 0 . Note that we exclude these scenarios when calculating the above evaluation metrics; however, this only occurs 1.9% of the time when brute-forcing 51,200 instances.

## 5.3.5. Other checks

Okada (1996) analyses this coalitional bargaining game in a non-collaborative routing setting, and proves that agents will cooperate by sharing gains equally in our setting. Therefore, in addition to the above metrics, we check that the agents' behaviour agrees with those predicted by Okada (1996). Firstly, we check that agents do converge to an equal gain share. Secondly, all agents should reach agreement in the first time-step in the three-player setting.

## 5.4. Results

We perform ten independent runs comparing our RL bot to the heuristic bot. Ten independent runs in (MA)RL is commonly accepted following the work of Henderson et al. (2019). We also compare to a random bot which simply proposes coalition structures and pay-off vectors as well as responds all uniformly at random.

From Figs. 8(a) and 8(b), we conclude that our agents have learnt close to optimal behaviour. Our agents reach an average accuracy of 77% and average optimality gap of 0.01 (or 3.9%). Moreover, we can see from Fig. 9(a) that Agent 1 learns to share gains equally - as expected by game theory (Okada, 1996). Whilst we only show the plot for Agent 1, similar plots can be made for Agents 2 and 3 but are omitted due to space constraints. Interestingly, three 'phases' of learning are identified as shown in Fig. 9(b). In Phase 1 (the first approximately 300 epochs), proposers act extremely myopically and propose that they receive the majority of the gain (up to 90%). Occasionally, the responders will accept these sub-optimal proposals and thus the proposer could receive high reward. However, the responders learn to reject more proposals so that they can potentially counter-offer in the next round. This leads to more rounds of bargaining. After about 300 epochs, both proposers and responders reach agreement quickly; however, the gains are not equally shared. In Phase 2, responders realise they can do better by rejecting proposals and potentially proposing counter-proposals. This drives the proposers to propose more equal gain shares. Finally, in Phase 3, we can see that agents have learnt to maximally cooperate with equal gain share and reach agreement within the first time-step as expected by Okada (1996).

## 5.4.1. Correlation with Shapley values

In Fig. 10, we see that the outcomes from our bargaining procedure correlate well with the calculated Shapley values. The three agents receive an 𝑅 2 score of 0.76, mean squared error of 0.08, and mean absolute error of 0.01 (averaged across all three agents). In addition, it is promising that when agent 1 is excluded from the coalition (denoted by orange cross markers), this is usually when Agent 1 has low marginal contribution (as seen by the orange kernel density estimate plot at the top of the x-axis). As a result, we conclude that our agents learn to agree to fair outcomes. This is important from a managerial perspective as fairness could be crucial to help incentivise carriers to participate in collaborative vehicle routing (Guajardo and Rönnqvist, 2016).

## 5.4.2. Ablations

We further perform two ablations to strengthen the confidence in our findings. Each ablation is carried out with 10 random seeds each. The first ablation changes the maximum number of bargaining rounds from 10 to 30. This ablation is carried out since the underlying coalitional bargaining game is infinite-horizon, yet we must set a maximum number of bargaining rounds. Our ablation shows that increasing the maximum number of time-steps does not significantly change the quality of our agents' solutions. The agents still agree to share gains equally, with an average optimality gap of 4.1% (up from 3.9%) and identifying the correct coalitions 76% of the time (down from 77%). Therefore, we conclude that using a maximum number of time-steps of 10 to be sufficient. This is expected as we deal with time-limits properly as discussed in section Section 4.8. The second ablation changes the agents' discount factor from 0.95 to 1.0. This ablation is carried out as we use a discount factor to reduce variance in the return. We test whether it is possible to use a higher discount factor. We find that using a discount factor of 1.0 decreases performance which we suspect to be due to the increased variance. With a discount factor of 1.0, agents achieve an average optimality gap of 6.07% (up from 3.9%). Agents do still learn to propose an approximately equal gain share but identifies the correct coalitions only 68% of the time (down from 77%). We conclude that using a discount factor of 0.95 is sufficient to achieve a set of strong agents.

Fig. 8. Learning curve of (a) average accuracy (b) average optimality gap across all 3 agents for readability. Solid lines denote mean accuracy across 10 independent runs. Shaded regions denote ± two standard deviations. After training for 10,000 epochs, our RL agents reach an average accuracy of 77% with an average optimality gap of 3.9%.

![Image](image_000014_7097f47dab6fd791147f26f4cc86832229bc70c3739461e6bb9c74e03a5592cd.png)

![Image](image_000015_674dfdc15b16d725b43c296c0d8187b233ecdcee2bb6adf1d9aaed4a38b5381e.png)

## 5.5. Discussion

In addition, our RL agents are able to reach agreement in 512 parallel instances within an average of 3.0 s (or 0.006 s per instance). We note that the prior literature assumes full access to the characteristic function, such as Krajewska et al. (2008). Using these prior methods to solve 512 instances takes 24.3 s (or 0.047 s per instance). Thus, our RL agents achieve a 88% reduction in computational time when compared with prior methods to calculate the Shapley value, such as in Krajewska et al. (2008). Whilst 0.047 s per instance may seem reasonable even with traditional methods, we stress that this is due to the simplistic VRP setting we consider - prior methods will not scale with the number of agents nor problem complexity via additional constraints such as time-windows. Importantly, our agents agree to outcomes that correlate well with Shapley values and thus we conclude that our method produces fair outcomes. This is important to fairly compensate carriers to enable wide-spread industrial adoption of collaborative vehicle routing. Our agents also reach agreement in a decentralised and self-interested manner, which overcomes the limitations of central orchestration methods mentioned in Section 2.

Furthermore, our MARL agents are able to outperform the two baseline bots in both accuracy and optimality. The heuristic bot and random bot has an accuracy of 62% and 25% respectively, and an optimality gap of 8% and 32% respectively. The relatively low performance of both the heuristic bot and random bot suggests that the experimental setup is sufficiently challenging (due to the NP-hard nature of vehicle routing problems), and that simple policies are not performant in this setting. The heuristic bot shows that 38% of the time, it is not desirable to form the grand coalition as some agents may contribute very little. The random bot's high optimality gap shows that, whilst there is symmetry in our problem and depots are equidistant, the choice of partners is still important. This necessitates more intelligent agents and thus complex methods such as MARL. More importantly, we conclude that our MARL agents have learnt interesting behaviours, such as to exclude opponents if they contribute little to the coalition, as seen in Fig. 10.

In this work, we make the assumption that each carrier possesses only one truck. We further assume that the same truck driver is assigned to the same truck. This is a reasonable assumption as the road freight industry is highly fragmented: for example, in

Fig. 9. Learning curve of (a) Agent 1's average proposed pay-offs (b) average number of bargaining rounds across all 3 agents for readability. Solid lines denote mean accuracy across 10 independent runs. Shaded regions denote ± two standard deviations. Dashed lines denote the proposed pay-off of an equal gain share agent. In (a), after 10,000 epochs, Agent 1 converges on an approximately equal gain share. In (b), after 10,000 epochs agents reach agreement after an averaged 1.03 rounds of bargaining. Both of these results agree with Okada (1996).

![Image](image_000016_79d775e8ed4a9f355793131d57fc68d8079525e7e200648a72762360ae113dc9.png)

![Image](image_000017_a97ab17858ea96ac91d60999a604b3515a709d30b369d579832bfc9fb18dec4a.png)

the UK, there are 60,000 registered carriers (Office for National Statistics, 2022) in 2022, and 1 million registered carriers in the EU in 2020 (Eurostat, 2020). However, if a single carrier possesses multiple trucks and thus multiple drivers, it would be possible to decompose the problem at different levels of granularity. One could consider coalitions of carriers; coalitions of trucks; or even coalitions of truck drivers. Our framework should be applicable to deal with all three types of modelling choices, but clearly the more granular the modelling choice, the more computational power that will be required.

The benefit of studying collaborative routing in a coalitional bargaining game is that game theory describes optimal, rational behaviour in this setting. As a result, we have a measure of the gap to optimality. This is important because of the challenging nature of 3-player, mixed-motive settings for MARL; thus, we can understand if the agents are learning correctly. However, there are three main limitations of this approach. Firstly, collaborative vehicle routing is most fruitful with a large number of participating carriers (Cruijssen et al., 2007a; Los et al., 2022). Future work must investigate scaling our MARL approach to a larger number of carriers. We believe this to be possible in a hybrid centralised-decentralised manner. The advantage of our decentralised MARL approach is that it enables us to provide a large volume of high-quality solutions to optimise the central agent. Secondly, future work should investigate the performance of MARL-based approaches on real-world data distributions with real-world constraints. One direction would be to study the effect of data imbalance (such as the locations of depots and customers, as well as the delivery volumes) on the performance of MARL-based methods. Another direction could be to study the effect of partial observability of other carriers' information; we currently consider the perfect information scenario where all delivery information is publicly shared (though, crucially, the characteristic function is still unknown). It would be interesting in future work to explore imperfect information settings, such as the value of information sharing. This could be tackled using decentralised, partially observable Markov decision processes (dec-POMDPs) (Oliehoek and Amato, 2016). Thirdly, our approach currently only incentivises carriers.

Fig. 10. The empirical pay-off agent 1 receives as a result of coalitional bargaining vs. the theoretical Shapley values for 2048 test instances. Green circle markers denote when agent 1 was included in the coalition. Orange cross markers denote when agent 1 was excluded from the coalition. 𝑅 2 score of 0.76, mean squared error of 0.08 and mean absolute error of 0.01.

![Image](image_000018_36cb8a1b73206ee3f5803b05e0f4d90c138bf56fc6bedf32e14c74448ea42466.png)

An independent third-party logistics provider may be required to enable collaborative routing. How should we incentivise third-party logistics providers? How should we incentivise shippers? What role could government play to incentivise collaboration? Moving in these directions with MARL would result in using more complex and flexible games; however, optimal, rational behaviour would be unknown. Nevertheless, MARL may still be applied to these complex games but in a descriptive manner (Shoham et al., 2007), i.e. to analyse the emergent behaviour of agents assuming a given MARL algorithm. We believe this to be an exciting line of future research.

## 6. Conclusions and managerial implications

Collaborative Vehicle Routing has promised cost savings between 4 - 46% in the last two decades. Yet industrial adoption remains limited. A key remaining barrier is the design of a gain sharing mechanism that is fair and scalable such that carriers are incentivised to collaborate. Orchestration of truck sharing is usually proposed via a central optimiser, where an intermediary would receive information from each carrier and allocate trucks to each route. Subscription to intermediaries do not necessarily outweigh costs, and carriers typically do not obtain any benefits from sharing their trucks. In this paper, we propose an automated, decentralised approach, where software agents representing carriers find optimal routes through a coalitional bargaining game, and any gain obtained via improved truck utilisation is shared between the carriers. Manual orchestration costs are also avoided as the approach is automated.

To facilitate decentralised optimisation and fair gain sharing we utilised deep multi-agent reinforcement learning. The main challenge of our setting is the inability of extant methods to fully evaluate the characteristic function due to high computational complexity. The characteristic function calculates the collaboration gain for every possible coalition, which requires solving an exponential number of NP-hard VRPs. The autonomous agents designed in this work are able to correctly reason over a highdimensional graph input to implicitly reason about the characteristic function instead. This eliminates the need to evaluate the expensive post-collaboration vehicle routing problem an exponential number of times and increases its practicability as we only need to evaluate this once. Furthermore, applying MARL to mixed-motive games is highly non-trivial and applying out-of-the-box MARL algorithms to this problem does not work. We show that we are able to achieve strong performance through careful design decisions, such as transfer learning, a targeted training design and COMA, and provide intuition for why these approaches help.

Moreover, the multi-agent reinforcement learning approach designed in our work is applicable to any coalitional bargaining game. Thus, our work may be suitable to problems in the broader collaborative logistics literature such as warehouse sharing.

Another important point is that collaboration is not centrally orchestrated but facilitated using decentralised decision making. This marks an important step towards real-world adoption which might encourage transportation planners to consider more profitable and fair collaboration scenarios. Whilst we initially envisage this system operating as a decision support system, as transportation planners gain trust in the agents' decisions, we ultimately envisage this system to operate fully autonomously. This would enable even faster decision making that is traceable and consistent, potentially enabling a more responsive supply chain (Brintrup et al., 2009). We urge transport planners and software system providers to consider potential adoption scenarios and integration into information systems.

Our work has limitations which provide avenues for future research. The current focus of this work is to obtain strong autonomous agents that maximally cooperate in the challenging mixed-motive setting of collaborative vehicle routing. Whilst we have achieved this, we have focused on a setting with 3 carriers as the focus of our work was to provide the theoretical link between collaborative vehicle routing, coalitional bargaining, and deep multi-agent reinforcement learning. Future work should investigate the scalability of a MARL approach to a larger number of agents. Furthermore, CVR problems typically include various additional considerations such as axle weights, goods compatibility, and packing orders, which have not yet been incorporated to the framework proposed here. Our approach is agnostic to the underlying optimisation design, and being so, we do not envisage the incorporation of additional problem features to hinder its function.

## Funding

This work was supported by the UK Engineering and Physical Sciences Research Council (EPSRC) grant on ''Intelligent Systems for Supply Chain Automation'' under Grant Number 2275316, as well as by the UK EPSRC Connected Everything Network Plus under Grant EP/S036113/1.

## Declaration of competing interest

The authors declare that they have no known competing financial interests or personal relationships that could have appeared to influence the work reported in this paper.

## Acknowledgements

This work was performed using resources provided by the Cambridge Service for Data Driven Discovery (CSD3) operated by the University of Cambridge Research Computing Service (www.csd3.cam.ac.uk), provided by Dell EMC and Intel using Tier-2 funding from the Engineering and Physical Sciences Research Council (capital grant EP/T022159/1), and DiRAC funding from the Science and Technology Facilities Council, United Kingdom (www.dirac.ac.uk).

We thank the three anonymous reviewers for their support and insightful comments during the review process which has greatly enhanced this paper. We also thank the Supply Chain Artificial Intelligence Lab (SCAIL) for their insightful discussions regarding early drafts of this paper.

## Appendix A. Capacitated vehicle routing problem

In our paper, the pre-collaboration social welfare can be calculated by first solving three independent Capacitated Vehicle Routing Problems, where we assume an arbitrarily high capacity for each vehicle.

The capacitated vehicle routing problem (CVRP) and their variants have been studied for over 60 years (Toth and Vigo, 2014). Here we show the three-index (vehicle-flow) formulation .

The CVRP considers the setting where goods are distributed to 𝑛 customers. The goods are initially located at the depot , denoted by nodes (or vertices) 𝑜 and 𝑑 . Node 𝑜 refers to the starting point of a route, and node 𝑑 the end point of a route. The customers are denoted by the set of nodes 𝑁 = {1 2 … , , , 𝑛 } . Each customer 𝑖 ∈ 𝑁 has a demand 𝑞 𝑖 ≥ 0 . In our setting, we consider 𝑞 𝑖 = 1 for all customers. A fleet of | 𝐾 | vehicles 𝐾 = {1 2 … , , , | 𝐾 | } are said to be homogeneous if they all have the same capacity 𝑄 &gt; 0 . In our setting, we consider only one vehicle and set its capacity 𝑄 to an arbitrarily high number to remove the capacity constraint. A vehicle must start at the depot, and can deliver to a set of customers 𝑆 ⊆ 𝑁 before returning to the depot. The travel cost 𝑐 𝑖,𝑗 is associated for a vehicle travelling between nodes 𝑖 and 𝑗 which we assume to be the Euclidean distance.

This problem can be modelled as a complete directed graph 𝐺 = ( 𝑉 , 𝐴 ) , where the vertex set 𝑉 ∶= 𝑁 ∪ { 𝑜, 𝑑 } and the arc set 𝐴 ∶= ( 𝑉 ⧵ { 𝑑 }) × ( 𝑉 ⧵ { }) 𝑜 . We define the in-arcs of 𝑆 as 𝛿 - ( 𝑆 ) = {( 𝑖, 𝑗 ) ∈ 𝐴 ∶ 𝑖 ∉ 𝑆, 𝑗 ∈ 𝑆 } . The out-arcs of 𝑆 is 𝛿 + ( 𝑆 ) = {( 𝑖, 𝑗 ) ∈ 𝐴 ∶ 𝑖 ∈ 𝑆, 𝑗 ∉ 𝑆 } .

The binary decision variables 𝑥 𝑖𝑗𝑘 denotes whether a vehicle 𝑘 ∈ 𝐾 travels over the arc ( 𝑖, 𝑗 ) ∈ 𝐴 . The binary decision variables 𝑦 𝑖𝑘 denotes whether a vehicle 𝑘 ∈ 𝐾 visits node 𝑖 ∈ 𝑉 . 𝑢 𝑖𝑘 denotes the load in vehicle 𝑘 before visiting node 𝑖 . We define the demand at the depot nodes 𝑜 and 𝑑 to be 0, i.e. 𝑞 𝑜 = 𝑞 𝑑 = 0 . This yields:

$$\minimize & \sum _ { k \in K } c ^ { T } x _ { k } \\ \text{subject to } & \sum _ { k \in K } y _ { i k } = 1, & \forall i \in N,$$

$$( 1 a )$$

$$( 1 b )$$

$$\psi$$

$$S. \, M a k \, e t \, a l \, & \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \  \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \
 \quad x _ { k } ( \delta ^ { + } ( i ) ) - x _ { k } ( \delta ^ { - } ( i ) ) = \begin{cases} 1, & i = 0, & \forall i \in V \, \subset \, [ d \}, k \in K, & \quad \, ( 1 d ) \\ 0, & i \in N, & \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \\ y _ { k } = x _ { k } ( \delta ^ { + } ( i ) ) & \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad\, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \\ \quad y _ { d k } = x _ { k } ( \delta ^ { - } ( d ) ) & \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \,\quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \, \quad \,
 \quad \quad \quad$$

- · The objective function (1a) minimises the Euclidean distance travelled by the vehicle.
- · Constraint (1c) ensures that the sum of vehicles entering node 𝑑 and exiting node 𝑑 is -1 . This ensures that a vehicle 𝑘 performs a route starting at 𝑜 and ending at 𝑑 .
- · Constraint (1b) ensures the vehicle only visits each customer once.
- · Constraint (1d) and (1e) couples variables 𝑥 𝑖𝑗𝑘 and 𝑦 𝑖𝑘 .
- · Constraint (1g) is the capacity constraint.
- · Constraint (1f) is the Miller-Tucker-Zemlin constraint which helps eliminate subtours.

## Appendix B. Multi-depot vehicle routing problem

In our paper, the post-collaboration social welfare can be calculated by solving the multi-depot vehicle routing problem (MDVRP) once. The number of depots corresponds to the number of agents within the accepted coalition. Again, we remove capacity constraints by setting the capacity of each vehicle to an arbitrarily large number. However, we add the additional constraint that each vehicle has to visit at least one customer.

The MDVRP is a simple extension of the CVRP formulation provided in Appendix A. Instead of having the depot simply represented by nodes 𝑜 and 𝑑 , the depots are extended to belong to a specific vehicle 𝑘 through nodes 𝑜 𝑘 and 𝑑 𝑘 . Doing so yields:

| minimize   | ∑ 𝑘 ∈ 𝐾 𝑐 𝑇 𝑥 𝑘                                               |                            | (2a)   |
|------------|---------------------------------------------------------------|----------------------------|--------|
| subject to | ∑ 𝑘 ∈ 𝐾 𝑦 𝑖𝑘 = 1 ,                                            | ∀ 𝑖 ∈ 𝑉 ,                  | (2b)   |
|            | 𝑥 𝑘 ( 𝛿 + ( 𝑖 )) - 𝑥 𝑘 ( 𝛿 - ( 𝑖 ))= 1 , 𝑖 = 𝑜 𝑘 , 0 , 𝑖 ∈ 𝑁, | ∀ 𝑖 ∈ 𝑉 ⧵ { 𝑑 𝑘 } , 𝑘 ∈ 𝐾, | (2c)   |
|            | 𝑦 𝑖𝑘 = 𝑥 𝑘 ( 𝛿 + ( 𝑖 ))                                       | ∀ 𝑖 ∈ 𝑉 ⧵ { 𝑑 𝑘 } , 𝑘 ∈ 𝐾, | (2d)   |
|            | 𝑦 𝑑 𝑘 𝑘 = 𝑥 𝑘 ( 𝛿 - ( 𝑑 𝑘 ))                                  | ∀ 𝑘 ∈ 𝐾,                   | (2e)   |
|            | 𝑦 𝑑 𝑘 𝑘 = 1                                                   | ∀ 𝑘 ∈ 𝐾,                   | (2f)   |
|            | 𝑢 𝑖𝑘 - 𝑢 𝑗𝑘 + 𝑄𝑥 𝑖𝑗𝑘 ≤ 𝑄 - 𝑞 𝑗                                | ∀( 𝑖, 𝑗 ) ∈ 𝐴, 𝑘 ∈ 𝐾,      | (2g)   |
|            | 𝑞 𝑖 ≤ 𝑢 𝑖𝑘 ≤ 𝑄                                                | ∀ 𝑖 ∈ 𝑉 , 𝑘 ∈ 𝐾,           | (2h)   |
|            | 𝑥 = ( 𝑥 𝑘 ) ∈ {0 , 1} 𝐾 × 𝐴 ,                                 |                            | (2i)   |
|            | 𝑦 = ( 𝑦 𝑘 ) ∈ {0 , 1} 𝐾 × 𝑉 .                                 |                            | (2j)   |

- · The objective function (2a) minimises the Euclidean distance travelled by all vehicles.
- · Constraint (2c) ensures that the sum of vehicles entering node 𝑑 𝑘 and exiting node 𝑑 𝑘 is -1 . This ensures that a vehicle 𝑘 performs a route starting at 𝑜 𝑘 and ending at 𝑑 𝑘 .
- · Constraint (2b) ensures that each vehicle only visits each customer once.
- · Constraint (2d) and (2e) couples variables 𝑥 𝑖𝑗𝑘 and 𝑦 𝑖𝑘 .
- · Constraint (2g) is the Miller-Tucker-Zemlin constraint which helps eliminate subtours.
- · Constraint (2f) ensures that each vehicle performs at least one delivery.
- · Constraint (2h) is the capacity constraint.

## Appendix C. Expected number of bargaining rounds by a random bot

Let 𝑋 be a discrete random variable denoting the number of bargaining rounds. Let us assume we have a random agent as discussed in Section 5.3.2 which proposes coalitions, pay-off vectors and responses uniformly at random. We wish to calculate the expected number of bargaining rounds achieved by three random bots, E [ 𝑋 ] . The maximum number of bargaining rounds is 10 in our experiments (although our ablations show that increasing this to 30 has no meaningful difference).

$$\mathbb { E } [ X ] = \sum _ { k = 1 } ^ { 1 0 } x \cdot P ( X = x )$$

$$= 1 \cdot P ( X = 1 ) + 2 \cdot P ( X = 2 ) + \dots + 1 0 \cdot P ( X = 1 0 )$$

To obtain 𝑃 𝑋 ( = 1) , note that e.g. for Player 2, the random bot can propose four coalitions, 𝐶 = {1 2 3} {1 2} {2 3} , , , , , , or {2} since Player 2 must be in the coalition 𝐶 . If the coalition 𝐶 = {1 2 3} , , is proposed, then both Players 1 and 3 must accept for the bargaining process to terminate, which yields a probability of acceptance (and thus termination) of 1 2 2 .

Therefore, 𝑃 𝑋 ( = 1) can be re-written as follows:

$$P ( X )$$

$$= \left [ 0. 2 5 \times \frac { 1 } { 2 } ^ { 2 } \right ] + \left [ ( 0. 2 5 + 0. 2 5 ) \times \frac { 1 } { 2 } \right ] + [ 0. 2 5 ]$$

$$P ( X = 1 ) & = \left [ P ( | C | = 3 ) \times \frac { 1 } { 2 } \right ] + \left [ P ( | C | = 2 ) \times \frac { 1 } { 2 } \right ] + [ P ( | C | = 1 ) ] \\ & = \left [ 0. 2 5 \times \frac { 1 } { 2 } \right ] + \left [ ( 0. 2 5 + 0. 2 5 ) \times \frac { 1 } { 2 } \right ] + [ 0. 2 5 ]$$

Repeating a similar logic to calculate E [ 𝑋 ] yields an expected number of bargaining rounds of 1.775.

$$\mathbb { E } [ X ] = ( 1 \cdot 0. 5 6 2 5 ) + ( 2 \cdot 0. 2 4 6 1 ) + ( 3 \cdot 0. 1 0 7 7 ) + ( 4 \cdot 0. 0 4 7 1 ) + \cdots + ( 1 0 \cdot 0. 0 0 3 )$$

$$= 1. 7 7 5$$

Empirically, our bots reach agreement at 1.777 rounds averaged over 10 runs.

## Appendix D. Pseudo-code of the entire pipeline

## Algorithm 1 Pseudo-code of MARL pipeline

1: Initialise 𝜽 = 𝜃 , 𝜃 1 2 , … , 𝜃 𝑛 , 𝜃 𝑐𝑟𝑖𝑡𝑖𝑐 // 𝑛 actors' (neural network) policy and critic 2: 3: // Supervised Pre-training (regression, minimise mean-squared error) 4: for 𝜃 𝑖 in 𝜽 do 5: ( 𝛥̂ 𝑦 𝜃 𝑖 ) 2 = ( 𝑣 𝐶 ( ) -̂ 𝑦 𝜃 𝑖 ) 2 // Calculate loss 6: 𝛥𝜃 𝑖 = ∇ 𝜃 𝑖 ( 𝛥̂ 𝑦 𝜃 𝑖 ) 2 // Calculate gradients 7: 𝜃 𝑖 = 𝜃 𝑖 + 𝛼𝛥𝜃 𝑖 // Update parameters 8: 9: // MARL Training 10: for each training epoch 𝑒 do 11: Initialise 𝑀 = 2048 parallel environments // Coalitional bargaining envs. 12: 𝑠 1 ∼ 𝜌 , 𝑡 1 = 0 // Sample the initial state 𝑠 1 from 𝜌 1 13: while 𝑠 𝑡 ≠ terminal and 𝑡 &lt; 𝑇 do 14: t += 1 15: // Calculate joint actions a 16: for i in N do 17: 𝑎 𝑖,𝑡 ∼ 𝜋 𝜃 𝑖 ( 𝑎 𝑖,𝑡 | 𝑠 𝑖,𝑡 ) // Select actions stochastically for exploration 18: 𝑠 𝑡 +1 ∼  ( 𝑠 𝑡 , 𝐴 𝑡 ) // Sample next state from transition dynamics 19: 𝑅𝑖,𝑡 ∼  ( 𝑠 𝑡 , 𝐴 , 𝑠 𝑡 𝑡 +1 ) ∀ 𝑖 ∈ 𝑁 // Calculate reward 20: Store each ⟨ 𝑠 𝑖,𝑡 , 𝑎 𝑖,𝑡 , log( 𝜋 𝜃 𝑖 ( 𝑎 𝑖,𝑡 | 𝑠 𝑖,𝑡 )) , 𝑠 𝑖,𝑡 +1 , 𝑅 𝑖,𝑡 ⟩ ∀ 𝑖 ∈ 𝑁 in agent 𝑖 's buffer 21: 22: // Here, all 𝑀 episodes will be finished 23: for 𝑡 = 1 to T do 24: 𝐺𝑖,𝑡 = ∑ 𝑇 𝑡 ′ = 𝑡 𝛾 𝑡 ′ -𝑡 𝑅𝑖,𝑡 ∀ 𝑖 ∈ 𝑁 // Calculate discounted returns 25: for t=T down to 1 do 26: ( 𝛥𝑄𝑖,𝑡 ) 2 = [ 𝐺𝑖,𝑡 -̂ 𝑄𝜃𝑐𝑟𝑖𝑡𝑖𝑐 ( 𝑠 𝑖,𝑡 , a ) ] 2 // Calculate critic loss 27: 𝛥𝜃 𝑐𝑟𝑖𝑡𝑖𝑐 = ∇ 𝜃 𝑐𝑟𝑖𝑡𝑖𝑐 ( 𝛥𝑄𝑖,𝑡 ) 2 // Calculate critic gradients 28: 𝜃 𝑐𝑟𝑖𝑡𝑖𝑐 = 𝜃 𝑐𝑟𝑖𝑡𝑖𝑐 + 𝛼𝛥𝜃 𝑐𝑟𝑖𝑡𝑖𝑐 // Update critic parameters 29: for t=T down to 1 do 30: // Calculate proposal baseline 31: 𝐴 𝑖 𝑡, 𝑝𝑟𝑜𝑝. = 𝐺𝑖,𝑡 -̂ 𝑉 𝑠, 𝜃 ( 𝑐𝑟𝑖𝑡𝑖𝑐 ) ∀ 𝑖 ∈ 𝑁 32: // Calculate response baseline 33: 𝐴 𝑖 𝑡, 𝑟𝑒𝑠𝑝. = 𝐺𝑖,𝑡 -∑ 𝑎 ̂ 𝑄 𝑠 𝑖,𝑡 ( , 𝑎, a -𝑎 , 𝜃 𝑐𝑟𝑖𝑡𝑖𝑐 ) 𝜋 𝜃 𝑖 ( 𝑎 𝑖,𝑡 | 𝑠 𝑖,𝑡 ) ∀ 𝑖 ∈ 𝑁 34: // Accumulate actor proposal gradients 35: 𝛥𝜃 𝑖 += ∇ 𝜃 𝑖 [ min( 𝑟 𝑡 ( 𝜃 𝑖 ) 𝐴 𝑖 𝑡,𝑝𝑟𝑜𝑝. , clip ( 𝑟 𝑡 ( 𝜃 𝑖 ) , 1 𝜀, 1 + 𝜀 𝐴 𝑖 ) 𝑡,𝑝𝑟𝑜𝑝. ] ∀ 𝑖 ∈ 𝑁 36: // Accumulate actor response gradients 37: 𝛥𝜃 𝑖 += ∇ 𝜃 𝑖 [ min( 𝑟 𝑡 ( 𝜃 𝑖 ) 𝐴 𝑖 𝑡,𝑟𝑒𝑠𝑝. , clip ( 𝑟 𝑡 ( 𝜃 𝑖 ) , 1 𝜀, 1 + 𝜀 𝐴 𝑖 ) 𝑡,𝑟𝑒𝑠𝑝. ] ∀ 𝑖 ∈ 𝑁 38: 𝜃 𝑖 = 𝜃 𝑖 + 𝛼𝛥𝜃 𝑖 ∀ 𝑖 ∈ 𝑁 // Update actors' policy parameters 39: 𝛥𝜃 𝑖 = 𝟎 // Reset gradients

## Appendix E. Holistic diagram of our pipeline

## 1) Sample initial state, 81 from p

## 3) Agent is Responding

![Image](image_000019_0a0cdd677bbe1d806bf4f0046c3bc80e62cc673657ee8d3d6a2805d945cf8dca.png)

![Image](image_000020_cdd24d47c1dd9d0a656f1fd7a6e9fc80c9758a904215ebec94aee2523b60670b.png)

![Image](image_000021_fef6899188d168bda38e6b573dace3a4f28c0aafd80c19b20fc701c490fdf541.png)

## 5) Update Actor and Critic Parameters

- [5.1) Calculate discounted returns
- [5.2) Calculate proposal and response baseline

[5.2) Update critic parameters to minimise mean Isquared error

- [5.3) Update actors' policy parameters

Fig. E.1. A holistic diagram of our proposed approach. We depict a sample trajectory where Agent 2 is selected to propose and that Agent 2 proposes to form the grand coalition with an equal payoff vector. Agents 1 and 3 then agree to the proposal. Finally, the actors' parameters and critic's parameters are updated accordingly.

## References

Adenso-Díaz, B., Lozano, S., Moreno, P., 2014. Analysis of the synergies of merging multi-company transportation needs. Transp. A Transp. Sci. 10 (6), 533-547, Publisher: Informa UK Limited.

- Agarwal, R., Schwarzer, M., Castro, P.S., Courville, A., Bellemare, M.G., 2022. Reincarnating reinforcement learning: Reusing prior computation to accelerate progress. arXiv:2206.01626 [cs, stat].
- Angelelli, E., Morandi, V., Speranza, M.G., 2022. Optimization models for fair horizontal collaboration in demand-responsive transportation. Transp. Res. C 140, 103725.
- Bachrach, Y., Everett, R., Hughes, E., Lazaridou, A., Leibo, J.Z., Lanctot, M., Johanson, M., Czarnecki, W.M., Graepel, T., 2020. Negotiating team formation using deep reinforcement learning. Artificial Intelligence 288, 103356.
- Baker, B., Kanitscheider, I., Markov, T., Wu, Y., Powell, G., McGrew, B., Mordatch, I., 2020. Emergent tool use from multi-agent autocurricula. arXiv:1909.07528 [cs, stat].
- Bo Dai, Chen, H., 2009. Mathematical model and solution approach for collaborative logistics in less than truckload (LTL) transportation. In: 2009 International Conference on Computers &amp; Industrial Engineering. IEEE.

Brintrup, A., 2021. AI in the supply chain: a classification framework and critical analysis of current state. In: Oxford Handbook of Supply Chain Management. Oxford University Press.

Brintrup, A., Ranasinghe, D., Kwan, S., Parlikad, A.K., Owens, K., 2009. Roadmap to self-serving assets in civil aerospace. In: Proceedings of the 1st CIRP Industrial Product-Service Systems (IPS2) Conference.

- Chalkiadakis, G., Boutilier, C., 2004. Bayesian reinforcement learning for coalition formation under uncertainty. In: Proceedings of the Third International Joint Conference on Autonomous Agents and Multiagent Systems. AAMAS 2004, pp. 1090-1097.
- Chalkiadakis, G., Elkind, E., Wooldridge, M., 2011. Computational Aspects of Cooperative Game Theory, first ed. In: Synthesis Lectures on Artificial Inetlligence and Machine Learning, Morgan &amp;amp; Claypool Publishers.
- Chou, P.-W., Maturana, D., Scherer, S., 2017. Improving stochastic policy gradients in continuous control with deep reinforcement learning using the beta distribution. In: Proceedings of the 34th International Conference on Machine Learning. PMLR, (ISSN: 2640-3498) pp. 834-843.
- Cruijssen, F., 2020. Cross-Chain Collaboration in Logistics: Looking Back and Ahead. In: International Series in Operations Research and Management Science, Springer.
- Cruijssen, F., Bräysy, O., Dullaert, W., Fleuren, H., Salomon, M., 2007a. Joint route planning under varying market conditions. Int. J. Phys. Distrib. Logist. Manage. 37 (4), 287-304, Publisher: Emerald.
- Cruijssen, F., Cools, M., Dullaert, W., 2007b. Horizontal cooperation in logistics: Opportunities and impediments. Transp. Res. E 43 (2), 129-142, Publisher: Elsevier BV.
- Deng, X., Papadimitriou, C.H., 1994. On the complexity of cooperative solution concepts. Math. Oper. Res. 19 (2), 257-266, Publisher: INFORMS. Eurostat, 2020. Annual detailed enterprise statistics for services (NACE Rev. 2 H-N and S95): SBS\_Na\_1a\_Se\_R2.
- Ferrell, W., Ellis, K., Kaminsky, P., Rainwater, C., 2020. Horizontal collaboration: opportunities for improved logistics planning. Int. J. Prod. Res. 58 (14), 4267-4284, Publisher: Informa UK Limited.
- Foerster, J.N., Assael, Y.M., de Freitas, N., Whiteson, S., 2016. Learning to communicate with deep multi-agent reinforcement learning. arXiv:1605.06676 [cs]. Foerster, J., Farquhar, G., Afouras, T., Nardelli, N., Whiteson, S., 2017. Counterfactual multi-agent policy gradients. arXiv:1705.08926 [cs].
- Fox, M.S., Barbuceanu, M., Teigen, R., 2000. Agent-oriented supply-chain management. In: Information-Based Manufacturing. Springer US, Boston, MA, pp. 81-104.
- Gabel, T., Riedmiller, M., 2012. Distributed policy search reinforcement learning for job-shop scheduling tasks. Int. J. Prod. Res. 50 (1), 41-61, Publisher: Informa UK Limited.
- Gansterer, M., Hartl, R.F., 2018a. Centralized bundle generation in auction-based collaborative transportation. OR Spectrum 40 (3), 613-635, Publisher: Springer Science and Business Media LLC.
- Gansterer, M., Hartl, R.F., 2018b. Collaborative vehicle routing: A survey. European J. Oper. Res. 268 (1), 1-12, Publisher: Elsevier BV.
- Gansterer, M., Hartl, R.F., 2020. Shared resources in collaborative vehicle routing. TOP 28 (1), 1-20, Publisher: Springer Science and Business Media LLC.
- Gansterer, M., Hartl, R.F., Vetschera, R., 2019. The cost of incentive compatibility in auction-based mechanisms for carrier collaboration. Networks 73 (4), 490-514, \_eprint: https://onlinelibrary.wiley.com/doi/pdf/10.1002/net.21828.
- Greensmith, E., Bartlett, P.L., Baxter, J., 2004. Variance reduction techniques for gradient estimates in reinforcement learning. J. Mach. Learn. Res. 5, 1471-1530, Publisher: JMLR.org.

Guajardo, M., Rönnqvist, M., 2016. A review on cost allocation methods in collaborative transportation. Int. Trans. Oper. Res. 23 (3), 371-392, Publisher: Wiley. Gurobi Optimization, LLC, 2021. Gurobi optimizer reference manual.

- Henderson, P., Islam, R., Bachman, P., Pineau, J., Precup, D., Meger, D., 2019. Deep reinforcement learning that matters. arXiv:1709.06560 [cs, stat].
- Ieong, S., Shoham, Y., 2005. Marginal contribution nets: a compact representation scheme for coalitional games. In: Proceedings of the 6th ACM Conference on
- Electronic Commerce - EC '05. ACM Press, Vancouver, BC, Canada, pp. 193-202.

Kosasih, E.E., Brintrup, A., 2021. Reinforcement learning provides a flexible approach for realistic supply chain safety stock optimisation. arXiv:2107.00913 [cs]. Krajewska, M.A., Kopfer, H., Laporte, G., Ropke, S., Zaccour, G., 2008. Horizontal cooperation among freight carriers: request allocation and profit sharing. J. Oper. Res. Soc. 59 (11), 1483-1491, Publisher: Informa UK Limited.

Kurach, K., Raichuk, A., Stańczyk, P., Zając, M., Bachem, O., Espeholt, L., Riquelme, C., Vincent, D., Michalski, M., Bousquet, O., Gelly, S., 2020. Google Research Football: A Novel Reinforcement Learning Environment. Technical Report, arXiv:1907.11180 [cs, stat] type: article.

Leibo, J.Z., Zambaldi, V., Lanctot, M., Marecki, J., Graepel, T., 2017. Multi-agent reinforcement learning in sequential social dilemmas. arXiv:1702.03037 [cs]. Los, J., Schulte, F., Gansterer, M., Hartl, R.F., Spaan, M.T.J., Negenborn, R.R., 2022. Large-scale collaborative vehicle routing. Ann. Oper. Res. Publisher: Springer Science and Business Media LLC.

- Lowe, R., Wu, Y., Tamar, A., Harb, J., Abbeel, P., Mordatch, I., 2020. Multi-agent actor-critic for mixed cooperative-competitive environments. arXiv:1706.02275 [cs].
- Mnih, V., Kavukcuoglu, K., Silver, D., Rusu, A.A., Veness, J., Bellemare, M.G., Graves, A., Riedmiller, M., Fidjeland, A.K., Ostrovski, G., Petersen, S., Beattie, C., Sadik, A., Antonoglou, I., King, H., Kumaran, D., Wierstra, D., Legg, S., Hassabis, D., 2015. Human-level control through deep reinforcement learning. Nature 518 (7540), 529-533.
- Mordatch, I., Abbeel, P., 2018. Emergence of Grounded Compositional Language in Multi-Agent Populations. Technical Report, arXiv:1703.04908 [cs] type: article.
- Murphy, K.P., 2021. Probabilistic Machine Learning: An Introduction. MIT Press.
- Nash, J., 1953. Two-person cooperative games. Econometrica 21 (1), 128, Publisher: JSTOR.
- Office for National Statistics, 2022. UK business: activity, size and location - Office for National Statistics.
- Okada, A., 1996. A noncooperative coalitional bargaining game with random proposers. Games Econom. Behav. 16 (1), 97-108, Publisher: Elsevier BV.
- Oliehoek, F.A., Amato, C., 2016. A Concise Introduction to Decentralized POMDPs. In: SpringerBriefs in Intelligent Systems, Springer International Publishing, Cham.

## OpenAI, 2022. ChatGPT. OpenAI.

OpenAI, Berner, C., Brockman, G., Chan, B., Cheung, V., De ¸biak, P., Dennison, C., Farhi, D., Fischer, Q., Hashme, S., Hesse, C., Józefowicz, R., Gray, S., Olsson, C., Pachocki, J., Petrov, M., Pinto, H.P.d.O., Raiman, J., Salimans, T., Schlatter, J., Schneider, J., Sidor, S., Sutskever, I., Tang, J., Wolski, F., Zhang, S., 2019. Dota 2 with large scale deep reinforcement learning. arXiv:1912.06680 [cs, stat].

Palhazi Cuervo, D., Vanovermeire, C., Sörensen, K., 2016. Determining collaborative profits in coalitions formed by two partners with varying characteristics. Transp. Res. C 70, 171-184, Publisher: Elsevier BV.

Pan, S., Trentesaux, D., Ballot, E., Huang, G.Q., 2019. Horizontal collaborative transport: survey of solutions and practical implementation issues. Int. J. Prod. Res. 57 (15-16), 5340-5361, Publisher: Informa UK Limited.

Pardo, F., Tavakoli, A., Levdik, V., Kormushev, P., 2018. Time limits in reinforcement learning. arXiv:1712.00378 [cs].

- Powell, W., 2022. Reinforcement Learning and Stochastic Optimization: A Unified Framework for Sequential Decisions. Wiley.
- Puterman, M.L., 1994. Markov Decision Processes: Discrete Stochastic Dynamic Programming, first ed. John Wiley &amp; Sons, Inc., USA.

Samvelyan, M., Rashid, T., de Witt, C.S., Farquhar, G., Nardelli, N., Rudner, T.G.J., Hung, C.-M., Torr, P.H.S., Foerster, J., Whiteson, S., 2019. The StarCraft Multi-Agent Challenge. Technical Report, arXiv:1902.04043 [cs, stat] type: article.

- Schulman, J., Wolski, F., Dhariwal, P., Radford, A., Klimov, O., 2017. Proximal policy optimization algorithms. arXiv:1707.06347 [cs].

Serrano, R., 2004. Fifty years of the Nash program, 1953-2003. SSRN Electron. J.

- Shoham, Y., Powers, R., Grenager, T., 2007. If multi-agent learning is the answer, what is the question? Artificial Intelligence 171 (7), 365-377, Publisher: Elsevier BV.

Silver, D., Huang, A., Maddison, C.J., Guez, A., Sifre, L., van den Driessche, G., Schrittwieser, J., Antonoglou, I., Panneershelvam, V., Lanctot, M., Dieleman, S., Grewe, D., Nham, J., Kalchbrenner, N., Sutskever, I., Lillicrap, T., Leach, M., Kavukcuoglu, K., Graepel, T., Hassabis, D., 2016. Mastering the game of Go with deep neural networks and tree search. Nature 529 (7587), 484-489.

Sutton, R.S., Barto, A.G., 2018. Reinforcement Learning: An Introduction, second ed. In: Adaptive Computation and Machine Learning Series, The MIT Press, Cambridge, Massachusetts.

Sutton, R.S., Mcallester, D., Singh, S., Mansour, Y., 2000. Policy gradient methods for reinforcement learning with function approximation. In: Advances in Neural Information Processing Systems 12. MIT Press, pp. 1057-1063.

Taylor, M.E., Stone, P., 2009. Transfer Learning for Reinforcement Learning Domains: A Survey. J. Mach. Learn. Res. 10, 1633-1685, Publisher: JMLR.org. Toth, P., Vigo, D. (Eds.), 2014. Vehicle Routing: Problems, Methods, and Applications, second ed. In: MOS-SIAM Series on Optimization, (no. 18), SIAM. UK BEIS, 2021. Final UK greenhouse gas emissions national statistics.

UK DfT, 2020. Road freight statistics: 2019.

Vinyals, O., Babuschkin, I., Czarnecki, W.M., Mathieu, M., Dudzik, A., Chung, J., Choi, D.H., Powell, R., Ewalds, T., Georgiev, P., Oh, J., Horgan, D., Kroiss, M., Danihelka, I., Huang, A., Sifre, L., Cai, T., Agapiou, J.P., Jaderberg, M., Vezhnevets, A.S., Leblond, R., Pohlen, T., Dalibard, V., Budden, D., Sulsky, Y., Molloy, J., Paine, T.L., Gulcehre, C., Wang, Z., Pfaff, T., Wu, Y., Ring, R., Yogatama, D., Wünsch, D., Mckinney, K., Smith, O., Schaul, T., Lillicrap, T., Kavukcuoglu, K., Hassabis, D., Apps, C., Silver, D., 2019. Grandmaster level in StarCraft II using multi-agent reinforcement learning. Nature 575, 350-354, Publisher: Springer Science and Business Media LLC.

Williams, R.J., 1992. Simple statistical gradient-following algorithms for connectionist reinforcement learning. Mach. Learn. 8 (3-4), 229-256, Publisher: Springer Science and Business Media LLC.

Wurman, P.R., Barrett, S., Kawamoto, K., MacGlashan, J., Subramanian, K., Walsh, T.J., Capobianco, R., Devlic, A., Eckert, F., Fuchs, F., Gilpin, L., Khandelwal, P., Kompella, V., Lin, H., MacAlpine, P., Oller, D., Seno, T., Sherstan, C., Thomure, M.D., Aghabozorgi, H., Barrett, L., Douglas, R., Whitehead, D., Dürr, P., Stone, P., Spranger, M., Kitano, H., 2022. Outracing champion Gran Turismo drivers with deep reinforcement learning. Nature 602 (7896), 223-228.

Xu, L., Mak, S., Brintrup, A., 2021. Will bots take over the supply chain? Revisiting agent-based supply chain automation. Int. J. Prod. Econ. 241, 108279, Publisher: Elsevier BV.

Yan, Y., Chow, A.H.F., Ho, C.P., Kuo, Y.-H., Wu, Q., Ying, C., 2022. Reinforcement learning for logistics and supply chain management: Methodologies, state of the art, and future opportunities. Transp. Res. E 162, 102712.

Yu, C., Velu, A., Vinitsky, E., Wang, Y., Bayen, A., Wu, Y., 2021. The surprising effectiveness of MAPPO in cooperative, multi-agent games. arXiv:2103.01955 [cs].

Zhang, M., Pratap, S., Huang, G.Q., Zhao, Z., 2017. Optimal collaborative transportation service trading in B2B e-commerce logistics. Int. J. Prod. Res. 55 (18), 5485-5501, Publisher: Informa UK Limited.