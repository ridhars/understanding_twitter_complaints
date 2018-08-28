## Project 4 Summary - Understanding Twitter Complaints

[Project MVP](https://docs.google.com/document/d/1d28zVa5IF9P7JuBjVT-8mh-T9SEiWj5dP3cNe-z96iI/edit?usp=sharing)
[Project Presentation](https://docs.google.com/presentation/d/1XYLo4zUUSxfnyJlms7eEk5o-huaslnlcYy3wT80KrxM/edit?usp=sharing)

### Project Design

In today's era of highly active social media usage, many companies took initiative to open a Customer Support account within these social media to help customers easily reach & raise any encountered issues. However, information from these media are difficult to keep track of due to the unstructured nature of the data. Getting insight from this social media data and work on a solution which will help majority of customers would be a very valuable byproduct aside from providing accessible channel and solving customers' inquiry 1-by-1.

In this project, I aim to gain understanding of customers' common issues being raised in Twitter, one of the most popular social media, by carrying out *topic modelling* & subsequently *cluster* similar information (tweets).

### Tools & Data

Data for this project is readily available on [Kaggle's Customer Support Data](https://www.kaggle.com/thoughtvector/customer-support-on-twitter). A raw data of 304,575 tweets from @AmazonHelp were used for this project. Data were taken between 2015 - 2017, however majority (~90%) of the data were collected between Oct - Dec 2017. All processing & visualization were done in iPython Jupyter Notebook.

### Algorithms

#### 1. Preprocessing
Tweet data consists of conversation between customer who's raising an issue and customer support agent who's assisting to solve the issue. The conversation may take between 2 tweets until any number necessary to have the issue solved. As the objective is to understand the *problems* being raised, I isolated only the first, initial inquiry coming from customers and excluded the rest. Additionally, all non-english conversations were removed.

Tweets' conversation are also less formal and often include contractions, therefore expanding these contractions and adding non-formal words to stopwords (on top of use-case specific words) were necessary. @ handler removal, URL link removal, and stemming were also used.

Two vectorizers, CountVectorizer and TF-IDF were tested. The best result for this data was achieved using CountVectorizer with bigram included.

#### 2. Topic Modelling
The aim for the topic modelling is to extract high-level topics but with sufficient details to become an actionable insight. Three topic modelling techniques (LSA, NMF, & LDA) with different combinations of vectorizers were used in the experiment. On the final result, the modelling yielded 16 topics by using LDA. 

In general, these 16 topics are related to delivery timing, poor customer support performance, membership inquiry, gift & books inquiry, and a mix of smaller topics (e.g. refund, returns, etc.)

#### 3. Clustering
Inspected over tSNE plot, clustering within the 16 topic space were unsuccessful due to data points being all lumped into a single blob. However, with dimensions of 35 topic space, observation on tSNE plot appeared more promising; well separated, clustering data points can be seen to a certain degree.

However, as exact number of expected clusters are unknown, two clustering methods were considered. 
1. Kmeans: using inertia and silhouette coefficient plot to identify optimum cluster value and cluster accordingly.
2. DBSCAN: experimenting the hyperparameters to allow DBSCAN to automatically cluster similar data points.

On first method, optimum value were not identified on both plots, and experimenting with different cluster number directly on Kmeans made the cluster interpreting part difficult, as the clusters did not have a strong pattern topically.

On the second method, mixed results were achieved.
* DBSCAN identified 12 clusters, although only 1/4 of the data were clustered. The remaining 15k data points were left as noise.
* Interpreting the 12 clusters were also difficult (albeit better than Kmeans). Some have recognizeable patterns, others are random. Moreover, many of the tweets within each cluster don't consistently align with the main pattern topically.

### Improvements
One hypothesis regarding the mixed result of DBSCAN were due to the presence of common words among all clusters, thus making the DBSCAN difficult to partition one from another. What I would do differently is to spend more time on preprocessing, to experiment with removing more words which caused this 'noise' while maintaining the topic interpretable.

Once we have a well separated clusters, we could possibly do another round of topic modelling for each of this cluster, to gain more understanding for each cluster. This information could be leveraged further to label the cluster and turn this into supervised learning problem. This could in turn be used to monitor how the issues evolve with time.
