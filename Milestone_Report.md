### Capstone Milestone Report: Predicting Enrollment: Developing a County Centered Marketing Strategy  ###

*Jonathan King*

***
**The Problem:** 

Insurance, in any market, is primarily focused on risk and risk reduction.  Consumers purchase insurance to reduce their financial risk should unexpected events leading to financial loss occur.  Likewise, insurance issuers reduce their financial risk by focusing on being able to make predictions about their consumers and their claims.  These predictions enable insurance issuers to set a monthly premium that is competitive with other issuers and will most likely generate a profit.  In health insurance, these predictions also enable the company to proactively reduce high cost claims by offering programs and education to help reduce or control chronic illness.  

Subsequently, this project will focus on predicting the `metallic tier` choices of a county based on its demographics.

***
**The Client:** 

The client in this project is a health insurance company.  With the ability to predict the `metallic tier` selections in each county, the company can create a tier focused marketing campaign.  Moreover, the company may also use the `metallic tier` predictions to create county based health management and education programs.    

`Metallic tiers` can be useful in making health predictions as a higher tier choice likely indicates a consumer who either has known chronic health issues, or expects health related expenses (ie. pregnancy).  A lower tier choice likely indicates a consumer who does not have any known chronic health issues or does not expect any upcoming health related expenses.  These likelihoods arise from the monthly premium to consumer `cost-share` relationship built into the `metallic tiered system`.

***
**The Available Data:** 

CMS has provided open data to www.data.cms.gov that contains up to 14 data sets regarding this analysis.  

*Data Sets Include:*  

-  Seven datasets of demographic data by county for plan selections in 2016.  Demographics include Cost-Share Reduced Plans, Age Group, Advanced Premium Tax Credit, `Metallic Tier`, Ethnicity, Household Income as a Percent of the Federal Poverty Level, and Type of Consumer.

-  Seven datasets of demographic data by county for plan selections in 2015.  Demographics include Cost-Share Reduced Plans, Age Group, Advanced Premium Tax Credit, `Metallic Tier`, Ethnicity, Household Income as a Percent of the Federal Poverty Level, and Type of Consumer.

