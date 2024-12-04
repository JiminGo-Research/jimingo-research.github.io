---
layout: page
title: Entrepreneurship Club
description: a project with a background image
img: assets/img/hogu_1.png
importance: 3
category: Additional
---
<b> Purpose of this study & Goal : </b>

In 2023, there was significant interest in South Korea in finding lower prices through <b>international direct purchases</b> (buying goods directly from overseas retailers) and <b>reverse direct purchases</b> (buying Korean goods to overseas retailers). I noticed that many people were <b>struggling to determine which overseas websites offered the best deals on specific products.</b> To address this, I decided to develop a service that could provide a solution. Together with two other students, I <b>created a website that compares prices of identical products across various platforms</b>, helping users identify the most cost-effective options. 

<b> Method : </b>

To achieve our goal, we needed to address two major challenges. The first was <b>determining how to crawl websites</b> effectively, as the <b>HTML structures of various international sites differ significantly</b>. The second was <b>identifying the same product being sold under different names across countries and sites</b>, ensuring accurate recognition of identical items.

We realized that the first issue could only be solved by developing an automated crawler, which itself became another research topic. To address this, we decided to focus on websites of companies with branches in multiple countries, like Amazon Korea and Amazon USA, as they share similar structures, making it easier to implement the crawling solution.

Instead, we dieced to concentrate on the second problem and developed a solution through three key steps. 

<b>Step 1: Comparing image simmiarities</b> We considered this crucial because while product names might differ, similar images would suggest identical products. 

We used Python's OpenCV library for this task, and the following images show the results. The left image compares <b>two identical products</b>, yielding an accuracy of <b>0.87%</b>. The middle image compares <b>two vastly different products</b>, with an accuracy of <b>0.21%</b>. The right image compares <b>two similar yet different products</b>, achieving an accuracy of <b>0.17%</b>. As seen in the right image, <b>the model successfully detects even subtle differences</b>, proving its effectiveness in identifying potentially confusing cases.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/hogu_3.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/hogu_4.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/hogu_5.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Results of OpenCV
</div>

<b>Step 2: Title Similarity Using Cosine Similarity</b> As shown in the image below, even though the product names differ—<b>"Laneige Lip Sleeping Mask Berry" in Korea</b> and </b>"Korean Moisturizing Strawberry Lip Mask" in Portugal</b>—they are the same item. <b>Without image similarity, title comparison alone might have led us to incorrectly classify these as different products.</b>

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/hogu_6.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/hogu_7.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<bStep 3: Price comparison</b> We observed that <b>products with different quantities or sizes were sometimes incorrectly identified as identical</b>. Since image and title similarities couldn't fully solve this, we introduced a third step—comparing product prices. For instance, if a product is sold for $10 in Korea and $50 in Portugal, we would consider them separate items, even if their image and title similarities were high. However, if the price difference is reasonable (e.g., $50 in Portugal versus $35 in Korea), we would consider them the same product.

<b> Results : </b>
Based on this method, I developed a beta version of the website using HTML, CSS, Bootstrap, React, JavaScript, and a database.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/hogu_2.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/hogu_1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<b> Reflection : </b>
Through this project, I gained a comprehensive understanding of full-stack development and had a valuable opportunity to experience web development. The project was supported by the Startup Support Center at Incheon National University, where I learned how to consider various scenarios that may arise when customers use the service. Additionally, we are still researching different methods to address the first issue we couldn't fully tackle. If you are interested in this problem or the work we have done, feel free to reach out.
