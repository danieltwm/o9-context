# Updating Data in IBPL

```
Such queries are used to set the value of a specific measure, to an updated value.
```
```
Example 1 : update Measure.[M1]@(Product.[Product].[P1], Time.[Month].[Jan])=100;
```
```
Result: the above query will update the measure value of Product P1 to 100 for the month of Jan.
```
```
Example 2 : update measure.[Segment Target Gross Revenue]@(Product.[Category].[Carbonated Cola],
Version.[Version Name].[CurrentWorkingView], Customer.[Sales Region].[Segment East], Date.[Fiscal
Month].[M08-2013]) =1000000000;
```
```
Result: Segment Target Gross Revenue, for Carbonated Cola, for Segment East, for M08-2013, is
updated to the value specified.
```
```
IBPL allows users to update multiple cells by using filter clauses on the dimension attribute while using
update query.
```
```
Example : update Measure.[OPP Override Forecast Lbs]@(Product.[Class Name].Filter(#.Name in
{[1627 Pita Chip],[1639 Pita Chip]}) , Product.[Flavor Name].[Simply Naked],[Warehouse].[Location
ID].[001310 BELOIT PLANT], Version.[Version Name].[CurrentWorkingView], Date.[Reporting
Week].Filter(#.Name in {[2x1-2013], [2x2-2013], [2x3-2013], [2x4-2013]})) = 1000;
```
```
Result: The above query updates the value of the measure, OPP Override Forecast Lbs to 1000 for the
reporting weeks [2x1-2013], [2x2-2013], [2x3-2013] and [2x4-2013].
```
```
It sets the value for this measure for the specified scope, i.e., for CurrentWorkingView, for the product
Cosmo Cola 12 oz., for sales domain Target, for all times, to be stated value. The right hand side of this
expression could be a measure itself, not just a value.
```
```
Scope: (&CurrentWorkingView* Product.[Product Name].Filter(#.Name = “Cosmo Cola 12 oz”)* SalesDomain.[Sales Domain
Name].filter(#.Name == “Target”) * Time.[Fiscal Months]);
Measure.[Gross Revenue]=1000;
end scope;
```
```
Retrieved from ‘https://platformwiki.o9solutions.com/index.php?title=Updating_Data_in_IBPL&oldid=18185’
```
```
This page was last modified on 5 October 2021, at 17:05.
```
## Single-Cell Update

## Multi-Cell Update

## Scope-Based Multi-Cell Update

10/15/25, 10:59 AM Updating Data in IBPL - o9 Solutions Wiki

https://platformwiki.o9solutions.com/index.php/Updating_Data_in_IBPL 1 / 1


