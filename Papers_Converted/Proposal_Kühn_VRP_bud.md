![Image](image_000000_da8f1eb061219649d78cb95ea422d148d089b6db1f62496375714183476b1033.png)

## Literature Review: Machine Learning-Based Approaches for Vehicle Routing Problems

## Motivation

Modern supply chain management faces increasing complexity, driven by global competition, the rise of e-commerce, and the urgent need for sustainable practices. Transportation costs, which make up a significant part of logistics expenses, are heavily influenced by challenges such as last-mile delivery and dynamic customer demands. These pressures make optimizing routing decisions a critical factor for companies looking to remain competitive. At the core of these efforts lies the Vehicle Routing Problem (VRP), which seeks to determine the most efficient vehicle routes to minimize costs and effectively meet customer requirements.

VRPs face significant challenges due to their computational complexity, with solution times increasing exponentially as the problem size grows. Traditional methods, such as linear and mixed-integer programming, often struggle to scale effectively, especially in dynamic environments with fluctuating customer demand or varying time constraints. Heuristic approaches, while faster, generally provide approximate solutions and lack the precision required for larger or more complex instances. One potential approach to address these challenges is using machine learning (ML). Approaches such as Graph Neural Networks allow the representation of complex structures in VRPs and can improve routing efficiency (Bai et al., 2021). Reinforcement learning methods allow for adaptive decision-making in dynamic environments, where customer demands or traffic conditions change frequently (Shahbazian et al., 2024). Furthermore, hybrid approaches that combine ML techniques with traditional heuristics have shown a potential to enhance scalability and solution quality for complex VRP variants (Bogyrbayeva et al., 2024).

## General contribution to research

This thesis by Dennis Kühn contributes to VRP research by providing a structured literature review of machine learning-based approaches, focusing on the challenges and opportunities unique to routing problems. By classifying ML models, analyzing their applications, and evaluating their performance against traditional methods, it establishes a foundation for future studies. The findings aim to support further research on combining ML with heuristic methods to address large-scale VRP challenges. The research will comprise the following working steps:

- 1. Establish the theoretical foundation by outlining the mathematical formulation of VRP and summarizing traditional solution methods while introducing relevant machine learning concepts (supervised, unsupervised, and reinforcement learning) and their applicability to VRP
- 2. Classify machine learning models used in VRP , focusing on the adaptation of techniques like Graph Neural Networks and clustering algorithms to address unique challenges in VRP
- 3. Investigate the application of ML-based approaches across various scenarios, including urban logistics, lastmile delivery, and transportation optimization, identifying key differences and use cases
- 4. Evaluate the performance of ML-based methods compared to traditional optimization techniques (exact and heuristic), focusing on solution quality, computational efficiency, and scalability
- 5. Identify common obstacles in applying ML to VRPs, including data requirements and challenges in integrating ML with traditional optimization methods
- 6. Suggest future research directions, including the combination of ML and heuristic methods for large-scale problems

## References

Bai R, Chen X, Chen ZL, Cui T, Gong S, He W, Jiang X, Jin H, Jin J, Kendall G, Li J, Lu Z, Ren J, Weng P , Xue N, Zhang H (2021) Analytics and machine learning in vehicle routing research. International Journal of Production Research 61:1-27.

Bogyrbayeva A, Meraliyev M, Mustakhov T, Dauletbayev B (2024) Machine learning to solve vehicle routing problems: A survey. IEEE Transactions on Intelligent Transportation Systems 25(6):4754-4772.

Shahbazian R, Pugliese LDP , Guerriero F, Macrina G (2024) Integrating machine learning into vehicle routing problem: Methods and applications. IEEE Access 12:93087-93115.