1. **Reinforcement Learning (RL)**
   - *Policy-Gradient-Methoden*:  
     - REINFORCE (z. B. mit Greedy Rollout, Baselines, etc.)  
     - Actor–Critic (inkl. Varianten wie Advantage Actor–Critic)  
   - *Value-based RL*:  
     - Q-Learning, Deep Q-Learning (DQN)  
   - *Multi-Agent RL*:
     - Gemeinsame (kooperative) RL-Ansätze für mehrere Fahrzeuge/Agenten  

2. **Neural Network Architekturen innerhalb von RL oder als Embeddings**
   - *Graph Neural Networks (GNN)*:  
     - Graph Convolutional Networks (GCN), Structure2Vec (S2V), etc.  
   - *Attention-/Transformer-Modelle*:  
     - Multi-Head Attention, Self-Attention, Pointer Networks  
   - *Recurrent/Sequential Modelle*:  
     - LSTM, RNN (teils für Zustandsmodellierung)  
   - *Variational Autoencoder (VAE)*:  
     - Für latente Repräsentationen von Teillösungen  

3. **Supervised Learning (SL)**
   - Vortrainierte Modelle zur Entscheidungsunterstützung (z. B. Insertionsheuristik)  
   - Klassifikatoren/Regressoren zur Parameterfeinjustierung in Heuristiken oder Spalten-/Arc-Auswahl  
   - Lernen von „Delegation“ oder „Arc-Präferenzen“ auf Basis vorhandener (Optimal-)Lösungen  

4. **Hyper-/Metaheuristik mit ML**
   - *Large Neighborhood Search (LNS)*, zerstören & reparieren mittels neuronaler Modelle  
   - *Evolutionäre Algorithmen*:  
     - GA/PSO/NSGA-II mit ML-gesteuerter Operator- oder Parameterwahl  
   - *Heuristikkombination*:  
     - Dynamische Hyper-Heuristik (z. B. Multiarmed-Bandit-Ansätze)  
   - *Clustering-gestützte Verfahren*:  
     - ML-Algorithmen (K-Means, DBSCAN, etc.) kombiniert mit Routingheuristiken  

5. **Hybride Ansätze (Kombination mehrerer ML-Methoden)**
   - RL + (Meta-)Heuristiken (z. B. OR-Tools, LNS, 2-Opt, Insert, Swap)  
   - Reinforcement Learning + Evolutionäre Operatoren  
   - Wertfunktionen/Heuristiken gelernt, klassische Optimierung als „Fein-Tuning“ (z. B. MIP, Column Generation)  