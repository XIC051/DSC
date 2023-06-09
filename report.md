# DSC80-Project3-Report
by Xiaojie Chen(A17015417), Chenri Luo(A16636808)
## Introduction

- This website is the report of our analysis centered around the following question: **"What is the relationship between the cooking time of a recipe and its average user rating?"**

- The dataset we use for our analysis is from merging two datasets, recipes and ratings, including data from food.com. After merging and cleaning, it has 83782 rows and 14 columns, with each row representing the dataset of an individual recipe. For the purpose of our analysis, the following columns are related to our question: 

1. `id`: The unique identifier for each recipe.
2. `minutes`: The cooking time required for the recipe in minutes.
3. `average rating`: The average rating given to the recipe by users.
4. `nutrition`: A list containing nutritional information about the recipe, including the number of calories, total fat, sugar, sodium, protein, saturated fat, and carbohydrates. We divided the list of nutrients into several columns, each column representing a nutrient. 
**Note**: as we have separated the list of nutrition into several columns, the nutrients that are related to our analysis are `calories` and `sodium`. So we chose to drop the `nutrition` column and other neccessary nutrients columns after cleaning the DataFrame.

- This analysis can provide valuable insight for relevent professionals and general public to understand the relationship bewteen the likeableness of a course (reflected by rating) and the level of sophistication for a recipe (reflected by cooking time). By the end of this analysis, readers can understand how they may be able to make their cuisine more attractive to chef and consumers by reducing or increasing the level of sophistication. It can provide insights to Questions like "are quicker recipes rated more favorably due to the convenience they offer?"

- In the following parts, we will show the sections of Data Cleaning and EDA (Exploratory Data Analysis), Assessment of Missingness, and Hypothesis Testing.

## Cleaning and EDA
### Data Cleaning

#### 1. We **left merged** the recipes and interactions datasets together and then **filled all ratings of 0 with** `np.nan`. 

This is because NaN values are typically excluded from statistical calculations such as mean, median, and standard deviation. By converting 0 ratings to NaN, we can ensure that these calculations accurately represent the ratings provided by users, without distorting the statistical measures due to explicit low ratings.

#### 2. We **dropped the duplicate id column**, which is `recipe_id`. 

As after merging two data frames, which are ratings and interactions, both dataframes have the recipe id columns, which are unneccessary to have two columns. So we delete one to save some time when doing the analysis. 

#### 3. We found the **averate rating per recipe** as a Series and then added this Series to the merged dataframe. 

As for the same recipe, it may have several rows (i.e receive more than one ratings and reviews), so by getting a average rating of each recipe, we can get a single representative rating that indicates overall sentiment or perceived quality of each recipe. 

#### 4. We cleaned the `nutrition` column, from values that are list-look-like strings to actual lists, and then we created a separate column for every unique value in the `nutrition` lists, so now each value in list of the `nutrition` column has its own column titled `calories`, `total fat`, `sugar`, `sodium`, `protein`, `saturated fat`, and `carbohydrates`.  

By giving a separate column for each type of nutrient, it makes our analysis process faster and easier for inidivual analysis of each nutrient and also filtering like filter and sort recipes based on specific nutritional criteria. Also, when assessing Missingness Dependency, we picked the column `sodium` to access the dependency of missingness of `average rating` on `sodium`. 

#### 5. We removed duplicate rows of unique recipe and only saved one row for each recipe. 

As we have gained the data of average ratings for each recipe, and we can perform analysis on the average ratings for each recipe instead of the several ratings coming from severl users. 

#### 6. Lastly, we dropped the `rating`,`user_id`, `date`, `review`, `contributor_id`, `steps`, `ingredients`, `tags`, `nutrition`, `submitted` columns

Throughout our whole analysis process, they don't have much usage. Removing them will reduce workload. 

- `head` of our cleaned DataFrame

