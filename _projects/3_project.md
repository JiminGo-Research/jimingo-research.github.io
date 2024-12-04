---
layout: page
title: 2024 Spring Research & Engagement Symposium
description: Multi label node classification for predicting brands in tweets
img: assets/img/springsymposium_framework.png
importance: 3
category: Research Project
---
I participated in the 2024 Spring Research & Engagement Symposium hosted by Weber State University. This research is a follow-up study on the limitations experienced during participation in the <a href="https://jimingo-research.github.io/projects/4_project/"> Game Day Analytics Challenge.</a>

<b>Problem description : </b>

One of our method's limitation of the Game Day Analytics Challenge was that <b>we were unable to use data where the brand's name was not explicitly mentioned in the tweet.</b> For example, if a tweet did not mention the brand's name but referred to a celebrity, song, object, or other elements featured in the advertisement, it should have been considered as related to the brand. Therefore, we aim to develop a <b>model using deep learning techniques and graph-based machine learning to classify posts as related to a brand, even when the brand name is not mentioned.</b>

<b>Proposed method : </b>

The figure below illustrates the framework of our research. Each node represents a tweet, and the edges are constructed based on mentions and retweets between nodes, forming the graph structure. Next, to better capture the text features, the input first goes through a DNN layer. Then, it passes through a graph convolution layer and a loss function to produce the final output. The output is designed as a multi-label classification.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/springsymposium_framework.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Proposed model architecture
</div>

<b>Experiments : </b>
To evaluate the effectiveness of adding a DNN layer to existing graph neural networks, we <b>compared the performance of three existing models with and without the DNN layer.</b> 

Additionally, we conducted two experiments to <b>investigate the impact of retweets as a node feature</b>, given their importance in Twitter data.
1. We only considered information of hashtag, author and text of tweet as node features.
2. We included the text of retweeted tweets into node features with hashtag, author and text of tweet.

<b> Results & Conclusion : </b>

The left figures shows the results when retweets were not used as a node feature, and right shows results with them. In both experiments, <b>adding a DNN layer improved the model's micro-ROC values.</b> Moreover, <b>using retweets as a node feature resulted in overall better model performance compared to when they were not used.</b>

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/springsymposium_result1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/springsymposium_result2.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Comparison of model perfomance on multi-label nodes
</div>

We believe our proposed model can help companies analyze social networks for their marketing strategies by leverating social media posts that were previously excluded from analysis or required manual classification.

If you would like to want more information about this project, please check our <a href="/assets/pdf/2024 Spring Symposium.pdf">presentation material</a>.
