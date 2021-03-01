# Hotel Hybrid Recommender System

<img src="https://github.com/mzcode98/hybrid-recommender-system/blob/main/images/Screen%20Shot%202021-02-28%20at%204.30.46%20PM.png" alt="alt text" width="600" height="400">

##### Matthew Zhang
### Overview:
Expedia is looking for a new and improved way of making hotel recommendations for their customers aside from traditional methods collaborative filtering and content based methods. After many years of making recommendations, many observed shortcomings of these methods become present. I hope to incorporate a new method for making better recommendations while addressing these shortcomings.

##### Abstract:
The purpose of this project is to develop a Hybrid Recommender system for better hotel recommendations. This hybrid model is an aggregate of multiple machine learning algorithms; where recommendations are made based on Content-Based and Collaborative Filtering. 
While manipulating libraries such as Sci-kit Learn, Surprise and LightFM, my model considers user and item features to predict for new users. The system additionally implements centroid based clustering and classification techniques to aggregate a successful hybrid.
Shortcomings to solve: scalability of data, sparsity within the utility matrix and the cold start problem.

## Goals
#### 1. Scalability of data
#### 2. Resolve the issue of high sparsity within the utility matrix for improved recommendation accuracy.
#### 3. Create a Hybrid Model that provives a viable solution to the cold start problem that standard collaborative filtering faces.

### Data and EDA
The dataset was taken from a Kaggle Competition where contestants would predict the correct hotel cluster. I have taken the dataset and changed the problem for my own research purposes.

Aside from standard cleaning processses, much of the cleaning emphasized feature engineering and manipulating the data so that I could feed it into my algorithms in the correct format. For example, both Surprise and LightFM require data to be restructured in a specific format of user-item interactions. 

Regarding feature engineering, I created proxy ratings based on whether a user clicked or booked a certain hotel page where the ratings were 1 or 5 respectively, and 0 otherwise. 
As for EDA, I was able to look at the distribution of my columns and get a quality understanding of the data I was working with.

#### Clustering
In terms of scalability; the original dataset included around 37M rows of data where I managed to clean and reduce down to about 3M rows. However, this was still too complex for matrix factorization and computation.
That being said, I decided to cluster my user_ids into user clusters to represent users for my recommender algorithm; where user clusters are users and pre-defined hotel clusters and items for the utility matrix. 
Expedia has their own algorithms for creating the hotel clusters based on user ratings and prices. 

While each user is put into 1 of 1,000 clusters, the ratings of users for each hotel are averaged within each cluster to get some representation of ratings. Here is what the clustered utility matrix looks like: 

<img src="https://github.com/mzcode98/hybrid-recommender-system/blob/main/images/Screen%20Shot%202021-02-28%20at%205.08.54%20PM.png" alt="alt text" width="350" height="200">

#### Ontology Model: Decision Tree Classifier
In order to solve the cold start problem, an ontology method was introduced. What this means is that, in theory, user profile (age, gender, occupation) represents user behavior. For my system, with new user profile information, I can predict the user cluster they will be in and create existing recommendations based on that.

So, I implemented a Decision Tree Classifier to learn from previous user profile and now classify new user profiles into their respective clusters. Keep in mind this is the solution to the cold start problem without using LightFM, so aggregating this with the SVD algorithm could work. 

## Predictive Modeling
As mentioned before, the modeling process is an aggregate of multiple algorithms, hence system. For simplicity sake, I will give a general overview of the steps I took when going through the modeling process.

I first ran multiple "pure collaborative filtering" models as baselines, but also to see how they perform on previous user similarity. I could potentially implement this into my system just for making recommendations for old users. To my "surprise" the Surprise SVD library performed tremendously with an RMSE of 0.45 and MAE of 0.38. This means that the model predicts ratings of users with an error of 0.45. So I decided to implement this into my system for returning/old users.

However, the collaborative filtering LightFM model performed the exact opposite with an AUC score of 0.36. This is where the Hybrid Model comes in and I added user-item features into the model where LightFM learns some embeddings from these features to represent the user and items. This is why it is a hybrid model incorporating both CF and Content-Based Filtering. So, my tuned Hybrid LightFM model was able to produce an AUC of 0.65
<img src="https://github.com/mzcode98/hybrid-recommender-system/blob/main/images/Screen%20Shot%202021-02-28%20at%207.59.55%20PM.png" alt="alt text" width="350" height="50">

On a side note, I have an the LightFM (LightFM does not suffer to the cold start problem) method for predicting upon new users where you input user features and the prediction numbers are returned for all 100 hotel clusters. Here is what that looks likes: 

<img src="https://github.com/mzcode98/hybrid-recommender-system/blob/main/images/Screen%20Shot%202021-02-28%20at%208.06.10%20PM.png" alt="alt text" width="400" height="300">

## Next Steps
#### Future Work:
- Consider Hierarchical Clustering where each point is a cluster to capture uniqueness of users as opposed to arbitrary cluster points.
- PCA of additional dataset with arbitrary numbers representing reviews.
- Combine user preference with hotel destination input. Individual user may not go to a high rating hotel within a cluster, which produces a great error if recommending the top hotel by purely clustered utility matrix.
- Hybrid Model through Neural Networks where layers are represented by CF and Content-Based methods

##### Conclusions: 
- I have an aggregate of many algorithms that make up my recommender system. 
- User preference and Item features are vital in recommender systems.
- Hybrid model successfully compensates the shortcomings of individual CF and Content
- Hybrid model produces an AUC score of 0.65


### The recommendations I have for better recommendations are: To incorporate Hybrid Modeling to reduce typical shortcomings of just looking at content-based or collaborative filtering. Also, develop a system to address these issues effectively. 