| name                                 |     id |   minutes |   n_steps | description                                                                                                                                                                                                                                                                                                                                                                       |   n_ingredients |   average rating |   calories |   total fat |   sugar |   sodium |   protein |   saturated fat |   carbohydrates |
|:-------------------------------------|-------:|----------:|----------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------------:|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|
| 1 brownies in the world    best ever | 333281 |        40 |        10 | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              |               9 |                4 |      138.4 |          10 |      50 |        3 |         3 |              19 |               6 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |        12 | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                            |              11 |                5 |      595.1 |          46 |     211 |       22 |        13 |              51 |              26 |
| 412 broccoli casserole               | 306168 |        40 |         6 | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 |               9 |                5 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |
| millionaire pound cake               | 286009 |       120 |         7 | why a millionaire pound cake?  because it's super rich!  this scrumptious cake is the pride of an elderly belle from jackson, mississippi.  the recipe comes from "the glory of southern cooking" by james villas.                                                                                                                                                                |               7 |                5 |      878.3 |          63 |     326 |       13 |        20 |             123 |              39 |
| 2000 meatloaf                        | 475785 |        90 |        17 | ready, set, cook! special edition contest entry: a mediterranean flavor inspired meatloaf dish. featuring: simply potatoes - shredded hash browns, egg, bacon, spinach, red bell pepper, and goat cheese.                                                                                                                                                                         |              13 |                5 |      267   |          30 |      12 |       12 |        29 |              48 |               2 |


### Other Data Cleaning

We also created several other dataframes basing on the original merged dataframe, which is `merged_df`, used for different analysis process as follows:

#### Cleaned dataframe for analysis involing column `minutes`

The original `merged_df` contains many receipes that have unreasonable cooking minutes,such as 1051200 minutes. We set cooking time with less than **720 mintues(12 hours)** as a resonable threshold. Therefore, we only want to focus on recipes with 720 or less cooking minutes and filter out the rows with extreme minutes values. 

The resulting dataframe is named `filtered_df`. 

#### Cleaned dataframe for analysis involing column `calories`

The original `merged_df` contains many receipes that have unreasonable calories, such as 45609.0. We set calories with less than **3000**
as a resonable threshold. Therefore, we only want to focus on recipes with 3000 or less calories and filter out the rows with extreme calories values. 

The resulting dataframe is named `filter_cal_df`. 

#### Cleaned dataframe for analysis of Missingness Dependency 

For missingness dependency, we picked column `average rating` to analyze. We also picked column `sodium` and `n_steps`, to assess the dependency of the missingness of `average rating` on other columns, which are `sodium` and `n_steps`. For convenience, we created two separate dataframe specific for the permutation process. As the only columns involed are `["average rating", "n_steps"]` and `["average rating", "sodium"]`, the new data frame consists of only the neccessary columns and add a new column named `rating_missing`, which contain a boolean series indicating the missingness of average rating. 

The resulting dataframes are named `df_step` and `df_sodium`. 

#### Cleaned dataframe for permutation testing

For the permutation testing, only columns `minutes`, `average rating` are involved. So we created a new DataFrame by getting only these two colmns first, then we dropped all the rows that have null values. Then we added a new column named `high rating`, which are boolean series indicating whether that specific recipe belongs to the low average-rating group (average rating <= 4.6) or high average-rating group (average rating > 4.6). Details about this grouping is explained in the **Hypothesis Testing Section**. 

The resulting dataframe is named `df_plot`. 
                                                                                                                                           
  
### Univariate Analysis

#### 1. The distribution of average rating of recipes

<iframe src="assets/rating-hist.html" width=800 height=600 frameBorder=0></iframe>

- In order to display the distribution of average rating, we create a **histogram** of column `average rating` from filtered_df, which has number of bins = 10. From the histogram, we can see that about 64% of average rating is in the range of 4.75 - 5. The number of the rest of the average ratings(rating from 1 - 4.75) add up to only 36% of the total. 

#### 2. The distribution of cooking minutes of recipes

<iframe src="assets/cooking_time_dist.html" width=800 height=600 frameBorder=0></iframe>

- In order to display the distribution of cooking minutes, we create a **histogram** of column `minutes` from filtered_df, which has number of bins = 240. From this hisogram, we can see the distribution is right skewed. Half of the recipes cooking minutes are in the range from 20 - 60 minutes, among them, about 9 percent of cooking mintues are in 30-34 range. 

### Bivariate Analysis

We want to use bivariate analysis to determine if there is a statistical link between the other variables and the variables that we are interested in, if so, how strong and in which direction that link is.

#### 1. scatter plot of cooking minuetes vs food calories

