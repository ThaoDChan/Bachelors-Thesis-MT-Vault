## :OHFWULF PRZHU T\VWHPV xHVHDUFK [[[ m[[[[R [[[

![Image](0_Artifacts/[aljohani2020]_Real-Time_metadata-driven_routing_optimization_for_electric_vehicle_energy_consumption_minimization_using_deep_reinforcement_learning_and_Markov_chain_model_artifacts/image_000000_e19039f68039287f67a43909f8a14c9b8f723101c96c63b090688cf5299a6481.png)

Contents lists available at ScienceDirect

## Electric Power Systems Research

journal homepage: www.elsevier.com/locate/epsr

## Real-Time metadata-driven routing optimization for electric vehicle energy consumption minimization using deep reinforcement learning and Markov chain model

Tawfiq M. Aljohani a,* , Ahmed Ebrahim a , Osama Mohammed a

- a Energy Systems Research Laboratories, Department of Electrical and Computer Engineering, Florida International University, Miami, FL 33174, USA

A R T I C L E  I N F O

## A B S T R A C T

## Keywords:

Energy minimization framework Artificial intelligence (AI) Electric vehicles (EVs) Routing optimization Markov Chain Model (MCM) Reinforcement Learning (RL) Google s API platform '

Double Deep Q-Network (DDQN)

A real-time, data-driven electric vehicle (EVs) routing optimization to achieve energy consumption minimization is  proposed  in  this  work.  The  proposed  framework utilizes  the  concept  of  Double  Deep  Q-learning Network (DDQN) in learning the maximum travel policy of the EV as an agent. The policy model is trained to estimate the agent s optimal action per the obtained reward signals and Q-values, representing the feasible routing options. ' The agent s energy requirement on the road is assessed following Markov Chain Model (MCM), with Markov s ' ' unit  step  represented  as  the  average  energy  consumption  that  takes  into  consideration  the  different  driving patterns, agent s surrounding environment, road conditions, and applicable restrictions. The framework offers a ' better exploration strategy, continuous learning ability, and the adoption of individual routing preferences. A real-time simulation in the python environment that considered real-life driving data from Google s API platform ' is  performed.  Results  obtained  for  two  geographically  different  drives  show  that  the  proposed  energy  consumption minimization framework reduced the energy utilization of the EVs to reach its intended destination by 5.89% and 11.82%, compared with Google s proposed routes originally. Both drives started at 4.30 PM on April ' 25th, 2019, in Los Angeles, California, and Miami, Florida, to reach EV s charging stations that are located six ' miles away from both of the starting locations.

## 1. Introduction

Automobiles have been contributing meaningfully to our lives by fulfilling  our  everyday  requirements  for  mobility.  However,  issues related to pollution emissions and fuel consumption have attracted the attention  of  researchers  toward  environment-friendly  and  efficient transportation. Electric vehicles (EVs) are seen as candidates to replace conventional Internal Combustion Engine (ICE) vehicles, as they can lower pollution and emissions while achieving higher energy efficiency. Rapidly improving EV technologies are promising to overcome many concerns related to the EV s battery capacities and energy management, ' such as extending the driving range of the vehicle and reducing the need for charging away from the customer s home. '

Unlike traditional methodologies, reinforcement learning (RL) is an artificial  intelligence  technique  that  has  been  widely  used  in  solving many  scientific  problems  in  recent  years  [1,  2].  The  concept  of Q-learning was first introduced in [3], and a Deep Q-learning Network (DQN) is an extension to it [4]. Applications of RL has been widely employed to perform studies related to EVs such as in proposing efficient smart charging algorithm [5], as well as energy management and control strategies for hybrid and pure electric vehicles [6, 7]. In addition to the  battery s  energy  management,  RL  techniques  and  methodologies ' have covered a wide range of topics related to power and energy engineering, such as electricity market trading in smart grids [8], energy production  scheduling  [9],  and  multi-agent  distributed  energy  management of a microgrid [10]. RL has been extensively investigated in the automotive industry in recent years. The authors of [6] employed RL to measure an adaptive optimal energy control strategy based on different driving schedules. They tested the learning ability of HEVs and verified via simulation the impact on fuel efficiency. They made comparisons of their proposed strategy with results obtained previously utilizing Stochastic Dynamic Programming (SDP). Similarly, based on the Q-learning algorithm, the authors of [11] studied a predictive energy management framework for a parallel hybrid  electric  vehicle  (HEV).  Their  results show significant reductions in both fuel consumption and computational time. Additionally, the authors in [12] tested the capabilities of deep reinforcement learning to train autonomous, self-driving automobiles

* Corresponding author. E-mail address: taljo005@fiu.edu (T.M. Aljohani).

## https://doi.org/10.1016/j.epsr.2020.106962

ipCuLCCFdo DiDi :OVHYLHU Abfib 4OO ULJKWV UHVHUYHGb Received 15 July 2020; Received in revised form 15 October 2020; Accepted 7 November 2020

