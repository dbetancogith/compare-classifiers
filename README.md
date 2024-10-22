### Conclusions:
### Data:
The dataset collected is related to 17 campaigns that occurred between May 2008 and November 2010, corresponding to a total of 79354 contacts. During these phone campaigns, an attractive long-term deposit application, with good interest rates, was offered. For each contact, a large number of attributes was stored and if there was a success (the target variable).
The target variable shows an unbalanced class, which is usually the case for campaigns.
There are not null values on the dataset.

### Business Objective:
The business objective is to increase the efficiency of phone campaigns for long-term deposit subscriptions by reducing the number of contacts, while identifying the key characteristics that influence success. This will enable better management of available resources—such as human effort, phone calls, and time—by focusing on a high-quality, cost-effective campaigns set of potential customers.

### Engineering Features:
Started with JamesSteinEncoder because of how it manages features with many unique values (i.e month), however, it gives lower performance for classification problems. 
Switch to TargetEncoder has similar features but it is better for binary classification. Noticed that the performance and metrics are better with TargetEncoder.
The transformer below encodes and scaled the data.

### Train/Test Split
Used test size of 40% given the large number of records (41188), the size of the test data also affects the results. Thus the larger the test data the lower the scores for the same models.

### First Iteration: A Baseline Model
Used a DummyClassifier for the baseline with accuracy of **0.8853**

### Second Iteration: A Simple Model
Use Logistic Regression to build a basic model on your data with accuracy of **0.91**

### Third Iteration: Model Comparisons
Build and fit 4 models: Logistic Regression, KNN, Decision Tree, and SVM using the default parameters.
**Results:*
*Best Score: LogisticRegression
*Best Fit Time: KNeighborsClassifier

![Comparing models.png](attachment:82b7d0cb-66ee-403b-8182-0b1151d9867a.png)

### Fourth Iteration: Improving the Model - Tuning Parameters
1. The remaining features, after gender, will stay in the dataset for two major reasons: First, as shown in the heapmap graph below, there is interdependence among the features, they are correlated. Second, the features do not have a normal distribution on the values, please see the graph for "education" below when the campaigns were successful.
2. However, given the size of the dataset I am using SelectFromModel with LogistRegression to get the most important features for the classification work with a reduced number of features. This is done to reduce the computational cost, specially for SVC the process does not end with all records in the dataframe.
3. As a result the scores will be a bit lower, however, still valid and can formulate conclusions.
**Results:**
*Best Score: Decision Tress
*Best Fit Time: KNeighborsClassifier

![Tuning Params.png](attachment:b11f3a7f-d905-405a-b10c-3ee4d43d1add.png)

**Best Parameters:**
![Best Params.png](attachment:503129e7-824f-4f8e-95c4-d7eb179605fa.png)

### Adjust your performance metric
So far fit time, accuracy, precision, and recall  are the metrics to compare the 4 models. Now Lift and ROC will be used adjusting the metrics.
Cumulative Gain Curves, or **Lift Charts**, are considered a better metric for marketing campaigns in classification models for several reasons, especially in scenarios where the objective is to target the most responsive or valuable segments of a population.

**Marketing campaigns, usually unbalanced**,  typically aim to optimize limited resources by identifying and targeting the most promising prospects (e.g., potential customers).

**The Cumulative Gain Curve** shows how well the model can rank instances (customers) in order of their likelihood to respond positively (e.g., make a purchase).

It helps marketers identify the smallest segment of the population that delivers the highest returns, and at the end the Cumulative Gain Curve directly addresses the business goal—targeting the right group of customers.

On the other hand **The ROC Curve** (Receiver Operating Characteristic curve) is a useful metric for marketing campaigns in classification models because it helps evaluate the model's ability to distinguish between classes (e.g., potential buyers vs. non-buyers) across different decision thresholds.

In this case Lift Charts are showing for example at the point 0.1 (10%), 0.5 (50%), meaning that if you score a dataset with the network and sort all of the cases by predicted probability of Yes, you would expect the top 10% to contain approximately 50% of all of the cases that actually take the category Yes, in the same way the top 30% of cases would contain >90% of sucessful calls, and so on. If you select 100% of the scored dataset, you obtain all of the successful clients in the dataset. Thus, using the model Logistic Regression can reduce the number of calls by 30% and get better results. These graphs look very similar because of the large number of records in the dataframe.

**The cumulative gain curves and ROC curves show the Logistic Regression as best model with AUC of 0.88 and 0.93.**

![Lift and ROC Graphs.png](attachment:573bb3c8-564e-452b-ac1b-bf75626972eb.png)

Next, **the lift curve* is derived from the cumulative gains chart; the values on the y axis correspond to the ratio of the cumulative gain for each curve to the baseline. Thus, the lift at 10% for the category Yes is 50%/10% = 5.0. It provides another way of looking at the information in the cumulative gains chart.

![Lift curve.png](attachment:8af7fdab-ba84-45de-8683-68551a28beb1.png)

Plotting **the calibration curves** of a classifier is useful for determining whether or not you can interpret their predicted probabilities directly as a confidence level. For instance, a well-calibrated binary classifier should classify the samples such that for samples to which it gave a score of 0.8, around 80% should actually be from the positive class.

![Calibration Plots.png](attachment:f4ad90c4-c606-4e2d-8a9f-d34003626c1f.png)

Finally **the confusion matrix** for each model shows the values for TP, FP and FN, in this case the idea is to increase the number of people in the campaign that will say Yes and reduce the number of calls.

![Confusion Matrix.png](attachment:a7418ece-8667-4ef6-ab43-675ff958adac.png)

### Next Steps:
1. Redefine further the parameters, due to the large dataset it takes quite some time to run, specially SVC - maybe get a better processing in GO 
2. Run the tuned models with the complete list of features
3. Metrics can be tune/redefine to get better understanding of the models outputs
4. Replace the GridSearchCV with HalvingRandomSearchCV or RandomizeSearchCV and compare the results