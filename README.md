# NYC-Restaurant-Cleanliness-R-Shiny-Project
This is a four-people-team-based project. I have contributed 35% for this project. 

This project produced two files:
1. The analysis file;
2. Reporting engine file which can generate visulization dashboard.

#1. The analysis file;
## Introduction

For anyone who has lived in New York City knows that New York is not the cleanest city. We were intrigued to learn more about the cleanliness of the restaurants in New York City restaurants and the process that goes behind grading the restaurants. Currently, most of the applications that provide customers with information about restaurants do not provide information on the food hygiene of the restaurants, and hence customers have limited awareness of the health and food hygiene problems especially of good quality restaurants. We believe that there should be more information or applications provided to customers with detailed information on the cleanliness of the restaurants. Our goal is to build an application that will provide customers with information on the cleanliness of the restaurants, and our application can also be used by inspection officers to better plan their inspection visits. Food hygiene is closely related to food safety and the health of customers, so our project intends to help minimize illnesses caused due to improper food.

Our study is based on the new york city restaurant inspection results. We are mainly interested in understanding the trends of the violations across restaurants in NYC, including the relationship of violations with boroughs, inspection dates, and cuisines, also the year trends and association among violations. Understanding the trends in the violations will help restaurants take precautions to avoid the violations, and it will also provide inspection officers with additional insights on the violations trends across the city. 


## About the data

