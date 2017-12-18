
# Walmart Recruiting Classification

Authors:

 - Timothée Givois M. ([@timgivois](https://github.com/timgivois))
 - Farid Oroz ([@farid7](https://github.com/farid7))


This project was taken from kaggle competion [site](https://www.kaggle.com/c/walmart-recruiting-trip-type-classification),  and the submission result was: ....

## Table of Contents  
[Challenge description](#challenge-description)  
[Data Fields](#data-fields)  
[Used Methods](#used-methods)
[Cleaning](#cleaning)

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
### Exploratory data analysis
In this part, it is visualized univariate and bivariate fatures of data. 
I was observed that most of features from raw_data are categorical (_TripType, VisitNumber, Weekday,
Upc, DepartmentDescription, FinelineNumber_), and just one feature is numerical (_ScanCount_). 

In univariate analysis, it is appreciated that most of _TripType_ either 40 or 39 category; most 
"busy" days are _sunday_ and _saturday_, and the less "busy" day is _thursday_. 

On the other hand, most purchased items are from _grocery dry foods_ and _dsd grocery_; and the quantity of 
other purchased items decay exponentially. Similarly, the most purchased item is under the category "5501".

ScanCount feature, is the number of items "purchased", and the top three quantity of items 
are 1, 2 and -1 (one returned). 

Is is also appreciated that the _VisitNumber_ is repeted in the dataset. It is because, for each item, a ner
row is generated, with the same _VisitNumber_ id (Only two observation usedmore than 150 items).

Regarding to unique products, most of them have the _Upc_ of 4011.

In bivariate graphs, it seems that _TripType_ is related to the kind of products (if they are consumed
frequently, for example food and personal care products). Also, on Sundays, the quantity of purchased items
have more variance. 




### Imputation
### Transform
### Feature Engineering
### Selection
### Filtering
### Pipeline Prediction
### Measurement
