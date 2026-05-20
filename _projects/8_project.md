---
layout: page
title: Physics-Aware Neural Operator for Thermal Design Optimization in Advanced Packaging
description:
img: "assets/img/Neural_operator.png"
importance: 1
category: Graduate Research
---
<b>Overview</b>

This is my current graduate research project at Arizona State University.

Advanced semiconductor packaging, particularly 2.5D SiP and 3D-IC architectures, faces critical thermal management challenges. As chip density increases, heat dissipation becomes a fundamental bottleneck for package reliability and performance. Traditional CFD-based simulation methods can accurately model thermal behavior, but they are computationally expensive when thousands of design variations must be evaluated during early-stage design-space exploration.
This project develops a <b>physics-aware inverse modeling pipeline</b> that learns to map thermal boundary conditions directly to optimal geometry parameters, enabling fast and physically consistent design generation without running a full simulation for each candidate.

For a more comprehensive summary of the work completed during Spring 2026, the full report is available <a href="https://docs.google.com/document/d/189XBAXbC74NB_mSahvGbcd9FsYJDYbVE/edit?usp=drive_link&ouid=104050058597919759139&rtpof=true&sd=true">here</a>. The following sections highlight the key research directions and current progress. 

<b>Motivation</b>

The core challenge is this: a designer exploring microchannel geometry for cooling a 3D-IC needs to evaluate how small changes in channel shape affect temperature distribution across the die. Running a full Ansys/Fluent CFD simulation for every candidate takes hours. A surrogate model that can predict the thermal field — or better yet, generate a geometry given a thermal target — can compress that loop from hours to milliseconds.
<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/FNO-1.jpg"
        title="Thermal hotspot formation in advanced 2.5D/3D integrated packaging architectures"
        class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Thermal hotspot formation in advanced 2.5D/3D integrated packaging architectures
</div>

<b>Research Arc: From Operator Learning Foundations to Inverse Design</b>

This project is built in stages. The foundational methodology work (neural operator learning) directly feeds into the full inverse design pipeline.

<b>Stage 1 — Literature Survey: Neural Operators for PDE Surrogate Modeling</b>

Before building the pipeline, I conducted a systematic review of neural operator methods for PDE-based surrogate modeling, covering:

- Graph Kernel Network (GKN) — the first neural operator formulation, using graph-based message passing to approximate solution operators across different discretizations

- Fourier Neural Operator (FNO) — performs non-local operator learning in Fourier space via FFT, enabling efficient global spatial reasoning

- Physics-Informed Neural Operator (PINO) — extends FNO by incorporating PDE residual constraints into training, improving physical consistency when labeled data is scarce

You can find detailed literature survey results <a href="https://docs.google.com/presentation/d/1mFQrFEUKPdpj7XBUYKJmyj5gPp7OLWH1/edit?usp=drive_link&ouid=104050058597919759139&rtpof=true&sd=true">here</a>.

<b>Stage 2 — FNO Reproduction</b>

To validate my implementation before applying FNO to the target problem, I reproduced the original <a href="
https://doi.org/10.48550/arXiv.2010.08895">FNO paper</a> across three benchmark problems (1D Burgers' equation, 2D Darcy flow, 2D Navier-Stokes), achieving lower relative errors than the reported baselines in all cases. For detailed results, please view <a href="https://docs.google.com/document/d/189XBAXbC74NB_mSahvGbcd9FsYJDYbVE/edit?usp=drive_link&ouid=104050058597919759139&rtpof=true&sd=true">comprehensive report</a> section 3.

<b>Stage 3 - Dataset Generation — Automated CFD Pipeline via PyFluent</b>

A key contribution of this stage was <b>building an end-to-end CFD automation script using PyFluent</b>, Ansys Fluent's Python API. Rather than manually configuring and running each simulation case in the Ansys GUI, which is time-consuming and error-prone at scale, the script programmatically handles the full simulation workflow for each geometry and boundary condition combination:

