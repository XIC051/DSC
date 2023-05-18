# Introduction
# Cleaning and EDA
### Data Cleaning
1. We **left merged** the recipes and interactions datasets together and then **filled all ratings of 0 with np.nan**. 
- This is because NaN values are typically excluded from statistical calculations such as mean, median, and standard deviation. By converting 0 ratings to NaN, we can ensure that these calculations accurately represent the ratings provided by users, without distorting the statistical measures due to explicit low ratings.
2. We **dropped the duplicate id column**, which is `recipe_id`. 
3. We found the **averate rating per recipe** as a Series and then added this Series to the merged dataframe, which finalizes our data frame used for all of our analysis. 
4. We cleaned the `nutrition` column, from values that are list-look-like strings to actual lists.
5. We create a separate column for every unique value in the `nutrition` lists, so now each value in list of the `nutrition` column has its own column titled `calories`, `total fat`, `sugar`, `sodium`, `protein`, `saturated fat`, and `carbohydrates`.  
6. Using the same procedure, we also clean the `sugar`, `sugar`,  and `sugar` columns into actual lists, which are all originally list-look-like strings. 
### Univariate Analysis
### Bivariate Analysis
### Interesting Aggregates

# Assessment of Missingness
# Hypothesis Testing
