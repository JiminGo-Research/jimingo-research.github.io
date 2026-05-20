---
layout: page
title: ElectroTwin
description: A Digital Twin-Driven Framework for Designing and Scheduling of Electroplating Facilities
img: 
importance: 1
category: Undergraduate
related_publications: True
---

<b>Overview</b>
Electroplating facilities require simultaneous decisions on both <b>facility design</b> (how many hoists) and <b>operation scheduling</b> (how each hoist moves) — a combined problem known as the <b>Cyclic Hoist Designing and Scheduling Problem (CHDSP)</b>. These decisions are currently delegated to outsourcing companies due to the complexity of the formulation, which is costly, time-consuming (often 24+ hours per case), and produces opaque results that operators cannot easily interpret or validate.

This project proposes <b>ElectroTwin</b>, a digital twin-driven decision-support framework that integrates an automated constraint-based search algorithm with visual analytics, enabling non-expert facility operators to make informed, data-driven investment and scheduling decisions.

This work is published as: <a href="https://doi.org/10.1007/s40684-026-00867-9">ElectroTwin: A Digital Twin-Driven Framework for Designing and Scheduling of Electroplating Facilities</a>, D. Lee, T. Noh, J. Go, I. Park, K. Kim
International Journal of Precision Engineering and Manufacturing-Green Technology, 2026, DOI: 10.1007/s40684-026-00867-9

This project extends the algorithmic foundation built in our earlier work — <a href="https://jimingo-research.github.io/projects/6_project/">the OOPP Engine and CTS algorithm</a> — into a full digital twin system with GUI-based visual analytics.

<b>Problem Definition</b>

Given an electroplating facility with $$M$$ stages and a target cycle time $$\tau$$, the objective is to determine:

- The <b>minimum number of hoists</b> $$Q$$ required
- The <b>feasible movement schedule</b> for each hoist

such that operational costs $$O_C$$ are minimized while maintaining part quality (no soaking time violations). The net objective $$F = O_C - O_P$$ balances operational cost against production profit, where $$O_P$$ depends on unit price, production volume, and operation time.
Key constraints include: no hoist collisions (minimum one-tank gap between hoists at all times), soaking times must stay within process bounds, and one part per hoist at a time.

<div class="row justify-content-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid
       path="assets/img/electrotwin_problem.png"
       title="Multi-hoist electroplating scheduling problem"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Fig. 1 from the paper — Gantt-style diagrams (a) and (b) showing why a single hoist fails to meet soaking constraints and how two hoists satisfy them, with handover stage marked.
</div>

<b>Proposed Framework</b>

<b>System Overview</b>

ElectroTwin is structured as a digital twin that mirrors the physical electroplating facility. Given process data (tank types, soaking times, hoist specs) and a target cycle time, the framework automatically generates feasible schedules and delivers interpretable visual analytics.

<div class="row justify-content-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid
       path="assets/img/electrotwin_outline.png"
       title="ElectroTwin system outline"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Outline of ElectroTwin
</div>

<b>C-Search (Constraints-Based Search Algorithm)</b>
The core scheduling engine is <b>C-Search</b>, which determines the appropriate number of hoists and their feasible movements by iteratively exploring the solution space under operational constraints:

- Initialize with a single hoist starting from the loading station
- For each part, check whether all stages in the current hoist's coverage zone satisfy soaking time and cycle time $$\tau$$
- If constraints are violated, introduce a new hoist at the handover stage $$e_q$$ and fix the previous hoist's schedule
- Repeat until all $$M$$ stages are covered

C-Search scales with complexity $$O(N^2M)$$ — manageable for industrial-scale problems without combinatorial explosion.

<b>Visual Analytics GUI</b>

ElectroTwin is implemented as a Python desktop application (Tkinter + Matplotlib) with five integrated functions:

- (a)Data load — constructs the digital twin from facility configuration files
- (b)Cycle time input & execution — triggers C-Search for a given $$\tau$$
- (c)Output — displays the number of hoists and handover points
- (d)Operation visualization — animates hoist movements and tank utilization in real time (with fast-forward option)
- (e) Visual analytics — Gantt chart, bottleneck analysis, and cost-profit analysis

<div class="row">
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid
       path="assets/img/electrotwin_gui.png"
       title="ElectroTwin GUI configuration"
       class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.liquid
       path="assets/img/electrotwin_operation.png"
       title="Operation visualization function"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
   Left — Fig. 4 from the paper showing the full GUI with labeled panels (a)–(e): settings, cycle time input, outputs, start/stop, and analytics tabs. Right — Fig. 5 from the paper showing the animated operation visualization with hoists and colored workpieces moving across the tank line.
</div>

<b></b>Experimental Results</b>
Three real-world industrial cases from South Korean electroplating facilities were tested (16–24 stages, up to 28 tanks, cycle times of 193–600 seconds).

<div class="row justify-content-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid
       path="assets/img/electrotwin_experiment_result.png"
       title="ElectroTwin Experiment Result"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Scheduling Performance vs. Conventional Method
</div>

C-Search reduced computation time from 24 hours to under 2 minutes, and found solutions with fewer hoists in Cases 2 and 3, directly lowering capital and maintenance costs.
