---
title:  "平滑曲線"
date:   2021-01-14 01:00:42 +0800
categories: 
  Lab
tags:
  Python
comments: true
---


參考[這篇文章](https://blog.csdn.net/weixin_42782150/article/details/107176500)

套在自己data上面大概變這樣

```python
def moving_average(interval, windowsize):
    window = np.ones(int(windowsize)) / float(windowsize)
    re = np.convolve(interval, window, 'same')
    return re
df = pd.read_csv("PTCDA_Abs Diff.txt",sep='\t')
x = df["Field (G)"][3:]
y = df["Diff. S21_Log Mag. (dB)"][3:]
plt.figure(dpi=300, facecolor="white")
plt.plot(x,y,label="original")
y_new = moving_average(y,11)
plt.plot(x,y_new,label="smooth")
plt.legend()
```

<img src="/Users/kuanchia/Documents/ttt50966.github.io/assets/20210114_Smooth Data.png" alt="20210114_Smooth Data" style="zoom:50%;" />