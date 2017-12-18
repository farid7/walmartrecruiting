
# Walmart Recruiting Classification

Authors:

 - Timothée Givois M. ([@timgivois](https://github.com/timgivois))
 - Farid Oroz ([@farid7](https://github.com/farid7))


This project was taken from kaggle competion [site](https://www.kaggle.com/c/walmart-recruiting-trip-type-classification),  and the submission result was: ....

## Table of Contents  
- [Challenge description](#challenge-description)  
- [Data Fields](#data-fields)  
- [Used Methods](#used-methods)
- [Cleaning](#cleaning)
- [Exploratory data analysis](#exploratory-data-analysis)
- [Imputation](#imputation)
- [Feature Engineering](feature-engineering)
- [Transform](#transform)
- [Selection/Filtering](#selection/filtering)
- [Pipeline Prediction](#pipeline-prediction)
- [Measurement](#measurement)

## Challenge description
For this competition, the objective is categorize shopping trip types based on the items that customers purchased. To understand better what a trip type is: a customer may make a small daily dinner trip, a weekly large grocery trip, a trip to buy gifts for an upcoming holiday, or a seasonal trip to buy clothes.

Walmart has categorized the trips contained in this data into 38 distinct types using a proprietary method applied to an extended set of data. You are challenged to recreate this categorization/clustering with a more limited set of features. This could provide new and more robust ways to categorize trips.

## Data fields

Field | Description
--- | ---
TripType| a categorical id representing the type of shopping trip the customer made. This is the ground truth that you are predicting. TripType_999 is an "other" category.
VisitNumber | an id corresponding to a single trip by a single customer
Weekday | the weekday of the trip
UPC |  the UPC number of the product purchased
ScanCount |  the number of the given item that was purchased. A negative value indicates a product return.
DepartmentDescription |  a high-level description of the item's department
FinelineNumber | a more refined category for each of the products, created by Walmart

## Used Methods
We used the following methods for this project:

### Cleaning
Data was downloaded in csv format, which had 4,129 missing data in columns _Upc_ and _FinelineNumber_.
First off, the function *clean_colnames* formatted all headers of the file (removed empty spaces and symbols),
then, the function *clean_data* formatted string in observation, to lower case.
   Used functions are in the -util.R- script.
### Exploratory data analysis
In this part, a visual analysis of univariate and bivariate data was made. It was easy appreciated that
_TripType_ 49 and 39 are the most common among others. Also, the most common days for buyers is
_Sunday_ and _Saturday_; the day less "busy" is Thursday.

Regarding to the _DepartmentDescription_, the quantity of purchased items decay exponentially, and the three
most common items are from _grocery dry  goods, dsd grocery_ and _produce_ departments. The most common
quantity of purchased items (independently of their department) is _1, 2_ and _-1_ items (returned). The most
common product is categorized into _5501_ _FinelineNumber_.

The _VisitNumber_ is usually repeated in the dataset. It is because one person buys different items, so,
there is an observation for each purchased (or returned) item. Only two visitors treaded more than 150 items.

The most common product has the _Upc_ of 4011.

On the other hand, under the bivariate analysis, it seems that the _TripType_ of 40 is related to commonly
used products (e.g. from _grocery, grocery dry goods_ and _personal care_ departments). Also, on Sundays,
There is more variance in the quantity of purchased items.

The dataset is plotted in the 03-eda.Rmd file.

### Imputation
1,492 rows have missing data, under the columns _Upc_ and _FinelineNumber_; maybe, it is because those
products are note registered in the database (correlation of 1 between those missing columns).

Missing data was imputed using the library *MissForest* in *R*.

The code for this part is in the *02-clean.R* script.

### Feature Engineering
For the feature engineering, we decided to rearrange groups for DepartmentDescription and also create DepartmentDescriptionGroups that are supposed to be more general and easier to create dummies from.
We defined 12 major groups for departments:
- accessories
- electronics
- food
- health & beauty
- clothes
- house
- home
- garden
- enfant
- media and gaming
- office
- games

At the end, these new categories have a similar distribution of items to the original categories of _DepartmentDescription_. 

On the other hand, two new variables were created using _ScanCount_ and _VisitNumber_ variables. Within _VisitNumber_ we identified how many different items were treated in that trip, creating the variable _numItems_,  and the variable _num_purchased_ is created by summing all the products in _ScanCount_ field. If the variable _num purchased_ is negative, it means that the client returned more items that those he/she bougth. 

### Transform
Two variables were transformed using the 'one-hot encoding' method (*pandas.get_dummies*): _DepartmentGroup_ (description above) and _weekday_. 

For those transformed variables,we applied the mean value to those new variables, gruped by _VisitNumber_ in order to get the same value per row (at the end, we want a dataset where each row corresponds to a single VisitNumber).

### Selection/Filtering
Once the data set had new variables, and transformed variables, it was eliminated "redundant" rows. Using the *gropu_by* funcion in pandas, se had one row per _VisitNumber_, also, it was eliminated the variables _ScanCount_ and _Upc_, because those variables were represented in new _num items_ and _num purchased_ variables.

The final data_frame, the input for machine learning models, had one row per _Visit Number_ with one label _TripType_. 

### Pipeline Prediction
### Measurement
