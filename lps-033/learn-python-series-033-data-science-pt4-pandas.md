
# Learn Python Series (#33) - Data Science Part 4 - Pandas

### Repository
- https://github.com/pandas-dev/pandas
- https://github.com/python/cpython

### What will I learn?
- You will learn that `pandas` provides you with powerful tools you can use to filter data with;
- individual and multiple conditions can be combined;
- inline conditions can be used;
- Boolean variables can be defined, useful for readability and re-use;
- "auto-magical" conditional expression strings can be passed as arguments to the `.query()` method, but beware for potential security vulnerabilities in case you're accepting external (user / bot) input!

### Requirements
- A working modern computer running macOS, Windows or Ubuntu;
- An installed Python 3(.7) distribution, such as (for example) the Anaconda Distribution;
- The ambition to learn Python programming.

### Difficulty
- Beginner, intermediate

### Additional sample code files
The full - and working! - iPython tutorial sample code file is included for you to download and run for yourself right here:


The example CSV file that was used in the episodes #31 and #32 is copied to the lps-033 folder as well:

https://github.com/realScipio/learn-python-series/blob/master/lps-033/btcusdt_20190602_20190604_1min_hloc.csv

### GitHub Account
https://github.com/realScipio

# Learn Python Series (#33) - Data Science Part 4 - Pandas

### Re-loading the actual BTCUSDT financial data using `pandas`
First, let's again read and open the file `btcusdt_20190602_20190604_1min_hloc.csv` [found here](https://github.com/realScipio/learn-python-series/blob/master/lps-033/btcusdt_20190602_20190604_1min_hloc.csv) on my GitHub account, (after having saved the file to your current working directory, from which you're also opening it using `.read_csv()`):


```python
import pandas as pd
df = pd.read_csv('btcusdt_20190602_20190604_1min_hloc.csv', 
                 parse_dates=['datetime'], index_col='datetime')
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>open</th>
      <th>high</th>
      <th>low</th>
      <th>close</th>
      <th>volume</th>
    </tr>
    <tr>
      <th>datetime</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-06-02 00:00:00+00:00</th>
      <td>8545.10</td>
      <td>8548.55</td>
      <td>8535.98</td>
      <td>8537.67</td>
      <td>17.349543</td>
    </tr>
    <tr>
      <th>2019-06-02 00:01:00+00:00</th>
      <td>8537.53</td>
      <td>8543.49</td>
      <td>8524.00</td>
      <td>8534.66</td>
      <td>31.599922</td>
    </tr>
    <tr>
      <th>2019-06-02 00:02:00+00:00</th>
      <td>8533.64</td>
      <td>8540.13</td>
      <td>8529.98</td>
      <td>8534.97</td>
      <td>7.011458</td>
    </tr>
    <tr>
      <th>2019-06-02 00:03:00+00:00</th>
      <td>8534.97</td>
      <td>8551.76</td>
      <td>8534.00</td>
      <td>8551.76</td>
      <td>5.992965</td>
    </tr>
    <tr>
      <th>2019-06-02 00:04:00+00:00</th>
      <td>8551.76</td>
      <td>8554.76</td>
      <td>8544.62</td>
      <td>8549.30</td>
      <td>15.771411</td>
    </tr>
  </tbody>
</table>
</div>



# Executing `pandas` conditional filters
If you've been following along about `pandas` in this Data Science sub-series, hopefully by now you've realised that `pandas` provides you with quite some powerful built-in methods to analyse, enrich, clean-up, and (re-)model data sets with. Like Excel / OpenOffice spreadsheets and like database management systems, `pandas` is able to "filter" data.

There exist multiple techniques to execute conditional filters.

### Inline conditionals
Using the same CSV file, containing 4320 1-minute ticks, fetched from Binance, on their BTCUSDT trading pair, on dates June 2, 2019 to June 4, 2019, you've already analysed (via the `df.describe()` statistical overview method) that the price of Bitcoin in the given interval was trading between 7481.02 and 8814.78.


```python
df.describe()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>open</th>
      <th>high</th>
      <th>low</th>
      <th>close</th>
      <th>volume</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>4320.000000</td>
      <td>4320.000000</td>
      <td>4320.000000</td>
      <td>4320.000000</td>
      <td>4320.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>8354.033475</td>
      <td>8359.921905</td>
      <td>8347.543243</td>
      <td>8353.818926</td>
      <td>34.183344</td>
    </tr>
    <tr>
      <th>std</th>
      <td>358.395024</td>
      <td>357.338897</td>
      <td>359.911089</td>
      <td>358.538551</td>
      <td>54.520356</td>
    </tr>
    <tr>
      <th>min</th>
      <td>7490.200000</td>
      <td>7533.430000</td>
      <td>7481.020000</td>
      <td>7494.110000</td>
      <td>1.351415</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>7985.045000</td>
      <td>7990.270000</td>
      <td>7979.205000</td>
      <td>7984.997500</td>
      <td>11.114809</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>8519.605000</td>
      <td>8524.985000</td>
      <td>8513.490000</td>
      <td>8518.845000</td>
      <td>19.566122</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>8661.080000</td>
      <td>8666.992500</td>
      <td>8656.007500</td>
      <td>8661.200000</td>
      <td>35.273851</td>
    </tr>
    <tr>
      <th>max</th>
      <td>8808.820000</td>
      <td>8814.780000</td>
      <td>8805.850000</td>
      <td>8809.910000</td>
      <td>949.563225</td>
    </tr>
  </tbody>
