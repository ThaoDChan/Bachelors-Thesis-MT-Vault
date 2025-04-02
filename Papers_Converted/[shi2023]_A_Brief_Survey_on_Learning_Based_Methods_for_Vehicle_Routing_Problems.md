A

v

a

i

l

b

e

o

n

t

Available online at www.sciencedirect.com

Available online at www.sciencedirect.com

Available online at www.sciencedirect.com

Procedia Computer Science 00 (2023) 000-000

![Image](image_000000_529e019127630b95c7d53b304ccfbbbe12dbf2262608fb175923da988ef244d4.png)

Available online at www.sciencedirect.com

![Image](image_000001_cb07fd54c9b6400c6d10c92a75ddfb73db858135647802273cf8c79368f68e84.png)

w

.

s

c

i

e

n

d

r

t

o

m

Procedia Computer Science 00 (2023) 000-000

Procedia Computer Science 00 (2023) 000-000

Procedia Computer Science 00 (2023) 000-000

t

S

c

i

e

n

D

r

Information Technology and Quantitative Management (ITQM 2023) Procedia Computer Science 00 (2023) 000-000

Procedia Computer Science 221 (2023) 773-780 Information Technology and Quantitative Management (ITQM 2023) Procedia Computer Science 00 (2023) 000-000

Information Technology and Quantitative Management (ITQM 2023

Information Technology and Quantitative Management (ITQM 2023) )

Information Technology and Quantitative Management (ITQM 2023)

A Brief Survey on Learning Based Methods for Vehicle Routing

## A Brief Survey on Learning Based Methods for Vehicle Routing A Brief Survey on Learning Based Methods for Vehicle Routing Problems e Management (ITQM 2023) A Brief Survey on Learning Based Methods for Vehicle Routing Information Technology and Quantitativ

## Problems Ruiyang Shi a,c , Lingfeng Niu b,c,1 Problems s ethods for Vehicle Routing Ruiyang Shi a,c c , Lingfeng Niu b,c,1 Ruiyang Shi a,c , sed Methods for Vehicle Routing Lingfeng Niu b,c,1 a School of Mathematical Sciences, University of Chinese Academy of Sciences, Beijing 100049, China b School of Economics and Management, University of Chinese Academy of Sciences, Beijing 100190, China Problem M Ruiyang Shi a, , Lingfeng Niu b,c,1 A Brief Survey on Learning Based Problems A Brief Survey on Learning Ba Problems

c

a

b School of Economics and Management, University of Chinese Academy of Sciences, Beijing 100190, China c CAS Research Center on Fictitious Economy ement, University of Chinese Academy of Sciences, Beijing 100190, China &amp; ng Shi Data Science, University of Chinese Academy of Sciences, Beijing 100190, China b School of Econom cs and Manag ement, University of Chinese Academy of Sciences, Beijing 100190, China c CAS Research Center on Fictitious Economy ces, University of Chinese Academy of Sciences, Beijing 100049, China &amp; Data Science, University of Chinese Academy of Sciences, Beijing 100190, China School of Mathematical Sciences, University of Chinese Academy of Sciences, Beijing 100049, China b School of Economics and Manag c CAS Research Center on Fictitious Economy &amp; Data Science, University of Chinese Academy of Sciences, Beijing 100190, China a School of Mathematical Scien Ruiya a,c , Lingfeng Niu b,c,1

School of Mathematical Sciences, University of Chinese Academy of Sciences, Beijing 100049, China 00190, China a School of Mathematical Sciences, University of Chinese Academy of Sciences, Beijing 100049, China a Ruiyang Shi a,c , Lingfeng Niu b,c,1

CAS Research Center on Fictitious Economy

&amp;

Data Science, University of Chinese Academy of Sciences, Beijing 1

b

School of Economics and Management, University of Chinese Academy of Sciences, Beijing 100190, China

a

c

CAS Research Center on Fictitious Economy ment, University of Chinese Academy of Sciences, Beijing 100190, China

&amp;

Data Science, University of Chinese Academy of Sciences, Beijing 100190, China

School of Mathematical Sciences, University of Chinese Academy of Sciences, Beijing 100049, China

b

School of Economics and Manage

CAS Research Center on Fictitious Economy

## Abstract c

&amp;

Data Science, University of Chinese Academy of Sciences, Beijing 100190, China

Abstract Vehicle Routing Problem (VRP) is a kind of combinatorial optimization problem with extensive application scenarios. At present, many methods for solving VRPs have been proposed, which can be divided into exact methods and heuristic methods. t t However, due to the complexity of VRPs, exact methods are limited extending to large scale VRPs, and heuristic methods . . usually needs manually tuning parameters. exact methods are limited extending to large scale VRPs, and heuristic methods In recent years, with the development of machine learning and deep learning, s many researchers have been successfully applied Learning Based Methods (LBM) to solve VRPs. rning and deep learning, , In this paper,learning t we give a brief review of LBMs for VRPs, including end-to-end approaches and iterative improvement approaches. paper, we give . Then through e experimental results, we analyzed the advantages and disadvantages of the two types of approaches. proaches. . uristic methods Finally, we summarize Abstract t Vehicle Routing Problem (VRP) is a kind of combinatorial optimization problem with extensive application scenarios. . A s present, many methods for solving VRPs have been proposed, which can be divided into exact methods and heuristic methods However, due to the complexity of VRPs, applied Learning Based Methods (LBM) to solve VRPs. pplication scenarios. usually needs manually tuning parameters. . of combinatorial optimization problem with extensive application scenarios. In recent years, with the development of machine lea rning and deep many researchers have been successfully applied Learning Based Methods (LBM) to solve VRPs. . s and heuristic methods In this uristic methods a brief review of LBMs for VRPs, including end-to-end approaches and iterative improvement ap proaches Then through , Vehicle Routing Problem (VRP) is a kind of combinatorial optimization problem with extensive application scenarios. At present, many methods for solving VRPs have been proposed, which can be divided into exact methods and heuristic methods. However, due to the complexity of VRPs, exact methods are limited extending to large scale VRPs, and heuristic method usually needs manually tuning parameters. In recent years, with the development of machine learning and deep learning, many researchers have been successfully d of combinatorial optimization problem with extensive a In this paper, we give t a brief review of LBMs for VRPs, including end-to-end approaches and iterative improvement approaches. uristic methods. Then through experimental results, we analyzed the advantages and disadvantages of the two types of approaches. s, and he Finally, we summarize the characteristics of them, and look forward to the future research directions. opment of machine learning and deep learning Abstrac Vehicle Routing Problem (VRP) is a kind of combinatorial optimization problem with extensive application scenarios A present, many methods for solving VRPs have been proposed, which can be divided into exact methods and heuristic methods However, due to the complexity of VRPs, exact methods are limited extending to large scale VRPs, and heuristic method usually needs manually tuning parameters In recent years, with the development of machine lea many researchers have been successfully applied Learning Based Methods (LBM) to solve VRPs d In this paper, we giv a brief review of LBMs for VRPs, including end-to-end approaches and iterative improvement ap s, and he Then through Abstract Vehicle Routing Problem (VRP) is a kin A present, many methods for solving VRPs have been proposed, which can be divided into exact methods and he However, due to the complexity of VRPs, exact methods are limited extending to large scale VRP usually needs manually tuning parameters. exact methods are limited extending to large scale VRP In recent years, with the devel Abstract Vehicle Routing Problem (VRP) is a kind A present, many methods for solving VRPs have been proposed, which can be divided into exact metho However, due to the complexity of VRPs,

