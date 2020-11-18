主要用到的就是PIL库
如何获取到一张图片上的某一个点的RGB值？
```
from PIL import Image
image=Image.open('ca.jpg')
piexl=image.load()[1,20]#图片上某一点的x,y坐标的RGB值，获取到的是一个三元数组
print(piexl)
```