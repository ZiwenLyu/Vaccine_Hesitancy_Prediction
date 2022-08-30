# Final-Project Topic: COVID 19 Vaccine Hesitancy Analysis

[Link to Tableau Story](https://public.tableau.com/app/profile/ziwen.lyu/viz/TheMapofVaccineHesitancySupplyLevelandOrders/Story2?publish=yes)

## Analysis Overview:
The purpose of this project is to analyze and predict the level of Vaccine Hesitancy in USA in late 2021, a year after the outbreak of the COVID pandemic. The datasets which we will be using for statistical calculations and other technology libraries will help identify and display trends, and predict the outcomes accordingly.

#### Questions Addressed:

- How hesitant a county is over vaccination based on local `public mask mandates,` 'stay-at-home orders', 'gathering bans' and the prevalence of certain `health conditions`? 

- How hesitant a county is over vaccination based on `CVAC levels` measuring a county's ability to handle a COVID outbreak?

- What counties will rank highly hesitant based on its `SVI levels` measuring a county's social vulnerability to disaster?

- What counties will rank highly hesitant based on the local COVID transimission level?

***

## Project Developmental Planning:
### Project planning can be split into four steps:

- **Data Integration**: PostgreSQL is the database we intend to use to construct the pipline, where we aggregate, transform, and load data to the warehouse.

- **Explotary Data Analysis**: We will explore features and visualize their relations in Tableau.

- **Data Cleaning and Analysis**: We will adopt Python to clean and preprocess the data for the model training.

- **Machine Learning**: Afterwards, we will use several regression models to train and validate our machine learning as listed below.
     - `Regression Tree`: Regression Tree model is similar to decision tree classifier, it runs like a tree with branches to sub-divide all records in training sets and recursively partition until a simple model can fit them. It is suitable for non-linear regression, when interactions are complicated.
     - `Linear Regression`: Linear Regression is the supervised Machine Learning model in which the model finds the best fit linear line between the independent and dependent variable.
     -  `Support Vector Regression (SVR)`:  This model improves the flexibility from the linear regression to give an acceptable areas of errors and find a hyperplane to fit the data. 

- **Importance Analysis**: Finally, we conduct an importance analysis of our model to check which feature weighs more in the prediction.

| Software & Libraries |   |
| --- | --- |
| Data Retrieval, Cleaning, and Analysis:  | Python, API |
| Database Management/Storage:  | PostgreSQL |
| Predictive Analysis:  | Machine Learning |
| Data Visualization:  | Tableau |

***

## Extract, Transform, and Load

### Extract

We extract data directly from CDC website in csv file format.

#### Data Source:

Vaccine providers: https://data.cdc.gov/Vaccinations/Vaccines-gov-COVID-19-vaccinating-provider-locatio/5jp2-pgaw

Vaccination: https://data.cdc.gov/Vaccinations/Fully-Vaccinated-Adults/jm79-dz78

Vaccine Hesitancy: https://data.cdc.gov/Vaccinations/COVID-19-Vaccinations-in-the-United-States-County/8xkx-amqh

Mask Mandates: https://data.cdc.gov/Policy-Surveillance/U-S-State-and-Territorial-Public-Mask-Mandates-Fro/42jj-z7fa

Stay-at-home orders: https://data.cdc.gov/Policy-Surveillance/U-S-State-and-Territorial-Stay-At-Home-Orders-Marc/y2iy-8irm

Gathering bans: https://data.cdc.gov/Policy-Surveillance/U-S-State-and-Territorial-Gathering-Bans-March-11-/7xvh-y5vh

Health conditions: https://chronicdata.cdc.gov/500-Cities-Places/PLACES-Census-Tract-Data-GIS-Friendly-Format-2021-/yjkw-uj5s

Transmission level: https://data.cdc.gov/Public-Health-Surveillance/United-States-COVID-19-County-Level-of-Community-T/nra9-vzzn

### Transform and Integration

- Connected all tables based on a primary key County level fip code.
- Cleaned data: setting data types, dropping redundancy, parsing and filtering all data to the same date range, aggregating features by county level, and correcting typos in SQL.
- Integrated all tables into one by selecting useful features in SQL.

### Load
Save the transformed data in the database and send it via SQLAlchemy to Python ready for the model training.

***

## The overview of transformed data

We covered 44 states, 2592 counties in the United States.

### Targets
There are three targets: U.S. counties' general hesitant, hesitant or unsure, and strongly hesistant rates. So our regression model will run three times for three targets.

### Features
We have 27 features in those fields below:

#### Vaccine Availability:

the_number_of_providers per FIPS code
the_average_supply_level for all location providers per FIPS code

#### Community Risk/Vaccination Measures:

- social_vulnerability_index_svi ranges from 0 (least) to 1 (most) to measure community's vulnerability to disaster
- cvac_level_of_concern_for_vaccination_rollout ranges from 0 (lowest) to 1 (highest) for concern for difficult vaccine rollout
- percent_adults_fully_vaccinated_against_covid_19_as_of_6_10_21
- community_transmission_level is either low, moderate, substantial, or high rate of transmission:
based on total new COVID-19 cases per 100,000 persons in the last 7 days and percentage of positive SARS-CoV-2 diagnostic nucleic acid amplification tests (NAAT) in the last 7 days (higher of the two if different)
- Percent of Population Fully Vaccinated
- the_number_of_providers: how many providers are in the county.
- the_average_supply_level: the average level of vaccine supply for each county. The supply level is ranging from -1 to 4.

- completeness_pct tells percent of people fully vaccinated with a reported, valid county FIPS code in the jurisdiction
- series_complete_pop_pct_ur_equity combines 'fully vaccinated' percentage and metro/non-metro metric for ages of population:

1 = 0-49.9% fully vaccinated / metro county

2 = 50-64.9% fully vaccinated / metro county

3 = 65-79.9% fully vaccinated / metro county

4 = ≥80% fully vaccinated / metro county

5 = 0-49.9% fully vaccinated / non-metro county

6 = 50-64.9% fully vaccinated / non-metro county

7 = 65-79.9% fully vaccinated /non-metro county

8 = ≥80% fully vaccinated / non-metro county

#### Orders/Bans and Other Guidance:

- masks_order_codewhere 1 = public mask mandate; 2 = no public mask mandate
- general_gb_order_code indicates size of gatherings banned:

1 = No order (for gathering ban) found

2 = Ban of gatherings over 101 or more people

4 = Ban of gatherings over 26-50 people

6 = Ban of gatherings over 1-10 people

7 = Bans gatherings of any size

- general_or_under_6ft_bans_gatherings_over tells max number of people that can gather without social distancing:
No ban, 0, 6, 8, 10, 50, 150, or 250
- stay_at_home_order_code based on stay at home order recommendation:

3 = Mandatory only for at-risk individuals in the jurisdiction

6 = Advisory/Recommendation

7 = No order for individuals to stay home or NA

#### Health Conditions (county average estimates of prevalence of specified health conditions in adults 2019):

- avg_copd (chronic obstructive pulmonary disease)
- avg_chd (coronary heart disease)
- avg_ghlth for fair or poor health
- avg_lpa for no leisure-time physical activity
- avg_mhlth for mental health not good for >=14 days
- avg_obesity
- avg_sleep for sleeping less than 7 hours
- avg_checkup
- avg_asthma
- avg_smoking
- avg_depression
- avg_diabetes

#### Other:
fips_code is Federal Information Processing Standards (FIPS) Code; unique to each county
metro_status for metro (metropolitan county) vs non-metro

***

## The EDA of Vaccine Hesitancy, Supply Level, Community orders, and Health Conditions

### The Map of General Vaccine Hesitant Rate and Community Orders
This is an overview of vaccine hesitancy and community orders on a county level. Blue colors stands for high vaccine hesitancy, while orange colors represents low vaccine hesitancy. Montana, Wyoming, Idaho, Mississippi, Oklahoma, Kentucky, and Georgia have high vacccine hesitancy rate.
![the map of vaccine hesitancy](https://github.com/ZiwenLyu/Vaccine_Hesitancy_Prediction/blob/main/screenshots/Tableau%20Screenshots/the_map_of_hesitancy.png)

### The Map of Vaccine Supply Level
The colors suggest the average vaccine supply level for each state. Lousiana, Vermont, Wyoming, Iowa, Nebraska, Oregon, Indiana, and Wisconsin are among the least vaccine supply level. 
![the map of vaccine hesitancy](https://github.com/ZiwenLyu/Vaccine_Hesitancy_Prediction/blob/main/screenshots/Tableau%20Screenshots/the_map_of_supply_level.png)

### The Community orders vs General Hesitancy
The graph illustrates how community orders (gathering bans, mask mandates, and stay-at-home orders) relates to the vaccine hesitancy rate. Obviously, counties without gathering ban have the highest hesitancy rate. This may explain how government's orders send messages to the public and raise people's awareness of public health.
![The Community orders vs Hesitancy](https://github.com/ZiwenLyu/Vaccine_Hesitancy_Prediction/blob/main/screenshots/Tableau%20Screenshots/the_orders_vs_hesitancy.png)

### The Social vulnerability index (SVI) and transmission level vs General Hesitancy
Usually counties with high vulnerability shoud be most concerned by the government, as virus are more fatal to them. The graph reveals a positive correlation between social vulnerable index and hesitancy, and apparantly very highly vulnerable counties are the most hesitant to take vaccines. Also, we can see that counties are doubtful about vaccines suffer from more severe transimission. The government should focus on highly vulnerable counties.
![The Social vulnerability index and transmission level vs Hesitancy](https://github.com/ZiwenLyu/Vaccine_Hesitancy_Prediction/blob/main/screenshots/Tableau%20Screenshots/the_svi_vs_hesitancy.png)

### The Mental health vs General Hesitancy
The less healthy mental situations suggest the less willingness to take vaccine. The graph highlights the positive relationship in mental health (no leisure time rate, depression rate, and long mental health issues rate) and the vaccine hesitancy rate.  
![The mental health vs Hesitancy](https://github.com/ZiwenLyu/Vaccine_Hesitancy_Prediction/blob/main/screenshots/Tableau%20Screenshots/the_mental_vs_hesitancy.png)

### The Concerns over vaccine roll out vs Public physical health conditions
The physical disease rates are sliding as concerns for vaccine roll out difficulty decreases. This reflects that people with physical issues are more likely to take vaccine and worry about the vaccine avaliability,
![The Concerns over vaccine roll out vs Public physical health conditions](https://github.com/ZiwenLyu/Vaccine_Hesitancy_Prediction/blob/main/screenshots/Tableau%20Screenshots/the_concern_vs_physical_health.png)

### The Number of Providers vs General Hesitancy 
Though the linear relationship between supply level and hesitancy is not apparant in the graph. But we can identify that some highly hesitant counties (rate >= 0.2) get less vaccine supply (providers < 200), and counties with more supply (providers > 1200) show less hesitant over vaccine (rate  < 0.14).
![The Number of Providers vs Hesitancy ](https://github.com/ZiwenLyu/Vaccine_Hesitancy_Prediction/blob/main/screenshots/Tableau%20Screenshots/the_providers_vs_hesitancy.png)

## Prepare data and Build Machine Models
### Processing the data
#### Handle the missing values
As we can see from the screenshot, some columns contain nulls needed to be deal with:
![null values](https://github.com/jvvyas/Final-Project/blob/main/screenshots/null_values.png)
- First, we transform some continous data to numeric data type:

`social_vulnerability_index_svi`, `cvac_level_of_concern_for_vaccination_rollout`, `percent_adults_fully_vaccinated_against_covid_19_as_of_6_10_21`, `estimated_hesitant`, `series_complete_pop_pct`, `series_complete_yes`, `cases_per_100k_7_day_count_change`, `series_complete_pop_pct_svi`,`series_complete_pop_pct_ur_equity`,`estimated_hesitant_or_unsure`,`estimated_strongly_hesitant`

![transform to numeric data](https://github.com/jvvyas/Final-Project/blob/main/screenshots/transform_to_numeric.png)

- Second, from the screenshot above we can see that, `avg_asthma`, `avg_chd`,`avg_checkup`, `avg_copd`, `avg_smoking`, `avg_depression`, `avg_diabetes`, `avg_ghlth`, `avg_lpa`, `avg_mhlth`, `avg_obesity`,`avg_sleep`,`social_vulnerability_index_svi`, `series_complete_yes`,`series_complete_pop_pct` only have no more than 21 rows of nulls. Since it's not a lot compared to the whole dataset, so we just directedly dropped those rows.

- Thrid, since `cases_per_100k_7_day_count_change` column contains too many nulls, we dropped this column.

- Fourth, `percent_adults_fully_vaccinated` is continous data and contains more than 200 nulls. It is not affordable for us to directly drop 200 more rows as we may lose lots of information. So we use mean values of this column to fill its nulls.

- Fifth, `series_complete_pop_pct_svi` and `series_complete_pop_pct_ur_equity` are categorical data and also have more than 200 nulls. We will use their modes, or most frequent value to fill their nulls.

 ![Mode_mean_completer](https://github.com/jvvyas/Final-Project/blob/main/screenshots/mean_mode_completer.png)

#### Label Categorical Data
After cleaning all the nulls, we are going to encode categorical data.

![categorical data](https://github.com/jvvyas/Final-Project/blob/main/screenshots/categorical_data.png)

According to the screenshot above, each categorical column contains more than 2 but less than 10 unique values, so we decide to adopt LabelEncoder to label each column's values.

![label_encoder](https://github.com/jvvyas/Final-Project/blob/main/screenshots/label_encoder.png)


### Train the models
Since the rates of estimated hesitancy, the strongly hesitancy, and the hesitancy or unsure are continous numbers, we will train regression models to predict each county's hesitancy rate. We splited the training and testing set by 75-25, standardly scaled the training set, fit and test the model for three different targets.

#### Linear Regression
Linear Regression is the supervised Machine Learning model in which the model finds the best fit linear line between the independent and dependent variable.

#### Regression Tree
Regression Tree model is similar to decision tree classifier, it runs like a tree with branches to sub-divide all records in training sets and recursively partition until a simple model can fit them. It is suitable for non-linear regression, when interactions are complicated.

#### SVM Regressor
This model improves the flexibility from the linear regression to give an acceptable areas of errors and find a hyperplane to fit the data.

### Results
We used Mean absolute error (MAE), Mean squared error (MSE), Root mean squared error (RMSE), and R Squared score (R2) to test the models and measure their accuracy. 

`Mean absolute error (MAE)`: The mean of the absolute values of the individual prediction errors on over all instances in the test set. It tells us how big of an error we can expect on average. The smaller the MAE is, the less errors the model will predict.

`Mean squared error (MSE)`: The mean of the squared prediction errors over all instances in the test set.

`Root mean squared error (RMSE)`: The square root of the mean of the square of all of the error.

`R Squared score (R2)`: A statistical measure of fit that indicates how much variation of a dependent variable is explained by the independent variable(s) in a regression model. The higher the R2, the better the model fits the data.

#### Linear Regression
![linear_regression_results](https://github.com/ZiwenLyu/Vaccine_Hesitancy_Prediction/blob/main/screenshots/linear_results.png)

#### Regression Tree
![regression_tree_results](https://github.com/jvvyas/Final-Project/blob/main/screenshots/tree_results.png)

#### SVM Regressor
![SVM_results](https://github.com/ZiwenLyu/Vaccine_Hesitancy_Prediction/blob/main/screenshots/SVR_results.png)

As we can see results from three models, regression tree gives the best performance overall. And the strong hesitancy is best predicted among other targets. The regression tree predicting the strong hesitancy scores MAE only 0.008, and the R2 is 80%. This means that our regression tree model can predict the strong hesitancy rate of counties with high accuracy and little error.

### Importance Analysis 
Since regression tree gives the best result, we will dig into this model and analyze the importance of each feature on the variations of target.

#### Hesitancy 

![Hesitancy_importance](https://github.com/ZiwenLyu/Vaccine_Hesitancy_Prediction/blob/main/screenshots/hesitancy_importance.png)

We can see that top correlated 4 features are:
- Complete Vaccination percentage
- Gathering bans
- Average no-leisure-time rate
- Average checkup frequency 

#### Unsure Hesitancy

![unsure_hesitancy](https://github.com/ZiwenLyu/Vaccine_Hesitancy_Prediction/blob/main/screenshots/hesitancy_unsure_importance.png)
We can see that top correlated 4 features are:

- Average mental health rate
- Complete Vaccination percentage
- Gathering bans
- Average no-leisure-time rate percentage

#### Strong Hesitancy

![strong_hesitancy](https://github.com/ZiwenLyu/Vaccine_Hesitancy_Prediction/blob/main/screenshots/strong_hesitancy_importance.png)

- Complete Vaccination percentage
- Gathering bans
- Average no-leisure-time rate 
- Average checkup frequency 

***

## Conclusions & Further Research Recommendations 
The Regression Tree model we built could predict the vaccine hesitant rate on a county level based on community health conditions, orders, SVI, and supply. It had the best overall predictive performance. It could offer suggestions for the government when campaigning and rolling out vaccines.

**Insights**: The government's orders and health guidance sent important messages to the public and help the people raise awareness towards the virus. As we see places with or without gathering bans rated significantly differently on their hesitancy. 
Also, the analysis highlights the social vulnerability index and health conditions as key points to consider when rolling out vaccine. Vulnerable people with fundmental diseases should be cared the most during the pandemic, but the analysis shows they are the most reluctant to vaccination and suffering from high or substantial transmission. The government though priotized seniors and vulnerable ones for vaccination, the analysis suggests more campaigns, guidances, or instructions should be done.  

For the further research, we will recommend:
- Wider the data scope to include more counties.
- Sentimental analysis on social media posts, as it also plays a key role in vaccine hesitancy.
- Scratch and collect news about vaccine and conduct keyword analysis.
