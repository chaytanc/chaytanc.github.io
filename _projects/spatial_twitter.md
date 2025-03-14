---
nav_exclude: true
layout: page
title: Spatial Twitter
category: "AI & Research"
description: "Exploring spatial statistical models applied to embedded tweets to predict pro-Brexit sentiment."
permalink: /projects/spatialtwitter
---
**Thinking Spatially About Language:** 

**An Exploration with 2022 Brexit Tweets**

Chaytan Inman

STAT 554

14 March 2025

********

**Research Question**

Can spatial statistics of embedded two dimensional tweets help model the prevalence of pro-Brexit tweets?

We start with a dataset of all pro or against Brexit tweets in the timeframe January - March 2022. The tweets were coded as pro or against based on the key words used to scrape the tweets (e.g. those containing “pro Brexit” were considered pro). Using this data as a ground truth, we will attempt to predict the proportion of pro-Brexit tweets in regions created by sampling tweets, embedding them within two dimensions, and clustering to create artificial shape files and regions. Then we will perform spatial analysis on the sampled tweets to try to predict the true proportions of pro-Brexit tweets in the created regions. The “Hit Sentence” column of this dataset was used for embeddings in this analysis, which, it should be noted, is not the full tweet but rather the first few sentences of each tweet. 

**Methodology**

1. Sample data

   1. Simple random sample with an _average_ of 1300 tweets per cluster (Appendix Table 1.0)

   2. Fit PCA, K-means, save the latter

   3. Create shape file from Voronoi boundaries

   4. Close Voronoi boundaries that were on the edges by adding points outside the frame then deleting polygons that were not in the original Voronoi boundaries

   5. Create sampled dataframe and geo-dataframe

2. Calculate true prevalences

   1. Embed every single tweet 

      1. Uses GPT-2 to produce embeddings 

   2. Cluster  every single tweet using sample’s saved K-means 

   3. Count number of tweets per region

   4. Construct dataframe that has embedding, tweet, pro/against column, 2d coordinates

3. Test spatial methodologies using sample

   1. Fit a spatial BYM binomial model

   2. Fit nonspatial IID binomial model

4. Perform Monte Carlo resampling procedure 20 times

**20 Cluster Results**

**Fig. 1.0**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdPUGGIwXBH7u7WdBmE-Xq2NMHfJqNpYNJn8i0NpTpawg2mN4vfS3Gah4StExD5_kzLvVQbeF2cMDMZ-NM-5lQ1wSDVmeCfhZKh3-DHUj-dzV82YCz93IrfvrO7RFv1jyuvVSklrg?key=bAy81A_OxX-Cie6B0l0K31gF)

We will begin to investigate the results of fitting our spatial model to the embedded and clustered Brexit tweets by looking at the predictions of the last sample. First, we can see that the true prevalences had one particularly strong anti-Brexit cluster at close to 0.25% prevalence of pro-Brexit tweets, and a fair gradient of 40-50% prevalences. This is a trend that continues as we resample later. We can see that both the IID and the spatial model failed to capture this well, and that both had strong errors in particular regions. Both models imposed a more gradual gradient than really existed between regions. We can also see the strong similarities between the spatial and IID models in general, with even the model differences graph not reaching the 1e-2 scale. 

**Fig 1.1**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdPT1LvvDLUIVEGTBrK4ktdB5aXNdYJDG5UHT93boxWW-b9oV27yfA-3sX7KT_vn3G_ih_3HOcFrDKSJUEjZVrqyRzRuoex911hdkcTBPLQfCevWE7Ykl4C9yOYe41M7zfnE4TtCg?key=bAy81A_OxX-Cie6B0l0K31gF)

Next we can see a timelapse of all 20 samples. From this we can see the general trend that both models have very similar absolute error patterns, though not exactly the same. We can also see clearly the trend that although the geometry of the cluster regions changes with each sample, within the true prevalences there is always one to two very strongly anti-Brexit clusters and usually a stronger pro-Brexit region centered near the coordinates (-10, 15). We see that both models struggle with this upper-middle left region in general.

**Fig. 1.2**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcJqerRB9hTIoSyKjARJKeP28Qyovoyfmru64H-dtGVmJ4hUQ0A8kQDPEbJwywY3FUFvyEoNnNMFt-fwbkioQdQvCmpvK_k31-Iy7sBKkSR2TMPHTOFBo8Coa3ZY9xCRdJtn_DjlA?key=bAy81A_OxX-Cie6B0l0K31gF)****

