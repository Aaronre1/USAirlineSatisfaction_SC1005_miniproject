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

**Categorical** : 
[`Class`](README.md/#class-ea)
[`Type of Travel`](README.md/#type-of-travel-ea)
[`Customer Type`](README.md/#customer-type-ea)
[`Gender`](README.md/#gender-ea)

**Numeric** : [`Age`](README.md/#age-ea)  

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
We observed that there are 6 unique values from 0 to 5.  
All of which are *rating* type variables.

We observed that generally, ratings 5 and 6 have higher statisfaction rate. 
But more notably, `Seat comfort` `Food and drink` `Inflight entertainment` seems to have strong relation to satisfaction.
Specifically speaking, ratings 5 and 6 have higher statisfaction rate and additionally ratings 3 and 4 have higher neutral/distatisfaction rate.

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
