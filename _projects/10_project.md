---
layout: page
title: SHIELD-USA
description: Substrate-based Heterogeneous Integration Enabling Leadership Demonstration for the USA
img: "assets/img/SHIELD_thumbnail.png"
importance: 3
category: Graduate Research
---

<b>Overview</b>

<a href="https://microelectronics.asu.edu/shield-usa/">SHIELD-USA</a> is a $100 million CHIPS R&D program funded by the U.S. Department of Commerce / NIST and led by Arizona State University in collaboration with Deca Technologies, AMD, IBM, Synopsys, and other major semiconductor partners. The program advances domestic advanced packaging capabilities by developing leap-ahead molded-core organic substrate technologies targeting 300 mm wafer-level and 600 mm panel-level manufacturing.

As a member of the EDA team, I contribute to the design automation infrastructure that enables heterogeneous integration research across the consortium. This <a href="https://drive.google.com/file/d/1yXbdnmCC_0EWTl2QvNxx0MDKmAds5o5m/view?usp=drive_link">work</a> is published at GOMACTech 2026.

<b>My Contributions</b>

<b>1. Monitor IC Design — Full Physical Implementation</b>

I led the complete physical design flow for monitor circuits used in the SHIELD-USA test vehicles:

- Monitor Design: Designed all monitor, such as serpentine, comb, serpentine-comb, long-line monitor, and daisy chains.

- Floorplanning & Placement: Defined die area, I/O ring, and standard-cell placement under advanced packaging constraints using Synopsys 3DIC Compiler
  
- Routing: Completed global and detail routing with DRC-clean results, managing congestion across multi-die integration scenarios
  
- LVS / DRC Debug: Performed full physical verification using Synopsys IC Validator, resolving layout vs. schematic mismatches and design rule violations specific to advanced packaging design rules

- Netlist Export: Exported post-layout netlists in Verilog format for use in downstream verification flows

- Parasitic Extraction: Currently setting up Synopsys StarRC for post-layout RC extraction to feed into the STA pipeline (in progress)

- Scripting & Automation: Wrote Tcl and Python scripts to automatically generate test cases for runset logic verification, replacing tedious manual case creation

- Sign-off: Coordinated the full verification flow to achieve clean sign-off across LVS and DRC checks

<b>2. SHIELD-USA Specific PDK / ADK — Verification & Feedback</b>

The SHIELD-USA EDA team develops a custom PDK/ADK to capture the unique design rules of molded fan-out substrates. My role was on the verification side: running DRC/LVS checks against the kit rules, understanding how the rule files are structured, and identifying inconsistencies encountered during my physical design work. When something in the runset behaved unexpectedly, I documented and reported it directly to Synopsys for resolution.

<b>3. Static Timing Analysis (STA) Pipeline — In Progress</b>

I am currently building an automated STA pipeline to support timing closure for monitor circuits embedded in the SHIELD-USA test vehicles:

- Scripted end-to-end timing analysis using Synopsys PrimeTime
- Developing parasitic extraction hooks to incorporate package-level interconnect parasitics (RLC models from 3DIC Compiler) into cell-level timing analysis
- Automating report generation and slack visualization to accelerate design iteration across the team

<b>Tool & Technology Stack</b>

<b>Physical Implementation</b>:Synopsys 3DIC Compiler

<b>Physical Verification</b>: Synopsys IC Validator (LVS/DRC)

<b>Parasitic Extraction</b>: Synopsys StarRC

<b>Layout Editing</b>: KLayout

<b>Static Timing Analysis</b>: Synopsys PrimeTime

<b>Netlist</b>: Verilog

<b>Scripting & Automation</b>: Tcl, Python

<b>Context & Impact</b>

SHIELD-USA targets a critical national security gap: the U.S. currently depends heavily on overseas advanced packaging capacity. The EDA infrastructure I help develop directly enables the R&D-to-manufacturing transfer pipeline — ensuring that novel substrate technologies can be designed, verified, and qualified efficiently at ASU's MacroTechnology Works facility before transfer to domestic foundry partners.