<iframe src="assets/minutes_calories.html" width=800 height=600 frameBorder=0></iframe>

- When analyzing this dataframe, we believe calories may have a relation to average rating and cooking minutes. So we first created a scatter plot of cooking minuetes vs food calories. However, when we observe the data of calories, we find that there are also a lot of food calories that we consider to be outliers. So we filtered the data down to foods with less than 3,000 calories. From this scatter plot, we can see a slightly positive correlation, which is calculated to be about 0.11. 

#### 2. scatter plot of average rating vs food calories

<iframe src="assets/rating_calories.html" width=800 height=600 frameBorder=0></iframe>

- We also wanted to observe whether calories were related to average rating, so we created a scatter plot of average rating vs food calories and used different colors for each point to distinguish minutes. But this graph doesn't show any inclination. So there's no connection between the two variables. 

#### 3. side-by-side boxplots for minutes of recipes with average rating under 4.6 vs above 4.6

<iframe src="assets/box.html" width=800 height=600 frameBorder=0></iframe>

- By analyzing this data frame, we know that the mean average rating is about 4.6. Therefore, we divided the data frame into two parts, one is the average rating of less than 4.6(low average rating), the other is greater than 4.6 (high average rating). We want to know if there is a difference between cooking minutes with a high average rating and cooking minutes with a low average rating. As can be seen from the figure, there is a slight difference between the two groups. The low average rating of cooking minutes median was 40, while the other group was 35. And their Q3 is 65 and 60, respectively.


### Interesting Aggregates

| ingredients_bin (vertical index)/ steps_bin (horizontal index)   |      <5 |    5-10 |   10-15 |   15-20 |     20+ |
|:------------------|--------:|--------:|--------:|--------:|--------:|
| <5                | 4.66445 | 4.63257 | 4.61267 | 4.64353 | 4.67499 |
| 5-10              | 4.62099 | 4.61062 | 4.61059 | 4.63636 | 4.66111 |
| 10-15             | 4.61949 | 4.61454 | 4.61632 | 4.63757 | 4.6527  |
| 15-20             | 4.68033 | 4.63403 | 4.66041 | 4.62675 | 4.61278 |
| 20+               | 4.94559 | 4.71494 | 4.7314  | 4.70667 | 4.65613 |

#### Note: the table has multi-index, the column index is the ingredients_bin, and the row index is the steps_bin. 

#### Columns We Chose: `average rating`, `n_steps`, `n_ingredients`

#### Interpretation

We put each value of `n_steps` and `n_ingredients` into five different bins, with ranges `bins = [0, 5, 10, 15, 20, np.inf]`. By grouping similar values together, we can now analyze and compare the data based on these categorized variables. 

##### Table Info

The pivot table presents the mean of average ratings for each combination of "ingredients_bin" and "steps_bin". The rows represent the different ranges of ingredient counts, while the columns represent the ranges of step counts. Each cell in the table contains the mean value of average rating for that specific combination.

##### What can we tell

From the table, we can observe that recipes in the 20+ number of ingredients bin and <5 number of steps have the highest mean of average ratings. So we can see this combination of steps and ingredients number is the most popular among other combinations. Also, by looking horizontally, we can observe that recipes with 20+ number of ingredients tend to have a higher mean of average rating than other recipes with lower number of ingredients. So we can derive that people tend to like recipes with more ingredients. 

##### Significance

By looking at this pivot table, we can assess whether recipes with more or fewer **ingredients** tend to receive higher ratings, at the same time, we can assess whether recipes with more of fewer **number of steps** tend to receive higher ratings. Also, by considering the mean of average ratings at the intersection of the rows and columns, you can explore how the combination of ingredient and step counts influences the recipe ratings. This analysis can reveal whether certain combinations lead to consistently higher or lower ratings compared to others.

## Assessment of Missingness

### NMAR Analysis

The column `description` is NMAR, because users might have chosen not to provide a description for certain recipes or left it blank intentionally. In order to further investigate potentially make it Missing at Random (MAR), additional data we would like to obtain can be user demographics, such as age, gender, or location, so we can further investigate whether the missingness of description depend on these factors or not. For example, if older users tend to not writing description when rating a recipe, it will change the missingness mechansim of `description` to MAR. 

