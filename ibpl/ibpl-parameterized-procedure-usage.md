# Parameterized IBPL Procedures Usage -


# Examples

#### Simple string input ...

#### ... to run stat forecast plugin

#### Defining Procedure

#### Executing Procedure

#### ... to run SCS

#### Defining Procedure

#### Executing Procedure

#### Using #if / #else ..

#### ... on RHS of a regular scope statement

#### Defining Procedure

#### Executing Procedure

#### ... inside regular scope statement to run different assignments

#### Defining Procedure

#### Executing Procedure

#### ... to run different scope statements

#### Defining Procedure

#### Executing Procedure

```
create parameterized procedure p_RunStatForecast_
begin procedure
exec plugin instance [Stat Fcst] for Measures {[Cleansed Sales Actuals (L2)]}
using scope (&AllSalesParts_Item * Dealer.[Dealer Forecast Group].filter(#.Name == [[[:Template:DFG]]]) *
&AllMonths * &CWV)
using arguments {([Best Fit], true), ([Persist All], true)};
end procedure;
```
#### exec procedure p_RunStatForecast_1 {"DFG" : "0/Southwest"};

## Contents

## Simple string input ...

## ... to run stat forecast plugin

#### Defining Procedure

#### Executing Procedure


##### IBPL executed for parameterized procedure P_RUNSTATFORECAST_1:

##### exec plugin instance [Stat Fcst] for Measures {[Cleansed Sales Actuals (L2)]}

#### using scope (&AllSalesParts_Item * Dealer.[Dealer Forecast Group].filter(#.Name == [0/Southwest])

#### * &AllMonths * &CWV) using arguments {([Best Fit], true), ([Persist All], true)};

```
create parameterized procedure p_RunSCS
begin procedure
exec plugin instance SCS_101 for Measures {[SCS Demand Quantity]} using scope (&CWV * Item.[Category] *
&CurrentAndNext52Weeks)
arguments {([Material Constrained], "Template:MC")
, ([Capacity Constrained], "Template:CC")
, ([Delete SCS Plan Data For Version], "True")
, ([End Time Bucket Name], &CurrentWeek.element(0).leadoffset(52).Name)
, ([Start Time Bucket Name], &CurrentWeek.element(0).Name)
, ([Inventory Plan Policy], "Template:IPP")
, ([Inventory Plan Type], "QUANTITY")
, ([Respect Max Stock Constraint In JIT Planning] , "False")};
end procedure;
```
```
exec procedure p_RunSCS {"MC" : "True", "CC" : "False", "IPP" : "QUANTITY"};
```
```
create parameterized procedure p_Test_If_
begin procedure
scope:(&CWVandS * &PastMonths);
Measure.[DBU Forecast (L10)] = {{#if Param_Check}} Measure.[Sales Actuals (L10)] * Template:Param 1
{{#else}} Measure.[Sales Actuals (L10)]
Template:/if;
end scope;
end procedure;
```
### ... to run SCS

#### Defining Procedure

#### Executing Procedure

## Using #if / #else ..

### ... on RHS of a regular scope statement

#### Defining Procedure

#### Executing Procedure


#### exec procedure p_Test_If_1 {"Param_Check" : "True", "Param_1" : 5};

##### IBPL executed for parameterized procedure P_TEST_IF_1:

```
scope:(&CWVandS * &PastMonths);
```
```
Measure.[DBU Forecast (L10)] = Measure.[Sales Actuals (L10)] * 5
```
###### ;

```
end scope;
```
```
create parameterized procedure p_Test_If_
begin procedure
scope:(&CWVandS * &PastMonths);
{{#if Param_Check}}
Measure.[DBU Forecast (L10)] = Measure.[Sales Actuals (L10)] * Template:Param 1;
{{#else}}
Measure.[DBU Forecast (L10)] = Measure.[Sales Actuals (L10)];
Template:/if
end scope;
end procedure;
```
#### exec procedure p_Test_If_2 {"Param_Check" : "True", "Param_1" : 5};

#### IBPL executed for parameterized procedure P_TEST_IF_2:

##### IBPL executed for parameterized procedure P_TEST_IF_2:

```
scope:(&CWVandS * &PastMonths);
```
```
Measure.[DBU Forecast (L10)] = Measure.[Sales Actuals (L10)] * 5;
```
```
end scope;
```
### ... inside regular scope statement to run different assignments

#### Defining Procedure

#### Executing Procedure

### ... to run different scope statements


```
create parameterized procedure p_Test_If_
begin procedure
{{#if Param_Check}}
scope:(&CWVandS * &PastMonths);
Measure.[DBU Forecast (L10)] = Measure.[Sales Actuals (L10)] * Template:Param 1;
end scope;
{{#else}}
scope:(&CWVandS * &PastMonths);
Measure.[DBU Forecast (L10)] = Measure.[Sales Actuals (L10)];
end scope;
Template:/if
end procedure;
```
#### exec procedure p_Test_If_3 {"Param_Check" : "True", "Param_1" : 5};

##### IBPL executed for parameterized procedure P_TEST_IF_3:

```
scope:(&CWVandS * &PastMonths);
Measure.[DBU Forecast (L10)] = Measure.[Sales Actuals (L10)] * 5;
end scope;
```
##### Retrieved from ‘https://platformwiki.o9solutions.com/index.php?title=Parameterized_IBPL_Procedures_Usage_-

##### _Examples&oldid=3983’

##### This page was last modified on 10 June 2020, at 07:37.

#### Defining Procedure

#### Executing Procedure


