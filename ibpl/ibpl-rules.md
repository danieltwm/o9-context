# IBPL Rules

GraphCube uses sever kinds of rules to automatically manage data within itself. For example a rule can
be created that derives a measure value whenever a related measure is modified. Each type of rule
defines a different kind of scope for the rule to work with, each with expressions that modify measures.

```
Active Rules
Recurrence Rules
Ordered Recurrence Rules
Block Scope Rules
Cartesian Scope Rules
Safety Limits
Spread Scope
Block Scope
Evaluate Member Scope
Evaluate Member Rule Performance
```
GraphCube is the (in-memory) storage & computational engine for o9 Business Planner. At startup,
GraphCube reads a set of rules defined in one or more rule files. These rules define how any measure
should be calculated by the GraphCube engine. At runtime, as users edit measures, GraphCube engine
computes the entire chain of dependent computations. Hence, these are called “Active rules”, since these
rules are evaluated immediately in response to edits.

For more information on examples, click **Active Rules**.

```
Note You can click Creating Active Ruleshere (https://o9ers-o9academy.talentlms.com/catalog/info/id:206) , if required. and take up a course on
```
Recurrence rules are used to model time-series based computations wherein the computation in a
particular time bucket depends on the result of the computation of the previous time bucket, as in this
example:

```
EndingOnHand(t) = BeginningOnHand(t) + Supply(t) - Demand(t)
BeginningOnHand(t) = EndingOnHand(t-1)
```
For more information on examples, click **Recurrence Rules - Examples**.

## Contents

## Active Rules

## Recurrence Rules


```
Note You can click Implement Recurrence & evaluatemember Scopehere (https://o9ers-o9academy.talentlms.com/catalog/info/id:210) , if required. and take up a course on
```
Execution of rules within an ordered recurrence is similar to rules within a recurrence rule, except that
the rules are evaluated in the exact order specified in the rule file, whereas the rules in a recurrence scope
are executed in dependency order.

For better performance, the scope expression can be prefixed with a block keyword when the following
conditions hold:

1. All LHS measures must belong to the same measure group
2. When the block scope appears in an active rule file,

```
None of the RHS measures can be of finer grain
```
```
None of the RHS measures can have coordinate expressions, as in Measure.X =
Measure.Y@(Time.@.LeadOffset(-1)), the measure Y on the RHS has a coordinate expression
@(Time.@.LeadOffset(-1))
```
3. When the block scope is executed on-demand (when it is residing within a procedure, or when it is
invoked from an action button), the restrictions 2.a and 2.b do not apply.
4. A Spread scope cannot be used along with the block scope.

The cartesian Scope creates or updates the cartesian (i.e., cross-product) set of intersections as specified
by the scope. Therefore, it is used to create new intersections and assign values to those. However, a user
needs to be careful with cartesian because that can bring down any database by using cartesian cross
products of large size. So the user needs to be sure of the size of the scope that is being used. If a user
provides a large scope, it will bloat the model. The size of the scope dramatically increases with the
number of dimensions in the measure granularity.

```
Note You can click Implement Cartesian Scopehere (https://o9ers-o9academy.talentlms.com/catalog/info/id:208) , if required. and take up a course on
```
**Safety1** : The cartesian part is supported for the following two types of assignments:

```
Assign a constant value
Assign a value based on other measures without any leadoffset/ancestor etc.
```
## Ordered Recurrence Rules

## Block Scope Rules

## Cartesian Scope Rules

### Safety Limits


For other kinds of assignments that involve multiple measure groups and other complications, the
cartesian keyword is ignored and it has no effect. The LS does write a warning about this in the log.

**Safety2** : The set of intersections upserted by cartesian can be constrained by an assortment measure
(that can be defined at fewer dimensions and usage of this is recommended, whenever is possible).

**Syntax**

Cartesian scope: (granularity at lowest level);Measure.[measureName]=0; end scope;

This creates a cross product of all defined members present in the cartesian scope statement and assigns
the corresponding measure to 0.

**Syntax** : spread scope:(Multiple Higher Level Intersections);

```
Measure.[Some Measure]=Measure.[Some Other Measure];
end scope;
```
```
The keyword spread is required before the scope statement
RHS can be a constant or any measure function
RHS value will spread to each higher level intersection
Scope statement with spreading assignment are also supported in active rules
```
```
Note You can click here and take up a course on Implement Spread Scope , if required.
```
**Spreading Assignment**