</table>
</div>



In order to derive how many 1-minute candles the data set contains in which the opening price (the price data in the "open" column) was higher than 8800, the condition itself is - of course:

`df['open'] > 8800`

To use this condition **inline**, pass that exact condition as a squared bracket "slicing argument" to the DataFrame `df`, like so:


```python
df_over_8000 = df[df['open'] > 8800]
df_over_8000
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>open</th>
      <th>high</th>
      <th>low</th>
      <th>close</th>
      <th>volume</th>
    </tr>
    <tr>
      <th>datetime</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-06-02 12:04:00+00:00</th>
      <td>8801.84</td>
      <td>8804.14</td>
      <td>8774.17</td>
      <td>8784.35</td>
      <td>98.534386</td>
    </tr>
    <tr>
      <th>2019-06-02 12:45:00+00:00</th>
      <td>8804.44</td>
      <td>8805.51</td>
      <td>8799.00</td>
      <td>8799.00</td>
      <td>45.084453</td>
    </tr>
    <tr>
      <th>2019-06-02 12:47:00+00:00</th>
      <td>8807.61</td>
      <td>8814.78</td>
      <td>8805.85</td>
      <td>8808.72</td>
      <td>33.917674</td>
    </tr>
    <tr>
      <th>2019-06-02 12:48:00+00:00</th>
      <td>8808.82</td>
      <td>8811.78</td>
      <td>8797.34</td>
      <td>8797.35</td>
      <td>68.527716</td>
    </tr>
  </tbody>
</table>
</div>



I hadn't explicitly explained using the `.count()` method before, for example in the "basic `pandas` statistic operations"-section in the 2nd Data Science sub-series episode ([found here](https://steemit.com/utopian-io/@scipio/learn-python-series-31-data-science-part-2-pandas)), but using the `.describe()` stats overview method, which was explained before, it was mentioned and used already. 

Regardless, if you want to count (to use for further processing or simply to return its value) the number of instances on which the condition is `True`, simply chain the `.count()` method, like so:


```python
df[df['open'] > 8800]['open'].count()
```




    4



And indeed, the `df_over_8000` result table printed 4 (in total) 1-minute data ticks correctly, containing opening prices higher than 8800.

### Filtering using multiple inline conditions
In order to filter (= keep) using two or more conditions, use multiple conditions as "filtering slice" via the `&` (and) and `|` (or) bitwise operators.

Say, for example, you want to filter all 1-minute ticks where opening price was above 8800 **and** trading volume was above 50 Bitcoins, execute:


```python
df_over_8000_high_volume = df[ (df['open'] > 8800) & (df['volume'] > 50) ]
df_over_8000_high_volume
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>open</th>
      <th>high</th>
      <th>low</th>
      <th>close</th>
      <th>volume</th>
    </tr>
    <tr>
      <th>datetime</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-06-02 12:04:00+00:00</th>
      <td>8801.84</td>
      <td>8804.14</td>
      <td>8774.17</td>
      <td>8784.35</td>
      <td>98.534386</td>
    </tr>
    <tr>
      <th>2019-06-02 12:48:00+00:00</th>
      <td>8808.82</td>
      <td>8811.78</td>
      <td>8797.34</td>
      <td>8797.35</td>
      <td>68.527716</td>
    </tr>
  </tbody>
