---
title:  "Matplotlib template"
date:   2020-03-25 00:00:48 +0800
categories: 
  Lab
tags:
  Python
  Data
  Lab
  template
comments: true
---

```python
import matplotlib.pyplot as plt
fig, ax=plt.subplots(dpi =300)
plt.tight_layout(pad=3, w_pad=4.8, h_pad=3.6)
plt.tick_params(direction = "in")
plt.xticks(fontsize = 12,fontweight='bold')
plt.yticks(fontsize = 12,fontweight='bold')
plt.setp(ax.spines.values(), linewidth =2)
ax.xaxis.set_tick_params(width=2)
ax.yaxis.set_tick_params(width=2)
plt.ylabel("Microwave Power(dBm)",fontweight='bold',fontsize = 16)
plt.xlabel("Input Power from NA(dBm)",fontweight='bold',fontsize = 16)
plt.title("Real Output from Amp. @6GHz",fontweight='bold',fontsize = 16)
ax.plot(data_6G["power"].values,data_6G_real_power,'-o')
plt.savefig("Real Output from Amp_6GHz.png", dpi = 300)
```

