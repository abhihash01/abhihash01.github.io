---
title: "GPT1" # Title of the blog post.
date: 2025-02-23T23:54:26-05:00 # Date of post creation.
description: "A primer on GPT1" # Description used for search engine.
featured: true # Sets if post is a featured post, making appear on the home page side bar.
draft: false # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
# menu: main
usePageBundles: true # Set to true to group assets like images in the same folder as this post.
#featureImage: "BERT.png" # Sets featured image on blog post.
#featureImageAlt: 'Bert Architecutre' # Alternative text for featured image.
#featureImageCap: 'Bert Architecture' # Caption (optional).
#thumbnail: "BERT.png" # Sets thumbnail image appearing inside card on homepage.
#shareImage: "BERT.png" # Designate a separate image for social media sharing.
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
categories:
  - Primer
  - LLM Series
  - Pretraining
tags:
  - LLM 
  - GPT1
series:
  - LLM
  - Pretraining
# comment: false # Disable comment if false.
---




# Primer for GPT-1 Paper: Improving Language Understanding Through Generative Pre-Training

This document provides a technical deep dive into the architecture, methodology, and implementation details of GPT-1 as presented in the original paper "Improving Language Understanding by Generative Pre-Training" (Radford et al., 2018). Contains all essential equations, architectural diagrams, and implementation considerations.

## 1. Architecture Overview
The GPT-1 model employs a transformer-based decoder architecture with **117 million parameters** organized as:

### 1.1 Core Components
**Transformer Blocks**: 12 successive decoder layers  
**Attention Heads**: 12 parallel attention mechanisms per layer  
**Model Dimension**: 768 (d_model)  
**Feed-Forward Dimension**: 3072 (d_ff)  
**Maximum Context Length**: 512 tokens  

The mathematical formulation of the transformer block:

$$
h_0 = UW_e + W_p \quad \text{(Input embedding + positional encoding)}
$$
$$
h_l = \text{transformer_block}(h_{l-1}) \quad \forall l \in [1,12]
$$
$$
P(u) = \text{softmax}(h_{12}W_e^T) \quad \text{(Output probability)}
$$

Where:  
- \(U\): Input token indices  
- \(W_e\): Token embedding matrix  
- \(W_p\): Positional embedding matrix

## 2. Pre-Training Objective
The language modeling objective maximizes:

$$
\mathcal{L}_1(\mathcal{D}) = \sum_{i=1}^m \log P(u_i | u_{i-k}, \ldots, u_{i-1}; \Theta)
$$

Implemented as **masked self-attention** with:
- Context window size \(k = 512\)
- Batch size 64
- Adam optimizer with learning rate 2.5e-4

## 3. Fine-Tuning Procedure
Task-specific adaptation uses:

$$
\mathcal{L}_2(\mathcal{D}) = \mathcal{L}_1(\mathcal{D}) + \lambda \sum_{(x,y)} \log P(y|x)
$$

Where \(\lambda\) is the auxiliary weight (typically 0.1-0.5). The input transformation for different tasks:

| Task Type       | Input Format                          |
|-----------------|---------------------------------------|
| Classification  | [Start] Text [Extract] [Delim] [Class]|
| Entailment      | [Start] Premise [Delim] Hypothesis   |
| Similarity      | [Start] Text1 [Delim] Text2 [Extract]|

## 4. Key Implementation Details

### 4.1 Tokenization
**Byte Pair Encoding (BPE)** with:
- Vocabulary size: 40,000 tokens
- Merges: 50,000 operations
- Special tokens: [Start], [Delim], [Extract], [Class]

### 4.2 Positional Encoding
Learned embeddings showing linear position relationships:
