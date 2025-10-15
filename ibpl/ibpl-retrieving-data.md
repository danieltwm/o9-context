# Retrieving Data Through Queries in IBPL

```
The select syntax is used to select data from a database. The output data is stored in the result table
called result-set. A select query in IBPL consists of the command “select”, followed by a list of
AttributeMemberSets (or named sets) separated by asterisks, followed by a list of measures enclosed in
curly braces, with the whole thing enclosed in parenthesis. Order by and Limit keywords can be used to
sort the results and limit the number of intersections displayed. Keywords asc or desc can be used to
display the results in ascending or descending orders.
```
```
For more information, click Select Syntax-Example.
```
```
These queries return the data related to the level attributes of a dimension.
```
```
Example 1 : Select (Product.[Product Name]);
```
```
Result: The query returns a list of all products in the model.
```
```
Example 2 : select CurrentUser();
```
```
Result: The query enables you to know the current user (logged in users)
```
```
Example 3 : select ([Electronics].[Item] *[Electronics].[Group]*[Electronics].[Category]) on row, () on
column include memberproperties {[Electronics].[Group], Band} {[Electronics].[Item], Size};
```
```
Result: the above query fetches the list of electronic items along with its respective group and category.
The member properties like Band and Size will also be displayed.
```
```
These queries return both the master data as well as transaction data. That is, all data related to the level
attributes of a dimension and measure groups.
```
```
Example 1 : Select (Version.[Version Name] * Product.[Product Name]* SalesDomain.[Sales Domain
Name] * Time.[Fiscal Months]* {Measure.[Gross Revenue]});
```
```
Result: the query returns the gross revenue for all product, sales domains, months and versions in the
system
```
```
Example 2 : Select ([Date].[Reporting Week] * [Location].[Location] * [Product].[BDC Name] *
[Version].[Version Name] ) on row, ({Measure.[Beginning On Hand], Measure.[Independent Demand],
Measure.[Propagated Demand], Measure.[Suggested Replenishment], Measure.[Target Inventory],
Measure.[Target WOS], Measure.[Total Demand]}) on column;
```
## Select Syntax

## Member Query

## Measure Query

10/15/25, 11:01 AM Retrieving Data Through Queries in IBPL - o9 Solutions Wiki

https://platformwiki.o9solutions.com/index.php/Retrieving_Data_Through_Queries_in_IBPL 1 / 2


```
Result: The above query returns the Beginning On Hand, Independent Demand and other measures
specified in the query for all weeks, locations, SKUs and versions in the system
```
```
Example 3 : Select ({Count([Channel].[Business Type] * [Demand Priority (DPC)].[Demand Priority
(DPC)] * [Demand Type].[Demand Type] * [Geography].[Market - Sub Affiliate] * [History Months].
[History Months] * [History Start Date].[History Start Date] * [Key Account].[Key Account] * [Product].
[10D] * [Product Life Cycle].[Product Life Cycle] * [Strategic Indicator].[Strategic Indicator] * [Time].
[Partial Week]) as Transient.CustCount using ~IsNULL(Measure.[Total Sell-In Fcst L103]) ||
IsNULL(Measure.[Total Sell-In Fcst L103])}) on row, () on column where {&CWV};
```
```
Result: the above query displays the row count of the measures.
```
```
Retrieved from ‘https://platformwiki.o9solutions.com/index.php?
title=Retrieving_Data_Through_Queries_in_IBPL&oldid=18183’
```
```
This page was last modified on 5 October 2021, at 15:56.
```
10/15/25, 11:01 AM Retrieving Data Through Queries in IBPL - o9 Solutions Wiki

https://platformwiki.o9solutions.com/index.php/Retrieving_Data_Through_Queries_in_IBPL 2 / 2


