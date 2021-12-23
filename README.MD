Project_PHG.Rmd:
The first half of this file is executed first to create the "msbb_phg_tocluster.txt" file.
It essentially first merges the metadata and biospecimen data together and prepares the raw count data by reducing it to expressive genes.
After Project_Clustering.Rmd has used the output of this file to create a clustering, we again import the clustering into Project_PHG.Rmd. 
Using this clustering we perform a differential gene expression analysis comparing the control group to each of the clusters.
We use this to calculate the qvalue to only keep the significant genes for the subsequent machine learning.

Project_Cluster.Rmd:
This file performs a naive clustering using the multiClust library. We have played around with the different paramters of the cluster_analysis method and observed that the SD_Rank probe ranking combined with a hierarchical clustering works best.
Using any other clustering algorithm such as K-Means results in a very unbalanced clustering assignment. The ouput of this file is a table which assigns every sample a cluster number between 1 and 3.

Project_Clustering_WGCNA.Rmd:
We have tried to implement the WGCNA algorithm for clustering. We were successfull in clustering the genes into three clusters. Nevertheless, we were unable to translate this clustering into a clustering of the samples, which is why we used the naive clustering method.
For implementing the WGCNA clustering method we have followed this guide: https://wikis.utexas.edu/display/bioiteam/Clustering+using+WGCNA.

Project_ML.ipynb:
We first load and prepare all the data into python. While we only use the phg data, we have also loaded and processed the data of all available MSBB and ROSMAP data sources for potential analysis.
Preparing the data means specifically merging the metadata and biospecimen file and dividing them into the different brain areas of the MSBB cohort.
We then import the clustering obtained by the Project_Cluster.Rmd file which is used as the label for the machine learning.
The qvalue file is imported to only use the significant genes of each cluster. For each cluster we only take the genes which have significance of <0.05.
As some clusters have much more significant genes than the others, we have reduced the significance value for cluster 2 and 3 so we obtain a similar amount of genes throughout the clusters.
While the authors have used a random forest ML model, we wanted to train different models to obtain the best model. After performing similar steps as in the lecture, we could observe that
CatBoost_BAG_L1 was the best performant model. Seperating the gene expression data randomly into 20% training data and 80% training data has only resulted in an accuracy of 50%.
Not using the significant genes but choosing the genes through mutual_info_classif has returned better results, but we believe if the clustering algorithm was improved this methodology would be better.
The clustering algorithm we currently use is unfortunately unbalanced and return a poor cluster #1.
