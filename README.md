# Recommender System project
## Movie Pairing Recommender System

### Datasets and data processing

The two datasets were used to train this system: 
- the MovieLens dataset was used to get movie ratings per users and some information on movies (genres)
- the IMDb dataset was used to complete the features per movie (with releaseYear, duration, average rating, other genres, ...)

Only movies were selected and the two datasets were merged, keeping only common movies from both sources. Then the user-item rating matrix was cleaned so that each user has at least 10 ratings and each movie has at least one rating.

The final movie feature matrix contains 2419 movies and 30 features each. 
The features are:
- genres (one hot encoded)
- isAdult
- releaseYear
- duration
- avgRating (from IMDb)
- number ratings (from IMDb)

The user-item rating matrix contains ratings from 6036 users. For a total of 703112 ratings.

### Method

Collaborative filtering was used for this recommender system.

The matrix `W` (user-feature matrix) and the vector `b` (bias per user) were learned by the algorithm, as the matrix X (containing the features for each movie) is already known. The `cofi cost function` was adapted for this case.

To compute the predictions for a couple, we take the average of the predictions per movie for both of the users. Then we recommend the movie with the highest predicted rating.

### Train and test split

To create a training and a test set, some couple of users are selected such that they have more than a certain number of movie ratings in common. Thus, we can mask some of the ratings and evaluate our predictions on these masked ratings.

The number of couples selected is 100.

Each couple has more than 200 movie ratings in common and 100 ratings are randomly masked for each of them.

### System evaluation

Multiple metrics were used to estimate how well the system predicts movies.

- the training loss was used to tune the hyperparameters (such as the learning rate or the lambda parameter)
- the MSE was used to evaluate the model predictions on the test set
- the accuracy was used to evaluate in terms of the recommended movie (i.e. if, amongst the masked ratings, the recommended movie corresponds to (one of)* the favorite movie of the couple)

* because the maximum rating of a couple can be obtained by multiple movies in the test set

### Conclusion

Collaborative filtering was used to estimate how a user would like a movie, based on his previous ratings, the features of each movie and the ratings of all the other users in the dataset. The learnt matrix `W` corresponds to how much a movie feature is important for the user to like the movie, and the bias `b` can be seen as the 'a priori' for a user when seeing a movie in general.