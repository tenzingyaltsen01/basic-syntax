---
layout: default
title: Python Data Assessment Syntax Briefing
---

Yes. For a **Python/data assessment**, I’d focus much more on being fluent with a compact set of syntax than on algorithms.

Think of this as the “I must be able to write this without Claude” sheet.

## 1. Core Python

```python
# variables
x = 5
name = "abc"
flag = True

# arithmetic
a + b
a - b
a * b
a / b       # float division
a // b      # integer division
a % b       # remainder
a ** b      # power

# comparisons
x == 5
x != 5
x > 5
x >= 5
x < 5
x <= 5

# boolean logic
x > 0 and x < 10
x < 0 or x > 10
not flag
```

Conditionals:

```python
if x > 0:
    result = "positive"
elif x == 0:
    result = "zero"
else:
    result = "negative"
```

Loops:

```python
for x in xs:
    print(x)

for i in range(10):
    print(i)

for i, x in enumerate(xs):
    print(i, x)

for a, b in zip(xs, ys):
    print(a, b)

while x < 10:
    x += 1
```

Functions:

```python
def mean(xs):
    return sum(xs) / len(xs)

def f(x, y=1):
    return x + y
```

---

# 2. Lists

```python
xs = [3, 1, 5, 2]

len(xs)
sum(xs)
min(xs)
max(xs)

xs[0]
xs[-1]

xs[1:3]
xs[:3]
xs[2:]

xs.append(10)
xs.extend([11, 12])

xs.sort()             # modifies xs
sorted(xs)            # returns new list
sorted(xs, reverse=True)
```

List comprehensions are very useful:

```python
[x**2 for x in xs]

[x for x in xs if x > 0]

[x**2 if x > 0 else 0 for x in xs]
```

---

# 3. Dictionaries

Extremely common for counting/grouping manually.

```python
d = {"a": 1, "b": 2}

d["a"]

d.get("c", 0)

d["c"] = 3

for key in d:
    print(key)

for key, value in d.items():
    print(key, value)
```

Frequency counting:

```python
counts = {}

for x in xs:
    counts[x] = counts.get(x, 0) + 1
```

Or:

```python
from collections import Counter

counts = Counter(xs)
```

---

# 4. Sets

Useful for unique values / membership / intersections.

```python
s = set(xs)

x in s

s.add(5)

a | b       # union
a & b       # intersection
a - b       # difference
```

Unique values:

```python
len(set(xs))
```

---

# 5. Strings

```python
s = "hello world"

s.lower()
s.upper()
s.strip()

s.split()
s.split(",")

",".join(["a", "b", "c"])

s.startswith("hello")
s.endswith("world")

"a" in s
```

---

# 6. Sorting with custom keys

Very useful.

```python
pairs = [("a", 3), ("b", 1), ("c", 2)]

sorted(pairs, key=lambda x: x[1])
```

Descending:

```python
sorted(pairs, key=lambda x: x[1], reverse=True)
```

---

# 7. NumPy basics

```python
import numpy as np
```

Create arrays:

```python
x = np.array([1, 2, 3, 4])

np.arange(10)
np.arange(0, 10, 2)

np.linspace(0, 1, 100)

np.zeros(10)
np.ones(10)
```

Basic stats:

```python
np.mean(x)
np.median(x)
np.std(x)
np.var(x)

np.min(x)
np.max(x)

np.sum(x)
np.cumsum(x)

np.percentile(x, 25)
np.percentile(x, 50)
np.percentile(x, 75)
```

Important: NumPy default variance/std uses population denominator \(n\).

```python
np.std(x, ddof=1)
np.var(x, ddof=1)
```

That gives sample standard deviation / variance with \(n-1\).

---

# 8. NumPy vectorization

Prefer:

```python
x * 2
x + 5
x ** 2

np.log(x)
np.exp(x)
np.sqrt(x)
np.abs(x)
```

over writing loops.

