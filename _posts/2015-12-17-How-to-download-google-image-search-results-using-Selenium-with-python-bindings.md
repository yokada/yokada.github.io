---
layout: default
title: How to download google images
---

The following short script is very rough.
```
from selenium import webdriver
import os
import urllib

chromedriver = "/path/to/your/selenium/chromedriver"
os.environ["webdriver.chrome.driver"] = chromedriver
driver = webdriver.Chrome(chromedriver)

driver.get('https://www.google.co.jp/search?q=selenium&safe=off&source=lnms&tbm=isch&sa=X')

imgs = driver.find_elements_by_css_selector('img.rg_i')
n = 1
for i in imgs:
	src = i.get_attribute('src')
	if src is None:
		continue
	urllib.urlretrieve(src, "capture_%d.jpg" % n)
	n+=1

#driver.close()
```