All datasets can be found [here](https://data.cms.gov/browse?category=Marketplace%20-%20Qualified%20Health%20Plan%20(QHP)).

This was all the available data for plan selections provided by CMS.  Plan selection data had likely been collected by individual health plans, but that data does not appear to be open for public use. 

***
**Crafting the Datasets:**

Three datasets were crafted for this analysis.  The first two datasets are the seven smaller plan selection datasets merged together to create `datasets by year`.  The third dataset is a merger of all 14 plan selection datasets together to create a `combined dataset`.  The `combined dataset` was crafted to maximize the total number of observations available for the machine learning model.

After initial investigation of the individual datasets, it was determined that all observations for each smaller dataset shared the same index.  As such, concatenation to create the `datasets by year` presented itself as the appropriate first step.  To do this, identical columns were dropped from all but one dataset.  Then, the datasets were concatenated together by row.

Subsequently, after being concatenated, the data needed to be cleaned.  Null values were represented as '.' in all datasets.  These Null values were converted to NaN values by applying Pandas' to_numeric method to the appropriate categories.   Null values were expected in the data as not all categories would be represented in individual counties.  However, enough data for each county would be required to make valuable predictions.  As such, a maximum threshold of 10 NaN values per observation was chosen.  This maximum threshold was chosen as 10 NaN values represented 25% of the available features in an observation.  All observations with NaN values above the threshold were dropped.

The final step for each `dataset by year` was to convert all NaN values to 0.  The 0 value was chosen as it is the most likely real value of the feature.  As previously stated, not all demographics would be represented in individual observations.  A simple fillna method was used to achieve this end.

Finally, it was time to create the `combined dataset`.  The first step was to add a 'Year' feature to each `dataset by year`, and then concatenate the two cleaned datasets together by observation.  Next, an additional `2016 dataset` feature not included in the `2015 dataset` needed to be cleaned.  To do this the NaN values for `2015` observations were converted to 0 and the `2016` feature was added to the `2015` feature that encompassed the `2016` feature (as the features were a part of a range).  The `2016` feature was then dropped from the `combined dataset`.     

All the code used for cleaning and concatenating the three datasets can be found [here](https://github.com/jsking751/Capstone_2/blob/master/Data%20Munging/Data_Munging_Merging.ipynb).


***
**Initial Findings:** 

After applying exploratory data analysis and machine learning, multiple discoveries were found.  Most importantly, that distributions among categories did not change drastically from year to year.  This is important as it will help to ensure the predictive model can operate for future years.

All code for the exploratory data analysis can be found [here](https://github.com/jsking751/Capstone_2/blob/master/EDA/EDA.ipynb).

The following figures show distribution comparisons for age groups, financial aid, and income (Figures 1-1, 1-2, and 1-3).  As shown in the figures some variation between years did occur.  The only significant change was with income distribution.  However, that was due to the inclusion of a new feature within the data of the `2016 dataset`. <br>

![Figure 1-1](https://github.com/jsking751/Capstone_2/blob/master/Figures/age_dist.png "Figure 1-1")
![Figure 1-2](https://github.com/jsking751/Capstone_2/blob/master/Figures/aid_dist.png "Figure 1-2")
![Figure 1-3](https://github.com/jsking751/Capstone_2/blob/master/Figures/income_dist.png "Figure 1-3")

The distribution of `metallic tiers` was also plotted.  The `Silver tier` was by far the most chosen tier.  This is not surprising as the `Silver tier` is the center point for all reduced cost-share and advance premium tax credit (APTC) plans.  Figure 2-1 displays the count distribution while Figure 2-2 displays the distribution of each tier by percent. <br>

![Figure 2-1](https://github.com/jsking751/Capstone_2/blob/master/Figures/tier_dist.png "Figure 2-1")
![Figure 2-2](https://github.com/jsking751/Capstone_2/blob/master/Figures/metallic_pie.png "Figure 2-2")


Linear Regression was the choice of model for this project.  I am seeking quantitative values of the `metallic tiers` for each county observation.   Furthermore, my EDA shows that not all features in the data will be helpful for regression.  As such, I have tried, and will continue to try, different models including Elastic Net, Ridge, Huber Regressor, and Lasso. Different scaling options will be tested on the model as well. R^2 and Root Mean Squared Error will be the metrics for the models. 

All the code used for machine learning can be found [here](https://github.com/jsking751/Capstone_2/blob/master/Machine%20Learning/Machine_Learning.ipynb).

Using Lasso for feature evaluation produced some interesting insights.  Each tier appears to be affected by different features.  Figures 3-1 to 3-5 demonstrate the differences for each `metallic tier`.  To briefly summarize:

-   `Catastrophic Tier`: Young Age Group with No APTC (as is the design of Catastrophic Plans)
-   `Bronze Tier`: Older Age Group, Low Income, Various Enrollment Types
-   `Silver Tier`: Various Age, Financial Aid, Income, and Enrollment Types
-   `Gold Tier`: Younger Age, No APTC, Auto Renewal Enrollment
-   `Platinum Tier`: Older Age Group (pre-retirement age), Lower-Middle Income Level, New and Automatic Enrollment Typ<br>


![Figure 3-1](https://github.com/jsking751/Capstone_2/blob/master/Figures/cat_lasso.png "Figure 3-1")
![Figure 3-2](https://github.com/jsking751/Capstone_2/blob/master/Figures/bronze_lasso.png "Figure 3-2")
![Figure 3-3](https://github.com/jsking751/Capstone_2/blob/master/Figures/silver_lasso.png "Figure 3-3")
![Figure 3-4](https://github.com/jsking751/Capstone_2/blob/master/Figures/gold_lasso.png "Figure 3-4")
![Figure 3-5](https://github.com/jsking751/Capstone_2/blob/master/Figures/plat_lasso.png "Figure 3-5")

Finally, a baseline Linear Regression model was built along with a Lasso and ElasticNet model.  As seen in Figures 4-1 to 4-3, the Lasso model with hyperparameter tuning appears to be the best model thus far.  However, this model by no means has a high enough R^2 or low enough Root Mean Squared Error to be satisfying.  <br>
***
**Linear Regression Model:**  
![Figure 4-1](https://github.com/jsking751/Capstone_2/blob/master/Figures/lin_model.PNG "Figure 4-1")
***
**Lasso Model:**  
![Figure 4-2](https://github.com/jsking751/Capstone_2/blob/master/Figures/lasso_model.PNG "Figure 4-2")
***
**ElasticNet Model:**  
![Figure 4-3](https://github.com/jsking751/Capstone_2/blob/master/Figures/enet_model.PNG "Figure 4-3")
***

In conclusion, more models need to be built and tested before a useful predictive model can be created.  Some ideas to improve my metrics include:
-   The trial of different Linear Regression models.
-   The use of Multiclass Logistic Regression models.
-   The use of a Deep Learning model like TensorFlow.
-   Reconfiguring the target data from aggregated sum to percent of the observations total plan selection.
-   Different Scaling and Preprocessing techniques. 