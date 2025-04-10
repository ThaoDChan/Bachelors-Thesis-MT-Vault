## Intro
You are a research assistant helping me summarize academic papers for my thesis. 

In the next section of this prompt you will recieve the following inputs:
A text block labled PAPER containing a scientific paper including references which you will summarize
A text block labled CONTEXT that outlines the scope of my thesis
A text Block labled BIB containing the literature I intend on using in my thesis

 I have converted the scientific paper from PDF to a Markdown (.md) file. This conversion may contain layout errors, OCR mistakes (including typos, misplaced symbols, or missing formulas), and no embedded images. If any part of the text is unclear or requires an image to fully understand the method or results, please let me know under “Debug & Action Items” at the end.

## Input

---
### PAPER

```

```

---
### TAGS

#ML-Supervised
#ML-SupervisedFromOptimal
#ML-Unsupervised
#ML-Clustering
#ML-PatternDiscovery
#ML-ReinforcementLearning
#ML-Hybrid
#ML-AssistedExact
#ML-AssistedBranchAndBound
#ML-AssistedHeuristics
#ML-AssistedLocalSearch
#ML-DestroyRepair

#RL-ValueBased
#RL-Qlearning
#RL-DQN
#RL-PolicyGradient
#RL-ActorCritic
#ProximalPolicyOptimization
#ImitationLearning
#PolicySearch
#PointerNetworks
#AttentionMechanismAM
#Seq2Seq
#GraphNeuralNetworks
#TransformerModels
#NeuralCombinatorialOptimization
#Structure2Vec
#MonteCarloTreeSearch
#ExperienceReplay
#MeanFieldRL
#MDP-PartialObservations

#DeterministicVRP
#DynamicVRP
#StochasticVRP
#OnlineVRP
#LargeScaleVRP
#GreenVRP
#VehicleDronesCollaboration

#TSP
#TSPTW
#CVRP
#VRPTW
#CVRPTW
#EVRPTW
#PDP
#HCVRP
#TDGVRPTW
#SDVRP
#mCVRPTW
#mVRPSTW
#ArcRouting
#TimeWindowConstraints

#LocalSearchHeuristics
#LargeNeighborhoodSearch
#AdaptiveLargeNeighborhoodSearch
#HyperHeuristic
#HeuristicOrExactBlends
#PatternBasedApproach
#BeamSearch
#ActiveSearch
#AdaptiveAntColony
#AntColonySystem
#GeneticAlgorithm
#GeneticProgramming
#TabuSearch
#TabuSearchHybrid
#HybridLocalSearchRL
#PopulationBasedEvolution

#BranchAndCut
#ColumnGenerationHeuristics
#CplexSolver
#LKH3Solver
#ORtoolsPackage
#VRPSolverBranchCutPrice
#SpreadsheetSolver

#SingleVehicleRouting
#MultiVehicleRouting
#CentralizedController
#MultiAgentRL
#MultiAgentRLCoordination
#RollingHorizon
#MDP
#PolicyIteration
#SafeRL
#SafeDrivingPolicy
#Symmetry
#RealWorldData
#EllipsoidalUncertainty
#BoxUncertainty
#DecisionTreeML
#RandomForestPredictor
#BayesianEnergyConsumption
#VRPBenchmarks
#ExplainableAIinVRP
#ResponsibleAI
#GANApproachesVRP
#TransferLearning
#Generalization
#DockerIntegration
#QuantumComputingVRP

#AlgorithmSelection

---
### CONTEXT

The goal of my thesis is to provide a **structured literature review** of **machine learning-based approaches** for the **Vehicle Routing Problem (VRP)** under **deterministic** conditions. Any paper focusing on stochastic or dynamic (non-deterministic) VRPs falls outside of this scope and should be marked as irrelevant.

Thesis Outline & Focus
1. **Literature Review (Chapter 3)**
   - Papers are clustered by **ML approach**:
     - **Supervised Learning** (e.g., imitation learning from optimal solutions, pointer networks in supervised mode)  
     - **Unsupervised Learning** (e.g., clustering for pre-processing, pattern discovery)  
     - **Reinforcement Learning (RL/DRL)** in deterministic settings (value-based RL, policy gradients, actor-critic)  
     - **Hybrid Methods** (ML-assisted exact solvers, ML-assisted heuristics)
   - We only include **deterministic VRP** research (no random/stochastic route planning).

