# Capstone Project

# Author: Huang Jiexiang


# Problem Statement: To Predict the Prices of private residential properties

There will be 2 notebooks. 

1. The 1st note book named as "Cleaning of Dataset for houses" is to carry out data exploration as well as data cleaning.
2. The 2nd notebook named as "Predicting house prices" is to carry out different type of modelling and prediction for house prices using different ML algorithms.


# Content for 1st notebook

1. Importing of dataset and preparation
2. Data Exploration for dataset
3. Data Cleaning
4. Data for Modelling


## 1. Importing of dataset and preparation

Import of libaries and dataset in csv format.
Datsesets extracted from URA website: https://www.ura.gov.sg/realEstateIIWeb/transaction/search.action

Dataset was concated with renaming of columns names as well as resetting the index so the iloc functions can be used as required.

## 2. Data Exploration for dataset

Data exploration was carried out on different features as compared to the target variable "price".
Features there were explored were:

### Market Segment
The amount of houses located in each region from Core Central Region(CCR), Rest of Central Region(RCR) and Outside of Cnetral Region (OCR). The boxplot gives information that the CCR has quite a number of outliers

### Postal District
The Postal District indicated to us that area for high prices of houses, in such a case, district 10 located in CCR Region consist of the outliers. Postal District is important because they can account for different area of cost price in a case when CCR, RCR or OCR is too generalized.

### Type of Sale
Most of the private residential properties is categorized under as resale

### Type
The house type was also looked into whether it is a Terrace, Semi - Detached or Detached.

### Tenure
A data exploration was done on the Tenure and it shows that there were a total amount of 65 different objects in the tenure column. Data cleaning is required for tenure as this is an important feature for the prediction as tenure represent how long can the owner holds on to the property.


## 3. Data Cleaning

### Tenure
Tenure was in the object format due to words and number values inside the data. Using a str split, we managed to convert the object to an integer value by using a simple addition and subtraction to obtain the remaining lease. Abnormally high integer (1 was at 9944 years) was replaced as 999 years to represent it under as freehold. The list was then inserted back into the dataframe as an integer column to do the ML prediction.

### Removal of 5% from Dataset ( top 2.5 and lowest 2.5 percentile)

A removal of 5% of data from the dataset was carried out to remove outliers detected during data exploration. The houses were of exceptionally high prices incur by some factors such as heritage area or en bloc situations which caused the price to increase.


### Removal of 10% from Dataset ( top 5 and lowest 5 percentile)

The removal of 10% of data was further carried out to remove more outliers as the histogram plot still shows a certain amount of outliers in the 5% removal of data.

[image]

## 4. Data for modelling

The other features that was not dropped was dummified so that it can be used to the prediction.

The data used for modelling will be merged_house_data :5 % removal of data
& merged_house_10: 10% removal of Data



# Content for 2nd notebook

1. Data Modelling for 5% data removed
2. Data Modelling for 10% data removed
3. Further actions of splitting dataset
4. Data Modelling for house categorized under different tenure
5. Conclusion and recommendations


## 1. Data Modelling for 5% data removed

4 types of ML algorithms was applied to this model.

- Linear Regression
A slight over fit from linear regression model using the difference between the RMSE of the train and test set (6.21%).

-Lasso Regression
With the overfit, lasso regression was applied but the difference between RMSE of the train and test set shows almost no improvement (5.89%).

- XGBoost and RandomForest Classfier
Both tree algorithms shows over fitting. The difference between RMSE of the train and test is 20.11% and 25.39% respectively


## 2. Data Modelling for 10% data removed

3 types of ML algorithms was applied to this model.

All 3 were overfitted. The table belows shows the results of 5% and 10% rmeoval of data.

| Data Type | RMSE (%) |
| --- | --- |
| Linear Regression with 5% removed| 6.21% |
| Lasso Regression with 5% removed| 5.89% |
| XGBoost with 5% removed| 20.11% |
| Random Forest Regressor 5% removed| 25.39%|
| Linear Regression with 10% removed| 7.39% |
| Lasso Regression with 10% removed| 22.91% |
| Random Forest Regressor 10% removed| 21.08%|


## 3. Further actions of splitting dataset

The tenure was splitted into 2 category as <=99years and >99 years because of a 99 years lease and freehold category under the holding of property.

The data was split into 2 category of data using the tenure. The reason for this split is because of the extreme ends of tenurethat was oberserved during the data exploration.

[image]


99year lease -> dataset is named as merged_house99
freeholding -> dataset is named as freehold

## 4. Data Modelling for house categorized under different tenure

| Data Type | RMSE (%) |
| --- | --- |
| Linear Regression with houses tenure <=99years| 100% |
| XGBoost with 5% removed with houses tenure <=99years| 15.7% |
| Random Forest with 5% removed with houses tenure <=99years| 30.17% |
| Linear Regression for freehold|1.67% |
| XGBoost with 10% removed for freehold| 15.7% |


## 5. Conclusion and recommendations

It was observed that the tree algorithms applied was not suitable for this dataset. However, our linear regression model was able to capture a none overfitted model for houses which is categorized as freehold with 1.67% RMSE.

The RMSE is chosen as our main metrics because it punishes predicted value that has a greater difference the actual value. This is imporant for us as we do not want to undervalue or overvalue a property unit. The RMSE also gives us a better intution of the error which is the target variable (price). This will tell us how far off the predicted and the actual value are.

For the 99 years lease category, we are unable to get an accurate view over it as the RMSE error is abnormal. This could be due to the small dataset extracted out from the actual data. Furthermore, the 99 years lease fall under the same category as HDB estate. Therefore, this could be a reason why the private residential area does not have a sufficient data for the 99 years. As such, the dataset cannot capture the true releationship from the actual and predicted prices.

In additional for 99 years lease category, it would be more logical or objective if it is compared to the HDB estate as the tenure will fall under the same category.
