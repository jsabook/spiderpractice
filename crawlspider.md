crawlspider

是继承与spider。和spider一样。但是多了一个属性。rule。其中rule是一个Rule为元素的集合。那Rule又是什么？下面是给出的官方定义

class scrapy.spiders.Rule(link_extractor, callback=None, cb_kwargs=None, follow=None, 

process_links=None, process_request=None)

其实这里主要就是两个参数比较重要，link_extractor和callback可以这样理解一个Rule就是两个参数。第一个参数，抓取什么URL去生成Request，第二个是生成的Request获取到的相应返回哪里。主要就是这两个参数。那link_extractor又是怎么生成的呢？

 