Boolean masks:

```python
x > 2

x[x > 2]

x[(x > 2) & (x < 10)]
```

Important NumPy/Pandas rule:

```python
# WRONG
(x > 2) and (x < 10)

# RIGHT
(x > 2) & (x < 10)
```

Similarly:

```python
|    # elementwise OR
~    # elementwise NOT
```

---

# 9. Random numbers / simulation

Potentially very relevant for quant work.

```python
rng = np.random.default_rng(42)
```

Then:

```python
rng.random()
rng.random(1000)

rng.integers(1, 7, size=1000)      # 1 through 6
rng.choice([1, 2, 3], size=1000)

rng.normal(0, 1, size=1000)

rng.binomial(n=10, p=0.5, size=1000)
```

Simulation example:

```python
rolls = rng.integers(1, 7, size=100000)

prob = np.mean(rolls >= 5)
```

This works because booleans act like 0/1.

---

# 10. Pandas: loading data

This is probably the highest-priority section.

```python
import pandas as pd

df = pd.read_csv("data.csv")
```

Inspect:

```python
df.head()
df.tail()

df.shape
df.columns
df.dtypes

df.info()
df.describe()
```

Useful:

```python
df["column"].describe()

df["column"].value_counts()

df["column"].unique()
df["column"].nunique()
```

---

# 11. Selecting columns

One column:

```python
df["price"]
```

Multiple:

```python
df[["price", "volume"]]
```

Access by row/column labels:

```python
df.loc[0, "price"]
```

Positionally:

```python
df.iloc[0, 2]
```

Rows 0–9:

```python
df.iloc[:10]
```

---

# 12. Filtering rows

Extremely important.

```python
df[df["price"] > 100]
```

Multiple conditions:

```python
df[(df["price"] > 100) & (df["volume"] > 1000)]
```

OR:

```python
df[(df["price"] > 100) | (df["volume"] > 1000)]
```

Membership:

```python
df[df["symbol"].isin(["AAPL", "MSFT"])]
```

Not in:

```python
df[~df["symbol"].isin(["AAPL", "MSFT"])]
```

Filter with `.loc`:

```python
df.loc[df["price"] > 100, ["price", "volume"]]
```

---

# 13. Creating columns

```python
df["return"] = df["price"] / df["price_prev"] - 1
```

Other examples:

```python
df["spread"] = df["ask"] - df["bid"]

df["mid"] = (df["bid"] + df["ask"]) / 2

df["log_price"] = np.log(df["price"])

df["positive"] = df["return"] > 0
```

Conditional:

```python
df["signal"] = np.where(df["return"] > 0, 1, -1)
```

Multiple conditions:

```python
conditions = [
    df["x"] < 0,
    df["x"] < 10
]

choices = [
    "low",
    "medium"
]

df["category"] = np.select(
    conditions,
    choices,
    default="high"
)
```

---

# 14. Sorting

```python
df.sort_values("return")
```

Descending:

```python
df.sort_values("return", ascending=False)
```

Multiple columns:

```python
df.sort_values(
    ["symbol", "timestamp"],
    ascending=[True, True]
)
```

---

# 15. Groupby

This is one of the biggest things to know.

Mean by group:

```python
df.groupby("symbol")["return"].mean()
```

Multiple stats:

```python
df.groupby("symbol")["return"].agg(["mean", "std", "count"])
```

Multiple columns:

```python
df.groupby("symbol")[["return", "volume"]].mean()
```

Multiple grouping variables:

```python
df.groupby(["symbol", "day"])["return"].mean()
```

Turn grouped result back into DataFrame:

```python
result = (
    df.groupby("symbol")["return"]
      .mean()
      .reset_index()
)
```

Custom aggregation:

```python
df.groupby("symbol").agg(
    mean_return=("return", "mean"),
    volatility=("return", "std"),
    n=("return", "count")
)
```

This syntax is worth knowing.