Here we see that indeed the BYM spatial model reduced the variance on average. This is unsurprising, as including a spatial component increased the bias (as we will see) and decreased the variance. We also see that both models have the highest variance in the upper region of the graph. This would be interesting to investigate with a model that can decode the cluster centroids to interpret what the average response is like in these particular regions. We also see that some of the regions with high variance tend to have low error (upper right regions) while others have higher than average error (upper middle region).

**Fig 1.3**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXerwbeIph10Qc2HOdJUps9KS78rhgw3iLh2eQJnoN-WIaNXY0LkmNUgcVR7VLG-nNny0umlSnBRa_TSLHxffOazcz6dciK5OIXJoJhDYNNxNTZHt3tsVof7v5ikFsKYNFzAY-4Gdg?key=bAy81A_OxX-Cie6B0l0K31gF)

Here we clearly see that the models follow the same trends very closely, generally regardless of the random sample.

**Fig 1.4**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcpkhS6QI_2j21ecNycbAPCBAuJdO68tQNBNjejHSxi7f17g4WvMqzloNHXDnfTvtPTRemSEFScaNMO4Tf2U0OUheZtyYBsOj7e1Xr_gvPisorzKB_UhzUB1dL7cNwnKZGpOIBAxw?key=bAy81A_OxX-Cie6B0l0K31gF)

In this graph we can see that both models tend to underestimate the true prevalence of pro-Brexit tweets rather than overestimate this value. This could be because clusters representing pro-Brexit tweets are sparser and more widely distributed than the one observed strongly anti-Brexit cluster seen in the true prevalences graph above (Fig. 1.1).

**Fig 1.5**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcWFCYxfm3eNRysoL8J2IxvLL_9PEEsIrnPhp-Ueb8Ny6YBuFJqLXqVawnIVi_GDCC1xUW7Oao6fEp2UAEYCri3tsfV9sCACxT4VF5-oIGTxfinOProbzZ4cmLPFMKZXw3mHf3WXw?key=bAy81A_OxX-Cie6B0l0K31gF)

**Fig 1.6**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe0Yc-c-WvFsgTXlVdOMLXVrGzOZxkyIXuAWLR9Lsvo_AkJsulZMxY3mubitBwZoseF1R4E61p08MnWWbxCZ5m0WZdwoWiumJ-YwiMRtD0DGb-SMtEgsuZX55zfsxilRAmXW3dMQw?key=bAy81A_OxX-Cie6B0l0K31gF)

Finally, in the last two graphs we can confirm the visual results see above. We see that the average mean bias for both models across the 20 samples is -0.029 respectively. These are roughly approximated by normal distributions centered at these means. We also confirm that both models indeed follow very similar amounts of error, both with a mean of 0.085. Finally we can see that the spatial model trades a lower variance for a lower accuracy, with its smaller ranges of confidence intervals covering a proportionally smaller area of regions’ true prevalences: the BYM model has an average coverage probability of 27.131% of regions compared to the IID model’s 28.821%, with mean variances of 2.253e-4 and 2.495e-4 respectively.

**10 Clusters Results**

**Fig 2.0**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfrBClTV7Khn1Hkt3YsZ440in1SbDM2uFY51tA27-96PYD-vHe8UE-_SKJRfl3uoyHriaLma93pGlx5NVZrVRQHOUmuGHoOlHBFOR62Pn3UT0j4LMJfAme8HnYqLNzjGN9frGfX?key=bAy81A_OxX-Cie6B0l0K31gF)

Here we show a few abbreviated results from experimenting with 10 cluster regions rather than 20. These regions were also resampled 20 times with the same hyperparameters as the 20 region results.

**Fig 2.1**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdwXDzuREkHLKvI1kePwlEWeyQshoIloB2L1HC6sCLJRSzLVvkBrOuHVEsAwY9ujMNwRFryW0IApr5qIulmxFaNRhUH7IgCoHeWSBk4r9r-ERrS5MqIT7VW9iZzY3mTz4OaoI5r?key=bAy81A_OxX-Cie6B0l0K31gF)