We sourced the data from NYC open data. [https://data.cityofnewyork.us/Health/DOHMH-New-York-City-Restaurant-Inspection-Results/43nn-pn8j]. The data is provided by the Department of Health and Mental Hygiene and hence considered reliable. The data is updated daily which ensures that the data is accurate and is representative of the current situation. The data consists of inspection results for restaurants and college cafeterias in New York City from the year 2016- 2019. The data only consists of currently active restaurants and all restaurants can be identified by a unique Camis(Restaurant id). Each record is one citation of violation (If a restaurant received more than one violation then the associated fields are repeated with every additional violation). The data also includes restaurants that have applied for a permit but have not been inspected yet.

Additionally, some of the other  important variables in the dataset are listed below:

**DBA:** Name of the restaurant

**Boro:** Name of the borough that the restaurant is located in

**Zipcode:** Zip Code of the location of the restaurant

**Cuisine Description:** The cuisine served by the restaurant

**Inspection date:** The date of the inspection of the restaurant. 

**Action:** Action taken by the department of health or if violations were cited in the data set

**Violation code:** This code is related to the type of violation found in the restaurant

**Violation Description:** The description of the violation found in the restaurant

**Critical Flag:** Information on whether the violation was critical or not

**Score:** The total score the restaurant gets based on its violations 

**Grade:** The final grade gave to the restaurant

**Inspection Type:** This provided us with information on whether it was an initial inspection or a re-inspection.


## Examination of the Data
In order to investigate the cleanliness of the restaurants, our first step should be a comprehensive understanding of the grading system.

### Understanding Grading Process:
Inspectors make unannounced visits to restaurants at least once a year. Inspectors check the temperature of the food, inspect how the food is prepared, and look for potential violations such as evidence of vermin or dirty cooking utensils. Based on the violations, restaurants are assigned points for each violation the inspector finds. The rules call for assigning 7 for public health violation, 5 for critical violation and 2 for general violation. Based on their score they are assigned a grade, the higher the violation score the lower the grade of the restaurant. If a restaurant receives a score between 0-13 in their initial inspections then they are assigned grade A, and the next initial inspection occurs in 11-13 months, if the score is between 14-27 then they are assigned a grade B, and the restaurant is re-inspected in 7+ days and the next initial inspection takes place in 5-7 months.  If the restaurant received a score over 28 on the initial inspection then it is assigned a grade  C, and a reinspection occurs in 7+ days, and the next initial inspection takes place in 3-5 months. This series of inspections is called a “cycle.” Restaurants that receive a grade B or Grade C can post their grade card or “Final grade to be determined card” till their final grade is determined. 

### Data cleaning

Like any dataset, our dataset requires cleaning as well. Initially, we checked for missing values, and we removed the restaurants that had an inspection date of  1/1/1900 since they had not been inspected yet. As we explored the data more, we found more problems with the data. To prepare the data for analysis, we did the following steps:

1.Corrected the format of the date

2.Cleaned and re-grouped the violation description: The violation description in the original dataset was overly descriptive and inconsistent. We identified similar violation types and regrouped them under one heading. for example, the various violations related to food temperature were re-grouped under “Food processing/temperature not met.”

3.Cleaned and re-grouped the cuisines: The cuisines’ information had to be re-grouped for consistency. In some cases, the same cuisine had different names. We re-grouped cuisines in such condition into one cuisine. For example, Californian and American were regrouped as “American.”

4.Investigated and fixed missing values: We removed the missing values that were probably caused by data entry and transfer errors. 

5.Geocoded data with Google API key- There wasn’t geographical information like Longitude and latitude, we cannot conduct map visualization on each restaurant. This will not achieve our goal of offering restaurant cleaning  information for diners. What we have done is to geocode each restaurant using its address information. 

6.Regrouped data into different seasons: We grouped the data into spring, summer, fall, and winter based on their inspection date so that we could inspect the total violations based on seasons. 

7.Regrouped data into the different year: We grouped the data by year based on their inspection date so that we could inspect the number of violations for each restaurant each year. 

## Analysis & Results
At the beginning of our study, we want to first depict the violation severity of NYC restaurants and have a general idea of how well do the restaurants and inspection system work.

### 1. Restaurant shut down investigation

The health department may order a restaurant to temporarily close down to correct a public health hazard that cannot be corrected before the end of the inspection or when the restaurant is operating without a valid permit. A restaurant may also be closed down if it scores 28 or more points on three consecutive inspections. We wanted to get an understanding of the number of restaurants that were closed by the health department and the percentage among them that were not eligible to be re-opened.

For the purpose of analyzing the number of restaurants closed by the health department, we got the unique restaurants by filtering out the action as "Establishment Closed by DOHMH." According to our analysis, `r n.close` current opening restaurants that have ever been ordered to be closed by the health department, and `r reclose.rate` of the closed restaurants were re-closed for a longer period. Because on the re-inspection after their closure, their sanitary conditions were not improved to re-opening level. We believe that this indicates a bad attitude displayed by those restaurants. Here are the rudest restaurants that have been closed for the most times and the top closures of chain restaurants.

The second table revealed that Kennedy fried chicken had the maximum number of closed outlets at 15, Subway had 14 closed outlets and Starbucks, and Crown fried chicken had ten closed outlets each. We believe that sometimes chain restaurants focus more on opening outlets and less on maintaining the cleanliness or quality of the outlets.


### 2. Violations in different seasons
We wanted to do further analysis of the relationship between the seasons and the violations. We divided data into seasons according to inspection dates. Surprisingly, there are a number of violations that happened in Winter and Spring. However, we found that this is simply because more inspections were carried out in Winter and Spring. Instead, we calculate the average number of violations per inspection and critical violations rate in each season, we then found that Fall and Summer have more severe problems, which make sense due to a higher temperature.

This mismatch might suggest that the health department should switch their inspection focus a little bit more to Summer and Fall. Taking the seasonal influence into account and make a season-specific inspection plan might help to reduce the violations during hot seasons.

### 3. Animal Violations in Grade A Restaurants
Living in new york, we have all seen the occasional rats and roaches in our households. Due to this, we were curious to know whether the restaurants that are graded. A had any evidence of live animals. We assumed that if a grade A restaurant would probably not have any evidence of live animals in the vicinity. However, our analysis shows that about 40% of grade A restaurants have evidence of live “animal/insects” which includes mice, filth flies, vermin and roaches. We can understand the situation since the problem of rats, roaches and flies is common in New York. Restaurants, whose kitchen are full of food, are suffering from this problem. The solution to this problem should be an overall environmental improvement in New York.

### 4. Violations with cuisince
We wanted to examine the correlation between the cuisines type and average violations. 
We investigated the cuisines with the top violations. Below are the results. We noticed that even though American cuisine constitutes to 22.9 % of the restaurants in NYC, American restaurants do not have the highest average violations, Bangladeshi cuisine restaurants have the most number of average violations. 

### 5. Investigation on neighborhoods | is NYC restaurants cleaner?
Health inspections in NYC aim to improve restaurants’ health quality and safety. However, while we are investigating average restaurants scores by boroughs, there is an increasing trend in scores of restaurants in New York. Does this mean the restaurant inspection system is not effective?  To answer this question, we need more information. While a restaurant has violations requiring immediate action to be addressed, health inspectors will require the restaurant to close and rectify.  This is the one effective way to improve cleanness. As we can see from the graph, over the past three years, 2562 restaurants have been closed, 430 restaurants have been re-closed, and 2523 restaurants have been reopened. While ‘reopened’ means that this restaurant improved their overall quality, and ‘closed’ and ‘re-closed’ mean that restaurants with food safety issues wouldn’t open in the market, we can see that the inspection system is working and maintaining NYC food safety. 

There may be certain reasons for the increasing average scores of each borough. For example, this increasing trend may root in the inspection system. One hypothesis is that the inspection process becomes stricter each year, causing a higher average violation score. However, in 2017,  City Council speaker Corey Johnson introduced a new law that would keep minor violations from affecting a restaurant’s final letter grade due to continued complaints from restaurant owners(Richardson, 2018). After investigating our dataset, we can also see more restaurants can have an ‘A’ grade. The increasing rate of ‘A’ grade is much higher than ‘B’ and ‘C.’ There is a conflict between increasing ‘A’ grade ratio and increasing violation scores.  But there’s also one thing we have neglected, which is the increasing ratio of ungraded restaurants. There is a high ratio of restaurants didn’t receive a grade while initial inspections among these restaurants resulted in a score of 14 points or higher. The annually increasing average violation score may be caused by the increasing amount of restaurants need to be re-inspected whose violation scores are higher than 14. Conclusions we can draw here are that even though the grading system is milder, there is an increasing trend in restaurant violation scores; the restaurant inspection system may be stricter in scoring and milder in grading. 

Besides, the number of restaurants inspections are becoming bigger, which means there’s more new restaurant opened or inspected. While there are more restaurants like Chinese or Indian restaurants opened due to increasing acceptance of diversified cultures, there should be an increasing violation each year since restaurants like Chinese restaurants are less clean than American restaurants. Newcomers may increase the average scores. 


### 6. Investigation of neighborhoods' performance
We have visualized average violation scores in each neighborhoods defining by zip code. The map shows which neighborhoods have the highest average violation scores. The common perception of New Yorkers would be that neighborhoods in Manhattan would have a higher average-violation scores since there are more restaurants and people. However, the map shows that Manhattan is not worse. Instead, lower Manhattan has the highest percentage of grade A restaurants. Staten Island has the highest average violation scores, even though it is less commercialized and with fewer people. This needs further investigations in the future. Besides, the inspection restaurants change each year, which can also show the different inspection process each year. 


### 7. Association Rule

Here are some most commonly appeared violations.

In addition of number of violations, the relationship among all the violation types is worth investigating. If there is a combination of violations that frequently happened together? If one violation frequently happened conditional on another? Understanding these patterns can help to further reveal the potential problems in food safety of restaurants and discover the correlation or causation between different violations.

To achieve this goal, we applied association rule learning and apriori algorithm. Association rules analysis is a technique to uncover how items are associated to each other. There are three measurements: support, confidence and lift (Annalyn, 2016). Support is the proportion of records that contain the item or itemset, which basically states the popularity of appearance. Confidence of X to Y is the likelihood of Y happened when X has happened. However, confidence is limited to describe the strength of association because it does not take in account of the popularity of Y itself. Hence, lift measures the likelihood of Y happened when X has happened while controlling for how popular the Y is.

We performed on the data which have 69 type of violations in total, and the data is re-structured into the following format.

The threshold of support is set to be 0.005 and we focus only on the violation or set that happens at least 0.5% of chance. The confidence is selected to be above 0.05. The result is partially showed in a scatter plot displaying the three measurements of 700 rules. After sorting the lift from highest to the lowest, the top strongest 8 rules are displayed in a network plot, with the meaning of each code below it.

For the network plot, the size of the circle denotes the support, and the deeper the shade, the higher the lift. There are several patterns we observed: 

1. The most common and highest lift combination is: not vermin proof facilities, improper non-food contact surface, and evidence of roaches. 

2. The following highest lift combinations share similar items and are: not vermin proof facilities, evidence of roaches, and either temperature requirement violation or potential food contamination.

3. Another most common combination is: not vermin proof facilities, improper pesticide and evidence of mice. 

These patterns are saying, if the restaurant is detected with roaches, it is highly possible that their facilities are not vermin proof and they also failed temperature requirement. This really tells some stories about why may customers have stomachache or sickness after eat out in the restaurants. After roaches or mice bring vermin to either food or non-food contact kitchen facilities, non-vermin-proof facilities may further spread it to the food material. Even worse that other food contamination sources or hazardous pesticide chemicals commonly happen at same time as well. At the end, when the hot food is not heated up enough or the cold food is not kept in low temperature, those vermin will grow or remained in the food and serve to the customers. It sounds reasonable for health department to decide the mice as critical violation but non-vermin proof facility as non-critical violation. However, we can see that the "facility is not vermin proof" violation strongly associates with other critical problems and it appears to be the center of the net. Even this one might not be the cause of other critical problems; this one is definitely important for reducing the bad consequences brought from the horrible combination of problems discovered above. If the health department can put the facility not vermin proof violation on the critical violation list or increase the fine on this violation, the food safety may improve in general.

### 8. Regression Model

We built a multinomial logistic regression model to help us have a better understanding of the factors that may influence Grade.

If we choose grades of restaurants as our response, it is hard to build a prediction or classification model using our dataset. First of all, the reason why we could not make a model to predict the grade of restaurants is that the grade is calculated by the violation type, violation critical type and numbers of violations by each restaurant. We could not use this information to predict grade. If we have the information about the violation type, violation critical type and numbers of violations by each restaurant, we could know grade by calculating directly instead of using a model. Besides, we think it is useless to predict grade since the customer will know the grade of a restaurant when the restaurant open. Thus, we don’t need to predict the grade. It is also unnecessary to build a classification model to classify the restaurants by the factors because we already have precise criteria to classify the restaurant into different grades.

Based on these features, we build logistic regression to study the influence of factors on grade since our response is categorical variable. We choose borough, season, violation type and the average number of violations by zip code to be our covariate. We investigate if these factors would influence the grade and investigate if the different levels are within each predictor group by using Wald test. 

Using logistic regression, we uses maximum likelihood estimation to calculate the odds of each categorical membership. Suppose the response has J categories, Let$$\pi_{ij}=\pi_{ij}(x_{1i},x_{2i},...,x_{pi})=P(Y_{ij}=1|x_{1i},x_{2i},...,x_{pi})$$
Choose a baseline or reference response category, the Jth say and let
$$log(\frac{\pi_{ij}(x_{1i},x_{2i},...,x_{pi})}{\pi_{iJ}(x_{1i},x_{2i},...,x_{pi})})=\beta_{0j} + \beta_{1j}x_{1i} + \beta_{2j}x_{2i} + ... + \beta_{pj}x_{pi}$$

Where 
$${\pi_{iJ}(x_{1i},x_{2i},...,x_{pi})})=\frac{1}{1+\sum_{j=1}^{J-1}exp(\beta_{0j} + \beta_{1j}x_{1i} + \beta_{2j}x_{2i} + ... + \beta_{pj}x_{pi})}$$


