# Read Between the Wines

## Overview

This analysis explored red and white wine datasets to predict quality and type based on physicochemical properties (physical and chemical characteristics).  This could potentially be useful to restauranteurs when choosing new wines for their restaurant based on the label. Our goal was to have a model with at least 80% accuracy.

## Process & Results

### Our Question

Can quality score (0-10) of both red and white wines be predicted using a combination of the following features with at least 80% accuracy?
  
- fixed acidity
- volatile acidity
- citric acid
- residual sugar
- chlorides
- free sulfur dioxide
- total sulfur dioxide
- density
- pH
- sulphates
- alcohol

### Exploratory Data Analysis (EDA)
<details>
<summary>K-means</summary>

Red and White datasets were read into dataframes for EDA which included:

- checking headers and ensuring that dataframes were compatible
- checking datatypes
- checking for null values
- finding and removing duplicate entries
- adding a "type" column to the dataframes
- performing counts
- joining the dataframes by .append
- checking for unique values
- generating a correlation table
- exploring data using visualizations
- scaling the combined data and performing PCA
- performing exploratory Kmeans analysis
- determining if meaningful clustering could be achieved

Meaningful clustering was not achieved.

<figcaption align = "center"><b>K-Means Wine Clusters</b></figcaption><img src="media/Kmeans_cluster_wines.png" width = 400>

We attemped to explore each dataset individually as above, with a similar result.

<figcaption align = "center"><b>K-Means Red Wine Cluster</b></figcaption><img src="media/Kmeans_cluster_red.png" width = 400 >

<figcaption align = "center"><b>K-Means White Wine Cluster</b></figcaption><img src="media/Kmeans_cluster_white4.png" width = 400 >
</details>

##### Clustering Outcome

Clustering wasn't effective when trying to determine the quality rating 0-10.

<details>
<summary>Classification</summary>

Further data exploration was completed using a RandomForestClassifier.  The datasets were examined individually first.

- Duplicates were dropped.
- Features and target (quality) were defined.
- Data was split into training and testing.
- A standard scaler was used to scale the data.
- Scaled data was fit to a RandomForest Classifier.
- Predictions were made, a confusion matrix was generated, and classification report  run.
- The red predictions were estimated to have ~60% accuracy
- The white predictions were estimated to have ~52% accuracy
- A booster (EasyEnsembleClassifier) was used in an attempt to increase accuracy, but it actually decreased for both wine types.
- Features were ranked by importance for each data set

The top 5 overlapping features were noted and the dataframe was reduced to only those features for the combined dataset.

- The same steps were taken as above, but no booster was attempted.
- The resultant accuracy score was ~53%.
</details>

##### Classification Outcome

Classification wasn't effective when trying to determine the quality rating 0-10.

### A New Question

After the road blocks with our data noted above, we needed to change our question based on what we learned.  We noted that the features that rated a better quality score were very different for reds and whites and even differed enough internally that the models' accuracy suffered.  Models had a really hard time predicting the ratings 5-6, and that is where the majority of the data fell.  Time to rethink our question.

Can we predict "Good" wine?  

Our group chose to rank the wines as "Good" or "Bad" by applying conditions.

### Preprocessing

Preprocessing included:

- The red and white data sets were read in as dataframes and combined using sqlalchemy, dropping duplicates.
  - The total number of wine in the combined dataset is 5,320.
    - White Wine: 3,961
    - Red Wine: 1,359
- Wines with a rating of greater than or equal to 7 were classified as "Good" (0) and less than 7 were given a "Bad" (1) rating.
- Features and target (quality) were defined.
- Data was split into training and testing.
- A standard scaler was used to scale the data.

### Quality Model

- Scaled data was fit to a RandomForest Classifier.
- Predictions were made, a confusion matrix was generated, and classification report run.

<figcaption align = "center"><b>Quality Prediction Confusion Matrix</b></figcaption><img src="media/quality_metrics.png" width = 300>

- Features were ranked by importance for this model.

<figcaption align = "center"><b>Quality Prediction Feature Importance</b></figcaption><img src="media/quality_features.png" width = 300>

### Additional Analysis

When we noted the differences between the wine types as related to quality, it was suggested that we may be able to predict the wine type based upon the physiochemical feature differences.

### Type Model

- Preprocessing was performed as in the above model with 2 exceptions:

  - The target was "type"
  - Conditions were not applied to determine "good" or "bad".

<figcaption align = "center"><b>Wine Type Prediction Confusion Matrix</b></figcaption><img src="media/type_metrics.png" width = 300>

<figcaption align = "center"><b>Wine Type Prediction Feature Importance</b></figcaption><img src="media/type_features.png"width = 300 >

## Discussion

We chose to use supervised learning because our data was labeled and had a smaller amount of variables.  We chose a classification model to label our output as "good" or "bad".  The only statistics calculated in this project are the classification report and include accuracy, precision, sensitivity, and F1. We also evaluated feature correlation during EDA.

