## Welcome to IBPL Quick Ref

This quick reference guide has been written for

- Newbies who get directions to use an IBPL command and are looking for example code
- Folks who forgot syntax of a rarely used command

Each entry is arranged as a brief description, example and other commands to check or refer. The intent is to provide executable code snippets and brief explanation of the statement. The examples can be executed in the debugging workspace.

## IBPL capabilities

- Querying data - Access data filtered and ordered as required for validation and verification. Usually the number of rows fetched will be few. If the number of rows is large one should transfer data using files for download.
- Modifying data - Update member data and run computations to update measures or edges. Usually large amount of measure data is updated. Actual scope statements are tested before converting them to active rules. Members, versions and measures could also be surgically manipulated.
- Transferring data - Upload or download member and measure data as files. Usually a large number of rows are fetched or uploaded.
- Querying model - Extract dimensions, measures, graphs etc. and their modelling details for the purposes of verification.
- Controlling access - create rules to grant or deny access to various types of data.

## Querying data

Use-cases

- Verify if dim or fact data uploaded through files has been uploaded correctly
- Verify if a scope statement worked or not
- Verify if a specific intersection is present while trying to debug some UI
- Verify if data is not present or just a UI filter issue
- Check hierarchy of some members
- Add more use-cases….

Usually this is achieved by using a select statement. In case the data is large and needs to be further manipulated in excel, then one would download the output of select data into files. Refer to Transfering data section for more details.

Learn to query data through IBPL

## Modifying data

Use-cases

- Erase data for certain measures while doing development or in case of errors. This is achieved through null assignment
- Initialize measures at certain intersections for later workflows to work properly.
- Create new intersections if not present through use cartesian or evaluatemember. Caution is advised when using cartesian assignment.
- Testing data manipulation rule before converting it into an active rule.

Usually this is achieved by executing scope statements. Regular scope statement doesn't create intersections except when using coalesce.

Learn to modify data through IBPL

## Transferring data

## Use-cases

- Extract data from mPower for further analysis in excel
- Verification of data in mPower through external computation tools like excel (where certain vlookup and string functions are available)
- Uploading data into mPower using flat files in csv or excel formats
- Instead of editing multiple values in UI which is tedious, upload a file with the measure data

Learn to Upload / Download data through IBPL

## Querying model

## Use-cases

- Investigate wrong data or absence of data on UI
- Confirm if data got loaded from external systems or files into the platform
- Bulk extraction of measures to create new measures or verify model after creation of large entities

Have a look at utilities besides the IBPL model queries.

Learn to query models through IBPL

## Controlling access

## Use-cases

- Hide irrelevant data from users
- Restrict access of UI elements to certain users
- Restrict read/write access for measure data
- Restrict data for users

Learn to control access through IBPL # Querying data Users query either

- Dimension data i.e. the members and their relationships
- Fact data i.e. the values of measures at appropriate granularity specified through dimension level attributes

## One also views the data by applying

- filtering based on members of certain level attributes or measure values
- sorting or ordering data in ascending or descending order of members or measure values
- limiting records in order to keep data volume for analysis low. The browser or other client may hang if too many rows are pulled. It's a good idea to limit data with any select query.

## Complex Query example

```
(  Select ( Version.[Version Name].[CurrentWorkingView]* Product.[ProductID]* Customer.[CustomerID]*Customer.[CustomerName]*Product.[SubCategory]* Customer.[State].filter(#.Name in {[Maharashtra], [Delhi], [Haryana]}) ) on row, ( {Measure.[PaymentMode], Measure.[UnitSold], Measure.[UnitCost], Measure.[UnitPrice]} ) on column where {Product.[Category].[Furniture]} ).filter(Measure.[PaymentMode] != [COD]); Select syntax Select ( [Product].[ProductID] * [Time].[Week] * {Measure.[UnitSold], Measure.[UnitPrice]} ) where {Version.[Version Name]. Filter (#. Key == 0)} orderby Measure.[UnitPrice] desc limit 6;
```

Result:

| ProductID       |   Week |   UnitSold | UnitPrice   |
|-----------------|--------|------------|-------------|
| TEC-AC-10001767 |      5 |          3 | 137,076     |
| TEC-MA-10001148 |      3 |          2 | 123,543     |
| TEC-MA-10000864 |      4 |          4 | 102,922     |
| FUR-FU-10002505 |      2 |          1 | 98,015      |

TEC-AC-10004659 1 3 93,606 FUR-FU-10003773 3 9 93,051

Above code contains below sections

