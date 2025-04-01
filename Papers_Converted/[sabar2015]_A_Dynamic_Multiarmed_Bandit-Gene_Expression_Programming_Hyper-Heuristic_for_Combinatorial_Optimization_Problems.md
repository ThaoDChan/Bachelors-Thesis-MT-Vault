## A Dynamic Multiarmed Bandit-Gene Expression Programming Hyper-Heuristic for Combinatorial Optimization Problems

![Image](image_000000_3466573eb12da3296a0e4851c3d957f3dbf6e5cadd539d7f73013f8905ed8742.png)

## A Dynamic Multiarmed Bandit-Gene Expression Programming Hyper-Heuristic for Combinatorial Optimization Problems

Nasser R. Sabar, Masri Ayob, Graham Kendall, Senior Member, IEEE , and Rong Qu, Senior Member, IEEE

Abstract -Hyper-heuristics are search methodologies that aim to provide high-quality solutions across a wide variety of problem domains, rather than developing tailor-made methodologies for each problem instance/domain. A traditional hyper-heuristic framework has two levels, namely, the high level strategy (heuristic selection mechanism and the acceptance criterion) and low level heuristics (a set of problem specific heuristics). Due to the different landscape structures of different problem instances, the high level strategy plays an important role in the design of a hyper-heuristic framework. In this paper, we propose a new high level strategy for a hyper-heuristic framework. The proposed high-level strategy utilizes a dynamic multiarmed bandit-extreme value-based reward as an online heuristic selection mechanism to select the appropriate heuristic to be applied at each iteration. In addition, we propose a gene expression programming framework to automatically generate the acceptance criterion for each problem instance, instead of using human-designed criteria. Two well-known, and very different, combinatorial optimization problems, one static (exam timetabling) and one dynamic (dynamic vehicle routing) are used to demonstrate the generality of the proposed framework. Compared with state-of-the-art hyper-heuristics and other bespoke methods, empirical results demonstrate that the proposed framework is able to generalize well across both domains. We obtain competitive, if not better results, when compared to the best known results obtained from other methods that have been presented in the scientific literature. We also compare our approach against the recently released hyper-heuristic competition test suite. We again demonstrate the generality of our approach when we compare against other methods that have utilized the same six benchmark datasets from this test suite.

Manuscript received January 31, 2013; revised July 30, 2013, March 8, 2014, and April 22, 2014; accepted May 11, 2014. Date of publication June 2, 2014; date of current version January 13, 2015. This paper was recommended by Associate Editor Q. Zhang.

This paper has supplementary downloadable multimedia material available at http://ieeexplore ieee org provided by the authors. This includes a PDF file . . with additional tables. This material is 0.060 MB in size.

- N. R. Sabar is with Data Mining and Optimization Research Group, Centre for Artificial Intelligent, Universiti Kebangsaan Malaysia, Bangi 43600, Malaysia, and also with University of Nottingham Malaysia Campus, Semenyih 43500, Malaysia (e-mail: nasser.sabar@nottingham.edu.my).
- M. Ayob is with Data Mining and Optimization Research Group, Centre for Artificial Intelligent, Universiti Kebangsaan Malaysia, Bangi 43600, Malaysia (e-mail: masri@ftsm.ukm.my).
- G. Kendall is with ASAP Research Group, School of Computer Science, University of Nottingham, Nottingham NG8 1BB, U.K., and also with University of Nottingham Malaysia Campus, Semenyih 43500, Malaysia (e-mail: graham.kendall@nottingham.ac.uk).
- R. Qu is with ASAP Research Group, School of Computer Science, University of Nottingham, Nottingham NG8 1BB, U.K. (e-mail: rong.qu@nottingham.ac.uk).

Color versions of one or more of the figures in this paper are available online at http://ieeexplore ieee org. . .

Digital Object Identifier 10.1109/TCYB.2014.2323936

Index Terms -CHeSC, dynamic optimization, gene expression programming, hyper-heuristic, timetabling, vehicle routing.

## I. INTRODUCTION

M ETA-HEURISTIC research communities have acknowledged the fact that meta-heuristic configurations (operators and parameter settings) play a crucial role in algorithmic performance [1]-[3]. Indeed, it has been shown that different meta-heuristic configurations work well for particular problem instances or only at particular stages of the solving process [4], [5]. Within this context, automated heuristic design methods have emerged as a new research trend [4]. The ultimate goal of these methods is to automate the algorithm design process as far as possible, to enable them to work effectively across a diverse set of problem domains [1], [6]. Hyper-heuristics [4] represent one of these methodologies. They are a search methodology that is able to provide solutions across a wide variety of problem domains, rather than being tailored for each problem or even each problem instance. Hyper-heuristics operate on the heuristic search space, rather than operating directly on the solution space, which is usually the case with meta-heuristic algorithms [7]. The key motivation behind hyper-heuristics is to raise the level of generality, by drawing on the strengths, and recognizing the weaknesses, of different heuristics. The most common hyper-heuristic framework has two levels; a high level strategy and a set of low level heuristics (LLHs). The high level strategy manages which LLH to call (heuristic selection mechanism) and then decides whether to accept the returned solution (the acceptance criterion). The LLHs contain a set of problem specific heuristics which are different for each problem domain.

