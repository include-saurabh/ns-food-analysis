# Network Analysis of Food and Nutrition Data

This project uses network science methodologies to analyze a dataset of 8,789 food items, each detailed with macronutrients, micronutrients, and calories. The goal is to uncover hidden patterns, identify key nutritional players, and provide a framework for data-driven dietary insights.

## Project Overview

Understanding the complex relationship between food items and their nutritional content is crucial for public health, dietary planning, and food science. This project leverages network science to transform a large food dataset into interconnected structures, offering a novel perspective on food relationships.

I constructed and analyzed three types of networks:
1.  A **Food-Nutrient Bipartite Graph** to understand nutrient distribution across foods.
2.  A **Nutrient-Nutrient Correlation Network** to identify relationships between nutrients.
3.  A **Food-Food k-Nearest Neighbors (k-NN) Similarity Graph** to cluster foods based on nutritional profiles.

Key analyses include community detection to identify nutritional categories, centrality measures to pinpoint archetypal and bridge foods, and structural component analysis. The findings reveal distinct nutritional groupings and influential food profiles.

A basic web-based user interface was also developed using Streamlit to explore food similarities and complementary food pairings.

## Methodology

### 1. Data Acquisition and Preprocessing
The dataset comprises 8,789 food items with detailed nutritional information (calories, macronutrients, micronutrients).
Preprocessing steps:
*   **Cleaning:** Handling missing values (imputation with zero), removing units, converting to numeric format.
*   **Standardization:** Converting nutrient values to common units (e.g., micrograms to milligrams).
*   **Normalization (Z-score):** Using `StandardScaler` from scikit-learn to normalize nutrient data, ensuring equal contribution to similarity measures.

### 2. Network Construction

*   **Food-Nutrient Bipartite Graph:**
    *   **Nodes:** Two sets: (1) Food items, (2) Nutrients.
    *   **Edges:** Connects a food to a nutrient if the food contains it.
    *   **Weights:** Normalized quantity (Z-score) of the nutrient in that food.
    *   **Tool:** Gephi for visualization of sampled versions.

*   **Nutrient-Nutrient Correlation Network:**
    *   **Nodes:** Nutrients.
    *   **Edges:** Connects two nutrients if their presence and quantities are correlated across foods.
    *   **Weights:** Spearman correlation coefficient (thresholded at `|correlation| > 0.4`).
    *   **Tool:** Python (pandas for correlation), Gephi for visualization.

*   **Food-Food k-Nearest Neighbors (k-NN) Similarity Graph:**
    *   **Nodes:** Food items.
    *   **Edges:** Directed edge from food A to food B if B is one of the top 'k' (k=10) most nutritionally similar foods to A.
    *   **Similarity Metric:** Cosine similarity on Z-score normalized nutrient profiles.
    *   **Weights:** Cosine similarity score.
    *   **Tool:** Python (scikit-learn for similarity, NetworkX for graph construction), Gephi for analysis and visualization.

### 3. Network Analysis Techniques
Standard network science metrics and algorithms were applied using Gephi and NetworkX:
*   **Centrality Measures:** In-Degree, PageRank, Betweenness Centrality.
*   **Community Detection:** Modularity (Louvain algorithm).
*   **Connected Components Analysis.**
*   **Layout Algorithms:** ForceAtlas2 for visualization.

## Key Findings & Analyses

### 1. Food-Nutrient Bipartite Graph Analysis