1. [Product].[ProductID] * [Time].[Week] * : It defines scope or granularity against which measure data is fetched. In the data fetched first column will have all the ProductID and second column will have the Week as present in data. Multiple attributes are joined using *
2. -They are written as [Dimension].[Attribute]
3. -Version filter is mandatory . If skipped 0 rows will be fetched. In above example it has been used in where clause.
2. {Measure.[UnitSold], Measure.[UnitPrice]} : It defines the measures which are represented against above defined scope. It will show the UnitPrice and UnitSold for the particular ProductID and in particular week.
5. -Measure. has to be prepended before measure names and measure names should be enclosed in Square Bracket
3. where {Version.[Version Name].Filter(#.Key == 0)} : where is used to limit the scope of data fetched. In above example, data where Version Name with MemberKey is 0 (CurrentWorkingView) will be fetched.
7. -Key - This is a unique no. assigned to every attributemember. Besides Key, Name can also be used to find the exact attributemember if the key is not known
8. -Filter - This command filters the data according to query specified in the paratheses()
9. -# is used here to represent Version.[Version Name] on which filter query has been applied
4. orderby Measure.[UnitPrice] desc : It is used to order the list by UnitPrice. Generally orderby orders the list in ascending order. desc has to be appended if list is required in descending order
5. limit 6 : It is used get only 6 rows of the result. top and bottom can be used instead of limit to display top and bottom n rows respectively.

This is the basic statement which is used to Query data. Any ibpl command must end with ; . It doesn't matter if it is written in one single line or multiple lines. (1) &amp; (2) are mandatory. (3), (4) and (5) are optional.

```
Select ([Product].[ProductID] * [Time].[Week]) on row , ({Measure.[UnitSold], Measure.[UnitPrice]}) on column where {Version.[Version Name]. Filter (#. Key == 0)}; OR Select ([Product].[ProductID] * [Time].[Week]* Version.[Version Name].[CurrentWorkingView]) on row ,
```

```
({Measure.[UnitSold], Measure.[UnitPrice]}) on column ;
```

The above statements are equivalent and demonstrate multiple ways to write the same statement.

Below are two ways to write ibpl in on row on column syntax and without it using * operator. Usually UI queries are sent with row/column keywords.

```
SYNTAX: Select (< rows >) on row , ({< columns >}) on column ; SYNTAX: Select (< rows >*{< columns >}); print select ("Hello IBPL"); Result: Hello IBPL select (5+3*2); Result: 11 where clause SYNTAX: select {< select query >} where {<condition>};
```

Where clause is used to print a filtered list on which boolean &lt;condition&gt; is satisfied

|   S.No. | <condition>  examples                                         | Results                                                                  |
|---------|---------------------------------------------------------------|--------------------------------------------------------------------------|
|       1 | Measure.[PaymentMode]=='COD'                                  | All the orders in which payment mode  is COD will be printed             |
|       2 | Version.[Version  Name].[CurrentWorkingView]                  | Results from CurrentWorkingView  Version will be displayed               |
|       3 | (Customer.[City].[Mumbai]) ||  (Customer.[State].[Karnataka]) | Orders of Customers belonging to  Mumbai or Karnataka will be  displayed |

## post filter clause

Note: Instead of where clause filter can also be used for inner query

SYNTAX:( select &lt; select query &gt; where {&lt; inner condition&gt;}). filter (&lt; outer condition&gt;)

This is simple post filter that can also be used in querying. First whole table is created by select query and then it is filtered using filter condition. Where clause select only those values then create the table whereas filter filters the value after been selected.

for more information refer to filter Section

## select member data

You can select either one levelattribute or multiple level attributes for a dimension. The command expects dimension name followed by level attributes. If multiple level attributes are specified they need to be separated by * .

## select single member

```
select [Item].[Brand].[Fructis];
```

Without round brackets all the properties of the member including key, Name, DisplayName, Image (URL location) and 'active' status w.r.t. LS is displayed. If the member is inactive, that means it has been soft deleted from the system.

|   Key | Name    | DisplayName   | Brand$InActive   |
|-------|---------|---------------|------------------|
|     2 | Fructis | Fructis       | null             |

## select attribute members

```
select [Item].[Brand] limit 100;
```

This provides 100 records ( or lesser records if the number of members in Brand is less than 10). Increase limit if you want to ensure all records. With one level attribute, one need not enclose the level attribute in round brackets. Output may look like below (if there are just two members)

| Key    | Name    | DisplayName                   | Image                         | IsEditable                    |
|--------|---------|-------------------------------|-------------------------------|-------------------------------|
| 2      | Fructis | Fructis                       | null                          | true                          |
| 3      | Liril   | Liril                         | null                          | true                          |
| select | select  | ([Item].[Brand])  limit  100; | ([Item].[Brand])  limit  100; | ([Item].[Brand])  limit  100; |

Since the level attribute was enclosed in round brackets (i.e. parentheses) just the brands are displayed without properties. Output will look like below

Brand | -- | Fructis | Liril |

## select memberproperties

A levelattribute might have properties which are used mainly to display additional information for a member but not used in aggregation. One needs to use memberproperties clause to include them. One can include as many properties as needed (note the absence of a comma between curly brackets).

In example below attribute Location has two properties PinCode,ContactNumber and LocationGroup has a single property called InChargeName . The below output is visible if one clicks 'execute datasource' and not 'execute' in the debugging workspace. The normal 'execute' button gives the properties in JSON which may be difficult to read through.

```
select ([SCSLocation].[Location] * [SCSLocation].[LocationGroup]) on row , () on column
```

```
include memberproperties {[SCSLocation].[Location], [PinCode]} {[SCSLocation].[Location], [ContactNumber]} limit 5 ;
```

```
{[SCSLocation].[LocationGroup], [InChargeName]} Locati on LocationK ey PinCo de ContactNum ber LocationGro up LocationGroup Key InChargeNa me Happy Land 1 74871 +1 800-4337300 Connecticut 3 Mr. Groot
```

```
select multiple attributes select ([Item].[Brand] * [Item].[SKU]) limit 5;
```

More attributes could be included for Item dimension by using * . Only 5 are shown based on variety of sorting criteria, but more members are present. You can increase the limit based on the needs or put filters to narrow and fetch required data. In the example picked there are SKU's under Liril as well but not fetched due to limit.

| Brand   | SKU                  |
|---------|----------------------|
| Fructis | Fructis Shine - 100  |
| Fructis | Fructis Shine - 500  |
| Fructis | Fructis Strong - 100 |
| Fructis | Fructis Strong - 500 |
| Fructis | Fructis Strong - 650 |

## select measure data

You can select one or more level attributes (Version being mandatory) and one or more measures. By default the data is ordered by sort order of level attributes present in the granularity.

```
select ( [Version].[Version Name] * [Item].[SKU] * [Time].[Year-Month]  * {Measure.[Sales], Measure.[Price]} ) limit 5;
```

Another way to write same statement (fired by UI when fetching data), but provides same data.

```
select ( [Version].[Version Name] * [Item].[SKU] * [Time].[Year-Month] ) on row , ( {Measure.[Sales], Measure.[Price]} ) on column limit 5;
```

| Version Name       | SKU                  | Year-Month   |   Sales | Price        |
|--------------------|----------------------|--------------|---------|--------------|
| CurrentWorkingView | Fructis Shine - 500  | 2016-M5      |     100 | 250.00000000 |
| CurrentWorkingView | Fructis Strong - 500 | 2016-M6      |     100 | 250.00000000 |
| CurrentWorkingView | Fructis Strong - 650 | 2016-M7      |     100 |              |

| CurrentWorkingView   | Liril Shine - 40   | 2016-M8   |   100 |   20.00000000 |
|----------------------|--------------------|-----------|-------|---------------|
| CurrentWorkingView   | Liril Shine - 75   | 2016-M9   |   100 |          37.5 |

## filter members

One can use various types of filters such as namesets, membersets, filter expressions applied to the level attributes in a filter. Below as an example using membersets using relatedmembers and nameset CWV for CurrentWorkingView.

```
select ( &CWV * [Item].[Brand].[Fructis].relatedmembers([SKU]) * [Time].[Year-Month]  * {Measure.[Sales], Measure.[Price]} ) limit 5;
```

| Version Name       | SKU                  | Year-Month   |   Sales | Price        |
|--------------------|----------------------|--------------|---------|--------------|
| CurrentWorkingView | Fructis Shine - 100  | 2016-M3      |     100 | 50.00000000  |
| CurrentWorkingView | Fructis Strong - 100 | 2016-M4      |     100 | 50.00000000  |
| CurrentWorkingView | Fructis Shine - 500  | 2016-M5      |     100 | 250.00000000 |
| CurrentWorkingView | Fructis Strong - 500 | 2016-M6      |     100 | 250.00000000 |
| CurrentWorkingView | Fructis Strong - 650 | 2016-M7      |     100 |              |

## filter using where

Where clause can be used to filter by either members or measures. Below is an example to filter by Brand

```
select ( [Version].[Version Name] * [Item].[SKU] * [Time].[Year-Month]  * {Measure.[Sales], Measure.[Price]} ) where {[Item].[Brand].[Fructis]} limit 5;
```

| Version Name       | SKU                  | Year-Month   |   Sales | Price        |
|--------------------|----------------------|--------------|---------|--------------|
| CurrentWorkingView | Fructis Shine - 100  | 2016-M3      |     100 | 50.00000000  |
| CurrentWorkingView | Fructis Strong - 100 | 2016-M4      |     100 | 50.00000000  |
| CurrentWorkingView | Fructis Shine - 500  | 2016-M5      |     100 | 250.00000000 |
| CurrentWorkingView | Fructis Strong - 500 | 2016-M6      |     100 | 250.00000000 |
| CurrentWorkingView | Fructis Strong - 650 | 2016-M7      |     100 |              |

One can use multiple conditions such as member and measures together in the where clause. The final condition is a logical AND of all the conditions provided.

```
select ( [Version].[Version Name] * [Item].[SKU] * [Time].[Year-Month]  * {Measure.[Sales], Measure.[Price]} ) where {[Item].[Brand].[Fructis], Measure.[Sales] > 200, Measure.[Price] > 200,
```

```
Measure.[Price] < 350} limit 10;
```

| Version Name       | SKU                  | Year-Month   |   Sales |   Price |
|--------------------|----------------------|--------------|---------|---------|
| CurrentWorkingView | Fructis Shine - 500  | 2018-M2      |     300 |     250 |
| CurrentWorkingView | Fructis Strong - 500 | 2018-M3      |     300 |     250 |
| CurrentWorkingView | Fructis Strong - 650 | 2018-M4      |     300 |     325 |

## order using orderby

Measure data can be ordered by one measure either in ascending or descending order. Example with descending order for one measure.

```
select ( [Version].[Version Name] * [Item].[SKU] * [Time].[Year-Month]  * {Measure.[Sales], Measure.[Price]} ) orderby Measure.[Price] desc limit 5;
```

| Version Name       | SKU                  | Year-Month   | Sales   |   Price |
|--------------------|----------------------|--------------|---------|---------|
| CurrentWorkingView | Fructis Strong - 650 | 2017-M2      | 200     |     325 |
| CurrentWorkingView | Fructis Strong - 650 | 2017-M9      | 200     |     325 |
| CurrentWorkingView | Fructis Strong - 650 | 2018-M4      | 300     |     325 |
| CurrentWorkingView | Fructis Shine - 500  | 2016-M5      | 100     |     250 |
| CurrentWorkingView | Fructis Shine - 500  | 2016-M12     |         |     250 |

Example with ascending order for a measure which contains null value for an intersection. Null is counted as zero for sorting.

```
select ( [Version].[Version Name] * [Item].[SKU] * [Time].[Year-Month]  * {Measure.[Sales], Measure.[Price]} ) orderby Measure.[Sales] asc limit 5;
```

| Version Name       | SKU                 | Year-Month   | Sales   |   Price |
|--------------------|---------------------|--------------|---------|---------|
| CurrentWorkingView | Fructis Shine - 500 | 2016-M12     |         |   250   |
| CurrentWorkingView | Liril Shine - 40    | 2016-M1      | 100     |    20   |
| CurrentWorkingView | Liril Shine - 40    | 2016-M8      | 100     |    20   |
| CurrentWorkingView | Liril Shine - 75    | 2016-M2      | 100     |    37.5 |
| CurrentWorkingView | Liril Shine - 75    | 2016-M9      | 100     |    37.5 |

One can also order by member properties. Note that one needs to specify whether it's name or key or any other property and not just level attribute.

```
select ([Version].[Version Name]  * [Time].[Year-Month] * [Item].[SKU] * {Measure.[SalesPrice], Measure.[UnitsSold]} )
```

orderby [Version].[Version Name].Name desc , [Item].[SKU].Name asc limit 100;

| Version Name       | Year-Month   | SKU                  | SalesPrice   |   UnitsSold |
|--------------------|--------------|----------------------|--------------|-------------|
| ScenarioAllPlays   | 2016-M5      | Fructis Shine - 500  | 250          |         100 |
| ScenarioAllPlays   | 2016-M6      | Fructis Strong - 500 |              |         100 |
| ScenarioAllPlays   | 2016-M1      | Liril Shine - 40     | 20           |           1 |
| CurrentWorkingView | 2016-M5      | Fructis Shine - 500  | 250          |         100 |
| CurrentWorkingView | 2016-M6      | Fructis Strong - 500 |              |         100 |
| CurrentWorkingView | 2016-M1      | Liril Shine - 40     | 20           |           1 |

One can also order by both member properties and measures as shown in example below.

```
select ([Version].[Version Name]  * [Time].[Year-Month] * [Item].[SKU] * {Measure.[SalesPrice], Measure.[UnitsSold]} ) orderby [Version].[Version Name].Name desc , Measure.[SalesPrice] desc limit 100;
```

| Version Name       | Year-Month   | SKU                  | SalesPrice   |   UnitsSold |
|--------------------|--------------|----------------------|--------------|-------------|
| ScenarioAllPlays   | 2016-M5      | Fructis Shine - 500  | 250          |         100 |
| ScenarioAllPlays   | 2016-M1      | Liril Shine - 40     | 20           |           1 |
| ScenarioAllPlays   | 2016-M6      | Fructis Strong - 500 |              |         100 |
| CurrentWorkingView | 2016-M5      | Fructis Shine - 500  | 250          |         100 |
| CurrentWorkingView | 2016-M1      | Liril Shine - 40     | 20           |           1 |
| CurrentWorkingView | 2016-M6      | Fructis Strong - 500 |              |         100 |

Lastly an example of sorting by member property which was user created and not available by default like Name or Key. One has to do 'execute datasource' in debugging workspace and not just 'execute' for properties to show up. PinCode is of string data type.

```
select ([Geo].[Region] * [Geo].[City]) on row , () on column include memberproperties {[Geo].[City], [PinCode]} orderby [Geo].[City].[PinCode] desc limit 10 ;
```

| Region   |   RegionKey | Region$InActive   | City      |   CityKey |   PinCode |
|----------|-------------|-------------------|-----------|-----------|-----------|
| South    |           2 | null              | Bangalore |         4 |    560001 |
| South    |           2 | null              | Bhopal    |         5 |    462001 |
| North    |           1 | null              | Nagpur    |         3 |    440001 |
| North    |           1 | null              | Jaipur    |         2 |    302001 |

## North 1 null Delhi 1 110001

## Modifying data

## Modifying member data

Member data (eg. properties, names) is updated through

- createmember command to create members
- updatemember command to update members and their properties
- copymember command to copy members
- deletemember command to delete members

Learn to modify members

## Modifying fact data

Fact data i.e. measure values and edge values are updated through

- scope statement
- copymeasure command
- update command to measure a single intersection (usually fired from UI)

In some places graph data maybe mentioned separately besides fact data. It's mainly used to differentiate edges (of graphs) from measures. However, in most place fact data will include both measure values and edge values.

## Learn to modify facts

## Modifying version data

Version data is special since all measures are grained by this and it's key data. It's updated through

- createversion command to create version either a normal version or scoped version
- updateversion command to update version properties
- deleteversion command to detele a version
- createscenario command to create scenario
- updatescenario command to update whole scenario or scoped part of it

Scenarios are copies of versions which can be modified by users. Versions except for CurrentWorkingView are archives of fact data and not modified by users.

## Modify Members content

## createmember

This command is used to add new members. One should create members along with their ancestors in case there is a hierarchy in the respective dimension.

Below statement returns zero rows indicating that there are no members with name Liril Strong - 100 in [Item].[SKU] .

```
select ([Item].[Brand] * [Item].[SKU].[Liril Strong - 100]) limit 5;
```

One can create Liril Strong - 100 member with Liril as parent using below command. Liril should exist as a member of Brand level attribute for this to work.

```
createmember([Item].[SKU] = { , "Liril Strong - 100"} , [Item].[Brand] = { , "Liril"} );
```

One must note that the first element inside curly brackets it empty since that's the key of the member. The key in this example is being auto-generated based on the level attribute configuration, hence not specified but name has been specified. If there was no parent for SKU levelattribute then one need to not specify the second argument indicating Brand .

The previous select statement would now return one row indicating new member has been created.

```
Brand SKU Liril Liril Strong - 100
```

For time dimension where usually the key is specified one can use below command to create member at the top most level say year.

```
createmember([Time].[Year] = {todatetime("2019-01-01 00:00:00.000"), "2019"});
```

One can observe that the key has been specified by using todatetime function to convert string to datetime datatype. Above will work as long as year is the top most level in Time dimension, else one must specify the ancestor or parent of that member.

## updatemember properties

One can update a specific member's properties using the updatemember command. In below example attribute Location has two properties PinCode and ContactNumber . One can update a single property as well by skipping additional arguments. Do note that for properties the value is specified as first argument inside the curly brackets where normally one places key of a member.

```
updatemember([SCSLocation].[Location]={,"Happy Land"}, [SCSLocation].[PinCode]={"74871",}, [SCSLocation].[ContactNumber]={"+1 800-433-7300 ",});
```

Once updated the below select statement using 'execute datasource' would show the member properties.

```
select ([SCSLocation].[Location] * [SCSLocation].[LocationGroup]) on row , () on column include memberproperties {[SCSLocation].[Location], [PinCode]} {[SCSLocation].[Location], [ContactNumber]} {[SCSLocation].[LocationGroup], [InChargeName]} limit 5 ;
```

| Locati on   |   LocationK ey |   PinCo de | ContactNum ber   | LocationGro up   |   LocationGroup Key | InChargeNa me   |
|-------------|----------------|------------|------------------|------------------|---------------------|-----------------|
| Happy  Land |              1 |      74871 | +1 800-433- 7300 | Connecticut      |                   3 | Mr. Groot       |

## updatemember parents

In the previous example Happy Land belongs to Connecticut . One can change the parent of Happy Land to Rhode Island due to say logistics sourcing rules using updatemember as follows.

```
updatemember([SCSLocation].[Location]={, "Happy Land"}, [SCSLocation].[LocationGroup]={, "Rhode Island"}); Location LocationGroup Happy Land Rhode Island
```

## deletemember toplevel

If one needs to cleanup members from the system, one can use deletemember commands. This command deletes members one at a time. For below to work one must not have any child member associated with the member being deleted.

Let's assume two LocationGroups exist Rhode Island and Connecticut . Rhode Island has Happy Land as a location under it.

```
select [SCSLocation].[LocationGroup].[Rhode Island] ;
```

```
Key Name DisplayName LocationGroup$InActive InChargeName 4 Rhode Island Rhode Island null null
```

One can execute below command to delete it.

```
deletemember ([SCSLocation].[LocationGroup] = {,[Rhode Island]}) ;
```

Post this Rhode Island will be soft deleted and marked as inactive in the system. This can be verified by selecting only this member as done before the deletemember. Output would change as shown below.

|   Key | Name         | DisplayName   | LocationGroup$InActive   | InChargeName   |
|-------|--------------|---------------|--------------------------|----------------|
|     4 | Rhode Island | Rhode Island  | true                     | null           |

One must do a purgemember to remove the members completely from the system.

## deletember cascade

It may not be a good idea to delete top level members without deleting the children. This can be achieved through the cascade clause of deletemember. Continuing from previous example, if we want Happy Land location to be deleted when we delete the location group.

Before we execute the cascade delete Happy Land is active.

|   Key | Name       | DisplayName   | Location$InActive   |   PinCode | ContactNumber   |
|-------|------------|---------------|---------------------|-----------|-----------------|
|     1 | Happy Land | Happy Land    | null                |     74871 | +1 800-433-7300 |

Below command deletes the parent Rhode Island LocationGroup and marks it inactive. Happy Land will be marked for deletion but not marked as inactive. However it will be deleted when purge members command is executed.

```
deletemember ([SCSLocation].[LocationGroup] = {,[Rhode Island]}) cascade ;
```

## purge members

This command removes members permanently. It's not a reversible change and should be used with caution.

Technically speaking it removes all members marked as 'inactive' by the delete command and also the children marked for deletion by the cascade clause.

Do experiment on dev environments w.r.t. cascade. Leaving hanging members or orphan members that is without any parents is not a good practice and may lead to strange computational behaviours.

purge members([SCSLocation].[LocationGroup]);

## Update measure

This command is used to upadte the value of a member in a particular measure.

Example:

```
Update Measure.[UnitPrice]@(Product.[ProductID].[FUR-BO-10004834], Version.[Version Name].[CurrentWorkingView])=12092;
```

result: Measure Unit Price will be updated to 12092 where the product ID is FUR-BO10004834 # Modify facts

Fact data can be modified through scope , exec plugin, copymeasure and update statements.

how to define different scopes in mpower.

## cartesian scope

This can be used to create a small set of intersections at all possible combinations of members specified and assign a constant value to it. Be careful because if member filters are not specified, a lot of intersections would be created. Fact file upload might be a preferable method to create large number of specific intersections.

Assuming granularity as specified in the select statement and 4 members in Time.Year level attribute viz 2016, 2017, 2018 and 2019 in Year levelattribute, below select query returns zero records to start with. The measure had been created for counting SKU's active in a year. It can be aggregated along various dimensions.

```
select ([Version].[Version Name].[CurrentWorkingView] * [Time].[Year] * [Item].[SKU]. filter (#.Name in }) * {Measure.[SKUByYear]}) limit 10;
```

```
cartesian scope : ([Version].[Version Name].[CurrentWorkingView] * [Time].[Year] * [Item].[SKU]. filter (#.Name in })); Measure.[SKUByYear] = 1; end scope ;
```

```
{"Liril Fresh - 75", "Fructis Shine - 100" Below cartesian scope limits the SKU's to two members but takes all Year members. {"Liril Fresh - 75", "Fructis Shine - 100"
```

The earlier select statement would return 8 records now.

| Version Name       |   Year | SKU                 |   SKUByYear |
|--------------------|--------|---------------------|-------------|
| CurrentWorkingView |   2016 | Fructis Shine - 100 |           1 |

| CurrentWorkingView   |   2016 | Liril Fresh - 75    |   1 |
|----------------------|--------|---------------------|-----|
| CurrentWorkingView   |   2017 | Fructis Shine - 100 |   1 |
| CurrentWorkingView   |   2017 | Liril Fresh - 75    |   1 |
| CurrentWorkingView   |   2018 | Fructis Shine - 100 |   1 |
| CurrentWorkingView   |   2018 | Liril Fresh - 75    |   1 |
| CurrentWorkingView   |   2019 | Fructis Shine - 100 |   1 |
| CurrentWorkingView   |   2019 | Liril Fresh - 75    |   1 |

## regular scope

Regular scope statement can be used to assign null or constant values and also carry out computations.

If SKUByYear had 8 intersections as described in previous example for 4 years 2016, 2017, 2018 and 2019 and two SKU's 'Liril Fresh - 75' and 'Fructis Shine - 100' , then below scope statement would null out 4 of them for 'Liril Fresh - 75' and a select query for SKUByYear would return only 4 rows for the other SKU.

```
scope : ([Version].[Version Name].[CurrentWorkingView] * [Time].[Year] * [Item].[SKU].[Liril Fresh - 75]); Measure.[SKUByYear] = null ; end scope ;
```

| Version Name       |   Year | SKU                 |   SKUByYear |
|--------------------|--------|---------------------|-------------|
| CurrentWorkingView |   2016 | Fructis Shine - 100 |           1 |
| CurrentWorkingView |   2017 | Fructis Shine - 100 |           1 |
| CurrentWorkingView |   2018 | Fructis Shine - 100 |           1 |
| CurrentWorkingView |   2019 | Fructis Shine - 100 |           1 |

Let's say you want to compute Revenue = UnitsSold * SKUPrice .

One could run a computation based on two measures as shown below. Note that the computation updates Revenue only in the case where Price is not zero else it's kept null ( for 'Liril Shine - 75' for year '2017').

```
scope : ([Version].[Version Name].[CurrentWorkingView] * [Time].[Year] * [Item].[SKU]); Measure.[Revenue] = if ( Measure.[SKUPrice] != 0) then Measure.[SKUPrice] * Measure.[UnitsSold] else null ; end scope ;
```

| Version Name       |   Year | SKU                |   SKUPrice |   UnitsSold | Revenue   |
|--------------------|--------|--------------------|------------|-------------|-----------|
| CurrentWorkingView |   2016 | Liril Shine - 75   |        100 |          20 | 2,000     |
| CurrentWorkingView |   2016 | Liril Strong - 100 |        150 |          10 | 1,500     |

```
CurrentWorkingView 2017 Liril Shine - 75 0 0 CurrentWorkingView 2017 Liril Strong - 100 160 11 1,760
```

## evaluatemember scope

Usually evaluatemember is used to create intersections in an finer granularity on the basis of another measure at a coarser granularity. To give an example, consider SKULanchDate and SKUWithdrawalDate measures are available by Version and SKU . We need to populate SKUActiveFlag grained by Version, SKU and Year-Month. SKUActiveFlag needs to be marked as 1 in buckets where the SKU is active and new intersection created. This involves comparing the Year-Month levelattribute key with the measure.

Below are the input measures fetched using a select query. Dates are in mm/dd/yyyy format.

```
select ([Version].[Version Name].[CurrentWorkingView]  * [Item].[SKU] * {Measure.[SKULaunchDate], Measure.[SKUWithdrawalDate]} ) limit 100;
```

| Version Name       | SKU                | SKULaunchDate   | SKUWithdrawalDate   |
|--------------------|--------------------|-----------------|---------------------|
| CurrentWorkingView | Liril Fresh - 75   | 2/10/2017       | 4/10/2017           |
| CurrentWorkingView | Liril Strong - 100 | 3/20/2018       | 6/20/2018           |

The SKUActiveFlag and it's measuregroup have no data (i.e. no intersections) to begin with. Below scope statement will populate them using evaluate member. Note that each Year-Month member's key is set to the 1st day of that month.

```
evaluatemember scope : ([Version].[Version Name].[CurrentWorkingView] * [Item].[SKU] * [Time].[Year-Month]); Measure.[SKUActiveFlag] = if (Time.#. Key >= Measure.[SKULaunchDate] && Time.#. Key < Measure.[SKUWithdrawalDate]) then 1 else null ;
```

## end scope ;

| Version Name       | Year-Month   | SKU                |   SKUActiveFlag |
|--------------------|--------------|--------------------|-----------------|
| CurrentWorkingView | 2017-M3      | Liril Fresh - 75   |               1 |
| CurrentWorkingView | 2017-M4      | Liril Fresh - 75   |               1 |
| CurrentWorkingView | 2018-M4      | Liril Strong - 100 |               1 |
| CurrentWorkingView | 2018-M5      | Liril Strong - 100 |               1 |
| CurrentWorkingView | 2018-M6      | Liril Strong - 100 |               1 |

## recurrence scope

Recurrence scope is used to model time-series based computations wherein the computation in a particular time bucket depends on the result of the computation of the previous time bucket.

## Example:

Let's say user wants to calculate Inventory for each week depending on the computation of the previous week Inventory and current week stock and demand.

| Item   |   Week | Inventory   |   Stock |   Demand |
|--------|--------|-------------|---------|----------|
| S9+    |      1 | 10          |      14 |       12 |
| S9+    |      2 |             |       8 |       10 |
| S9+    |      3 |             |       5 |       15 |
| S9+    |      4 |             |       8 |       12 |

computation formula:

I(t) = I(t - 1) + S(t) - D(t)

```
where I denote inventory, S is stock, D is demand and t is the current time. query for regular scope. scope:([Version].[Version Name] * [Item].[SKU] * [Time].[Week]); Measure.[Inventory] = if (Time.#.Key == &PastWeeks.element(0).Key) then Measure.[Inventory] else coalesce(Measure.[Inventory]@ (Time.#.LeadOffset(-1)),0) + coalesce(Measure.[Stock],0)coalesce(Measure.[Demand],0); end scope; query for recurrence scope recurrence scope: ([Version].[Version Name] * [Item].[SKU] * [Time].[Week]); Measure.[Inventory] = if (Time.#.Key == &PastWeeks.element(0).Key) then Measure.[Inventory] else coalesce(Measure.[Inventory]@ (Time.#.LeadOffset(-1)),0) + coalesce(Measure.[Stock],0)coalesce(Measure.[Demand],0);
```

## end scope;

Result: Comparison between regular and recurrence scope.

| Item   |   Week |   Stock |   Demand |   Expected value  for Inventory |   'regular scope'  Inventory |   'recurrence  scope'  Inventory |
|--------|--------|---------|----------|---------------------------------|------------------------------|----------------------------------|
| S9+    |      1 |      14 |       12 |                              10 |                           10 |                               10 |
| S9+    |      2 |       8 |       10 |                               8 |                            8 |                                8 |
| S9+    |      3 |       5 |       12 |                               1 |                           -7 |                                1 |
| S9+    |      4 |       8 |        9 |                               0 |                           -1 |                                0 |

Note: the result for regular scope inventory is wrong for week 3 and 4.

In the above result, regular scope inventory is the inventory when the scope is regular scope and the recurrence scope inventory is the inventory when the scope is recurrence scope

Explanation: The Regular scope does the calculation on existing values not the updated values. Whereas In recurrence scope case the Measure gets updated after each calculation step.

## Similar Example:

| Version Name       |   Week | SKU     |   Stock | Inventory   |   Demand |
|--------------------|--------|---------|---------|-------------|----------|
| CurrentWorkingView |      1 | Cinthol |      13 | 6           |        2 |
| CurrentWorkingView |      2 | Cinthol |      10 |             |        1 |
| CurrentWorkingView |      3 | Cinthol |      10 |             |       19 |
| CurrentWorkingView |      4 | Cinthol |       4 |             |        3 |
| CurrentWorkingView |      5 | Cinthol |       2 |             |        5 |

select (Version.[Version Name]*Item.[SKU].[Cinthol]*Time.[Week]* {Measure.[Inventory],Measure.[Stock],Measure.[ Demand ]});

## Result:

| Version Name       | SKU     |   Week |   Inventory |   Stock |   Demand |
|--------------------|---------|--------|-------------|---------|----------|
| CurrentWorkingView | Cinthol |      1 |           6 |      13 |        2 |
| CurrentWorkingView | Cinthol |      2 |          15 |      10 |        1 |
| CurrentWorkingView | Cinthol |      3 |           6 |      10 |       19 |
| CurrentWorkingView | Cinthol |      4 |           7 |       4 |        3 |
| CurrentWorkingView | Cinthol |      5 |           4 |       2 |        5 |

## block scope

A block scope is a regular scope whose LS computation method has been changed for achieving better performance. Please refer to GraphCube guide for details on some restrictions on it. Below is a simplistic explanation of the same.

Let's say you want to calculate error in forecast when compared to the actual orders for an item we ship from our factory. Two measures Forecast and Actuals are already available. One can compute the error using two calculations

- Error = Forecast - Actuals
- AbsoluteError = ABS ( Forecast - Actuals )

The above are used just for illustration purposes, refer to any forecasting book for better ways to compute forecast accuracy. If one were to sum up the two measures, in case of calculation 1 the total error at an aggregate level might be lower due to +ve and -ve numbers, but in calcualation 2 the total error at an aggregate level will reflect it better.

All the measures belong to the same measuregroup. Below is a block scope.

```
block scope : ([Version].[Version Name].[CurrentWorkingView] * [Time].[Year] * [Item].[SKU]); Measure.[Error] = Measure.[Forecast] - Measure.[Actuals]; Measure.[AbsoluteError] = abs( Measure.[Forecast] - Measure.[Actuals] ); end scope ;
```

Without block scope (i.e. regular scope) the computation would run according to the sequence 1,2,3,4 .. as indicated below

| Version Name       | Year-Month   | SKU                |   ErrorComp |   AbsoluteErrorComp |
|--------------------|--------------|--------------------|-------------|---------------------|
| CurrentWorkingView | 2017-M3      | Liril Fresh - 75   |           1 |                   5 |
| CurrentWorkingView | 2017-M4      | Liril Fresh - 75   |           2 |                   6 |
| CurrentWorkingView | 2018-M4      | Liril Strong - 100 |           3 |                   7 |
| CurrentWorkingView | 2018-M5      | Liril Strong - 100 |           4 |                   8 |

With block scope the computation would run according to the sequence 1,2,3,4 .. as indicated below. The computation happens row-wise processing each row at once and hence reading-writing row for all the columns in that row. This is faster than the regular scope approach.

| Version Name       | Year-Month   | SKU                |   ErrorComp |   AbsoluteErrorComp |
|--------------------|--------------|--------------------|-------------|---------------------|
| CurrentWorkingView | 2017-M3      | Liril Fresh - 75   |           1 |                   1 |
| CurrentWorkingView | 2017-M4      | Liril Fresh - 75   |           2 |                   2 |
| CurrentWorkingView | 2018-M4      | Liril Strong - 100 |           3 |                   3 |
| CurrentWorkingView | 2018-M5      | Liril Strong - 100 |           4 |                   4 |

The above is only an analogy for ease of understanding. The actual GraphCube code may implement it differently.

## spread scope

There are various types of spreading available within the system. Please refer to the Graph Cube guide for configuration of the same. Below focusses on how the syntax works for the same.

One may have data for launch and withdrawal dates grained by Version and SKU. This could be converted to time-grained SKUActiveFlag using evaluatemember scope. It might happen that one needs to setup an adjustment factor which needs to sum up to a specific value at aggregate level, but needs to spread equally in all the buckets in which SKUActiveFlag is setup.

Spreading config has two settings

| Basis Measure Type       | Basis Measure   | SpreadingType      |
|--------------------------|-----------------|--------------------|
| Basis Measure            | SKUActiveFlag   | DistributeToLeaves |
| Assortment Basis Measure | SKUActiveFlag   | DistributeToLeaves |

Below two spread scope statements would update SKUAdjFactor as indicated in the table after the statements.

```
spread scope : ([Version].[Version Name].[CurrentWorkingView] * [Item].[SKU].[Liril Fresh - 75]); Measure.[SKUAdjFactor] = 20; end scope ; spread scope : ([Version].[Version Name].[CurrentWorkingView] * [Item].[SKU].[Liril Strong - 100]); Measure.[SKUAdjFactor] = 100; end scope ;
```

| Version Name       | SKU                | Year-Month   |   SKUActiveFlag |   SKUAdjFactor |
|--------------------|--------------------|--------------|-----------------|----------------|
| CurrentWorkingView | Liril Fresh - 75   | 2017-M3      |               1 |             10 |
| CurrentWorkingView | Liril Fresh - 75   | 2017-M4      |               1 |             10 |
| CurrentWorkingView | Liril Strong - 100 | 2018-M4      |               1 |             33 |
| CurrentWorkingView | Liril Strong - 100 | 2018-M5      |               1 |             33 |
| CurrentWorkingView | Liril Strong - 100 | 2018-M6      |               1 |             33 |

## graph scope

Example:

Graph- Production Pegging

| From  To                                                                                                                                                                                                                                                                                                                                                                                                                                           | From  To                                                                                                                                                                                                                                                                                                                                                                                                                                           | From  To                                                                                                                                                                                                                                                                                                                                                                                                                                           | From  To                                                                                                                                                                                                                                                                                                                                                                                                                                           | From  To                                                                                                                                                                                                                                                                                                                                                                                                                                           | From  To                                                                                                                                                                                                                                                                                                                                                                                                                                           | From  To                                                                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Item.[Item]                                                                                                                                                                                                                                                                                                                                                                                                                                        | Item.[Item]                                                                                                                                                                                                                                                                                                                                                                                                                                        | Item.[Item]                                                                                                                                                                                                                                                                                                                                                                                                                                        | Item.[Item]                                                                                                                                                                                                                                                                                                                                                                                                                                        | Item.[Item]                                                                                                                                                                                                                                                                                                                                                                                                                                        | Item.[Item]                                                                                                                                                                                                                                                                                                                                                                                                                                        | Item.[Item]                                                                                                                                                                                                                                                                                                                                                                                                                                        | Item.[Item]                                                                                                                                | Item.[Item]                                                                                                                                | Item.[Item]                                                                                                                                | Item.[Item]                                                                                                                                | Item.[Item]                                                                                                                                | Item.[Item]                                                                                                                                |
| Location.[Location]                                                                                                                                                                                                                                                                                                                                                                                                                                | Location.[Location]                                                                                                                                                                                                                                                                                                                                                                                                                                | Location.[Location]                                                                                                                                                                                                                                                                                                                                                                                                                                | Location.[Location]                                                                                                                                                                                                                                                                                                                                                                                                                                | Location.[Location]                                                                                                                                                                                                                                                                                                                                                                                                                                | Location.[Location]                                                                                                                                                                                                                                                                                                                                                                                                                                | Location.[Location]                                                                                                                                                                                                                                                                                                                                                                                                                                | Location.[Location]                                                                                                                        | Location.[Location]                                                                                                                        | Location.[Location]                                                                                                                        | Location.[Location]                                                                                                                        | Location.[Location]                                                                                                                        | Location.[Location]                                                                                                                        |
| Activity.[Activity]                                                                                                                                                                                                                                                                                                                                                                                                                                | Activity.[Activity]                                                                                                                                                                                                                                                                                                                                                                                                                                | Activity.[Activity]                                                                                                                                                                                                                                                                                                                                                                                                                                | Activity.[Activity]                                                                                                                                                                                                                                                                                                                                                                                                                                | Activity.[Activity]                                                                                                                                                                                                                                                                                                                                                                                                                                | Activity.[Activity]                                                                                                                                                                                                                                                                                                                                                                                                                                | Activity.[Activity]                                                                                                                                                                                                                                                                                                                                                                                                                                | Order.[Order Name]                                                                                                                         | Order.[Order Name]                                                                                                                         | Order.[Order Name]                                                                                                                         | Order.[Order Name]                                                                                                                         | Order.[Order Name]                                                                                                                         | Order.[Order Name]                                                                                                                         |
| Time.[FiscalWeek]  Time.[FiscalWeek]                                                                                                                                                                                                                                                                                                                                                                                                               | Time.[FiscalWeek]  Time.[FiscalWeek]                                                                                                                                                                                                                                                                                                                                                                                                               | Time.[FiscalWeek]  Time.[FiscalWeek]                                                                                                                                                                                                                                                                                                                                                                                                               | Time.[FiscalWeek]  Time.[FiscalWeek]                                                                                                                                                                                                                                                                                                                                                                                                               | Time.[FiscalWeek]  Time.[FiscalWeek]                                                                                                                                                                                                                                                                                                                                                                                                               | Time.[FiscalWeek]  Time.[FiscalWeek]                                                                                                                                                                                                                                                                                                                                                                                                               | Time.[FiscalWeek]  Time.[FiscalWeek]                                                                                                                                                                                                                                                                                                                                                                                                               |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |
| Lot.[Lot ID  Edge Property                                                                                                                                                                                                                                                                                                                                                                                                                         | Lot.[Lot ID  Edge Property                                                                                                                                                                                                                                                                                                                                                                                                                         | Lot.[Lot ID  Edge Property                                                                                                                                                                                                                                                                                                                                                                                                                         | Lot.[Lot ID  Edge Property                                                                                                                                                                                                                                                                                                                                                                                                                         | Lot.[Lot ID  Edge Property                                                                                                                                                                                                                                                                                                                                                                                                                         | Lot.[Lot ID  Edge Property                                                                                                                                                                                                                                                                                                                                                                                                                         | Lot.[Lot ID  Edge Property                                                                                                                                                                                                                                                                                                                                                                                                                         | Customer.[CustomerID]                                                                                                                      | Customer.[CustomerID]                                                                                                                      | Customer.[CustomerID]                                                                                                                      | Customer.[CustomerID]                                                                                                                      | Customer.[CustomerID]                                                                                                                      | Customer.[CustomerID]                                                                                                                      |
| Material Production Pegged Quantity                                                                                                                                                                                                                                                                                                                                                                                                                | Material Production Pegged Quantity                                                                                                                                                                                                                                                                                                                                                                                                                | Material Production Pegged Quantity                                                                                                                                                                                                                                                                                                                                                                                                                | Material Production Pegged Quantity                                                                                                                                                                                                                                                                                                                                                                                                                | Material Production Pegged Quantity                                                                                                                                                                                                                                                                                                                                                                                                                | Material Production Pegged Quantity                                                                                                                                                                                                                                                                                                                                                                                                                | Material Production Pegged Quantity                                                                                                                                                                                                                                                                                                                                                                                                                |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |
| MaterialConstraintQty                                                                                                                                                                                                                                                                                                                                                                                                                              | MaterialConstraintQty                                                                                                                                                                                                                                                                                                                                                                                                                              | MaterialConstraintQty                                                                                                                                                                                                                                                                                                                                                                                                                              | MaterialConstraintQty                                                                                                                                                                                                                                                                                                                                                                                                                              | MaterialConstraintQty                                                                                                                                                                                                                                                                                                                                                                                                                              | MaterialConstraintQty                                                                                                                                                                                                                                                                                                                                                                                                                              | MaterialConstraintQty                                                                                                                                                                                                                                                                                                                                                                                                                              |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |
| MaterialConstraintType  Select  ( From .[Time].[Fiscal Week]*  From .[Item].[Item] *  From To .[Customer].[Customer Name] *  To To .[ Order ].[ Order  Name] *  To .[Time].[Fiscal Week])  ({Edge.[Production_Pegging].[Material Production Pegged Quantity] ,  Edge.[Production_Pegging].[MaterialConstraintQty] ,  Edge.[Production_Pegging].[MaterialConstraintType]})  where  {RelationshipType.[Production_Pegging], Version.[Version Name]}; | MaterialConstraintType  Select  ( From .[Time].[Fiscal Week]*  From .[Item].[Item] *  From To .[Customer].[Customer Name] *  To To .[ Order ].[ Order  Name] *  To .[Time].[Fiscal Week])  ({Edge.[Production_Pegging].[Material Production Pegged Quantity] ,  Edge.[Production_Pegging].[MaterialConstraintQty] ,  Edge.[Production_Pegging].[MaterialConstraintType]})  where  {RelationshipType.[Production_Pegging], Version.[Version Name]}; | MaterialConstraintType  Select  ( From .[Time].[Fiscal Week]*  From .[Item].[Item] *  From To .[Customer].[Customer Name] *  To To .[ Order ].[ Order  Name] *  To .[Time].[Fiscal Week])  ({Edge.[Production_Pegging].[Material Production Pegged Quantity] ,  Edge.[Production_Pegging].[MaterialConstraintQty] ,  Edge.[Production_Pegging].[MaterialConstraintType]})  where  {RelationshipType.[Production_Pegging], Version.[Version Name]}; | MaterialConstraintType  Select  ( From .[Time].[Fiscal Week]*  From .[Item].[Item] *  From To .[Customer].[Customer Name] *  To To .[ Order ].[ Order  Name] *  To .[Time].[Fiscal Week])  ({Edge.[Production_Pegging].[Material Production Pegged Quantity] ,  Edge.[Production_Pegging].[MaterialConstraintQty] ,  Edge.[Production_Pegging].[MaterialConstraintType]})  where  {RelationshipType.[Production_Pegging], Version.[Version Name]}; | MaterialConstraintType  Select  ( From .[Time].[Fiscal Week]*  From .[Item].[Item] *  From To .[Customer].[Customer Name] *  To To .[ Order ].[ Order  Name] *  To .[Time].[Fiscal Week])  ({Edge.[Production_Pegging].[Material Production Pegged Quantity] ,  Edge.[Production_Pegging].[MaterialConstraintQty] ,  Edge.[Production_Pegging].[MaterialConstraintType]})  where  {RelationshipType.[Production_Pegging], Version.[Version Name]}; | MaterialConstraintType  Select  ( From .[Time].[Fiscal Week]*  From .[Item].[Item] *  From To .[Customer].[Customer Name] *  To To .[ Order ].[ Order  Name] *  To .[Time].[Fiscal Week])  ({Edge.[Production_Pegging].[Material Production Pegged Quantity] ,  Edge.[Production_Pegging].[MaterialConstraintQty] ,  Edge.[Production_Pegging].[MaterialConstraintType]})  where  {RelationshipType.[Production_Pegging], Version.[Version Name]}; | MaterialConstraintType  Select  ( From .[Time].[Fiscal Week]*  From .[Item].[Item] *  From To .[Customer].[Customer Name] *  To To .[ Order ].[ Order  Name] *  To .[Time].[Fiscal Week])  ({Edge.[Production_Pegging].[Material Production Pegged Quantity] ,  Edge.[Production_Pegging].[MaterialConstraintQty] ,  Edge.[Production_Pegging].[MaterialConstraintType]})  where  {RelationshipType.[Production_Pegging], Version.[Version Name]}; | From .[Activity].[Activity] *  .[Location].[Location] *  From .[Lot].[Lot  ID ] *  .[Item].[Item] *  To .[Location].[Location] *  on row , | From .[Activity].[Activity] *  .[Location].[Location] *  From .[Lot].[Lot  ID ] *  .[Item].[Item] *  To .[Location].[Location] *  on row , | From .[Activity].[Activity] *  .[Location].[Location] *  From .[Lot].[Lot  ID ] *  .[Item].[Item] *  To .[Location].[Location] *  on row , | From .[Activity].[Activity] *  .[Location].[Location] *  From .[Lot].[Lot  ID ] *  .[Item].[Item] *  To .[Location].[Location] *  on row , | From .[Activity].[Activity] *  .[Location].[Location] *  From .[Lot].[Lot  ID ] *  .[Item].[Item] *  To .[Location].[Location] *  on row , | From .[Activity].[Activity] *  .[Location].[Location] *  From .[Lot].[Lot  ID ] *  .[Item].[Item] *  To .[Location].[Location] *  on row , |
| result:                                                                                                                                                                                                                                                                                                                                                                                                                                            | result:                                                                                                                                                                                                                                                                                                                                                                                                                                            | result:                                                                                                                                                                                                                                                                                                                                                                                                                                            | result:                                                                                                                                                                                                                                                                                                                                                                                                                                            | result:                                                                                                                                                                                                                                                                                                                                                                                                                                            | result:                                                                                                                                                                                                                                                                                                                                                                                                                                            | result:                                                                                                                                                                                                                                                                                                                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |
| Fi sc al  W ee k                                                                                                                                                                                                                                                                                                                                                                                                                                   | Acti vity                                                                                                                                                                                                                                                                                                                                                                                                                                          | Ite m                                                                                                                                                                                                                                                                                                                                                                                                                                              | Loc atio n                                                                                                                                                                                                                                                                                                                                                                                                                                         | Lot  ID                                                                                                                                                                                                                                                                                                                                                                                                                                            | Cust ome r  Na me                                                                                                                                                                                                                                                                                                                                                                                                                                  | It e m                                                                                                                                                                                                                                                                                                                                                                                                                                             | Loc atio n                                                                                                                                 | Or der  Na me                                                                                                                              | Fi sc al  W ee k                                                                                                                           | rial  Prod uctio n  Pegg ed  Qua ntity                                                                                                     | MaterialC onstraint Qty                                                                                                                    | MaterialCon straintType                                                                                                                    |
| W                                                                                                                                                                                                                                                                                                                                                                                                                                                  | BOH                                                                                                                                                                                                                                                                                                                                                                                                                                                | R                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Pla                                                                                                                                                                                                                                                                                                                                                                                                                                                | SCS                                                                                                                                                                                                                                                                                                                                                                                                                                                | Cust                                                                                                                                                                                                                                                                                                                                                                                                                                               | F                                                                                                                                                                                                                                                                                                                                                                                                                                                  | RD                                                                                                                                         | Or                                                                                                                                         | W                                                                                                                                          | 400                                                                                                                                        |                                                                                                                                            |                                                                                                                                            |
| 01 -                                                                                                                                                                                                                                                                                                                                                                                                                                               | Acti vity                                                                                                                                                                                                                                                                                                                                                                                                                                          | M_ 1                                                                                                                                                                                                                                                                                                                                                                                                                                               | nt_ 1                                                                                                                                                                                                                                                                                                                                                                                                                                              | BOH                                                                                                                                                                                                                                                                                                                                                                                                                                                | _1                                                                                                                                                                                                                                                                                                                                                                                                                                                 | G _1                                                                                                                                                                                                                                                                                                                                                                                                                                               | C_1                                                                                                                                        | der _1                                                                                                                                     | 04 - 20                                                                                                                                    |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |
| 20 15                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                            |                                                                                                                                            | 15                                                                                                                                         |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |
| W                                                                                                                                                                                                                                                                                                                                                                                                                                                  | BOH                                                                                                                                                                                                                                                                                                                                                                                                                                                | R                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Pla                                                                                                                                                                                                                                                                                                                                                                                                                                                | SCS                                                                                                                                                                                                                                                                                                                                                                                                                                                | Cust                                                                                                                                                                                                                                                                                                                                                                                                                                               | F                                                                                                                                                                                                                                                                                                                                                                                                                                                  | RD                                                                                                                                         | Or                                                                                                                                         | W                                                                                                                                          | 200                                                                                                                                        |                                                                                                                                            |                                                                                                                                            |
| 01 -                                                                                                                                                                                                                                                                                                                                                                                                                                               | Acti vity                                                                                                                                                                                                                                                                                                                                                                                                                                          | M_ 1                                                                                                                                                                                                                                                                                                                                                                                                                                               | nt_ 1                                                                                                                                                                                                                                                                                                                                                                                                                                              | BOH                                                                                                                                                                                                                                                                                                                                                                                                                                                | _1                                                                                                                                                                                                                                                                                                                                                                                                                                                 | G _1                                                                                                                                                                                                                                                                                                                                                                                                                                               | C_1                                                                                                                                        | der _2                                                                                                                                     | 31 -                                                                                                                                       |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |
| 20                                                                                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                            |                                                                                                                                            | 20                                                                                                                                         |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |
| 15                                                                                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                            |                                                                                                                                            | 15                                                                                                                                         |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |
| W                                                                                                                                                                                                                                                                                                                                                                                                                                                  | BOH                                                                                                                                                                                                                                                                                                                                                                                                                                                | R                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Pla                                                                                                                                                                                                                                                                                                                                                                                                                                                | SCS                                                                                                                                                                                                                                                                                                                                                                                                                                                | Cust                                                                                                                                                                                                                                                                                                                                                                                                                                               | F                                                                                                                                                                                                                                                                                                                                                                                                                                                  | RD                                                                                                                                         | Or                                                                                                                                         | W                                                                                                                                          | 400                                                                                                                                        |                                                                                                                                            |                                                                                                                                            |
| 01 -                                                                                                                                                                                                                                                                                                                                                                                                                                               | Acti vity                                                                                                                                                                                                                                                                                                                                                                                                                                          | M_ 2                                                                                                                                                                                                                                                                                                                                                                                                                                               | nt_ 1                                                                                                                                                                                                                                                                                                                                                                                                                                              | BOH                                                                                                                                                                                                                                                                                                                                                                                                                                                | _1                                                                                                                                                                                                                                                                                                                                                                                                                                                 | G _1                                                                                                                                                                                                                                                                                                                                                                                                                                               | C_1                                                                                                                                        | der _1                                                                                                                                     | 04 -                                                                                                                                       |                                                                                                                                            |                                                                                                                                            |                                                                                                                                            |

```
20 15 20 15 W 01 -20 15 BOH Acti vity R M_ 2 Pla nt_ 1 SCS BOH Cust _1 F G _1 RD C_1 Or der _2 W 31 -20 15 200 W 02 -20 15 FG_1 -Ship men t via Wat er from -Plan t_1 to DC_ 1 FG _1 DC_ 1 000S CS_L OT Cust _1 F G _1 RD C_1 Or der _1 W 04 -20 15 400 LeadtimePla nBeforeCurr ent W 02 -20 15 SubAsse mbl y for FG_1 -RM_ 1 conv erte d to INT_ 11 at Plan t_1 IN T_ 11 Pla nt_ 1 000S CS_L OT Cust _1 F G _1 RD C_1 Or der _1 W 04 -20 15 400 W 02 -20 15 SubAsse mbl y for FG_1 -RM_ IN T_ 21 Pla nt_ 1 000S CS_L OT Cust _1 F G _1 RD C_1 Or der _1 W 04 -20 15 400
```

```
2
```

conv erte d to INT\_ 21 at Plan t\_1 W 03 -20 15 Fina l Asse mbl y for FG\_1 at Plan t\_1 FG \_1 Pla nt\_ 1 000S CS\_L OT Cust \_1 F G \_1 RD C\_1 Or der \_1 W 04 -20 15 400 W 04 -20 15 FG\_1 -Ship men t via Air from Plan t\_1 to DC\_ 1 FG \_1 DC\_ 1 000S CS\_L OT Cust \_1 F G \_1 RD C\_1 Or der \_1 W 04 -20 15 400 W 05 -20 15 FG\_1 -Ship men t via Air from DC\_ 1 to RDC \_1 FG \_1 RD C\_1 000S CS\_L OT Cust \_1 F G \_1 RD C\_1 Or der \_1 W 04 -20 15 400 W 25 -20 SubAsse mbl y for FG\_1 IN T\_ 11 Pla nt\_ 1 000S CS\_L OT Cust \_1 F G \_1 RD C\_1 Or der \_2 W 31 -20 200

| 15               | -  RM_                                                           |          |           |              |         |        |        |           | 15           |     |     |                |
|------------------|------------------------------------------------------------------|----------|-----------|--------------|---------|--------|--------|-----------|--------------|-----|-----|----------------|
| W 25 - 20        | Sub- Asse mbl y for  FG_1  -  RM_ 2                              | IN T_ 21 | Pla nt_ 1 | 000S CS_L OT | Cust _1 | F G _1 | RD C_1 | Or der _2 | W 31 - 20 15 | 200 |     |                |
| 15  W 25 - 20 15 | conv erte d to  INT_ 21  at  Plan t_1  Expe cted  Reci ept  Acti | R M_ 1   | Pla nt_ 1 | 000S CS_L OT | Cust _1 | F G _1 | RD C_1 | Or der _2 | W 31 - 20 15 |     | 200 | MaterialSho rt |
| W 25 - 20 15     | 1)  Expe cted  Reci ept  Acti                                    | R M_ 2   | Pla nt_ 1 | 000S CS_L OT | Cust _1 | F G _1 | RD C_1 | Or der _2 | W 31 - 20 15 |     | 200 | MaterialSho rt |
| W 26 - 20        | Fina l  Asse mbl                                                 | FG _1    | Pla nt_ 1 | 000S CS_L OT | Cust _1 | F G _1 | RD C_1 | Or der _2 | W 31 - 20    | 200 |     |                |

```
15 y for FG_1 at Plan t_1 15 W 29 -20 15 FG_1 -Ship men t via Wat er from -Plan t_1 to DC_ 1 FG _1 DC_ 1 000S CS_L OT Cust _1 F G _1 RD C_1 Or der _2 W 31 -20 15 200 W 31 -20 15 FG_1 -Ship men t via Wat er from DC_ 1 to RDC _1 FG _1 RD C_1 000S CS_L OT Cust _1 F G _1 RD C_1 Or der _2 W 31 -20 15 200
```

## Example:

Graph- Consumption Pegging

| From                | To                    |
|---------------------|-----------------------|
| Item.[Item]         | Item.[Item]           |
| Location.[Location] | Location.[Location]   |
| Activity.[Activity] | Order.[Order Name]    |
| Time.[FiscalWeek]   | Time.[FiscalWeek]     |
| Lot.[Lot ID         | Customer.[CustomerID] |
| Edge Property       |                       |

Material Consumption Pegged Quantity

Select ( From .[Time].[Fiscal Week]* From .[Activity].[Activity]* From .[Item].[Item]* From .[Location].[Location]* From .[Lot].[Lot ID ]* To .[Customer].[Customer Name]* To .[Item].[Item]* To .[Location].[Location]* To .[ Order ].[ Order Name]* To .[Time].[Fiscal Week]) on row , ({Edge.[Consumption\_Pegging].[Material Consumption Pegged Quantity]}) on column where {RelationshipType.[Consumption\_Pegging], Version.[Version Name]};

Result:

| Fisc al  We ek   | Activit y                          | Item           | Locati on   | Lot ID   | Custo mer  Name   | Ite m   | Locati on   | Order  Name   | Fisc al  We ek   |   Material  Consump tion  Pegged  Quantity |
|------------------|------------------------------------|----------------|-------------|----------|-------------------|---------|-------------|---------------|------------------|--------------------------------------------|
| W0 4- 201 5      | FG_1 -  Shipm ent via  Air         | FG_1  DC_1     | 000SCS_ LOT |          | Cust_1            | FG_ 1   | RDC_ 1      | Order _1      | W0 4- 201 5      |                                        400 |
| W0 3- 201 5      | FG_1 -  Shipm ent via  Air         | FG_1  Plant_ 1 | 000SCS_ LOT | Cust_1   |                   | FG_ 1   | RDC_ 1      | Order _1      | W0 4- 201 5      |                                        400 |
| W2 7- 201 5      | FG_1 -  Shipm ent via  Water  FG_1 | Plant_ 1       | 000SCS_ LOT | Cust_1   |                   | FG_ 1   | RDC_ 1      | Order _2      | W3 1- 201 5      |                                        200 |
| W2 9- 201 5      | FG_1 -  Shipm ent via  Water       | FG_1  DC_1     | 000SCS_ LOT |          | Cust_1            | FG_ 1   | RDC_ 1      | Order _2      | W3 1- 201 5      |                                        200 |

RDC\_1 W0 2201 5 Final Assem bly for FG\_1 at Plant\_1 INT\_ 11 Plant\_ 1 000SCS\_ LOT Cust\_1 FG\_ 1 RDC\_ 1 Order \_1 W0 4201 5 400 W2 5201 5 Final Assem bly for FG\_1 at Plant\_1 INT\_ 11 Plant\_ 1 000SCS\_ LOT Cust\_1 FG\_ 1 RDC\_ 1 Order \_2 W3 1201 5 200 W0 2201 5 Final Assem bly for FG\_1 at Plant\_1 INT\_ 21 Plant\_ 1 000SCS\_ LOT Cust\_1 FG\_ 1 RDC\_ 1 Order \_1 W0 4201 5 400 W2 5201 5 Final Assem bly for FG\_1 at Plant\_1 INT\_ 21 Plant\_ 1 000SCS\_ LOT Cust\_1 FG\_ 1 RDC\_ 1 Order \_2 W3 1201 5 200 W0 1201 5 SubAssem bly for FG\_1 -RM\_1 conver ted to INT\_11 at Plant\_1 RM\_ 1 Plant\_ 1 000SCS\_ LOT Cust\_1 FG\_ 1 RDC\_ 1 Order \_1 W0 4201 5 400 W2 4201 5 SubAssem bly for FG\_1 -RM\_1 conver ted to INT\_11 at Plant\_1 RM\_ 1 Plant\_ 1 000SCS\_ LOT Cust\_1 FG\_ 1 RDC\_ 1 Order \_2 W3 1201 5 200 W0 1201 SubAssem bly for RM\_ 2 Plant\_ 1 000SCS\_ LOT Cust\_1 FG\_ 1 RDC\_ 1 Order \_1 W0 4201 400

```
5 FG_1 -RM_2 conver ted to INT_21 at Plant_1 5 W2 4201 5 SubAssem bly for FG_1 -RM_2 conver ted to INT_21 at Plant_1 RM_ 2 Plant_ 1 000SCS_ LOT Cust_1 FG_ 1 RDC_ 1 Order _2 W3 1201 5 200
```

## copymeasure

## update

## delete data for model

One may want to delete all interserctions (rows) and hence all measures within a measuregroup based on version or other member filters. One can achieve this through delete data for model command. Below is an example to delete data and intersections with the measuregroup CapacityReports only for CurrentWorkingView .

To begin with for every delete data example we start with a measuregroup SalesHistory having data as indicated by the below select query.

```
select ([Version].[Version Name]  * [Time].[Year-Month] * [Item].[SKU] * {Measure.[SalesPrice], Measure.[UnitsSold]} ) orderby [Version].[Version Name].Name desc , [Time].[Year-Month]. Key asc limit 100;
```

| Version Name       | Year-Month   | SKU                  | SalesPrice   |   UnitsSold |
|--------------------|--------------|----------------------|--------------|-------------|
| ScenarioAllPlays   | 2016-M1      | Liril Shine - 40     | 20           |           1 |
| ScenarioAllPlays   | 2016-M5      | Fructis Shine - 500  | 250          |         100 |
| ScenarioAllPlays   | 2016-M6      | Fructis Strong - 500 |              |         100 |
| CurrentWorkingView | 2016-M1      | Liril Shine - 40     | 20           |           1 |
| CurrentWorkingView | 2016-M5      | Fructis Shine - 500  | 250          |         100 |
| CurrentWorkingView | 2016-M6      | Fructis Strong - 500 |              |         100 |

One can remove all data for CurrentWorkingView using below query. Results of the select will change after that.

```
delete data for model [SalesHistory] where {Version.[Version Name].[CurrentWorkingView]};
```

| Version Name     | Year-Month   | SKU                  | SalesPrice   |   UnitsSold |
|------------------|--------------|----------------------|--------------|-------------|
| ScenarioAllPlays | 2016-M1      | Liril Shine - 40     | 20           |           1 |
| ScenarioAllPlays | 2016-M5      | Fructis Shine - 500  | 250          |         100 |
| ScenarioAllPlays | 2016-M6      | Fructis Strong - 500 |              |         100 |

One can remove all data for CurrentWorkingView and Fructis by using related memers for Fructis using query below or else one could use filter clause on [Item].[SKU] . Note that Liril Shine - 40 rows will be retained.

```
delete data for model [SalesHistory] where {Version.[Version Name].[CurrentWorkingView], [Item].[Brand].[Fructis].relatedmembers([SKU]) };
```

| Version Name       | Year-Month   | SKU                  | SalesPrice   |   UnitsSold |
|--------------------|--------------|----------------------|--------------|-------------|
| ScenarioAllPlays   | 2016-M1      | Liril Shine - 40     | 20           |           1 |
| ScenarioAllPlays   | 2016-M5      | Fructis Shine - 500  | 250          |         100 |
| ScenarioAllPlays   | 2016-M6      | Fructis Strong - 500 |              |         100 |
| CurrentWorkingView | 2016-M1      | Liril Shine - 40     | 20           |           1 |

One can also remove entries based on measure filter besides version filter using below query. Note that only one row is matching the criteria specified.

```
delete data for model [SalesHistory] where {Version.[Version Name].[ScenarioAllPlays], ~isnull(Measure.[SalesPrice]), Measure.[UnitsSold] > 10 };
```

| Version Name       | Year-Month   | SKU                  | SalesPrice   |   UnitsSold |
|--------------------|--------------|----------------------|--------------|-------------|
| ScenarioAllPlays   | 2016-M1      | Liril Shine - 40     | 20           |           1 |
| ScenarioAllPlays   | 2016-M6      | Fructis Strong - 500 |              |         100 |
| CurrentWorkingView | 2016-M1      | Liril Shine - 40     | 20           |           1 |
| CurrentWorkingView | 2016-M5      | Fructis Shine - 500  | 250          |         100 |
| CurrentWorkingView | 2016-M6      | Fructis Strong - 500 |              |         100 |

## delete data for relationshiptype

#Transferring Data

## Download data file

The DownloadDataFile command is used to create Excel or text files from an IBPL select query.

Example: Exceute the query:

Select (Version.[Version Name]*Product.[ProductID]*Customer.[CustomerName]* Customer.[City]*{Measure.[UnitPrice], Measure.[UnitSold]});


User can download the data file either in CSV or Excel format by clicking on the respective button.

## Export

This command dumps the entire Live Server data in form of dsv files. User can do selective export depending on the requirement like:

- ExportAll(); - exports all data in GraphCube
- ExportAll() include Dimensions All; - Exports only the master data, All dim tables
- ExportAll() include Models All; - Exports all fact data
- ExportAll() include Graphs All; - Exports all graph data
- ExportAll() include Dimensions All except Product,Customer; - Exports all dimensions except Product and Customer dimension.
- ExportAll() include dimension Customer include Models [Sales Plan], [Demand Plan];

Example:

Step1: Exceute the required Export query. ExportAll();

Step2: Go to data mangement workspace and select the file management view and download then latest exported folder.



This will download the live server data into users PC/Laptop.

## Number Formats

| Format  Symbol   | Description                                                                                 |
|------------------|---------------------------------------------------------------------------------------------|
| ;                | Seperate positive no. format from negative no. format                                       |
| ,                | putting  ,  anywhere in the format string displays numbers with commas like  12345 → 12,345 |
| $                | Currency Marker, shows local format with language selection in tenant (e.g. $               |

|          | in en-US, ₹ in en-IN)                                                                                                                               |
|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| .        | decimal point seperator.                                                                                                                            |
| a        | ending the number format with  a  displays the no. as 1.1m 78k                                                                                      |
| o        | ending format string with  o  display no. like 1st, 34th                                                                                            |
| %        | ending format string  %  converts the ratio to percentage (multiplies by 100  too)                                                                  |
| .000…0   | Maximum no. of digits after decimal is determined by no. of zeroes. numbers  are forced to be of this length extra zeroes are added (2.1 → 2.100…0) |
| .[000…0] | Maximum no. of digits after decimal is determined by no. of zeroes, extra  zeroes are not been added (2.1 → 2.1)                                    |

Remember: 0 has been used but any other character (like #) would also give same result except a o ( )

There should always be a character before decimal point. Generally #, 0 are used

1. Example 1 = 1234567.1256
2. Example 2 = -9875.23

| Format String   | Example 1      | Example 2   |
|-----------------|----------------|-------------|
|                 | 1234567        | -9875       |
| #.00            | 1234567.13     | -9875.23    |
| .00             | .13            | -.23        |
| #.0000          | 1234567.1256   | -9857.2300  |
| #.[0000]        | 1234567.1256   | -9857.23    |
| #.00,           | 1,234,567.126  | -9,875.230  |
| ,.00            | 1,234,567.13   | -9,875.23   |
| $ ,.00          | $ 1,234,567.13 | -$ 9,875.23 |
| #.00 a          | 1.23 m         | -9.86 k     |
| .00 a           | .23 m          | -.86 k      |
| ,.00;           | 1,234,567.13   | -9875       |
| ;,.00           | 1234567        | -9,875.00   |
| ,.00;(,.00)     | 1,234,567.13   | (9,875.23)  |
| #.00 a;( ,.00)  | 1.23 m         | (9,875.23)  |
| %               | 123456713%     | -987523%    |

## Date Time Format

Let's say date is

1. Thu 30 August 2018 8:42:54 (UTC +05:30)
2. Sat 6 October 2018 23:01:08 (UTC +05:30)

It's format is ddd dd MMMM yyyy H:mm:ss (UTC zzz)

| Format   | Denotion                                                                                           | Thu 30 August  2018 8:42:54 (UTC  +05:30)   | Sat 6 October 2018  23:01:08 (UTC  +05:30)   |
|----------|----------------------------------------------------------------------------------------------------|---------------------------------------------|----------------------------------------------|
| d        | single digit Date                                                                                  | 30                                          | 6                                            |
| dd       | Double digit Date                                                                                  | 30                                          | 06                                           |
| ddd      | day of the week                                                                                    | Thu                                         | Sat                                          |
| dddd     | day of the week (full)                                                                             | Thurday                                     | Saturday                                     |
| D        | non recognizable                                                                                   | -                                           |                                              |
| h        | single digit hour (12 hr format)                                                                   | 8                                           | 11                                           |
| hh       | double digit hour (12 hr format)                                                                   | 08                                          | 11                                           |
| H        | single digit hour (24 hr format)                                                                   | 8                                           | 23                                           |
| HH       | double digit hour (24 hr format)                                                                   | 08                                          | 23                                           |
| M        | single digit Month                                                                                 | 8                                           | 10                                           |
| MM       | Double digit month                                                                                 | 08                                          | 10                                           |
| MMM      | Three alphabet month                                                                               | Aug                                         | Oct                                          |
|          | MMMM  Full Month Name                                                                              | August                                      | October                                      |
| m        | single digit minute                                                                                | 42                                          | 1                                            |
| mm       | double digit minute                                                                                | 42                                          | 01                                           |
| mmm      | read as mm,m                                                                                       | 4242                                        | 011                                          |
| tt       | AM/PM                                                                                              | AM                                          | PM                                           |
| yy       | 2 digit year                                                                                       | 18                                          | 18                                           |
| yyy      | 2 digit year followed by y                                                                         | 18y                                         | 18y                                          |
| yyyy     | 4 digit year                                                                                       | 2018                                        | 2018                                         |
| f        | one-tenths of second, more accuracy  increase the no. of f (fff miliseconds,  ffffff microseconds) | 0                                           | 0                                            |
| z        | single digit time Zone, shows integer  value for fractional time zones                             | +5                                          | +5                                           |
| zz       | double digit time Zone, shows integer  value for fractional time zones                             | +05                                         | +05                                          |
| zzz      | Complete time zone                                                                                 | +05:30                                      | +05:30                                       |

Anything else mentioned in datetime display format will be displayed as it is for e.g. take these 2 dates

1. Thu 30 August 2018 8:42:54 (UTC +05:30)

2. Sat 6 October 2018 23:01:08 (UTC +05:30)

| Display Format                                        | Example 1                                             | Example 2                                                |
|-------------------------------------------------------|-------------------------------------------------------|----------------------------------------------------------|
| MM-dd-yyyy                                            | 08-30-2018                                            | 10-06-2018                                               |
| ddd, dd-MMM-yyyy                                      | Thu, 30-Aug-2018                                      | Sat, 06-Oct-2018                                         |
| MMM/dd/yyyy                                           | Aug/30/2018                                           | Oct/06/2018                                              |
| MM/dd/yyyy                                            | 08/30/2018                                            | 10/06/2018                                               |
| MM.dd.yyyy                                            | 08.30.2018                                            | 10.06.2018                                               |
| yyyy/MM/dd HH:mm:ss  yyyy/MM/dd HH:mm                 | 2018/08/30 08:42:54  2018/08/30 08:42                 | 2018/10/06 23:01:08  2018/10/06 23:01                    |
| yyyy/MM/dd                                            | 2018/08/30                                            | 2018/10/06                                               |
| dd/MMM/yyyy                                           | 30/Aug/2018                                           | 06/Oct/2018                                              |
| dd/MM/yyyy                                            | 30/08/2018                                            | 06/10/2018                                               |
| dd-MM-yyyyTHH:mm:ss                                   | 30-08-2018T08:42:54                                   | 06-10-2018T23:01:08                                      |
| dd-MM-yyyy  dd-MMM-                                   | 30-08-2018  30-Aug-2018T08:42:54                      | 06-10-2018  06-Oct-2018T23:01:08                         |
| yyyyTHH:mm:ss                                         | 30-Aug-2018 08:42:54                                  |                                                          |
| dd-MMM-yyyy HH:mm:ss  dd-MMM-yyyy HH:mm               | 30-Aug-2018 08:42                                     | 06-Oct-2018 23:01:08  06-Oct-2018 23:01                  |
| dd-MMM-yyyy  dd-MMM-yy                                | 30-Aug-2018  30-Aug-18                                | 06-Oct-2018  06-Oct-18                                   |
| ddd MMM dd yyyy  HH:mm:ss                             | Thu Aug 30 2018 08:42:54                              | Sat Oct 06 2018 23:01:08                                 |
| yyyy-MM-                                              | 2018-08- 30T08:42:54.000000+05:30                     | 2018-10-                                                 |
| ddTHH:mm:ss.fffffffzzz  yyyy-MM- ddTHH:mm:ss.fffzzz   | 2018-08- 30T08:42:54.000+05:30                        | 06T23:01:08.000000+05:30  2018-10- 06T23:01:08.000+05:30 |
| yyyy-MM- ddTHH:mm:sszzz  yyyy-MM- ddTHH:mm:ss.fffffff | 2018-08-30T08:42:54+05:30  2018-08-30T08:42:54.000000 | 2018-10-06T23:01:08+05:30  2018-10-06T23:01:08.000000    |
| yyyy-MM- ddTHH:mm:ss.fff                              | 2018-08-30T08:42:54.000                               | 2018-10-06T23:01:08.000                                  |
| yyyy-MM-ddTHH:mmzzz                                   | 2018-08-30T08:42+05:30                                |                                                          |
|                                                       |                                                       | 2018-10-06T23:01+05:30                                   |
| yyyy-MM-ddTHH:mm:ss                                   | 2018-08-30T08:42:54                                   | 2018-10-06T23:01:08                                      |
| yyyy-MM-ddTHH:mm                                      | 2018-08-30T08:42                                      | 2018-10-06T23:01                                         |
| yyyy-MM-dd HH:mm:ss  yyyy-MM-dd HH:mm                 | 2018-08-30 08:42:54  2018-08-30 08:42                 | 2018-10-06 23:01:08                                      |
|                                                       |                                                       | 2018-10-06 23:01                                         |
| yyyy-MM-dd                                            | 2018-08-30                                            | 2018-10-06                                               |

| MMM-yy                 | Aug-18                    | Oct-18                 |
|------------------------|---------------------------|------------------------|
| HH:mm:ss               | 08:42:54                  | 23:01:08               |
| HH:mm                  | 08:42                     | 23:01                  |
| h:mm:ss tt             | 8:42:54 AM                | 11:01:08 PM            |
| yyeahH!                | 18ea88!                   | 18ea1123!              |
| M..mmattyy             | 8..42aAM18                | 10..01aPM18            |
| jumps over little dog. | ju42p54 over liAMle 30og. | ju1p8 over liPMle 6og. |

Explanation: yy,h,H in third last case and M,mm,tt,yy in second last case has been recognised seperately, similarly for last case and those has been assigned corresponding values

```
Default Format: yyyy-MM-ddThh:mm:ss.ffffffzzz
```

## Arithmetic Operators

An arithmetic operator is a mathematical function that takes two operands and performs a calculation on them.

The arithmetic operators supported in IBPL include: addition (+), subtraction (-), multiplication (*), division (/), power (^). They each accept two numeric values as input and return a numeric value.

IBPL follows BODMAS calculation

## Addition

The addition operator produces the sum of numeric operands or string concatenation.The Addition Operator is the plus sign (+)

```
Example: select (5 + 3); result = 8 Example: Select (Version.[Version Name]*Product.[ProductID]* {Measure.[UnitPrice], (Measure.[UnitPrice]+1000) as Transient.NewUnitPrice}); result = one thousand added to unit price Explaination: Vesrion Name ProducrID UnitPrice NewUnitPrice CurrentWorkingView FUR-BO-10001798 10,858 11,858
```

CurrentWorkingView

FUR-BO-10002545 10,671

11,671

CurrentWorkingView

FUR-BO-10002613 19,458

20,458

## Subtraction

The subtraction operator subtracts the two operands, producing their difference.The Subtraction Operator is the minus sign (-)

Example:

<!-- formula-not-decoded -->

result = 2

Exampe:

```
Select (Version.[Version Name]*Product.[ProductID]* {Measure.[UnitPrice], (Measure.[UnitPrice]-1000) as Transient.NewUnitPrice});
```

result = one thousand is subtracted from unit price

## Explaination:

| Vesrion Name       | ProducrID       | UnitPrice   | NewUnitPrice   |
|--------------------|-----------------|-------------|----------------|
| CurrentWorkingView | FUR-BO-10001798 | 10,858      | 9,858          |
| CurrentWorkingView | FUR-BO-10002545 | 10,671      | 9,671          |
| CurrentWorkingView | FUR-BO-10002613 | 19,458      | 18,458         |

Example:

```
Select (Version.[Version Name]*Product.[ProductID]* {(Measure.[UnitPrice]-Measure.[UnitCost]) as Transient.Profit});
```

result = difference between unit price and unit cost

## Explaination:

| Vesrion Name       | ProducrID       | UnitPrice   | UnitCost   | Profit   |
|--------------------|-----------------|-------------|------------|----------|
| CurrentWorkingView | FUR-BO-10001798 | 10,858      | 10,000     | 858      |
| CurrentWorkingView | FUR-BO-10002545 | 10,671      | 8,000      | 2,671    |
| CurrentWorkingView | FUR-BO-10002613 | 19,458      | 18,000     | 1,458    |

Note:while adding or subtracting measures, if a measure has a null value, then the result of the opeartion will be null. To avoid this, user should use the Coalesce() function to take the non null values.

## Multiplication

The multiplication operator produces the product of the operands.The Multiplication Operator is the asterisk (*)

Example:

Select

(5 * 3);

result = 15

Example:

```
Select (Version.[Version Name]*Product.[ProductID]* {Measure[UnitPrice], (Measure.[UnitPrice]*100) as Transient.NewUnitPrice});
```

result = one hundred times unit price

Explaination:

| Vesrion Name       | ProducrID       | UnitPrice   | NewUnitPrice   |
|--------------------|-----------------|-------------|----------------|
| CurrentWorkingView | FUR-BO-10001798 | 10,858      | 10,85,800      |
| CurrentWorkingView | FUR-BO-10002545 | 10,671      | 10,67,100      |
| CurrentWorkingView | FUR-BO-10002613 | 19,458      | 19,45,800      |

Example:

```
Select (Version.[Version Name]*Product.[ProductID]* {Measure.[UnitPrice], Meaure[UnitSold], (Measure.[UnitPrice]*Measure.[UnitSold]) as Transient.PriceTimesUnitSold });
```

result = product of unit sold and unit price

## Explaination:

| Version Name       | ProductID       | UnitPrice   |   UnitSold | PriceTimesUnitSold   |
|--------------------|-----------------|-------------|------------|----------------------|
| CurrentWorkingView | FUR-BO-10001798 | 10,858      |          3 | 32,573               |
| CurrentWorkingView | FUR-BO-10002545 | 10,671      |          1 | 10,671               |
| CurrentWorkingView | FUR-BO-10002613 | 19,458      |          3 | 58,374               |

## Division

The Division Operator is the asterisk (/).

Example:

<!-- formula-not-decoded -->

result = 1.6666666667

Example:

```
Select (Version.[Version Name]*Product.[ProductID]* {(Measure.[UnitPrice]/100) as Transient.NewUnitPrice});
```

result = unit price divided by one hundred

## Explanation:

| Vesrion Name       | ProducrID       | UnitPrice   |   NewUnitPrice |
|--------------------|-----------------|-------------|----------------|
| CurrentWorkingView | FUR-BO-10001798 | 10,858      |            109 |
| CurrentWorkingView | FUR-BO-10002545 | 10,671      |            107 |
| CurrentWorkingView | FUR-BO-10002613 | 19,458      |            195 |

Example:

```
Select (Version.[Version Name]*Product.[ProductID]* {Measure.[UnitPrice], Measure.[UnitCost], (Measure.[UnitPrice] / Measure.[UnitCost]) as Transient.PriceRatioCost});
```

result = ratio of unit price to unit cost

Note:when dividing measures, if a measure has a null value, then the return value of the Division Operator will be null. Also, if the divisor is equal to zero, this will cause an error. To avoid this, you should use the if/then/else function to check whether the divisor is nonzero.

## Power

The Power Operator is the caret symbol (^). It raises the first input to the power of the second input and returns the result as a number.

```
Select (Version.[Version Name]*Product.[ProductID]* {Measure.[UnitPrice], (Measure.[UnitPrice] ^ 2) as Transient.PricePower2});
```

```
Example: Select (5^3); result=125 Example: result = Unit price is squared
```

| Version Name       | ProductID       | UnitPrice   | PricePower2   |
|--------------------|-----------------|-------------|---------------|
| CurrentWorkingView | FUR-BO-10001798 | 10,857.63   | 117,888,129   |
| CurrentWorkingView | FUR-BO-10002545 | 10,671.23   | 113,875,150   |
| CurrentWorkingView | FUR-BO-10002613 | 19,457.96   | 378,612,207   |

CurrentWorkingView FUR-BO-10004695 15,274.25 233,302,713

Example:

```
Select (Version.[Version Name]*Product.[ProductID]* {Measure.[UnitPrice], Measure.[UnitSold], (Measure.[UnitPrice] ^ Measure.[UnitSold]) as Transient.PricePowerUnitSold});
```

```
result = Unit price raised to the power of Unit Sold.
```

| Version Name       | ProductID       | UnitPrice   |   UnitSold | PricePowerUnitSold   |
|--------------------|-----------------|-------------|------------|----------------------|
| CurrentWorkingView | FUR-BO-10001798 | 10,857.63   |          3 | 1,279,985,688,429    |
| CurrentWorkingView | FUR-BO-10002545 | 10,671.23   |          1 | 10,671               |
| CurrentWorkingView | FUR-BO-10002613 | 19,457.96   |          3 | 7,367,021,186,354    |
| CurrentWorkingView | FUR-BO-10004695 | 15,274.25   |          3 | 3,563,523,964,995    |

## Relational Operators

A relational operator checks the relationship between two operands.

The relational operators supported in IBPL include Equal (==), NotEqual (!=), LessThan (&lt;), LessThanOrEqual (&lt;=), GreaterThan (&gt;), and GreaterThanOrEqual (&gt;=).They each accept two value expressions as input and return a logical value. The inputs can be of type date, number or string.

## Equal(==)

The Equal Operator is represented by two equal signs (==). It accepts two inputs and returns true if they are the same, and false otherwise.

- Single equal to (=) is an assignment operator. So when you write a=b (instead of a==b ). It tries to assign the value stored in b to a instead of comparing both. When it is used in an argument expecting boolean results the query fails as a=b doesn't give boolean result.

```
Example Result Select(1==1); true Select(1==2); false Select('String'=='String'); true Select('String'=='String1'); false Example: select (1.0)=="1"; result= true
```

Explaination: 1.0 is stored as a integer 1 and it is converted into string '1' before comparison Example: select (1.0)=="1.0"; reult= false Explaination:Conversion of number to string removes the extraneous zero and decimal point. Example: select (Version.[Version Name] * Product.[ProductID]*{Measure.[UnitPrice], (Measure.[UnitPrice]==Measure.[UnitPrice]) as Transient.Result }); result=true Explaination Version Name ProductID UnitPrice Result CurrentWorkingView FUR-BO-10001798 10,858 true CurrentWorkingView FUR-BO-10002545 10,671 true CurrentWorkingView FUR-BO-10002613 19,458 true Example: select (Version.[Version Name] * Product.[ProductID]* { Measure.[UnitPrice], Measure.[UnitCost], (Measure.[UnitPrice]==Measure.[UnitCost]) as Transient.Result }); result: Version Name ProductID UnitPrice UnitCost Result CurrentWorkingView FUR-BO-10001798 10,858 10,000 false CurrentWorkingView FUR-BO-10002545 10,671 8,000 false CurrentWorkingView FUR-BO-10002613 19,458 18,000 false CurrentWorkingView FUR-BO-10004695 15,274 15,000 false CurrentWorkingView FUR-BO-10004834 12,091 12,000 false select ToDateTime("08/27/2018 08:30PM")=="2018-08-27T20:30"; result=true Explaination:ToDateTime() function converts a string representing a date into a date value S.No. ToDateTime() Argument Comparison String Result

| 1  08/27/2018              | 2018-08-27T20:30   | false   |
|----------------------------|--------------------|---------|
| 2  08/27/2018 08:30PM      | 2018-08-27         | false   |
| 3  2018-08-27              | 2018-08-27T00:00   | true    |
| 4  18-08-27                | 2018-08-27T00:00   | ERROR!  |
| 5  27-08-2018              | 2018-08-27T00:00   | ERROR!  |
| 6  27-Aug-2018             | 2018-08-27T00:00   | true    |
| 7  August 27 2018 08:30 PM | 2018-08-27T20:30   | true    |

## Explanation

1. If no time is mentioned ToDateTime takes time to be 00:00
2. Comparison string should be in the format yyyy-MM-ddThh:mm
3. Year is written as yyyy thats why it recognise it to be year
4. Server tries to map first no. to be month if its a 2 digit no.
5. Server tries to map first no. to be month if its a 2 digit no. and it finds no mention of the month in words
6. Since month has been written verbally

## Not Equal (!=)

The Not Equal Operator is represented by exclamatory and an equal sign (!=). It accepts two inputs and returns false if they are the same, and true otherwise.(&lt;&gt;) can also be used as the NotEqual operator.

| Example                      | Result   |
|------------------------------|----------|
| Select(1!=1);                | false    |
| Select(1!=2);                | true     |
| Select('String'!='String');  | false    |
| Select('String'!='String1'); | true     |

Example:

Select

(1.0)!="1";

result=false

Explaination: 1.0 is converted into string eqaul to '1'

Example:

Select

(1.0)!="1.0";

## result=true

Explaination: Conversion of number to string removes the extraneous zero and decimal point.

## Example:

```
select (Version.[Version Name] * Product.[ProductID]*{ (Measure.[UnitPrice]!=Measure.[UnitPrice]) as Transient.Result });
```

```
result: Version Name ProductID UnitPrice Result CurrentWorkingView FUR-BO-10001798 10,857.63 false CurrentWorkingView FUR-BO-10002545 10,671.23 false CurrentWorkingView FUR-BO-10002613 19,457.96 false Example: select (Version.[Version Name] * Product.[ProductID]* { Measure.[UnitPrice], Measure.[UnitCost], (Measure.[UnitPrice]!=Measure.[UnitCost]) as Transient.Result }); result: Explaination:
```

| Version Name       | ProductID       | UnitPrice   | UnitCost   | Result   |
|--------------------|-----------------|-------------|------------|----------|
| CurrentWorkingView | FUR-BO-10001798 | 10,858      | 10,000     | true     |
| CurrentWorkingView | FUR-BO-10002545 | 10,671      | 8,000      | true     |
| CurrentWorkingView | FUR-BO-10002613 | 19,458      | 18,000     | true     |
| CurrentWorkingView | FUR-BO-10004695 | 15,274      | 15,000     | true     |
| CurrentWorkingView | FUR-BO-10004834 | 12,091      | 12,000     | true     |

## Example:

```
select ToDateTime("08/27/2018 08:30PM")!="2018-08-27T08:30";
```

result=true

Explaination:ToDateTime() function converts a string representing a date into a date value.so input 08/27/2018 08:30PM will give the result 2018-08-27T20:30 and not 201808-27T08:30 .

## Less Than(&lt;)

The less than operand is represented by (&lt;) sign. It accepts two inputs and returns true if the first is strictly less than the second, and false otherwise.

| Example      | Result   |
|--------------|----------|
| Select(1<1); | false    |

Select(1&lt;2); true Select('String'&lt;'String') false Select('String&lt;'String1") true Example Select (1.0)&lt;"1"; result= false Explaination: 1.0 is converted to a string equal to 1 which in case can not be less than 1. Example: select (11)&lt;"2"; result= true Explaination: the number is converted to the string '11' which is less than '2') Example: select (Version.[Version Name] * Product.[ProductID]*{Measure.[UnitPrice], (Measure.[UnitPrice]&lt;8000) as Transient.Result}); Use of Transient result:

| Version Name       | ProductID       | UnitPrice   | Result   |
|--------------------|-----------------|-------------|----------|
| CurrentWorkingView | FUR-BO-10001798 | 10,857.63   | false    |
| CurrentWorkingView | FUR-BO-10002545 | 10,671.23   | false    |
| CurrentWorkingView | FUR-CH-10000454 | 578.92      | true     |
| CurrentWorkingView | FUR-CH-10000513 | 964.06      | true     |
| CurrentWorkingView | FUR-CH-10000863 | 973.79      | true     |

## Example:

```
select ToDateTime("08/27/2018 08:30PM")<"2018-08-27T08:30";
```

result= false

## LessThanOrEqual (&lt;=)

Less Than or equal operand is represented by the less than (&lt;) and an Equal (=) sign.It accepts two inputs and returns true if the first is less than or equal to the second, and false otherwise.

| Example   | Result   |
|-----------|----------|

```
Select(1<=1); true Select(1<=5); true Select(5<=1); false Select('String'<='String') true Select('String'<='String1') true Select('String1'<='String') false
```

## Example:

```
Select (1.0)<="1";
```

result: true

Explaination:1.0 is converted into string eqaul to '1'

Example:

select

(11)&lt;="2";

result= true

Explaination: the number is converted to the string '11' which is less than '2')

Example:

```
Select (Version.[Version Name]*Product.[ProductID]*{ Measure.[UnitPrice], (Measure.[UnitPrice]<=10857.63) as transient.result});
```

result:

| Version Name       | ProductID       | UnitPrice   | result   |
|--------------------|-----------------|-------------|----------|
| CurrentWorkingView | FUR-BO-10001798 | 10,857.63   | true     |
| CurrentWorkingView | FUR-BO-10002545 | 10,671.23   | true     |
| CurrentWorkingView | FUR-BO-10002613 | 19,457.96   | false    |
| CurrentWorkingView | FUR-BO-10004695 | 15,274.25   | false    |

## Example:

```
select ToDateTime("08/27/2018 08:30PM")<="2018-08-27T08:30";
```

result= false

## GreaterThan(&gt;)

The greater than operand is represented by (&gt;) sign. It accepts two inputs and returns true if the first is strictly greater than the second, and false otherwise.

Example

Result

```
Select(1>1); false Select(1>2); false Select('String'>'String') false Select('String1>'String") true Example Select (1.0)>"1"; result= false Explaination: 1.0 is converted to a string equal to 1 which in case can not be less than 1. Example: select (11)>"2"; result= false Explaination: the number is converted to the string '11' which is less than '2') Example: Select (Version.[Version Name]*Product.[ProductID]*{ Measure.[UnitPrice], (Measure.[UnitPrice]>10857.63) as transient.result}); result: Version Name ProductID UnitPrice result CurrentWorkingView FUR-BO-10001798 10,857.63 false CurrentWorkingView FUR-BO-10002545 10,671.23 false CurrentWorkingView FUR-BO-10002613 19,457.96 true CurrentWorkingView FUR-BO-10004695 15,274.25 true Example: Example: select ToDateTime("08/27/2018 08:30PM")>"2018-08-27T08:30"; result= true
```

GreaterThanOrEqual (&gt;=) Greater Than or equal operand is represented by the greater than (&gt;) and an Equal (=) sign.It accepts two inputs and returns true if the first is greater than or equal to the second, and false otherwise. Example Result Select(1&gt;=1); true Select(1&gt;=5); false

```
Select(5>=1); true Select('String'>='String') true Select('String'>='String1') false Select('String1'>='String') true Example Select (1.0)>="1"; result= true Explaination: 1.0 is converted to a string equal to 1. Example: select (11)>="2"; result= false Explaination: the number is converted to the string '11' which is less than '2') Example: Select (Version.[Version Name]*Product.[ProductID]*{ Measure.[UnitPrice], (Measure.[UnitPrice]>=10857.63) as transient.result}); result: Version Name ProductID UnitPrice result CurrentWorkingView FUR-BO-10001798 10,857.63 true CurrentWorkingView FUR-BO-10002545 10,671.23 fasle CurrentWorkingView FUR-BO-10002613 19,457.96 true CurrentWorkingView FUR-BO-10004695 15,274.25 true Example: select ToDateTime("08/27/2018 08:30PM")>="2018-08-27T08:30"; result= true
```

## Logical operators

These operators are used to perform logical operations on any required given expressions.The binary logical operators supported in IBPL include AND (&amp;&amp;) and OR (||).

## Logical AND(&amp;&amp;)

The Logical And Operator is represented by two ampersands (&amp;&amp;). It returns a true value if both of the input expressions evaluate to true, and returns false if either expression evaluates to false.

## Example:

```
Example Result Select((1>0)&&(1<2)); true Select((1<0)&&(1<2)); false Example: select (Version.[Version Name] * Product.[ProductID]*{ Measure.[UnitPrice], Measure.[UnitCost], (Measure.[UnitPrice]<12000 && Measure.[Unitprice]>Measure.[UnitCost]) as Transient.Result});
```

## result:

| Version Name       | ProductID       | UnitPrice   | UnitCost   | Result   |
|--------------------|-----------------|-------------|------------|----------|
| CurrentWorkingView | FUR-BO-10001798 | 10,857.63   | 10,000.00  | true     |
| CurrentWorkingView | FUR-BO-10002545 | 10,671.23   | 8,000.00   | true     |
| CurrentWorkingView | FUR-BO-10002613 | 19,457.96   | 18,000.00  | false    |
| CurrentWorkingView | FUR-BO-10004695 | 15,274.25   | 15,000.00  | false    |
| CurrentWorkingView | FUR-CH-10000454 | 578.92      | 500.00     | true     |

## Logical OR(||)

The Logical Or Operator is represented by vertical bars (||). It returns a true value if either of the input expressions evaluate to true, and returns false only if both expressions evaluate to false.

## Example:

```
Example Result Select((1>0) Select((1<0) select (Version.[Version Name] * Product.[ProductID]*{ Measure.[UnitPrice], measure.[UnitCost], (Measure.[UnitPrice]<12000 || Measure.[Unitprice]>measure.[UnitCost]) as Transient.Result}); result:
```

| Version Name       | ProductID       | UnitPrice   | UnitCost   | Result   |
|--------------------|-----------------|-------------|------------|----------|
| CurrentWorkingView | FUR-BO-10001798 | 10,857.63   | 10,000.00  | true     |
| CurrentWorkingView | FUR-BO-10002545 | 10,671.23   | 8,000.00   | true     |
| CurrentWorkingView | FUR-BO-10002613 | 19,457.96   | 18,000.00  | true     |
| CurrentWorkingView | FUR-BO-10004695 | 15,274.25   | 15,000.00  | true     |
| CurrentWorkingView | FUR-CH-10000454 | 578.92      | 500.00     | true     |

## Example:

```
( Select (Version.[Version Name]*Customer.[CustomerID]*{ Measure.[UnitSold],Measure.[PaymentMode], Measure.[OrderMode]})). filter (Measure.[PaymentMode]==[COD] || Measure.[PaymentMode]==[Credit Card]);
```

## result:

| Version Name       | CustomerID   |   UnitSold | PaymentMode   | OrderMode   |
|--------------------|--------------|------------|---------------|-------------|
| CurrentWorkingView | AA-10480     |         21 | Credit Card   | Web         |
| CurrentWorkingView | AD-10180     |          4 | Credit Card   | Web         |
| CurrentWorkingView | AG-10270     |          4 | COD           | E-mail      |
| CurrentWorkingView | AM-10360     |          4 | Credit Card   | Web         |

## Logical IN

Example:

```
select (Version.[Version Name]*Product.[ProductID]* Order .[OrderID]*Customer.[City]. filter (#.Name in {[Mumbai],[Kanpur]})* {Measure.[UnitSold], Measure.[UnitPrice]});
```

## result:

| Version Name       | ProductID       |   CustomerID | City   |   UnitSold | UnitPrice   |
|--------------------|-----------------|--------------|--------|------------|-------------|
| CurrentWorkingView | FUR-CH-10003061 |    101117837 | Mumbai |          1 | 337.46      |
| CurrentWorkingView | FUR-FU-10001706 |    101117837 | Mumbai |          1 | 85,338.28   |
| CurrentWorkingView | OFF-AR-10000940 |    102443921 | Kanpur |          3 | 1,272.84    |
| CurrentWorkingView | OFF-BI-10001721 |    101489913 | Kanpur |          2 | 52.80       |

## Example:

```
( Select (Version.[Version Name]*Customer.[CustomerID]*{ Measure.[UnitSold],Measure.[PaymentMode], Measure.[OrderMode]})). filter (Measure.[PaymentMode] in {[COD], [Credit Card]});
```

## result:

Version Name

CustomerID UnitSold PaymentMode OrderMode

| CurrentWorkingView   | AA-10480   |   21 | Credit Card   | Web    |
|----------------------|------------|------|---------------|--------|
| CurrentWorkingView   | AD-10180   |    4 | Credit Card   | Web    |
| CurrentWorkingView   | AG-10270   |    4 | COD           | E-mail |
| CurrentWorkingView   | AM-10360   |    4 | Credit Card   | Web    |

## Logical Not (~)

The Logical Not Operator is represented by Tilda (~).

Example:

```
( Select (Version.[Version Name]*Customer.[CustomerID]*{ Measure.[UnitSold],Measure.[PaymentMode], Measure.[OrderMode], Measure.[DeliveryMode]})). filter (~(Measure.[PaymentMode] in {[COD], [Credit Card]}) && Measure.[DeliveryMode] in {[2 Day delivery],[Standard Delivery]});
```

result:

| Version Name        | CustomerI D   |   UnitSol d | PaymentMod e   | OrderMod e   | DeliveryMod e      |
|---------------------|---------------|-------------|----------------|--------------|--------------------|
| CurrentWorkingVie w | DN-13690      |          15 | Debit Card     | Web          | Standard  Delivery |
| CurrentWorkingVie w | HM-14980      |           3 | Debit Card     | App          | Standard  Delivery |
| CurrentWorkingVie w | IM-15070      |           4 | Debit Card     | E-mail       | Standard  Delivery |
| CurrentWorkingVie w | JS-15685      |          13 | Debit Card     | Web          | 2 Day  delivery    |

## Special Operators

The special operators supported by IBPL are Attribute member operator and string operator.

## Attribute member operator

## AT(@)

At operator is represented by (@) symbol.It is used in a measure formula to refer to the value of a measure for a different member of one of the dimensions. The return value is an AttributeMember.

Example:

```
Select (Version.[Version Name]*Customer.[CustomerID]* Order .[OrderID]* Product.[ProductID]*Time.[Week].[1]*{ Measure.[UnitSold]@(Time.#.leadoffset(2)) as transient.Result});
```

## result:

| Version Name       | CustomerID   |   OrderID | ProductID       |   Week |   Result |
|--------------------|--------------|-----------|-----------------|--------|----------|
| CurrentWorkingView | AA-10480     | 100189578 | OFF-AP-10002892 |      1 |        4 |
| CurrentWorkingView | AA-10480     | 100189578 | OFF-BI-10003656 |      1 |        1 |
| CurrentWorkingView | AA-10480     | 100189578 | OFF-BI-10003910 |      1 |        4 |
| CurrentWorkingView | AA-10480     | 100189578 | OFF-PA-10002365 |      1 |        3 |

Explaination: In above query, it finds the value of unit sold for two weeks later, i.e. 3rd week and uses those values for week 1.

## ##String operator

## Concatenation (+)

Concatenation The Concatenation Operator is the plus sign (+). It is used to concatenate two strings. For example, "Web" + "COD" In this example, the two strings are combined and the return value is the single string 'WebCOD' .

## Example:

```
Select (Version.[Version Name]*[Product].[ProductID]*{ ("Rs. "+ Measure.[TextPrice]+"/- only") as Transient.Result, Measure.[TextPrice]});
```

Note: Here TextPrice is a measure created to store data of the measure UnitPrice as text string.

## result:

| Version Name       | ProductID       | Result              |   TextPrice |
|--------------------|-----------------|---------------------|-------------|
| CurrentWorkingView | FUR-BO-10001798 | Rs. 10857.63/- only |     10857.6 |
| CurrentWorkingView | FUR-BO-10002545 | Rs. 10671.23/- only |     10671.2 |
| CurrentWorkingView | FUR-BO-10002613 | Rs. 19457.96/- only |     19458   |
| CurrentWorkingView | FUR-BO-10004695 | Rs. 15274.25/- only |     15274.2 |

## String Functions

String functions are used in Ibpl to manipulate a string and query information about a string.

## ToString

```
select lower("Hello World!"); result: hello world! Example: result: Version Name CustomerName lowerpaymentmode CurrentWorkingView Suresh Chandra credit card CurrentWorkingView Kshitij Bajaj credit card
```

```
Converts the argument to string Example: select ToString("2+3"); result: 2+3 Upper It converts string of alphabets to uppercase. This cannot be used for Attribute Members. select upper("Hello World!"); result: HELLO WORLD! Example: select (Version.[Version Name]*Customer.[CustomerName]) on row , ({Upper(Measure.[PaymentMode]) as transient.UPPERPAYMENTMODE}) on column ; result: Version Name CustomerName UPPERPAYMENTMODE CurrentWorkingView Suresh Chandra CREDIT CARD CurrentWorkingView Kshitij Bajaj CREDIT CARD Lower It converts string of alphabets to lowercase. This cannot be used for Attribute Members. Example: select (Version.[Version Name]*Customer.[CustomerName]) on row , ({Lower(Measure.[PaymentMode]) as transient.lowerpaymentmode}) on column ;
```

## Numeric Functions

Numeric functions always return numbers (or the system-missing value whenever the result is indeterminate).The expression to be transformed by a function is called the argument. Most functions have a variable or a list of variables as arguments.

## Avg

Avg function is used to take the average of the input values.

Example:

```
Select (avg(8,4,9)); result: 7 Example: Select (Version.[Version Name]*Product.[ProductID]*{ Measure.[UnitPrice], Measure.[UnitCost],avg(measure.[UnitPrice], Measure.[UnitCost]) as transient.average});
```

result:

| Version Name       | ProductID       | UnitPrice   | UnitCost   | average   |
|--------------------|-----------------|-------------|------------|-----------|
| CurrentWorkingView | FUR-BO-10001798 | 10,857.63   | 10,000.00  | 10,429    |
| CurrentWorkingView | FUR-BO-10002545 | 10,671.23   | 8,000.00   | 9,336     |
| CurrentWorkingView | FUR-BO-10002613 | 19,457.96   | 18,000.00  | 18,729    |
| CurrentWorkingView | FUR-BO-10004695 | 15,274.25   | 15,000.00  | 15,137    |
| CurrentWorkingView | FUR-BO-10004834 |             | 12,000     | 12,000    |

## AvgWithNulls

AvgwithNulls function is used to get the averge including the null values in the data.

Example:

```
Select (Version.[Version Name]*Product.[ProductID]*{ Measure.[UnitPrice], Measure.[UnitCost], avgWithNulls(Measure.[UnitPrice], Measure.[UnitCost]) as transient.average});
```

result:

| Version Name       | ProductID       | UnitPrice   | UnitCost   | average   |
|--------------------|-----------------|-------------|------------|-----------|
| CurrentWorkingView | FUR-BO-10001798 | 10,857.63   | 10,000.00  | 10,429    |
| CurrentWorkingView | FUR-BO-10002545 | 10,671.23   | 8,000.00   | 9,336     |

| CurrentWorkingView   | FUR-BO-10002613   | 19,457.96   | 18,000.00   | 18,729   |
|----------------------|-------------------|-------------|-------------|----------|
| CurrentWorkingView   | FUR-BO-10004695   | 15,274.25   | 15,000.00   | 15,137   |
| CurrentWorkingView   | FUR-BO-10004834   |             | 12,000      | 6,000    |

## Count

Count function is used to count the number of members in an attribute member set.

Example:

```
select Customer.[CustomerName].count; result: 100 Example: 1.1 select (Customer.[City].[Mumbai]*Customer.[CustomerName].count);
```

```
result: error
```

Exaplaination: in above query it's showing error because there are two level attributes in the query where as count function can only work on a single level attribute.

1.2

```
select Customer.[City].[Mumbai].relatedmembers([CustomerName]).count; result:11
```

Note: The RelatedMembers() function finds related members at either a higher or lower level of the hierarchy.

Explaination: in above example its not showing error because there is only one level attribute.So its in above query it is counting the number of customers belonging to city Mumbai

Example:

```
select Customer.[City]. filter (#. Name in {[Mumbai], [Kanpur]}).relatedmembers([CustomerName]).count; result: 13
```

## Min

Min function is used to get the minimum value from the input values.

Example:

```
Select (min(8,5,9));
```

```
select result: 4/3=1.33;
```

```
result:5 Example: Select (Version.[Version Name]*Product.[ProductID]*{ Measure.[UnitPrice], Measure.[UnitCost], min(Measure.[UnitPrice], Measure.[UnitCost]) as transient.Result}); result: Version Name ProductID UnitPrice UnitCost Result CurrentWorkingView FUR-BO-10001798 10,857.63 10,000.00 10,000 CurrentWorkingView FUR-BO-10002545 10,671.23 8,000.00 8,000 CurrentWorkingView FUR-BO-10002613 19,457.96 18,000.00 18,000 CurrentWorkingView FUR-BO-10004695 15,274.25 15,000.00 15,000 Max Max function is used to get the maximum value from the input values. Example: Select (max(8,4,9)); result: 9 Example: Select (Version.[Version Name]*Product.[ProductID]*{ Measure.[UnitPrice], Measure.[UnitCost], max(Measure.[UnitPrice], Measure.[UnitCost]) as transient.Result}); result: Version Name ProductID UnitPrice UnitCost Result CurrentWorkingView FUR-BO-10001798 10,857.63 10,000.00 10,858 CurrentWorkingView FUR-BO-10002545 10,671.23 8,000.00 10,671 CurrentWorkingView FUR-BO-10002613 19,457.96 18,000.00 19,458 CurrentWorkingView FUR-BO-10004695 15,274.25 15,000.00 15,274 Safe Divide Example: (safedivide(4,3));
```

## Example:

```
Select (safedivide(8,5,3)); result: 8/5=1.6
```

Explaination: in above example the result is 1.6 as 8 can be divided by 5 so safe divide doesn't considers the third input value whereas it gives the calculated value as result.

Example:

```
Select (safedivide(8,0,3)); result: 3
```

Explaination: in above example the result is 3 because, instead of giving an error while dividing 8 by 0 safe divide function gives the third input value as result.

Example:

```
Select (Version.[Version Name]*Product.[ProductID]*{ Measure.[UnitPrice], Measure.[UnitCost], safedivide( (Measure.[UnitPrice]-Measure.[UnitCost])*100, Measure.[UnitCost],9) as transient.profitPercentage});
```

result:

| Version Name       | ProductID       | UnitPrice   | UnitCost   |   profitPercentage |
|--------------------|-----------------|-------------|------------|--------------------|
| CurrentWorkingView | FUR-BO-10001798 | 10,857.63   | 10,000.00  |                  9 |
| CurrentWorkingView | FUR-BO-10002545 | 10,671.23   | 8,000.00   |                 33 |
| CurrentWorkingView | FUR-BO-10002613 | 19,457.96   | 18,000.00  |                  8 |
| CurrentWorkingView | FUR-BO-10004695 | 15,274.25   | 15,000.00  |                  2 |

Note: in above example the third input value in safe divide function is goven as 9 so that where ever in the data if unit cost is 0 than it will give the profit percentage as 9 to result.

## Abs

Abs function is used to find the absolute value (modulus) of the number. Example:

```
Select (abs(-8.5)); result: 8.5 Example: Select (Version.[Version Name]*Product.[ProductID]*{ Measure.[UnitPrice], Measure.[unitCost], (Measure.[UnitCost] Measure.[UnitPrice]) as transient.NegProfit,abs(transient.NegProfit) as
```

```
transient.Profit});
```

result:

| Version Name       | ProductID       | UnitPrice   | UnitCost   | NegProfit   | Profit   |
|--------------------|-----------------|-------------|------------|-------------|----------|
| CurrentWorkingView | FUR-BO-10001798 | 10,857.63   | 10,000.00  | (858)       | 858      |
| CurrentWorkingView | FUR-BO-10002545 | 10,671.23   | 8,000.00   | (2,671)     | 2,671    |
| CurrentWorkingView | FUR-BO-10002613 | 19,457.96   | 18,000.00  | (1,458)     | 1,458    |
| CurrentWorkingView | FUR-BO-10004695 | 15,274.25   | 15,000.00  | (274)       | 274      |

## Coalesce

This function is used to replace null values with user supplied value.

```
SYNTAX: coalesce (<Q1>,<Q2>:value
```

Example:

```
Select (Version.[Version Name]*Product.[ProductID]*{ coalesce(Measure.[UnitPrice],0) as transient.Result});
```

## result:

| Version Name       | ProductID       | Result   |
|--------------------|-----------------|----------|
| CurrentWorkingView | FUR-BO-10001798 | 10,858   |
| CurrentWorkingView | FUR-BO-10002545 | 10,672   |
| CurrentWorkingView | FUR-BO-10002613 | 19,458   |
| CurrentWorkingView | FUR-BO-10004695 | 15,274   |
| CurrentWorkingView | FUR-BO-10004834 | 0        |
| CurrentWorkingView | FUR-CH-10000454 | 579      |

## Example:

```
Select (Version.[Version Name]*Product.[ProductID]*{ coalesce(Measure.[UnitPrice], 1.4*Measure.[UnitCost], 0) as transient.Result});
```

## result:

| Version Name       | ProductID       | Result   |
|--------------------|-----------------|----------|
| CurrentWorkingView | FUR-BO-10001798 | 10,858   |
| CurrentWorkingView | FUR-BO-10002545 | 10,672   |
| CurrentWorkingView | FUR-BO-10002613 | 19,458   |
| CurrentWorkingView | FUR-BO-10004695 | 15,274   |
| CurrentWorkingView | FUR-BO-10004834 | 16800    |
| CurrentWorkingView | FUR-CH-10000454 | 579      |

if Q1 is null &gt;)

## Round

This function rounds off the given number to the nearest integer. It doesn't accept textstring as input.

| Examples              | Result   |
|-----------------------|----------|
| select round (2.85);  | (3)      |
| select round (-2.85); | (-3)     |
| select round (2.5);   | (2)      |
| select round (-2.5);  | (-2)     |
| select round (3.5);   | (4)      |

Example:

```
Select (Version.[Version Name]*Product.[ProductID]*{ measure.[UnitPrice], round(Measure.[UnitPrice]) as transient.Result});
```

result:

| Version Name       | ProductID       | UnitPrice   | Result   |
|--------------------|-----------------|-------------|----------|
| CurrentWorkingView | FUR-BO-10001798 | 10,857.63   | 10,858   |
| CurrentWorkingView | FUR-BO-10002545 | 10,671.23   | 10,671   |
| CurrentWorkingView | FUR-BO-10002613 | 19,457.96   | 19,458   |
| CurrentWorkingView | FUR-BO-10004695 | 15,274.25   | 15,274   |

## Ceiling

The ceiling function takes numeric value and returns the least possible integer that is greater than or equal to the given numeric value.

| Examples                | Result   |
|-------------------------|----------|
| select ceiling (2.05);  | (3)      |
| select ceiling (-2.05); | (-2)     |

## Floor

The floor function takes numeric value and returns the greatest possible integer that is lesser than or equal to the given numeric value.

| Examples              | Result   |
|-----------------------|----------|
| select floor (2.05);  | (2)      |
| select floor (-2.05); | (-3)     |
| select floor ("2.85") | ERROR!   |

Note: floor and ceiling function don't accept text string as inputs that's why it has thrown an error in the last case!

select ([Version].[Version Name]* Product.[ProductID]) on row , ({Measure.[UnitPrice], floor(Measure.[UnitPrice]) as transient.floorfunction, ceiling(Measure.[UnitPrice]) as transient.ceilfunction}) on column ;

| Version Name       | ProductID       | UnitPrice   | floorfunction   | ceilfunction   |
|--------------------|-----------------|-------------|-----------------|----------------|
| CurrentWorkingView | FUR-BO-10001798 | 10,857.63   | 10,857          | 10,858         |
| CurrentWorkingView | FUR-BO-10002545 | 10,671.23   | 10,671          | 10,672         |
| CurrentWorkingView | FUR-BO-10002613 | 19,457.96   | 19,457          | 19,458         |
| CurrentWorkingView | FUR-BO-10004695 | 15,274.25   | 15,274          | 15,275         |
| CurrentWorkingView | FUR-BO-10004834 | 12,090.79   | 12,090          | 12,091         |

## Integer

It gives the integral value of the number. It can convert string to number also. It acts like floor function positive integers and cieling function for negative integers.

| Examples                | Result   |
|-------------------------|----------|
| select integer (2.85);  | (2)      |
| select integer (-2.85); | (-2)     |

## Float

It gives floating point decimal value from a number stored as text string

```
select ([Version].[Version Name]* Product.[ProductID]* {Measure.[TextPrice], (2*float(Measure.[TextPrice])) as transient.floattest});
```

Here TextPrice has datatype of string so if multiplication is tried without float (or integer) function it would throw an error. So the float command changes the text string to float and hence multiplication is possible.

## result:

| Version Name       | ProductID       |   TextPrice | floattest   |
|--------------------|-----------------|-------------|-------------|
| CurrentWorkingView | FUR-BO-10001798 |     10857.6 | 21,715      |
| CurrentWorkingView | FUR-BO-10002545 |     10671.2 | 21,342      |
| CurrentWorkingView | FUR-BO-10002613 |     19458   | 38,916      |
| CurrentWorkingView | FUR-BO-10004695 |     15274.2 | 30,549      |

## Sum

It sums all the arguments written within sum() argument

```
select (Version.[Version Name]*Product.[ProductID]*{ Measure.[UnitPrice], Measure.[UnitCost], Sum(Measure.[UnitCost], Measure.[UnitPrice]) as transient.Result}); following query is same as previous as they produce same results select (Version.[Version Name]*Product.[ProductID]*{ Measure.[UnitPrice], Measure.[UnitCost], (Measure.[UnitCost] + Measure.[UnitPrice]) as transient.Result});
```

result:

| Version Name       | ProductID       | UnitPrice   | UnitCost   | Result   |
|--------------------|-----------------|-------------|------------|----------|
| CurrentWorkingView | FUR-BO-10001798 | 10,857.63   | 10,000.00  | 20,858   |
| CurrentWorkingView | FUR-BO-10002545 | 10,671.23   | 8,000.00   | 18,671   |
| CurrentWorkingView | FUR-BO-10002613 | 19,457.96   | 18,000.00  | 37,458   |
| CurrentWorkingView | FUR-BO-10004695 | 15,274.25   | 15,000.00  | 30,274   |

Note: Remember it doesn't give the sum of the all data in the Measure instead it adds the 2 measures

## Time Functions

Default Format:

yyyy-MM-ddThh:mm:ss

## Current Date Time (now())

now() function displays current date-time

select

now()

Result:

## ToDateTime

Converts the text string to date

select

todatetime("Aug 30 2018");

Result: 2018-08-30T00:00:00

- If month is in numeral format then it should be placed before date otherwise todatetime might throw an error.
- If year is in yy format then the string should be MM/dd/yy (month-date-year) other string would give wrong results
- If month is in text and year is in yyyy format then no problem in placement.
- If year is in yy format date should be before year. So allowed formats would be datemonth-year or month-date-year

## DateAdd

This function is used to get a date, shifted forward or backward by a specified period of time.

```
SYNTAX: select DateADD (<date>, <number of intervals>, < type of interval>)
```

```
Keyword Usage <date> input date in which the interval have to be added or subtracted <number of intervals> no. of intervals that have to be added like 2 days, 5 days, 8 weeks. So 2,5,8 will be written in place of this <type of interval> it can be [year], [month], [week], [day] select dateadd (todatetime("08/27/2018"),2,[week]); Result: 2018-09-10T00:00:00 DateDiff This function is used to find the difference between to dates in terms of days, weeks,
```

months, years

```
SYNTAX: select DateDiff (<date1>, <date2, < type of
```

```
interval>)
```

```
Keyword Usage <date1>, <date2> 2 dates between which difference has to be found out <type of interval> it can be [year], [month], [week], [day], [hour], [minute], [second], select datediff (todatetime("12/31/1999T23:59:59.999999+00:00"), todatetime("08/31/2018"),[month]); Result: 224 (18 × 12 + 8 = 224) Snippet for Extracting Date, Month, Year from given string of time select ([Customer].[CustomerID]* Version.[Version Name]*{Measure.[TimeJ] as Transient.StoredTime, datediff(todatetime("1/1/2000"),Measure.[TimeJ],[year]) as transient.Year, (datediff(todatetime("1/1/"+ tostring(transient.Year)), Measure.[TimeJ],[month])+1) as transient.Month, (datediff(todatetime(tostring(transient.Month)+"/1/"+ tostring(transient.Year)), Measure.[TimeJ],[day])+1) as transient.Day, (datediff(todatetime(tostring(transient.Month)+"/"+tostring(transient.Day)+"/ "+ tostring(transient.Year)), Measure.[TimeJ],[ hour ])) as transient. Hour });
```

Result

| CustomerID   | StoredTime      |   Year |   Month |   Day |   Hour |
|--------------|-----------------|--------|---------|-------|--------|
| AA-10480     | 8/30/2018 08 AM |     18 |       8 |    30 |      8 |
| AD-10180     | 10/6/2018 11 PM |     18 |      10 |     6 |     23 |
| AG-10270     | 11/7/2011 12 AM |     11 |      11 |     7 |      0 |
| AM-10360     | 3/16/2007 09 PM |      7 |       3 |    16 |     21 |
| BH-11710     | 8/31/2018 08 AM |     18 |       8 |    31 |      8 |

## Conditional functions

The conditional functions all evaluate a condition and then return different answers depending on the condition value.

## If Then else

The if-then-else function is used to specify conditional logic in calculating the value for a measure.If-Then-else function works in the following way:

If condition is True, the statements following Then are executed. If condition is False, each ElseIf (if any) is evaluated in turn. When a True condition is found, the statements following the associated Then are executed.

## Example:

```
Select if (3>2) then (3+2) else (3-2); result: 5 Example: Select (Version.[Version Name]*Product.[ProductID]*{ Measure.[UnitPrice], Measure.[UnitCost], ( if ((Measure.[UnitPrice]-Measure.[UnitCost]>1000)) then ("High Profit Margin") else "Low Proft Margin") as transient.ProfitMargin});
```

result:

| Version Name       | ProductID       | UnitPrice   | UnitCost   | ProfitMargin       |
|--------------------|-----------------|-------------|------------|--------------------|
| CurrentWorkingView | FUR-BO-10001798 | 10,857.63   | 10,000.00  | Low Proft Margin   |
| CurrentWorkingView | FUR-BO-10002545 | 10,671.23   | 8,000.00   | High Profit Margin |
| CurrentWorkingView | FUR-BO-10002613 | 19,457.96   | 18,000.00  | High Profit Margin |
| CurrentWorkingView | FUR-BO-10004695 | 15,274.25   | 15,000.00  | Low Proft Margin   |

## Example:

```
Select (Version.[Version Name]*Product.[ProductID]*{ (Measure.[UnitPrice]*Measure.[UnitSold]) as transient.totalRevenue, ( if ((transient.totalRevenue > 5000)) then (0.15*Measure.[UnitPrice]) else 0) as transient.Discount,Measure.[UnitPrice], (Measure.[UnitPrice]-transient.Discount) as transient.NewUnitPrice});
```

## result:

| Version Name       | ProductID        | totalRevenue   | Discount   | UnitPrice   | NewUnitPrice   |
|--------------------|------------------|----------------|------------|-------------|----------------|
| CurrentWorkingView | FUR-BO- 10001798 | 32,573         | 1,629      | 10,857.63   | 9,229          |
| CurrentWorkingView | FUR-BO- 10002545 | 10,671         | 1,601      | 10,671.23   | 9,071          |
| CurrentWorkingView | FUR-BO- 10002613 | 58,374         | 2,919      | 19,457.96   | 16,539         |
| CurrentWorkingView | FUR-BO- 10004695 | 45,823         | 2,291      | 15,274.25   | 12,983         |

## IsNull

Example:

```
Select (Version.[Version Name]*Product.[ProductID]*{ Measure.[UnitPrice], Isnull(Measure.[UnitPrice]) as transient.Result});
```

result:

| Version Name       | ProductID       | UnitPrice   | Result   |
|--------------------|-----------------|-------------|----------|
| CurrentWorkingView | FUR-BO-10001798 | 10,857.63   | false    |
| CurrentWorkingView | FUR-BO-10002545 | 10,671.23   | false    |
| CurrentWorkingView | FUR-BO-10002613 | 19,457.96   | false    |
| CurrentWorkingView | FUR-BO-10004695 | 15,274.25   | false    |

Example:

```
Select (Version.[Version Name]*Product.[ProductID]*{ Measure.[UnitPrice], ~Isnull(Measure.[UnitPrice]) as transient.Result});
```

result:

| Version Name       | ProductID       | UnitPrice   | Result   |
|--------------------|-----------------|-------------|----------|
| CurrentWorkingView | FUR-BO-10001798 | 10,857.63   | true     |

| CurrentWorkingView   | FUR-BO-10002545   | 10,671.23   | true   |
|----------------------|-------------------|-------------|--------|
| CurrentWorkingView   | FUR-BO-10002613   | 19,457.96   | true   |
| CurrentWorkingView   | FUR-BO-10004695   | 15,274.25   | true   |

Note: in above query ~ is used before Isnull to make the function as IsNotNull.

## Analytical

Executing analytical functions organizes data into partitions, computes functions over these partitions in a specified order, and returns the result.

## Member set analytical function

## filter

This function filters the data according to the provided condition(s) that are specified in the paratheses()

## Example:

```
Select (Version.[Version Name]*Customer.[CustomerName]* Customer.[State]. filter (#. Name in {[Maharashtra], [Uttar Pradesh]})*{ Measure.[UnitSold]});
```

## result:

| Version Name       | CustomerName    | State         |   UnitSold |
|--------------------|-----------------|---------------|------------|
| CurrentWorkingView | Mohammed Zakir  | Uttar Pradesh |          3 |
| CurrentWorkingView | Ashish Dungdung | Maharashtra   |          4 |
| CurrentWorkingView | Chloe Bhatore   | Maharashtra   |         11 |
| CurrentWorkingView | Farooq Mohammed | Uttar Pradesh |          8 |

## Example:

```
( Select (Version.[Version Name]*Product.[ProductID]*Customer.[CustomerID]* Customer.[CustomerName]*Product.[SubCategory]*Customer.[State]. filter ( #.Name in {[Maharashtra], [Delhi]})*{Measure.[UnitSold], Measure.[PaymentMode]})) . filter (Measure.[PaymentMode] in {[COD], [Credit Card]});
```

## result:

| Version Name        | Produc tID     | Custom erID   | Customer Name    | SubCate gory   | State        |   UnitS old | Payment Mode   |
|---------------------|----------------|---------------|------------------|----------------|--------------|-------------|----------------|
| CurrentWorki ngView | OFF- PA- 10001 | AD- 10180     | Ashish  Dungdung | Resistor s     | Maharas htra |           2 | Credit  Card   |

| CurrentWorki        | 950  FUR-          | GH-       | Gaurav  Hashmi   | Bed   | Delhi        |   1 | Credit   |
|---------------------|--------------------|-----------|------------------|-------|--------------|-----|----------|
| CurrentWorki ngView | FUR- CH- 10001 146 | PS- 18970 | Piya  Sharma     | Chair | Delhi        |   4 | COD      |
| CurrentWorki ngView | OFF- FA- 10000 304 | DP- 13000 | Diana  Penty     | Fan   | Maharas htra |   4 | COD      |

Note: In above example filter has been used twice in the query firstly the filter function will work along with the select query and will filter the data for the states 'Maharashtra' and 'Delhi' and will give the corrosponding required input attributes and measure.whereas there is a second filter which is used as post filter which will work when the select query is executed and filter the select query on the basis of payment mode that should be 'COD' and 'Credit Card'.

## Find

find function is used to locate a specific member in the data.

```
Example: Select (Version.[Version Name]*Customer.[City].find("Kanpur")*{ Measure.[UnitSold]}); Result: Version Name City UnitSold CurrentWorkingView Kanpur 15 Note: the above query can also be writen as Select (Version.[Version Name]*Customer.[City].[Kanpur]*{Measure.[UnitSold]});
```

## Filter Predicate

Filter predicate is similar to filter function but it filters the member of level attributes based on the following expressions

## Startswith:

It gives the member which starts with the user supplied string.

## Example:

```
Select (Version.[Version Name]*Customer.[City]*Customer.[CustomerName]. filter (#. Name startswith([A]))*{Measure.[UnitSold]});
```

## result:

| Version Name       | City     | CustomerName    |   UnitSold |
|--------------------|----------|-----------------|------------|
| CurrentWorkingView | Pune     | Ashish Dungdung |          4 |
| CurrentWorkingView | Valsad   | Aarush Mittal   |          4 |
| CurrentWorkingView | Tirupati | Akash Gowtham   |          4 |
| CurrentWorkingView | Bareilly | Atish Awasthi   |         21 |

## Example:

```
Select (Version.[Version Name]*Customer.[City]*Customer.[CustomerName]. filter (#. Name  startswith([S]) || #.Name startswith([J]))*{Measure.[UnitSold]});
```

## result:

| Version Name       | City    | CustomerName         |   UnitSold |
|--------------------|---------|----------------------|------------|
| CurrentWorkingView | Mumbai  | Jitendra Kapoor      |          3 |
| CurrentWorkingView | Mumbai  | Shahrukh Khan        |         11 |
| CurrentWorkingView | Chennai | Shruti Hassan        |          3 |
| CurrentWorkingView | Mumbai  | Jacqueline Fernandes |          3 |

## Endswith:

It gives the member which ends with the user supplied string.

## Example:

```
Select (Version.[Version Name]*Customer.[City]*Customer.[CustomerName]. filter (#. Name endswith([A]))*{Measure.[UnitSold]});
```

## result:

| Version Name       | UnitSold   | City            |   CustomerName |
|--------------------|------------|-----------------|----------------|
| CurrentWorkingView | Gwalior    | Tanya Sharma    |              3 |
| CurrentWorkingView | Jhansi     | Ishaan Mishra   |              4 |
| CurrentWorkingView | Gurgaon    | Ravinder Ahuja  |              7 |
| CurrentWorkingView | Gurgaon    | Deepak Kathuria |              5 |

## Example:

```
Select (Version.[Version Name]*Customer.[City]*Customer.[CustomerName]. filter (#. Name  endswith([E]) || #.Name endswith([S]))*{Measure.[UnitSold]});
```

## result:

| Version Name       | City    | CustomerName         |   UnitSold |
|--------------------|---------|----------------------|------------|
| CurrentWorkingView | Chennai | Jigyasa E            |         23 |
| CurrentWorkingView | Panaji  | Mary Jane            |          1 |
| CurrentWorkingView | Mumbai  | Jacqueline Fernandes |          3 |
| CurrentWorkingView | Nashik  | Chloe Bhatore        |         11 |

## Contains

It gives the member which contains the user supplied string.

Exanple:

```
Select (Version.[Version Name]*Customer.[City]. filter (#.Name contains "ja" || #.Name contains "mu" )*{Measure.[UnitSold]});
```

## result:

| Version Name       | City        |   UnitSold |
|--------------------|-------------|------------|
| CurrentWorkingView | Muzzafarpur |          8 |
| CurrentWorkingView | Mumbai      |         59 |
| CurrentWorkingView | Yamunanagar |          5 |
| CurrentWorkingView | Jaisalmer   |         15 |
| CurrentWorkingView | Jaipur      |          1 |
| CurrentWorkingView | Jallandhar  |          3 |

Note: startswith, endswith and contains can be used together using or(||) or and(&amp;&amp;) operator in between.

## Category

This is used only for the defined attribute properties to show the data with respect to the properties.

Example:

## Filter\_By\_Updates

This function is used to get the updated data or cells at the time of transaction.if no changes has been made in the data than it doesn't returns anything.

Note: Whenever there is a change made in excel UI or in Web UI and those changes aren't saved on the Live server, User can check the changes made using Filter\_by\_Upadtes function.

Example: Let's say the user has changed the unit price for some product IDs. So one can check the changes using below query

```
SelectSELECT (Product.[ProductID] * Version.[Version Name].[CurrentWorkingView] * {Measure.[UnitPrice] }) on row , () on column FILTER_BY_UPDATES ;
```

result: It will show the updated unit price rows.

Example:

```
update Measure.[UnitPrice]@(Product.[ProductID]. filter (#.Name in {[FUR-BO-10002545]}), Version.[Version Name]. filter (#. Key == 0)) = 10672 SELECT (Product.[ProductID] * Version.[Version Name].[CurrentWorkingView] * {Measure.[UnitPrice] }) on row , () on column FILTER_BY_UPDATES ;
```

result:

ProductID Version Name UnitPrice FUR-BO-10004834 CurrentWorkingView 12,092

## Element

This function is used to get the single member out of a list of members that have been queried.

```
Example: (Version.[Version Name]*Customer.[CustomerName].
```

```
Select element(9)*{Measure.[UnitSold]}); result: Version Name CustomerName UnitSold CurrentWorkingView Chetan Sharma 19
```

Explaination: here in element() paranthesis a digit is specified like 0, 1 etc that tells which element in the data set is to be extracted. 0 stands for 1st element, 1 for 2nd and so on. In anove example, 10th element is extracted by giving the value 9 to element argument.

Note: for directly extracting the first and last element user can use the first() and last() function

Examlple:

```
Select (Version.[Version Name]*Customer.[CustomerName].First()*{Measure.[UnitSold]}); result Version Name CustomerName UnitSold CurrentWorkingView Aarush Mittal 4 Example: Select (Version.[Version Name]*Customer.[CustomerName].last()*{Measure.[UnitSold]}); result: Version Name CustomerName UnitSold CurrentWorkingView Zayn Dawood 6 AncestorAtLevel This function is used to traverse up in the hierarchy by giving the name of higher level. Example 1.1: Select Time.[Day].[30].ancestorsAtLevel([Week]); result: error Example 1.2: Select Time.[Day]. filter (#. Name == "30").ancestorsAtLevel([Week]); result: 5 Note: for reason look into the explaination section at the bottom of the page. Example: Select Time.[Day]. filter (#. Name in {[12],[1],[30]}).ancestorsAtLevel([Week]); result: 2, 1, 5 Example: select Customer.[City]. filter (#. Name in {[Kanpur], [Mumbai]}).ancestorsAtLevel([State]); result: Uttar Pradesh, Maharashtra DescendantAtLevel
```

This function is used to traverse down in the hierarchy by giving the name of lower level.

## Example1.1:

```
Select Time.[Week]. filter (#. Name == [5]).descendantsAtLevel([Day]); result: 27, 28, 29, 30, 31 Example1.2: Select Time.[Week].[5].descendantsAtLevel([Day]);
```

result: error

Note: for reason look into the explaination section at the bottom of the page.

## Example:

select Customer.[State]. filter (#. Name in {[Maharashtra], [Uttar Pradesh]}).descendantsAtLevel([City]);

result: Pune, Nagpur, Mumbai, Nashik, Bareilly, Allahabad, Kanpur,Jhansi etc

## RelatedMembers

This function is used to get the related members at the lower or higher level in the hierarchy.If the user wants to move up in the hierarchy than this function is similar to ancestorsAtLevel function whereas if to move down in the hierarchy than it is similar to decsendantsAtLevel function.

## Example:

```
Select (Version.[Version Name]*Customer.[CustomerName]. filter (#. Name in relatedmembers([City])*{Measure.[UnitSold]});
```

```
{[Jacqueline Fernandes], [Rajnikanth]}). result: Version Name City UnitSold CurrentWorkingView Mumbai 59 CurrentWorkingView Chennai 29 Example: select Customer.[City]. filter (#.Name in {[Mumbai], [Kanpur], [ New Delhi]}) .relatedmembers([CustomerName]).count; result: 17
```

Exaplaination: In above query the total customers from city Mumbai, Kanpur and New Delhi is counted using count function.

## UnionWith

This function joins two lists of members together into one unique list of members.

Example:

```
Select (Version.[Version Name]*Time.[Week]. filter (#.Name==[1]). unionWith(Time.[Week]. filter (#.Name==[5]))*{Measure.[UnitSold]});
```

```
result: Version Name Week UnitSold CurrentWorkingView 1 95 CurrentWorkingView 5 98
```

## IntersectWith

This function finds the members in one list that are also available in other list. The return value is an AttributeMemberSet. The function takes one parameter, which is the second AttributeMemberSet to intersect with the first.

## Example:

```
Select (Version.[Version Name]*Time.[Week]. filter (#.Name==[1]). intersectWith(Time.[Week].[1])*{Measure.[UnitSold]}); result: 1 Example: Select (Version.[Version Name]*Time.[Week]. intersectWith(Time.[Week].[1])*{Measure.[UnitSold]}); result: 1 Example: Select (Version.[Version Name]*Time.[Week].[1]. intersectWith(Time.[Week])*{Measure.[UnitSold]}); result: 1
```

## Member analytical function

## Ancestor

This function is used to traverse the user supplied hierarchy from a member at one level to a parent at a higher level in the hierarchy.

## Example 1.1:

```
Select (Version.[Version Name]*Product.[SubCategory].[ Table ]. ancestor(Product.[Product],1)*{Measure.[UnitSold]});
```

result:

```
Version Name Category UnitSold CurrentWorkingView Furniture 134
```

Example 1.2:

```
Select (Version.[Version Name]*Product.[SubCategory]. filter (#.Name ==[ Table ]). ancestor(Product.[Product],1)*{Measure.[UnitSold]});
```

result: error

Note: for reason look into the explaination section at the bottom of the page.

## Children

This function is used to traverse the user supplied hierarchy from a member at one level to a Child at a Lower level in the hierarchy.

## Example :

```
Select (Version.[Version Name]*Customer.[State].[Uttar Pradesh]. children(Customer.[Customer Hierarchy ])*{Measure.[UnitSold]});
```

result:

| Version Name       | City        |   UnitSold |
|--------------------|-------------|------------|
| CurrentWorkingView | Moradabad   |          3 |
| CurrentWorkingView | Muzzafarpur |          8 |
| CurrentWorkingView | Jhansi      |         13 |
| CurrentWorkingView | Kanpur      |         15 |
| CurrentWorkingView | Varanasi    |          4 |
| CurrentWorkingView | Allahabad   |         13 |
| CurrentWorkingView | Bareilly    |         21 |

Example:

```
select (Version.[Version Name]*Time.[Day].[24].ancestor(Time.[Time Hierarchy ],1)*{Measure.[UnitSold]});
```

result:

| Version Name       |   Week |   UnitSold |
|--------------------|--------|------------|
| CurrentWorkingView |      4 |        119 |

Example:

```
select (Version.[Version Name]*Time.[Week].[4].children(Time.[Time Hierarchy ])*{Measure.[UnitSold]});
```

result:

| Version Name       |   Day |   UnitSold |
|--------------------|-------|------------|
| CurrentWorkingView |    21 |          6 |
| CurrentWorkingView |    22 |          9 |
| CurrentWorkingView |    23 |         24 |
| CurrentWorkingView |    24 |         15 |
| CurrentWorkingView |    25 |         25 |
| CurrentWorkingView |    26 |         15 |
| CurrentWorkingView |    27 |         25 |

## LeadOffset

This function is used to traverse from one member to another member at the same level in the hierarchy.

## Example:

```
Select (Version.[Version Name]*Time.[Day].[22].leadoffset(12)*{Measure.[UnitSold]}); result: |Version Name|Day|UnitSold| |-|-|-| |CurrentWorkingView|10|19| Example: Select (Version.[Version Name]*Time.[Week].[2].leadoffset(2)*{Measure.[UnitSold]}); result: Version Name Week UnitSold CurrentWorkingView 4 119
```

## Between

This function is used to find all of the members with keys or element() greater than or equal to the first member and less than or equal to the second member.

## Example:

```
Select between (Customer.[CustomerName].element(0), Customer.[CustomerName].element(2));
```

result: Arush Mittal, Akash Gowtham, Ashish Dungdung

Example:

```
Select Between (Time.[Week].[2], Time.[Week].[4]); result: 2, 3, 4
```

Note: above queries can also be executed using filter function and operators rather than using between function. as bewteen function doesn't allow the user to execute bewteen function query along with corresponding measures which can be done in filter function.

## With Calculated members

This Function is used to get the agrregation value of any measure.

Example:

```
With calculted measure Version.[Version Name].[CurrentWorking] Select (Version.[Version Name].[CurrentWorkingView]*{Measure.[UnitPrice]});
```

result:

```
Version Name UnitPrice CurrentWorkingView 12,615
```

## Example:

```
With calculted measure Version.[Version Name].[CurrentWorking] Select (Version.[Version Name].[CurrentWorkingView]*{Measure.[UnitSold]});
```

result:

Version Name UnitSold CurrentWorkingView 651

Note: In above examples the aggreagtion for Unit price is Average and for Unit Sold is Sum.

## Explanation:

There are two ways of selecting member in scope

1. [Dimension].[Attribute].[Member] (Writing the name using . )
2. [Dimension].[Attribute].filter(#.Name == [Member]) (using filter)

Although these seem to be similar but they are not same. First one is member selection and second one is attribute selection.

Let's look at the results when we run it for [Time].[Week] and Member=3

select [Time].[Week].[3]; Result: Key Name DisplayName Week 𝐼𝑛𝐴𝑐𝑡𝑖𝑣𝑒|𝑊𝑒𝑒𝑘 InPast Week$IsCurrent

```
2018-0813T00:00:00 3 3 select [Time].[Week]. filter (#.Name == [3]);
```

```
Result:
```

| Key                  |   Name |   DisplayName | Image   | IsEditable   | MemberProperties                                |
|----------------------|--------|---------------|---------|--------------|-------------------------------------------------|
| 2018/08/13  00:00:00 |      3 |             3 |         | true         | [{'ValueDataType':'boolean',  'PropertyValue':n |

## Contrasting with

select

[Time].[Week]

Result:

| Key                     |   N a m e |   Displ ayNa me | Im ag e   | IsEd itabl e   | MemberProperties                                                                                                                                                                                                                              |
|-------------------------|-----------|-----------------|-----------|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2018 /07/ 30  00:00 :00 |         1 |               1 |           | true           | [{'ValueDataType':'boolean','PropertyValue':null,'Transl ationName':null,'PropertyName':"Week$InActiv  |2018/08/06  00:00:00|2|2||true|[{"ValueDataType":"boolean","Propert yValue":null,"TranslationName":null,"PropertyName":"We ek$InActiv |
| 2018 /08/ 13  00:00     |         3 |               3 | true      |                | [{'ValueDataType':'boolean','PropertyValue':null,'Transl ationName':null,'PropertyName':"Week$InActiv                                                                                                                                         |

Notice that filter is just filtering the row of [Time].[Week] which has Member = 3. So it is like selecting attribute. Whereas, [Time].[Week].[3] is going to selection of member.

** So ancestor, children are attribute analytic function where as AncestorAtLevel, DecsendantAtLevel are member analytic function. **

## Spreading Rules

Spreading is assigning values at the child level according to the value Provided at the parent level.

mPower has two choices of Spreading Rules to choose from. The rule can be set through the Configuration UI. In the tenant database, the Spreading Rule is defined in the table 'MeasureSpreads'.

If no spreading rule is defined for a measure, then the measure cannot be updated/edited at a higher level of aggregation.

All types of Basis measure should have the same granularity as the target measure

Note: Spreading can't be done on calculative measures user need to upload the fact file comprising of the measure in which spreading needs to be done and it must contain the grains at higher level.

## Types of Spreading.

## CopyToLeaves

If user choose spreading type as CopyToLeaves, the new value set at the higher level of aggregation is copied to all of the children. Let's say the value of unit sold at state level is changed to 200, then value of unit sold for each city comes under that state will be set as 200.

Note:Copy to leaves makes sense when aggregation type is set to 'Min', 'Max', 'Avg', 'AvgNonNull' and 'AvgNonNullOrZero'.

Explaination: Let's say if the aggregation type is 'Sum' and the parent value is given as 100 and it has two two childs so it will copy the value 100 to both the childs .as the aggregation is sum when it will get aggregated the value at parent level will come as 200 but it was assigned as 100. Whereas if the aggregation type was 'Avg' it will give the value as 100 which is value assigned at parent level.Similarly it makes sense for 'AvgNonNull', 'AvgNonNullOrZero', 'Min', 'Max'.

## Spreading Config:

| Basis Measure Type   | Basis Measure   | SpreadingType   |
|----------------------|-----------------|-----------------|
| BasisMeasure         | UnitSold        | CopyToLeaves    |

Example:

| City      |   UnitSold |
|-----------|------------|
| Valsad    |          4 |
| Ahmedabad |          6 |

If the parent (State) value for these cities is changed to 8 then this value will be copied at all leaves of target measure.

## Result:

Select (Version.[Version Name]*Customer.[State].[Gujarat]*Customer.[City]* {Measure.[UnitSold], Measure.[NewUnitSold]});

| Version Name       | State   | City   |   UnitSold |   NewUnitSold |
|--------------------|---------|--------|------------|---------------|
| CurrentWorkingView | Gujarat | Valsad |          4 |             8 |

CurrentWorkingView Gujarat Ahmedabad 6

## DistributeToLeaves

If user choose spreading type as DistributeToLeaves, the child values are set proportional to corresponding child values of a specified measure. If the measure with the spreading rule uses itself as the basis, then this is referred to a 'self basis'.

## Spreading Config:

| Basis Measure Type   | Basis Measure   | SpreadingType      |
|----------------------|-----------------|--------------------|
| BasisMeasure         | UnitSold        | DistributeToLeaves |

Example: Self basis

| City      |   UnitSold |
|-----------|------------|
| Valsad    |          4 |
| Ahmedabad |          6 |

If the parent value that is state for these cities is changed to 15 then the values will be changed proportional to their current values.

Result:

Select (Version.[Version Name]*Customer.[State].[Gujarat]*Customer.[City]* {Measure.[UnitSold], Measure.[NewUnitSold]});

| Version Name       | State   | City      |   UnitSold |   NewUnitSold |
|--------------------|---------|-----------|------------|---------------|
| CurrentWorkingView | Gujarat | Valsad    |          4 |             6 |
| CurrentWorkingView | Gujarat | Ahmedabad |          6 |             9 |

Example: Basis Measure

Spreading Config:

| Basis Measure Type   | Basis Measure   | SpreadingType      |
|----------------------|-----------------|--------------------|
| BasisMeasure         | UnitSoldRatio   | DistributeToLeaves |
| City                 | UnitSold        |                    |
| Valsad               | 4               |                    |
| Ahmedabad            | 6               |                    |

If the parent value that is state for these cities is changed to 18 then the values will be changed proportional to the basis measure UnitSoldRatio.

Result:

Select (Version.[Version Name]*Customer.[State].[Gujarat]*Customer.[City]* {Measure.[UnitSold], Measure.[UnitSoldRatio] Measure.[NewUnitSold]});

| Version Name       | State   | City      |   UnitSold |   UnitSoldRatio |   NewUnitSold |
|--------------------|---------|-----------|------------|-----------------|---------------|
| CurrentWorkingView | Gujarat | Valsad    |          4 |               1 |             6 |
| CurrentWorkingView | Gujarat | Ahmedabad |          6 |               2 |            12 |

Note: In above example UnitSoldRatio is the basis measure according to whose proportinal the unit sold value is distributed among cities, NewUnitSold is the target measure where the spreading is done.

## Types of Basis Measures

Note: All the examples are setup as per the DistributeToLeave type spreading

## Basis Measure

- Higher level value is distributed to leaves, proportional to the Basis Measure Value at the leaves.
- If the sum of Basis Measure at leaves is zero or null then the higher level value is distributed equally among all possible leaves. New intersections are created.

## Spreading Config:

| Basis Measure Type   | Basis Measure  SpreadingType   |
|----------------------|--------------------------------|
| BasisMeasure         | Sold  DistributeToLeaves       |

Example: When basis measure has non zero and non null values

| Version Name       | Brand     |   Price |
|--------------------|-----------|---------|
| CurrentWorkingView | Palmolive |      80 |
| CurrentWorkingView | Herbal    |      40 |
| CurrentWorkingView | Colgate   |      45 |

the basis measure is set a sold according to whose proportional the unit Price values will be distributed at SKU Level.

```
Select (Version.[Version Name]*Item.[Brand]*Item.[SKU]* {Measure.[Price], Measure.[Sold], Measure.[NewPrice]});
```

## Result:

| Version Name       | Brand     | SKU        |   Price |   Sold |   NewPrice |
|--------------------|-----------|------------|---------|--------|------------|
| CurrentWorkingView | Palmolive | Cinthol    |      50 |      1 |         20 |
| CurrentWorkingView | Herbal    | Hamam      |      40 |      1 |         40 |
| CurrentWorkingView | Palmolive | Breeze     |      30 |      3 |         60 |
| CurrentWorkingView | Colgate   | Attraction |      25 |      2 |         30 |
| CurrentWorkingView | Colgate   | Sensitive  |      20 |      1 |         15 |

Example: When the basis Measure has zero and null values.

| Version Name                                                                                                    | Brand                                                                                                           | Price                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| CurrentWorkingView                                                                                              | Palmolive                                                                                                       | 80                                                                                                              |
| CurrentWorkingView                                                                                              | Herbal                                                                                                          | 40                                                                                                              |
| CurrentWorkingView                                                                                              | Colgate                                                                                                         | 45                                                                                                              |
| Select (Version.[Version Name]*Item.[Brand]*Item.[SKU]* {Measure.[Price], Measure.[Sold], Measure.[NewPrice]}); | Select (Version.[Version Name]*Item.[Brand]*Item.[SKU]* {Measure.[Price], Measure.[Sold], Measure.[NewPrice]}); | Select (Version.[Version Name]*Item.[Brand]*Item.[SKU]* {Measure.[Price], Measure.[Sold], Measure.[NewPrice]}); |

## Result:

| Version Name       | Brand     | SKU        |   Price | Sold   |   NewPrice |
|--------------------|-----------|------------|---------|--------|------------|
| CurrentWorkingView | Palmolive | Cinthol    |      50 |        |         40 |
| CurrentWorkingView | Herbal    | Hamam      |      40 | 0      |         40 |
| CurrentWorkingView | Palmolive | Breeze     |      30 | 0      |         40 |
| CurrentWorkingView | Colgate   | Attraction |      25 | 1      |         15 |
| CurrentWorkingView | Colgate   | Sensitive  |      20 | 2      |         30 |

Note: In above result for brand Palmolive the basis measure values are null or zero so the distrubution of new unit price is done in equal Proportion for its SKUs.

## Similar example:

| Version Name       | Brand     | SKU        |   Price | Sold   |   NewPrice |
|--------------------|-----------|------------|---------|--------|------------|
| CurrentWorkingView | Palmolive | Cinthol    |      50 |        |         40 |
| CurrentWorkingView | Herbal    | Hamam      |      40 | 0      |         40 |
| CurrentWorkingView | Palmolive | Breeze     |      30 | 0      |         40 |
| CurrentWorkingView | Colgate   | Attraction |      25 |        |         23 |
| CurrentWorkingView | Colgate   | Sensitive  |      20 |        |         23 |

Note: Here for all the brands basis measure values are zero or null so the distribution of unit price is in respective equal proportion for their SKUs

## Null Basis Measure

This measure is given after giving the basis measure. Null Basis Measure works in the following way.

If sum of Basis Measure at all the leaves is zero/null then Null Basis Measure is used for spreading. So firstly the basis measure is checked for spreading but if the sum of basis measure values at all the leaves is zero/null then null basis measure values are used for spreading.

## Spreading Config:

| Basis Measure Type   | Basis Measure        | SpreadingType      |
|----------------------|----------------------|--------------------|
| BasisMeasure         | SoldBasisMeasure     | DistributeToLeaves |
| NullBasisMeasure     | SoldNullBasisMeasure | DistributeToLeaves |

## Example:

| Version Name       | Brand     |   Price |
|--------------------|-----------|---------|
| CurrentWorkingView | Palmolive |      80 |
| CurrentWorkingView | Herbal    |      40 |
| CurrentWorkingView | Colgate   |      45 |

Select (Version.[Version Name]*Item.[Brand]*Item.[SKU]*

{Measure.[Price], Measure.[SoldBasisMeasure],

Measure.[SoldNullBasisMeasure], Measure.[NewPrice]});

Result:

| Version Name        | Brand      | SKU         |   Pric e | SoldBasisMea ure   |   SoldNullBasisMea sure | NewPri ce   |
|---------------------|------------|-------------|----------|--------------------|-------------------------|-------------|
| CurrentWorking View | Palmoli ve | Cinthol     |       50 |                    |                       1 |             |
| CurrentWorking View | Herbal     | Hamam       |       40 | 0                  |                       1 | 40          |
| CurrentWorking View | Palmoli ve | Breeze      |       30 | 3                  |                       4 | 80          |
| CurrentWorking View | Colgate    | Attracti on |       25 |                    |                       1 | 15          |
| CurrentWorking View | Colgate    | Sensitiv e  |       20 |                    |                       2 | 30          |

Note: In above example SoldBasisMeasure is the basis measure and SoldNullBasisMeasure is the Null Basis Measure. For brand Colgate in basis measure the values are zero/null so the distribution takes place according to the values in null basis measure.

## Similar example:

| Version Name        | Brand      | SKU     |   Pric e | SoldBasisMea sure   |   SoldNullBasisMea sure |   NewPri ce |
|---------------------|------------|---------|----------|---------------------|-------------------------|-------------|
| CurrentWorking View | Palmoli ve | Cinthol |       50 |                     |                       1 |          16 |
| CurrentWorking View | Herbal     | Hamam   |       40 | 0                   |                       1 |          40 |
| CurrentWorking View | Palmoli ve | Breeze  |       30 | 0                   |                       4 |          64 |

| CurrentWorking View   | Colgate   | Attracti on   |   25 |   1 |   15 |
|-----------------------|-----------|---------------|------|-----|------|
| CurrentWorking        | Colgate   | Sensitiv      |   20 |   2 |   30 |

## Similar example:

| Version Name        | Brand      | SKU         |   Pric e | SoldBasisMea sure   | SoldNullBasisMea sure   |   NewPri ce |
|---------------------|------------|-------------|----------|---------------------|-------------------------|-------------|
| CurrentWorking View | Palmoli ve | Cinthol     |       50 |                     |                         |          40 |
| CurrentWorking View | Herbal     | Hamam       |       40 | 0                   |                         |          40 |
| CurrentWorking View | Palmoli ve | Breeze      |       30 | 0                   | 0                       |          40 |
| CurrentWorking View | Colgate    | Attracti on |       25 |                     |                         |          23 |
| CurrentWorking View | Colgate    | Sensitiv e  |       20 |                     |                         |          23 |

Note: In above example values for basis measure as well as null basis measure are zero/Null so the distribution takes place in equal proportion.

## Assortment Measure

This measure is used to flag the leaf level assignments.This measure is given after giving the Basis measure/Null basis measure or after both.

- If this measure is setup then assignment happens only on leaves where Assortment Basis Measure is not null &amp; greater than zero.
- If the assortment basis measure values are Zero/Null then value wil not be spreaded at that leaf.

## Spreading Config:

| Basis Measure Type     | Basis Measure              | SpreadingType      |
|------------------------|----------------------------|--------------------|
| BasisMeasure           | SoldBasisMeasure           | DistributeToLeaves |
| AssortmentBasisMeasure | SoldAssortmentBasisMeasure | DistributeToLeaves |

## Example:

| Version Name       | Brand     |   Price |
|--------------------|-----------|---------|
| CurrentWorkingView | Palmolive |      80 |
| CurrentWorkingView | Herbal    |      40 |

CurrentWorkingView Colgate

45

Select (Version.[Version Name]*Item.[Brand]*Item.[SKU]*

{Measure.[Price], Measure.[SoldBasisMeasure],

Measure.[SoldAssortmentBasisMeasure], Measure.[NewPrice]});

| Version Name        | Brand      | SKU         |   Pri ce |   SoldBasisMe asure |   SoldAssortmentBasis Measure | NewPr ice   |
|---------------------|------------|-------------|----------|---------------------|-------------------------------|-------------|
| CurrentWorkin gView | Palmol ive | Cinthol     |       50 |                   1 |                             0 |             |
| CurrentWorkin gView | Herbal     | Hama m      |       40 |                   1 |                             1 | 40          |
| CurrentWorkin gView | Palmol ive | Breeze      |       30 |                   3 |                             1 | 80          |
| CurrentWorkin gView | Colgate    | Attracti on |       25 |                   1 |                             1 | 45          |
| CurrentWorkin gView | Colgate    | Sensiti ve  |       20 |                   2 |                             0 |             |

## Similar example:

| Version Name        | Brand      | SKU         |   Pri ce |   SoldBasisMe asure | SoldAssortmentBasis Measure   | NewPr ice   |
|---------------------|------------|-------------|----------|---------------------|-------------------------------|-------------|
| CurrentWorkin gView | Palmol ive | Cinthol     |       50 |                   1 |                               |             |
| CurrentWorkin gView | Herbal     | Hama m      |       40 |                   1 | 1                             | 40          |
| CurrentWorkin gView | Palmol ive | Breeze      |       30 |                   3 | 1                             | 80          |
| CurrentWorkin gView | Colgate    | Attracti on |       25 |                   1 | 1                             | 45          |
| CurrentWorkin gView | Colgate    | Sensiti ve  |       20 |                   2 |                               |             |

Note: For spreading type CopyToLeaves

- Basis measure is used to flag to indicate that Target Measure is setup for Copy To Leaves spreading. Value of this measure plays no role in final assignment.

Explaination:In Copy to leaves setting up basis measure is used for flagging the target measure whether the spreading will happen or not.so if the basis measure contains value non Zero/non Null then spreading will take place and the values will be copied to the corrosponding TargetMeasure cells.whereas if the basis measure values are zero/null than it will not copy the value at corresponding TargetMeasure cells.

- If assortment basis measure is setup then only those leaves are assigned where the value for assortment basis measure is non zero/Non Null.

## Respread Basis Measure

Respreading Kicks in with the Respreading Command.

## Spreading Config:

| Basis Measure Type   | Basis Measure   | SpreadingType      |
|----------------------|-----------------|--------------------|
| BasisMeasure         | SubTotal        | DistributeToLeaves |
| RespreadBasisMeasure | SubTotal        | DistributeToLeaves |

## Example:

| Version Name       | City     | Discount   |
|--------------------|----------|------------|
| CurrentWorkingView | Bareilly | 21,708     |
| CurrentWorkingView | Pune     | 3,232      |

Let's say we have the data of Discount at city level and we want to spread it at ProductID level.spreading has been already defined for it but their are some changes made in the new data so user want to respread the data.This canbe done through executing the respreading query.

```
RESPREAD Measure.[DiscountAtProduct] USING Scope :([Customer].[Customer. [City] *[Product].[ProductId] * [Version].[Version Name].[CurrentWorkingView]);
```

Note: In the above query the value of the target measure (DiscountAtProduct) is being respreaded according to the scope defined. User must define the respread measure while defining the basis measure in UI which will be used during respreading.

All other spreading behaviour remains same, such as interaction of the Basis Measure, Null Basis Measure &amp; Assortment Basis Measure.

The result can be checked by executing the required query. For the above example it is

```
Select (Version.[Version Name]*Customer.[City]*Product.[ProductID]* {Measure.[SubTotal], Measure.[Discount], Measure.[DiscountAtProduct]});
```

## Result:

| Version Name       | City     | ProductID        |   SubTotal | Discount   |   DiscountAtProduct |
|--------------------|----------|------------------|------------|------------|---------------------|
| CurrentWorkingView | Bareilly | OFF-AP- 10002892 |        180 | 21,708     |                  18 |
| CurrentWorkingView | Bareilly | OFF-BI- 10003656 |        345 | 21,708     |                  34 |
| CurrentWorkingView | Bareilly | OFF-PA-          |         41 | 21,708     |                   4 |

|                    |          | 10002365         |         |        |        |
|--------------------|----------|------------------|---------|--------|--------|
| CurrentWorkingView | Bareilly | TEC-PH- 10002275 | 144,293 | 21,708 | 14,429 |
| CurrentWorkingView | Bareilly | FUR-TA- 10001539 | 41,939  | 21,708 | 4,194  |
| CurrentWorkingView | Bareilly | TEC-PH- 10002033 | 29,157  | 21,708 | 2,916  |
| CurrentWorkingView | Bareilly | OFF-BI- 10003910 | 1,126   | 21,708 | 113    |
| CurrentWorkingView | Pune     | TEC-PH- 10001254 | 32,270  | 3,232  | 3,227  |
| CurrentWorkingView | Pune     | OFF-PA- 10001950 | 47      | 3,232  | 5      |

Note: In above example Subtotal is the Basis Measure and the data was given at City level and then it is spreaded at ProductID level.