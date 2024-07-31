---
title: "PythonCheatSheet"
date: 2023-08-21T20:07:55-07:00
draft: false
tags: [
    "Python"
]
categories: [
    "Python"
]

---


# Python 3 Cheat Sheet
##### Base Types
- int  
`783 0 -192`\
`# binary 0b010`\
`# octal 0o642`\
`# hexa 0xF3`
- float \
`9.23 0.0 -1.7e-6`
- bool \
`True`
`False`
- str \
`"One Two" ` \
`# New Line \n`\
`# Escaped Tab \t` \
`# Multiline string """X Y Z`\
`1 2 3"""`
- bytes \
`b"toto\xfe\755"`

##### Container Types
**Ordered Sequences**
- **list** \
`[1,2,9] ["x",11,8.9] ["mot"]`
- **tuple**  
*non modifiable values (immutables)* \
`(1,5,9) 11,"y",7.4  ("mot",)`
- **str bytes** 
*ordered sequences of chars / bytes* \
`b"toto"` \
##### 
**Key Containers**
*key containers, no a priori order, fast key access, each key is unique* 
- **dictionary** *key value associations* \
`dict{"key":"value"} dict{a=3,b=4,k="v"}`
- **collection** *keys=hashable values (base types, immutables...)* \
`set {"key1","key2"} {1,9,3,0}`

##### Identifiers 
*For variables, function, modules, classes, names, etc...* \
`a..zA..Z_` followed by `a..zA..Z_0...9`
- Diacritics allowed but should be avoided 
- Language keywords forbidden 
- lower/UPPER case discrimination 

##### Conversions
- `int("15")` → `15`
- `int("3f",16)` → `63` *can specify integer number base in 2nd parameter*
- `int(15.56)` → `15` *truncate decimal*
- `float("-11.24e8")` → `-1124000000.0`  
- `round(15.56,1)` → `15.6` *rounding to 1 decimal (0 decimal →  integer number)*
- `bool(x)` ***False** for null **x**, empty container **x**, **None** or **False x;True** for other **x*** 
- `str(x)` → `"..."` *representation string of **x** for display*
- `chr(64))` → `@`  *code <-> char*
- `repr(x)` → `"..."` *literal representation string of **x***
- `bytes([72,9,64])` → `b'H\t@`
- `list("abc")` → `['a','b','c']`  
- `dict([(3,"three"),(1,"one")])` → `{1:'one',3:'three'}`
- `set(["one","two"])` → `f{'one','two'}`
- *separator **str** and sequence of **str** → assembled **str*** `':'.join(['toto','12','pswd'])` → `'toto:12:pswd'`
- ***str** splitted on whitespaces → **list** of **str***
`"words with spaces".split()` → `['words','with','spaces']`
- ***str** splitted on separator **str** → **list** of **str***
`"1,4,8,2".split(",")` → `['1','4','8','2']`
- *sequence of one type → **list** of another type (via list comprehension)*
`[int(x) for x in ('1','29','-3')]` → `[1,29,-3]`

##### Variables Assignment
*assignment ⇔ binding of a name with a value*
1. evaluation of right side expression value 
2. assignment in order with left side names
- `x=1.2+8+sin(y)`
- `a=b=c=0` *assignment to same value*
- `y, z, r=9.2,-7.6,0 `*multiple assignments*
- `a, b=b, a` *values swap*
- `a, *b=seq`
- `*a, b=seq` *unpacking of sequences in item and list*
- `x=None` *« undefined » constant value*
- `del x` *remove name **x***
- `x+=3` *increment ⇔ x=x+3*
- `x-=2` *decrease ⇔ x=x-3*
Operator | Example | Same As
--- | --- | ---
+= | x+=3 | x=x+3
-= | x-=2 | x=x-2
*= | x*=3 | x=x*3
/= | x/=3 | x=x/3
%= | x%=3 | x=x%3

##### Sequence Containers Indexing

Negative Index | Positive Index | Positive Slice | Negative Slice
--- | --- | --- | ---
-5 | 0 | 0 | -5
-4 | 1 | 1 | -4
-3 | 2 | 2 | -3
-2 | 3 | 3 | -2 
-1 | 4 | 4 | -1