The success of a hyper-heuristic framework is usually due to the design of the high level strategy and it is not surprising that much research has been focused in this area [8]. The variety of landscape structures and the difficulty of the problem domains, or even problem instances, usually require an efficient heuristic selection mechanism and acceptance criteria to achieve good performance [4]. Both components are crucial and many works have shown that different combinations and configurations usually yield different performance [7]-[9].

Therefore, in this paper, we address these challenges by proposing a new high level strategy for the hyper-heuristic framework with the following two components (see Fig. 1).

Fig. 1. Proposed gene expression programming-based hyper-heuristic (GEP-HH) framework.

![Image](image_000001_8db19fca42a1a05c3d5c21032e8bef82deda1c0870987b23c8c9d594eedd1c81.png)

## A. Heuristic Selection Mechanism

The proposed framework utilizes the dynamic multiarmed bandit (DMAB)-extreme value-based reward [10] as an online heuristic selection mechanism. The attractive feature of a DMAB is the integration of the Page-Hinkly (PH) statistical test to determine the most appropriate heuristic for the next iteration by detecting the changes in average reward for the current best heuristics. In addition, the extreme value-based reward credit assignment mechanism records the historical information of each heuristic to be used during the heuristic selection process.

## B. Acceptance Criterion

Instead of using an acceptance criterion manually designed by human experts (which usually requires ongoing tuning), we propose an automatic framework to automatically generate, during the solving process, different acceptance criteria for different instances or problem domains as well as to consider the current problem state by using gene expression programming (GEP) [11]. We choose GEP to automatically generate the acceptance criteria due to its ability to always generate solutions that are syntactically correct and to avoid the problem of code bloat.

In the scientific literature, automatic program generation methods, such as genetic programming (GP), have been successfully utilized as hyper-heuristics to generate heuristics for optimization problems such as 2-D packing and MAXSAT problems [7]. However, despite the success of GP-based hyper-heuristics, the same hyper-heuristic cannot be used to generate heuristics for other domains such as exam timetabling or vehicle routing problems. This is because existing GPbased hyper-heuristics generate heuristics for only one domain. Hence, the function and terminal sets that have been defined for one domain cannot usually be used for other domains. In other words, to solve a new problem domain we have to define a different set of functions and terminals that are suitable to the problem at hand. In this paper, we propose an automatic program generation framework to automatically generate the high level strategy competent of the hyper-heuristic framework. The novelty of the proposed framework is that it is being used at the higher level of abstraction and can tackle many optimization problems using the same set of functions and terminals. This feature distinguishes our framework from existing GP-based hyper-heuristics. In practice, evolving or optimizing algorithm components will not only alleviate user intervention in finding the most effective configuration, but can also facilitate algorithm configurations. In addition, manual configurations usually only represent a small fraction of the available search space and they often require a considerable amount of expertise and experience. Hence, exploring the search space using a suitable search methodology (i.e., GEP in this paper) might yield a better performance compared to a manually configured search methodology.

Our objectives are as follows.

- 1) To propose an online hyper-heuristic framework that can adapt itself to the current problem state using a heuristic selection mechanism which integrates a statistical test to select the most appropriate LLH.
- 2) To propose an online framework to automatically generate, for each instance, an acceptance criterion that uses the current problem state, and that is able to cope with changes that might occur during the search process. This will be achieved using a GEP algorithm.
- 3) To test the generality and consistency of the proposed hyper-heuristic framework on two different problem domains (both static and dynamic) and compare its performance against the state-of-the-art of hyper-heuristics and the best known bespoke methods in the scientific literature.

Two well-known combinatorial optimization problems, but with very different search space characteristics, are used as our benchmarks. The problems are: the exam timetabling problem (ITC 2007 instances [12]) and the dynamic vehicle routing problem (DVRP) (Kilby instances [13]). We also further test the generality of the approach by comparing against the recently introduced hyper-heuristic test suite of problems [hyper-heuristic competition (CHeSC 2011)] [14]. To the best of our knowledge, this is the first work in the hyperheuristic literature which has considered both dynamic and static problems.

## II. HYPER-HEURISTICS AND RELATED WORKS

Burke et al . [4] defined a hyper-heuristic as 'an automated methodology for selecting or generating heuristics to solve hard computational search problems' . Hyper-heuristics have been widely used, with much success, to solve various classes of problems. A traditional hyper-heuristic framework has two levels, a high and a low level.

The high level strategy, which are problem-independent and have no knowledge of the domain, control the selection or generation of heuristics (without knowing what specific function it performs) at each decision point in the search process. In contrast to meta-heuristics, hyper-heuristics search the space of heuristics instead of directly searching the solution space. The LLHs represent a set of problem-dependent heuristics which operate directly on the solution space.

