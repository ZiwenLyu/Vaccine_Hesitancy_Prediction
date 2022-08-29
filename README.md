# Final-Project Topic: COVID 19 Vaccine Hesitancy Levels Analysis

[Link to Google Slides presentation](https://docs.google.com/presentation/d/1QMLQhXdtwhx3Y2UqlmNIYpZkR67QkC8ZYHKdYIHn1MU/edit#slide=id.p)

[Link to Tableau Story](https://public.tableau.com/app/profile/ziwen.lyu/viz/TheEDAofVaccineHesitancySupplyLevelOrdersandHealthConditions/Story1?publish=yes)

## Analysis Overview:
The purpose of this project is to analyze and predict the level of Vaccine Hesitancy in USA in late 2021, a year after the outbreak of the COVID pandemic. The datasets which we will be using for statistical calculations and other technology libraries will help identify and display trends, and predict the outcomes accordingly.

#### Questions Addressed:

- How hesitant a county is over vaccination based on local `public mask mandates,` 'stay-at-home orders', 'gathering bans' and the prevalence of certain `health conditions`? 

- How hesitant a county is over vaccination based on `CVAC levels` measuring a county's ability to handle a COVID outbreak?

- What counties will rank highly hesitant based on its `SVI levels` measuring a county's social vulnerability to disaster?

- What counties will rank highly hesitant based on the local COVID transimission level?

 
## Data Sources:

https://data.cdc.gov/Vaccinations/Vaccines-gov-COVID-19-vaccinating-provider-locatio/5jp2-pgaw

https://data.cdc.gov/Vaccinations/Fully-Vaccinated-Adults/jm79-dz78

https://data.cdc.gov/Vaccinations/COVID-19-Vaccinations-in-the-United-States-County/8xkx-amqh

https://data.cdc.gov/Policy-Surveillance/U-S-State-and-Territorial-Public-Mask-Mandates-Fro/42jj-z7fa

https://data.cdc.gov/Policy-Surveillance/U-S-State-and-Territorial-Stay-At-Home-Orders-Marc/y2iy-8irm

https://data.cdc.gov/Policy-Surveillance/U-S-State-and-Territorial-Gathering-Bans-March-11-/7xvh-y5vh

https://chronicdata.cdc.gov/500-Cities-Places/PLACES-Census-Tract-Data-GIS-Friendly-Format-2021-/yjkw-uj5s

https://data.cdc.gov/Public-Health-Surveillance/United-States-COVID-19-County-Level-of-Community-T/nra9-vzzn


| Software & Libraries |   |
| --- | --- |
| Data Retrieval, Cleaning, and Analysis:  | Python, API |
| Database Management/Storage:  | PostgreSQL |
| Predictive Analysis:  | Machine Learning |
| Data Visualization:  | Tableau |

***

## Project Developmental Planning:
### Project planning can be split into four steps:

- **Database Storage**: PostgreSQL is the database we intend to use to construct the pipline, where we aggregate, transform, and load data to the warehouse.

- **Explotary Data Analysis**: We will explore features and visualize their relations in Tableau.

- **Data Cleaning and Analysis**: We will adopt Python to clean and preprocess the data for the model training.

- **Machine Learning**: Afterwards, we will use several regression models to train and validate our machine learning as listed below.
     - `Regression Tree`: Regression Tree model is similar to decision tree classifier, it runs like a tree with branches to sub-divide all records in training sets and recursively partition until a simple model can fit them. It is suitable for non-linear regression, when interactions are complicated.
     - `Linear Regression`: Linear Regression is the supervised Machine Learning model in which the model finds the best fit linear line between the independent and dependent variable.
     -  `Support Vector Regression (SVR)`:  This model improves the flexibility from the linear regression to give an acceptable areas of errors and find a hyperplane to fit the data. 

- **Importance Analysis**: Finally, we conduct an importance analysis of our model to check which feature weighs more in the prediction.

***

## The EDA of Vaccine Hesitancy, Supply Level, Community orders, and Health Conditions

### The Map of Vaccine Hesitancy and community orders
This is an overview of vaccine hesitancy and community orders on a county level. Blue colors stands for high vaccine hesitancy, while orange colors represents low vaccine hesitancy. Montana, Wyoming, Idaho, Mississippi, Oklahoma, Kentucky, and Georgia have high vacccine hesitancy rate.
![the map of vaccine hesitancy]()

### The Map of Vaccine Supply Level
The colors suggest the average vaccine supply level for each state. Lousiana, Vermont, Wyoming, Iowa, Nebraska, Oregon, Indiana, and Wisconsin are among the least vaccine supply level. 
![the map of vaccine hesitancy]()

### The Community orders vs Hesitancy
The graph illustrates how community orders (gathering bans, mask mandates, and stay-at-home orders) relates to the vaccine hesitancy rate. Obviously, counties without gathering ban have the highest hesitancy rate. This may explain how government's orders send messages to the public and raise people's awareness of public health.
![The Community orders vs Hesitancy]()

### The Social vulnerability index (SVI) and transmission level vs Hesitancy
Usually counties with high vulnerability shoud be most concerned by the government, as virus are more fatal to them. The graph reveals a positive correlation between social vulnerable index and hesitancy, and apparantly very highly vulnerable counties are the most hesitant to take vaccines. Also, we can see that counties are doubtful about vaccines suffer from more severe transimission. The government should focus on highly vulnerable counties.
![The Social vulnerability index and transmission level vs Hesitancy]()

### The Mental health vs Hesitancy
The less healthy mental situations suggest the less willingness to take vaccine. The graph highlights the positive relationship in mental health (no leisure time rate, depression rate, and long mental health issues rate) and the vaccine hesitancy rate.  
![The mental health vs Hesitancy]()

### The Concerns over vaccine roll out vs Public physical health conditions
The physical disease rates are sliding as concerns for vaccine roll out difficulty decreases. This reflects that people with physical issues are more likely to take vaccine and worry about the vaccine avaliability,
![The Concerns over vaccine roll out vs Public physical health conditions]()

### The Number of Providers vs Hesitancy 
Though the linear relationship between supply level and hesitancy is not apparant in the graph. But we can identify that some highly hesitant counties (rate >= 0.2) get less vaccine supply (providers < 200), and counties with more supply (providers > 1200) show less hesitant over vaccine (rate  < 0.14).
![The Number of Providers vs Hesitancy ]()