</table>
</div>



By "eye-balling" the first (single-condition) example output, you can see the results are correct.

If we would replace `&` for `|`, we're filtering something completely different, being:

- let's keep all rows in which either opening price is above 8800,
or
- volume is above 50:


```python
df_over_8000_OR_high_volume = df[ (df['open'] > 8800) | (df['volume'] > 50) ]
df_over_8000_OR_high_volume.head(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>open</th>
      <th>high</th>
      <th>low</th>
      <th>close</th>
      <th>volume</th>
    </tr>
    <tr>
      <th>datetime</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-06-02 01:22:00+00:00</th>
      <td>8586.33</td>
      <td>8595.12</td>
      <td>8583.23</td>
      <td>8594.99</td>
      <td>51.051786</td>
    </tr>
    <tr>
      <th>2019-06-02 03:02:00+00:00</th>
      <td>8535.29</td>
      <td>8550.20</td>
      <td>8529.55</td>
      <td>8545.56</td>
      <td>56.543773</td>
    </tr>
    <tr>
      <th>2019-06-02 07:33:00+00:00</th>
      <td>8588.05</td>
      <td>8599.85</td>
      <td>8586.00</td>
      <td>8599.36</td>
      <td>61.909827</td>
    </tr>
    <tr>
      <th>2019-06-02 07:34:00+00:00</th>
      <td>8599.56</td>
      <td>8615.00</td>
      <td>8598.09</td>
      <td>8614.65</td>
      <td>197.822432</td>
    </tr>
    <tr>
      <th>2019-06-02 07:35:00+00:00</th>
      <td>8611.87</td>
      <td>8652.75</td>
      <td>8611.05</td>
      <td>8648.88</td>
      <td>292.018398</td>
    </tr>
    <tr>
      <th>2019-06-02 07:36:00+00:00</th>
      <td>8648.90</td>
      <td>8680.00</td>
      <td>8648.87</td>
      <td>8670.11</td>
      <td>239.552508</td>
    </tr>
    <tr>
      <th>2019-06-02 07:37:00+00:00</th>
      <td>8673.25</td>
      <td>8673.25</td>
      <td>8653.92</td>
      <td>8661.19</td>
      <td>96.012789</td>
    </tr>
    <tr>
      <th>2019-06-02 07:38:00+00:00</th>
      <td>8661.68</td>
      <td>8676.26</td>
      <td>8661.68</td>
      <td>8674.99</td>
      <td>54.226364</td>
    </tr>
    <tr>
      <th>2019-06-02 07:39:00+00:00</th>
      <td>8674.99</td>
      <td>8720.00</td>
      <td>8671.88</td>
      <td>8715.01</td>
      <td>188.849318</td>
    </tr>
    <tr>
      <th>2019-06-02 07:40:00+00:00</th>
      <td>8710.01</td>
      <td>8714.87</td>
      <td>8685.24</td>
      <td>8703.31</td>
      <td>118.652782</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_over_8000_OR_high_volume['open'].count()
```




    671



Remembering that `df_over_8000_high_volume` returned only 2 rows in which both conditions were met, if we substract those 2 from the individual conditionals combined, indeed we get 671 as a result:


```python
price_over_8800 = df[(df['open'] > 8800)]['open'].count()
volume_over_50 = df[df['volume'] > 50]['open'].count()
num_df_over_8000_high_volume = df_over_8000_high_volume['open'].count()

print(f"price_over_8800: {price_over_8800}")
print(f"volume_over_50: {volume_over_50}")
print(f"num_df_over_8000_high_volume: {num_df_over_8000_high_volume}")
```

    price_over_8800: 4
    volume_over_50: 669
    num_df_over_8000_high_volume: 2


- In 4 instances, price was over 8800; 
- In 669 instances, volume was over 50;
- In 2 instances, price was over 8800 and volume over 50

Ergo: (669 + 4) - 2 = 671