2. **Performance Evaluation & Benchmarking (Chapter 4)**
   - Standard VRP metrics: solution quality (gap to best-known solutions), runtime, scalability  
   - Common benchmark libraries/datasets (e.g., CVRPLIB, TSPLIB)  
   - Comparison of ML-based models vs. classical optimization methods

3. **Outlook (Chapter 5)**
   - Challenges of applying ML to VRP: data availability, interpretability, integration with real logistics systems  
   - Future directions: combining ML + heuristics for large-scale VRPs, multi-objective VRP, all in deterministic settings

Research Motivation & Contribution
- Modern supply chains demand efficient, cost-effective, large-scale routing solutions.  
- VRP is computationally hard; exact methods can be slow to scale, while heuristics may yield only approximate solutions.  
- ML-based approaches (e.g., GNNs, RL) show promise in improving solution quality or computational speed, especially when combined with operations research techniques.  
- This thesis will categorize ML models, analyze their VRP applications, compare performance to classical methods, and outline future research directions.

Summary Format & Focus
- The overall literature review will span approximately 15 pages.
- Each paper will be **summarized in ~60-80 words**, highlighting:
  1. **Core Contribution** or innovation (What problem does it solve? What’s new?)  
  2. **ML Model & VRP Variant** (e.g., supervised pointer network for CVRP, RL-based approach for TSP, etc.)  
  3. **Research Gap** or primary challenge the paper addresses  
  4. **Relevance** to deterministic VRP research (papers on stochastic/dynamic VRP are excluded)

What to Emphasize When Analyzing Papers
1. **Deterministic VRP Only**: Exclude papers focusing on stochastic or dynamic VRPs.  
2. **ML Model & VRP Variant**: Clearly identify the Machine Learning model used and the specific VRP variant addressed (e.g., TSP, CVRP).  
3. **Core Contribution**: Summarize the main innovation or problem the paper addresses.  
4. **Research Gap**: Identify any notable research gap or challenge the paper aims to solve. 


## Task

1. **Read & Interpret the Paper**:  
	   - The paper focuses on Machine Learning-Based Approaches for Vehicle Routing Problems (VRP), as described in the CONTEXT section. 
	   - The .md file might contain OCR errors; do your best to interpret them correctly.  
	   - If crucial details (e.g., formulas, tables) are too garbled to interpret, please mention this under “Debug & Action Items”. 

