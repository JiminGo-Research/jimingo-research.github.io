---
layout: page
title: Graph Attention Networks for Supply Chain Relationship Prediction
description: Multi-feature representation-based graph attention networks for predicting potential supply relationships in a large-scale supply chain network
img: assets/img/GAT_thumbnail.png
importance: 2
category: Undergraduate
related_publications: True
---

<b>Overview</b>

Supply chain disruptions — as seen during COVID-19 — expose the risks of relying on a fixed, partially-observed set of supplier relationships. Identifying <b>latent (undiscovered) supply relationships</b> in large-scale networks enables companies to proactively diversify suppliers, reduce procurement costs, and build resilience.

This project proposes <b>MFRGAT (Multi-Feature Representation-based Graph Attention Network)</b>, a graph learning framework that predicts potential supplier relationships by capturing both the topological structure of the supply chain network and the semantic industry characteristics of each company.

<a href="https://10.1016/j.eswa.2025.128593"Multi-feature representation-based graph attention networks for predicting potential supply relationships in a large-scale supply chain network</a>, D. Lee, J. Go, T. Noh, S. Song, Expert Systems with Applications, 2025, DOI: 10.1016/j.eswa.2025.128593

<b>Problem Definition</b>

A supply chain network is modeled as a directed graph $$G = (V, E)$$, where nodes $$V$$ represent companies and edges $$E$$ represent known supply relationships. The task is link prediction: given the observed network and node features, predict which unobserved company pairs are likely to have a supply relationship.
This is challenging because:

- The network is <b>large-scale and sparse</b> — 211,434 companies, 220,971 relationships across 20 industries in South Korea
- Standard ML methods (SVM, random forest) capture node features but <b>ignore network topology</b>
- Standard GNNs use </b>fixed aggregation weights</b> that fail to distinguish the varying importance of different supplier connections

<div class="row justify-content-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid
       path="assets/img/mfrgat_distribution.png"
       title="Industry distribution of supply chain dataset"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Fig. 2 from the paper — the two chord diagrams showing the distribution of companies across 20   industries in (a) training dataset and (b) test dataset. Manufacturing dominates at ~53%.
</div>

<b>Proposed Method: MFRGAT</b>

MFRGAT operates in an <b>encoder–decoder</b> architecture:
<div class="row justify-content-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid
       path="assets/img/GAT_thumbnail.png"
       title="MFRGAT framework overview"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  MFRGAT framework overvie
</div>

<b>Encoder: Multi-Feature Representation</b>

- <b>Graph Representation (node2vec)</b>: Captures the topological structure of the supply network using random walks. Each company is embedded into a 128-dimensional vector $$T_i$$ that encodes its position and connectivity patterns within the network.
- <b>Product Representation (fastText)</b>: Encodes the semantic meaning of each company's product portfolio using a pre-trained 300-dimensional word embedding. Product descriptions are aggregated into a company-level semantic vector $$S_i$$.
-The final input to the decoder for each company $$i$$ is the concatenation $$h_i = {T_i, S_i, I_i}$$, where $$I_i$$ is a one-hot industry feature vector (20 dimensions), yielding a 448-dimensional representation.

<b>Decoder: GAT-Based Link Prediction</b>

A <b>Graph Attention Network (GAT)</b> decoder learns attention coefficients $$\alpha_{ij}$$ between company pairs, weighting the influence of each neighbor adaptively. A prediction above 0.5 is classified as a positive supply relationship.

<b>Experimental Results</b>

<b>Performance vs. Baselines</b>

MFRGAT was evaluated against three baselines — SVM, a DNN-based business partner recommendation model (DBR), and IDNS-GCN — across five test datasets with varying negative-to-positive sampling ratios (D1–D5).

All performance differences were statistically significant (paired t-test, 99% confidence, all p-values < 0.01).

<div class="row justify-content-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid
       path="assets/img/mfrgat_f1_industry.png"
       title="F1 score by industry: IDNS-GCN vs MFRGAT"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Fig. 5 from the paper — the two industry-level F1 score heatmaps (a) IDNS-GCN and (b) MFRGAT, with axes representing industry indices. MFRGAT achieves F1 > 0.8 across most industries while IDNS-GCN falls below 0.7 in many categories.
</div>

<b>Ablation Study</b>
Graph representation contributes more to performance than product representation alone, but combining both yields the best results across all datasets and class imbalance settings.

<div class="row justify-content-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid
       path="assets/img/mfrgat_ablation.png"
       title="Ablation study results"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Fig. 6 from the paper — three bar charts comparing GAT, PRGAT, GRGAT, MFRGAT across D1–D5 on (a) Accuracy, (b) F1 score, (c) Specificity. MFRGAT consistently leads across all settings.
</div>

<b>Supply Chain Extension Visualization</b>
<div class="row justify-content-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid
       path="assets/img/mfrgat_network.png"
       title="Original vs. extended supply chain network"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Fig. 7 from the paper — side-by-side circular network visualizations of (a) the original supply chain and (b) the extended network after MFRGAT predictions for a suburban region in South Korea. Node color = industry, node size = degree centrality.
</div>
