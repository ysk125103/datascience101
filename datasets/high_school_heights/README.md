# High School Heights Dataset

You will find three datasets containing heights of the high school students. 

All heights are in __inches__. 

__The data is simulated__. The heights are generated from a normal distribution with different sets of mean and standard deviation for boys and girls. 

|   Height Statistics (inches)    | Boys| Girls |
| ----------- | ----------- | ----------- |
| Mean       | 67       | 62 |
| Standard Deviation   | 2.9        | 2.2 |

There are 500 measurements for each gender.

Here are the datasets:

* __hs_heights.csv__: contains a single column with heights for all boys and girls. There's no way to tell which of the values are for boys and which ones are for girls.   

* __hs_heights_pair.csv__: has two columns. The first column has boy's heights. The second column contains girl's heights.

* __hs_heights_flag.csv__: has two columns. The first column has the flag __is_girl__. The second column contains a girl's height if the flag is __1__. Otherwise, it contains a boy's height.     



## How to (re)generate these datasets

Here's the code to create these datasets:
  


```python
import numpy as np
import pandas as pd
```

### Generate heights from normal distribution


```python
# good ones - 109 or 100. 
# maybe - 148(tallest), 151, 122
np.random.seed(180)

boys = np.random.normal(loc=67, scale=2.9, size=500)
girls = np.random.normal(loc=62, scale=2.2, size=500)

boys = boys.round(2)
girls = girls.round(2)
```

### Dataset: `hs_heights.csv`


```python
heights_combined = np.concatenate([boys, girls])

np.random.shuffle(heights_combined)

pd.DataFrame(heights_combined).to_csv("hs_heights.csv", index=False)
```


```python
pd.read_csv('hs_heights.csv')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>61.53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>63.55</td>
    </tr>
    <tr>
      <th>2</th>
      <td>63.96</td>
    </tr>
    <tr>
      <th>3</th>
      <td>64.66</td>
    </tr>
    <tr>
      <th>4</th>
      <td>63.88</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>995</th>
      <td>66.19</td>
    </tr>
    <tr>
      <th>996</th>
      <td>67.77</td>
    </tr>
    <tr>
      <th>997</th>
      <td>64.65</td>
    </tr>
    <tr>
      <th>998</th>
      <td>63.57</td>
    </tr>
    <tr>
      <th>999</th>
      <td>67.47</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 1 columns</p>
</div>



### Dataset: `hs_heights_pair.csv`


```python
df = pd.DataFrame({
    'boys':boys,
    'girls':girls
})

df.to_csv("hs_heights_pair.csv", index=False)
```


```python
df.describe().round(2)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>boys</th>
      <th>girls</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>500.00</td>
      <td>500.00</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>67.16</td>
      <td>61.98</td>
    </tr>
    <tr>
      <th>std</th>
      <td>2.89</td>
      <td>2.12</td>
    </tr>
    <tr>
      <th>min</th>
      <td>59.16</td>
      <td>56.68</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>65.18</td>
      <td>60.60</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>67.13</td>
      <td>62.00</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>68.95</td>
      <td>63.38</td>
    </tr>
    <tr>
      <th>max</th>
      <td>77.15</td>
      <td>69.35</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.read_csv('hs_heights_pair.csv')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>boys</th>
      <th>girls</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>64.44</td>
      <td>62.47</td>
    </tr>
    <tr>
      <th>1</th>
      <td>67.40</td>
      <td>63.27</td>
    </tr>
    <tr>
      <th>2</th>
      <td>63.93</td>
      <td>62.99</td>
    </tr>
    <tr>
      <th>3</th>
      <td>69.29</td>
      <td>64.60</td>
    </tr>
    <tr>
      <th>4</th>
      <td>66.96</td>
      <td>60.73</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>495</th>
      <td>70.89</td>
      <td>65.69</td>
    </tr>
    <tr>
      <th>496</th>
      <td>62.13</td>
      <td>58.06</td>
    </tr>
    <tr>
      <th>497</th>
      <td>72.97</td>
      <td>62.42</td>
    </tr>
    <tr>
      <th>498</th>
      <td>67.23</td>
      <td>62.18</td>
    </tr>
    <tr>
      <th>499</th>
      <td>71.85</td>
      <td>58.54</td>
    </tr>
  </tbody>
</table>
<p>500 rows × 2 columns</p>
</div>



### Dataset: `hs_heights_flag.csv`


```python
boys_with_flag  = [(0, boy) for boy in boys]
girls_with_flag  = [(1, girl) for girl in girls]

df = pd.DataFrame(boys_with_flag + girls_with_flag).sample(frac=1, random_state=180)
df.columns = ['is_girl', 'height']
df.reset_index(drop=True, inplace=True)
```


```python
df.groupby('is_girl').describe().T.round(2)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>is_girl</th>
      <th>0</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="8" valign="top">height</th>
      <th>count</th>
      <td>500.00</td>
      <td>500.00</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>67.16</td>
      <td>61.98</td>
    </tr>
    <tr>
      <th>std</th>
      <td>2.89</td>
      <td>2.12</td>
    </tr>
    <tr>
      <th>min</th>
      <td>59.16</td>
      <td>56.68</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>65.18</td>
      <td>60.60</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>67.13</td>
      <td>62.00</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>68.95</td>
      <td>63.38</td>
    </tr>
    <tr>
      <th>max</th>
      <td>77.15</td>
      <td>69.35</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.to_csv("hs_heights_flag.csv", index=False)
```


```python
pd.read_csv("hs_heights_flag.csv")
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>is_girl</th>
      <th>height</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>65.22</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>62.50</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>66.80</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>70.86</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>65.92</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>995</th>
      <td>1</td>
      <td>63.46</td>
    </tr>
    <tr>
      <th>996</th>
      <td>0</td>
      <td>67.21</td>
    </tr>
    <tr>
      <th>997</th>
      <td>0</td>
      <td>65.83</td>
    </tr>
    <tr>
      <th>998</th>
      <td>1</td>
      <td>64.00</td>
    </tr>
    <tr>
      <th>999</th>
      <td>1</td>
      <td>61.03</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 2 columns</p>
</div>


