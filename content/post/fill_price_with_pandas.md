---
date: 2022-03-18
title: How to fill price to BOM using Pandas
weight: 10
---

# Problems

I have a csv table with all the prices with the indexes.

I also have the BOM (Bill of Meterial), I want to fill the price to the BOM so
that BOM cost could be calculated.

# Average way

The average way is to loop over the BOM and find the price in the price table
one by one.

# Better Way: Using Pandas

```python
import pandas as pd
```

Create a BOM

```python
BOM = pd.DataFrame({"pn":["dd","ee","ff"], "description":["R1","C2","C3"]},index=["10R","20C","20A"])
```


This is the price list

```python
price = pd.Series([6.0,2.1,2.3,6.1],index=["10R","20C","20A","50R"])
```

One line of code to fill the price to BOM:

```python
BOM["price"] = price
```

The logic behind that is how Pandas is working, BOM["price"] will create a new
column, the real price will be picked from "price" and fill according to the
indexes which has in BOM.
