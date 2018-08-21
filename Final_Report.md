## Predicting Health Plan Tier Enrollment
*Jonathan King*
<br>
<br>
**The Problem:** 
<br><br>
The creation of the `Affordable Care Act (ACA)` led to nationwide reform of the United States healthcare system.  The `ACA` requires insurance issuers, who offer coverage through the Health Insurance Marketplace, to offer insurance plans based on a `metallic tiered system`.  These tiers include Catastrophic (which is only available to those who meet specific requirements), Bronze, Silver, Gold, and Platinum.   Each `metallic tier` is hierarchical with Catastrophic plans having the lowest monthly premium and highest potential `cost-share` and Platinum plans having the highest monthly premium and lowest potential `cost-share`. 

More information about the `ACA’s` `metallic tier system` can be found [here](https://www.healthcare.gov/choose-a-plan/plans-categories/).

Insurance, in any market, is primarily focused on risk and risk reduction.  Consumers purchase insurance to reduce their financial risk should unexpected events leading to financial loss occur.  Likewise, insurance issuers reduce their financial risk by focusing on being able to make predictions about their consumers and their claims.  These predictions enable insurance issuers to set a monthly premium that is competitive with other issuers and will most likely generate a profit.  In health insurance, these predictions also enable the company to proactively reduce high cost claims by offering programs and education to help reduce or control chronic illness.  

Subsequently, this project will focus on predicting the `metallic tier` of a county based on its demographics.
<br>
<br>
**The Client:** 
<br><br>
Our client in this project is a health insurance company.  With the ability to predict the `metallic tier` selections in each county, the company can create a tier focused marketing campaign.  Moreover, the company may also use the `metallic tier` predictions to create county-based health management and education programs.    

`Metallic tiers` can be useful in making health predictions, as a higher tier choice likely indicates a consumer who either has known chronic health issues or expects health related expenses (i.e. pregnancy).  A lower tier choice likely indicates a consumer who does not have any known chronic health issues or does not expect any upcoming health related expenses.  These likelihoods arise from the monthly premium to consumer `cost-share` relationship built into the `metallic tiered system`.
<br>
<br>
**The Data:**
<br><br>
CMS has provided open data to www.data.gov that contains up to 14 data sets regarding this analysis.  

The data used in this analysis includes the following:  

-  Seven datasets of demographic data by county for plan selections in 2016.  Demographics include Cost-Share Reduced Plans, Age Group, Advanced Premium Tax Credit, `Metallic Tier`, Ethnicity, Household Income as a Percent of the Federal Poverty Level, and Type of Consumer.

-  Seven datasets of demographic data by county for plan selections in 2015.  Demographics include Cost-Share Reduced Plans, Age Group, Advanced Premium Tax Credit, `Metallic Tier`, Ethnicity, Household Income as a Percent of the Federal Poverty Level, and Type of Consumer.

All datasets can be found [here](https://data.cms.gov/browse?category=Marketplace%20-%20Qualified%20Health%20Plan%20(QHP)).

CMS has yet to provide additional datasets involving plan choices.  However, I suspect additional datasets may be produced for plan years 2017 and 2018 in the future.
<br>
<br>
**Crafting the Datasets:**
<br><br>
This project required a single primary dataset to be crafted for model building and analysis.  Once this single primary dataset was crafted two additional datasets were derived from it.  The three datasets include the `final raw counts dataset`, `targets as percent of tps dataset`, and the `features as percent of tps dataset`.  Prior to creating these final datasets, two datasets based on year were crafted.
<br><br>
To begin, all the individual `2015 datasets` were read into multiple DataFrames.  A list containing the appropriate column names was created and then referenced to assign column names to each `2015 dataset`.  Finally, the `total plan selections (tps)` feature was dropped for all but one dataset, and the individual `2015 datasets` were merged together on `county`, `state`, and `county FIPS code` to create the singular `merged 2015 dataset`.
<br><br>
Subsequently, after being merged, the next step was to clean the data.  Null values were represented as "." or NaNs.  To get an idea of how many NaN values were in the data, each feature was converted to numeric values.  After which, any observation with more than 10 NaN values was dropped.  Ten NaN values was arbitrarily chosen as it represented 1/4 of the 40 features within the data.  After the observations above the threshold were dropped, the remaining NaN values were filled with 0.  Zero was chosen as several of the NaN values were NaN because there were no selections for that feature.  Other methods to replace NaN values such as using the average, using the previous value, or the following value, were not appropriate as each county could range from less than 100 `tps` to tens of thousands of `tps`.
<br><br>
The process described above was used in the creation of the `merged 2016 dataset`.  As such, its creation process will not be further expounded upon.
<br><br>
The `final merged dataset` was now ready to be created.  To begin, each `dataset by year` was given an additional feature indicating which year the observation was taken.  Then the datasets were concatenated together into the `final merged dataset`.   Once concatenated, a feature that was present in the `merged 2016 dataset`, but not present in the `merged 2015 dataset` required some engineering.  The feature in question was an additional percent above the federal poverty level.  This feature represented a bin and was easily engineered to be added into the bin that existed for both merged datasets.  Finally, two observations were found with mostly NaN and 0 values; these two observations were removed and the `final raw counts dataset` was complete.
<br><br>
Wrapping up, the `targets as percent of tps dataset` was crafted by dividing the raw counts of each `metallic tier` and dividing it by the observation's `tps`.  The same process was used on every numeric feature in the `features as percent of tps dataset`.  It is important to note the `features as percent of tps dataset` was ultimately rejected as a useful dataset.  This is due to the primary objective of the project, which is to predict Metallic Tier choices on a county level, not making sense with the `features as percent of tps dataset`.  When acquiring new data about a county, our client would not receive it as a percent of an unknown `tps`.  As such, any predictions made using this dataset, wouldn't be helpful or useful to the client.
<br><br>
The code used to create and clean every dataset can be found [here](https://github.com/jsking751/Capstone_2/blob/master/Data%20Munging/Data_Munging_Merging.ipynb).
<br><br>
**Findings:** 
<br><br>
*Exploratory Data Analysis (EDA)*
Some important initial findings were made through initial EDA.  The most important being that the major difference between each year was an increase in overall enrollment, and not in the distribution of key features.  Figures 1-1 through 1-3 show the similarities of the feature distributions for 2015 and 2016.  The similarities in distribution among plan years was encouraging as it implies applicability towards predicting future `plan selections`, which is the whole purpose of this project!

![Figure 1-1](https://github.com/jsking751/Capstone_2/blob/master/Figures/age_dist.png "Age Distribution")
![Figure 1-2](https://github.com/jsking751/Capstone_2/blob/master/Figures/aid_dist.png "Financial Aid Distribution")
![Figure 1-3](https://github.com/jsking751/Capstone_2/blob/master/Figures/income_dist.png "Income Distribution")

An important distribution for our analysis was that of our target values, the `metallic tiers`.  As seen in Figure 2-1, the `Silver Tier` dominated as the most selected `tier`. Furthermore the `Catastrophic Tier` never reached above 1% of `total plan selections`.  Due to the low `tps` and highly regulated nature of the `Catastrophic Tier` (only specific portions of the population can even choose this `tier`), building a prediction model around this `tier` seemed highly impractical.

![Figure 2-1](https://github.com/jsking751/Capstone_2/blob/master/Figures/tier_dist.png "Metallic Tier Distribution")

The code used for initial EDA can be found [here](https://github.com/jsking751/Capstone_2/blob/master/EDA/EDA.ipynb).

With EDA complete, it was time to build prediction models.
<br><br>
*Machine Learning Models*
<br><br>
Several machine learning models and algorithms were tested.  Those models include:
-  [Linear Regression](https://github.com/jsking751/Capstone_2/blob/master/Machine%20Learning/Machine_Learning_Linear_Regression.ipynb)
-  [Logistic Regression](https://github.com/jsking751/Capstone_2/blob/master/Machine%20Learning/Machine_Learning_Logistic_Regression.ipynb)
-  [Random Forests](https://github.com/jsking751/Capstone_2/blob/master/Machine%20Learning/Machine_Learning_RandomForests.ipynb)
-  [Deep Learning with Keras](https://github.com/jsking751/Capstone_2/blob/master/Machine%20Learning/Machine_Learning_Keras.ipynb)<br>   

Ultimately two best models were chosen.
<br><br>
The first was the `Linear Regression` model using the `Huber` algorithm on the `final raw counts dataset`. The model worked best on the `Bronze`, `Silver`, and `Gold` `Tiers`.  The model produced a 5-Fold CV R^2 of 0.987, 0.999, and 0.911 respectively.  Furthermore, the Root Mean Squared Errors were 346, 311, and 195.  While these RMSEs may sound a bit high, the raw counts could be in the tens of thousands, as such the RMSE was quite acceptable.  To further illustrate the accuracy of these models please review Figure 3-1 below.

![Figure 3-1](https://github.com/jsking751/Capstone_2/blob/master/Figures/bsg_predictions.png "Predicted Counts vs Actual Counts")

The second model chosen was a multinomial hyperparameter tuned `Random Forest Classifier` on the `targets as percent of tps dataset`.  This model worked best for the `Platinum Tier`.  The model produced a training accuracy of 0.968 and a test accuracy of 0.963.  It is important to note, that the `percent of tps` feature had to be further binned into bins of 5 percent i.e. 0 represented 0-4%, 1 represented 5-9%, ect. to get these results.  Several classifiers were tested and the `Random Forest Classifier` came out on top.  A review of the average ROCs for each classifier can be found in Figure 4-1.  The plot only shows the average ROC for each classifier as each classifier was multinomial.

![Figure 4-1](https://github.com/jsking751/Capstone_2/blob/master/Figures/ROC_Curves.png "Average ROC Curves by Model")

The code used for the best prediction models can be found [here](https://github.com/jsking751/Capstone_2/blob/master/Machine%20Learning/Machine_Learning_Best_Models.ipynb).
<br>
<br>
**Conclusion:**
<br><br>
After several model iterations I was able to find predictive models for each `metallic tier`.  While the `Catastrophic Tier` was dropped, building a successful model is well within the realm of possibility using what I've learned.
<br><br>
I believe using a classification model for the `Platinum Tier` was more successful due to the low selection amount and reduced variability.  Since many counties had 0 selections for the `Platinum Tier`, binning the available selections into a classification model enhanced its predictability.  Conversely, the remaining `metallic tiers` fared better in a regression model due to their high amount of variability and overall selection amount.  With proper binning for 'low', medium', and 'high' priorities, a successful multinomial classification model could be built for these `tiers`.  Said binning would need to be at the discretion of the client.
<br><br>
In conclusion, this project shows the ability to predict `tier` counts and/or percent of `tps` for the individual `tiers` that are available on the Federal Marketplace.
<br>
<br>
**Recommendations and Next Steps:**
<br><br>
*Bootstrapping for the Random Forest Classifier Mode*
<br><br>
While the average ROC for the Random forest model appears very useful, further inspection shows the individual classifiers within the multinomial model leave much to be desired.  A visual of each classifier's ROC is available in Figure 5-1.  Moreover, additional testing and training with bootstrapping could prove to build a more accurate model.

![Figure 5-1](https://github.com/jsking751/Capstone_2/blob/master/Figures/ROC_by_Class_RF.png "ROC by Class: Random Forest Model")
<br>
*Dive Deeper Into Deep Learning*
<br><br>
Preliminary models with Kears did not produce significant results over the existing models.  Given enough time, and other deep learning algorithms, a useful model could potentially be built for every available `tier`.  That said, the model may require extensive data engineering and time to produce meaningful results: time and energy that could be used strengthening the already existing models.
<br><br>
*Get More Data*
<br><br>
While CMS has yet to release more data regarding county plan selections, the client may have proprietary data on plan selections that could be used to supplement the current models.  More county observations are likely to produce models that make even better predictions.
<br><br>
*Investigate Feature Effects on Plan Selections*
<br><br>
Some initial feature investigation shows that each `tier` is predicted by a different assortment of features.  Further investigation into these relationships could lead to insights that may help the client create `tier options` that are specially tailored to consumers most likely to choose said `tier`.  Figures 6-1 to 6-3 display some of the feature evaluation already completed in this project.
<br><br>
![Figure 6-1](https://github.com/jsking751/Capstone_2/blob/master/Figures/silver_lasso.png "Silver Feature Importance (Lasso)")
![Figure 6-2](https://github.com/jsking751/Capstone_2/blob/master/Figures/silver_forest.png "Silver Feature Importance (Random Forest)")
![Figure 6-3](https://github.com/jsking751/Capstone_2/blob/master/Figures/bronze_lasso.png "Bronze Feature Importance (Lasso)")