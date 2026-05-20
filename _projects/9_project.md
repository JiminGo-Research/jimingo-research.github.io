---
layout: page
title: Physics-Aware Thermal Field Prediction for Multi-Layer Substrate Warpage Description
description:
img: "assets/img/Warpage_thumbnail.png"
importance: 2
category: Graduate Research
---

<b>Overview</b>

As semiconductor packages shift toward multi-layer stacked substrate architectures, thermomechanical warpage becomes a critical reliability concern. When only the top substrate carries an active die, non-uniform heat generation creates complex thermal gradients that propagate through the stack and drive differential warpage — making package-level deformation difficult to predict with purely simulation-based approaches.
This project develops a <b>Physics-AI hybrid pipeline</b> that combines high-fidelity <b>CFD/thermal simulation</b> with deep learning models to predict the full thermal field across the stacked substrate assembly, enabling fast and accurate warpage estimation without repeated expensive simulations.

Status: Active — dataset generation in progress; initial model benchmarking complete.

<div class="row justify-content-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid
       path="assets/img/bbox_optimization.gif"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<b>Problem Formulation</b>

<b>Setup</b>:

- Multiple substrate layers stacked vertically
- Only the top substrate carries an active die (localized heat source)
- Objective: predict the thermal field distribution across the entire multi-layer stack given varying die power, material configurations, and boundary conditions

<b>Why it's hard</b>:

- Heat conduction through dissimilar materials creates sharp thermal gradients at interfaces
- Warpage is sensitive to the spatial distribution of temperature, not just peak temperature
- Full 3D CFD simulation is accurate but computationally prohibitive for design-space exploration

<b>Methodology</b>

<b>1. Dataset Generation — PyFluent CFD Simulations</b>

I use Ansys PyFluent to automate thermal simulations across a parametric design space:

- Vary die power density, substrate material properties (CTE, thermal conductivity), layer thicknesses, and boundary conditions
- Extract full 3D thermal field outputs as structured mesh data
- Pipeline produces paired input–output samples: (geometry + boundary conditions) → thermal field

<b> 2. Baseline: Uniform Temperature Assumption</b>

As an initial benchmark, both models below were evaluated under a uniform temperature assumption (spatially constant input temperature) to establish baseline performance before introducing full spatial thermal field inputs.

<b>Model1: DeepONet (Deeo Operator Network)</b>

DeepONet learns a mapping between function spaces — here, from the input thermal boundary condition function to the output temperature field function. This operator-learning framework is well-suited for physics problems where the output is a continuous field.

- Branch net: encodes the input boundary condition (die power distribution)
- Trunk net: encodes the query spatial coordinates
- Trained on PyFluent simulation data

<div class="row justify-content-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid
       path="assets/img/DeepONet_Framework.png"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  DeepONet Architecture
</div>

<b> Model 2 — U-Net (Convolutional Encoder-Decoder)</b>

U-Net treats the thermal field prediction as an image-to-image regression task. The symmetric encoder-decoder with skip connections is effective at preserving spatial structure across multiple resolution scales.

- Input: 2D/3D spatial map of boundary conditions and geometry encoding
- Output: predicted temperature field over the substrate domain
- Skip connections preserve fine-grained spatial details critical for accurate warpage estimation

<div class="row justify-content-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid
       path="assets/img/Unet-architecture.png"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  UNet Architecture
</div>

<b>Next Steps</b>

- Complete full parametric dataset generation with PyFluent (non-uniform temperature inputs)
- Retrain and evaluate both models on spatially varying thermal inputs
- Explore Physics-Informed Neural Networks (PINNs) as a data-efficient alternative by embedding heat conduction PDE constraints
- Couple thermal field predictions with a warpage/stress model (analytical or FEM-based) for end-to-end deformation prediction

<b>Technology Stack</b>

- <b>CFD Simulation</b>: Ansys PyFluent
- <b>Deep Learning</b>: PyTorch
- <b>Neural Networks</b>: DeepONet, Physics-Informed DeepONet
- <b>Data Preprocessing</b>: Numpy, SciPy
- <b>Visualization</b>: Matplotlib
