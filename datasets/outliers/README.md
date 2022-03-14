The dataset __outliers.csv__ contains two lists. 

The first list "no_outliers" contains normally distributed data. 

The second list is a copy of the first list except 5 items: they contain very large outliers.


Here's the code to generate it: 

```python
import numpy as np
import pandas as pd

random_seed = 135
np.random.seed(random_seed)

total_count = 35
outliers_count = 5

mean = 10
standard_deviation = 15

outlier_min = 250
outlier_max = 251

normal1 = np.random.normal(mean, standard_deviation, total_count-outliers_count)
normal2 = np.random.normal(mean, standard_deviation, outliers_count)

outliers = np.random.uniform(outlier_min, outlier_max, outliers_count)

no_outliers_list = np.append(normal1.copy(), normal2)
with_outliers_list = np.append(normal1.copy(), outliers)

dataset = pd.DataFrame({
    'no_outliers': no_outliers_list.round(2), 
    'with_outliers': with_outliers_list.round(2)
})

# shuffle the entire dataset
dataset = dataset.sample(frac=1, random_state=random_seed)

dataset.to_csv('outliers.csv', index=False)
```