---
layout: page
title: Developing Simulation-based Integrated Operation Scheduling System
description:
img: "assets/img/electroplating_3.png"
importance: 4
category: Research Project
---

This project is my first lead project in Industrial Intelligence Laboratory at Incheon National University. Based on this work, our team published <a href="/assets/pdf/patent_certification.pdf">patent</a> and awarded an 19th Industrial Engineering Project Competition hosted by Korean Institute of Industrial Engineers(KIIE).

<b>Purpose of this study : </b>

This study was initiated to address the challenges faced by a <a href="http://www.ik-tech.co.kr/eng/" target="_blank">company</a> in South Korea that manufactures and sells plating line. When a request is made to create a plating line capable of producing a specific type of plating within given seconds, <b>the company currently tests its feasibility based solely on the expertise and experience of professionals, without any system in place.</b> The company mentioned that manual scheduling takes anywhere from 3 days to over a week, and <b>they were looking for ways to reduce this time</b>. Additionally, due to the manual nature of the scheduling process, <b>mistakes have occured, such as failing to identify opportunites to reduce the number of hoists.</b> 

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/electroplating_1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/electroplating_2.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left image shows the actual plating line manufactured by the company. Right displays the scheduling result visualization used by the company.
</div>

<b>Our goal : </b> 

To address their difficulties, we believed the following three things were necessary:

<b>1. A program that automatically designs plating lines</b> Since the company faced an urgent need to reduce the time required for plating line design, it was clear that an automated system was necessary. We viewed this problem as a type of flow shop scheduling and decided to develop a rule-based algorithm to quickly find a solution.

<b>2. A program for ananlyzing scheduling results</b> Due to reliance on experts, it was difficult to explain to clients why certain designs were necessary to meet given requirement. As shown in the figure above, the scheduling result chart used by the company was not easy to understand at a glance. Therefore, we decided to develop a bottleneck analysis tool and a more user-friendly Gantt chart.

<b>3. Graphic User Interface (GUI)</b> To help the worker understand how the scheduling results actually functioned and enable manufacturing on the factory, we also decided to develop a GUI.

<b>Problem definition : </b>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/electroplating_3.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Example of electroplating line considered by this research
</div>

Here are some <b>assumptions</b> and <b>constraints</b> necessary to understand the problem:

1. Each workpiece goes through all the stages present in the plating line sequentially, and after a process is completed, it moves to the next tank.
2. Each tank can process only one workpiece at a time, and if the deeping time falls outside the range betweeen the minimum and maximum deeping time, the item is considered defective.
3. Each hoist can transport only one workpiece at a time, and it is assumed that there will be no stops during the transport process.
4. Hoists must be operated with at least one tank gap between tank.
5. hoists move at high, medium, or low speed depending on the travel distance, and it takes 10 seconds for ascending and descending
6. If a stage has a significantly longer immersion time compared to others, parallel tanks may be used.
7. Hoist overlapping is allowed when the turnover point is determined in parallel tanks.
8. If the immersion time in a tank is less than 5 seconds, it cannot be a turnover point.
9. Up to three dummy tanks (empty tanks without chemicals, not used in the actual process but employed to avoid collisions between hoists) can be added.

<b>Our method : </b>

Unlike the company, which only considered the feasibility of a single solution, we aim to present <b>a variety of feasible solutions that could emerge from different combinations of factors</b> such as the number of hoists, number of tanks, number of parallel tanks, number of dummy tanks, cycle time, and the deeping time of each tank. 

We named this method <b>Optimized Operation Planning for Plating (OOPP) Engine.</b> This consist of 4 steps: Understanding the situation -> Diagnotic analysis -> Operational strategy improvement -> Deriving optimal operational strategy. By continuously repeating this process until no further improvements can be made, we search for all possible feasible solutions.

<b>Step 1: Understanding the situation.</b> This step involves <b>inputting the basic information for scheduling into the simulator.</b> It includes details, including the process the electroplated items must undergo, minimum and maximum deeping time for each process, hoist's low, medium, and high speed times, and much more. 

<b>Step 2: Diagnotic analysis.</b> In this step, the <b>minimum cycle time is calculated</b> based on the provided process information, and <b>bottleneck processes are analyzed.</b> Cycle time refers to the time it takes for a workpiece to be processed from input to completion. The bottleneck analysis includes assessing how much work is present in each tank during the process and calculating the efficiency of each tank when the plating line is operated at the minimum cycle time.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/electroplating_4.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/electroplating_5.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Example of our bottleneck analysis: In the bar graph on the left, the process with the red-bordered frame indicates the bottleneck.
</div>

