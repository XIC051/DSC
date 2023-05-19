# DSC80-Project3-Report
by Xiaojie Chen(A17015417), Chenri Luo(A17015417)
## Introduction
## Cleaning and EDA
### Data Cleaning

#### 1. We **left merged** the recipes and interactions datasets together and then **filled all ratings of 0 with** `np.nan`. 

- This is because NaN values are typically excluded from statistical calculations such as mean, median, and standard deviation. By converting 0 ratings to NaN, we can ensure that these calculations accurately represent the ratings provided by users, without distorting the statistical measures due to explicit low ratings.

#### 2. We **dropped the duplicate id column**, which is `recipe_id`. 

- As after merging two data frames, which are ratings and interactions, both dataframes have the recipe id columns, which are unneccessary to have two columns. So we delete one to save some time when doing the analysis. 

#### 3. We found the **averate rating per recipe** as a Series and then added this Series to the merged dataframe. 

- As for the same recipe, it may have several rows (i.e receive more than one ratings and reviews), so by getting a average rating of each recipe, we can get a single representative rating that indicates overall sentiment or perceived quality of each recipe. 

#### 4. We cleaned the `nutrition` column, from values that are list-look-like strings to actual lists, and then we created a separate column for every unique value in the `nutrition` lists, so now each value in list of the `nutrition` column has its own column titled `calories`, `total fat`, `sugar`, `sodium`, `protein`, `saturated fat`, and `carbohydrates`.  

- By giving a separate column for each type of nutrient, it makes our analysis process faster and easier for inidivual analysis of each nutrient and also filtering like filter and sort recipes based on specific nutritional criteria. 

#### 5. Using the same procedure, we also clean the `tags`, `steps`,  and `ingredients` columns into actual lists, which are all originally list-look-like strings. 

#### 6. We dropped the `review` column. 

- It doesn't have much usage. 

#### 7. We removed duplicate rows of unique recipe and only saved one row for each recipe. 

- As we have gained the data of average ratings for each recipe, and we can perform analysis on the average ratings for each recipe instead of the several ratings coming from severl users. 


- `head` of our cleaned DataFrame
                                                                                                                                             
  
### Univariate Analysis

### Bivariate Analysis

### Interesting Aggregates


## Assessment of Missingness
### NMAR Analysis

The column `description` is NMAR, because users might have chosen not to provide a description for certain recipes or left it blank intentionally. In order to further investigate potentially make it Missing at Random (MAR), additional data I would like to obtain can be user demographics, 























## Hypothesis Testing
