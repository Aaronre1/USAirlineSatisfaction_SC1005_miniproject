# US Airline Satisfaction Mini Project 

In this Project, we would like to peform some analysis on a dataset of __[US Airline passenger satisfaction survey](https://www.kaggle.com/datasets/najibmh/us-airline-passenger-satisfaction-survey?resource=download)__.

## Contents
- [Problem](README.md/#problem)
- [Data Preparation](README.md/#data-preparation)
- [Exploratory Analysis](README.md/#exploratory-analysis)
- [Models](README.md/#models)

## Problem
Based on passenger ratings, we would like to find out how the different indivudal ratings affect the passenger's final decision for a _satisfied_ or _unsatisfied_ with the service provided by US Airline.
**Specifically**:
1. Can we predict if customer would be satified?
1. What are the most important factors that affect customer satisfaction?

## Data Preparation
#### Initial Observations
* There are `24` columns and `129880` rows in the dataset.   
* The response variable seems to be `satisfaction_v2`.
* There is `1` column for ID
* The remaining `22` columns are potential predictor variables.

#### Predictor Variables
* There are `16` variables identified as `int64` by default. But it seems like only `Flight Distance` and `Departure Delay in Minutes` are actually numeric. The remaining `14` variables are ratings from 0 to 5 and should be considered as Categorical.
* The `Arrivial Delay in Minutes` variable identified as `float64` by default, and it seems to be Numeric.
* The `Class` `Gender` `Customer Type` `Type of Travel` variables identified as `object` by default, and are most likely Categorical.  
* We noted that `Arrivial Delay in Minutes` seems to be missing some values.

### Dataset Cleaning
**Missing Values** : It's noted that `Arrivial Delay in Minutes` has count `129487` instead of `129880`. 
This is due to it containing `NULL` values. We will replace them with `0` here.

**Ordinal Categorical Variables**
Most ordinal categorical variables are rating types in the `int` form. No conversion required. 
But we will convert for `non-int` types like `Class` and `Customer Type` in [Exploratory Analysis](README.md/#exploratory-analysis)

## Exploratory Analysis

### Response Variable
Lets take a look at the response variable `satisfaction_v2`. 
The `satisfied` to `neutral/dissatisfied` ratio of `71087 : 58794` is acceptable. 
We will not perform any rebalancing for now. 

### Predictor Variables
We will look at the other `22` variables and observe its relation to `satisfaction_v2`.  
We shall split them into the following subcategories.

* Passenger: variables relating to the passenger.
* Service: variables corresponding to the services provided by the airline.
* Others: variables that are do not fall in the above categories.
---
#### Passenger Variables
Variables relating to the passenger.

Categorical : 
[`Class`](README.md/#class-ea)
[`Type of Travel`](README.md/#type-of-travel-ea)
[`Customer Type`](README.md/#customer-type-ea)
[`Gender`](README.md/#gender-ea)

Numeric : [`Age`](README.md/#age-ea)  

##### Categorical
<a id="class-ea"></a>
###### Class
The class variable seems to describle the type of flight class the passenger was in.
Since this is normally choosen by the passenger, we labeled it under **Passenger Variables**.  
We observed that there are 3 unique values for `Class` variable.
It seems like their ordinal values(ascending) are as follows:  
1: `Eco` 2: `Eco Plus` 3: `Business`.

The most common value is `Business` which is followed closely by `Eco`. `Eco Plus` has the least distribution. 
`Business` class have the higest satisfied rate while passengers from `Eco` and
`Eco Plus` have higher neutral/disatisfied rate.

<a id="type-of-travel-ea"></a>
###### Type of Travel
This variable seems to describle type/purpose of travel of the passenger with 2 unique values `Personal Travel` `Business travel`.  
`Business travel` has the higher distribution of 89693 and appears to have higher satisfaction rates.

<a id="type-of-travel-ea"></a>
###### Customer Type
This variable seems to describle if passenger is a loyal customer with 2 unique values `Loyal Customer` `disloyal Customer`.  
`Loyal Customer` has the higher distribution of 106100 and appears to have higher satisfaction rates.

<a id="gender-ea"></a>
###### Gender
Even distribution of `Male` : `Female` =  63981 : 65899 .  
It appears that `Female` passengers have a higher satisfaction rate.  

##### Numeric

###### Age
It appears that passengers aged `40` to `60` have a higher satisfaction rate.  

###### Age + Gender
Analyse the relation with Age + Gender, we observe that the graphs are fairly similar. Not much difference in relation.

---
#### Service Variables
For our problem case, we will be focusing mainly on services on board the plane and not on online services.

**Focus**:
`Seat comfort` 
`Food and drink` 
`Inflight wifi service` 
`Inflight entertainment` 
`On-board service` 
`Leg room service` 
`Checkin service`
`Cleanliness`

**Non-Focus**:
`Online support` 
`Ease of Online booking` 
`Online boarding`  

We observed that there are 6 unique values from 0 to 5.  
All of which are *rating* type variables.

We observed that generally, ratings 4 and 5 have higher statisfaction rate. 
But more notably, `Seat comfort` `Food and drink` `Inflight entertainment` seems to have strong relation to satisfaction.
Specifically speaking, ratings 4 and 5 have higher statisfaction rate and additionally ratings 2 and 3 have higher neutral/distatisfaction rate.


![download](https://user-images.githubusercontent.com/65995623/164957502-f6b9bddb-84be-4d17-aa4c-6e3a869141b2.png)
![download](https://user-images.githubusercontent.com/65995623/164957507-46f572ed-057d-4176-a91f-5c9c3cdfbe57.png)
![download](https://user-images.githubusercontent.com/65995623/164957510-d7761238-9f32-44b2-9c0f-e64eb632c109.png)



---
#### Other Variables
Other variables that are not related to customer or airline service.  

**Categorical** : 
[`Gate location`](README.md/#gate-location-ea) 
[`Departure/Arrival convenient`](README.md/#departure-arrival-convenient-ea) 
[`Baggage handling`](README.md/#baggage-handling-ea) 

**Numeric** : 
[`Flight Distance`](README.md/#flight-distance-ea) 
[`Departure/Arrivial Delay in Minutes`](README.md/#departure-delay-ea) 


##### Categorical
We observed that there are 6 unique values from 0 to 5 and are *rating* type variables. 
**Distribution**  
`Gate location`: Rating 3 has highest frequency.  
`Departure/Arrival time convenient` and `Baggage handling` : Rating 4 has highest frequency.

**Relation**  
No clear relation for `Gate location` and `Departure/Arrival time convenient`.  
But rating `4` `5` for `Baggage handling` have higher satisfaction rate.

<a id="gate-location-ea"></a>
###### Gate Location
This variable most likely represent the convenience of the gate location.  
As the airline may not choose their gate location, we did not include under service.  

<a id="departure-arrival-convenient-ea"></a>
###### Departure/Arrival convenient
This variable seems to describle covenience of the flight departure and arrival times.  
Although flight timings are provided by the airline, the passenger normally pick the timeslot.  
As such, we labeled it under **Other Variables**

<a id="baggage-handling-ea"></a>
###### Baggage handling
As baggage handling are not on-board services, we did not include it under service.

##### Numeric

<b>Numeric</b> : 
<a href="#flight-distance-ea"><code>Flight Distance</code></a>
<a href="#departure-delay-ea"><code>Departure Delay in Minutes</code> <code>Arrival Delay in Minutes</code> </a>

<a id="flight-distance-ea"></a>
###### Flight Distance  
This variable describes the flight distance most likely in miles.  
It appears that at below `1000` miles, satisfaction rate seems to be higher.  

<a id="departure-delay-ea"></a>
###### Departure/Arrival Delay in Minutes
We excluded rows with `0` delays.  
Generally, longer delays have higher neutral/disatisfaction rate.


## Models
We create models for `satisfaction_v2` with different attempts/methods.
For our problem case, we focus on our service variables:  
> `Seat comfort` `Food and drink` `Inflight wifi service` `Inflight entertainment`
> `On-board service` `Leg room service` `Checkin service` `Cleanliness`

### Attempt 1 - Multi-Variate Classification Tree 1
### Attempt 2 - Multi-Variate Classification Tree 2
### Attempt 3 - Random Forest 1
### Attempt 4 - Random Forest 2
### Attempt 5 - Logistic Regression

#### Receiver Operating Characteristic(ROC) Curve

A plot for the true positive rate against the false positive rate.  
![download](https://user-images.githubusercontent.com/65995623/164957453-ed60ec33-1374-4407-87f2-ecd6396cada4.png)

AUC score of ~0.87. Consider good. As 1 represents perfect classifier and 0.5 represents a worthless classifier.

## Conclusion
| # | Attempt 1 | Attempt 2 | Attempt 3 | Attempt 4 |
| --- | --- | --- | --- | --- |
| Accuracy | 0.8451936402833384 | 0.8538144348024864 | 0.9153324717115501 | 0.9151830067083411 | 
| TPR | 0.9337160567745572 | 0.8171225402189601 | 0.9028916171245647 | 0.9034345357734758 |
| TNR | 0.7385075767222717 | 0.8904347826086957 | 0.9277665465412674 | 0.9268690482453833 | 
| FPR | 0.26149242327772826 | 0.10956521739130434 | 0.07223345345873253 | 0.07313095175461672 | 
| FNR | 0.06628394322544288 | 0.1828774597810399 | 0.09710838287543533 | 0.0965654642265242 | 

- Attempt 1 & 2 => Multi-Variate Classification Tree
- Attempt 3 & 4 => Random Forest
- Attempt 5 => Logistic Regression

From the models created above, we can conclude that the models created using Random Forest performs better.

As it has a higher accuracy rate, true positive rate and true negative rate. Also with lower false positive rate & false negative rate.

### Take-Aways
From our analysis, we can see that certain services are more important to the passengers.  
Airlines can choose to targest such key services to improve thier passengers satisfaction ratings.  
From our models, we can predict if an airline can provide satisfactory services.  

## Referencences
__[US Airline passenger satisfaction survey](https://www.kaggle.com/datasets/najibmh/us-airline-passenger-satisfaction-survey?resource=download)__

## Contributors
- Aaron Yong
- Chen Jian
- Chen Bin
