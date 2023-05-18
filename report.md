# DSC80-Project3-Report
by Xiaojie Chen(A17015417), Chenri Luo(A17015417)
## Introduction
## Cleaning and EDA
### Data Cleaning
1. We **left merged** the recipes and interactions datasets together and then **filled all ratings of 0 with** `np.nan`. 
- This is because NaN values are typically excluded from statistical calculations such as mean, median, and standard deviation. By converting 0 ratings to NaN, we can ensure that these calculations accurately represent the ratings provided by users, without distorting the statistical measures due to explicit low ratings.
2. We **dropped the duplicate id column**, which is `recipe_id`. 
- As after merging two data frames, which are ratings and interactions, both dataframes have the recipe id columns, which are unneccessary to have two columns. So we delete one to save some time when doing the analysis. 
3. We found the **averate rating per recipe** as a Series and then added this Series to the merged dataframe, which finalizes our data frame used for all of our analysis. 
- As for the same recipe, it may have several rows (i.e receive more than one ratings and reviews), so by getting a average rating of each recipe, we can get a single representative rating that indicates overall sentiment or perceived quality of each recipe. 
4. We cleaned the `nutrition` column, from values that are list-look-like strings to actual lists, and then we created a separate column for every unique value in the `nutrition` lists, so now each value in list of the `nutrition` column has its own column titled `calories`, `total fat`, `sugar`, `sodium`, `protein`, `saturated fat`, and `carbohydrates`.  
- By giving a separate column for each type of nutrient, it makes our analysis process faster and easier for inidivual analysis of each nutrient and also filtering like filter and sort recipes based on specific nutritional criteria. 
5. Using the same procedure, we also clean the `sugar`, `sugar`,  and `sugar` columns into actual lists, which are all originally list-look-like strings. 
### Univariate Analysis

### Bivariate Analysis
### Interesting Aggregates

## Assessment of Missingness
## Hypothesis Testing
