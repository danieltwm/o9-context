# Syntax for Parameterized IBPL Procedures

## Create Procedure

## Execute Procedure

## Limitations

```
Use of “parameterized” keyword in create procedure syntax distinguishes the two types (Parameterized v/s Basic IBPL
procedure).
Parameterized procedures can optionally specify parameter schema json; but it is not allowed for basic procedures.
Body of the procedure must be valid IBPL for basic procedures. It can contain Mustache tags for parameterized
procedures.
Body of the procedure of parameterized procedure must start with “begin procedure” and end with “end procedure;”.
For basic procedures it can omit the procedure in the “begin procedure” and “end procedure” (for backward
compatibility).
Parameter schema for procedure creation and arguments for procedure execution are specified as Json. If the Json
syntax conflicts with IBPL syntax (for example, square brackets for arrays; single quote for properties, etc), enclose the
Json string within “begin jsonblock” and “end jsonblock” tags.
```
```
Parameterized procedure execution must specify argument json; It’s not relevant for basic procedures.
```
```
Currently any default value for the parameters in the parameterized IBPL procedures cannot be specified.
This is yet to be supported with online model changes (which means Graphcube server save and restart are required).
Update members from parameterized procedure is not supported.
```
```
Retrieved from ‘https://platformwiki.o9solutions.com/index.php?
title=Syntax_for_Parameterized_IBPL_Procedures&oldid=3950’
```
```
This page was last modified on 10 June 2020, at 07:03.
```
## Contents

## Create Procedure

## Execute Procedure

## Limitations

10/15/25, 11:00 AM Syntax for Parameterized IBPL Procedures - o9 Solutions Wiki

https://platformwiki.o9solutions.com/index.php/Syntax_for_Parameterized_IBPL_Procedures 1 / 1


