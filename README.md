# World Data League 2022

## 🎯 Challenge
***Stage 1:*** Predict Waste Production for its Reduction

## 👥 Authors
* Cristiana Carpinteiro
* Diogo Polónia
* João Matos
* João Pereira
* Patrícia Rocha

## ✨ Introduction
The world is becoming more and more conscious about the environmental problems associated with waste, but habits are hard to change. In Austin, Texas, between 2010 and 2019, more than 4.7 billion pounds of waste has been produced, roughly 53.7% of which was common garbage. Because of the growing population - the U.S. population is projected to cross the 400-million threshold in 2058 -, this number is expected to increase.

Unless we take measures towards producing less solid waste, this issue will heavily impact the urban environment. Collection and transport cause a significant carbon footprint as a consequence of stops and starts instead of constant speed driving. Thus there is an urge towards a more efficient collection.

## 🔢 Data
* ***[Daily Waste Collection Report for Austin](https://data.austintexas.gov/Utilities-and-City-Services/Waste-Collection-Diversion-Report-daily-/mbnu-4wq9)***
Main table of our analysis. Since it provides waste loads from 2003 to 2021, it provided us with historical waste production data, per route, per type of waste. We solely used garbage and recycle waste. We suggest adding information on the vehicle type and shift duration, as it would enable the computation of trucks' CO2 emissions, assess trucks' free space at the end of the route and optimize routes based on those goal functions.
* ***[Garbage Routes](https://data.austintexas.gov/Locations-and-Maps/Garbage-Routes/azhh-4hg8)***
Spatial information about each garbage route. There are 184 garbage collection routes and each one takes place once a week, from Monday to Friday. We recommend adding information about stopping points, total length, and covered area per zip code. It would enable matching spatial data with more precision and more robust route optimization.
* ***[Recycling Routes](https://data.austintexas.gov/dataset/BOUNDARIES_recycle_collection_routes/7tin-f8k2)***
Spatial information about each recycling route. There are 190 recycling collection routes and each one takes place every two weeks. We extend to this dataset the recommendations done to garbage routes dataset.
* ***[2010 Census Demographic Data per Zip Code](http://zipatlas.com/us/tx/austin.htm)***
Demographic data per zip code, in Austin. We recommend using more up-to-date data, as well as curating the dataset with FAIR features, to avoid ethnicity-biased conclusions; data covering all zip codes in Austin would also be welcome, to avoid having routes unmatched with demographic data.


## 🧮 Methods and Techniques
***1. Identify Critical Regions***
To identify critical regions - where people recycle less and could benefit from awareness campaigns -, we calculated a recycling rate by route considering the load weights of recycling \(R\) and garbage collections (G) as follows: R/(R+G)

***2. Identify Critical Demographic Groups***
Critical demographic groups for recycling and waste collection were identified using a Random Forest regression model (evaluated using the mean absolute percentage error), which receives as input demographic-based features collected from Austin’s census and returns the total weight of garbage or recycling in that year, for that group of population (with the input demographic features).

***3. Forecast Waste Production in Future***
For the forecasting algorithm, the data was first opened as a pandas dataframe, processed, and divided into train (past) and test (future). The module *statsmodels* was used to analyze the time series (trends, seasonality, etc.) and to create/train a SARIMA model. The test predictions of this model were evaluated using the mean absolute error method of the *sklearn* module.

***4. Detect Improvement Opportunities in Garbage Collection for Optimization***
Opportunity detection was based on the ratio between the total weight and total loads collected per route. Routes were clustered using the *k-means* technique (using the *scikit-learn* python library) based on that ratio.
A *non-linear optimization model* was built, using the *gekko* library, to determine the number of loads required per route in order to minimize CO2 emissions (full-loaded trucks burn more fuel than semi-loaded ones)

## 💡 Main Insights

Although we've seen a huge increase in environmental consciousness in the past decade, that is reflected in Austin's waste collection data, but, surprisingly, not on the level we were expecting. On one hand, total waste production increased, from 2010 to 2019, by 8.5% while recycling increased by 11.8% (highlighting only a 3.3% increase in the proportion of recycling of total waste). On the other hand, common garbage in 2019 still represented over 50% of all waste collections in terms of weight. In terms of recycling, Austin recycles a little less than 30% of garbage which goes hand-in-hand with other studies about recycling that report that, in the USA, only 30% is recycled, even though 75% of garbage could be, in fact, recycled.

When it comes to waste production, it is not necessarily correlated with population density (as we might be initially inclined to think) as much as it is associated with demographical characteristics such as age, household income, gender, financial and labor situation, and the data even suggests race-based influences. However, the causality we found between racial features and production of waste can be themselves a result of other conditions we were not able to analyze (e.g. sanitary structure, road accessibility, trash collection infrastructure, etc.), but might be important to enrich our models.

Regarding waste collection, there is a very high discrepancy in the weight collected per trip for different routes (maximum average weight per load in 2019 was 30540lbs, while the minimum was 1387lbs, which is less than half). This indicates that there is a lot of space to improve waste collection to be more efficient in the use of garbage trucks and consequently reduce fuel-based carbon emissions, even though there is a slight trade-off between collecting more waste and doing more trips, as heavier trucks consume more fuel!

## 🛠️ Product
### Definition
A dashboard with data-driven insights for targeted environmental campaigns and forecasting tools for waste collection optimization.

__or__

A decision support system for targeted environmental campaigns and waste collection management.

### Users
Austin's management departments for:
* **Waste Collection**, when planing or tweaking the waste collection routes 
* **Environmental Education**, when raising public awareness on better recycling practices and more responsible waste management


### Activities
Our solution:
1. Identifies **critical Austin regions** in terms of: total garbage production, recycling done, and garbage vs. recycling ratio
2. Predicts yearly amount of produced waste per zone, as well as recycling done, from relevant **demographic data**, thus identifying most relevant demographic features to define **priority target groups**
3. **Forecasts** future **daily garbage** production for Austin city, from historical data
4. Segments routes into 3 groups, depending on garbage weight per number of loads, pinpointing a set of **critical routes in which collection is not optimized**
5. Suggests optimal number of loads per route required to collect all garbage produced in order to minimize CO2 emissions (of garbage trucks), based on garbage production historical data or predicted data.



### Output

A user-friendly, data-rich dashboard with 4 tabs:
#### Tab 1:
![Forecast](https://i.imgur.com/9EKUcK3.png)
The decision support system we propose is able to forecast garbage production for the next day. Taking into account historical data, it helps the decision maker decide to decrease, keep or increment collection capacity for the next day.

#### Tab 2:
![Campaigns](https://i.imgur.com/CkP9Ywu.png)


This tab of the dashboard shows critical zones in Austin, per year, when it comes to **recycling rates**. Such zones are pinpointed and identified as **priority for campaigns**.

In parallel, the marketing team can see where in the city certain demographic groups are dominant - and it's a match! The available demographic groups have been selected as relevant to determine the amounts of waste production in that year. Knowing who and where, **targetted environmental awareness raising** is easier than ever.

#### Tab 3:
![Route Clusters](https://i.imgur.com/ZVsxhce.png)
This tab highlights the collection routes clustered by weight/load ratio, that allows the identification of the "Low Ratio" zones which are the ones that require more urgently an improvement of the collection plan.

#### Tab 4:
![Route Optimizer](https://i.imgur.com/a6ZoXPf.png)
The route optimizer is able to find the optimal weight to load ratio in order to minimize CO2 emissions, as there is a trade-off in load quantity (because a heavier truck consumes more fuel) and the number of trips.
This tab could take as input the predicted waste production values estimated in Tab 1.


## 🌍 Social Impact Measurement
### Outcome
* To increase the quantity of waste recycled in order to decrease envrionmental impact.
* To decrease the number of collection loads per route to save on CO2 emissions.

### Impact Metrics
* Total collected common garbage waste weight.
* Total collected recycled waste weight.
* Yearly fuel consumption by garbage trucks.
* Estimated total CO2 emissions by garbage trucks.

### Impact Measurement
* ***Based on research***: Studies have shown that 75% of common garbage could be recycled. As a conservitive estimation, we expect to increase recycling from the current 30% to 50%.
* ***Based on model predictions***: Our model estimates a decrease by 30% in CO2 emissions by optimizing the number of loads per route.