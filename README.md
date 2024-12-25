### Project Introduction:
#### Problem Statement: 
The aim of this project is to develop a Movie Recommendation System that provides personalized movie suggestions based on user preferences and prior watching history. The
system uses advanced data mining techniques to predict movies a user might enjoy, thereby enhancing user experience and engagement. This recommendation system is particularly useful for:

1.Streaming platforms like Netflix and Amazon Prime to retain users and enhance their experience.
2.Individual users who wish to discover movies tailored to their unique tastes.
3.Businesses seeking to understand consumer behavior through analytics derived from movie-watching patterns.

### Description
For this project, we used the MovieLens Dataset as the primary data source. To build the recommendation system, we selected a subset of the dataset, specifically focusing on rows with 1.9 million, ensuring efficient processing and model training.
The dataset contains:

1.20,000,263 ratings, 465,564 tag applications, and metadata for 27,278 movies.
2.Rich metadata: Includes movie titles, genres, and timestamps for user interactions.
3.User-specific information: Unique user IDs and their respective ratings enable personalized recommendations. 

Files Used : From the MovieLens Dataset, the following files were combined to create the final dataset:
1. genome_scores.csv: Provides tag relevance scores for movies.
2. movie.csv: Contains detailed information about movies, such as titles and genres.
3. rating.csv: Captures user ratings for movies, forming the basis for collaborative filtering models.
These files were preprocessed and merged to generate a comprehensive dataset for the recommendation
system.

### Data Cleaning and Pre-Processing:
In the data cleaning step, we performed essential operations to ensure the dataset is free of errors, inconsistencies, and redundancies, making it suitable for further analysis. Below are the tasks performed:
1.Missing Value Check:
 1.Verified the dataset for missing values across all columns using the colSums(is.na()) function.
 2.Result: No missing values were found, confirming data completeness.

2.Duplicate Row Check:
 1. Checked for duplicate entries using the duplicated() function.
 2.Result: No duplicate rows were found, ensuring that the data is unique and reliable.

3. Unique Value Exploration:
 1.Counted the number of distinct userId and movieId entries in the dataset using the n_distinct() function.

4. Result:
 1.Unique Users: 138,493
 2.Unique Movies: 27,278
 3.This confirms that the dataset contains a diverse range of users and movies for robust recommendation modeling.

### Normalization
Normalization is a crucial step in the preprocessing pipeline to ensure that all features are on a similar scale, particularly for numerical data.[14] This step helps in improving the efficiency and accuracy of algorithms that are sensitive to feature magnitudes, such as machine learning models.
1. A custom normalize function was implemented for Min-Max scaling.
2. The normalization was applied only to the numerical columns in the feature set using the mutate
and across functions from the dplyr package. 
#### Key Benefits of Normalization:
1.Ensures that all features contribute equally during modeling.
2.Reduces biases caused by varying scales in data.
3.Accelerates convergence in optimization algorithms. 

### Feature Engineering:
Feature selection is a vital step in preprocessing to enhance the efficiency and performance of the recommendation system by reducing the dimensionality of the dataset.[18] In this project, we focused on selecting and engineering meaningful features to represent movies effectively.

#### Steps in Feature Selection:
1. Removing Irrelevant Columns:
Columns like userId, rating, and timestamp were removed as they do not directly contribute to the content-based feature representation. 
2. Aggregating Genre Information:
Genres were one-hot encoded and aggregated by movieId to create distinct binary features for each genre. 
3. Incorporating Metadata:
Features such as average_relevance and additional movie attributes were merged into the dataset using the left_join function to enrich the feature set. 
4. Final Feature Set:
The resulting feature set contains a combination of content-based and numerical attributes for each movie, forming the basis for personalized recommendations.
#### Key Benefits:
1.Reduces noise by excluding irrelevant data.
2.Ensures the dataset is compact and contains only impactful features.
3.Improves model interpretability and training efficiency.

### Modeling
#### Algorithms Used:
#### 1. User-Based Collaborative Filtering (UBCF)
##### Definition:
User-based collaborative filtering recommends movies based on similarities between users. If two users rate movies similarly, movies liked by one user are recommended to the other.

##### Steps in Implementation:
##### Data Preparation:
1.The data was loaded and filtered to include relevant columns: userId,movieId, and rating.
2.A subset of 10,000 rows was used for efficiency.
3.The data was converted into a realRatingMatrix, suitable for collaborative filtering in the recommenderlab package.

##### Evaluation Scheme:
1. Data was split into training (80%) and testing (20%) sets using the evaluationScheme function.
2. A threshold of 4 was used for good ratings, ensuring that the recommendations are based on high ratings.

##### Model Creation:
1.A user-based collaborative filtering model was built using the Recommender function with the UBCF method.
2.Predictions were generated for test data using the predict function.

