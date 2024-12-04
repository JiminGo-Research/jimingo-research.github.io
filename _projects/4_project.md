---
layout: page
title: Information Systems Game Day Analytics Competition
description: The most effecitve brand of Super Bowl LVIII on various perspectives
img: assets/img/gdac_logo.png
importance: 3
category: Additional
---

I participated in the <b>Game Day Analytics Challenge</b> hosted by the David Eccles School of Business at the University of Utah. In this competition, we created an <a href="/assets/pdf/GDAC_Infographic.pdf" class="infographic">infographic</a>, <a href="/assets/pdf/GDAC_White Paper.pdf">white paper</a>, and <a href="/assets/pdf/GDAC_Presentation.pdf">presentation</a>. Our team secured <b>2nd place in the undergraduate division</b>.

<b>Our goal : </b> Which brand delivered the most effective Super Bowl advertisement?

<b>Brief summarization of our strategy :</b> We first thought about <b>"What makes an ad successful?"</b> and conducted various analyses, including the number of tweets per brand over time, cost per tweet by brand, and sentiment analysis. <b>However, each analysis raised questions for us, and we coulnd't definitively say tat any one brand performed consistently well across all criteria.</b>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/gdac_1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Results of tweets per barand over time
</div>

<b>Multi-Criteria Decision Making : </b> Therefore, we decided to evaluate the effectiveness of the advertisements by <b>considering all the factors comprehensively and chose to use a MCDM model</b>. We used a model that combines <b>AHP</b> and <b>TOPSIS</b>. The weights were determined through the AHP model, while the TOPSIS model was used to evaluate how close the alternatives were to the given criteria in order to identify the most ideal alternative. 

As shown in the structure of the AHP model at the bottom left, we used the number of tweets, cost per tweet, positive tweet ratio, and negative tweet ratio as the criteria. The image on the right shows the brand rankings when more weight is given to each criterion. For example, in the case of Universal, when more weight is given to the number of tweets, it ranks 5th, but when more weight is given to the positive tweet ratio, it drops to 63rd. This analysis suggests that Universal did not receive many positive tweets.

<div class="row justify-content-sm-center">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/gdac_2.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/gdac_3.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Structure of AHP and TOPSIS results
</div>

<b>Who is the real winner?</b> Upon seeing the experimental results, where the NFL ranked first in all categories, we began to wonder <b>if it was simply because the NFL hosted the game, leading to a higher number of mentions</b>. This curiosity led us to <b>social network analysis</b>. We difined nodes as each user and brand, and we determined that an edge exists when a user mentions a brand name in a tweet. We also examined how the network evolved during the initial game time, half game time, and entire game time. What was surprising was that, <b>after the game ended, contrary to the results from the MCDM, Verizon had the largest node size</b>.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/gdac_4.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Visualization of the social network analysis for the entire game time.
</div>

<b>Conclusion : </b> Rather than asserting that one method is right over the other, we believe that both MCDM and social network analysis are valuable approaches that companies can consider when evaluating the effectiveness of their advertising.

This competition took place just two months after I arrived in the United States, and I felt a lot of pressure knowing that we had to complete data analysis, create three materials, and present them professionally in English within the given timeframe. Despite that, I am incredibly proud of the great results that our team and I achieved!

If you would like to learn more about this competition, please refer to this <a href="https://eccles.utah.edu/programs/undergraduate/game-day-ad-analytics/" target="_blank">link.</a> 
