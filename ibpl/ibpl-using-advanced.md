## Using Advanced

## IBPL Concepts

## ABSTRACT

### This tutorial guides the

### reader through the

### steps to understand

### various advanced IBPL

### concepts.

```
2
```
## Table of Contents

Tutorial 1: Using Advanced IBPL Concepts ...................................................................................... 3

```
Purpose: .................................................................................................................................................... 3
Prerequisites ........................................................................................................................................... 4
Problem: .................................................................................................................................................... 5
Solution: .................................................................................................................................................... 6
Conclusion .............................................................................................................................................. 21
```
```
3
```
## Tutorial 1: Using Advanced IBPL Concepts

## Purpose:

```
The purpose of this tutorial is to apply the usage of following in problem-solving:
```
1. Data Definition Language ( DDL) commands to create and delete
    measures.
2. Recording Action Button (AB) Execution Details.
3. Use of IN and OUT options in AB.
4. Use of Sequencer.
5. CreateMember Function.
6. Block Scope.
7. Running rules in parallel.
8. Parameterized Procedure.
9. Parallelization of Procedure.
10. Procedure Debugging using Expand Command.
11. Using NameSet as a filter in widgets.

```
4
```
## Prerequisites

```
As part of the prerequisites to learn the Advanced IBPL concepts, you must
understand Model Library (MLib) and Config Management (CM) and their differences.
```
```
MLib and CM are platform functionalities that use a set of platform features to
provide users with the ability to package and move tenant configurations between
two tenants.
```
```
Please follow this page to understand the difference between MLib & CM and the use
cases where they should be used.
```
- To learn about MLib, please visit this link.
- To learn about CM, please visit this link.
- To learn about Config Audit, please visit this link.

```
For the participants undergoing the Advanced IBPL Workshop, we have created an
MLib package that must be imported. This package contains all the necessary models
and layouts needed to perform hands-on.
```
```
Follow the instructions to import the MLib package:
● Login to your tenant.
● Visit the Model Library workspace.
● Click on the Model Library (Public) tab.
● Search for the package AdvanceIBPL.
```
```
● Click on the Open package to import button.
● Scroll down and Click on Import All.
```
```
● On the right side of the page, you will find the import button. Click on it.
● Import Summary will pop up on your screen. Click on import.
● After a few minutes, you will see a success message on your screen. Click OK.
● Close the package, go to the Designer workspace, and refresh the page.
● All the necessary models, layouts, and workspaces have been successfully
imported.
```
```
5
```
```
● Please execute save(); in the debugging workspace.
● To start with the program, download and extract the dataset zip file that has
been shared as part of this program.
● Now, upload all the Dimension files first, followed by only the
Fact.FactPlanningMeasures.csv file. You can ignore the rest of the Fact files
for now. This will ensure that your dataset is set up correctly and ready to be
used in the program.
```
## Problem:

Calculate the values for Stock and Net Demand Measures using the below
formula:

```
Stock = Shipment - Consumption
Net Demand = Forecast - Order Qty
Requirements:
```
1. Allow the end-user to perform the below operations using an Action Button:
    ● Create two measures, Stock and Net Demand, using DDL.
    ● Use procedures to calculate the measure values and log Action Button
       execution details such as AB Name, Date & Time, and User Info.
2. Create two reports Stock&NetDemandReport and ABExecutionLogs.
    ● Stock&NetDemandReport should display the Forecast, Order Qty, Net
       Demand, Shipment, Consumption, and Stock measures against Version
       Name, Account, SKU, and Month. Select the filters appropriately.
    ● ABExecutionDetails should display the Date, Action Button Name,
       and User measures against the Version Name and Execution ID. Select
       the filters appropriately.
    ● Create a New Workspace, Page, and two views under the page. Attach
       the above-created reports to individual views.
    ● Attach the Action Button to the Stock&NetDemandReport.
3. Enable the user to filter the data in the Stock&NetDemandReport using
time-related namedsets in the dashboard.
4. Allow the user to delete a measure via an Action Button using DDL.