**`lst = [10, 20, 30, 40, 50]`**\
*Length Count:  **len(lst)→5** index from 0 (here from 0 to 4)*\
*Individual access to **items** via `lst[index]`*
- `lst[0]→10` *first item*
- `lst[-1]→50` *last item*
- `lst[1]→20` *second item*
- `lst[-2]→40` *second to last item*

*Access to **sub-sequences** via lst*
`[start slice: end slice: step]`
- `lst[:-1]→[10,20,30,40]`
- `lst[1:-1]→[20,30,40]`
- `lst[::2]→[10,30,50]`
- `lst[::-1]→[50,40,30,20,10]`
- `lst[::-2]→[50,30,10]`
- `lst[:]→[10,20,30,40,50]` *shallow copy of sequence*
- `lst[1:3]→[20,30]`
- `lst[-3:-1]→[30,40]`
- `lst[:3]→[10,20,30]`
- `lst[3:]→[40,50]`\
*Missing slice indication → from start / up to end.*\
*On mutable sequences (**list**), remove with **del** `lst[3:5]` and modify with assignment `lst[1:4]=[15,25]`*

##### Boolean Logic
Comparisons | Operator
--- | ---
Less Than | <
Greater Than | >
Less Than or Equal to | <=, ≤
Greater Than or Equal To | >=, ≥
Equal | ==
Not Equal | !=, ≠
Assigning | =
AND | and
OR | or
Logical Not | not a
True | True
False | False

##### Modules/Names Imports
- `from monmod import nom1,nom2 as fct` *renaming with **as***
- `import monmod` *access via **monmod.nom1***

##### Conditional Statements
```
if logical condition:
    statemnet block
```
- *Can go with several elif, elif... and only one final else. Only the block of first true condition is executed.*
```
if age<=18:
    state="Kid"
elif age>65:
    state="Retired"
else:
    state="Active"
```

##### Conditional Loop Statement 
*statement block executed as long as condition is true*
```
while logical condition: 
    statement block
```
**Loop Control**
- `break` *immediate exit*
- `continue` *next iteration*
- `else` *normal loop exit*

##### Iterative Loop Statement 
*statement block executed for each item of a container or iterator*
```
for var in sequence: 
    statement block
```
```
lst = [11,18,9,12,23,4,17]
lost = []
for idx in range(len(lst)):
    val = lst[idx]
    if val > 15:
        lost.append(val)
        lst[idx] = 15
print("modif:",lst,"-lost:",lost)
```


##### Math
`from math import sin, pi, cos, sqrt, log, ceil, floor`
Operators | Meaning
--- | ---
`+` | Addition
`-` | Subtraction
`*` | Multiplication 
`/` | Division
`//` | Floor Division
`%` | Modulus
`**` | Exponentiation
**Modules** 
| math
| statistics
| random 
| decimal 
| fractions 
| numpy 
| etc. 


##### Container Options
- `len(c)` *items count*
- `min(c)` `max(c)` `sum(c)`
- `sorted(c)` *list sorted copy*
- `val in c` *boolean, membership operator in (absence not in)*
- `enumerate(c)` *iterator on (index, value)*
- `zip(c1,c2)` *iterator on tuples containing c, items at same index*
- `all(c)` *True if all c items evaluated to true, else False*
- `any(c)` *True if at least one item of c evaluated to true, else False*
*Specific to ordered sequence containers (list, typles, strings, bytes)*
- `reversed(c)` *inversed iterator*
- `c.index(val)`
- `c.count(val)` 
`import copy`
- `copy.copy(c)` *shallow copy of container*
- `copy.deepcopy(c)` *deep copy of container*

##### Operations on Lists
- `lst.append(val)`
- `lst.extend(seq)` *add sequence of items at end*
- `lst.insert(idx,val)` 
- `lst.remove(val)`
- `lst.pop(idx)` *remove and return item at index (default last)*
- `lst.sort()`
- `lst.reverse()`

##### References
[Python Cheat Sheet](https://perso.limsi.fr/pointal/_media/python:cours:mementopython3-english.pdf)




