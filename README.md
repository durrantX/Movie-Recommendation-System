## Final Project Submission
## Movie recommendation System

*  Author: Deon Durrant

# Business and Data Understanding

The King Isle event coordinator is responsible for organizing and recommending movie screenings for the residents of the island. The coordinator's goal is to ensure that each resident is offered movies they are more inclined to watch and enjoy based on their previous movie preferences. King Isle’s movie recommendation system should be able to provide tailored movie suggestions for each resident, allowing them to have a more enjoyable viewing experience.

## Problem Statement
The event coordinator needs to provide top 5 personalized movie recommendations to each resident based on their previous movie ratings. By analyzing the historical ratings given by residents to other movies, the system should predict which movies each resident would likely enjoy in the future and prioritize the highest-rated ones.

## Data Understanding
 The dataset was retreived from  https://grouplens.org/datasets/movielens/latest/ . The ratings dataset has 100836 entries. All 100,836 movie entries have a rating, with maximum rating of 5 and minimum of 0.5. There are 610 user ids indicating that users are rating multiple movies.The average rating is 3.5.

# Summary 
* A movie recommendation system was developed to address the business problem using the MovieLens dataset.  
* Data exploration revealed a positive bias in ratings and data sparsity due to low user engagement.
* Model-based collaborative filtering approach using Sparks MLlib was employed.
* A baseline model was tested with explicit feedback, and a cold-start strategy defined to handle null predictions. 
* Hyperparameter tuning was conducted using Spark's machine learning pipeline, leveraging ParamGridBuilder and CrossValidator to optimize model performance. 
* Root Mean Square Error (RMSE) was the primary accuracy metric for model evaluation, with the final model achieving an RMSE of 0.8769 compared to the baseline of 0.8813.
* The recommendation system was developed and assessed using both RMSE and Precision@K,
 *  The proof of concept included recommendations to a distinct set of users and movies, demonstrating the system’s effectiveness. 
 * Personalized recommendations for individual user were also generated to gain deeper insights. 

# Model-based collaborative filtering 
 Utilize Sparks MLlib 

 # Evaluation Metrics
 * Root mean square error
 * Precision@K

# Exploratory Data Analysis

## Variables Distribution

## Explore the rating distribution


  **Rating Distribution**  
