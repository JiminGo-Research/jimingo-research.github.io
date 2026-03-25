---
layout: page
title: Agentic AI-Towards Robust GUI Agents for Real-World Applications
description: GUI Agent Tuning for Interface Prediction 
img: "assets/img/Agentic_AI.webp"
importance: 1
category: Graduate
related_publications: false
---

This project is group Project for CSE 475: Foundations of Machine Learning at Arizona State University

<b>Role</b>: Team Leader (Project Management, Data Preprocessing, and Transformer Model Development)

You can find the full implementation, datasets at the following link and here is the <a href="assets/pdf/Final_Report_Group 17.pdf"> paper </a> . 

<b>Background & Objective</b>

In the rapidly evolving landscape of Human-Computer Interaction (HCI), Graphical User Interfaces (GUIs) have become a primary focus for machine learning applications aimed at enhancing user productivity. The core objective of this project was to develop a robust "Agentic AI" capable of accurately predicting a user’s next action within a digital interface. By anticipating user intent, such an agent can significantly reduce the cognitive load and time required to navigate complex software applications, ultimately leading to a more seamless and intuitive user experience across a generalized set of platforms.

<b>Data Engineering & Methodology</b>

To achieve high-fidelity predictions, our team focused on modeling the "user workflow" as a sequential data problem. We utilized a comprehensive dataset comprising various UI elements, user actions (clicks, scrolls, inputs), and screen transitions. A critical phase of the project involved sophisticated data engineering, where these multi-modal interactions were transformed into <b>token-based sequences</b>. This tokenization allowed the models to process human-computer interaction as a structured language, enabling them to learn the underlying patterns and dependencies of typical user workflows.

<b>Technical Implementation & Comparative Analysis</b>
The research involved a rigorous comparative study of several state-of-the-art deep learning architectures to identify the most effective model for real-time GUI prediction. We designed and implemented multiple pipelines using:

<b> Convolutional Neural Networks (1D-CNN)</b> to capture local spatial-temporal patterns in action sequences.

<b> Recurrent Neural Networks (GRU and LSTM)</b> to manage long-term dependencies within extended user sessions.

<b>Transformer Models</b> to leverage self-attention mechanisms for global context understanding.

Throughout the experimentation phase, I conducted extensive hyperparameter tuning and architectural refinements to discover the optimal configuration for each model. This process involved balancing model complexity with inference speed, a crucial factor for real-world GUI agents that must respond near-instantaneously to user inputs.

<b>Results & Key Insights</b>

The experimental results yielded a significant architectural insight: while Transformers are often the gold standard for sequence tasks, a lightweight 1D-CNN-based model demonstrated superior performance in the specific domain of GUI interaction sequences. This model achieved a Validation Accuracy of 0.5598 and a Mean Reciprocal Rank (MRR) of 0.7173, outperforming more complex counterparts in both accuracy and generalization. This finding suggests that for GUI-based agents, capturing local sequential patterns is often more effective than global attention-based modeling. The project concluded with a proven framework for intelligent GUI automation, providing a foundation for future AI-driven interface optimization and autonomous digital assistants.