![bipartite](https://github.com/user-attachments/assets/98f1b077-6c31-44b3-a8c4-493b85396023)

*   **Nutrient Node Degree (Prevalence):**
    *   *Highest Degree (Most Prevalent):* Calories, Water, Sodium, Protein, Fat, Ash, Iron, Potassium, Calcium – fundamental components found across most foods.
    *   *Lowest Degree (Specialized):* Alcohol, Vitamin B12, Galactose, Theobromine, Caffeine – found in specific food categories.
*   **Food Node Degree (Nutritional Diversity):**
    *   *Highest Degree (Most Diverse):* Complex, multi-ingredient prepared foods (e.g., Chicken pot pie, fast food wraps/pizza).
    *   *Lowest Degree (Least Diverse):* Simple items (e.g., Bottled waters, Stevia extract).
*   **Insights:** Effectively differentiates core, ubiquitous nutrients from specialized ones and highlights the nutritional diversity spectrum of foods.

### 2. Food-Food k-NN Similarity Network Analysis

![food](https://github.com/user-attachments/assets/af58fdcd-81a8-4ec9-92fc-dbc1d584c2ea)

![food1](https://github.com/user-attachments/assets/ab2683d4-09e1-431b-b006-3916cf075851)

This network connects foods based on the similarity of their normalized nutritional profiles (k=10).
*   **Community Detection:** Revealed clusters of foods with similar nutritional profiles, forming data-driven nutritional categories (e.g., "Leafy Greens & Low-Calorie Vegetables," "Nuts & Seeds," "Red Meats").
*   **Centrality Analysis:**
    *   *In-Degree Centrality:* Identified "archetypal" or "reference" foods within nutritional niches (e.g., 'spinach' in a green vegetable cluster).
    *   *PageRank Centrality:* Reinforced In-Degree findings, highlighting influential food profiles.
    *   *Betweenness Centrality:* Identified "nutritional bridges" – foods with mixed profiles (e.g., some fast foods, cakes) connecting diverse food groups.
*   **Insights:** The k-NN graph reveals natural food clustering, key archetypal foods, and bridge foods, supporting the food similarity feature in the UI.

### 3. Nutrient-Nutrient Correlation Network Analysis

![Screenshot 2025-06-02 143540](https://github.com/user-attachments/assets/ed6a9150-a898-4ffd-8fee-2b5e4ebafaf5)

This network explores relationships between nutrients based on co-occurrence patterns.
*   **Network Construction:** Built using Spearman correlation coefficients (thresholded).
*   **Community Detection:** Revealed clusters of nutrients that tend to vary together, such as:
    *   Macronutrients and their components (Protein with Amino Acids; Fat with Fatty Acids).
    *   Fat-soluble vitamins (A, D, E, K) with fats.
    *   B-vitamins.
    *   Electrolytes.
*   **Centrality Analysis:**
    *   *Degree Centrality:* Identified highly connected nutrients like protein, fat, water, potassium.
    *   *Betweenness Centrality:* Nutrients like 'Protein' bridge different clusters (e.g., amino acid clusters).
*   **Insights:** Confirmed known biochemical relationships and highlighted co-occurrence patterns, distinguishing core nutrients from specialized ones and showing separation between fat-centric and carb-centric clusters.

## Web User Interface

A web application was developed using Streamlit for interactive exploration of food similarities and complementary food pairings.

### Features:
1.  **Food Similarity Exploration:**
    *   Users select a food item.
    *   The system displays the top N most nutritionally similar foods (from the k-NN graph).
    *   Provides a local network visualization and side-by-side nutritional profile comparison.

![web1](https://github.com/user-attachments/assets/c9c6be47-847b-4041-bfd0-23fbae98880a)

![web2](https://github.com/user-attachments/assets/ddfebc57-be42-41a9-ace3-137614a35afb)

2.  **Complementary Food Recommendation:**
    *   **Single Food Mode:** Identifies nutrients a selected food is low in (based on Z-scores) and suggests other foods high in those lacking nutrients (using the Food-Nutrient Bipartite Graph).
    *   **Multi-Food Group Mode:** Calculates the average nutritional profile of a selected group, identifies collective low-scoring nutrients, and suggests complementary foods.
  
![web3](https://github.com/user-attachments/assets/40e36d42-0fa1-428e-a621-f42f4fd4a8fa)

### Insights from UI:
The UI translates complex network analysis into actionable insights, facilitating healthier food choices and exploration of nutritional relationships.

## Technologies Used

*   Python
*   Pandas (Data manipulation)
*   Scikit-learn (Normalization, Cosine Similarity)
*   NetworkX (Graph construction and analysis)
*   Gephi (Network visualization and analysis)
*   Streamlit (Web application development)

## Conclusion

This project demonstrates the significant potential of network science in unraveling complex relationships within food and nutrition data. I successfully constructed multiple network representations, identified key structural features, and developed an interactive tool.

This data-driven understanding of the food landscape can empower individuals in making informed dietary choices and aid researchers in public health and food science.
