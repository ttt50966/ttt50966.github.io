---
title:  "台大付款自動化查詢"
date:   2021-01-10 16:30:42 +0800
categories: 
  Life
tags:
  Python
comments: true
---

一個懶人腳本，很多時候在Terminal裡面直接執行這個程式比起開網頁登入還要快
中間省下的時間可以做別的事😆
它會登入[這個網站](https://mis.cc.ntu.edu.tw/pay/Default.aspx)，然後取得你薪資欄的最後一位並顯示到帳日期
如果今年都還沒有薪資入帳，就不會顯示去年的最後一條(目前沒打算修正這個小bug)
記得要裝好`selenium`還有去抓Chrome符合你瀏覽器版本的`webdriver`


```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.options import Options
import time
import datetime
month = time.localtime(time.time()).tm_mon
today = datetime.datetime.now().date()

chrome_options = Options()  
chrome_options.add_argument("--headless")  
#使用chrome的webdriver
browser = webdriver.Chrome( options=chrome_options)
#開啟google首頁
browser.get('https://info2.ntu.edu.tw/pay/Default.aspx')
#如果需要執行完自動關閉，就要加上下面這一行
browser.find_element_by_id('ButtonSSO').click()
browser.find_element_by_name('user').send_keys('') #輸入你的帳號
browser.find_element_by_name('pass').send_keys('') #輸入你的密碼
browser.find_element_by_name('Submit').click()
element_array = []
amount_array = []

for month in range(2,100):
    try:
      element = browser.find_element_by_xpath('/html/body/form/div[7]/div/div/table/tbody/tr[%d]/td[2]' %(month)).text
      amount = browser.find_element_by_xpath('/html/body/form/div[7]/div/div/table/tbody/tr[%d]/td[4]' %(month)).text
      element_array.append(element)
      amount_array.append(amount)
    except:
      break

try:
    print("最後發薪日："+element_array[-2]+"\n所得："+amount_array[-2])
except:
    print("目前無資料(跨年度)")

browser.quit()

```

