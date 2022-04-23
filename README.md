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
* The`Class` variable identified as `object` by default, and are most likely Categorical.  
* We noted that `Arrivial Delay in Minutes` seems to be missing some values.

### Dataset Cleaning
<div class="alert alert-block alert-info">
    <b>Missing Values: </b> It's noted that <code>Arrivial Delay in Minutes</code> has count <code>129487</code> instead of <code>129880</code>. This is due to it containing <code>NULL</code> values. We will replace them with <code>0</code> here.
</div>
<br>
<div class="alert alert-block alert-info">
    <b>Ordinal Categorical Variables</b><br>
    Most ordinal categorical variables are rating types in the <code>int</code> form. No conversion required. <br>
    But we will convert for <code>non-int</code> types <code>Class</code> and <code>Customer Type</code> in 
    <a href="#exploratory-analysis">Exploratory Analysis</a>
</div>

<a id="exploratory-analysis"></a>
## Exploratory Analysis

### Response Variable
Lets take a look at the response variable `satisfaction_v2`.
<div class="alert alert-block alert-info">
    The <code>satisfied</code> to <code>neutral/dissatisfied</code> ratio of <code> 71087 : 58794 </code> is acceptable. We will not perform any rebalancing. 
</div>

### Predictor Variables
Lets take a look at the other `22` variables.<br>
We shall split them into the following subcategories.