![image](https://github.com/user-attachments/assets/a8a20a4a-6560-4521-bb50-46e6d45d60f2)

Rating are either implicit or explicit. 
Explicit ratings/feedback occurs when users directly rates the item.
Explicit rating is on a scale 0-5.  

The data distributed indicates a positive ratings of movies  are more common. These higher ratings suggest that users are generally satisfied with the  movies. There may be a positive bias in the ratings. The data distibution and business task may influence the model's performance. 

**Ratings Percentage Distribution**  
![image](https://github.com/user-attachments/assets/4715a434-c75a-4f25-8c77-66c961e5c9db)
Percentage distribution of ratings skews toward higher ratings, which could be an indication of positive bias.
* Ratings 4.0 and above: The combined percentage for ratings 4.0, 4.5, and 5.0 is about 48.17%. 4.0 (26.60%),
* Ratingd 3.0 to 3.5:  Makes  up 32.91% of the ratings 
* Ratings below 3.0: Account for a much smaller portion of the ratings ( 18.91%). 

Implication of this postive bias will likely skew recommendations toward high-rated movies and may not truly reflect the users' diverse preferences.

To address the issue the following strategies can be used .
* Normalization of the ratings  to account for individual user bias toward higher ratings.
* Regularization  of the ALS model(matrix factorization) to reduce the impact of extreme ratings by preventing the model from overfitting to popular, positively rated items.

**Distribution of ratings across users** 
To understand the distribution of the number of ratings across many users the histogram will be used 
![image](https://github.com/user-attachments/assets/91cca7e0-f734-4989-aee2-cb0d960445ea)

* The majority of users fall into the first bin, suggesting that many users might have rated few movies. This suggest low engagement in terms of ratings. 
* Users  rating fewer items  leads to sparse datasets. 
* There are fewer users with high rating engagements which could have a significant impact on the recommendation model. 

 To address sparsity, matrix factorization techniques like Alternating Least Squares will be used. 

# Model selection

To address the business problem collaborative filtering CF approaches will be considered. 

## Collaborative Filtering  Approaches
* User-based filtering- recommends items to the target user based on preferences of other users.
* Item based filtering - recommends new items to the target user based on the user’s previous interactions with similar items
* Model based CF -Model-based approach creates a predictive machine learning model of the data. The present values in the utility matrix are used as the training data while predictions are made on the interactions that are yet to occur. 

Model based collaborative filtering will be used to address the business problem,  specifically  Sparks from MLlib. 

## Modeling
* Import the Alternating Least Squares(ALS) Method and apply it using default parameters
* Divide the data using random split into train_data 80%  and test_data 20%  
* Build the recommendation model using ALS  default params on the training data
* coldStartStrategy="drop"  to remove any rows in the DataFrame of predictions that contain NaN values.
* Make predictions with  baseline  model 

## Hyperparameter tuning
* Determine the best parameters for the business task by applying a range of parameters. 
* Employ ParamGridBuilder  with CrossValidator to optimize model performance
* Extract the best model from hypertuning exercise using ParamGridBuilder

Best model parameter 
*  Rank of 8 indicates a lower number of latent factors  capturing the user-item interactions and  reduced risk of overfitting(rank determines dimensions,  the matrices) 
* Max Iterations of 15 is the  the number of iterations the ALS algorithm  will run. More iterations allow the algorithm to reduce errors and converge to better solutions.
* Regularization Parameter of 0.2 applies a moderate penalty to user item latent factor values, preventing the model from overfitting while allowing it to capture patterns in the data

**Model Comparison**

| **Model**            | **RMSE**   |
|----------------------|------------|
| Baseline Model       | 0.8813     |
| Final Tuned Model    | 0.8753     |
* The final model performs better than the baseline, with a reduced RMSE. 
* Suggests that tuning  provides more accurate movie recommendations based on user ratings
* Implication- additional  tuning or additional techniques such hybrid models could yield greater performance enhancements.

# Develop Recommendations System

To address the business problem, the recommendation system was designed to:

* Generate the top 5 recommended movies for each user
* Generate the top 5 recommended users for each movie

 **Top 5 Recommended Movies for Each Users**
| userId | title                                                                                                                        | pred_rating |
|--------|------------------------------------------------------------------------------------------------------------------------------|-------------|
| 1      | Dragon Ball Z: The History of Trunks (Doragon bôru Z: Zetsubô e no hankô!! Nokosareta chô senshi - Gohan to Torankusu) (1993) | 5.0         |
| 1      | On the Beach (1959)                                                                                                          | 5.0         |
| 1      | Seve (2014)                                                                                                                  | 5.0         |
| 1      | Deathgasm (2015)                                                                                                             | 5.0         |
| 1      | Frozen River (2008)                                                                                                          | 5.0         |


# Evaluate Accuracy of Top 5 Movies Recommended for Each User

In addition to RMSE, the Precision@K evaluation metric will be used to assess the accuracy. This metric compares the relevance of the top 5 recommended movies to a set of relevant movies for each user (ground truth).


### Top 5 Movies Recommendation Evaluation

| Metric                               | Value  | Interpretation                                                                                 |
|--------------------------------------|--------|-----------------------------------------------------------------------------------------------|
| RMSE                                 | 0.4464 | Predicted ratings deviate by approximately 0.44 units, indicating strong prediction accuracy.  |
| Average Precision@K (4.5 relevance)  | 0.6614 | 66% of the top 5 recommended movies for each user are considered relevant based on a 4.5 threshold. |
| Average Precision@K (4.0 relevance)  | 0.8763 | 87% of the top 5 recommended movies for each user are considered relevant based on a 4.0 threshold. |



# Top 5 Users Recommendations for Each Movie

| userId | title                             | pred_rating |
|--------|-----------------------------------|-------------|
| 53     | Dracula: Dead and Loving It (1995)| 3.548811    |
| 558    | Dracula: Dead and Loving It (1995)| 3.3245075   |
| 327    | Dracula: Dead and Loving It (1995)| 3.2381494   |
| 371    | Dracula: Dead and Loving It (1995)| 3.0953596   |
| 276    | Dracula: Dead and Loving It (1995)| 3.0835876   |



**Model Predicted Ratings and Evaluation for Top 5 Users per Movie**

| Metric                           | Value  | Interpretation                                                                                 |
|-----------------------------------|--------|-----------------------------------------------------------------------------------------------|
| RMSE                             | 0.6255 | Predicted ratings deviate by approximately 0.63 units, indicating the model closely approximates user ratings. |
| Precision@K (4.5 relevance)       | 0.2830 | 28.3% of the top 5 recommended users for each movie are considered highly relevant based on a 4.5 threshold. |
| Precision@K (4.0 relevance)       | 0.5944 | 59.4% of the top 5 recommended users for each movie are considered relevant based on a 4.0 threshold. |


# Proof of concept 
To demonstrate that the recommendation system functions recommendations to a distinct set of users and movies, confirms the system’s effectiveness.

**Recommend 5 Movies to Distinct User in the Subset**

* Select a random distinct set of users (20)
* Recommend 5 movies to each user
* Calculate accuracy and Precicion@K
* 
| userId | title                                                                                           | pred_rating |
|--------|-------------------------------------------------------------------------------------------------|-------------|
| 540    | Dragon Ball Z: The History of Trunks
(Doragon bôru Z: Zetsubô e no hankô!! Nokosareta chô senshi - Gohan to Torankusu) (1993)                   | 5.0         |
| 540    | On the Beach (1959)                                                                             | 5.0         |
| 540    | Frozen River (2008)                                                                             | 5.0         |
| 540    | Visitor, The (2007)                                                                             | 5.0         |
| 540    | Deathgasm (2015)                                                                                | 5.0         |

**Note**: Only showing top 5 rows.



**Evaluation  of Recommend 5 movies to 20 distinct users**

The recommendation system was evaluated by recommending 5 movies to 20 distinct users. The results are summarized below:

| **Metric**                     | **Value**    | **Interpretation**                                                                 |
|---------------------------------|-------------|-------------------------------------------------------------------------------------|
| **Root Mean Square Error (RMSE)**| 0.2510     | Predicted ratings deviate from the actual ratings by a small margin (0.25 units).   |
| **Precision@K (4.5 relevance)** | 0.840       | 84% of the top 5 recommended movies are highly relevant based on a 4.5 threshold.   |
| **Precision@K (4.0 relevance)** | 0.960       | 96% of the recommended movies are considered relevant based on a 4.0 threshold.     |

### Summary

- **RMSE of 0.2510**: This suggests that the model is performing very well, with a small deviation from actual user ratings.
- **Precision@K (4.5 relevance)**: 84% of the recommendations are highly relevant to users when considering a strict threshold of 4.5.
- **Precision@K (4.0 relevance)**: A more lenient threshold (4.0) leads to 96% relevance, meaning that the recommendations are highly aligned with user preferences.

These metrics indicate that the recommendation system is highly effective at predicting and recommending relevant movies to users.


 # Generate  Users Recommendation for  distinct list of  movies

**Recommend  Distinct Movies to 5 Users in the Subset**

* Select a random distinct set of movies  (5)
* Recommend 5 users to each movies
* Calculate accuracy and Precicion@K
    
# Evaluation: Recommend 5 Distinct Movies to 5 Users
| userId | title                             | pred_rating |
|--------|-----------------------------------|-------------|
| 53     | Dracula: Dead and Loving It (1995)| 3.548811    |
| 558    | Dracula: Dead and Loving It (1995)| 3.3245075   |
| 327    | Dracula: Dead and Loving It (1995)| 3.2381494   |
| 371    | Dracula: Dead and Loving It (1995)| 3.0953596   |
| 276    | Dracula: Dead and Loving It (1995)| 3.0835876   |




The recommendation system was tested by recommending 5 distinct movies to 5 users. The results are summarized below:

| **Metric**                     | **Value** | **Interpretation**                                                                 |
|---------------------------------|----------|-------------------------------------------------------------------------------------|
| **Root Mean Square Error (RMSE)**| 0.5131  | The RMSE indicates moderate accuracy, with some deviation from actual ratings.       |
| **Precision@K (4.5 relevance)** | 0.36     | Only 36% of the top 5 recommended movies are considered highly relevant (4.5 threshold). |
| **Precision@K (4.0 relevance)** | 1.0      | All recommended movies (100%) are considered relevant with a more lenient 4.0 threshold. |

### Summary

- **RMSE of 0.5131**: The model shows moderate accuracy, with some deviation from the actual ratings.
- **Precision@K (4.5 relevance)**: Only 36% of the recommendations meet the stricter 4.5 relevance threshold, suggesting difficulty in achieving high relevance.
- **Precision@K (4.0 relevance)**: At a more lenient threshold of 4.0, 100% of the recommend


 # Demonstration of Personalized Recommendations
 
To offer insights into the model's performance, personalized recommendations will be generated for an individual user.
* Recommend 10 movies users 54
* Precicion@K-evaluates how many of the top  k (10) recommended items are actually relevant to user54
    
| userId | title                                                                                                                        | genres                     | pred_rating |
|--------|------------------------------------------------------------------------------------------------------------------------------|----------------------------|-------------|
| 54     | Dragon Ball Z: The History of Trunks (Doragon bôru Z: Zetsubô e no hankô!! Nokosareta chô senshi - Gohan to Torankusu) (1993) | Action, Adventure, Animation | 4.0924816  |
| 54     | On the Beach (1959)                                                                                                          | Drama                      | 4.0924816   |
| 54     | Seve (2014)                                                                                                                  | Documentary, Drama         | 4.00343     |
| 54     | Deathgasm (2015)                                                                                                             | Comedy, Horror             | 3.9777083   |
| 54     | Saving Face (2004)                                                                                                           | Comedy, Drama, Romance     | 3.9490638   |


     
**Precision@K for user54**

| Relevance Threshold | Precision@K | Interpretation                                                                                   |
|---------------------|-------------|--------------------------------------------------------------------------------------------------|
| 4.0                 | 0.4         | 40% of the top 10 recommended movies are considered relevant by the user.                        |
| 4.5                 | 0.0         | None of the top K recommended items were considered relevant under the stricter 4.5 threshold.    |

### Summary:
- The recommendation model suggests movies that user54 has not rated yet.
- With a relevance threshold of **4.0**, 40% of the top 10 recommended movies are considered relevant by the user.
- When the threshold is more conservative at **4.5**, none of the top K recommended movies were considered relevant.
- The model performs well with a less stringent relevance threshold (4.0), identifying movies that the user enjoys. However, at the 4.5 threshold, it struggles to recommend movies the user would "love" rather than just "like," indicating room for improvement in the model's accuracy.
    

# Conclusion 

This collaborative filtering system successfully addressed the business needs by offering relevant and high-quality recommendations.
* RMSE Performance range: 0.2510- 0.6255 indicating highly to moderately accurate predictions.
* Precision@K: 
   * The system performed well with a lenient threshold of 4.0, achieving up to 96% relevance in recommendations.
    * Struggled with stricter 4.5 threshold, indicating  room for improvement in identifying movies users would "love" versus those they  "like."

 * The recommendation system effectively balances prediction accuracy and relevance, with strong performance under more lenient thresholds. 
 

# Next Steps

* Refine Model Parameters and Improve Accuracy:
Conduct further hyperparameter tuning to enhance the model's accuracy and robustness. 

* Incorporate Hybrid Recommendations:
Incorporate movie metadata to generate more relevant recommendations. Combine content-based filtering  collaborative filtering approach to develop a hybrid recommendation system.

* Expand Evaluation Metrics:
 Utilize a mix of accuracy metrics(eg. Recall@K) and qualitative metrics (like Diversity, Coverage) to provide a comprehensive evaluation. 
    
* Recommendations Refinement:
Derive and incorporate implicit feedback  to better capture user preferences and improve recommendation quality for individual users.

For More Information:

See the full analysis in the Jupyter Notebook or review the presentation.

project_directory
 


