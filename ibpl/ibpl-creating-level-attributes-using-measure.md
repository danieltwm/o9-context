## Creating level attributes using a measure

## Creating level

## attribute Members

## using a measure

## o)Academy

## ABSTRACT

## Understand how to

## create level attribute

## members using a

## measure value. This also

## covers the usage of the

## if else statement.

## •

## Creating level attributes using a measure

## Table of Contents

## Tutorial 5: Creating Level Attribute Members using a Measure ............................................... 3

## Purpose: .................................................................................................................................................... 3

## Prerequisites ........................................................................................................................................... 3

## Use Case: ................................................................................................................................................. 3

## Solution: .................................................................................................................................................... 4

### 2


## •

## •

## Creating level attributes using a measure

## Tutorial 5: Creating Level Attribute Members

## using a Measure

## Purpose:

## The purpose of this tutorial is to:

## Create Level Attribute member using a measure values.

## Use If-else.

## Prerequisites

## Tutorial 1 from Module 1 should be completed.

## Use Case:

## Create a new String measure named "SKU Performance", that will hold the

## performance with respect to each SKU. The performance will be calculated based on

## the shipment values. Once the "Performance" measure is populated, then, using the

## same measure, we will create a new Attribute "SKU Performance" under the Product

## dimension.

### 3


## createmember

## scope: ([Product].[SKU] * [Version].[Version

## Name].[CurrentWorkingView] );

## Measure.[SKU Performance] =

## if(Measure.[Shipment]<=4800000) then "Bad" else

## if(Measure.[Shipment]>4800000 &&

## Measure.[Shipment]<5100000) then "Very" + " Good" else

## ("Excellent") ;

## end scope;

## select ([Product].[SKU] * [Version].[Version

## Name].[CurrentWorkingView] * {Measure.[SKU

## Performance]});

## createmember ([Product].[SKU Performance]={,Measure.[SKU

## Performance]})

## using scope ([Version].[Version

## Name].[CurrentWorkingView] * [Product].[SKU]);

## Creating level attributes using a measure

## Solution:

## Steps to Implement:

## 1. Check the below-mentioned modeling elements must be present in your tenant,

## else complete Tutorial 1 from Module 1 first.

## Plan: Sales Plan

## Measure Group: Performance (Granularity: Version Name, SKU)

## Measures: SKU Performance (Type: String)

## Dimension: Product

## Level Attribute: SKU Performance

## 2. Write a scope statement to populate SKU Performance with the values "Bad",

## "Very Good" and "Excellent" based on shipment values.

## 3. Write a select query to check the values.

## 4. Write a command using a scope, as shown below.

### 4


## select ([Product].[SKU Performance]);

## Creating level attributes using a measure

## 5. Generate a select query to fetch the Level Attribute data.

### 5