---

# 16. `groupby().transform()`

Very useful when you need group statistics but want to keep original rows.

Example:

```python
df["group_mean"] = (
    df.groupby("symbol")["return"].transform("mean")
)
```

Normalize within groups:

```python
df["demeaned"] = (
    df["return"]
    - df.groupby("symbol")["return"].transform("mean")
)
```

---

# 17. Missing values

Inspect:

```python
df.isna()

df.isna().sum()
```

Drop rows:

```python
df.dropna()
```

Only require particular columns:

```python
df.dropna(subset=["price"])
```

Fill:

```python
df["price"] = df["price"].fillna(0)
```

Forward fill:

```python
df["price"] = df["price"].ffill()
```

Backward fill:

```python
df["price"] = df["price"].bfill()
```

Never casually replace missing financial data with zero unless it makes conceptual sense.

---

# 18. Duplicates

```python
df.duplicated()

df.duplicated().sum()

df.drop_duplicates()
```

Specific columns:

```python
df.drop_duplicates(subset=["timestamp", "symbol"])
```

---

# 19. Merge / join

Could easily appear.

```python
pd.merge(
    df1,
    df2,
    on="symbol",
    how="inner"
)
```

Other joins:

```python
how="left"
how="right"
how="outer"
```

Different column names:

```python
pd.merge(
    df1,
    df2,
    left_on="ticker",
    right_on="symbol"
)
```

---

# 20. Concatenating data

Stack datasets vertically:

```python
pd.concat([df1, df2])
```

Reset index:

```python
pd.concat([df1, df2], ignore_index=True)
```

---

# 21. Dates and times

Potentially important if the dataset has trading data.

```python
df["timestamp"] = pd.to_datetime(df["timestamp"])
```

Then:

```python
df["timestamp"].dt.date
df["timestamp"].dt.hour
df["timestamp"].dt.minute
df["timestamp"].dt.day
df["timestamp"].dt.dayofweek
```

Sort chronologically:

```python
df = df.sort_values("timestamp")
```

Difference:

```python
df["time_diff"] = df["timestamp"].diff()
```

---

# 22. `shift()`

Very important for time series.

Previous value:

```python
df["prev_price"] = df["price"].shift(1)
```

Return:

```python
df["return"] = df["price"] / df["price"].shift(1) - 1
```

Within each stock:

```python
df["prev_price"] = (
    df.groupby("symbol")["price"].shift(1)
)
```

This is a classic assessment pattern.

---

# 23. `diff()`

```python
df["price_change"] = df["price"].diff()
```

Grouped:

```python
df["price_change"] = (
    df.groupby("symbol")["price"].diff()
)
```

---

# 24. Rolling windows

Could appear in market/data analysis.

```python
df["rolling_mean"] = (
    df["return"].rolling(20).mean()
)
```

Rolling volatility:

```python
df["rolling_vol"] = (
    df["return"].rolling(20).std()
)
```

Within groups:

```python
df["rolling_mean"] = (
    df.groupby("symbol")["return"]
      .transform(lambda x: x.rolling(20).mean())
)
```

---

# 25. Correlation and covariance

```python
df["x"].corr(df["y"])

df["x"].cov(df["y"])

df.corr(numeric_only=True)
```

NumPy:

```python
np.corrcoef(x, y)
```

Remember conceptually:

\[
\rho_{XY}
=
\frac{\operatorname{Cov}(X,Y)}
{\sigma_X\sigma_Y}
\]

Correlation near zero does **not** imply no relationship.

---

# 26. Mean, variance, standard deviation

Pandas:

```python
df["x"].mean()
df["x"].median()

df["x"].var()
df["x"].std()
```

Pandas uses sample variance/std by default, i.e. denominator \(n-1\).

Quantiles:

```python
df["x"].quantile(0.25)
df["x"].quantile(0.5)
df["x"].quantile(0.95)
```

---

# 27. Conditional averages