$$\pi_{ij}(x_{1i},x_{2i},...,x_{pi})=\frac{exp(\beta_{0j} + \beta_{1j}x_{1i} + \beta_{2j}x_{2i} + ... + \beta_{pj}x_{pi})}{1+\sum_{j=1}^{J-1}exp(\beta_{0j} + \beta_{1j}x_{1i} + \beta_{2j}x_{2i} + ... + \beta_{pj}x_{pi})}$$

We use Manhattan, autumn and other violations to be the baseline of our model to compare the odds of grade B and grade C restaurants with grade A restaurants. As we could see above, most of the variables have a significant influence in grade using a Wald test with type 3 error since our data is unbalanced. 

The grade of restaurants is different between different areas. For grade B restaurants, restaurants’ borough changes from Manhattan to another borough while holding other factors fixed, the odds of restaurant graded as B increases instead of A.  For grade C restaurants, restaurants’ borough change from Manhattan to Bronx while holding other factors fixed, the odds of restaurant graded as C instead of A increases. However, if it changed to other areas,  the odds of restaurant graded as C decreases. 

Winter also influences the grade. Compared with autumn, spring and winter decrease the odds of a restaurant graded as B or C compared with A. But Summer does not have a significant difference with autumn in grading.

Changing from other violation to critical violations as we grouped while holding other factors constant, the odds of getting a grade B or C instead of A would increase in most situations which meet our expectation. Having violations in animals: mice/rats/roaches/flies will lead to a higher risk of having a grade B or C compared with having a grade A. This shows grade A restaurants are doing much better than grade B or C restaurants in controlling living animals.