POHDVH FLWH WKLV DUWLFOH DVI jDZϧT )b 4OMRKDQLh GLYPH&lt;0&gt;(GLYPH&lt;0&gt;OGLYPH&lt;0&gt;HGLYPH&lt;0&gt;FGLYPH&lt;0&gt;WGLYPH&lt;0&gt;UGLYPH&lt;0&gt;LGLYPH&lt;0&gt;F GLYPH&lt;0&gt;3GLYPH&lt;0&gt;RGLYPH&lt;0&gt;ZGLYPH&lt;0&gt;HGLYPH&lt;0&gt;U GLYPH&lt;0&gt;6GLYPH&lt;0&gt;\GLYPH&lt;0&gt;VGLYPH&lt;0&gt;WGLYPH&lt;0&gt;HGLYPH&lt;0&gt;PGLYPH&lt;0&gt;V GLYPH&lt;0&gt;5GLYPH&lt;0&gt;HGLYPH&lt;0&gt;VGLYPH&lt;0&gt;HGLYPH&lt;0&gt;DGLYPH&lt;0&gt;UGLYPH&lt;0&gt;FGLYPH&lt;0&gt;K h KWWSVIooGRLbRUJo,ib,i,doMbHSVUbDiDib,idFdD

![Image](0_Artifacts/[aljohani2020]_Real-Time_metadata-driven_routing_optimization_for_electric_vehicle_energy_consumption_minimization_using_deep_reinforcement_learning_and_Markov_chain_model_artifacts/image_000001_3591aa841b0e1794ca4671f2e3b6835c9441006e1429a02602cc4e66c23fe7d2.png)

T.M. Aljohani et al.

| Nomenclature   | Nomenclature                                                         | s ′ r( s +                                                      | The updated next state                                          |
|----------------|----------------------------------------------------------------------|-----------------------------------------------------------------|-----------------------------------------------------------------|
|                |                                                                      | a )                                                             | Reward function corresponding to a i and s i                    |
| EVs            | Electric Vehicles                                                    | P(s ' |s, a)                                                    | Probability that state transition between two states was        |
| HEVs           | Hybrid Electric Vehicles                                             |                                                                 | given an action.                                                |
| RL             | Reinforcement Learning                                               | Fr                                                              | CNN layer that transfer image into numeric for reward           |
| SDP            | Stochastic Dynamic Programming                                       |                                                                 | calculation.                                                    |
| ADP            | Asynchronous Dynamic Programming                                     | Π                                                               | Inferred travel policy of the agent                             |
| γ              | Discounted factor                                                    | ℑ sec , min, hour Multiple time-constant factors of the battery | ℑ sec , min, hour Multiple time-constant factors of the battery |
| R              | Reward                                                               |                                                                 | consumption behavior with time.                                 |
| DDQN           | Double Deep Q-learning Network Algorithm                             | I battery                                                       | Current load in the battery.                                    |
| SDKs           | Software Development Kits                                            | C battery                                                       | Capacity of the battery.                                        |
| MCM            | Markov Chain Modeling                                                | w i                                                             | Average energy consumption of the agent ' s battery on the      |
| MDP            | Markov Decision Process                                              |                                                                 | i th state.                                                     |
| CNN            | Convolutional Neural Network                                         | D                                                               | Diagonal matrix that has weights w, i i = 1, … , n.             |
| T M            | Transition Matrix                                                    | F rol                                                           | Force for overcoming the rolling resistance.                    |
| Q M            | Updated transition matrix with the inclusion of average weights w. i | F acc F slope                                                   | Accelerating force. Hill climbing force.                        |
| SoC            | State of Charge                                                      | a                                                               | The EV ' s acceleration factor.                                 |
| a i            | Action i was taken by the agent                                      | A v                                                             | Area in front of the vehicle.                                   |
| s i            | State of the agent during action i.                                  | M                                                               | Vehicle mass at speed V .                                       |
| s j            | State of the agent during action j.                                  | µ rol , ρ , C d , g Driving constants.                          | µ rol , ρ , C d , g Driving constants.                          |
| p ij           | Probability of transition from state i to state j.                   | φ                                                               | Incline of the traveled path.                                   |
| g i            | Edge that represents geocode locations at state i.                   | x 1                                                             | Travelled a distance of the cruising phase.                     |
| A              | The complete set of possible actions                                 | x 2                                                             | Travelled a distance of the deceleration process.               |
| S              | The complete set of possible states of the agent                     | x 3                                                             | Total length travel path.                                       |
| a ′            | The updated next action                                              |                                                                 |                                                                 |

that are aware of other elements in their surroundings, such as pedestrians, other vehicles, etc. The probabilistic nature of driving patterns of EVs is modeled in many recent works of literature as a Markov decision process (MDP). In [13], researchers tested the behavioral response of the stochastic charging of an EV station using MDP methodology. In [14] MDP was utilized to model the impact of stochastic driving, parking, and charging  patterns  of  EV  loads  on  the  local  distribution  grid.  Range anxiety is one of the major obstacles in the EVs market.

In addition to the automotive industry, RL has been a widely used technique in many fields of research in recent years, such as in robotic control  [15,  16,  17],  computer  systems  applications  [18,  19],  image processing [20, 21], agent-learning systems [22], traffic improvement and coordination systems [23, 24], as well as wirelesses and communication networks [25, 26, 27].

This paper proposes an RL-based energy consumption minimization strategy  to  achieve  an  EV  routing  optimization  considering  real-life driving  environment.  This  RL  study  is  based  on  evaluating  Q-values through the Double Deep Q-Network (DDQN) algorithm to train the EV as an agent that aims to choose actions corresponding to best obtained Q-values. This eventually leads to extending the vehicle s driving range. ' Modeling energy consumption on the road has been achieved based on Markov Chain Modeling (MCM) to estimate the energy requirements of traveling paths in accordance to input parameters and learning strategy. The learning experience is supported with real-time data retrieved from Google s API platform that serves as the source for input information, ' feeding the agent with real-time status of roads.

The remainder of this paper is organized as follows: Section 2 illustrates the main concept of the RL and MCM applied in this work with the DDQN algorithm; Section 3 presents the structure of our framework of modeling energy management for the EVs routing problem; the metadata utilization based on Google s API; and the application of the RL by ' calculating the reward arrangement in the RL algorithm; and Section 4 presents the results obtained based on experimentation and validation of the proposed methodology on two geographically distinct routes; Section 5 concludes the work with final remarks.

## 2. Reinforcement learning

## 2.1. Deep Q-Learning network (DQN)

RL is based on the concept of an agent, an environment, an inferred policy, Π , trajectory, a value V , set of actions ai , set of states si , computed Q-values and established rewards R . An agent is an entity or unit that performs an action, with the action being any possible move that the agent  can  make  within  a  given  environment.  The  environment  only provides a list of some possible actions that are self-explanatory, from which the agent can choose a specific action depending on its current state. Depending on the action chosen by the agent, a reward, either positive or negative, is initiated by the learning algorithm and the agent receives a discounted factor to decrease the effect of the current reward on the agent s choice. This discounted factor is denoted as ' γ , and the closer it gets to unity, the more influence the next reward will have on the current state, and vice versa. Furthermore, the reward could be seen as  feedback  that  indicates  the  possibility  of  failure  or  success  of  the action in relation to the overall driving experience of the agent. The reward can be either immediate or delayed, depending on the agent s ' available choices for a specific action. Additionally, a set of Q-values is generated based on the number of actions considered at each state. The state-action relationship is considered a generalized policy iteration and is better described by the following equation that integrates both the future and current reward of a given action:

<!-- formula-not-decoded -->

Where A is the complete set of possible actions ai ∈ A; si ∈ S is the complete set of possible states the agent may experience, with Qi ( s , a ) as the Q-value based on them; r i ( s , a ) is the reward function corresponding to ai and si ; P(s | s, a) ' is the probability of state transition between two states, given an action; s , a ' ' are the updated next state and next action, respectively.

In an environment, i.e., roads in a given region to navigate toward a destination, the agent can find itself in an instant real situation, known

GLYPH&lt;0&gt;5GLYPH&lt;0&gt;HGLYPH&lt;0&gt;VGLYPH&lt;0&gt;HGLYPH&lt;0&gt;D

T.M. Aljohani et al.

as the state. The agent, presumed to be an EV in our study, has the authority to move from its current state to the next state by inferring a strategy that achieves maximal rewards/minimal energy consumption, known as the travel policy Π . This policy determines the next action to be taken by the agent, based on the artificial learning provided by the RL algorithm. The information accumulated from the learning process influences a long-term reward value (v) of the current travel policy and is denoted as V π (s).

The Q-value is used to show the long-term benefit of the current state.  The  concept  of  Q-values  was  first  introduced  by  Watkins  in reference [3] as an extension of Asynchronous Dynamic Programming (ADP). Q-values give the agent powerful learning capabilities via successive evaluations of each action the agent aims to perform. Specifically, the agent and the environment interact dynamically, based on discrete  time  steps,  and,  at  each  step,  the  agent  receives  feedback regarding the current state of the environment.

With each additional step the agent takes, the next reward ri + ∈ 1 R is achieved. This reward influences the learning process to establish the new state, s , with a particular probability for the transformation be-' tween  states  corresponding  to  each  performed  action.  This  mapping constitutes the policy of the agent s learning process and the sequence of ' actions and states for achieving any reward is known as a trajectory. Fig. 1 provides a clear illustration of the concept of RL performed in this work.

For  exploration  and  exploitation,  the  agent  uses  the  action-value method  via  the  greedy  policy  to  achieve  the  optimum  result  by choosing the highest-value actions at each state. The following value function grades the states and measures the relative strength of the actions based on the weights of the associated Q-values:

<!-- formula-not-decoded -->

In a continuous-state environment, we may end up having a very large number of states. This leads to the fact that the Q-function may not fully converge, even when we can estimate the condition under which it should converge. In such cases, we need to perform approximations to allow generalization for the states the agent has not yet visited. The deep

Fig. 1. Illustration of the learning process in RL.

![Image](0_Artifacts/[aljohani2020]_Real-Time_metadata-driven_routing_optimization_for_electric_vehicle_energy_consumption_minimization_using_deep_reinforcement_learning_and_Markov_chain_model_artifacts/image_000002_5814fa6f75ab2f42d132c1553f851250423c4029b990dc944058da900dabb6bd.png)

Q-learning network (DQN) estimates for us the Q-values of these states and continues to improve them until optimal solution is found, which happen when the estimation of these states converges. To properly train the Q-function estimator, a common loss function is used as follows

<!-- formula-not-decoded -->

Where Li is the loss value to be estimated at the i th iteration, θ i is the parameter of the Q-function at the i th iteration, and gi is the target value at the i th iteration which could be found as follows

<!-- formula-not-decoded -->

To  ensure  more  stability  while  decreasing  the  loss  function,  we deploy a target network Qi ( s , a ) that has the same neural network architecture of the regular Q-value network Qi ( s , a ) ,  but  with  different parameters. When the DQN algorithm updates the Q-values, this target network is updated as well. Then, the updated target network is used to calculate the target value as follows

<!-- formula-not-decoded -->

The drawback of only using the DQN method is the problem of the overestimation of the Q-values. Typically, DQN estimates the Q-value as follows

<!-- formula-not-decoded -->

Sometimes, taking the highest Q-value to represent the best action at the beginning of the training process could be misleading, since we have less  information  about  the  states.  This  could  even  complicate  the learning  process  of  the  agent  especially  when  non-optimal  actions falsely give higher Q-values than those provided by the optimal-actions. To solve the overestimation problem, the DDQN algorithm was proposed by the authors of references [1, 2]. Utilizing the DDQN, we first use the DQN to find the best action for the agent corresponding to the highest Q-value. Then, we use a target network to estimate the target Q-value of the agent taking that action for the next state. Accordingly, Eq. (6) can be rewritten as follows

<!-- formula-not-decoded -->

Where argmaxQi ( s ′ , a ) is the DDQN target network which is used to assess  the  Q-value  of  taking  an  action, a , at  a  state s ′ . This  adds robustness and stability to the learning process and eliminate any potentials for overestimation of the Q-values

## 2.2. Double deep Q-Learning network (DDQN)

As we mentioned earlier, the role of the DDQN in this work is to produce a set of actions to be taken by the EV in Google API s envi-' ronment and arrange the obtained Q-values in descending order from highest to lowest for the EV to adopt the action with the highest value. While Van Hasselt [2] thoroughly explained the DDQN algorithm, the performance of the algorithm was investigated and tested in [28] on the Atari  2600  games  console,  achieving  performance  close  to  a  human level. Specifically, they built a massively distributed architecture that lays the foundation for other applications to utilize the DDQN algorithm, based on four major aspects: actors that influence new behavior; artificial learners, stored consistently, that are trained based on a previous learning  experience;  a  neural  network  to  model  a  value  function  in accordance with a behavioral policy; and, eventually, a set of continually stored experiences to direct the next step of learners.

In this work, we mainly relied on the DDQN algorithm to model and represent the EV drive s learning experience to achieve optimal energy ' consumption  by  routing  the  vehicle  toward  an  alternative  path  that reduces the vehicle s energy utilization. Metadata for the region of study ' in  this  experiment  was  acquired  from  Google s  API  platform,  as '

GLYPH&lt;0&gt;5GLYPH&lt;0&gt;HGLYPH&lt;0&gt;VGLYPH&lt;0&gt;HGLYPH&lt;0&gt;D

T.M. Aljohani et al.

explained in Section 3 of this work.

## 2.4. Markov chain modeling of traffic dynamics

## 2.3. Operation modes

The main mission of the neural network in this work is to allow mapping from Rn  to Rm, correspondingly from the state,  , to updated s Q s ( ,. ) .The input is a two-dimensional vector that represents coordinates of  the  geocoded  location  of  the  agent,  while  the  output  is  an  eightdimensional vector that contains the generated Q-values representing the possible actions, i.e., the  directions in front of our agent (North, South, West, East, Northeast, Northwest, Southeast, and Southwest). We carefully integrated the hidden layers between the input and output to avoid any data trapping. Network depth represents the capacity of our framework, and proper modeling of the problem is required, as establishing extra or fewer neurons than needed in the hidden layers could lead to significant issues of either over-fitting or under-fitting. Moreover, misrepresentation  of  the  neurons ' numbers  results  in  trapping  the training data in extreme simulation times that are impractical [29, 30, 31].

In our energy consumption minimization framework, the input layer represents the metadata information retrieved from Google s API plat-' form, by means of geocoding that comprises the longitude and latitude of the starting location for each segment of the road during the trip. It is followed by two connected hidden layers, with the first hidden layer of 12 neurons connected to the second hidden layer of 10 neurons. Lastly, the output represents an eight-dimensional layer of eight actions, which are the directions that are offered for the EVs to choose from. In this experiment, we set the drop-out rate to 0.30, and the learning rate to 0.1 for every step. It worth noting that the drop-out rate is a regularization factor that is used to reduce overfitting and improve generalization in deep learning process. On the other hand, learning rate is a factor that controls the speed of the learning process in the neural network model. Fig. 2 shows the integrated neural network for our framework. It should be noted that the Q-learning algorithm converges, given that two conditions are met. The first condition is that the reward levels are bounded, meaning that there exists a positive scalar k , such that the absolute value of r ( s , a ) is always less than k ,  while the second condition is that the agent strictly explores all the action-state pairs produced in the learning experience. We emphasize that both conditions were met while conducting our experiment.

Output is 8 dimensional Vector that represent list of Actions with Q-values

Fig. 2. Integrated NN for our proposed framework.

![Image](0_Artifacts/[aljohani2020]_Real-Time_metadata-driven_routing_optimization_for_electric_vehicle_energy_consumption_minimization_using_deep_reinforcement_learning_and_Markov_chain_model_artifacts/image_000003_ab8a99b9e03ac6986a405db0a505b22ad86bbb33aaee45f06994ce21c2338f5e.png)

MCM is a popular method for generating traffic models, which we utilize in this work to model EV driving patterns and their corresponding energy consumption. The microscopic behavior of the MCM allows us to model the journey and driving possibilities of the EV realistically. The performance of the MCM was initially evaluated in [32] for capturing and modeling a dynamically complex system. It is also suitable for use with large datasets containing millions of data, and hence, it is useful for urban network traffic modeling as it can handle a large number of roads. MCM is based on a stochastically discrete process with finite states. In a homogenous MCM, these states change probabilistically at each time step. The probabilities of transition between these states largely depend on the state of the previous time step.

Modeling roads and vehicle travel is dynamic and is difficult to estimate properly. Consequently, the best way to handle these difficulties in the analysis of complex dynamic systems is to perturb them to produce the Markov model for the agent s travel and energy consumption ' requirements that we can integrate with our RL energy minimization framework.  As  required  for  any  dynamic  system,  we  need  to  set  a reference point to quantify the contribution from all involved points in the boundary of study. Such a reference point is set up to establish the probability of transition between any two states. We define the region of study (G, T) to be a mapping from T: G , where G is a non-empty subset belonging to the space T and is partitioned into mutually exclusive and collectively exhaustive connected sets { S 1 , S 2 , ..., ST } , where T is the total number of states. Each set is labeled with a specific state i of the Markov chain.

To put it succinctly, the Markov chain modeling process starts when the agent moves from state si to state sj , in what is known as a step. Such a move can be continued successively, with Pij describing the probability of moving between any two states. The collection of these probabilities forms the transition probabilities, which in return, establish a matrix of associated transitional probabilities, known as the transition matrix TM needed to model the EV s drive. '

The  drive  starts  at  a  particular,  pre-defined  location  (the  agent starting position), which establishes the initial probability distribution for the MCM process. It should be noted that the process could be a standstill in a specific state, which, in this case, will have a probability of Pii . Generally speaking, when we have r states in an experiment, then the ij th element of the transition matrix after n steps can be found using

<!-- formula-not-decoded -->

After calculating p and pn ,  we normalize pn so that the sum of all probabilities equals 1, as follows:

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

It should be highlighted that the transition matrix TM must have no negative entries since the sum of probabilities will be added to one, with the probabilities of transitions forming its diagonal entries. However, we show in the next section how we deal with negative entries that are injected due to the regenerative braking property of the EV. Also, in this work, a set of graphs is used to model the EV s geocode history for each ' drive and is retrieved from Google s API as described in part 1 of the next ' section.

We assume in our modeling that all transition matrices produced are primitive  and  irreducible.  The  Mean  First  Passage  Time  (MFPT),  an average factor that assess timescale for a stochastic event to first occur, estimates the possible paths between vertexes in each episode in the road randomly [33]. Modeling the road of interest can be achieved using

T.M. Aljohani et al.

Markov  chain  with vertexes representing specific geocodes that resemble intersections on roads. Vertexes i and j are connected by an oriented graph G if there is a road segment between them such that G = 〈 Gi , Gj 〉 is represented by the weights pij if pij &gt; 0. We define the weight Wij as the average energy consumed during the drive between vertexes i and  , as we will discuss in part 2 of Section 3 of this work. Segments j connect the vertexes whenever there is a drivable path. Fig. 3 provides an illustrative example of graph modeling, considering MCM. Vertexes G 1 , … , G 4 represent road points in a region where an EV is driving. The segment G G 1 2 is the connecting road between geocodes represented as G 1and G 2, which represent the geocodes found in what would be the least  energy-consuming  path,  based  on  the  learning  experience.  It  is worth mentioning that the main concept in MCM is its description of the actual transition from one node to another as a step unit of time. In this work, we modify this concept to represent the energy consumption information between these nodes instead, in the form of a unit step of energy consumed. We considered in our model the extension of the work in [34, 35].

## 3. RL for energy management of the EVs

RL  provides  a  promising  learning  environment  for  different  EV studies.  As  mentioned  earlier,  RL  focuses  on  the  cumulative  return instead of immediate reward. Likewise, the EV s energy management ' focuses  on  minimizing  the  energy  consumption  of  the  entire  driving cycle. As an agent, the EV s energy management relies on information on ' its current vehicle power demand, battery level, and driving conditions, including speed, road condition, etc.

Similarly,  RL  ensures  that  the  agent  only  needs  the  current  state knowledge  and  resultant  reward;  it  does  not  have  to  acquire  prior knowledge of the whole system state. Our RL experiment is based on the DDQN utilized in the TensorFlow open library. In this experiment, the EV serves as our agent navigating in an environment (Google s Map ' platform). The set of actions to be made is to choose the best direction in the driving cycle, in accordance with the energy consumption minimization requirement in the battery model that is set up for eight choices, as described previously.

The main concept of learning is that the EV, as an agent, randomly starts the drive, providing geocodes of its current location as an input layer to the neural network. The two inner layers have Rectifier Linear Unit (ReLU) serving as an activation function, with the set of actions gradually reduced as a result of feedback from the DDQN algorithm, which consists of arranging decisions (actions) based on their respective Q-values, provided in the output layer of the network.

The amount of energy consumed due to changes of state during the EV s drive is modeled, based on the MCM, as a step unit of energy. ' Similar changes were made in other studies, where the authors in [36], for example, utilized the step unit as a unit of pollution in their modeling rather  than  a  unit  of  time.  The  GridSearch  tool  was  used  for  hyperparameter  tunning  in  this  work.  This  defines  a  boundary  for  each

Fig. 3. Example of graph representation considering MCM.

![Image](0_Artifacts/[aljohani2020]_Real-Time_metadata-driven_routing_optimization_for_electric_vehicle_energy_consumption_minimization_using_deep_reinforcement_learning_and_Markov_chain_model_artifacts/image_000004_5aa19291b811bee71b0dfd103ec9c94ed9b82135f5943dc984f26c0b1dc00f90.png)

hyperparameter, which values will be tested. Our discounting factor and learning rate were set to be 0.85 and 0.1, respectively, in this experiment. Losses along the drive train are assumed to be 15%, and only 50% of the energy can be retained during deceleration [37]. The effect of the regenerative  braking  has  been  carefully  considered  in  this  study,  as described in part 5 of this section.

## 3.1. Interaction with Google s API '

The Google Maps Geocoding API is a service provided by Google to transform any physical location into geographical coordinates on a map. We utilize the geocoding API to get the navigation geocodes needed in the experiment, from the starting through the destination positions. We used both the starting and destination geocodes to construct a rectangular boundary on the grid map so that the system allows the EV to navigate only within its boundaries. The grid map may not always be a rectangle, given spherical geometry and any restriction imposed on the length of the stride, with the stride being the distance of a step (segment) the vehicle drives toward its final destination.

In our study, the EV is set to move in one of eight possible directions. Therefore, the Direction API iteratively provides eight possible navigation instructions to the agent at each state. The Elevation API provides the height of each position by using the navigation API instruction list geocodes. We highlight that, in the case of an unreachable position on the map (e.g., a lake on the road), then the Direction API will return 'FALSE , whereas in cases of a reachable position, the route is evaluated ' by making sure the agent will only navigate within the boundary of the grid map utilized. Google s Roads API provides the agent with real-time ' metadata concerning the road to be navigated, such as imposed speed limits, number of traffic signals and stop signs, traffic congestion, etc. Such information is important to determine the optimal energy consumption route for our agent throughout the learning process. We also highlight that raw traffic data were estimated in this experiment based on the returned ' duration in traffic ' results for each trip provided in Google s Duration, as access to instantaneous traffic data is restricted ' and hard to obtain.

## 3.2. Energy consumption in the Markov Chain traffic model

As mentioned in Section 2, modeling the road of the region of interest can be achieved using an oriented graph, where each vertex represents a specific geocoded location, with edges ( Gi , Gj ) as the path between each pair of vertexes. We define the weights w Gi ( , Gj ) which correspond to the average energy consumption of the agent s battery on the ' i th state during the traveling path. These weights can be estimated as follows

<!-- formula-not-decoded -->

Where i = 1 … n, and β can be chosen as the step size in the interval between zero and weights wi . To properly integrate these weights into our problem, we transform the transitional matrix TM into another MCM matrix, QM , as follows:

<!-- formula-not-decoded -->

Where D = diag  ( w 1 , .., wn ),  which  is  a  diagonal  matrix  that  has weights wi = 1, … , n, is the number of vertexes in the grid map, with I as the identity matrix of the same dimensions of the transition matrix TM . Since the step unit in MCM is modeled as a step unit of energy, rather than a step unit of time, the energy consumption is calculated according to the driving style of the agent. In this work, three phases of driving are assumed: cruising, acceleration, and deceleration phases. The sum of energy used is calculated as the integral of the EV dynamics considering the three forces, Facc , Frol , Fad , together with Fslope . In this calculation, Facc , is the accelerating force; Frol is the force for overcoming rolling resistance; Fad is the aerodynamic drag force; and Fslope is the hill-climbing

GLYPH&lt;0&gt;5GLYPH&lt;0&gt;HGLYPH&lt;0&gt;VGLYPH&lt;0&gt;HGLYPH&lt;0&gt;D

GLYPH&lt;0&gt;(GLYPH&lt;0&gt;OGLYPH&lt;0&gt;HGLYPH&lt;0&gt;FGLYPH&lt;0&gt;WGLYPH&lt;0&gt;UGLYPH&lt;0&gt;LGLYPH&lt;0&gt;F GLYPH&lt;0&gt;3GLYPH&lt;0&gt;RGLYPH&lt;0&gt;ZGLYPH&lt;0&gt;HGLYPH&lt;0&gt;U GLYPH&lt;0&gt;6GLYPH&lt;0&gt;\GLYPH&lt;0&gt;VGLYPH&lt;0&gt;WGLYPH&lt;0&gt;HGLYPH&lt;0&gt;PGLYPH&lt;0&gt;V

T.M. Aljohani et al.

force. These dynamics are calculated as:

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

Where α is the EVs acceleration factor; A is the area in front of the vehicle; m is the vehicle mass at speed V ; μ ro 1 , ρ , Cd and g are constants and φ is the incline of the traveled path. Table 1 presents the parameters of the EV modeled in our experiment, where x 1 is the traveled distance of the cruising phase; x 2 is the traveled distance at which the deceleration process starts; while x 3 corresponds to the total length of the travel path. Such information provides accuracy for our modeling, as geophysical metadata  and  relative  limitations  are  reflected in the equations accordingly.

## 3.3. Value-Iteration network

During our simulation, we successfully overcame the issues related to continuous control and visual constraints by depending on the approximation model of the Value Iteration Network (VIN) planning algorithm illustrated in [38]. Specifically, the author of [38] introduced the VI module as a neural network (NN) layer that could encode and enable a differentiable planning computation. As highlighted in reference [42], each iteration of the VIN module can be assumed to pass the previous of both the value and reward functions through both a CNN layer and a max-pooling layer. Considering such an analogy, each channel in the CNN layer will correspond to a Q-function to estimate a specific action. Similarly,  the  convolution kernel  weights  will  correspond  to  weights belong  to  the  discounted  transitional  probabilities.  We  utilize  this concept to develop a VIN structure with backpropagation, to improve the learning process and reduce errors in the obtained travel policy. As described in Fig. 4, the input represents an image of the coordinates at the  current  state  of  the  drive.  The  output  produced,  based  on  the attention and observation logic, influences the travel policy to ensure robust outcomes. From the input, both the Pij , state-transition function, and fr ,  which  is  a  convolutional  neural  network  (CNN)  layer  that transforms the input grid image into another, belong to the reward, with pixels  considered  reward  values.  The  results  from  the  MCM  directly influence the value-function described in Eq. (2). Fig. 5 explains further the mechanism of the VI module in our training process. Specifically, at each iteration, the module treats the input of the probabilities of transitions and reward values from the actions into the CNN layer to influence the value of V.

As we illustrated, each channel in the CNN layer represents a Q-value corresponding to the list of actions produced for the EV. Moreover, the CNN layer provides weights based on the discounting factor and then starts the process of the next iteration. This layer arranges the actions in accordance with their Q-values, where V then provides feedback signals on successive iterations of Z to reach a finalized reward information

Table 1 Parameters of the assumed EV.

| Parameter                     | Value              |
|-------------------------------|--------------------|
| Gravity (g)                   | 9.81 m/s 2         |
| Air density ( P )             | 1.2 kg/m 3         |
| Rolling resistance ( μ ro 1 ) | 0.01               |
| Drag coef. (Cd)               | 0.35               |
| Area in front of the EV ( A ) | 1.6                |
| Acceleration constant (a 1 )  | 3.5 m/s 2          |
| Deceleration constant (a 2 )  | GLYPH<0> 3.5 m/s 2 |
| Mass (m)                      | 1961 kg            |

![Image](0_Artifacts/[aljohani2020]_Real-Time_metadata-driven_routing_optimization_for_electric_vehicle_energy_consumption_minimization_using_deep_reinforcement_learning_and_Markov_chain_model_artifacts/image_000005_84bf64ab99b6255687bedf6e58d498487aec865a71434f81fbfc5919bc0e1e37.png)

Fig. 4. Representation of the VIN structure of our framework.

Fig. 5. Iterations feedback for the value function.

![Image](0_Artifacts/[aljohani2020]_Real-Time_metadata-driven_routing_optimization_for_electric_vehicle_energy_consumption_minimization_using_deep_reinforcement_learning_and_Markov_chain_model_artifacts/image_000006_2908e9a7eae088ecb509126493393003f14482b7f2e80a93481cf226ff2720b4.png)

corresponding  to  the  current  state  of  the  journey.  Once  iterations conclude, the final policy values are produced.

## 3.4. Battery model

The battery model incorporated in our simulation is adapted from [39]. Originally, they utilized the battery model in [40], but incorporating  an  update  to  include  the  transient  response  effect  after  long driving schedules to report an accurate SoC throughout the different driving cycles. Studies [41, 42, 43] present fast time factors of lithium batteries during driving cycles and are incorporated into the calculation of the state of charge by [43] for more accurate representations of battery condition during driving patterns. The SoC, battery s voltage, as ' well as power losses, can be evaluated using the following mathematical formulation:

<!-- formula-not-decoded -->

Where T sec , T min , T hour are  multiple  time-constant  factors  of  the battery consumption behavior with time, with the temperature, number of cycles, and, together with discharge rate, have been adapted from [43]. Vterminal , Voc , are battery s terminal and open circuit voltage level; ' Rint , Rtransient are the internal and trainset resistance of the battery; Cbattery and I battery are the battery s capacity and corresponding current, and is ' modeled as a current source [44]. It is found as follows:

/

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

Parameters used in our simulation study is adopted from table 1 of reference [43]. We highlight that the degradation level of the battery itself is not incorporated in our experiment.

GLYPH&lt;0&gt;5GLYPH&lt;0&gt;HGLYPH&lt;0&gt;VGLYPH&lt;0&gt;HGLYPH&lt;0&gt;D

T.M. Aljohani et al.

## 3.5. Dealing with negative values from regenerative braking

Then, the DDQN target network estimates the Q-value of the action, as follows

Practically  speaking,  the  diagonal  entries  in  the  transition  matrix should be only positive values, which will not be the case considering the impact of the regenerative braking in the EV during the trip as a result of the influence of the weights, which may either lead to zero entry on the diagonal or produce eigenvectors that are highly complex to solve. This leads us to strictly require the energy summation on any road segment  to  never  produce  a  zero-entry  in  the  diagonal.  This  can  be achieved by introducing a medial matrix that has absolute values of the unit step of energy in the traveled path. Accordingly, all the weights are rearranged as follows:

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

Where α indicates the sign of a change of energy transferred from the vehicle to the network and is strictly limited within the range 0 &lt; α &lt; minimum wi .

## 3.6. Application of reinforcement learning

The reward is defined as the relative factor to prioritize the levels of energy consumption minimization from the current position to the next in one step for the EV. It should be noted that the design of any reward signal  is  mainly  heuristic,  aiming  to  accelerate  the  learning  process through providing feedback signals to indicate if desired objectives have been reached. We decided to simplify the reward feedback in the algorithm, rather than establishing a complex reward function, as this usually will not advance the learning process but will result in trapping the agent in local optima. The cornerstone of RL is to allow the agent to navigate through different alternatives based on the reward feedback signal received. The agent starts with an initial state and follows the learning process to achieve the goal of optimal energy consumption ( + 1 reward) while establishing knowledge at the same time on how to escape the other set of states ( GLYPH&lt;0&gt; 1 reward) that are presented in its environment. So, generally speaking, the reward is multiplied by either 1 or GLYPH&lt;0&gt; 1 as follows

<!-- formula-not-decoded -->

The discounted sum of rewards is defined using the optimal value of a state(s) and calculated as follows

<!-- formula-not-decoded -->

Because of uniqueness, its value can be reformulated as

<!-- formula-not-decoded -->

We calculate the optimal control policy as follows

<!-- formula-not-decoded -->

The updated Q-learning algorithm is expressed as

<!-- formula-not-decoded -->

The Q-value is obtained using Eq. (1), while optimal Q-values are calculated as follows

<!-- formula-not-decoded -->

<!-- formula-not-decoded -->

It is important to remember that we have assumed the discounting factor to be 0.85.

## 4. Results and discussion

Consistent  with  the  energy  minimization  requirement,  the  EV optimal routing problem is achieved based on the learning experience. Translating this problem within the context of Q-values, our agent follows  the  learning  process  to  find  a  travel  policy  that  maximizes  the reward of its decisions as a mean of minimizing energy consumption throughout  the  trip.  Through  the  MCM  process,  we  represented  the dynamics of the traveling path as a Markov chain, influenced by pn that are updated throughout the drive to reflect energy consumed at each path  between  two  vertexes.  Our  experiment  protocol  is  established based on DDQN.

The EV is evaluated over 140 episodes of the drive it was trained on, where each episode represents a complete set of steps, states and rewards derived from the agent-environment interaction. The agent was allowed to navigate until the end of each segment of the road or was terminated  if  violates  any  restrictions  placed  on  episodes.  Such  restrictions may be domain-specific, like violating several allowed steps per  episode,  having  already  achieved  the  objective  of  driving  that segment optimally, or simply returning FALSE due to an un-derivable path option (e.g., flooding or a permanently closed road, among others).

As the agent is a purely electric vehicle, the only source available to its drive is the energy stored in its battery. Therefore, our objective in training the agent is to find the optimal path that requires least energy consumption to get the agent to its final destination, by extending the driving range of the vehicle as much as possible. However, we emphasize that some routes may actually result in more drive time for the EV s ' owner. Therefore, the duration of the drive is neglected in our study, assuming that the EV driver s main concern is to reach the charging ' location with the most optimal energy.

Two routes have been selected to test the accuracy and strength of our proposed energy consumption minimization framework. The first route starts at the College of Engineering and Computing of the Florida International University (FIU), Miami, Florida and the destination is set as an electric vehicle charging station located nearly six miles away, in Doral, Florida. The second route was chosen in a location that exhibits different  geographic  characteristics,  to  add  more  credibility  to  the testing of the proposed strategy.

It starts at the famous J. Paul Getty Museum, located at a hilltop site in  the  Santa  Monica Mountains in West Los Angeles, California. The destination is set as a charging station located 5.8 miles away in Ventura, California. The geocodes of the starting and ending locations of each route are given in table 2, whereas Table 3 shows the total number of episodes and steps obtained during the simulation of both routes. The type of car used in both experiments is assumed to be a Tesla V, and its parameters are shown in Table 1 in part 2 of Section 3.

We assume 200 m as a length of stride to evaluate the energy needed to travel between any two vertexes in the grid map, with no more than 80  steps  within  an  episode;  otherwise,  it  will  be  considered  an  unreachable position and thus labeled as a failed step. The battery degradation level has been ignored in this study, as the focus of this study is on finding optimal routing to minimize the energy requirements of the EV. Specific features of the dynamic environment, such as time, location and route, could be different from one experiment to another. It should be noted that the performed date and time for these trips were April 25th at 4:30 PM for each location. It is also of interest to highlight that the energy for internal utilization (e.g., using the air conditioner in the car during the trip) has been ignored in this experiment.

GLYPH&lt;0&gt;5GLYPH&lt;0&gt;HGLYPH&lt;0&gt;VGLYPH&lt;0&gt;HGLYPH&lt;0&gt;D

T.M. Aljohani et al.

Table 2

Experimental information and results.

| Routes                                                                     | Starting Geocodes              | Destination Geocodes           | Energy consumed by framework   | Energy consumed by Google ' s main route   | Energy from Regenerative Braking   | Simulation time   |
|----------------------------------------------------------------------------|--------------------------------|--------------------------------|--------------------------------|--------------------------------------------|------------------------------------|-------------------|
| FIU college of Engineering & Computing - Doral ' s EV ' s Charging Station | 25.768506, GLYPH<0> 80.366891  | 25.809732, GLYPH<0> 80.331379  | 1.9388 Kwh at 25 min           | 2.0548 Kwh at 19 min                       | Negligible                         | 3709 s            |
| J. Paul Getty Museum - Ventura ' s EV Charging Station                     | 34.077823, GLYPH<0> 118.475863 | 34.158980, GLYPH<0> 118.499940 | 2.1066 kwh at 31 min           | 2.3548 kwh at 22 min                       | 0.211 kwh                          | 2881 s            |

Table 3 Total number of episodes and steps per route.

|                          |   No. of Episodes |   No. of Failed episodes |   Steps |   Unreachable positions |
|--------------------------|-------------------|--------------------------|---------|-------------------------|
| First Route Second Route |               140 |                       20 |    5715 |                    1711 |
|                          |               143 |                       18 |    7226 |                    1501 |

The simulation was carried out in an extended Python environment, with TensorFlow, NumPy, and Pandas library requirements, at the FIU energy systems research laboratories. The input data for this simulation were the current geocodes for each vehicle, which provided the physical context around the vehicle and correspondent data related to the road (e.g.,  elevation, height, traffic flow, and allowed speed, among other factors).  These  data  were  imported  into  the  simulation directly  from Google s  Map  platform,  where  a  combination  of  APIs  and  Software ' Development Kits (SDKs) allow retrieval of information from Google s ' map systems. Also, we highlight that traffic information and data were not  easily  obtained,  as  we  had  to  query  the  Google s  Directions  API ' every time during the simulation to estimate the traveling time for each route considering real-time traffic conditions.

The simulation takes into consideration the energy consumption for each state during the vehicle traveling, and then produced the results for the best optimal route that saves energy the most during the proposed trip. Then, the results are compared with those obtained when strictly imposing Google s suggested route on the vehicle. '

Fig. 6 and Fig. 7 show the consumed energy throughout the trip, based on our framework versus Google s suggested routes for the trips ' taken in Miami, FL, and Los Angeles, CA, respectively. The results show that, in both simulation scenarios, the EV can reach its intended destination with lower energy than the main route proposed by Google Maps,

Fig. 6. Energy consumption of the first route by the proposed framework vs. Google s suggested route. '

![Image](0_Artifacts/[aljohani2020]_Real-Time_metadata-driven_routing_optimization_for_electric_vehicle_energy_consumption_minimization_using_deep_reinforcement_learning_and_Markov_chain_model_artifacts/image_000007_1b73551d66000e38538fd581ad60c3d831dcc3dacea024c1c5c35cb33211262e.png)

Fig. 7. Energy consumption for the second route by the proposed framework vs. Google s suggested route. '

![Image](0_Artifacts/[aljohani2020]_Real-Time_metadata-driven_routing_optimization_for_electric_vehicle_energy_consumption_minimization_using_deep_reinforcement_learning_and_Markov_chain_model_artifacts/image_000008_20dd2168f91470e4a652d486564b6861ad8097e9a8dc55d10001d3c98b6b7467.png)

by 5.89% and 11.82%. The reason for such differences in the reported results is due to the significant temporal and spatial differences at each location, which influence the level of energy consumption.

The results of both trips are presented in Table 3. It is noted that no detectable amount of energy was produced as a result of the regenerative braking mechanism, as the value recorded in the first route was trivial, while we can mark 0.211 kWh regenerated energy in the second route experiment. This amount of reproduced energy is attributed to driving downhill to reach Ventura from the hilltop location of the Getty Museum. We emphasize that more energy might have been consumed if the route had been in the opposite direction, such that if the drive was toward the museum instead of away from it.

Fig. 8. Reward details per number of steps in the first route Framework vs. Google s suggested route. '

![Image](0_Artifacts/[aljohani2020]_Real-Time_metadata-driven_routing_optimization_for_electric_vehicle_energy_consumption_minimization_using_deep_reinforcement_learning_and_Markov_chain_model_artifacts/image_000009_6e1a6c8741cd8d7e4e42906a18d374ccc2620d4aa42eb291924d0af718f4807c.png)

GLYPH&lt;0&gt;5GLYPH&lt;0&gt;HGLYPH&lt;0&gt;VGLYPH&lt;0&gt;HGLYPH&lt;0&gt;D

T.M. Aljohani et al.

Based on 140 episodes of simulation, Figs. 8 and 9 present the level of reward returns as feedback that influenced the drive of the agent in the two  routes.  The  reward  value  is  influenced  by  the  number  of  steps received by the learning experiment, returning negative values whenever there is a failed episode. Furthermore, the returned reward presented in Figs. 8 and 9 provides insight into the level of benefits of each action taken by the EV at each state throughout the trip; it shows the accumulated sum achieved for each step of the travel policy, based on the discounted rate set earlier, γ , at each time-step  . We highlight that t the produced rewards of the experiment show low variation, which is an indication of the robustness of our proposed framework in executing actions within the timeframe of the experiment. However, we believe that the limitation of the number of episodes per experiment, due to the highly constrained  access  to Google s  API  platform,  has  greatly ' restricted our ability to perceive variant rewards. It is of note that, as the training process advanced, the random behavior decreased significantly, as shown in Fig. 10.

This has an immediate influence on the obtained reward, as the trend of  the  returned  rewards  indicates  more  positive  outcomes  and  less negative values that represent failed steps. It is also noteworthy that the agent starts the navigation on a limited random action basis, but the impact of the learning process reduces this randomness effectively, as Fig.10 shows for the first route. Throughout the journey, it is observed that the level of SoC improves with the improving certainty of actions taken as a result of improvement in energy consumption retained in the battery.

## 5. Conclusion

An  energy  minimization  framework  was  developed  in  this  work, based on reinforcement learning, to optimize the energy consumption of EVs. The main concept of reaching optimal decisions for the EV movement  is  achieved  based  in  on  the  deep  reinforcement  learning.  The DDQN was utilized in order to overcome any potential overestimation of Q-values produced from the target network. A carefully integrated but appropriate neural network was designed to provide feedback to the agent as a means of optimizing the travel policy. For the EV s traffic ' dynamics, we utilized the MCM concept to model each part of the roads in a sequenced manner, based on successive portioning.

To verify the proposed framework in this work, two experiments were conducted considering two routes of similar length, but at locations that exhibit different geographic characteristics. Both of the experiments were set at a specific time and date, assuming limited SoC of the EV s battery. The results of the simulations show that the energy ' consumption based on the proposed framework is considerably lower than when following the main routes proposed by Google Maps, for each journey.

Additionally, we considered the journey of the EV to be at continuous states, and the transitional probabilities are updated throughout as the agent considers real-time data absorbed into the framework from the Google Maps Platform. It is worth mentioning that a greater number of occurrences  would  be  desirable  to  produce  more  accurate  results. However, this would have resulted in an extremely long simulation time, possibly extending to days, in addition to which restrictions from Google s API platform occasionally interrupt the simulations. '

## Credit author statement

Conceptualization, T.M.A. and A.F.E.; Methodology, T.M.A. and A.F. E. Software, T.M.A. and A.F.E.; Validation, T.M.A, A.F.E and O.A.M.; Formal Analysis, T.M.A.; Investigation, T.M.A.; Resources, T.M.A.; Data Curation, T.M.A. and A.F.E.; Writing-Original Draft Preparation, T.M.A.; Writing-Review &amp; Editing, T.M.A, A.F.E and O.A.M.; Visualization, T.M. A.  and  A.F.E.;  Supervision,  O.A.M.;  Project  Administration,  O.A.M.; Funding Acquisition, T.M.A.

Fig. 9. Reward returns per number of steps in the second route.

![Image](0_Artifacts/[aljohani2020]_Real-Time_metadata-driven_routing_optimization_for_electric_vehicle_energy_consumption_minimization_using_deep_reinforcement_learning_and_Markov_chain_model_artifacts/image_000010_d09d23b9b74b5fb9dd7fc3524c773074d1105fc9b2683f12f5991cb3d8cab1ca.png)

Fig. 10. SoC vs. probability of taking random actions.

![Image](0_Artifacts/[aljohani2020]_Real-Time_metadata-driven_routing_optimization_for_electric_vehicle_energy_consumption_minimization_using_deep_reinforcement_learning_and_Markov_chain_model_artifacts/image_000011_5f9273ae47e0babaf6701d4bb02ee01f174c7765028f40e2366a8091f6cfe475.png)

## Declaration of Competing Interest

We wish to confirm that there are no known conflicts of interest associated  with  this  publication  and  there  has  been  no  significant financial support for this work that could have influenced its outcome. We confirm that  the  manuscript  has  been  read  and  approved  by  all named authors and that there are no other persons who satisfied the criteria for authorship but are not listed. We further confirm that the order of authors listed in the manuscript has been approved by all of us. We understand that the Corresponding Author is the sole contact for the Editorial process (including Editorial Manager and direct communications  with  the  office).  He  is  responsible  for  communicating  with  the other  authors  about  progress,  submissions  of  revisions  and  final approval of proofs. We confirm that we have provided a current, correct email address which is accessible by the Corresponding Author. Aljohani, et al.

## References

- [1] Van Hasselt, H., Guez, A., &amp; Silver, D. (2016, March). Deep reinforcement learning with double q-learning. In Thirtieth AAAI Conf. Artif. Intell.
- [2] Hasselt, H.V. (2010). Double Q-learning. In Advances in Neural Information Processing Systems (pp. 2613 -2621).
- [3] P. Dayan, C.J.C.H. Watkins, Q-learning, Mach Learn 8 (3) (1992) 279 -292.

GLYPH&lt;0&gt;5GLYPH&lt;0&gt;HGLYPH&lt;0&gt;VGLYPH&lt;0&gt;HGLYPH&lt;0&gt;D

GLYPH&lt;0&gt;(GLYPH&lt;0&gt;OGLYPH&lt;0&gt;HGLYPH&lt;0&gt;FGLYPH&lt;0&gt;WGLYPH&lt;0&gt;UGLYPH&lt;0&gt;LGLYPH&lt;0&gt;F GLYPH&lt;0&gt;3GLYPH&lt;0&gt;RGLYPH&lt;0&gt;ZGLYPH&lt;0&gt;HGLYPH&lt;0&gt;U GLYPH&lt;0&gt;6GLYPH&lt;0&gt;\GLYPH&lt;0&gt;VGLYPH&lt;0&gt;WGLYPH&lt;0&gt;HGLYPH&lt;0&gt;PGLYPH&lt;0&gt;V

T.M. Aljohani et al.

- [4] Mnih, V., Kavukcuoglu, K., Silver, D., Graves, A., Antonoglou, I., Wierstra, D., &amp; Riedmiller, M. (2013). Playing atari with deep reinforcement learning. arXiv preprint arXiv:1312.5602.
- [5] Valogianni, K., Ketter, W., Collins, J., &amp; Zhdanov, D. (2014, June). Effective management of electric vehicle storage using smart charging. In Twenty-Eighth AAAI Conference on Artificial Intelligence.
- [6] T. Liu, Y. Zou, D. Liu, F. Sun, Reinforcement learning of adaptive energy management with transition probability for a hybrid electric tracked vehicle, IEEE Trans. Ind. Electron. 62 (12) (2015) 7837 -7846.
- [7] X. Qi, G. Wu, K. Boriboonsomsin, M.J. Barth, J. Gonder, Data-driven reinforcement learning-based real-time energy management system for plug-in hybrid electric vehicles, Transp. Res. Rec. 2572 (1) (2016) 1 -8.
- [8] H. Wang, T. Huang, X. Liao, H. Abu-Rub, G. Chen, Reinforcement learning in energy trading game among smart microgrids, IEEE Trans. Ind. Electron. 63 (8) (2016) 5109 5119. -
- [9] B.G. Kim, Y. Zhang, M. Van Der Schaar, J.W. Lee, Dynamic pricing and energy consumption scheduling with reinforcement learning, IEEE Trans. Smart Grid 7 (5) (2015) 2187 2198. -
- [10] E. Foruzan, L.K. Soh, S. Asgarpoor, Reinforcement learning approach for optimal distributed energy management in a microgrid, IEEE Trans. Power Syst. 33 (5) (2018) 5749 5758. -
- [11] T. Liu, X. Hu, S.E. Li, D. Cao, Reinforcement learning optimized look-ahead energy management of a parallel hybrid electric vehicle, IEEE/ASME Trans. Mech. 22 (4) (2017) 1497 1507. -
- [12] A.E. Sallab, M. Abdou, E. Perot, S. Yogamani, Deep reinforcement learning framework for autonomous driving, Electron. Imag. 2017 (19) (2017) 70 -76.
- [13] M.M. Karbasioun, I. Lambadaris, G. Shaikhet, and E. Kranakis, ' Optimal charging strategies for electrical vehicles under real-time pricing, ' in Proc. IEEE Int. Conf. Smart Grid Commun., Nov. 2014, pp. 746 -751.
- [14] D. Tang, P. Wang, Probabilistic modeling of nodal charging demand based on spatial-temporal dynamics of moving electric vehicles, IEEE Trans. Smart Grid 7 (2) (2016) 627 -636. Mar.
- [15] H.R. Beom, H.S. Cho, A sensor-based navigation for a mobile robot using fuzzy logic and reinforcement learning, IEEE Trans. Syst. Man Cybern 25 (3) (1995) 464 477. -
- [16] V. Gullapalli, J.A. Franklin, H. Benbrahim, Acquiring robot skills via reinforcement learning, IEEE Control Syst. Mag. 14 (1) (1994) 13 -24.
- [17] Y. Hasegawa, T. Fukuda, K. Shimojima, Self-scaling reinforcement learning for fuzzy logic controller-applications to motion control of two-link brachiation robot, IEEE Trans. Ind. Electron. 46 (6) (1999) 1123 -1131.
- [18] S.Y. Oh, J.H. Lee, D.H. Choi, A new reinforcement learning vehicle control architecture for vision-based road following, IEEE Trans. Veh. Technol. 49 (3) (2000) 997 1005. -
- [19] E. Barrett, E. Howley, J. Duggan, Applying reinforcement learning towards automating resource allocation and application scalability in the cloud, Concurr. Comput. 25 (12) (2013) 1656 -1674.
- [20] G. Rafiee, S.S. Dlay, W.L. Woo, A review of content-based image retrieval. In 2010 7th International Symposium On Communication Systems, Networks &amp; Digital Signal Processing (CSNDSP 2010), IEEE, 2010, pp. 775 -779.
- [21] M. Asada, S. Noda, S. Tawaratsumida, K. Hosoda, Vision-based reinforcement learning for purposive behavior acquisition, in: In Proceedings of 1995 IEEE International Conference On Robotics and Automation, 1, IEEE, 1995, May, pp. 146 -153.
- [22] A. Bonarini, Evolutionary learning, reinforcement learning, and fuzzy rules for knowledge acquisition in agent-based systems, Proc. IEEE 89 (9) (2001) 1334 1346. -
- [23] W. Liu, G. Qin, Y. He, F. Jiang, Distributed cooperative reinforcement learningbased traffic signal control that integrates v2x networks ' dynamic clustering, IEEE Trans. Veh. Technol. 66 (10) (2017) 8667 -8681.
- [24] X. Huang, T. Yuan, G. Qiao, Y. Ren, Deep reinforcement learning for multimedia traffic control in software defined networking, IEEE Netw. 32 (6) (2018) 35 -41.
- [25] A. Ortiz, H. Al-Shatri, X. Li, T. Weber, A. Klein, Reinforcement learning for energy harvesting point-to-point communications. In 2016 IEEE International Conference on Communications (ICC), IEEE, 2016, May, pp. 1 -6.
- [26] N.C. Luong, D.T. Hoang, S. Gong, D. Niyato, P. Wang, Y.C. Liang, D.I. Kim, Applications of Deep Reinforcement Learning in Communications and networking: A survey, IEEE Communications Surveys &amp; Tutorials, 2019.
- [27] R.S. Sutton, A.G. Barto, Reinforcement Learning: An Introduction, MIT press, 2018.
- [28] Nair, A., Srinivasan, P., Blackwell, S., Alcicek, C., Fearon, R., De Maria, A., Panneershelvam, V., Suleyman, M., Beattie, C., Petersen, S. and Legg, S., 2015. Massively parallel methods for deep reinforcement learning. arXiv preprint arXiv:1 507.04296.
- [29] G. Cybenko, ' Approximations by superpositions of sigmoidal functions, Math. Control Signals. Syst. 2 (4) (1989) 303 -314.
- [30] Kurt Hornik, ' Approximation capabilities of multilayer feedforward networks, Neural Netw. 4 (2) (1991) 251 -257, https://doi.org/10.1016/0893-6080(91) 90009-T.
- [31] G.E. Hinton, S. Osindero, Y.W. Teh,  A fast learning algorithm for deep belief nets ' ' (PDF), Neural Comput 18 (7) (2006) 1527 -1554.
- [32] G. Froyland, Extracting dynamical behavior via Markov models. In Nonlinear Dynamics and Statistics, Birkhauser, Boston, MA, 2001, pp. 281 ¨ -321.
- [33] J.G. Kemeny and J.L. Snell. Finite Markov Chains. Van Nostrand, Princeton, NJ, USA, 1960.
- [34] R. Maia, M. Silva, R. Ara\_ujo, and U. Nunes. Electric vehicle simulator for energy consumption studies in electric mobility systems. In Proc. IEEE Forum on Integrated and Sustainable Transportation Systems, pages 227 -232, Vienna, Austria, June 2011.
- [35] Schlote Crisostomi E., S. Kirkland, R. Shorten, Tra\_c modelling framework for electric vehicles, Int. J. Control 85 (7) (2012) 880 -897.
- [36] E. Crisostomi, S. Kirkland, and R. Shorten. Markov Chain based emissions models: a precursor for green control. In J.H. Kim and M.J. Lee, Eds., Green IT: Technologies and Applications, pages 381 -400. Springer, Heidelberg, 2011.
- [37] E. Apostolaki-Iosifidou, P. Codani, W. Kempton, Measurement of power loss during electric vehicle charging and discharging, Energy 127 (2017) 730 -742.
- [38] A. Tamar, Y. Wu, G. Thomas, S.;. Levine, P. Abbeel, Value iteration networks. In Advances in Neural Information Processing Systems, 2016, pp. 2146 -2154.
- [39] R.C. Kroeze, P.T. Krein, Electrical battery model for use in dynamic electric vehicle simulations. In 2008 IEEE Power Electronics Specialists Conference, IEEE, 2008, pp. 1336 -1342.
- [40] M. Chen, G.A. Rincon-Mora, Accurate electrical battery model capable of predicting runtime and I-V performance, IEEE Trans. Energy Conversion 21m (2) (Jun. 2006) 504 -511.
- [41] B. Schweighofer, K.M. Raab, G. Brasseur, Modeling of high-power automotive batteries by the use of an automated test system, IEEE Trans. Instrum. Meas. 52 (4) (2003) 1087 1091. Aug. -
- [42] S. Gold, ' A PSPICE macromodel for lithium-ion batteries, ' in Proc. 12th Annu. Battery Conf. Applications and Advances, 1997, pp. 215 -222.
- [43] L. Gao, S. Liu, R.A. Dougal, Dynamic lithium-ion battery model for system simulation, IEEE Trans. Component. Packag. Tech 25 (3) (2002) 495 -505. Sept.
- [44] C. Sun, S.J. Moura, X.S. Hu, F.C. Sun, Dynamic traffic feedback data enabled energy management in plug-in hybrid electric vehicles, IEEE Trans. Control Syst. Technol. 23 (3) (2015) 1075 -1086. May.

GLYPH&lt;0&gt;5GLYPH&lt;0&gt;HGLYPH&lt;0&gt;VGLYPH&lt;0&gt;HGLYPH&lt;0&gt;D