# NBA Predict Best Lineup Combinations
This project is based on the article published in the MIT SLOAN Sports Conference in 2020 named: <a href="https://global-uploads.webflow.com/5f1af76ed86d6771ad48324b/5f6a65517f9440891b8e35d0_Kalman_NBA_Line_up_Analysis.pdf"> NBA	Lineup	Analysis on	Clustered	Player Tendencies: A	new	approach	to	the	positions	of	basketball	& modeling	lineup	efficiency	of	soft	lineup	aggregates.</a> 
The approach is making a prediction of the best possible combinations for a NBA 5-man lineup based on a previous probabilistic clustering using Gaussian Mixture Models.

# First Step (Choosing Data)
The first step is choosing the data to work with. I choose to use data of all NBA players from seasons 2008-2009 to 2018-2019, selecting 23 characteristics that shows the impact of a player during a match. These have to be in percentage or per 100 posessions to avoid great numbers from players that play too much minutes in comparison to the ones that not. The data was obtained scraping <a href="www.basketball-reference.com">Basketball reference web page</a> and the code is in 
<a href="https://github.com/piperojas0618/NBApredict/blob/master/SCRAP_DATA.ipynb"> SCRAP_DATA.ipynb</a>. 
The final csv is called <a href = "https://github.com/piperojas0618/NBApredict/blob/master/Data/Data_2009_2019.csv"> Data_2009_2019 </a> in the Data folder. 

# Second (Cluster Models)
The data obtained is to perform the clustering. First I tried K - MEANS, evaluating the performance of it with the <a href="https://scikit-learn.org/stable/auto_examples/cluster/plot_kmeans_silhouette_analysis.html#sphx-glr-auto-examples-cluster-plot-kmeans-silhouette-analysis-py"> silhouette score </a>. This metric helps to select the correct amount of clusters based on how well each sample is assigned to one of them. 
Due to the bad performance of K - MEANS, it was necessary to try a probabilistic clustering method like GMM. It makes sense taking into account that today NBA players can play in various positions, and have various characteristics, so they aren't labeled in one specific group. 
For the implementation of GMM there are multiple parameters to take into account: the initialization parameters for each gaussian distribution and the amount of clusters,for example. So to evaluate multiple options I decided to make initializations on my own; varying the weights and the means with different number of clusters. I choose values evenly, or randomly. For choosing the correct amount of clusters the metric used was <a href="https://medium.com/@analyttica/what-is-bayesian-information-criterion-bic-b3396a894be6">BIC</a>, and not the "score" sklearn suggests; that is the average log-likelihood. After evaluating the multiple options, I chose the amount of clusters and parameters that had the lowest BIC (8 clusters) and train a model to obtained the probability of each player being in each of the 8 clusters. 

# Third  (Cluster Models Results)
Based on the characteristics that highlight each cluster, I named them. This table shows how I named them and based on what. 

![image](https://user-images.githubusercontent.com/54789284/109889222-f258cb00-7c52-11eb-8775-be08a0f3221d.png)

The boxplots created showing the highlighted characteristics are <a href="https://github.com/piperojas0618/NBApredict/blob/master/Cluster%20Models/Box%20Plot%20results.ipynb"> here. </a>

# Fourth (Data Management)
For making the regression models it was necessary to get the <a href="https://www.basketball-reference.com/teams/DAL/2011/lineups/">lineups data</a>. These had to be modified by adding the first name of each player, and then adding the team name like in the Data_2009_2019 csv for then related them. In the Data folder are all the csv's for the lineups from 2008-2009 to 2018-2019 seasons. Also it was important to adjust the net rating of each lineup due to the huge range between the best and the worst, so to avoid outliers and to have a better performance with the regression models it was necessary to apply an empirical bayes formula to adjust this. 
Finally, it's necessary to add the probability of the players to the lineup tables and get, at the end, a sum of probabilities that will be the columns, or the X values, to train the regression model. The final table or DataFrame it's called Lineups and Clusters in the regression model folder. 

# Fifth (Regression Models)
I tried 3 different algorithms; Random Forest Regressor, SVR, AdaBoost Regressor.
To find the best parameters for each algorithm I made "for" loops, then save all the data in a dataframe, then sort it based on the MSE between the train data and its prediction. Finally train the best model for each algorithm, plot the predicted data vs the real data and get the Mean Squared Error of each to use the best model to get the final results. Take into account that the final prediction is a Net Rating for each lineup.

# Sixth (Results)
To check the model in a completely new dataset, I create a permutation matrix in which it's included all possible permutations when the sum of probabilities, in the 8 columns that identify each cluster, it's equal to 5 with intervals of 0.25. 
'\n'
![image](https://user-images.githubusercontent.com/54789284/110065300-d893b280-7d3c-11eb-8bc8-339131244215.png)

The best Net Rating obtained was +16.7 with this type of players: 2 Dominant Scorers, 1.75 General Player, 0.25 of a 3-PT shooter and 1 Center. 
