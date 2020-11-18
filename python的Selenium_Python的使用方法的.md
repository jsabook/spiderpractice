# 如何使用seleium？
要先写以下代码：
```
from selenium import webdriver
browser = webdriver.Firefox()
```
实例化一个火狐浏览器
连接url
```
browser.get("http://baidu.com/")
```
关闭浏览器
```
browser.close()
```
http://www.cnblogs.com/taceywong/p/6602715.html

# 定位元素：
这里介绍两种定位方式
定位一个元素
`tag`定位
```
browser.find_element_by_xx(tag_name)("xx"(tag_value))
```
`xpath`定位
```
browser.find_element_by_xpath("xx"(xpath表达式))
```
查找多个节点
只要在element后面加上s就可以了。
```
如：browser.find_elements_by_xx(tag_name)("xx"(tag_value))
```
# 与浏览器的交互:
定位元素后，比较常见的方法有send_keys()这个可以在文本框中输入内容，也可以输入回车。clear()是清空文字。点击按钮是用click()
如这个例子
```
input=browser.find_element_by_id(“p”)
input.click()
```
获取属性
```
tag=browser.find_element_by_id(“p”)
print(tag.get_attribute())
```
获取文本值
```
tag=browser.find_element_by_id(“p”)
print(tag.text)
```
获取一个节点在web页面中的外观信息
比如location,size,其中location是获取节点在页面中的相对位置。size是获取这个显示出来图案的大小。
获取页面的源码
```
pageSource = browser.page_source
```
获取页面的截图
```
browser.save_screenshot("screenshut.png")
```
等待，selenium等待有两种。分别是显示等待和隐式等待。这两种都不好。这里介绍一下两者折中的方法。半显示半隐式等待
```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Firefox()
driver.get("http://somedomain/url_that_delays_loading")
wait= WebDriverWait(driver,10)
element = wait.until(
        EC.presence_of_element_located((By.ID, "myDynamicElement"))
)
```
上面就是实例化waite ，在加入until的条件
# cookies处理
获取cookies
```
browser.get_cookies()
```
加入cookies
```
browser.add_cookie({})
```
删除cookies
```
browser.delete_all_cookies()
```