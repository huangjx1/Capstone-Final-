# Capstone Project

# Author: Huang Jiexiang

## Content
1. Problem Statement
2. Preparation of data and cleaning
3. Exploratory Data Analysis and cleaning of features
4. Feature Engineering
5. Modelling and evaluation (part 1 and part 2)
6. Conclusion, recommendation & Next Steps


# 1. Problem Statement: To Predict the Prices of private residential properties

Import of libaries and dataset in csv format.

Datsesets extracted from URA website: https://www.ura.gov.sg/realEstateIIWeb/transaction/search.action

# 2. Preparation of data and cleaning

- The data has text at the beginning and ending has alot of text and NaN.

- As the csv cannot be downloaded all at once, the download has to happen over 5 downloads, after cleaning the NaN, the csv file is concated into 1 foldercall merged_house. Checking of the data using isnull is used to ensure no NaN is inside dataset.

- Next, with merged_house, the S/N columns numbers do not tailly because the dataset is concated. A reset index is required to be carry out to ensure index is the same as searching with iloc will require the index to be same as the row we are searching for.

- checking of index using merged_house.tail() is done

- Next, we export the file out to csv file to save it as a clean dataset

- Renaming of the columns and do a merged_house.columns.str.lower() is carried out so that the columns(features) can be easily typed out.


# 3 & 4. Exploratory Data Analysis and cleaning of features and Feature Engineering

- Few features were selected out to do some data exploration
- Y feature is unit price per psf

1. Market segment(CCR, RCR, OCR)
2. Postal district
3. Tenure
4. Type
5. Type of sale

- A boxplot was drawn out for market segment to identify which has the highest unit price per psf. It was seen that inside the Core Central Region, there were quite a bit of outliers. Dummifying the market segement to include it as a type of feature for modelling

- Features check was done on postal district, there were only abit of outliers in district 10, the rest of the postal district was relatively well balanced. This feature was also dummified.

-Next, a feature check was done on tenure but the tenure data was found to be in "obj" due to words inside the list such as lease and commencing. a str.split().str[0] and [-1] was applied to move the str out to convert it as an int. After cleaning the tenure data, the tenure was added back into the dataset.

- The tenure utimately show skewness of the dataset both right and left side, the red line was draw to shift out the =<99 years lease tenure houses.

# INSERT IMG HERE
![](https://git.generalassemb.ly/martinyong/Project4/blob/master/img/num_mosqitoes_2007.png)

- A box plot of hosue type was drawn and there seems to be not much difference. The feature was also dummified to be included into the modelling portion

- From the box plot, the type of sale is also dummfied to be icnluded as a feature.

### 2 top outliers were removed for emerald hill road (conservation area) and cairnhills were included. These 2 areas have exceptional high value and will cause the regressor model to be skewed. 

-Dataset was saved as merged_house1


# 5. Modelling (part 1) - Houseprices for psf

Linear regression was used and the there were no overfit. However, the accuracy was not ideal and the RMSE is $330 unit price psf.
XGboost and Random Forest Regressor  was used, all of the model was overfitted and a change of y features to overall price is required since areasq is a feature we can cosnider.


# Insert IMG


merged_house1, we used the unit price psf to remove certain abnormal high prices and low prices

merged_house2 was to include all unit price <= $3000 and >- $500.

There were onyl about 100+ row removed.

Next, with this dataframe we carried out Linear Regression and XGboost was carrie dout and XGboost still shows overfitting.

# 5. Modelling (part 2) - Main House Prices

merged_house1 was loaded and the target variable is set as Price

XGBoost and Random Forest regressor both shows great imporvement in accuracy and reduced in overfitting issues. However, the model was still overfitted. A more in depth study was carried for target variable Price.

With merged_house1, we removed 2.5% of the higest and 2.5% of the lowest price as a bar graph was draw to show a skewness of the price difference from left to right

# Insert img
# Insert img

Linear Regression, XGboost and Random Forest Regressor was used and linear regression show an accuracy for 0.85 with a slight overfitof >2% and a RMSE of$110494

For XGBoost, it shows an accuracy of 0.91 with 弄over fitting and an RMSE of $860700, Random Forest Regressor also shows no overfitting with an RMSE of $856674

Next, as there was extreme skewness of the tenure features, the dataset is being split up into =<99 years and >99 years tenure. This was done to see if we can gauge 2 models to a obtain a lower RMSE score.

Dataset was split to merged_house99 (1470 rows) and freehold (8020 rows)

Linear Regression and XGBoost was done on both dataset. Both model shows roughly about 2.5% overfit

for merged_house99, the RMSE is $713246 and for freehold the RMSE is $866714.