The summary statistics for the 10 region samples show on average a higher coverage probability than the 20 sample experiment: both models achieve 36.591% average coverage of the true prevalence being within the 95% confidence interval for each region. This is compared to the 20 sample coverage probability of 27.131% and 28.821% for the BYM and IID models respectively. Interestingly, with only 10 regions, the BYM model still reduces variance but is no longer lacking in coverage probability. The mean absolute error for the models also decreased with 10 regions as opposed to 20, from 0.085 to about 0.062. Finally, the mean bias was also lower in the 10 region experiment compared to the 20 region experiment, however in both cases the bias of the error was significantly skewed negative.

**Discussion and Considerations**

From the results we can conclude that spatially modeling the embeddings of these tweets did not offer greater insight into the nature of the tweets or the prevalence of pro/anti Brexit tweets in each region. We can conclude this from the higher coverage probability of the IID model, although both mean coverage probabilities were within the 95% confidence interval of each other. Regardless, the very similar performance of the models with respect to average absolute error over 20 trials of resampling confirms that the spatial model did not offer significant improvements in accuracy. This is surprising, as there does appear to be a spatial component to the distribution of true prevalences. This may be caused by extremely sparse sampling of particular clusters and oversampling of the “urban” or more common clusters. Therefore, a promising future direction is performing weighted analysis or fitting a Fay-Herriot model to the data may show stronger performance. Furthermore, including covariates in the analysis may strengthen results. Possible covariates from this particular dataset included the sentiment score of each tweet, the country of origin, the language, the time/date, and more. Finally, to more intuitively understand the cluster mapping of the data, a model trained to decode embeddings back into natural language would provide significant insight into the strengths and weaknesses of the predictions made by these models.

To conduct this investigation, I used the hyperparameters listed in Appendix Fig 1.0, and all of which are deserving of further investigation, though particularly N\_CLUSTERS which significantly changes the possibilities of spatial correlations by changing the shape of clustering and the meaning. Preliminary investigations into altering this parameter show that in this case lowering the number of clusters seems to increase model performance, perhaps by increasing the number of samples per cluster, but this should be further studied. Varying the average number of samples per cluster may also be more systematically investigated, to find the minimal number of samples to achieve a certain desired coverage probability threshold.

When we resample in this investigation, we recalculate the K-means clustering for the new sampling data because it seemed that the sampling was suffering from oversampling “urban” or high density clusters, so I thought different samples may dramatically change the boundaries. In the future I would test clustering all samples with the original K-means clustering and Voronoi shape file, since it seems that the boundaries and often the prevalences within clusters themselves changed minimally. However, this test would be best paired with a way to investigate whether the data clustered with a singular K-means in clusters which accurately represent that tweet’s meaning, such as the finetuned embedding decoding mechanism discussed above.

**Conclusions**

Overall, this experiment was a fun exploration of how using embeddings from large language models can enable us to think about natural language in a spatial context. Spatial statistics may be able to build models that predict the sentiment of social media and other natural language domains. This exploratory project shows that there are promising directions to take spatial modeling and that there are many configurations of hyperparameters and modeling techniques to explore in order to improve this approach. 

********

**Appendix**

GitHub Repository (missing various datafiles because too large): <https://github.com/chaytanc/spatial_twitter>

Table 1.0

|                                                            |       |
| ---------------------------------------------------------- | ----- |
| Hyperparameter                                             | Value |
| MAX\_LENGTH (longest amount of tokens per tweet processed) | 30    |
| EMBEDDING\_DIM (unchangeable feature of gpt2)              | 768   |
| N\_CLUSTERS                                                | 20    |
| SAMPLES\_PER\_CLUSTER                                      | 1300  |
| RESAMPLE\_N                                                | 20    |

**References**

Lynch, G. (2022). Twitter datasets: Brexit related tweets from pro and anti Brexit accounts January - March 2022 (Brexit leaning based on their Twitter bios). Harvard Dataverse. <https://doi.org/10.7910/DVN/FXEFZT>

Wakefield, J. (2025, January 22). 2025 554 SUMMER Package R Notes. STAT 554 Canvas.

Departments of Biostatistics and Statistics, University of Washington

Wakefield, J., & Gao, P. (2025, January 22). 2025 Small Area Estimation Notes. STAT 554 Canvas. Departments of Statistics and Biostatistics, University of Washington