Very likely useful.

```python
df.loc[df["signal"] > 0, "return"].mean()
```

Compare positive vs negative signal:

```python
df.groupby(df["signal"] > 0)["return"].mean()
```

Bin a continuous variable:

```python
df["bucket"] = pd.qcut(df["signal"], 10)
```

Then:

```python
df.groupby("bucket", observed=True)["return"].mean()
```

This is a very useful way to test whether a variable predicts returns.

---

# 28. `value_counts`

Categorical frequency:

```python
df["side"].value_counts()
```

Relative frequency:

```python
df["side"].value_counts(normalize=True)
```

So if you want an empirical probability:

```python
df["won"].mean()
```

if `won` is boolean.

Or:

```python
(df["return"] > 0).mean()
```

That gives the fraction of positive returns.

---

# 29. Index of max/min

```python
df["return"].idxmax()
df["return"].idxmin()
```

Then:

```python
df.loc[df["return"].idxmax()]
```

Top 5:

```python
df.nlargest(5, "return")

df.nsmallest(5, "return")
```

---

# 30. Basic plotting

I would know minimal matplotlib, although plotting may not be required.

```python
import matplotlib.pyplot as plt
```

Histogram:

```python
plt.hist(df["return"], bins=50)
plt.show()
```

Scatter plot:

```python
plt.scatter(df["signal"], df["return"])
plt.xlabel("signal")
plt.ylabel("return")
plt.show()
```

Line:

```python
plt.plot(df["timestamp"], df["price"])
plt.show()
```

Pandas shortcut:

```python
df["return"].hist()
```

---

# 31. Linear regression

I wouldn't make this your top priority, but know the basics conceptually.

Using NumPy:

```python
x = df["signal"].to_numpy()
y = df["return"].to_numpy()

beta, intercept = np.polyfit(x, y, 1)
```

Or, if statsmodels is available:

```python
import statsmodels.api as sm

X = sm.add_constant(df["signal"])
model = sm.OLS(df["return"], X).fit()

print(model.summary())
```

Understand:

\[
Y = \alpha + \beta X + \varepsilon
\]

`beta > 0` means higher X tends to correspond to higher Y.

But don't conclude “predictive” from in-sample regression alone.

---

# 32. Standardization / z-scores

Could be useful.

```python
x = df["x"]

df["z"] = (x - x.mean()) / x.std()
```

Concept:

\[
z_i = \frac{x_i-\bar x}{s}
\]

---

# 33. Outliers

Basic inspection:

```python
df["x"].quantile([0.01, 0.5, 0.99])
```

Extreme values:

```python
df.nlargest(10, "x")
df.nsmallest(10, "x")
```

Don't automatically delete them.

First ask whether they are:

- genuine extreme observations
- bad data
- structural events
- data-entry errors

---

# 34. Useful probability simulation patterns

Coin flips:

```python
rng = np.random.default_rng()

flips = rng.integers(0, 2, size=100000)

flips.mean()
```

Two dice:

```python
d1 = rng.integers(1, 7, size=100000)
d2 = rng.integers(1, 7, size=100000)

np.mean(d1 + d2 >= 10)
```

Expected payoff:

```python
payoff = np.where(d1 + d2 >= 10, 5, -1)

payoff.mean()
```

Conditional probability:

```python
A = d1 == 6
B = d1 + d2 >= 10

np.mean(B[A])
```

Equivalent to estimating:

\[
P(B\mid A)
\]

---

# 35. Cumulative quantities

Cumulative returns:

For simple returns:

```python
wealth = (1 + df["return"]).cumprod()
```

Cumulative sum:

```python
df["pnl_cumulative"] = df["pnl"].cumsum()
```

Potentially useful if they give trade-level P&L.

---

# 36. Market-style calculations

Midprice:

```python
mid = (bid + ask) / 2
```

Spread:

```python
spread = ask - bid
```

Relative spread:

```python
relative_spread = (ask - bid) / mid
```

Simple return:

```python
r = new_price / old_price - 1
```

Log return:

```python
r = np.log(new_price / old_price)
```

Portfolio P&L:

```python
pnl = position * price_change
```

For multiple observations:

```python
df["pnl"] = df["position"] * df["price_change"]
df["pnl"].sum()
```

---

# 37. Things I'd memorize because they're easy to forget

```python
# AND
(df["x"] > 0) & (df["y"] < 10)

# OR
(df["x"] > 0) | (df["y"] < 10)

# NOT
~(df["x"] > 0)
```

Not:

```python
and
or
not
```

for Series.

Also:

```python
# equality
df["x"] == 5

# NOT
df["x"] != 5
```

Not:

```python
df["x"] = 5
```

because that assigns 5 to the whole column.

---

# 38. One very important Pandas gotcha

Prefer:

```python
df.loc[df["x"] > 0, "signal"] = 1
```

rather than chained indexing like:

```python
df[df["x"] > 0]["signal"] = 1
```

The second can give `SettingWithCopyWarning` and behave unexpectedly.

---

# 39. Common assessment workflow

If they hand you an unfamiliar CSV, my first ~5 minutes would look almost mechanically like this:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("data.csv")

print(df.shape)
print(df.head())
print(df.dtypes)
print(df.isna().sum())
print(df.describe())
```

Then identify:

```python
df["target"].describe()
df["feature"].describe()

df["feature"].corr(df["target"])

plt.scatter(df["feature"], df["target"])
plt.show()
```

Then conditional analysis:

```python
df["bucket"] = pd.qcut(
    df["feature"],
    10,
    duplicates="drop"
)

result = (
    df.groupby("bucket", observed=True)["target"]
      .agg(["mean", "std", "count"])
)

print(result)
```

That alone gets you surprisingly far.

---

# 40. What they may actually be testing

The syntax itself may be less important than whether you can look at a dataset and ask sensible questions.

Imagine the prompt:

> Here are 100,000 trades. Find anything interesting.

A weak approach is:

> I'll fit a complicated ML model.

A stronger approach is:

**Understand columns → check data quality → inspect distributions → formulate hypotheses → condition/group data → quantify relationships → check robustness → explain economic/statistical interpretation.**

For example:

```python
df.groupby("side")["future_return"].agg(
    ["mean", "std", "count"]
)
```

Then:

```python
df["size_bucket"] = pd.qcut(
    df["trade_size"],
    10,
    duplicates="drop"
)

df.groupby(
    ["side", "size_bucket"],
    observed=True
)["future_return"].mean()
```

Now you're actually investigating structure.

---

## What I would NOT spend tonight learning

Don't go down rabbit holes with:

- dynamic programming algorithms
- trees/graphs
- LeetCode mediums/hards
- object-oriented design
- decorators
- generators
- multithreading
- TensorFlow/PyTorch
- advanced sklearn
- SQL unless they explicitly mentioned it
- obscure pandas functions

For this interview, I would want you **very fluent with simple Python + NumPy + pandas + basic statistical reasoning** instead.

The crucial functions to be able to type without thinking are:

```python
pd.read_csv
head
shape
dtypes
describe
isna
dropna
fillna
sort_values
groupby
agg
mean
std
count
value_counts
merge
shift
diff
rolling
corr
quantile

np.mean
np.std
np.var
np.where
np.log
np.sqrt
np.random.default_rng
```

If you can use those confidently, you're no longer walking into the data assessment helpless.

**The best thing you could do now is spend ~60–90 minutes actually typing this stuff rather than reading it.** Open a notebook, generate a random DataFrame, and reproduce the major sections above without copying.

After that, I can give you a **realistic 60-minute data assessment from scratch**, where I provide the dataset and deliberately test `groupby`, filtering, returns, conditional means, correlation, missing values, and statistical interpretation.
