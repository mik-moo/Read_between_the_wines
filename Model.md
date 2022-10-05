# Model Design

## What model and why?

* We will build an unsupervised learning model with a clustering algorithm.  We chose this model because it is the best option for grouping the red and white wine data to predict the quality score of the next wine.

* We will split, train, and test the model by fitting the model with the trained wine data.

* We will then add new wine data and see if the model can predict the quality of the new wine data set.

* We will try to obtain a 80% accuracy rating for this model.

* We expect this model to predict a quality rating of 0 to 10 with an 80% accuracy.


### Preprocessing

After some road blocks with our data, we slightly changed our question; Can we predict "Good" wine?  During exploratory data analysis (EDA) we realized that the features that were important for quality between the two wine types were very different.  Our clustering wasn't effective when trying to determine the quality rating 0-10.  Our group chose to rank the wines as "Good" or "Bad" by applying conditions.  Preprocessing included:

* The red and white data sets were combined and read into the notebook.

* Duplicates were dropped.

* Wines with a rating of greater than or equal to 7 were classified as "Good" and less than 7 were given a "Bad" rating.

* Features and target (quality) were defined.

* Data was split into training and testing.

* A standard scaler was used to scale the data.

### Model

* Scaled data was fit to a RandomForest Classifier.

* Predictions were made, a confusion matrix was generated, and classification report run.

<figcaption align = "center"><b>Quality Prediction Confusion Matrix</b></figcaption><img src="images/quality_metrics.png" >

* Features were ranked by importance for this model.

<figcaption align = "center"><b>Quality Prediction Feature Importance</b></figcaption><img src="images/quality_features.png" >

* Our original goal for sensitivity was 80%.  Our current model will be accurate a projected ~85% of the time.  It is more sensitive for the detection of "Bad" wines at 95%, but the low score(~40%) may indicate some false negatives in the "Good" category.  The F1-score is supportive of using this model with a higher score of 83%.

We chose to use supervised learning because our data was labeled and had a smaller amount of variables.  We chose a classification model to label our output as "good" or "bad".  The only statistics calculated in this project are the classification report and include accuracy, precision, sensitivity, and F1. We also evaluated feature correlation during EDA.

### Additional Analysis

When we noted the differences between the wine types as related to quality, it was suggested that we may be able to predict the wine type based upon the physiochemical feature differences.

* Preprocessing was performed as in the above model with 2 exceptions:

  * The target was "type"

  * Conditions were not applied to determine "good" or "bad".

* The model was the same as above. 

<figcaption align = "center"><b>Wine Type Prediction Confusion Matrix</b></figcaption><img src="images/type_metrics.png" >

<figcaption align = "center"><b>Wine Type Prediction Feature Importance</b></figcaption><img src="images/type_features.png" >

* This model will be accurate a projected ~99% of the time.  It rates high for sensitivity, precision, and F1-score. With a model rating this high, there was a concern for data leakage skewing the model.  A correlation table was generated and the highest correlation seen was ~69% which did not indicate that data leakeage was present.

<figcaption align = "center"><b>Wine Type Prediction Feature Importance</b></figcaption><img src="images/type_corr.png" >

In the next stages of the project, we will test to see if our model works on real world data.