# base64、base32、base16编码解码
## base64加密解密
加密：
```
>>> import base64
>>> encode = base64.b64encode(b'I love you')
>>> encode
b'SSBsb3ZlIHlvdQ=='
```
解密：
```
>>> import base64
>>> decode = base64.b64decode(b'SSBsb3ZlIHlvdQ==')
>>> decode
b'I love you'
```
二、base32加密解密
跟base64相似就是将base64.b64encode变成base64.b32encode
加密：
```
>>> import base64
>>> encode = base64.b32encode(b'I love you')
>>> encode
b'JEQGY33WMUQHS33V'
```
解密：
```
>>> import base64
>>> decode = base64.b32decode(b'JEQGY33WMUQHS33V')
>>> decode
b'I love you'
```
## base16加密解密
类似的将base64.b32encode变成base64.b16encode
加密解密过程与base64、base32一样，这里就不在赘述


# md5

在python3的标准库中，已经移除了md5，而关于hash加密算法都放在hashlib这个标准库中，如SHA1、SHA224、SHA256、SHA384、SHA512和MD5算法等。

另：在网上找关于python的md5加密，发现要不是比较旧的不适用当前py版本的文章，或者是说得不够清楚的文章，所以还是自己去看下官方文档比较好，顺便整理下关于md5的使用方法。

对于学习任何一门程序类知识，我都认为去看官方文档这种学习方式最有效的之一，只不过一般这些文档都是英文版的，对于一些学习者来说可能会有一定门槛，但习惯于阅读英文文章，是非常重要的。

建议直接阅读python3的hashlib文档： 
https://docs.python.org/3/library/hashlib.html?highlight=hashlib#credits

在hashlib库的hash算法中，提供了很多加密算法，有 sha1()、sha224()、sha256()、sha384()、sha512()、blake2b()和 blake2s()、md5()，这些方法都通过统一接口返回一个对象，例如，使用sha256()可以创建一个SHA-256的哈希对象。

当然，进行md5加密算法，就要用到md5()方法：
```
>>> import hashlib
>>> m = hashlib.md5()
>>> m.update(b'123')
>>> m.hexdigest()
'202cb962ac59075b964b07152d234b70'

 # 或者可以这样
>>> hashlib.md5(b'123').hexdigest()
'202cb962ac59075b964b07152d234b70'

# 也可以使用hash.new()这个一般方法
>>> hashlib.new('md5', b'123').hexdigest()
'202cb962ac59075b964b07152d234b70'
```

以上是对于英文进行md5加密的，如果要对中文进行加密，发现按照上面来写会报错，原因在于字符转码问题，要如下写：
```
>>> import hashlib
>>> data = '你好'
>>> hashlib.md5(data.encode(encoding='UTF-8')).hexdigest()
'7eca689f0d3389d9dea66ae112e5cfd7'
```
此处先将数据转换成UTF-8格式的，使用网上工具对比下加密的结果，发现有的md5加密工具并不是使用UTF-8格式加密的。 
经测试目前发现可以转为UTF-8、GBK、GB2312、GB18030，不分大小写(因为GBK/GB2312/GB18030均是针对汉字的编码，所以md5加密后结果一样)。 
除了这些编码格式之外，还会有其他编码的，目前还没发现，等各位补充。 
看下面实例：
```
>>> hashlib.md5('你好'.encode(encoding='UTF-8')).hexdigest()
'7eca689f0d3389d9dea66ae112e5cfd7'

>>> hashlib.md5('你好'.encode(encoding='GBK')).hexdigest()
'b94ae3c6d892b29cf48d9bea819b27b9'

>>> hashlib.md5('你好'.encode(encoding='GB2312')).hexdigest()
'b94ae3c6d892b29cf48d9bea819b27b9'

>>> hashlib.md5('你好'.encode(encoding='GB18030')).hexdigest()
'b94ae3c6d892b29cf48d9bea819b27b9'
```
如果你仅仅查md5的写法，看上面实例就够了； 
如果你是python新手，想了解这些方法的意思和用法，继续看下面内容。

## 解析

### hashlib.new(name[, data])方法

这是个一般性方法。 
name传入的是哈希加密算法的名称，如md5； 
data传入的是需要加密的数据，可忽略，在之后update()中传入。

```
>>> m = hashlib.new('md5')
>>> m.update(b'123456')
>>> m.hexdigest()
'202cb962ac59075b964b07152d234b70'
```
可以使用hashlib.algorithms_guaranteed或者hashlib.algorithms_available这两个内置属性查看hashlib支持哪些加密算法。

hashlib.algorithms_guaranteed是在所有平台上，保证被hashlib模块支持的hash算法名称的集合； 
hashlib.algorithms_available是在当前运行的python编译器可用的hash算法名称的集合，由于OpenSSL的原因，在这当中可能会出现重复的hash算法名称。 
hashlib.algorithms_guaranteed是hashlib.algorithms_available的子集。 
看下面输出：

```
>>> hashlib.algorithms_guaranteed
{'sha3_384', 'md5', 'blake2s', 'sha3_512', 'blake2b', 'shake_128', 'sha384', 'sha3_256', 'sha1', 'shake_256', 'sha3_224', 'sha512', 'sha256', 'sha224'}
>>> hashlib.algorithms_available
{'whirlpool', 'ripemd160', 'dsaEncryption', 'sha1', 'SHA224', 'sha512', 'sha256', 'SHA512', 'blake2s', 'blake2b', 'SHA256', 'sha384', 'sha3_256', 'SHA384', 'sha', 'sha224', 'RIPEMD160', 'shake_128', 'sha3_512', 'SHA', 'MD5', 'shake_256', 'DSA', 'sha3_384', 'DSA-SHA', 'ecdsa-with-SHA1', 'md5', 'SHA1', 'dsaWithSHA', 'md4', 'MD4', 'sha3_224'}

````
### hash.update(arg)
传入arg对象来更新hash的对象。必须注意的是，该方法只接受byte类型，否则会报错。这就是要在参数前添加b 来转换类型的原因：
```
>>> m = hashlib.md5()
>>> m.update('123456')
TypeError: Unicode-objects must be encoded before hashing
```

同时要注意，重复调用update(arg)方法，是会将传入的arg参数进行拼接，而不是覆盖。必须注意这一点，因为你在不熟悉update()原理的时候，你很可能就会被它坑了。 
也就是说，m.update(a); m.update(b) 等价于m.update(a+b)，看下面例子：
```
>>> m = hashlib.md5()
>>> m.update(b'123')
>>> m.hexdigest()
'202cb962ac59075b964b07152d234b70'
>>> m.update(b'456')
>>> m.hexdigest()
'e10adc3949ba59abbe56e057f20f883e'

>>> hashlib.md5(b'123456').hexdigest()
'e10adc3949ba59abbe56e057f20f883e'
```



### hash.hexdigest()
都知道，在英语中hex有十六进制的意思，因此该方法是将hash中的数据转换成数据，其中只包含十六进制的数字。另外还有hash.digest()方法。
