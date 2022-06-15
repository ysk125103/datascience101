The authors of [Introduction to Statistical Learning, 2e](https://www.statlearning.com/) has provided multiple datasets for practical labs and exercises. You can find the R package containing all of these datasets [here](https://cran.r-project.org/web/packages/ISLR2/index.html). 

The __Default__ dataset contains credit card debt information for 10,000 consumers. I've made few minor updates to the dataset: 
* The binary categorical columns __student__ and __default__ contain values __Yes__ and __No__. I've modified updated the dataset slightly to map these values to __1__ and __0__ respectively.
* Removed the first column with the range index

Why did I do these changes? 

I plan to use this dataset in a few blog posts at [Proclus Academy](https://proclusacademy.com). The blog covers basic Machine Learning concepts. And I don't want to include preprocessing steps in the blog as it takes away the focus from the main topic. Hence I'm getting the dataset in a shape I can use directly in the blog.


```python
import pandas as pd
```


```python
dataset = pd.read_csv('Default_original.csv')
dataset
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>default</th>
      <th>student</th>
      <th>balance</th>
      <th>income</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>No</td>
      <td>No</td>
      <td>729.526495</td>
      <td>44361.625074</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>No</td>
      <td>Yes</td>
      <td>817.180407</td>
      <td>12106.134700</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>No</td>
      <td>No</td>
      <td>1073.549164</td>
      <td>31767.138947</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>No</td>
      <td>No</td>
      <td>529.250605</td>
      <td>35704.493935</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>No</td>
      <td>No</td>
      <td>785.655883</td>
      <td>38463.495879</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>9995</th>
      <td>9996</td>
      <td>No</td>
      <td>No</td>
      <td>711.555020</td>
      <td>52992.378914</td>
    </tr>
    <tr>
      <th>9996</th>
      <td>9997</td>
      <td>No</td>
      <td>No</td>
      <td>757.962918</td>
      <td>19660.721768</td>
    </tr>
    <tr>
      <th>9997</th>
      <td>9998</td>
      <td>No</td>
      <td>No</td>
      <td>845.411989</td>
      <td>58636.156984</td>
    </tr>
    <tr>
      <th>9998</th>
      <td>9999</td>
      <td>No</td>
      <td>No</td>
      <td>1569.009053</td>
      <td>36669.112365</td>
    </tr>
    <tr>
      <th>9999</th>
      <td>10000</td>
      <td>No</td>
      <td>Yes</td>
      <td>200.922183</td>
      <td>16862.952321</td>
    </tr>
  </tbody>
</table>
<p>10000 rows × 5 columns</p>
</div>



### Remove the index column


```python
# Drop the first column - that's just a range index
dataset.drop(dataset.columns[0], axis='columns', inplace=True)
```

### Update the `default` column


```python
dataset['default'].value_counts()
```




    No     9667
    Yes     333
    Name: default, dtype: int64




```python
dataset['default'] = dataset['default'].map({'Yes': 1, 'No': 0})
```

### Update the `student` column


```python
dataset['student'].value_counts()
```




    No     7056
    Yes    2944
    Name: student, dtype: int64




```python
dataset['student'] = dataset['student'].map({'Yes': 1, 'No': 0})
```


```python
dataset
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>default</th>
      <th>student</th>
      <th>balance</th>
      <th>income</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>729.526495</td>
      <td>44361.625074</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>1</td>
      <td>817.180407</td>
      <td>12106.134700</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>1073.549164</td>
      <td>31767.138947</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>0</td>
      <td>529.250605</td>
      <td>35704.493935</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>785.655883</td>
      <td>38463.495879</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>9995</th>
      <td>0</td>
      <td>0</td>
      <td>711.555020</td>
      <td>52992.378914</td>
    </tr>
    <tr>
      <th>9996</th>
      <td>0</td>
      <td>0</td>
      <td>757.962918</td>
      <td>19660.721768</td>
    </tr>
    <tr>
      <th>9997</th>
      <td>0</td>
      <td>0</td>
      <td>845.411989</td>
      <td>58636.156984</td>
    </tr>
    <tr>
      <th>9998</th>
      <td>0</td>
      <td>0</td>
      <td>1569.009053</td>
      <td>36669.112365</td>
    </tr>
    <tr>
      <th>9999</th>
      <td>0</td>
      <td>1</td>
      <td>200.922183</td>
      <td>16862.952321</td>
    </tr>
  </tbody>
</table>
<p>10000 rows × 4 columns</p>
</div>




```python
dataset.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 10000 entries, 0 to 9999
    Data columns (total 4 columns):
     #   Column   Non-Null Count  Dtype  
    ---  ------   --------------  -----  
     0   default  10000 non-null  int64  
     1   student  10000 non-null  int64  
     2   balance  10000 non-null  float64
     3   income   10000 non-null  float64
    dtypes: float64(2), int64(2)
    memory usage: 312.6 KB


### Save the updated dataset


```python
dataset.to_csv('Default.csv', index=False)
```


```python
pd.read_csv('Default.csv')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>default</th>
      <th>student</th>
      <th>balance</th>
      <th>income</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>729.526495</td>
      <td>44361.625074</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>1</td>
      <td>817.180407</td>
      <td>12106.134700</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>1073.549164</td>
      <td>31767.138947</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>0</td>
      <td>529.250605</td>
      <td>35704.493935</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>785.655883</td>
      <td>38463.495879</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>9995</th>
      <td>0</td>
      <td>0</td>
      <td>711.555020</td>
      <td>52992.378914</td>
    </tr>
    <tr>
      <th>9996</th>
      <td>0</td>
      <td>0</td>
      <td>757.962918</td>
      <td>19660.721768</td>
    </tr>
    <tr>
      <th>9997</th>
      <td>0</td>
      <td>0</td>
      <td>845.411989</td>
      <td>58636.156984</td>
    </tr>
    <tr>
      <th>9998</th>
      <td>0</td>
      <td>0</td>
      <td>1569.009053</td>
      <td>36669.112365</td>
    </tr>
    <tr>
      <th>9999</th>
      <td>0</td>
      <td>1</td>
      <td>200.922183</td>
      <td>16862.952321</td>
    </tr>
  </tbody>
</table>
<p>10000 rows × 4 columns</p>
</div>




```python

```
