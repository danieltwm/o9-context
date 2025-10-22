Creating, Updating, and Deleting Member Data

## Creating, Updating,

## and Deleting

## Members

#### o)Academy

## ABSTRACT

### Understand how to

### create new members,

### and update and delete

### existing ones. This also

### covers the types of

### deletion.

_o9 Solt1tiorts, Joe. Coofkler1t1£Jt & Prop,-iet.Jty. Your u£e of these m.2teri.9/s is governed by the terms of your' wr'itten £1greenle11t with o9 Sofutio,1s. All w1.1uthorized use and tepmduction or distribt1tio,1 is ptchibited._


## Table of Contents

Tutorial 4: Creating, Updating, and Deleting Member Data ...................................................... 3

```
Purpose: .................................................................................................................................................... 3
Prerequisites ........................................................................................................................................... 3
Use Case: ................................................................................................................................................. 3
Solution: .................................................................................................................................................... 4
```
```
2
```

##### •

##### •

##### •

##### •


## Tutorial 4: Creatingt Updatingt and Deleting

## Member Data

## Purpose:

The purpose of this tutorial is to understand Creating, Updating, and Deleting

```
Member Data.
```
## Prerequisites

Tutorial 1 from Module 1 should be completed.

Dimension: Product
Level Attribute: All Products, Classification, Product Category and SKU

Level Attribute Property: SKU Image

## Use Case:

```
Create a new SKU "Red Bull" under a new Product Category "Drinks" (Add this
```
```
new classification) and Classification "D" (Add this new classification).
```
```
Realign the SKU "Red Bull" with the category "Refreshment".
```
```
Update the attribute property (SKU Image) for the SKU "Red Bull".
```
```
Then finally do a soft delete for the Category "Refreshment" and its children
```
```
using cascade so that all the children under the category are marked as
```
```
lnActive first and then are finally deleted by the purge command.
```
```
3
```

```
createmember
```
```
updatemember
```
```
updatemember
```
```
createmember([Product].[All Products]={,"All
Products"},[Product].[Classification]={,"D"},[Product].[Product
Category]={,"Drinks"},[Product].[SKU]={,"Red Bull"});
```
```
updatemember([Product].[All Products]={,"All
Products"},[Product].[Classification]={,"D"},[Product].[SKU]={,
" Red Bull "}, [Product].[Product Category]={, "Refreshment"});
```
```
updatemember([Product].[All Products]={,"All
Products"},[Product].[Classification]={,"D"},[Product].[SKU]={,
"Red Bull"}, [Product].[Product Category]={,
"Refreshment"},[Product].[SKU
Image]={"https://images.app.goo.gl/a4F43CyWM5qX7QA49",});
```
```
deletemember([Product].[Product
Category]={,[Refreshment]})cascade;
```
```
purge members();
```

## Solution:

### Steps to Implement:

1. Write a
    below.

```
command to create a new SKU "Red Bull" as shown
```
```
Note: Category "Drinks" and classification "D" will also get created along with
SKU "Red Bull"
```
2. Update the category of SKU "Red Bull" from "Drinks" to "Refreshment" using the
    command.
3. Update the attribute property (SKU Image) for the SKU "Red Bull" using the
    command.
4. Mark the category "Refreshment" and its members inActive using cascade with
    deletemember command. This is a soft delete.
5. Delete all the members marked as inActive permanently from the system. This is
    a hard delete.

```
4
```