- <b>Geometry & mesh generation</b>:wavy channel geometry is parameterized by amplitude, wavelength, and phase; the script generates and meshes each variant automatically

- <b>Solver configuration</b>:boundary conditions (inlet velocity, wall temperature, outlet pressure) are set programmatically per case

- <b>Solution & export</b>:the solver runs to convergence and exports the resulting pressure, temperature, and velocity fields as structured data

This automation reduced per-case turnaround from manual GUI operations to a fully unattended batch process, making it practical to generate the 1,200-sample dataset needed for training. The script is designed to scale — adding new geometry parameters or boundary condition ranges requires only changes to the input configuration, not manual re-work.

<b>Stage 4 - Multiphysics Field Prediction</b>

I then extended FNO to a custom CFD-generated dataset: a 2D wavy channel with varying geometry (amplitude, wavelength) and inlet velocity, simulated in Ansys Fluent. The model was trained to predict four coupled output fields simultaneously, pressure, temperature, x-velocity, and y-velocity, from boundary condition and geometry inputs alone.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/FNO-2.jpg"
        title="Interpolation case: Unit Gaussian vs MinMax normalization"
        class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Interpolation case: Unit Gaussian vs MinMax normalization
</div>

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/FNO-3.jpg"
        title="Extrapolation case with the lowest inlet velocity, uin=0.0501 m/s: Unit Gaussian vs. MinMax normalization"
        class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Extrapolation case with the lowest inlet velocity, uin=0.0501 m/s: Unit Gaussian vs. MinMax normalization
</div>

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/FNO-4.jpg"
        title="Extrapolation case with the highest inlet velocity, uin=0.1999 m/s: Unit Gaussian vs. MinMax normalization"
        class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Extrapolation case with the highest inlet velocity, uin=0.1999 m/s: Unit Gaussian vs. MinMax normalization
</div>
<b>Key findings:</b>
FNO successfully learned geometry-dependent multi-field predictions from 1,200 CFD samples
Model width had a clear effect on accuracy; the problem required sufficient latent capacity to learn coupled field relationships
Normalization strategy critically affected extrapolation robustness: Unit Gaussian normalization maintained stable errors on extreme inlet velocities where MinMax normalization degraded sharply (e.g., pressure relative L2 error: 0.067 vs. 3.58 under extrapolation)

<b>Stage 5 — Physics-Aware Inverse Modeling Pipeline (In Progress)</b>

The full pipeline combines two components:

- Forward model (FNO): Given a microchannel geometry and boundary conditions, predict the resulting thermal field. This is trained on CFD simulation data generated via automated PyFluent workflows.

- Inverse model (Diffusion Model): Given a target thermal profile, generate candidate microchannel geometries that would produce it. The diffusion model is conditioned on the thermal target and guided by the FNO forward model to enforce physical feasibility.

Together, these form an autonomous design generation loop: a thermal requirement goes in, physically plausible geometries come out — no manual design iteration required.

<b>Tools & Methods</b>

<b>CFD Simulation</b>: Ansys Fluent / PyFluent (automated data generation pipeline)

<b>Neural Operators</b>: Fourier Neural Operator (FNO), Physics-Informed Neural Operator (PINO)

<b>Generative Modeling</b>: Diffusion model for geometry generation

<b>Frameworks</b>: PyTorch, CUDA GPU computing

<b>Target Application</b>: Microchannel cooling in 2.5D SiP and 3D-IC advanced packaging


<b>Current Status & Next Steps</b>

The FNO forward model and CFD data pipeline are complete. 

Current work is focused on:

Gradually extending the extrapolation test range to stress-test FNO generalization as operating conditions move farther from the training distribution
Incorporating PDE residual losses (PINO approach) to improve physical consistency in pressure and velocity predictions under extrapolation
Training and integrating the diffusion model for the full inverse design pipeline