Mean scores of restaurants by zip code affect the grade. Given the restaurant is B, a unit increase in the mean scores of restaurants near it will increase 29% of the odds of graded as B, not A. Given the restaurant is C, a unit increase in the mean scores of restaurants near it will increase 43.5% of the odds of graded as C instead of A. This means the distribution of restaurants has a clustering feature that it may be affected by the restaurants around them. If the restaurants around them have low scores which are cleaner, they trend to be cleaner. This may be due to the competition with nearby restaurants. 

## Conclusion

In summary of all our analysis, we found that the food hygiene problems in NYC are indeed worth attention.From several statistics and regression models, we found that season and the mean score of surrounding restaurants are essential components that affect the grade of the restaurant, and different boroughs, cuisines and violation types are leading to difference on grade as well. By understanding the cluster of violation types, we can discover some potential process of how food hygiene problems lead to health issues of customers.  We believe our findings bring some impact on inspection policies, like including seasonal changes into the inspection routine and penalize heavier on currently non-critical violations which are the central problems of all violations. By studying average violation scores in each year and the restaurants required to close and rectify, we do notice the inspection system makes restaurants in  New York City cleaner and better. We also found that the grading system is not as strict as before, making more restaurants can have an 'A' grade.

For customers, our application can also greatly benefit them with gaining the most recent hygiene situation of each restaurant, which can help reduce the public health issues when customers dine out. Besides, our application can also help improve the restaurant's cleanliness because transparent cleanliness information our application can show to customers will motivate restaurants to improve.

