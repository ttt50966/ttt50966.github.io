---
title:  "Fitting template"
date:   2020-03-25 03:00:48 +0800
categories: 
  Lab
tags:
  Python
  Data
  Lab
  template
comments: true
---

## Fitting inverse spin Hall voltage

### 定義 `residual` and `fit_data` 函式

```python
from scipy.signal import find_peaks
import lmfit
import numpy as np
def residual(params, x, y):
    const = params['const'].value
    slope = params['slope'].value
    c = params['c'].value
    A = params['A'].value
    B = params['B'].value
    T = params['T'].value
    Hfmr = params['Hfmr'].value
    model = const + slope * (x-c) + A* T**2/((x-Hfmr)**2+T**2) - B * 2*T*(x-Hfmr)/((x-Hfmr)**2+T**2)
    return np.sqrt((y-model)**2)
def fit_data(params, x):
    const = params['const'].value
    slope = params['slope'].value
    c = params['c'].value
    A = params['A'].value
    B = params['B'].value
    T = params['T'].value
    Hfmr = params['Hfmr'].value
    model = const + slope * (x-c) + A* T**2/((x-Hfmr)**2+T**2) - B * 2*T*(x-Hfmr)/((x-Hfmr)**2+T**2)
    return model

```

### Fit the data

```python
import pandas as pd
data = pd.read_csv(file, sep='\t')
x = data["Field (G)"].values
y = data["Voltage R (V)"].values * 1E6
peaks, _= find_peaks(y, height=0.2) # 尋找比0.2高的峰值
params = lmfit.Parameters() # 新增Fitting參數物件
params.add('const', value= 0) #增加參數及其value
params.add('slope', value=0)
params.add('c', value=0)
params.add('A',value=0)
params.add('B',value=0)
params.add('T', value=50)
params.add('Hfmr', value=x[peaks][0])
out = lmfit.minimize(residual, params, args=(x,y)) #利用lmfit.minimize 找尋residual的局部極小值
```