* Passenger: variables relating to the passenger.
* Service: variables corresponding to the services provided by the airline.
* Others: variables that are do not fall in the above categories.
---
<a id="passenger-variables-ea"></a>
#### Passenger Variables
Variables relating to the passenger. <br>
**Categorical** : 
[`Class`](README.md/#class-ea)
[`Type of Travel`](README.md/#type-of-travel-ea)
[`Customer Type`](README.md/#customer-type-ea)
[`Gender`](README.md/#gender-ea) <br>
**Numeric** : [`Age`](README.md/#age-ea) <br>

<a id="class-ea"></a>
<div class="alert alert-block alert-info">
    <b>Class (Categorical)</b><br>
    The class variable seems to describle the type of flight class the passenger was in.<br>
    Since this is normally choosen by the passenger, we labeled it under <b>Passenger Variables</b><br>
    <b>Values</b><br>
    We observed that there are 3 unique values for <code>Class</code> variable.<br>
    It seems like their ordinal values(ascending) are as follows:<br>
    1: <code>Eco</code> 2: <code>Eco Plus</code> 3: <code>Business</code><br> 
    We will convert them accordingly. <br>
    <b>Distribution</b><br>
    The most common value is <code>Business</code> which is followed closely by <code>Eco</code>.<br>    
    <code>Eco Plus</code> has the least distribution. <br>
    <b>Relation</b><br>
    <code>Business</code> class have the higest satisfied rate while passengers from <code>Eco</code> and
    <code>Eco Plus</code> have higher neutral/disatisfied ratings.
</div>

<a id="type-of-travel-ea"></a>
<div class="alert alert-block alert-info">
    <b>Type of Travel (Categorical)</b><br>
    This variable seems to describle type/purpose of travel of the passenger.<br>
    <b>Values</b><br>
    We observed that there are 2 unique values <code>Personal Travel</code> <code>Business travel</code> <br>
    <b>Distribution</b><br>
    <code>Business travel</code> has the higher distribution of 89693. <br>
    <b>Relation</b><br>
    <code>Business travel</code> appears to have higher satisfaction
</div>

<a id="customer-type-ea"></a>
<div class="alert alert-block alert-info">
    <b>Customer Type (Categorical)</b><br>
    This variable seems to describle if passenger is a loyal customer.<br>
    <b>Values</b><br>
    We observed that there are 2 unique values <code>Loyal Customer</code> <code>disloyal Customer</code> <br>
    We will assign them the weights as follows: <br>
    1: <code>disloyal Customer</code> 2: <code>Loyal Customer</code> <br>
    <b>Distribution</b><br>
    <code>Loyal Customer</code> has the higher distribution of 106100. <br>
    <b>Relation</b><br>
    <code>Loyal Customer</code> appears to have higher satisfaction rate
</div>

<a id="gender-ea"></a>
<div class="alert alert-block alert-info">
    <b>Gender (Categorical)</b><br>
    <b>Values</b><br>
    There are 2 unique values <code>Male</code> and <code>Female</code> <br>
    <b>Distribution</b><br>
    Even distribution 63981 : 65899  <br>
    <b>Relation</b><br>
    It appears that <code>Female</code> passengers have a higher satisfaction rate 
</div>

<a id="age-ea"></a>
<div class="alert alert-block alert-info">
    <b>Age (Numeric)</b><br>
    <b>Values</b><br>
    There are 2 unique values <code>Male</code> and <code>Female</code> <br>
    <b>Relation</b><br>
    It appears that ages <code>40</code> to <code>60</code>passengers have a higher satisfaction rate   
</div>

<div class="alert alert-block alert-info">
    <b>Age + Gender</b><br>
    Analyse the relation with Age + Gender.<br>
    We observe that the graphs are fairly similar. Not much difference in relation.
</div>


<a id="service-variables-ea"></a>
#### Service Variables
For our problem case, we will be focusing mainly on services on board the plane.<br>
**Focus**:
<code>Seat comfort</code> 
<code>Food and drink</code> 
<code>Inflight wifi service</code> 
<code>Inflight entertainment</code> 
<code>On-board service</code> 
<code>Leg room service</code> 
<code>Checkin service</code>
<code>Cleanliness</code>

**Non-Focus**:
<code>Online support</code> 
<code>Ease of Online booking</code> 
<code>Baggage handling</code>
<code>Online boarding</code>

<div class="alert alert-block alert-info">
    <b>Focus Service Variables (Categorical)</b><br>
    <b>Values</b><br>
    We observed that there are 6 unique values from 0 to 6.<br>
    All of which are <i>rating</i> type variables.<br>
    <b>Relation</b><br>
    We observed that generally, ratings 5 and 6 have higher statisfaction rate.<br>
    But more notably, 
    <code>Seat comfort</code>
    <code>Food and drink</code>
    <code>Inflight entertainment</code> seems to have strong relation to satisfaction. <br>
    Specifically speaking, ratings 5 and 6 have higher statisfaction rate and additionally,<br>
    ratings 3 and 4 have higher neutral/distatisfaction rate
</div>

<a id="other-variables-ea"></a>
#### Other Variables
Other variables that are not related to customer or airline service. <br>
<b>Categorical</b>: 
<a href="#gate-location-ea"><code>Gate location</code></a>
<a href="#departure-arrival-convenient-ea"><code>Departure/Arrival convenient</code></a>
<br>
<b>Numeric</b> : 
<a href="#flight-distance-ea"><code>Flight Distance</code></a>
<a href="#departure-delay-ea"><code>Departure Delay in Minutes</code></a>
<a href="#arrival-delay-ea"><code>Arrival Delay in Minutes</code></a>

<a id="gate-location-ea"></a>
<div class="alert alert-block alert-info">
    <b>Gate Location (Categorical)</b><br>
    This variable most likely represent the convenience of the gate location.<br>
    As the airline may not choose their gate location, we did not include under service.<br>
    <b>Values</b><br>
    We observed that there are 6 unique values from 0 to 6.<br>
    It is a <i>rating</i> type variable.<br>
    <b>Distribution</b><br>
    Rating <code>3</code> has the highest distribution. <br>
    Rating <code>0</code> has the lowest distribution. <br>
    <b>Relation</b><br>
    <code>Loyal Customer</code> appears to have higher satisfaction rate 
    <br><br><a href="#other-variables-ea">Return</a>
</div>

<a id="departure-arrival-convenient-ea"></a>
<div class="alert alert-block alert-info">
    <b>Departure/Arrival time convenient (Categorical)</b><br>
    This variable seems to describle covenience of the flight departure and arrival times.<br>
    Although flight timings are provided by the airline, the passenger normally pick the timeslot.<br>
    As such, we labeled it under <b>Other Variables</b><br>
    <b>Values</b><br>
    We observed that there are 6 unique values from 0 to 6.<br>
    It is a <i>rating</i> type variable.<br>
    <b>Distribution</b><br>
    Rating <code>3</code> has the highest distribution followed closely by <code>2</code> and <code>4</code><br>    
    Rating <code>0</code> has the lowest distribution. <br>
    <b>Relation</b>
    <br><br><a href="#other-variables-ea">Return</a>
</div>

<a id="flight-distance-ea"></a>
<div class="alert alert-block alert-info">
    <b>Flight Distance (Numeric)</b><br>
    This variable describes the flight distance most likely in miles.<br>
    <b>Relation</b><br>
    It appears that at below <code>1000</code> miles, satisfaction rate seems to be higher.
    <br><br><a href="#other-variables-ea">Return</a>
</div>
<a id="arrival-delay-ea"></a>
<div class="alert alert-block alert-info">
    <b>Departure Delay in Minutes & Arrival Delay in Minutes (Numeric)</b><br>
    We excluded <code>0</code> departure delays<br>
    <br><br><a href="#other-variables-ea">Return</a>
</div>