Specifically, both season, borough, violation type and mean scores in its zip code area will influence the odds of a restaurant’s grade. The restaurants in Manhattan perform better than the restaurants in other borough and restaurants trend to have higher grade in Spring during the whole year. Among all critical violations, we know restaurants in grade A will have less critical and non-critical violations compared to grade B restaurants and grade C restaurants for sure since the grading is based on the violations. However, after building the multinomial logistic regression model, we would see among all kinds of violations, the biggest difference between grade A restaurants and grade B or grade C restaurants is in violation that “Filth animals: mice/rats/roaches/flies”. Having violations "Filth animals: mice/rats/roaches/flies” instead of “other violations” while holding other factors fixed, the odds of restaurant graded as B or C instead of A almost doubled. Restaurants’ grade also be affected by restaurants around them. They tend to have better performance if restaurants nearby are good. This results is also consistent with conclusion we made before that borough have effect on restaurants’ grade.


## Discussion
One of the biggest limitations in the data set was that when an inspection occurs if the restaurant received a Grade A then they receive a grade and the next inspection occurs in 11-13 months but if the restaurant received Grade B or Grade C then a reinspection occurs in 7 days and the restaurant can post the card for "Grade pending" until the final grade is determined. This led to gaps in the dataset regarding restaurants that could have possibly received Grade B and Grade C. 