```
6
```
```
Hint: The EXPAND feature of the procedure can be used to check the call
made to the parameterized procedure.
Note: Develop an optimized and efficient solution as per your knowledge.
```
## Solution:

## Steps to implement:

1. The **below Dimensions** and **Attributes** should have been **imported** into the
    **tenant**.

```
7
```
2. The below **Plan, Measure Group, and Measures** should have been **imported** into
    the **tenant**.
       **Plan** : Sales Plan
       **Measure Group** : Planning Measures (Granularity: Version Name, Account,
       SKU, Month)
       **Measures** : Forecast and Order Qty

```
8
```
```
Plan : Sales Plan
Measure Group : AB Execute Record (Granularity: Version Name, Execution
ID)
Measures : AB Name, Date and User
```
3. **Upload** the below-mentioned **Dimension** files followed by the **Fact** files.

```
Dimension files:
Dimension.DimCustomer.csv
Dimension.DimProduct.csv
Dimension.DimTime.csv
```
```
Fact files:
Fact.FactPlanningMeasures.csv
```
4. The below-mentioned **Procedure** to **Calculate Stock** and **Net Demand** should
    have been **imported** into the **tenant**.

```
a. Go to the Rules tab in Designer Workspace.
b. Click on the Rule File Advance IBPL under Procedures.
c. Go to Debugging Workspace and execute the below expand command to
verify the procedure:
expand procedure StockAndNetDemand
{"Stock":"Stock","NDemand":"Net Demand"};
```
```
9
```
5. The below-mentioned **Procedure** to **log User details** for **Action Button** execution
    should have been **imported** into the tenant.

```
a. Go to the Debugging Workspace and execute the expand command to
verify the procedure:
expand procedure LogABDetails {"ExecID":"ID -
999","ABName":"Test","UserName":"Aurgho"};
```
6. **Create** a **Master Procedure** to **execute** the above procedures.

```
a. Click the + icon to add a new procedure to the file.
b. Fill in the details as shown in the image.
c. Click Save and Close.
```
```
10
```
```
d. Go to the Debugging Workspace and execute the expand command to
verify the procedure:
```
```
expand procedure Master {"Stock":"Stock","NDemand":"Net
Demand","ExecID":"ID -
999","ABName":"Test","UserName":"Aurgho"};
```
7. **Create** the below **Sequence** to **generate** a unique **Action Button** execution ID.

```
a. Go to the Sequence tab in the Designer Workspace.
b. Click the + icon to add a new sequence.
c. Fill in the details as shown in the image.
d. Click Save , and then close the window.
Note : The above sequence won’t work until you execute a save(); in the
Debugging workspace and restart the tenant.
```
```
11
```
8. **Create** the below **Action Button**.

```
a. Fill in the details in the image under the Options tab.
```
```
12
```
b. **Create** a new **literal Field Binding** under the **FieldBindings** tab.

c. **Fill** in the details as shown in the image.

```
d. Create a new literal Field Binding under the FieldBindings tab.
e. Fill in the details as shown in the image.
```
```
13
```
```
f. Create a new literal Field Binding under the FieldBindings tab.
g. Fill in the details as shown in the image.
```
```
h. Create a new Data Source under the DataSources tab.
i. Fill in the details as shown in the image.
```
```
14
```
```
j. Create a new datasource Field Binding under the FieldBindings tab.
k. Fill in the details as shown in the image.
l. Uncheck the visible and isEditable checkboxes.
```
m. **Click** the **+** icon to **add** a new **IBPL rule** under the **RuleOptions** tab.

n. **Fill** in the details as shown in the image.

o. DDL Command to **create** Stock Measure:

