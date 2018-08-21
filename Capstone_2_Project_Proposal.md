### Predicting Enrollment: Developing a County Centered Marketing Strategy 
*Jonathan King*
<br>
<br>
<br>
**1. What is the problem you want to solve?:** 

The creation of the `Affordable Care Act (ACA)` lead to nationwide reform of the United States healthcare system.  The `ACA` requires insurance issuers, who offer coverage through the Health Insurance Marketplace, to offer insurance plans based on a `metallic tiered system`.  These tiers include Catastrophic (which is only available to those who meet specific requirements), Bronze, Silver, Gold, and Platinum.   Each `metallic tier` is hierarchical with Catastrophic plans having the lowest monthly premium and highest potential `cost-share` and Platinum plans having the highest monthly premium and lowest potential `cost-share`. 

More information about the `ACA’s` `metallic tier system` can be found [here](https://www.healthcare.gov/choose-a-plan/plans-categories/).

Insurance, in any market, is primarily focused on risk and risk reduction.  Consumers purchase insurance to reduce their financial risk should unexpected events leading to financial loss occur.  Likewise, insurance issuers reduce their financial risk by focusing on being able to make predictions about their consumers and their claims.  These predictions enable insurance issuers to set a monthly premium that is competitive with other issuers and will most likely generate a profit.  In health insurance, these predictions also enable the company to proactively reduce high cost claims by offering programs and education to help reduce or control chronic illness.  

Subsequently, this project will focus on predicting the `metallic tier` of a county based on its demographics.
<br>
<br>
<br>
**2. Who is your client and why do they care about this problem?:** 

Our client in this project is a health insurance company.  With the ability to predict the `metallic tier` selections in each county, the company can create a tier focused marketing campaign.  Moreover, the company may also use the `metallic tier` predictions to create county based health management and education programs.    

`Metallic tiers` can be useful in making health predictions as a higher tier choice likely indicates a consumer who either has known chronic health issues, or expects health related expenses (ie. pregnancy).  A lower tier choice likely indicates a consumer who does not have any known chronic health issues or does not expect any upcoming health related expenses.  These likelihoods arise from the monthly premium to consumer `cost-share` relationship built into the `metallic tiered system`.
<br>
<br>
<br>    
**3. What data are you going to use for this? How will you acquire this data?:**

CMS has provided open data to www.data.gov that contains up to 14 data sets regarding this analysis.  

Data Sets Include:  

-  Seven datasets of demographic data by county for plan selections in 2016.  Demographics include Cost-Share Reduced Plans, Age Group, Advanced Premium Tax Credit, `Metallic Tier`, Ethnicity, Household Income as a Percent of the Federal Poverty Level, and Type of Consumer.

-  Seven datasets of demographic data by county for plan selections in 2015.  Demographics include Cost-Share Reduced Plans, Age Group, Advanced Premium Tax Credit, `Metallic Tier`, Ethnicity, Household Income as a Percent of the Federal Poverty Level, and Type of Consumer.

All datasets can be found [here](https://data.cms.gov/browse?category=Marketplace%20-%20Qualified%20Health%20Plan%20(QHP)).
<br>
<br>
<br>
**4. In brief, outline your approach to solving this problem:**

1.	I intend to merge the data for each year into two data frames.  I will also munge the data in this step to ensure it is tidy for further analysis.

2.	I will run basic EDA and inferential stats on the data to observe the types of distributions and existing relationships within the data.

3.	I will create multiple supervised machine learning models using the 2016 data.  My target data will be the `metallic tier`.  

4.	I will test my model using the 2015 data.  If needed I will incorporate the 2015 observations into my model should more accuracy be needed.    
<br>
<br>

**5. What are your deliverables?:**

My deliverables will include a narrative of the project in the form of a paper, and the code, which will be made available as part of a GitHub repository.  I will also include a PowerPoint slide deck.
