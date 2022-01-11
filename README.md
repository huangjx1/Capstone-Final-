# Capstone Project

# Author: Huang Jiexiang

There will be 2 notebooks. 

1st notebook named as "Cleaning of Dataset" - This notebook is to carry out cleaning of the data as well as doing data exploration
2nd notebook named as "Predicting house prices" - This notebook is to carry out modelling and the predcition of the houserpices.

## Content

1. Problem Statement
2. Preparation of data and cleaning
3. Exploratory Data Analysis and cleaning of features
4. Feature Engineering
5 & 6. Modelling and evaluation (part 1 and part 2)
7. Conclusion, recommendation & Next Steps

# Problem Statement: To Predict the Prices of private residential properties


Import of libaries and dataset in csv format.

Datsesets extracted from URA website: https://www.ura.gov.sg/realEstateIIWeb/transaction/search.action


# 2. Preparation of data and cleaning

1. The downloaded dataset has text at the beginning and ending of each csv file. The csv file is cleaned from those text.
2. The csv is concated into a whole dataframe.
3. Reseting the index for the S/N column is required to index the rows of the dataframe properly. eg. such as when carrying out an iloc command the dataframe should re reflecting the correct row.
4. checking of the head() and tail() of the dataframe is carried out
5. Renaming the columns of the datafram eis carriedout for easy typing and referencing of the columns.
6. The clean data is exported out to a csv file as merged_house.



# 3 & 4. Exploratory Data Analysis and cleaning of features and Feature Engineering

- Few features were selected out to do some data exploration, we will use these features to predict the atrget variable.
- Y feature is unit price per psf

There is a few features selected out from the dataframe to do data exploration

1. Market Segment (CCR, RCR, OCR)
Each house in different region shows a different pricing. However, for CCR region, the boxplot shows a greater pricing with a huge amount of outliers. Hence, dummfiying these feature is required.

2. Postal District
A feature check was done on the postal district and the outliers were detected in district 10 (supposedly the CCR region). This features have to be dummfied as well.

3. Tenure
A feature check was done for tenure and the tenure column was in "object" format due to words inside the values. A cleaning was done for data using str.split().str[0] and [-1] was applied to move the str out to convert the value as an integer as well as deducting the amoutn of tenure left. This clean integer data is inserted back into the dataframe as an integer column. The pricing of the house prediction will be based on the integer amount.

The tenure was also split into 2 categories which is categorized under 0 - 99 years and freehold (>99 years) for the dataset. This will be explained more during the data modelling section

![](https://github.com/huangjx1/Capstone-Final-/blob/main/img/house%20category.png)

4. Type of house
The type of house was explored and it shows that the detached house category would fetch as higher price as compared to semi- detached and terrace. Dummifying of this feature

5. Type of sale
The type of sale categorized under resale is the highest. Hence, dummifiying of this feature is carried out.

Additional cleaning

5% of the dataframe was removed as there were outliers spotted causing skewness of the graph when the histogram is plotted. In total, 468 rows were removed from the dataframe.

The dataframe was saved to csv as merged_house_clean

Next, all the features such as market segment, postal dsitrict, type and type sale were dummified and the dataframe was saved to csv as merged_house_data.


# 5.1 Modelling for merged_house_data (5% reduction of outliers)

### 5.1a. Linear Regression was carried out with standard scaler being applied

There were no overfitting from the train and test score from linear regression with score 0.835 and 0.833 respectively. However, the RMS of the prediction is at $942134. Hence, we will look into optuna (XGboost).

*Note: A Lasso regression was carried out but since there were no overfitting at the start. There is not much differences whehn lasso is applied.

Check for linearity was carried out and the dataframe shows that as the houseprices increase, the results start to deviate from linear regression rules.

### 5.1b. XGBoost without hyper parameter tuning

An overfitted model with score and test of 0.945 and 0.873 respectively.


### 5.1c. XGboost using Optuna

With the Optuna hyperparameter tuning. The XGboost score for the train and test were 0.911 and 0.877 respectively. There is around a 3.5% of the overfitting.

As mentioned in #3 and #4 "Tenure", the dataset was split into 2 set of data once belong to house categorized with 99 years and the other >99 years (also known as freehold).


# 5.21 Modelling for house (within 99 years)

A scatterplot was plotted out and we can see a co-relation of the overall price increases as the tenure increases.

The data was reduced to all houses within the 99 years tenure. The dataset was saved to csv as merged_house99.

![](https://github.com/huangjx1/Capstone-Final-/blob/main/img/99%20years.png)

### 5.21a Linear regression (for merged_house99)

The train and test results of linear regression were 0.931 and 0.874 respectively. This indicate an overfitting of linear regression.


### 5.21b. XGboost using Optuna

The train and test results of XGboost were 0.967 and 0.919. This still indicates an overfitting.


# 5.22 Modelling for house (freehold)

Another dataset was split out within the > 99 years and the dataset was saved to csv as freehold. However, we can see that this dataframe holds 7900 rows which may implies that the XGboost will be overfit again.

![](https://github.com/huangjx1/Capstone-Final-/blob/main/img/freehold.png)

### 5.22a Linear regression (freehold)

There were no overfitting from the train and test score from linear regression with score 0.838 and 0.837 respectively. 
The RMS is $921233 which is very close to the merged_house_data RMS of $942134.

### 5.22b XGboost using optuna

Using optuna as hyperparamter tuning. The XGboost score for train and test is 0.904 and 0.861. This is very closely related to 0.911 and 0.877 from #5.1c.


## As the models are overfitted, a further reduction on dataset was done removing outliers at 10%, 5% from top and 5% from the bottom.

From the Cleaning of Dataset notebook. merged_house_data was further reduced to 8753 rows as saved as csv merged_house_10

Compare 2 histogram from 5% and 10%

![](https://github.com/huangjx1/Capstone-Final-/blob/main/img/5%25%20reduction.png)
![](https://github.com/huangjx1/Capstone-Final-/blob/main/img/10%25%20reduction.png)

We can see from the histogram as there were lesser outliers from the second image with 10% reduction.


# 6.1 Modelling for merged_house_10 (10% reduction)

### 6.1a. Linear Regression was carried out with standard scaler being applied

Linear Regression shows a train and test score of 0.796 and 0.785 with no overfitting.

### 6.1b XGboost using optuna

Using optuna as hyperparamter tuning. The XGboost score for train and test is 0.880 and 0.842. This is a slight overfit with RMS of $613257



# 6.21 Modelling for house (within 99 years)


### 6.21a Linear regression was carried out with standard scaler being applied



### 6.21b XGboost using optuna

Using optuna as hyperparameter tuning. The XGboost has a train and test score of 0.960 and 0.950 respectively. This shows that there is no overfitting. The RMS error is significantly reduced to $246307

## IS there a data leakage?


# 6.22 Modelling for house (freehold)

### 6.22a Linear regression was carried out with standard scaler being applied

Linear Regression shows a train and test score of 0.801 and 0.814 with no overfitting.


### 6.21b XGboost using optuna

Using optuna as hyperparamter tuning. The XGboost score for train and test is 0.893 and 0.835 respectively. This is an overfitting with RMS of $581159.


# Conclusion

In conclusion, there is overfitting on the freehold apartment. This maybe due to "tenure" that may not be a main factor and the location might be a deciding factor for freehold apartment. We can attempt to look into the location wise of the freehold apartment beside its district.