```
ddl{"ModelType": "Measure","Action" : "Create","Payload":
{"MeasureName":"{{Stock}}","MeasureGroupName" :
"{{MGName}}","MeasureDescription":"Measure to store the
Total Stocks","ToolTip":"","MeasureType":
"Regular","IsReportingMeasure":"false","AssociationMeasure":
"false","ApplyConversion":"false","ConversionFormula":"","Me
asureFormula": "","PickListName":
"","TenantId":"","DataType"
:"number","AggregateFunction":"Sum","FormatString":"#,##0;(#
,##0)","IsEditable":false}};
```
```
15
```
p. **Click** the **+** icon to **add** a new **IBPL rule** under the **RuleOptions** tab.

q. **Fill** in the details as shown in the image.

r. DDL Command to **create** Net Demand Measure:

```
ddl{"ModelType": "Measure","Action" : "Create","Payload":
{"MeasureName":"{{NDemand}}","MeasureGroupName" :
"{{MGName}}","MeasureDescription":"Measure to store the Net
Demand","ToolTip":"","MeasureType":
"Regular","IsReportingMeasure":"false","AssociationMeasure":
"false","ApplyConversion":"false","ConversionFormula":"","Me
asureFormula": "","PickListName":
"","TenantId":"","DataType"
:"number","AggregateFunction":"Sum","FormatString":"#,##0;(#
,##0)","IsEditable":false}};
```
```
16
```
```
s. Click the + icon to add a new IBPL rule under the RuleOptions tab.
t. Fill in the details as shown in the image.
u. IBPL Command to fetch current user details:
select CurrentUser().Name;
```
```
v. Click the + icon to add a new IBPL rule under the RuleOptions tab.
w. Fill in the details as shown in the image.
x. IBPL Command to execute master procedure:
exec procedure Master
{"Stock":"{{Stock}}","NDemand":"{{NDemand}}","ExecID":"{{Unique
ID}}","ABName":"CalculateStockandNetDemand","UserName":"{{User}
}"};
y. Save the Action Button.
```
9. **Create** a **pivot** (Stock&NetDemandReport) to display the **Forecast** and **Order Qty**
    measures against Version Name, Account, SKU, and Month. **Select** the filters
    appropriately.

```
17
```
10. **Add** the **Time** Dimension to the **filter** of the above report.
11. **Click** on the settings icon on the **Time** Dimension filter.
12. **Select all** the **NamedSets** required to be **part** of the filter.

```
18
```
13. **Create** another **pivot** (ABExecutionDetails) to **display** the Date, AB Name, and
    User measures against the Version Name and Execution ID. **Select** the filters
    appropriately.
14. **Create** a **New Workspace** , Page, and two views under the page. **Attach** the
    above-created **reports** to the individual views.
15. **Attach** the **Action Button** created in the above steps to the report
    Stock&NetDemandReport.
16. **Execute** the **Action Button** with details as shown below.
17. **Edit** the report Stock&NetDemandReport to add the newly created measures.
18. **Verify** the report Stock&NetDemandReport and ABExecutionDetails to **view**
    the **data** of the newly created **measures** and **Action Button** execution logs,
    respectively.

```
19
```
19. **Create** the below-mentioned **Action Button** to **enable** the user to **delete** a
    measure.

```
a. Fill in the details in the image under the Options tab.
```
```
20
```
```
b. Create a new literal Field Binding under the FieldBindings tab.
c. Fill in the details as shown in the image.
```
d. **Create** a new **literal Field Binding** under the **FieldBindings** tab.

e. **Fill** in the details as shown in the image.

f. **Click** the **+** icon to **add** a new **IBPL rule** under the **RuleOptions** tab.

g. **Fill** in the details as shown in the image.

```
21
```
```
h. DDL Command to delete a measure:
ddl{"ModelType": "Measure","Action" : "Delete","Payload":
{"MeasureName":"{{MeasureName}}","MeasureGroupName" :
"{{MGName}}" }};
i. Save the Action Button.
```
20. **Attach** the **Action Button created** in the above steps to the report
    Stock&NetDemandReport.
21. **Execute** the **Action Button** with details as shown below.
22. **Check** the Measure Group – Planning Measures to **verify** that the measure has
    been **deleted**.

## Conclusion

```
Upon completing this tutorial, you have acquired an understanding of various
advanced IBPL concepts.
```