<b>Step 3: Operational strategy improvement.</b> <b>Parallel tanks are added to the bottleneck process</b> from Step 2, and their impact is assessed using the same performance metrics.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/electroplating_6.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/electroplating_7.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The results show when one tank was added to the processes identified as bottlenecks in Step 2, revealing that the bottleneck shifted to a different process.
</div>

<b>Step 4: Deriving optimal strategy.</b>This step is the core stage of the OOPP Engine, where the <b>turnover points and hoist paths are determined</b> to complete the scheduling. We named this process Concurrent Task Search (CTS), and it is divided into four sub-stages: Determining the optimal turn over point -> Deciding the hoist path -> Adding dummy tanks -> Find feasible solution

<b>1) Determining the optimal turn over point.</b> Determining the turnover point has a significant impact on the complexity of the hoist path decision and the feasibility of the final solution. Through case studies, we found that <b>when the turnover point is determined at parallel tanks, issues arise</b> where the gap between two hoists becomes too narrow, or the hoists cross each other, leading to collisions. Additionally, we discovered that <b>problems can occur even when there is no limit on the number of tanks a single hoist can handle</b>, and we have enabled users to input this into the simulator.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/electroplating_8.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The left image is a Gantt chart illustrating a situation where a single hoist is responsible for too many tanks, causing it to miss the given cycle time. The red line appears when the hoist moves from tank 9 to tank 2, indicating a time shortage. To make this feasible, the right image shows the necessary adjustments.
</div>

<b>2) Deciding the hoist path & Adding dummy tanks.</b> This part is the most challenging aspect of our algorithm. Once the turnover point is determined, the hoist paths are decided by considering the movements of neighboring hoists within the defined turnover point. The most difficult part of determining the hoist paths was ensuring that there is a gap of at least one tank between the hoists.

We categorized potential collision scenarios into <b>upper collision, lower collision, and upper & lower collision.</b>

(1) Upper collision: This occurs when a collision happens with the hoist above. To avoid the collision, the hoist needs to move down.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/electroplating_9.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

(2) Lower collision: This occurs when a collision happens with the hoist below. In this case, the hoist must move upwards.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/electroplating_10.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

(3) Upper & lower collision: This occurs when collisions happen with both the hoists above and below. In this case, the collisions must be compared, and the hoist should move up or down appropriately to avoid the collisions.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/electroplating_11.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

If avoiding collisions is not possible without adding dummy tanks, we add the dummy tanks and then re-run the collision avoidance logic.

<b>3) Find feasible solution.</b> This process is repeated until all collisions are resolved, leading to the final solution.

<b>Results : </b>

The image below shows the result of the CTS logic we developed. The turnover points are distinguished by green lines, and the movements of the hoists over time are represented by dashed and solid lines. The dashed lines indicate the movement of the hoists while carrying workpieces, while the solid lines represent the movement without workpieces. Each workpiece is represented by a bar of the same color. In cases where a dummy tank is added, the label "Dummy" is written on the y-axis. Additionally, the entire process from warm-up to cool-down is shown, allowing workers to effectively understand the overall process.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/electroplating_12.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Additionally, the <b>simulator</b> includes features such as <b>process operation visualization</b>, which shows how the various solutions discovered by the OOPP Engine are implemented, as well as <b>what-if-based cost-profit analysis.</b> By inputting estimated costs for designing the plating line, including hoists, tanks, chemicals, and production profits, users can analyze break-even points between solutions and make more informed decisions.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/electroplating_13.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/electroplating_14.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<b>Conclusion : </b>

Our developed simulator and algorithms are now actively used within the company for designing plating lines. This has eliminated the need to consult external vendors when assessing design suitability, resulting in significant time savings. After nearly a year of hard work, the project has been successfully completed, and it is truly rewarding to see the tangible benefits it brings to the company.

<b>Reflection : </b>
This project holds a special place for me personally because it marked several firsts. It was my first collaboration with a company, my first leading project in the lab, and it provided opportunities to present our research at conferences, file patents, and even win awards. There were countless development iterations before we finalized the OOPP Engine and CTS algorithm.

Previous research had mostly focused on reducing cycle time, so there were limited references, and developing an algorithm that could account for all real-world constraints was no easy task. Reflecting on the journey, I realize how deeply immersed I was in the research—consulting with other labs, seeking advice on the challenges I encountered, and dedicating hours to the mathematical formulations. Despite many setbacks and failures, the time I spent relentlessly asking, "How can we improve?" led to immense personal and professional growth.

I am currently designing experiments to tackle this project using reinforcement learning. If you are interested in the research we have conducted or the research I am planning to pursue, feel free to reach out with any questions.
