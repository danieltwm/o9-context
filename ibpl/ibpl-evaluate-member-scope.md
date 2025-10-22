```
Implementing Evaluatemember Scope
```
## Implementing

## Evaluatemember

## Scope

#### o)Academy

## ABSTRACT

### Understand how to

### write an

### Evaluatemember scope

### and make use of nested

### if statements along with

### some date-time-related

### functions.

Implementing Evaluatemember Scope <:f) Academy

## Table of Contents

```
Tutorial 6: Implementing Evaluatemember Scope ........................................................................ 3
Purpose: .................................................................................................................................................... 3
Prerequisites ........................................................................................................................................... 3
Use Case: ................................................................................................................................................. 3
Model: ........................................................................................................................................................ 4
Solution: .................................................................................................................................................... 5
```
```
2
```

##### •

##### •

##### •

##### •

Implementing Evaluatemember Scope <:f) Academy

## Tutorial 6: Implementing Evaluatemember

## Scope

## Purpose:

The purpose of this tutorial is to use:

```
Evaluatemember Scope
Nested If statement
todatetime function
datediff function
```
## Prerequisites

Tutorial 1 from Module 1 should be completed.

## Use Case:

```
The requirement is to find out Actual Lead Time based on measures "Active
Supplier", "Committed Lead Time", "Order Date" and "Delivery Date" stored in a
finer grain measure group when compared to the calculated measures (ALT and
SP).
The committed lead Time column is populated using a fact file and is the LT
provided by the supplier in the initial days. Using the measures Order Date and
Delivery Date find out the Actual Lead Time in months.
```
```
3
```

##### •

```
o
```
-
    o

Implementing Evaluatemember Scope

## Model:

```
Files - Fact.FactSupplierlnfo.csv
Dimensions - Version Name, Supplier, Time, Item
Measure Groups:
Supplier Info (Version Name X Supplier X Item)
```
```
<:f) Academy
```
```
Measures - Active Supplier, Committed Lead Time, Order Date,
Delivery Date
Supplier L TP (Version Name X Supplier X Month X Item)
Measures - Actual Lead Time, Supplier Performance
```
```
4
```

```
evaluatemember scope: ([Supplier].[Supplier] * [Time].[Month] *
[Version].[Version Name].[CurrentWorkingView] * [Item].[Item]);
Measure.[Actual Lead Time] = if(Measure.[Active
Supplier]==1)then (if(Time.#.Key == todatetime(Measure.[Order
Date]))then (datediff(todatetime(Measure.[Order
Date]),todatetime(Measure.[Delivery Date]),Month)));
end scope;
```
```
Select ([Version].[Version Name].[CurrentWorkingView] *
[Time].[Month] * [Supplier].[Supplier] *[Item].[Item])on row,
({Measure.[Actual Lead Time]})on column;
```
Implementing Evaluatemember Scope <:f) Academy

## Solution:

### Steps to Implement:

1. Check the above-mentioned modelling elements must be present in your tenant
    along with data of all members and measures, else complete the previous
    tutorials first.
2. Write an evaluatemember scope to populate the "Actual Lead Time" measure.
Note: The Measures used to calculate the Actual Lead Time are stored in a
    different measure group when compared to the Actual Lead Time. So, we need to
       use an evaluatemember scope for the operation.
3. Write a select query to check the values in "Actual Lead Time".

```
5
```