### Missingness Dependency

#### 1. depends on

The missingness of column `average rating` **does** depend on the column `n_steps`. 

##### Null hypothesis: 
the distribution of `n_steps` is the same when column `average rating` is missing and when column `average rating` is not missing. 

##### Alternative hypothesis: 
the distribution of `n_steps` is not the same when column `average rating` is missing and when column `average rating` is not missing.
 
##### Distribution of column `n_steps` when column `average rating` is missing and not missing

<iframe src="assets/steps_missingness_1st.html" width=800 height=600 frameBorder=0></iframe>

##### test statistic

We choose k-s as our test statistic 

##### Result interpretation: 

After doing 500 times of shuffling of the n_steps column and calculates the k-s test statistic, we get a p-value of 0.0, indicating that we should reject the null hypothesis under any siginificance value. We can draw the conclusion that  column `average rating` is MAR dependent on `n_steps`. 

<iframe src="assets/steps_missingness_2nd.html" width=800 height=600 frameBorder=0></iframe>

#### 2. does not depend on

The missingness of column `average rating` **does not** depend on the column `sodium`. 

##### Null hypothesis: 

the distribution of `sodium` is the same when column `average rating` is missing and when column `average rating` is not missing.

##### Alternative hypothesis: 

the distribution of `sodium` is not the same when column `average rating` is missing and when column `average rating` is not missing.

##### Distribution of column `sodium` when column `average rating` is missing and not missing

<iframe src="assets/sodium_missingness_1st.html" width=800 height=600 frameBorder=0></iframe>

##### Result interpretation: 

After doing 1000 times of shuffling of the sodium column and calculates the k-s test statistic, we get a p-value of 0.128, indicating that we fail to reject the null hypothesis under siginificance value of 0.05. We can draw the conclusion that the missingness of column `average rating` does not depend on column `sodium`.

<iframe src="assets/sodium_missingness_2nd.html" width=800 height=600 frameBorder=0></iframe>


## Hypothesis(Permutation) Testing

#### 1. Clarification

We categorize the recipes into two different groups, using a threshold that is the mean average-rating of all recipes, which is around 4.6 

**high average-rating**: recipes with average-rating that is higher than 4.6

**low average-rating**: recipes with average-rating that is lower than 4.6

We chose 4.6 because as mentioned above in the **Univariate Analysis** section, about 64% of average rating is in the range of 4.75 - 5, so most of the recipes receive a very high rating. Therefore, we choose to use the mean of all average ratings to distinguish recipes with **high average-rating** and **low average-rating**. We choose to do a **permutation testing** to see whether the two groups of recipes with low and high average-ratings come from the same distribution or not. 

#### 2. Null Hypothesis

In the population, cooking time (minutes) of recipes with a **high average-rating** and recipes with a **low average-rating** have the same distribution, and the observed differences in our samples are due to random chance.

#### 3. Alternative Hypothesis

In the population, recipes with a high average-rating have shorter cooking time than recipes with low average-rating. The observed difference in our samples cannot be explained by random chance alone.

#### 4. Test Statistic

**difference in means:**

mean minutes of high average-rating recipes - mean minutes of low average-rating recipes

#### 5. Significance Level

0.05

#### 6. P-value

0.0
 
#### 7. Visualization of Distribution of cooking time  (minutes) by rating Status

By dividing each recipe into two groups by their rating status, which is either **high average-rating group** or **low average-rating group**, we can draw two histograms of the distribution of cooking time of each rating status in one graph. So we can have a more direct sense of differences in the two distributions.  

<iframe src="assets/dist_permutation.html" width=800 height=600 frameBorder=0></iframe>
 
#### 8.Justification

As 0.0 < 0.05, we reject our null hypothesis that cooking time (minutes) of recipes with a **high average-rating** and recipes with a **low average-rating** come from the same distribution. Although we cannot say that longer cooking times lead to lower average-rating of recipes, we can conclude that there is a significant relationship between the average rating of recipes and their cooking time. Under a significance level of 0.05, there is sufficient evidence to support that ecipes with a high average-rating **tend to have shorter cooking times** compared to recipes with a low average-rating. 

<iframe src="assets/permutation.html" width=800 height=600 frameBorder=0></iframe>