©

This is an open access article under the CC BY-NC-ND license (https://creativecommons.org/licenses/by-nc-nd/4.0) Peer-review under responsibility of the scientific committee of the Tenth International Conference on Information Technology and Quantitative Management © 2023 The Authors. Published by Elsevier B.V. he future research directions. . (LBM) to solve VRPs. Selection and or peer-review under responsibility of ITQM 2023. d-to-end approaches; iterative improvement approaches / Keywords: vehicle routing problem; learning based methods; end-to-end approaches; iterative improvement approaches the characteristics of them, and look forward to t the future research directions © 2023 The Authors. Published by Elsevier B.V. . d-to-end approaches and iterative improvement approaches. Selection and or peer-review under responsibility of ITQM 2023. . / Selection and or peer-review under responsibility of ITQM 2023. aches and iterative improvement approaches. paper, we give Keywords: vehicle routing problem; learning based methods; en the characteristics of them, and look forward to ied Learning Based Methods © 2023 The Authors. Published by Elsevier B.V Selection and or peer-review under responsibility of ITQM 2023 ages of the two types of approaches. / a brief review of LBMs for VRPs, including end-to-end appro Then through experimental results, we analyzed the advantages and disadvantages of the two types of approaches. Finally, we summarize the characteristics of them, and look forward to the future research directions. © 2023 The Authors. Published by Elsevier B.V. he future research directions. many researchers have been successfully appl In this a brief review of LBMs for VRPs, including en Then through experimental results, we analyzed the advantages and disadvant Finally, we summarize the characteristics of them, and look forward to t

© 2023 The Authors. Published by Elsevier B.V. the characteristics of them, and look forward to the future research directions. wo types of approaches. . ning and deep learning, experimental results, we analyzed the advantages and disadvantages of the t two types of approaches r Finally, we summarize 2023 The Authors. Published by Elsevier B.V.d Learning Based Methods (LBM) to solve VRPs. / experimental results, we analyzed the advantages and disadvantages of the Finally, we summarize many researchers have been successfully applie recent years, with the development of machine lea In this paper, we give usually needs manually tuning parameters. In

Keywords: : vehicle routing problem; learning based methods; end-to-end approaches; iterative improvement approaches 1. Introduction Keywords vehicle routing problem; learning based methods; end-to-end approaches; iterative improvement approaches Selection and or peer-review under responsibility of ITQM 2023. / © 2023 The Authors. Published by E sevier B.V.

Selection and or peer-review under responsibility of ITQM 2023.

/

The Vehicle Routing Problem (VRP) is a generic name for a fleet of vehicles to access customers, and involve

1.

Introduction

Keywords:

vehicle routing problem; learning based methods; end-to-end approaches; iterative improvement approaches vehicle routing problem; learning based methods; end-to-end approaches; iterative improvement approaches

The Vehicle Routing Problem (VRP) is a generic name for a fleet of vehicles to access customers, and involve

## 1. . Introduction 1 Introduction Keywords:

to find the optimal route [1]. It is one of the most famous and extensively researched combinatorial optimization to find the optimal route [1]. It is one of the most famous and extensively researched combinatorial optimization problems [2]. timal route [1]. It is one of the most famous and extensively researched combinatorial optimization The basic VRP contains one depot, and many customers with known demands. es must be started These customers are supplied by vehicles, which pick up the cargoes from the depot. ers with known demands. customer is served Delivery paths for vehicles must be started s and finished at the depot. At the end, the demands for every customers should be met and each customer is served by just one vehicle. According to the wide applications in the real world, the VRP derives many variants, such as Traveling Salesman Problem (TSP), Capacitated VRP (CVRP), Vehicle Routing with Time Windows (VRPTW), s etc. veling Salesman Problem (TSP), Capacitated VRP (CVRP), Vehicle Routing with Time Windows (VRPTW), . Figure 1 gives a brief classification framework of VRP variants.le Routing with Time Windows (VRPTW), Since VRPs are always challenging optimization tasks due to the NP-hard characteristic [3], solving them with satisfactory results is still worth exploring. Therefore, researchers have concentrated on designing di ing them with satisfactory results is still worth exploring. . ff erent kinds of e tisfactory results is still worth exploring ff ective approaches to deal with VRPs. - , The traditional optimization methods for VRPs can be roughly classified into two categories: exact methods The Vehicle Routing Problem (VRP) is a generic name for a fleet of vehicles to access customers, and involve to find the op by vehicles, which pick up the cargoes from the depot. ively researched combinatorial optimization problems [2]. at the depot. At the end, the demands for every customers should be met and each omers, and involve The basic VRP contains one depot, and many custom ers with known demands. These customers are supplied by vehicles, which pick up the cargoes from the depot. orld, the VRP derives many variants, such as Delivery paths for vehicles must be started and finished at the depot. At the end, the demands for every customers should be met and each customer is served , by just one vehicle. According to the wide applications in the real world, the VRP derives many variants, such as Tra zation tasks due to the NP-hard characteristic [3], solving them with satisfactory results is still worth exploring etc. erefore, researchers have concentrated on designing di ry customers should be met and each customer is served Figure 1 gives a brief classification framework of VRP variants. of e nce VRPs are always challenging opti Since VRPs are always challenging optimization tasks due to the NP-hard characteristic [3], solv be roughly classified into two categories: ariants, such as Therefore, researchers have concentrated on designing di 14]. nt kinds of e ff erent kinds of e ods, which are mainly based on direct ff ective approaches to deal with VRPs. . problems [2]. le Routing Problem (VRP) is a generic name for a fleet of vehicles to access customers, and involve The basic VRP contains one depot, and many customers with known demands. These customers are supplied ptimal route [1]. It is one of the most famous and extens Delivery paths for vehicl and finished ]. by just one vehicle. According to the wide applications in the real w . Traveling Salesman Problem (TSP), Capacitated VRP (CVRP), Vehicle Routing with Time Windows (VRPTW) d etc. blems [2]. Figure 1 gives a brief classification framework of VRP variants. ld, the VRP derives many variants, such a Since VRPs are always challenging opti d s mi aveling Salesman Problem (TSP), Capacitated VRP (CVRP), Vehic Th c. ff erent kinds . ff ective approaches to deal with VRPs. s The traditional optimization methods for VRPs can ving them with sa exact methods [4, 5, 6, 7, 8, 9] and heuristic methods [10, 11, 12, 13, i Exact meth nce VRPs are always challenging opti ) The Vehic to find the o problems [2 The basic VRP contains one depot, and many custom These customer are supplied by vehicles, which pick up the cargoes from the depot Delivery paths for vehicles must be started and finished at the depot. At the end, the demands for every customers should be met and each customer is serve by just one vehicle. According to the wide applications in the real wor Tr et Figure 1 gives a brief classification framework of VRP variants Si mization tasks due to the NP-hard characteristic [3], sol Therefore, researchers have concentrated on designing d ff ere ff ective approaches to deal with VRPs 1. Introduction The Vehicle Routing Problem (VRP) is a generic name for a fleet of vehicles to access cust to find the optimal route [1]. It is one of the most famous and extensively researched combinatorial optimization problems [2].timal route [1]. It is one of the most famous and extensively researched combinatorial optimization The basic VRP contains one depot, and many customers with known demands. These customers are supplied by vehicles, which pick up the cargoes from the depot. ers with known demands. Delivery paths for vehicles must be starte r and finished at the depot. At the end, the demands for every customers should be met and each customer is served by just one vehicle. According to the wide applications in the real world, the VRP derives many variants, such a Traveling Salesman Problem (TSP), Capacitated VRP (CVRP), Vehicle Routing with Time Windows (VRPTW), etc.veling Salesman Problem (TSP), Capacitated VRP (CVRP), Vehicle Routing with Time Windows (VRPTW Figure 1 gives a brief classification framework of VRP variants. Si 1. Introduction The Vehicle Routing Problem (VRP) is a generic name for a fleet of vehicles to access customers, and involve to find the op pro The basic VRP contains one depot, and many custom These custome are supplied by vehicles, which pick up the cargoes from the depot. Delivery paths for vehicles must be started and finished at the depot. At the end, the demands for eve by just one vehicle. According to the wide applications in the real world, the VRP derives many v Tra

E-mail address:

niulf@ucas.ac.cn.

[4, 5, 6, 7, 8, 9] and heuristic methods [10, 11, 12, 13, 14]. ughly classified into two categories: exact methods Exact methods, which are mainly based on direct s The traditional optimization methods for VRPs can be ro ughly classified into two categories: exact method [4, 5, 6, 7, 8, 9] and heuristic methods [10, 11, 12, 13, 14]. . Exact methods, which are mainly based on direct t ∗ Corresponding author. timization methods for VRPs can be roughly classified into two categories: eal with VRPs. The traditional optimization methods for VRPs can be ro [4, 5, 6, 7, 8, 9] and heuristic methods [10, 11, 12, 13, 14] Exact methods, which are mainly based on direc mization tasks due to the NP-hard characteristic [3], solving them with satisfactory results is still worth exploring. Therefore, researchers have concentrated on designing di ing them with satisfactory results is still worth exploring. ff erent kinds of e ff ective approaches to deal with VRPs. The traditional op s have concentrated on designing di exact methods etc. Figure 1 gives a brief classification framework of VRP variants. Since VRPs are always challenging opti mization tasks due to the NP-hard characteristic [3], solv Therefore, researcher ff erent kinds of e ff ective approaches to d

The traditional optimization methods for VRPs can be r

[4, 5, 6, 7, 8, 9] and heuristic methods [10, 11, 12, 13, 14]. oughly classified into two categories:

Exact methods, which are mainly based on direct s exact method

niulf@ucas.ac.cn.methods [10, 11, 12, 13, 14].

∗

Corresponding author.

Exact methods, which are mainly based on direct

E-mail address:

E-mail address: ∗ Corresponding author. . ∗ Corresponding author [4, 5, 6, 7, 8, 9] and heuristic niulf@ucas.ac.cn. .

E-mail address:

niulf@ucas.ac.cn

This is an open access article under the CC BY-NC-ND license (https://creativecommons.org/licenses/by-nc-nd/4.0) E-mail address: niulf@ucas.ac.cn. ∗ Corresponding author.

Peer-review under responsibility of the scientific committee of the Tenth International Conference on Information

E-mail address:

niulf@ucas.ac.cn.

Technology and Quantitative Management

10.1016/j.procs.2023.08.050

Author name

/

Procedia Computer Science 00 (2023) 000-000

Fig. 1. Di ff erent variants of the VRP.

![Image](image_000002_934fbdaf9228effba5ee550ec7d44d97c86df66c5457467f99fb3e06e114f476.png)

tree search methods [5, 6], dynamic programming [4], and integer linear programming [7, 8, 9], are suitable for solving small-scale VRPs, but hard extending to larger-scale instances for their exponential computational expense. Heuristic methods, which are mainly based on classical heuristics and meta-heuristics, are able to obtain feasible or sub-optimal solutions with probable worst case guarantees in polynomial time, but the optimality cannot be guaranteed and their computational complexity is not satisfactory [15]. Besides, these two kinds of algorithms are highly dependent on specialized knowledge and specific design [16], and thus result in adequate research time.

In the past twenty years, with the rapid development of deep learning technology [17], researchers have got new inspirations and have made many excellent breakthroughs by using deep learning based model to solve VRPs [18, 19, 20, 21, 22]. The learning based methods (LBMs) produce a trained model from VRP instances for training, and then the obtained model can guide the solution generating process. According to the solution generating process, the LBMs can be divided into end-to-end approaches and iterative improvement approaches. In end-to-end approaches, the trained model represents an approximate implicit mapping function between the input and the final solution, and it is used to output solutions directly from the input instance during evaluation. While in iterative improvement approaches, the trained model acts as an assistor of improvement operator, such as selecting node pairs for local search operators [23], destroy-repair operators [24, 25], etc. Unlike end-to-end approaches, the overall optimization process improves a solution iteratively.

Compared with traditional methods, LBMs can build fast approximation or iteration process in a general way without the needing of designing new explicit algorithms. Besides, they can overcome the shortages of the tedious parameter tuning of traditional methods, and quickly solve online instances with the advantage of o ffl ine training [16]. Therefore, we find representative works that related to the LBMs for VRPs in recent years and try to give a brief review and experimental comparison. In order to focus more on the principles of the method itself rather than the di ff erences between VRP variants, we only consider TSP and CVRP in this paper. Besides, the paper discusses the limitations and bottlenecks of LBMs, and lists possible directions of future works.

## 2. Learning based methods for VRPs

In this section, we give a brief review of LBMs for VRPs as much as possible. According to the solution generating process, we describe the related works in chronological order from two parts: end-to-end approaches, and iterative improvement approaches.

## 2.1. End-to-end approaches

End-to-end approaches learn a model to play the role of implicit mapping relationship between the input instances and the final solutions. When the model has been successfully trained, it can directly predict a feasible route given unseen instances. The e ffi ciency for training and inference of end-to-end approaches is generally

Author name

/

Procedia Computer Science 00 (2023) 000-000

better compared to iterative improvement approaches. However, obtaining high quality solutions is challenging. Besides, improving the generalization ability in problem size and data distribution is still a di ffi cult task.

The application of LBM to solve VRPs began with the work of Pointer Network [18] in 2015. The motivation behind Pointer Network is that the existing sequence-to-sequence paradigms require the size of the output dictionary to be fixed, which in fact depends on the length of the input sequence for TSP. Therefore, the authors considered addressing this limitation by reusing attention mechanism [26] to create pointers to input elements. The nodes are gradually selected in an auto-regressive manner until the complete city tour route is obtained. The model is trained under the supervised learning (SL) framework. The experimental results are implemented on TSP instances with less than 50 nodes, and demonstrate that Pointer Network can obtain well solutions when the number of nodes is less than 30, but performs poorly as the size grows larger.

As a pioneering work, the proposal of Pointer Network has discovered a new continent and guided other researchers to continue improving. In fact, SL is not applicable to most VRPs, because there is no guarantee that the obtained training labels are optimal. In others words, obtaining enough optimal solutions for training is time consuming. Therefore, Bello et al. [19] proposed Reinforcement Learning (RL) based [27] Pointer Network in 2017. The results demonstrate that their model exceeds the supervised Pointer Networks [18] on TSP50 and TSP100. However, the Pointer Network model can not apply to solve CVRP yet, because the model assumes that the system is static over time. In CVRP, the demand of customer changes overtime: if it has been served or visited, its demand becomes zero. Therefore, Nazari et al. [20] proposed a simpler alternative Pointer model, which can e ff ectively handle the static and dynamic elements of the system. Specifically, at each step, the embedding of static elements is the input of the RNN decoder, and the output of RNN and dynamic element embedding are fed into attention to generate a feasible destination distribution at the next decision point. The experimental results show that the model reduces 60% inference time compared with OR Tools [28], and achieve better results than the Clarke Wright savings [10] and sweep algorithms [11].

With the development of graph neural network (GNN) [29], some researchers states that Pointer Network can't reflect the graph structure e ff ectively of VRPs [21]. In this way, di ff erent from the sequence-to-sequence paradigm, Khalil et al. [21] used the combination of RL and a GNN named structure2vec to solve TSP. Based on the graph structure, they constructed feasible solutions by continuously adding nodes and maintaining the feasibility that meet the constraints. The results show that their model can achieve similar performance compared with [19]. At the meantime, inspired by the great performance on sequence decision problem achieved by Transformer [30], Deudon et al. [31] first used the multi-head attention and a feed-forward layer as an encoder and pointing mechanism in the decoding process for solving TSP in 2018. The results perform well on the instance with less than 50 nodes.

With the successful applications of GNN and transformer in VRP, it had entered a prosperous era of LBM for VRPsince 2019. Joshi et al. [32] introduced a Graph Convolution Network (GCN) based model for approximating TSP instances under the SL framework. Kool et al. [22] maintained the multi-head attention based encoder, and applied self-attention and a modified context node vector during decoding in 2019. This work makes spectacular contribution for VRPs and is one of the most representative end-to-end approaches. Ma et al. [33] introduce graph pointer networks trained under RL for TSP in 2020. The results show that their model has better generalization ability on large size instances with no more than 1,000 nodes compared with the previously proposed LBMs. Peng et al. [34] proposed a dynamic attention model based on [22] in 2020, which has a dynamic encoder-decoder architecture for CVRP. Their model can e ff ectively explores and utilizes hidden structural information in di ff erent construction steps. Kwon et al. [35] analysed the symmetries in RL based approaches for TSP as an example, and leveraged such symmetries during training via parallel multiple roll-outs. Besides, they used data augmentation to compensate that the number of trajectories can't be arbitrarily large.

In the past two years, more competitive end-to-end approaches have been proposed, which mainly aim to improve:

- · The solution quality on small or medium size instances;
- · The generalization ability on large size instances;
- · The generalization ability on di ff erent distribution instances.

In 2021, Xin et al. [36] modified the model in [22] by using multiple decoders with di ff erent parameters to train policies, and select the best to determine the next node to visit at each step. To force di ff erent decoders

Author name to learn distinct construction patterns and output diverse solutions, they regularized the model with KullbackLeibler divergence between the produced probability distributions from each decoder. Experiments proved the e ff ectiveness of their approach compared with [22]. Xu et al. [37] modified the model in [22] with enhanced node embeddings via batch normalization reordering and gate aggregation. Besides, they additionally add the dynamic information to context node vector. Fu et al. [38] successfully generalized a small pre-trained model to large TSP instances. Specifically, an SL based graph convolutional residual network with attention mechanism is trained on small scale instances, which is able to construct a heat map over the edges given an instances. Then, a graph sampling technique is used to extract sub-graphs from the initial large graph, and the small pre-trained model outputs the corresponding heat map. In the end, all the heat maps are merged together, and the Monte Carlo tree search is used to find the best solution. They evaluated their model on 10,000 nodes TSP instances. The results show that their model outperforms all the previously proposed LBMs, and the inference time is acceptable. The only thing which should be supplemented is that the performance on large scale real world datasets. In 2022, Jiang et al. [39] exploited the group distributionally robust optimization to improve the cross-distribution generalization ability, and designed a convolutional embedding layer to further improve the performance. Although the model indeed performs well on synthetic datasets with di ff erent distributions, there is still potential for improvement on real world dataset.

## 2.2. Iterative improvement approaches

Iterative improvement approaches improve the solutions in an iterative manner. This kind of approaches can usually find a promising solution, but the time costs for training and inference is more expensive than end-to-end approaches for their larger search space.

The first work of iterative improvement approach for VRP was proposed in 2019 by Chen and Tian [40]. Given an initial solution, their approach selects a fragment of the current solution through the learned policies to re-optimize and improve performance iteratively until convergence. The policies are divided into two parts: region-picking and rule-picking, both of which are parameterized by neural networks and optimized simultaneously under RL framework. Inspired by this work, Lu et al. [41] employed a more richer set of improvement operators. Overall, they designed an RL based iterative improvement approach. A threshold-based rule is exploited to determine whether to keep improving the current solution, or to perturb and restart. Compared with other end-to-end methods proposed before, these two works have greatly improved the quality of solution on the CVRP instances with no more than 100 nodes. However, they did not evaluate their approaches on larger size instances. After that, in 2021, Wu et al. [23] proposed an RL based iterative improvement approach named LIH to solve TSP and CVRP, which used a transformer-like architecture to select node pairs and applied 2-opt local search operator to improve the solutions until achieving maximum step. Ma et al [42] proposed an updated version of LIH [23], which learned separate groups of embeddings for the node and positional features, and was equipped with cyclic positional encoding to seize the circularity and symmetry of solutions. The results show that the modifications are e ff ective for improving the performance on TSP and CVRP compared with [23].

In addition, inspired by Large Neighborhood Search (LNS) algorithms [12], some learning based LNS approaches have been proposed as well. The LNS algorithm uses a neighborhood generation mechanism iteratively, which depends on destroy operation by a removal heuristic, and repair operation by an insertion heuristic. These two operators are implemented successively, and the newly generated solution is evaluated via an acceptance function in order to decide whether to accept or reject this solution as the new one. The first learning based LNS approach was proposed by Hottung and Tierney [24] for solving CVRP in 2020. They formulated the repair process as an RL problem, and used a Pointer Network based policy network. Because their approach still used two manually designed destroy operators, which depended the size of neighborhood and directly related to the performance of the model, it is ine ffi ciently for large-scale instances. Gao et al. [43] designed an element-wise graph attention network with edge-embedding, which is named EGATE, as an encoder to not only consider the node information but also the arc information. The decoder is a GRU-based architecture to perform destroy operator, and is trained under RL framework. Then, the destroyed nodes are repaired in a strict order following the leastcost principle. Kim et al. [25] innovatively created a seeding-revising framework in 2021. The seeding process first samples various candidate solutions. Then, each candidate is decomposed into several segments, and they are revised iteratively to find the best one until maximum iterations.

Author name

/

Procedia Computer Science 00 (2023) 000-000

Except for the works mentioned above, Xin et al. [44] came up with the idea in 2021 that using LBM to assist LKH [45], a powerful heuristic solver. They trained a Sparse Graph Network to output edge scores with SL and node penalties with un-SL, which are key parts of LKH. Then their model created the edge candidate set and transformed edge distances to guide the searching process. The results show that their model saves solving time and has good generalization ability on large scale and di ff erent distributed TSP instances. Li et al. [46] contributed to solve large scale CVRP. They adopted the divide and conquer idea: the original problem are divided into multiple sub-problems, then the sub-problem selector chooses one of them and sends it to a sub-solver to obtain high quality solution, finally it is merged with others. In the sub-problem selection process, a Transformer architecture is trained on a generated training set of problem instances under SL, to predict the sub-solution cost after being solved by sub-solvers. Then the sub-problem is selected according to the best cost improvement. The experiments are implemented on CVRP up to 3,000 nodes. The proposed approach accelerates the LKH3 [47] by an order of magnitude while obtaining competitive solution qualities.

## 3. Experimental Study

In this section, we give the experimental comparisons of some approaches mentioned above from three aspects, namely tour length, optimality gap, and time costs, on uniformly distributed synthetic TSP and CVRP instances.

## 3.1. Experimental approaches

Since the works before 2019 has been compared in many other reviews, we will not consider them here. The experimental approaches we will compare in this paper are listed as follows, and all hyper-parameter settings of these approaches are consistent with those in the original articles.:

## · End-to-end approaches:

- -AM[22]: An RL graph attention network model, which has made important contributions for solving VRP.
- -POMO [35]: A modified AM model which leverages symmetries characteristics during training via parallel multiple roll-outs.
- -MRAM [37]: A multiple relational AM model, which enhances node embeddings and improves the context node vector.
- -MDAM [36]: A multi-decoder AM model, which forces to learn distinct construction patterns and output diverse solutions.

## · Iterative improvement approaches:

- -NeuRewriter [40]: The first RL based iterative improvement approach where the policy is divided into a region-picking and a rule-picking component. The number of iterations is 200.
- -NLNS [24]: A neural based LNS approach which uses two manually designed destroy operators and an RL based Pointer Network repair operators.
- -LIH [23]: An improvement heuristics which uses a transformer-like architecture to select node pairs and applies 2-opt local search operator to improve the solutions. The number of iterations is 5,000.
- -LCP [25]: A seeding-revising framework which samples various candidate solutions first, then decomposes and revises iteratively. In the seeding process, the sample width is 1,280. While in the revision process, the revision length is 10, and the number of iterations of the reviser is 10 for solving TSP, and 1 for solving CVRP.

## 3.2. Experimental datasets and evaluation metrics

We consider Euclidean TSP and CVRP instances with 20, 50, and 100 nodes, respectively. The location of TSP nodes, CVRP customers and depot are uniformly generated in the unit square [0 1] , × [0 , 1]. For CVRP, the capacities of a vehicle are 30, 40, 50 for CVRP20, CVRP50 and CVRP100, respectively. The demand of each customer is chosen uniformly at random in { 1 , ..., 9 , and the demand of depot is set as 0. }

The evaluation metrics contain Tour Length (Tour Len.), Optimality Gap (Opt. Gap) and Testing Time (Time). The unavailable items are marked as '-'. The optimality gap is the ratio of the di ff erence between the tour length

Author name and the optimal solution to the optimal solution. To compute it, we use the exact solver Concorde [48] to get the optimal tour length for TSP and LKH3 [47] for CVRP.

## 3.3. Comparison study

Table 1. Comparison of tour length and solving time of di ff erent methods.

| Method            | TSP20     | TSP20     | TSP20   |                |                 |                | TSP100         | TSP100         | TSP100         |
|-------------------|-----------|-----------|---------|----------------|-----------------|----------------|----------------|----------------|----------------|
|                   | Tour Len. | Opt. Gap. | Time    | Tour Len.      | TSP50 Opt. Gap. | Time           | Tour Len.      | Opt. Gap.      | Time           |
| Concorde          | 3.83      | 0.00 %    | 5m      | 5.69           | 0.00 %          | 13m            | 7.76           | 0.00 %         | 1h             |
| AM(greedy)        | 3.85      | 0.34%     | 1s      | 5.79           | 1.68%           | 2s             | 8.12           | 4.53%          | 5s             |
| AM(sampling)      | 3.84      | 0.08%     | 3m      | 5.73           | 0.51%           | 10m            | 7.94           | 2.19%          | 23m            |
| POMO (no augment) | 3.83      | 0.04%     | 1s      | 5.70           | 0.21%           | 3s             | 7.80           | 0.46%          | 11s            |
| POMO (augment)    | 3.83      | 0.00 %    | 1s      | 5.69           | 0.03 %          | 16s            | 7.77           | 0.14 %         | 1m             |
| MRAM(greedy)      | 3.84      | 0.27%     | 2s      | 5.78           | 1.55%           | 3s             | 8.08           | 4.13%          | 6s             |
| MRAM(sampling)    | 3.84      | 0.06%     | 5m      | 5.72           | 0.46%           | 14m            | 7.92           | 1.93%          | 27m            |
| MDAM(greedy)      | 3.84      | 0.05%     | 5s      | 5.73           | 0.62%           | 15s            | 7.93           | 2.19%          | 36s            |
| MDAM(beam search) | 3.83      | 0.00 %    | 2m      | 5.70           | 0.04%           | 7m             | 7.80           | 0.48%          | 20m            |
| NeuRewriter       | -         | -         | -       | -              | -               | -              | -              | -              | -              |
| NLNS              | -         | -         | -       | -              | -               | -              | -              | -              | -              |
| LIH               | 3.83      | 0.00 %    | 1h      | 5.70           | 0.20%           | 1.5h           | 7.87           | 1.42%          | 2h             |
| LCP               | 3.83      | 0.00 %    | 42m     | 5.70           | 0.10 %          | 1.25h          | 7.85           | 1.13 %         | 2.5h           |
| Method            | CVRP20    | CVRP20    | CVRP20  | CVRP50 CVRP100 | CVRP50 CVRP100  | CVRP50 CVRP100 | CVRP50 CVRP100 | CVRP50 CVRP100 | CVRP50 CVRP100 |
|                   | Tour Len. | Opt. Gap. | Time    | Tour Len.      | Opt. Gap.       | Time           | Tour Len.      | Opt. Gap.      | Time           |
| LKH3              | 6.11      | 0.00%     | 1h      | 10.38          | 0.00 %          | 5h             | 15.68          | 0.00 %         | 9h             |
| AM(greedy)        | 6.40      | 4.75%     | 1s      | 10.98          | 5.78%           | 2s             | 16.76          | 6.89%          | 6s             |
| AM(sampling)      | 6.27      | 2.89%     | 5m      | 10.62          | 2.38%           | 20m            | 16.24          | 3.89%          | 51m            |
| POMO (no augment) | 6.17      | 0.82%     | 1s      | 10.49          | 1.14%           | 4s             | 15.83          | 0.98%          | 19s            |
| POMO (augment)    | 6.14      | 0.21 %    | 5s      | 10.42          | 0.45 %          | 26s            | 15.73          | 0.32 %         | 2m             |
| MRAM(greedy)      | 6.39      | 4.77%     | 2s      | 10.95          | 5.49%           | 3s             | 16.69          | 6.43%          | 7s             |
| MRAM(sampling)    | 6.25      | 2.43%     | 7m      | 10.61          | 2.34%           | 22m            | 16.20          | 3.32%          | 53m            |
| MDAM(greedy)      | 6.24      | 1.79%     | 7s      | 10.74          | 3.47%           | 16s            | 16.40          | 4.86%          | 45s            |
| MDAM(beam search) | 6.14      | 0.26%     | 3m      | 10.50          | 1.18%           | 9m             | 16.03          | 2.49%          | 31m            |
| NeuRewriter       | 6.16      | 0.74%     | 18m     | 10.51          | 1.25%           | 22m            | 16.10          | 2.71%          | 1h             |
| NLNS              | 6.19      | 1.31%     | 7m      | 10.54          | 1.55%           | 24m            | 15.99          | 1.98 %         | 1h             |
| LIH               | 6.12      | 0.39 %    | 2h      | 10.45          | 0.70 %          | 4h             | 16.03          | 2.47%          | 5h             |
| LCP               | 6.16      | 0.92%     | 15m     | 10.54          | 1.54%           | 34m            | 16.03          | 2.43%          | 1.25h          |

1 The gap is computed based on the best solutions given by Concorde and LKH3.

2 Bold represents the best among each group.

The comparison results are displayed in Table 1. From Table 1, we first observe that considering the testing time, the end-to-end approaches can obtain solutions more quickly than exact solvers, namely Concorde on TSP and LKH3 on CVRP, and iterative improvement approaches. While considering the solution quality, since TSP is relatively simple problem compared to CVRP, end-to-end approaches can already achieve results that are very close to the optimal solution, thus the results of iterative improvement approaches are not significantly better compared to end-to-end approaches. On CVRP, the advantages of neighborhood search or local search which are used by the iterative improvement approaches are more evident.

Next, we conduct more detailed observations of end-to-end approaches and iterative improvement approaches. Benefiting from learning a construction model by sampling diverse trajectories, POMO [35] produces excellent solutions on both TSP and CVRP instances, and is recognized as the state-of-the-art end-to-end approach. However, as pointed in [42], POMO still has potential to enhance the generalization ability, so do other end-to-end approaches. While for the the listed iterative improvement approaches, their performances are all highly competi-

Author name tive. Besides, due to the heuristic idea fitting with the problem, their generalization ability is better than end-to-end approaches [23, 25, 42]. However, the inference time is required more compared to end-to-end approaches.

## 4. Conclusion and discussion

In this paper, we give a brief survey of LBMs for VRP, including end-to-end approaches and iterative improvement approaches. The experimental comparisons prove that the time e ffi ciency of end-to-end approaches is generally better compared to iterative improvement approaches. However, the generalization ability of end-to-end approaches remains a weakness.

At present, the works of designing end-to-end models still dominate the majority of LBMs for VRP, because the original intention of adopting LBMs is to build fast approximation in a general way without the need for designing new explicit algorithms. Therefore, viewing of the current shortcomings, some possible future research directions are as follows:

- 1. Enhancing the generalization ability on large scale instances: Fu et al. [38] successfully generalized a small pre-trained model to large TSP instances. Li et al. [46] contributed to solve large scale CVRP by adopting the divide and conquer idea. In this way, the idea of segmenting a large scale instances into small scale instances and then improving them separately is feasible.
- 2. Enhancing the cross-distribution generalization ability: There is relatively little research in this direction for VRP, and currently Jiang et al. [39] exploited the group distributionally robust optimization to improve the cross-distribution generalization ability. But there are extensive works focusing on distributionally robust neural networks [49] and out-of-distribution generalization [50]. Hence it is worth studying how to correctly combine these technologies with end-to-end models for VRPs.

## Acknowledgements

This work was supported by National Natural Science Foundation of China (11991021, 11991020, 12271503).

## References

- [1] P. Toth, D. Vigo, The vehicle routing problem, SIAM, 2002.
- [2] G. Laporte, Fifty years of vehicle routing, Transportation Science 43 (4) (2009) 408-416.
- [3] J. K. Lenstra, A. R. Kan, Complexity of vehicle routing and scheduling problems, Networks 11 (2) (1981) 221-227.
- [4] N. Christofides, A. Mingozzi, P. Toth, State-space relaxation procedures for the computation of bounds to routing problems, Networks 11 (2) (1981) 145-164.
- [5] G. Laporte, Y. Nobert, A branch and bound algorithm for the capacitated vehicle routing problem, Operations-Research-Spektrum 5 (2) (1983) 77-85.
- [6] G. Laporte, H. Mercure, Y. Nobert, An exact algorithm for the asymmetrical capacitated vehicle routing problem, Networks 16 (1) (1986) 33-46.
- [7] M. Desrochers, J. Desrosiers, M. Solomon, A new optimization algorithm for the vehicle routing problem with time windows, Operations research 40 (2) (1992) 342-354.
- [8] R. Baldacci, N. Christofides, A. Mingozzi, An exact algorithm for the vehicle routing problem based on the set partitioning formulation with additional cuts, Mathematical Programming 115 (2) (2008) 351-385.
- [9] N. Azi, M. Gendreau, J.-Y. Potvin, An exact algorithm for a vehicle routing problem with time windows and multiple use of vehicles, European Journal of Operational Research 202 (3) (2010) 756-763.
- [10] G. Clarke, J. W. Wright, Scheduling of vehicles from a central depot to a number of delivery points, Operations research 12 (4) (1964) 568-581.
- [11] B. E. Gillett, L. R. Miller, A heuristic algorithm for the vehicle-dispatch problem, Operations research 22 (2) (1974) 340-349.
- [12] P. Shaw, A new local search algorithm providing high quality solutions to vehicle routing problems, APES Group, Dept of Computer Science, University of Strathclyde, Glasgow, Scotland, UK 46.
- [13] M. Gendreau, A. Hertz, G. Laporte, A tabu search heuristic for the vehicle routing problem, Management science 40 (10) (1994) 12761290.
- [14] B. M. Baker, M. Ayechew, A genetic algorithm for the vehicle routing problem, Computers &amp; Operations Research 30 (5) (2003) 787-800.
- [15] R. Goel, R. Maini, Vehicle routing problem and its solution methodologies: a survey, International Journal of Logistics Systems and Management 28 (4) (2017) 419-435.
- [16] B. Li, G. Wu, Y. He, M. Fan, W. Pedrycz, An overview and experimental study of learning-based optimization algorithms for the vehicle routing problem, IEEE CAA Journal of Automatica Sinica 9 (7) (2022) 1115-1138. /

Author name

- [17] Y. LeCun, Y. Bengio, G. Hinton, Deep learning, nature 521 (7553) (2015) 436-444.
- [18] O. Vinyals, M. Fortunato, N. Jaitly, Pointer networks, in: Proceedings of Neural Information Processing Systems, 2015, pp. 2692-2700.
- [19] I. Bello, H. Pham, Q. V. Le, M. Norouzi, S. Bengio, Neural combinatorial optimization with reinforcement learning, in: International Conference on Learning Representations, 2017.
- [20] M. Nazari, A. Oroojlooy, L. Snyder, M. Tak´ ac, Reinforcement learning for solving the vehicle routing problem, in: Proceedings of Neural Information Processing Systems, 2018, pp. 9861-9871.
- [21] E. Khalil, H. Dai, Y. Zhang, B. Dilkina, L. Song, Learning combinatorial optimization algorithms over graphs, in: Proceedings of Neural Information Processing Systems, 2017, pp. 6348-6358.
- [22] W. Kool, H. Van Hoof, M. Welling, Attention, learn to solve routing problems!, in: International Conference on Learning Representations, 2019.
- [23] Y. Wu, W. Song, Z. Cao, J. Zhang, A. Lim, Learning improvement heuristics for solving routing problems, IEEE Transactions on Neural Networks and Learning Systems (2021) 1-13.
- [24] A. Hottung, K. Tierney, Neural large neighborhood search for the capacitated vehicle routing problem, in: ECAI 2020, 2020, pp. 443450.
- [25] M. Kim, J. Park, j. kim, Learning collaborative policies to solve np-hard routing problems, Advances in Neural Information Processing Systems 34 (2021) 10418-10430.
- [26] D. Bahdanau, K. H. Cho, Y. Bengio, Neural machine translation by jointly learning to align and translate, in: International Conference on Learning Representations, 2015.
- [27] R. J. Williams, Simple statistical gradient-following algorithms for connectionist reinforcement learning, Machine learning 8 (3) (1992) 229-256.
- [28] O. t. Google Inc., Or-tools: Google optimization tools, https: // developers.google.com optimization . / /
- [29] Z. Wu, S. Pan, F. Chen, G. Long, C. Zhang, S. Y. Philip, A comprehensive survey on graph neural networks, IEEE transactions on neural networks and learning systems 32 (1) (2020) 4-24.
- [30] A. Vaswani, N. Shazeer, N. Parmar, J. Uszkoreit, L. Jones, A. N. Gomez, Ł. Kaiser, I. Polosukhin, Attention is all you need, in: Proceedings of Neural Information Processing Systems, 2017, pp. 6000-6010.
- [31] M. Deudon, P. Cournut, A. Lacoste, Y. Adulyasak, L.-M. Rousseau, Learning heuristics for the tsp by policy gradient, in: Integration of Constraint Programming, Artificial Intelligence, and Operations Research, 2018, pp. 170-181.
- [32] C. K. Joshi, T. Laurent, X. Bresson, An e ffi cient graph convolutional network technique for the travelling salesman problem, arXiv preprint arXiv:1906.01227.
- Q. Ma, S. Ge, D. He, D. Thaker, I. Drori, Combinatorial optimization by graph pointer networks and hierarchical reinforcement learning,
- [33] in: AAAI Workshop on Deep Learning on Graphs: Methodologies and Applications, 2020.
- [34] B. Peng, J. Wang, Z. Zhang, A deep reinforcement learning algorithm using dynamic attention model for vehicle routing problems, in: Artificial Intelligence Algorithms and Applications, 2020, pp. 636-650.
- [35] Y. Kwon, J. Choo, B. Kim, I. Yoon, Y. Gwon, S. Min, Pomo: Policy optimization with multiple optima for reinforcement learning, Advances in Neural Information Processing Systems 33 (2020) 21188-21198.

[36]

L. Xin, W. Song, Z. Cao, J. Zhang, Multi-decoder attention model with embedding glimpse for solving vehicle routing problems, in:

Proceedings of the AAAI Conference on Artificial Intelligence, Vol. 35, 2021, pp. 12042-12049.

- [37] Y. Xu, M. Fang, L. Chen, G. Xu, Y. Du, C. Zhang, Reinforcement learning with multiple relational attention for solving vehicle routing problems, IEEE Transactions on Cybernetics 52 (10) (2021) 11107-11120.
- [38] Z. Fu, K. Qiu, H. Zha, Generalize a small pre-trained model to arbitrarily large tsp instances, in: Proceedings of the AAAI Conference on Artificial Intelligence, V ol. 35, 2021, pp. 7474-7482.
- [39] Y. Jiang, Y. Wu, Z. Cao, J. Zhang, Learning to solve routing problems via distributionally robust optimization, in: Proceedings of the AAAI Conference on Artificial Intelligence, Vol. 36, 2022, pp. 9786-9794.

[40]

X. Chen, Y. Tian, Learning to perform local rewriting for combinatorial optimization, in: Proceedings of Neural Information Processing

Systems, 2019, p. 6278-6289.

- [41] H. Lu, X. Zhang, S. Yang, A learning-based iterative method for solving vehicle routing problems, in: International Conference on Learning Representations, 2020.
- [42] Y. Ma, J. Li, Z. Cao, W. Song, L. Zhang, Z. Chen, J. Tang, Learning to iteratively solve routing problems with dual-aspect collaborative transformer, Advances in Neural Information Processing Systems 34 (2021) 11096-11107.
- [43] L. Gao, M. Chen, Q. Chen, G. Luo, N. Zhu, Z. Liu, Learn to design the heuristics for vehicle routing problem, arXiv preprint arXiv:2002.08539.
- [44] L. Xin, W. Song, Z. Cao, J. Zhang, Neurolkh: Combining deep learning model with lin-kernighan-helsgaun heuristic for solving the traveling salesman problem, Advances in Neural Information Processing Systems 34 (2021) 7472-7483.

[45]

K. Helsgaun, An e ff

ective implementation of the lin-kernighan traveling salesman heuristic, European journal of operational research

126 (1) (2000) 106-130.

- [46] S. Li, Z. Yan, C. Wu, Learning to delegate for large-scale vehicle routing, Advances in Neural Information Processing Systems 34 (2021) 26198-26211.
- [47] K. Helsgaun, An extension of the lin-kernighan-helsgaun tsp solver for constrained traveling salesman and vehicle routing problems, Roskilde: Roskilde University 12.
- [48] D. L. Applegate, R. E. Bixby, et al., Concorde tsp solver, http: // www.math.uwaterloo.ca tsp concorde. / /
- [49] S. Sagawa*, P. W. Koh*, T. B. Hashimoto, P. Liang, Distributionally robust neural networks, in: International Conference on Learning Representations, 2020.
- [50] H. Ye, C. Xie, T. Cai, R. Li, Z. Li, L. Wang, Towards a theoretical framework of out-of-distribution generalization, Advances in Neural Information Processing Systems 34 (2021) 23519-23531.