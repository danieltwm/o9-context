# IBPL Language Structure

```
Statements
Referring to User Supplied Data
Dot Notation
Expressions
Unary Arithmetic Operators
Binary Arithmetic Operators
String Concatenation
Binary Comparison Operators
Binary logical Operators
Attribute Member At(@) Operator
Notes
```
In its simplest form, IBPL statements are a command or function followed by a semi-colon.

```
save();
```
Commands and functions may have arguments and options that customize the behavior being requested.

```
save() arguments {"DoNotBlockMergedVersions": true};
```
There are no limits to what data can be entered into and managed by GraphCube. Data, even data that is
identifying (i.e. keys), can have whitespace or special characters in them. When referring to data with
white space or special characters in statements, they will need to be surrounded by either quotes or
square brackets. These are optional for data that does not have whitespace or special characters. All of
these are equivalent to each other:

```
Product.[Product Name]
Product.”Product Name”
“Product”.[Product Name]
“Product”.”Product Name”
[Product].[Product Name]
[Product].”Product Name”
```
## Contents

## Statements

## Referring to User Supplied Data


Using square brackets is the more common form when building expressions and is used throughout this
documentation.

In IBPL, a period is used to chain together objects and functions.

One example is above, where the Product Name level is referred to by [Product].[Product Name].

In most contexts, when a dimension object is followed by a period, then the next object is a level. One
counterexample is found with the Children function:

```
[Time].[Fiscal Year].find([FY2013]).children([Time].[Fiscal YQM])
```
In this case, [Time].[Fiscal YQM] refers to a hierarchy.

Technically, the datatype of [Product].[Product Name] is a member set. So if you execute the following
query in IBPL, the result will be the members of the Fiscal Year level:

```
select [Time].[Fiscal Year];
```
A member set followed by a period is then followed by a member object (or by a function that operates
on a member set). So the following are all ways to refer to the member FY2013 which is a member of the
Fiscal Year level:

```
[Time].[Fiscal Year].[FY2013]
```
```
[Time].[Fiscal Year].find([FY2013])
```
```
[Time].[Fiscal Year].filter(#.Name==[FY2013]).element(0)
```
A member object followed by a period is then followed by a level object (or by a function that operates on
a member). So the following are all ways to get a member set with the Fiscal Quarter members that are
children to FY2013:

```
[Time].[Fiscal Year].[FY2013].[Fiscal Quarter]
```
```
[Time].[Fiscal Year].[FY2013].children([Time].[Fiscal YQM])
```
```
[Time].[Fiscal Year].[FY2013].relatedMembers([Fiscal Quarter])
```
Expressions are string representations of logical or mathematical functions that will be evaluated in the
context of the statement being executed. Terms are either user entered data from either the dimension
structures or members, or from a function that has a return value. Parenthesis can be used to change the

## Dot Notation

## Expressions


order of evaluation. The following operators can be used to build expressions.

The following unary arithmetic operators are supported in IBPL. They accept a single scalar value
expression as input and return a scalar value.

```
Unary Plus Example
+5 => 5
```
```
Unary Minus Example
-(5) => -
```
The binary arithmetic operators supported in IBPL include:

```
addition (+), subtraction (-), multiplication (*), division (/), power (^).
```
They each accept two numeric values as input and return a numeric value.

```
Note Observe the use of the Coalesce() function for handling the null values correctly.
```
For more information, click the following links:

```
Addition
Subtraction
Multiplication
Division
Power
```
The Concatenation Operator is the plus sign (+). It is used to concatenate two strings.

Example: “First” + “Last”

In this Example, the two strings are combined and the return value is the single string “FirstLast”.

The binary comparison operators supported in IBPL include Equal (==), NotEqual (!=), LessThan (<),
LessThanOrEqual (<=), GreaterThan (>), and GreaterThanOrEqual (>=). They each accept two value
expressions as input and return a logical value. The inputs can be of type date, number or string.

### Unary Arithmetic Operators

### Binary Arithmetic Operators

### String Concatenation

### Binary Comparison Operators


Generally, the two inputs are of the same type, but that is not required. If one input is a string and the
other is a date or a number, then the date/number is converted to a string before the comparison is
made. In the case of a date, the conversion to string will look like “1/27/2013 12:00:00 AM”. In the case
of a number, the conversion to string may use scientific notation if the value is very large (e.g.,
1,234,567,890,000,000 will become “1.23456789E+15”).

However, if one input is a date and the other is a number, then you will get an error.

For more information, click the following links:

```
Equal
NotEqual
LessThan
LessThanOrEqual
GreatherThan
GreaterThanOrEqual
```
The binary logical operators supported in IBPL include AND (&&) OR (||) and NOT (~). While AND and
OR operators accept two logical value expressions as input and return a logical value, NOT accepts just
one logical value expression as input and returns a logical value as result.

For examples && and||operators can be used as below. This statement will compute Base Sales-Out for
the time buckets between start date and end of life dates or for new products.

```
scope:(&AllProducts * &AllAccounts * &PlanningWeeks * &CwvAndScenarios);
Measure.[Base Sales-Out] = if(Time.#.Key >= Measure.[Sales-Out Start Date] && Time.#.Key <= Measure.[EOL Date] ||
Product.#.[IsNewProduct] )
then Measure.[Sales-Out Rate] else 0.0;
```
For more information, click the following links:

```
Logical And
Logical Or
Not
```
The At Operator uses the at symbol (@). It is used in a measure formula to refer to the value of a measure
for a different member of one of the dimensions. The return value is an AttributeMember.

Examples:

```
Measure.[Segment Forecast Detail Gross Revenue LY] = Measure.[Segment Forecast Detail Gross
Revenue]@([Time].#.LeadOffset(-12))
```
```
Measure.[Segment Forecast Detail Gross Revenue] = Measure.[Segment Forecast Detail Gross Revenue
Actual]@(Time.Week.filter(#.Key == &CurrentWeek.element(0).Key).element(0)) * Measure.[Growth %];
```
### Binary logical Operators

### Attribute Member At(@) Operator


This example finds the value of “Segment Forecast Detail Gross Revenue” for the member twelve months
earlier, and uses that as the value for “Segment Forecast Detail Gross Revenue LY”.

```
Keywords, functions and other built in language structures are case-insensitive. Data, meaning
ANYTHING entered by a user or external process, is case-sensitive.
As a data query and manipulation language, IBPL will look similar to other query languages, and
most of the concepts can be mapped from one to the other. IBPL has grown organically, an as such
some languages structures and syntax are available in limited use cases where the feature is
intended.
```
Retrieved from ‘https://platformwiki.o9solutions.com/index.php?title=IBPL_Language_Structure&oldid=18181’

**This page was last modified on 5 October 2021, at 14:47.**

## Notes


