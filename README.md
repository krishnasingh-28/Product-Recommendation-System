# Product Recommendation System

## Project Overview

This project builds a product recommendation system using a dataset of Amazon product reviews and ratings. It explores two different collaborative filtering techniques to provide recommendations:

1.  **User-Based Collaborative Filtering** using K-Means clustering to group similar users.
2.  **Item-Based Collaborative Filtering** using K-Nearest Neighbors to find similar products.

## Dependencies

This project utilizes the following Python libraries:

* **pandas & numpy:** For data manipulation and numerical operations.
* **matplotlib & seaborn:** For data visualization.
* **scikit-learn:** For implementing K-Means clustering, standardization, and K-Nearest Neighbors.
* **scipy:** For creating a sparse matrix to handle the large dataset efficiently.
* **surprise:** A library for building and analyzing recommender systems.

## Dataset

The project uses the `amazon.csv` dataset, which contains detailed information about products, including:

* `product_id`, `product_name`, `category`
* `discounted_price`, `actual_price`, `rating`, `rating_count`
* `about_product`, `user_id`, `user_name`
* `review_title`, `review_content`

## Methodology

### 1. Data Preprocessing & Cleaning

* **Handling Missing Values:** Rows with any missing data (specifically in `rating_count`) were dropped to ensure data quality.
* **Data Type Conversion:** Columns like `discounted_price`, `actual_price`, `rating`, and `rating_count` were cleaned by removing currency symbols (₹) and commas, and then converted from string to numeric types for analysis. The `discount_percentage` column was also converted to a numeric type.
* **Feature Engineering:** The `category` column, which contained multiple levels separated by `|`, was split into main category and sub-category columns to allow for more granular analysis.

### 2. User-Based Recommendation (K-Means Clustering)

This approach groups users with similar rating patterns into clusters.

* **User-Item Matrix:** A pivot table was created with `product_name` as the index, `user_id` as columns, and `rating` as the values. This matrix represents user ratings for different products.
* **Standardization:** The user features (ratings) were standardized using `StandardScaler` to ensure that each feature contributes equally to the clustering process.
* **Clustering:** The K-Means algorithm was applied to group users into 5 distinct clusters based on their standardized rating behavior.
* **Recommendation:** A function was created to recommend products to a specific user. It identifies the user's cluster and suggests the top-rated items among all users within that same cluster.

### 3. Item-Based Recommendation (K-Nearest Neighbors)

This method recommends items that are similar to a product the user likes.

* **Sparse Matrix:** The user-item matrix was converted into a CSR (Compressed Sparse Row) matrix from `scipy.sparse`. This is highly efficient for datasets where most users have not rated most items (i.e., the matrix is sparse).
* **Model Training:** A `NearestNeighbors` model from scikit-learn was trained on the sparse matrix using a 'brute' force algorithm to calculate distances.
* **Recommendation Function:** A function, `recommend_item`, was defined to find the 6 nearest neighbors (most similar products) to a given product name based on the user rating patterns.

## Example Recommendation

Using the item-based approach, here are the recommendations for the "Havells Ventil Air DX 200mm Exhaust Fan (White)":

1.  Havells Ventil Air DX 200mm Exhaust Fan (White)
2.  Khaitan ORFin Fan heater for Home and kitchen-K0 2215
3.  Personal Size Blender, Portable Blender, Battery Powered USB Blender...
4.  Green Tales Heat Seal Mini Food Sealer-Impulse Machine...
5.  SHREENOVA ID116 Plus Bluetooth Fitness Smart Watch...
6.  MR. BRAND Portable USB Juicer Electric USB Juice Maker...

## Conclusion

This project successfully implements two distinct collaborative filtering models for product recommendation. The user-based model leverages K-Means to find users with similar tastes, while the item-based model uses K-Nearest Neighbors to identify similar products, providing a robust foundation for a personalized recommendation engine.