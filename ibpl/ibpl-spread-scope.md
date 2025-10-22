## Implementing Spread Scope

## Implementing

## Spread Scope

## o)Academy

## ABSTRACT

## Understand how to

## trigger spreading at

## higher level values that

## spread down only as

## integer values.

## Implementing Spread Scope <:f) Academy

## Table of Contents

## Tutorial 7: Implementing Spread Scope ............................................................................................ 3

## Purpose: .................................................................................................................................................... 3

## Prerequisites ........................................................................................................................................... 3

## Use Case: ................................................................................................................................................. 3

## Model: ........................................................................................................................................................ 4

## Solution: .................................................................................................................................................... 5

### 2


## •

## •

## •

## ●

## Implementing Spread Scope

## Tutorial 7: Implementing Spread Scope

## Purpose:

## The purpose of this tutorial is to use:

## Spreading

## Distribute Integer type of spreading

## Spreading using a Basis measure

## Prerequisites

## Previous tutorials must be completed.

## Use Case:

## <:f) Academy

## Once the forecast is generated, the forecasted number is communicated to plant

## managers so that the manufacturing process can initiate for that specified number

## of units. As a result, the measure "Units to Produce" is stored at a weekly level. The

## number of units to be produced is calculated and updated at the monthly level.

## To accomplish this, create a solution using a distributed integer and use a

## different measure as the basis measure to spread the values from

## months to weeks.

### 3


## •

## o

## Implementing Spread Scope

## Model:

## Files - Fact.FactMonthlyProduction.csv

## Dimensions-Version Name, Time

## Measure Groups:

## Manufacturing (VN X Week)

## <:f) Academy

## Measures - Units To Produce(Old), Basis, Null Basis, Units To

## Produce( New}

### 4


## Implementing Spread Scope <:f) Academy

## Solution:

## Steps to Implement:

## 1. Check the above-mentioned modeling elements must be present in your tenant

## along with data of all members and measures, else complete the previous

## tutorials first.

## 2. Create a report as shown below and make all these measures "editable" so that

## the user can manipulate the values if required.

###### Manufacturing Report

##### 8 Showing 5 rows o' data.

```
Actions M ont h
~ B MOt:
```
#### 6 Download - V Filters .. Layout Local Edit

###### 7 t Week

```
(All)
W14-? 0 ?:J
W15-2 023
W1f...?0?
W17-20 23
```
###### 7 Units To Produce(Old) ~ Bass

```
35
<O
?S
JO
```
###### ~ Nul Basis "='" Unit s To ProcLce(New_)

```
130
nS
'
```
```
22
```
## Note: This report will already be created if you restore the "Advanced IBPL"

## package mentioned in module 1. One can go to the report setting and check all the

## settings applied for this report, for example enabling simple subtotals.

## 3. Apply spreading settings to the measure "Units to Produce (New)" as shown

## below.

#### o Units To Produc e(New)

## Units To A'oduce(New) ✓ v x

### Regula r Spreadings +

##### Basis Measure Type• l±i El

## [ Basis Measur~ X .. I

###### Basis Measure•

## Aasis X ..

###### S1Xea:ling Type•

## Distribute Integer X ..

###### Measure Rel:;1tionship

## ..

## Differential Spreadings +

```
No content
```
### 5

## Fact.FactMonthlyProduction

# o9 Internal and o9 Academy use only

## Implementing Spread Scope

## 4. Upload the file

```
11
```
## spreading.

## " to trigger the

## 5. Verify the distributed values using the report created previously.

### 6

## <:f) Academy