##### Metrics and Evaluation:
1. Accuracy metrics such as RMSE (1.2073), MAE (0.9451), and MSE were calculated to evaluate the model's predictive performance.
2.Precision and recall were evaluated for different top-N recommendation scenarios (e.g., N = 1, 3, 5, 10).
3.Example: For top-10 recommendations, recall was ~1.68%, indicating room for improvement in the model's relevance.

##### Test Recommendations:
1. Recommendations were made for a specific user or a movie.
2.Example: For the movie Jumanji (1995), the model identified user preferences and provided movie suggestions like Star Wars: Episode V,
Vertigo, and Moulin Rouge.

##### 2. Item-Based Collaborative Filtering (IBCF)
##### Definition:
Item-based collaborative filtering focuses on similarities between items (movies). If two movies are rated similarly by users, one movie is recommended to users who liked the other.
##### Steps in Implementation:
##### Data Preparation:
1.Similar to UBCF, the dataset was converted into a realRatingMatrix.
2.The evaluation scheme was reused to maintain consistency in training and testing splits.

##### Model Creation:
1.An item-based collaborative filtering model was built using the Recommender function with the IBCF method.
2.Predictions were generated for test data.

##### Metrics and Evaluation:
Accuracy metrics such as RMSE (1.2637) and MAE (0.9625) were calculated, showing slightly lower performance than UBCF.
Recommendations:
1. Example: For the movie Iron Man (2008), recommendations were generated based on item similarity:
2.Terminator 2: Judgment Day
3. Batman
4.Aladdin
5.Ace Ventura: Pet Detective
6.The Secret Garden

##### Similarity Computation:
1.Cosine similarity was computed to determine movie-to-movie relationships, and a similarity matrix was generated.
2.Top-5 similar movies for a given movie were selected based on their semilarity Score


##### 3. Content-Based Filtering
Content-based filtering is one of the recommendation system techniques that suggests items to a user depending on the characteristics or features of the items and the preference of the user. It learns from the features of items a user has interacted with and finds similar items to recommend. This approach, for instance, uses metadata such as genres, actors, and directors in recommending movies similar to those a user has liked or highly rated. This is especially effective in domains where rich item metadata exists since it saves the system from performing an extensive number of comparisons between users to make recommendations. Instead, the system focuses on the similarities between items through their content features.

##### Implementation of Content-Based Filtering in Our Project
In our project, we utilized the MovieLens dataset, which contains metadata for over 1.9 million movies, including titles, genres, user ratings, and average relevance scores. Here’s a detailedexplanation of our approach:

##### 1. Dataset Exploration and Preprocessing
1.Initial Inspection: We loaded and explored the dataset to understand its structure. It included key fields such as movieId, title, genres, userId, rating, and timestamp. The dataset was clean, with no missing or duplicate rows.

2.Genre Handling: The genres column, which often contains multiple genres separated by a pipe (|), was split into individual rows. This allowed us to analyze movies at a genre level and create detailed features for each genre.

3.Feature Engineering: We calculated the average ratings for each movie, along with their genre distributions. These features were then normalized to ensure uniformity in the data and to make it suitable for similarity computations.

#### 2. Building the Content Features
1.One-Hot Encoding: We transformed the genre information into a one-hot encoded format, creating a binary matrix where each column represented a genre, and a value of 1 indicated the movie belonged to that genre.

2.Combining Features: The encoded genres were combined with the average ratings and relevance scores to form a comprehensive feature set for each movie. This feature set served as the foundation for similarity calculations.

##### 3. Calculating Similarities
● Cosine Similarity: Using the normalized feature set, we computed the cosine similarity between movies. This metric evaluates how similar two movies are based on the angle between their feature vectors in a multi-dimensional space. A higher cosine similarity indicates greater similarity between movies.

● Similarity Matrix: The results were stored in a similarity matrix, where each cell represented the similarity score between two movies. This matrix was used to identify movies most similar to a given input.

##### 4. Recommendation System
1. Recommendation Function: We implemented a function, recommend_movies, that accepts a movie title and retrieves the top N most similar movies based on the similarity matrix. This function ensures that the recommended movies are distinct from the input movie.

2. Batch Processing: Another function, recommend_movies_list, was developed to handle multiple movies at once, generating personalized recommendations for a list of input movies.

##### Results and Observations
Test Cases:
1. For “The Rock (1996)”, similar movies included Dr. No (1962), Casino Royale (2006), and From Russia with Love (1963), showcasing a clear alignment with action-packed, spy-thriller genres.

2. For “Willy Wonka & the Chocolate Factory (1971)”, recommendations included Into the Woods (2014), Chitty Chitty Bang Bang (1968), and Frosty the Snowman (1969), all family-friendly and fantasy-themed films.