2. **Extract Metadata**
	 - **Link to PDF**: (provide a placeholder for me to manually insert)  
     - **Tags**: Provide **10-20** relevant tags as inline “#tags” separated by spaces.  The goal is for me to be able to identify correlating papers and group them together. a 100% fit is not necessary as long as the paper has a close thematic link to the tag and it makes sense to cluster the specific paper with other papers holding the same tag. Tags MUST include the ML techniques and VRP variants used
	    - For example: `#ML-Supervised #ML-SupervisedImitation` etc. 
	     - Refer to the existing tag list in the `TAGS` section (see below).  
	     - Select the **relevant tags** from that existing list (use the format `#TagName`).  
	     - If the paper introduces new concepts not covered by the existing tags, propose **new** tags.  Make sure to NOT GENERATE GENERIC TAGS like VRP. The aim of the tags is to finely cluster the different papers; correlate them with a specific subsection within my thesis and then further correlate papers that e.g. use the same ML approach for the same VRP variant within the subsections. generic tags like '#VRP' are of no use. Make sure to only suggest new tags if its  necessary for a key aspect of the paper that would otherwise fall under the table (i don't want my tag list to get too large)
	     - **List these new tags** under “Debug & Action Items” in a fenced code block, with **one tag per line** (format `#NewTag`).  
	     - If **no** new tags are needed, mention that explicitly under “Debug & Action Items”.
     - **Relevant marker**: Boolean field — specify whether this paper is relevant (`true`) or not (`false`) to my overarching research scope as outlined in the CONTEXT section. Papers focussing on NON DETERMINISTIC VRP are automatically flagges as NOT RELEAVNT. please provide a very short explanation for your resoning behind your flagging of the paper as relevant/irrelevant in the “Debug & Action Items” section.
     - **Fit Score**: A number from **0** to **10**, indicating how well this paper fits into the context of my thesis on ML-based VRP solutions as outlined in the CONTEXT section (10 = very strong fit, 0 = not a fit).  The goal is to only include papers in my thesis that fit my research question. I have 20-30% excess in papers so I will rely on this to sort out the irrelevant ones
     - **State of the Art (SoA) Concepts**: A list (bullet) of key methods, VRP variants, or ML techniques that I might need to include in my “State of the Art” chapter if I reference this paper.  
     - **Performance Evaluation**: A Boolean field — does the paper conduct a standardized VRP performance evaluation? (yes/no)  
     - **Performance Evaluation Framework**: If yes, list the standard framework or dataset used (e.g., Solomon benchmarks, Christofides, Golden, etc.). Otherwise `"-"`. 

3. **Create a Structured Summary in English**:
	   - Summarize the paper highlighting the key findings, most significant insights and implications, key theories, and gaps identified
	   - Make sure to Write the summary in the format outlined in the following section
	   - Make sure the summary is taylored to my thesis with respect to the Info you have from the CONTEXT section
	   - Aim for about **1-2 pages** of text (roughly 600-1200 words), but primarily ensure all relevant information is included. The word count/length is not fixed, you are allowed to use more or less words depending on paper length. Consider that my thesis is a literature review, so i will only write the most important facts for a given paper in the span of 60-80 words.
	   - The summary shall be written in bullets, english
	   - Use clear, concise academic writing. 
	   - If the text references any images or diagrams that appear crucial to understanding, explicitly request them in “Debug & Action Items”.  
	   - Ignore small OCR or layout inconsistencies unless they completely block interpretation. 
	   - If the text includes formulas or code snippets that are partially garbled, do your best to reconstruct them or note the issue.  
	   - Use neutral, third-person tone.  

4. **Include the Following Sections** (in the order shown):
   - **Document Title**  
    - Format: `#  <Full Title of the Paper>`  
    - Example: `# [alesiani2022] Constrained clustering for the capacitated vehicle routing problem
    - don't put the heading in the document start with the title as the main md chapter and then proceed with the sub-chapter Metadata as stated below
    
   - **Metadata**  
     - As stated above
     
   - **Abstract**  
     - Copy the Abstract directly from the .md text if available. If not clearly marked, provide the best guess from the intro or summary portion. 
 
   - **Summary**  
     - Summarize the paper’s main ideas, methodology, and conclusions. Emphasize how it relates to Machine Learning and VRP. 
     - focus on the sections Introduction, Method, Experiments, Conclusion, and only briefly look at the chapter regarding related work
     - Aim to cover:  
       1. What ML techniques or algorithms are used?  
       2. Which specific VRP variant(s) are addressed?  
       3. Key results or findings.  
       4. Strengths, weaknesses, limitations.  
       5. Potential relevance for or impact on my broader research question as outlined in CONTEXT section. 
  

## **Output Formatting**  
   - Provide the entire summary in **one fenced code block** using Markdown syntax. For example:
     ```
     # Title

     ## Metadata
     ...
     
     ## Abstract
     ...
     
     ## Summary
     ...
     ```
   - After the code block, include any **Debug & Action Items** or additional comments (e.g., requests for images, notifications about OCR errors, disclaimers if something is missing).


---
**Now, please proceed with the following steps:**

1. **Name this chat** according to the provided paper tag, e.g. reed2014 and the ML variant used (so I can easily pin this chat in my notes).  
2. **Create the summary** in the required Markdown structure according to the guidelines above. 
3. **Place any additional questions or comments** outside the code block, under “Debug & Action Items”.  

---

```
## TAGS
<!-- Insert your existing list of NEW tags here, e.g.:
#NewTag1
#NewTag2
...
-->
```