### Filtering passing Boolean variables
As you may have noticed, filtering by multipal inline conditions very quickly leads to long lines of code. For readability matters, it's also possible to assign Boolean variables, and pass those instead. The following syntax (same example) also works:


```python
price_over_8800 = df['open'] > 8800
volume_over_50 = df['volume'] > 50
df_over_8000_high_volume = df[price_over_8800 & volume_over_50]
df_over_8000_high_volume
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>open</th>
      <th>high</th>
      <th>low</th>
      <th>close</th>
      <th>volume</th>
    </tr>
    <tr>
      <th>datetime</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-06-02 12:04:00+00:00</th>
      <td>8801.84</td>
      <td>8804.14</td>
      <td>8774.17</td>
      <td>8784.35</td>
      <td>98.534386</td>
    </tr>
    <tr>
      <th>2019-06-02 12:48:00+00:00</th>
      <td>8808.82</td>
      <td>8811.78</td>
      <td>8797.34</td>
      <td>8797.35</td>
      <td>68.527716</td>
    </tr>
  </tbody>
</table>
</div>



--- this looks much better!

### Filtering using the `.query()` method
A third way to filter DataFrame data, is by using the `.query()` method. `.query()` expects to receive a string as its `expr=` argument, using **Boolean** (instead of bitwise) operators, ergo: `and` / `or`.

Also, the column names are referenced inside the conditional expression string. Like so (again: same example):


```python
query_expression = 'open > 8800 and volume > 50'
df.query(query_expression)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>open</th>
      <th>high</th>
      <th>low</th>
      <th>close</th>
      <th>volume</th>
    </tr>
    <tr>
      <th>datetime</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-06-02 12:04:00+00:00</th>
      <td>8801.84</td>
      <td>8804.14</td>
      <td>8774.17</td>
      <td>8784.35</td>
      <td>98.534386</td>
    </tr>
    <tr>
      <th>2019-06-02 12:48:00+00:00</th>
      <td>8808.82</td>
      <td>8811.78</td>
      <td>8797.34</td>
      <td>8797.35</td>
      <td>68.527716</td>
    </tr>
  </tbody>
</table>
</div>



**Nota bene:** Please stand still for a second and realise that, in order to make the last mentioned `.query()` method work, only a "humanized" looking string needed to be passed-in as an argument. It seems to "just work auto-magically"...

Under the hood, `.query()` utilises the (not yet explained) `.eval()` method of `pandas`, which is - in general - able to evaluate strings to derive column-wise vectorised operations from. How magical and convenient that may be (it is very cool actually!) it also allows `.eval()` to execute arbitrary code. So be careful when using it on an interface where others (users, bots) are allowed to pass their (potentially dangerous) input strings as `.eval()` arguments!

# What did we learn, hopefully?
Hopefully you've learned that `pandas` provides you with techniques for filtering data based on one or more conditional filters. When using bitwise operators `&` / `|`, or boolean operators `and` / `or` (using `.query()`), keep in mind you're writing expressions about which rows you want to **keep** (not drop).

### Thank you for your time!

### Curriculum (of the `Learn Python Series`):