```
spread scope:(Product.[Product].[P1] * Time.[Month].Filter(#.Name in {“Jan”, “Feb”}));
Measure.[M1]=100;
end scope;
is equivalent to
update Measure.[M1]@(Product.[Product].[P1], Time.[Month].[Jan])=100;
update Measure.[M1]@(Product.[Product].[P1], Time.[Month].[Feb])=100;
```
All other functionality remains the same.

For more information on examples, click **Spread Scope - Examples**.

A block scope statement has the same syntax as an IBPL rule with scope statement, the keyword block as
an extra prefix is the only difference with respect to syntax. A block scope statement, like any IBPL rule,
can be invoked on-demand (from IBPL clients), from within an IBPL procedure, or as an active rule.

The following constraints apply when a block scope statement is an active rule:

```
All the LHS measures must belong to the same measure group
All RHS measures must have the same grain as the LHS measure group
No skew coordinates allowed, such as Measure.X@(Time.#.LeadOffset(..))
```
## Spread Scope

## Block Scope


```
No references to scalar expressions involving dimension properties etc allowed, such as Time.#.(..),
```
When a block statement is executed on-demand, or from within a procedure, the only constraint is that
all the LHS measures should belong to the same measure group.

The rule will only compute for the LHS measure values when RHS is non-null and does not require
cartesian intersections in advance. Evaluate member will work in both cases when:

```
RHS measure can be fully fetched at the LHS measure granularity
LHS measure has one extra dimension. RHS computation must use the property from the extra
dimension in this case.
```
IBPL syntax to compute above rules effectively, which can be written as:

```
evaluatemember scope: else if
(&AllSkus * &AllPromos * &CurrentAndFutureWeeks * &CurrentWorkingView); Measure.[Initiative Active] = If (Time.#.Key >=
Measure.[Initiative
Start Date] &&
Time.#.Key <= Measure.[Initiative End Date]) then 1 else if (Measure.[Initiative Active] > 0) then 0;
end scope;
```
```
Note You can click Implement Recurrence & evaluatemember Scopehere (https://o9ers-o9academy.talentlms.com/catalog/info/id:210) , if required. and take up a course on
```
**Limitations**

```
Computation must use at least one member property
If the computation is happening when RHS measure have one missing dimension (as compared to
LHS measure) then all properties in the RHS computation from that missing dimension. Otherwise, it
will result into cartesian intersections.
LHS measure must be computed at the lowest level of the dimensionality i.e. it should be a leaf level
computation.
```
It is seen that certain evaluate member scopes are triggered as part of the incremental plan, which are
redundant (No. of executions are 0). Hence, it takes a longer time to execute the query. This
enhancement optimizes the performance of such rules.

**Functionality Description**

The optimization is applicable if the following conditions are satisfied (for the rule inside the
**evaluatemember** scope).

```
The RHS should be a If statement (without an else clause).
The condition clause has an equality condition.
In the condition clause, LHS should be a Name or Key property.
In the condition clause, RHS should be a Measure or a literal.
```
## Evaluate Member Scope

### Evaluate Member Rule Performance


```
Series Attribute = Extra Attribute of LHS Measure
```
**Example**

```
EvaluateMember scope:(&CwvAndScenarios*PurchaseOrder.[PO Line] * Item.[Item]*Location.[Location]*Supplier.
[Supplier]*Response.[Response Line]*Time.[Day]);
Measure.[Intermediate Future Production] = if(Time.#.Key == Measure.[Response Production Date]) then Measure.[Future
Production];
end scope;
```
The tables below displays the comparison between the query execution time for createmember command
and fact file upload.

**Table1:** Query Execution Time for Createmember Command

```
Sl No.
```
```
Time Taken (ms)
% improvement
Before After
1 136245 117877 13%
2 29839 21707 27%
3 131760 121741 8%
4 30477 20782 32%
5 29491 23980 19%
```
**Table2:** Query Execution Time for Fact File Upload

```
Sl No.
```
```
Time Taken (s)
Improvement
Before After
1 11072 122 90x
```
Retrieved from ‘https://platformwiki.o9solutions.com/index.php?title=IBPL_Rules&oldid=18803’

**This page was last modified on 25 October 2021, at 19:35.**