Our original goal for the Quality Model sensitivity was 80%.  Our current model will be accurately projected ~85% of the time.  It is more sensitive for the detection of "Bad" wines at 95%, but the low score(~40%) may indicate some false negatives in the "Good" category.  The F1-score is supportive of using this model with a higher score of 83%.

The Type Model will be accurately projected ~99% of the time.  It rates high for sensitivity, precision, and F1-score. With a model rating this high, there was a concern for data leakage skewing the model.  A correlation table was generated and the highest correlation seen was ~69% which did not indicate that data leakeage was present.

<figcaption align = "center"><b>Wine Type Correlation</b></figcaption><img src="media/type_corr_heatmap.png" width = 400>

## Summary

- Clustering wasn't effective when trying to determine the quality rating 0-10.
- Classification wasn't effective when trying to determine the quality rating 0-10.
- The Quality Model is predicted to be accurate ~85% of the time when classifying wines as "good" or "bad".
- The Type Model is predicted to be accurate ~99% of the time when classifying red and white wines.

## Datasets

Datasets used were Red Wine Quality (winequality-red.csv) and White Wine Quality (winequality-white.csv) available from [Kaggle](https://www.kaggle.com/).  These were compiled into tables using PostgreSQL tools in PGAdmin and utilized a basic Entity Relationship Diagram (ERD).

## Resources & Technologies

Python was used for this analysis and incorporated tools including Pandas, Scikit-Learn, Imbalanced-Learn, Tensorflow, Seaborn, and others. See the requirements.txt in this repository for a comprehensive list. PostgreSQL in combination with AWS was used for data storage.

## Dashboard
Tableau was used to present our analysis. 

The dashboard displays the finding on using different types of white and red wines to predict the quality of wine.
The total number of wines used for this analysis is 5,320. White is 3,961 and  red wine is 1,3569. 
The quality percentage shows percentages of the wine type that is good or bad, From the visualization, more than 95% of the red and white wine fall under the bad wine. The quality of wine is rated from zero (0) to ten (10), six (6) below classified as bad wine and seven (7) and above as good wine.

The Wine Types Features show how some of the features used for the prediction relate to the wine types. Total sulfur dioxide in white is twice as much as that of red wine. The fixed acidity of the red wine is higher than that of the white wine and the residual sugar of the white wine is higher than the residual sugar in red wine. There is no significant difference in the other features such as chlorides, density, PH, citric acid, quality, and alcohol, in red and white wine.


<figcaption align = "center"><b>Wine Type Prediction Dashboard</b></figcaption><img src="media/Dashboard.png" >

- Quality Percentage:
  - Bad Red Wine - 98.5%
  - Good Red Wine 1.25%
  - Bad White Wine - 96.57%
  - Good White Wine - 3.43%

- Alcohol Variance
  - Good White Wine - 1.11
  - Good Red Wine - 1.51
  - Bad White Wine - 1.43
  - Bad Red Wine - 1.13

- Wine Types & Features
- There is a difference between wine types in Average Fixed Density and Average Residual Sugar but there is not a significant difference for wine types in Average Alcohol, Average Density, Average Quality, Average Sulphate, and Average Density.

## Database

### Entity Relationship Diagram (ERD) 

- Our diagram consists of four tables (shown below).  The red and white datasets have all columns in common and were joined via union to create a combined dataframe (wine).  The fourth table is real world data to test our model in future analysis. 

<figcaption align = "center"><b>Entity Relationship Diagram</b></figcaption><img src="media/project_erd.png" width = 700 >


Violin Plots reflecting our 3 highest features within our model.

![alcohol](https://user-images.githubusercontent.com/105396400/194209174-17dc8314-805d-4498-b3f0-0abc5ec61ee2.png)

We can clearly see that at the alcohol content goes up for both red and white, so too does the quality.

![chlorides](https://user-images.githubusercontent.com/105396400/194209211-c3f752ea-9ecd-4dbf-a52f-765e426f2351.png)


Overall, white wines contain a lower level of chlorides than red wines on the maagnitude of ~.05.

![density](https://user-images.githubusercontent.com/105396400/194209227-e38b9677-551f-4a5d-9ec3-d26a0d8ce299.png)

White wines generally run less dense than reds. In addition, the concentration of the density of a red wine is more defined to smaller range than white wines.

### Future Analysis

In the next stage of the project, we will test our models against real world data.

Other questions we would like to answer:

- Can we expand our type model to predict wine types from the five major categories? 
  - Rose
  - Dessert
  - Sparkling
  - Red
  - White

- Does ranking the wine as good or bad based on physicochemical properties differ from an opinion based rating?

## Project Links

[Tableau](https://public.tableau.com/app/profile/fidelia1205/viz/WineQuality_16646390058510/WineQualityPrediction?publish=yes)

[Google Slides](https://docs.google.com/presentation/d/1l_aqOm0VKkD-InpUv269a5dMCSIgSu_PcFoH5URTsb8/edit?usp=sharing)

## Contributors

- [Heather Harrah-Lea](https://github.com/mik-moo)

- [Mia Goodwin](https://github.com/MLGood1)

- [Fidelia Akparu](https://github.com/Fakparu)

- [Andrew Taylor](https://github.com/aotreaux)