- [Learn Python Series - Intro](https://steemit.com/utopian-io/@scipio/learn-python-series-intro)
- [Learn Python Series (#2) - Handling Strings Part 1](https://steemit.com/utopian-io/@scipio/learn-python-series-2-handling-strings-part-1)
- [Learn Python Series (#3) - Handling Strings Part 2](https://steemit.com/utopian-io/@scipio/learn-python-series-3-handling-strings-part-2)
- [Learn Python Series (#4) - Round-Up #1](https://steemit.com/utopian-io/@scipio/learn-python-series-4-round-up-1)
- [Learn Python Series (#5) - Handling Lists Part 1](https://steemit.com/utopian-io/@scipio/learn-python-series-5-handling-lists-part-1)
- [Learn Python Series (#6) - Handling Lists Part 2](https://steemit.com/utopian-io/@scipio/learn-python-series-6-handling-lists-part-2)
- [Learn Python Series (#7) - Handling Dictionaries](https://steemit.com/utopian-io/@scipio/learn-python-series-7-handling-dictionaries)
- [Learn Python Series (#8) - Handling Tuples](https://steemit.com/utopian-io/@scipio/learn-python-series-8-handling-tuples)
- [Learn Python Series (#9) - Using Import](https://steemit.com/utopian-io/@scipio/learn-python-series-9-using-import)
- [Learn Python Series (#10) - Matplotlib Part 1](https://steemit.com/utopian-io/@scipio/learn-python-series-10-matplotlib-part-1)
- [Learn Python Series (#11) - NumPy Part 1](https://steemit.com/utopian-io/@scipio/learn-python-series-11-numpy-part-1)
- [Learn Python Series (#12) - Handling Files](https://steemit.com/utopian-io/@scipio/learn-python-series-12-handling-files)
- [Learn Python Series (#13) - Mini Project - Developing a Web Crawler Part 1](https://steemit.com/utopian-io/@scipio/learn-python-series-13-mini-project-developing-a-web-crawler-part-1)
- [Learn Python Series (#14) - Mini Project - Developing a Web Crawler Part 2](https://steemit.com/utopian-io/@scipio/learn-python-series-14-mini-project-developing-a-web-crawler-part-2)
- [Learn Python Series (#15) - Handling JSON](https://steemit.com/utopian-io/@scipio/learn-python-series-15-handling-json)
- [Learn Python Series (#16) - Mini Project - Developing a Web Crawler Part 3](https://steemit.com/utopian-io/@scipio/learn-python-series-16-mini-project-developing-a-web-crawler-part-3)
- [Learn Python Series (#17) - Roundup #2 - Combining and analyzing any-to-any multi-currency historical data](https://steemit.com/utopian-io/@scipio/learn-python-series-17-roundup-2-combining-and-analyzing-any-to-any-multi-currency-historical-data)
- [Learn Python Series (#18) - PyMongo Part 1](https://steemit.com/utopian-io/@scipio/learn-python-series-18-pymongo-part-1)
- [Learn Python Series (#19) - PyMongo Part 2](https://steemit.com/utopian-io/@scipio/learn-python-series-19-pymongo-part-2)
- [Learn Python Series (#20) - PyMongo Part 3](https://steemit.com/utopian-io/@scipio/learn-python-series-20-pymongo-part-3)
- [Learn Python Series (#21) - Handling Dates and Time Part 1](https://steemit.com/utopian-io/@scipio/learn-python-series-21-handling-dates-and-time-part-1)
- [Learn Python Series (#22) - Handling Dates and Time Part 2](https://steemit.com/utopian-io/@scipio/learn-python-series-22-handling-dates-and-time-part-2)
- [Learn Python Series (#23) - Handling Regular Expressions Part 1](https://steemit.com/utopian-io/@scipio/learn-python-series-23-handling-regular-expressions-part-1)
- [Learn Python Series (#24) - Handling Regular Expressions Part 2](https://steemit.com/utopian-io/@scipio/learn-python-series-24-handling-regular-expressions-part-2)
- [Learn Python Series (#25) - Handling Regular Expressions Part 3](https://steemit.com/utopian-io/@scipio/learn-python-series-25-handling-regular-expressions-part-3)
- [Learn Python Series (#26) - pipenv & Visual Studio Code](https://steemit.com/utopian-io/@scipio/learn-python-series-26-pipenv-and-visual-studio-code)
- [Learn Python Series (#27) - Handling Strings Part 3 (F-Strings)](https://steemit.com/utopian-io/@scipio/learn-python-series-27-handling-strings-part-3-f-strings)
- [Learn Python Series (#28) - Using Pickle and Shelve](https://steemit.com/utopian-io/@scipio/learn-python-series-28-using-pickle-and-shelve)
- [Learn Python Series (#29) - Handling CSV](https://steemit.com/utopian-io/@scipio/learn-python-series-29-handling-csv)
- [Learn Python Series (#30) - Data Science Part 1 - Pandas](https://steemit.com/utopian-io/@scipio/learn-python-series-30-data-science-part-1-pandas)
- [Learn Python Series (#31) - Data Science Part 2 - Pandas](https://steemit.com/utopian-io/@scipio/learn-python-series-31-data-science-part-2-pandas)
- [Learn Python Series (#32) - Data Science Part 3 - Pandas](https://steemit.com/utopian-io/@scipio/learn-python-series-32-data-science-part-3-pandas)