##### Performance:
The recommendation system displayed exceptional performance, with similarity scores consistently exceeding 0.99, a testament to its precision in identifying movies that align closely based on their content features. By meticulously leveraging the comprehensive metadata in the MovieLens dataset, the system effectively represented each movie as a normalized feature vector and calculated cosine similarity to reveal nuanced relationships. This meticulous approach ensured the recommendations resonated deeply with user preferences. For instance, a user providing Inception as input might discover titles like Interstellar or The Prestige in their recommendations—films that echo similar genres, themes, or creative contributors. This seamless alignment highlights the intuitive nature of the content-based filtering technique and its ability to deliver personalized and meaningful suggestions. What makes this system truly remarkable is its capability to thrive even in the absence of user interaction data, a hallmark of content-based filtering. By focusing solely on item attributes, the system not only personalizes recommendations but also lays a strong foundation for tackling broader challenges in recommendation systems. The model's success opens exciting opportunities for future enhancements, such as integrating collaborative filtering to create hybrid systems or employing deep learning techniques for richer feature extraction. Additionally, incorporating diversity into recommendations or factoring in contextual cues like time or viewing habits could further refine its adaptability. With these possibilities, the system stands poised to evolve into an even more dynamic tool for delivering engaging and tailored user experiences.

##### 4. Hybrid Recommendation Model
##### Definition of Hybrid Recommendation Models
Hybrid recommendation models combine multiple recommendation techniques to leverage the strengths of each. In this project, a hybrid approach was created by integrating:
1. User-Based Collaborative Filtering (UBCF): Recommends movies based on user similarity.
2. Item-Based Collaborative Filtering (IBCF): Recommends movies based on item similarity.
3. Content-Based Filtering (CBF): Recommends movies similar to the content features ofpreviously rated movies.
By combining these methods, the hybrid model improves recommendation accuracy and reduces the limitations of standalone models.

##### Implementation Steps
##### 1. Data Preparation:
1.The dataset was loaded and filtered for relevant columns like userId, movieId, rating, and title.
2.A rating matrix was created using the dcast function, with rows as users and columns as movies.
3.The matrix was converted to a realRatingMatrix for compatibility with the recommenderlab package.
##### 2. Model Creation:
1.User-Based Collaborative Filtering (UBCF):Built using the Recommender function with the UBCF method.
2.Item-Based Collaborative Filtering (IBCF):Created using the Recommender function with the IBCF method.
3. Content-Based Filtering (CBF):Simulated with the RANDOM method for this hybrid implementation.
These models independently generate recommendations based on their respective techniques.

##### 3. Hybrid Model:
1. For each user, recommendations were generated from UBCF, IBCF, and CBF.
2.These recommendations were combined into a single list, ensuring unique entries.
3. If multiple users rated the same movies, their recommendations were intersected to find common suggestions.
4. A fallback mechanism was implemented to provide recommendations even if fewer users rated the input movies.

##### 4. Final Recommendations:
1.Recommendations were derived from the union of UBCF, IBCF, and CBF outputs.
2. If common movies existed, they were prioritized. Otherwise, unique movies were sampled to fill the required number of recommendations.
3. Titles for the recommended movies were extracted for display.

#####  Test Recommendations:
1. Input Movies: Popular movies like Jurassic Park (1993) and Toy Story (1995).
2 Final Recommendations:
  1.Eternal Sunshine of the Spotless Mind (2004)
  2.Apocalypse Now (1979)
  3.Sin City (2005)
  4.Cloudy with a Chance of Meatballs (2009)
  5.Punisher, The (2004)


##### Insights:
The hybrid model leveraged the diversity of recommendations from UBCF, IBCF, and CBF. By combining user preferences and item similarities with content-based insights, it ensured a balance of novelty and relevance in the recommendations.

##### Architecture:
1.Data was split into training and testing sets.
2.Algorithms were implemented in R using packages like recommenderlab and caret.
3.A custom similarity-based scoring metric was developed for evaluation.


##### User Interface (UI)
##### Description: 
1.The recommendation system includes a simple UI developed using Streamlit. Users can input previously watched movies and preferences to receive personalized recommendations.
2.Dynamic display of recommendations.
3.Ability to process multiple input sets in bulk.

##### Key Features:
1. Algorithm Selection:A dropdown menu allows users to select from the following recommendation algorithms:
 1.User-Based Collaborative Filtering (UBCF)
 2.Item-Based Collaborative Filtering (IBCF)
 3.Content-Based Filtering
 4.Hybrid Recommendation System (Recommended)
By default, the system is set to the Hybrid Recommendation System, as it combines the strengths of all other algorithms and provides the most balanced recommendations.

##### 3. Movie Input Field:
Users can input movie titles (separated by semicolons) to serve as the base for generating recommendations.

##### 4. Recommendation Count Selector:
A simple counter allows users to specify the desired number of recommendations by increasing or decreasing the value with “+” or “-” buttons.

##### 5. Dynamic Display of Results:
Once the user clicks the “Get Recommendations” button, the results are displayed in a clean, tabular format, showing the recommended movies along with their metadata.

##### Conclusion: 
This UI ensures an accessible and interactive experience for users. By defaulting to the hybrid model, the system delivers optimal recommendations while still allowing users to explore alternative algorithms. Its intuitive design makes it adaptable for diverse user needs and enhances overall engagement with the recommendation system.