### Areas of Future Investigation:
Going forward, We believe that it would be beneficial to investigate the reasons for the increasing number of violations. Current investigation remains at a hypothesis stage, further investigation should be conducted with more information and data on environment, population and so forth. Besides, it is also worth further efforts on investigation of why Staten Island has the highest average violation scores considering that it is less commercialized and with fewer people.

For this analysis, external attrubutions are not included due to difficulty of merging data from other sources.  However, we are also interested in if review and rates of restaurants are related with restaurant grades or violations; if environment cleanliness of surrounding streets affect the grade. If we are able to build a prediction model for restaurant violation situations, that will be helpful for government to focus their inspections on some specific restaurants instead of random check which is not very effective and efficient. We are also willing to further develop our application to have a recommendation system, which takes the user’s interested restaurants and return them a list of similar restaurants but with better food hygiene situation.


## References

[1] *City of New York.* (n.d.). Retrieved from https://www1.nyc.gov/

[2] restaurantinspection.nyc.gov/RestaurantInspection/SearchBrowse.do

[3] Calgary, O. (n.d.). Retrieved from https://data.cityofnewyork.us/Health/DOHMH-New-York-City-Restaurant-Inspection-Results/43nn-pn8j

[4] Richardson, N. (2018, September 20). *More New York City Restaurants Have 'A' Grades Than Ever Before.* Retrieved from http://www.grubstreet.com/2018/09/record-amount-of-a-grades-for-new-york-city-restaurants.html

[5] Trevor Hastie, Robert Tibshirani, Jerome Friedman(2009). *The Elements of Statistical Learning: Data Mining,Inference, and Prediction.* Retrieved from https://web.stanford.edu/~hastie/ElemStatLearn//

[6] Annalyn N. (2016, April) *Association Rules and the Apriori Algorithm: A Tutorial.*<i> Retrieved from https://www.kdnuggets.com/2016/04/association-rules-apriori-algorithm-tutorial.html




#2. Reporting engine file which can generate visulization dashboard.
Introduction
=====================================  


Based on DOHMH New York City Restaurant Inspection Results, this report contains information on violation trends in different locations, seasons, cusine types and violation information of unique restaurants. 

Aiming to solve information asymmetry issue in NYC food industry, this report allowed restaurant consumers to make informed decisions on where they want to eat and motivated NYC restaurants to improve their inspection score in order to attract more customers. It also provides insights for health department to improve their inspection procedures. 

Click on the tabs to see different reports.

Shiny dashboard: 
Introduction and Purpose: 

We built a shiny dashboard to display the information investigated above. 
The shiny can be used by users provide them with information of restaurant violations so that they can make better and more informed decisions about dining out. 
Our dashboard can also be used by the department of health to plan their inspections so that they can target the right restaurants and plan their visits efficiently. 
If the violation information of each restaurant is available to the users, it will also motivate the restaurants improve their inspection score as an increase in the violation score would have a direct impact on their business. 

Shiny- Violations 

Under the violations tab of the shiny users can filter the information for critical or non-critical violations and select the number of cuisines. They can also filter information of the grade A restaurants with different violations types. 

Shiny-  Map Suggestions

Under the map suggestions tab the users can get violation information for the restaurants around NYC. The map provides users with information of the total violations of each restaurant during the period they choose and the exact location of the restaurant. We use the most recent grade of a restaurant to be the grade in its information. The dot gets darker as the number of violations of each restaurant increases. 

Shiny-  Model

We also built a logistic regression model in Shiny to help us have better understanding of the factors that may influence Grade. Each time we could choose two type of grades to build a model that could study the influence of factor variables on them. Comparing to build a multinomial logistic regression which can only compare the odds of one type with baseline type, our model in Shiny could easily between the difference between any subset of grades. This is more flexible and contains more information. 
