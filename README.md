# NBA Predict Best Lineup Combinations
This project is based on the article published in the MIT SLOAN Sports Conference in 2020 named: <a href="https://global-uploads.webflow.com/5f1af76ed86d6771ad48324b/5f6a65517f9440891b8e35d0_Kalman_NBA_Line_up_Analysis.pdf"> NBA	Lineup	Analysis on	Clustered	Player Tendencies: A	new	approach	to	the	positions	of	basketball	& modeling	lineup	efficiency	of	soft	lineup	aggregates.</a> 
The approach is making a prediction of the best possible combinations for a NBA 5-man lineup based on a previous probabilistic clustering using Gaussian Mixture Models.

# First Step
The first step is choosing the data to work with. I choose to use data of all NBA players from seasons 2008-2009 to 2018-2019, selecting 23 characteristics that shows the impact of a player during a match. These have to be in percentage or per 100 posessions to avoid great numbers from players that play too much minutes in comparison to the ones that not. The data was obtained scraping <a href="www.basketball-reference.com">Basketball reference web page</a> and the code is in SCRAP_DATA.ipynb.

# Second
The data obtained is to perform the clustering. First I tried K - MEANS, evaluating the performance of it with the <a href="https://scikit-learn.org/stable/auto_examples/cluster/plot_kmeans_silhouette_analysis.html#sphx-glr-auto-examples-cluster-plot-kmeans-silhouette-analysis-py"> silhouette score </a>. This metric helps to select the correct amount of clusters based on how well each sample is assigned to one of them. 
Due to the bad performance of K - MEANS, it was necessary to try a probabilistic clustering method. 