Generally, the high level abstraction of the hyper-heuristic framework means that it can be applied to multiple problem domains with little (or no) additional development effort. Recently, Burke et al . [4] classified hyper-heuristic frameworks

based on the nature of the heuristic search space and the source of feedback during learning. The source of feedback can either be online, if the hyper-heuristic framework uses feedback obtained during the problem-solving procedure, or offline, if the hyper-heuristic framework uses information gathered during a training phase in order to be used when solving future unseen instances. The nature of the heuristic search space is also classified into two subclasses known as heuristics to choose heuristics and heuristics to generate heuristics. The classification specifies whether heuristics (either chosen or generated) are constructive or perturbative. Our proposed hyper-heuristic framework can be classified as an online perturbative heuristic to choose heuristics.

## A. Heuristic to Choose Heuristics

Most hyper-heuristic frameworks that have been published to date have been heuristics to choose heuristics [4]. For a given problem instance, the role of the hyper-heuristic framework is to intelligently choose a LLH from the provided set, and apply it. The idea is to combine the strengths of several heuristics into one framework. Meta-heuristics and machine-learning methodologies have been used as heuristic selection mechanisms [15], for example tabu search (TS) with reinforcement learning [16], and scatter search [17]. Many different acceptance criteria have also been used, including all moves [18], only improving [18], improving and equal [18], Monte Carlo [19], record-to-record travel [20], simulated annealing [21], [22], late acceptance [23], great deluge [24], and TS [25]. More details are available in [7].

Recently, the cross-domain heuristic search (CHeSC) competition was introduced, which provides a common software interface for investigating different (high level) hyper-heuristics and provides access to six problem domains where the LLHs are provided as part of the supplied framework [14]. The algorithm designer only needs to provide the high level components (heuristic selection and acceptance criterion). Further details about the competition, including results, are available in [14].

## B. Heuristics to Generate Heuristics

Heuristics that generate heuristics focus on designing new heuristics by combining existing heuristic components and then applying them to the current solution. Generative GP hyper-heuristics have been utilized to solve many combinatorial optimization problems including SAT [26], [27], timetabling [27], [28], vehicle routing [28], job shop scheduling [29], and bin packing [30]. A recent review on hyperheuristic is available in [7] which provides more details about this area.

Although promising results have been achieved, GP has been utilized as an offline heuristic/rule builder using a specific set of functions and terminals. Besides being computationally expensive due to the need for training and testing, they do not guarantee to deliver the same performance across different domains or even different instances of the same domain. This is because the generated heuristics/rules are suited to only the instance(s) that have been used in the training phase.

View publication stats

Furthermore, they are tailored to solve specific problems and were only applied to a single (static) domain, which raises the question: to what extent will they generalize to other domains?

The success of the above work, which has some resemblance to the proposed GEP, is the main motivation for proposing an online GEP framework to generate the acceptance criteria for the hyper-heuristic framework. The benefit of this framework is the ability to generate different acceptance criteria for different instances based on the problem state and thus enabling it to cope with changes that might occur during the solving process.

## III. PROPOSED FRAMEWORK

We start by describing the proposed perturbative-based hyper-heuristic framework, followed by the components of the high level strategy, i.e., the heuristic selection and acceptance criterion mechanisms. Finally, we describe the LLHs that will be used in our framework.

## A. Perturbative-Based Hyper-Heuristic Framework

Our online perturbative-based hyper-heuristic framework comprises a high level strategy and a set of LLHs. The high level strategy consists of two components, heuristic selection and acceptance criterion. The goal of the high level strategy is to select a LLH to be applied at a given decision point. The low level contains a set of perturbative heuristics that are applied to the problem instance, when called by the high level strategy. Therefore, based on the utilized LLHs, the proposed hyper-heuristic framework is an improvement-based method as the hyper-heuristic starts with an initial solution and iteratively improves it using a set of perturbative LLHs.

The proposed hyper-heuristic iteratively explores the neighborhood of the incumbent solution, seeking for improvement. Given a set of LLHs, a complete initial solution, S , (generated either randomly or via a constructive heuristic) and the objective function, F , the aim of the hyper-heuristic framework is to select an appropriate LLH and apply it to the current solution ( S' = LLH S ( )) . Next, the objective function is called to evaluate the quality of the resultant solution ( F S' ( )) , followed by the acceptance criterion which decides whether to accept or reject S' . If S' is accepted, it will replace S . Otherwise, S' will be rejected. The hyper-heuristic will update the algorithmic parameters and start another iteration. This process is repeated for a given number of iterations. Next, we discuss the components of the proposed high level strategy.

## B. High-Level Strategy

We propose a new high level strategy, as shown in Fig. 2. It has two components: a heuristic selection mechanism (DMAB-extreme value-based reward) and an acceptance criterion (GEP).

1) Heuristic Selection Mechanism: The heuristic selection mechanism successively invokes two tasks, credit assignment (extreme value-based reward [10]) and heuristic selector (DMAB mechanism [10]). The credit assignment mechanism maintains a value (reward) for each LLH and these values are used by the heuristic selector mechanism to decide which heuristic will be executed at each decision point.