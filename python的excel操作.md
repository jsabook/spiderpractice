# 初始操作
## 引用三个库
```
import xlrd
import xlwt
from xlutils.copy import copy
```
通过xlrd与xlet与xlutilds.copy的导入来对excel做基本的操作。
建立excel表格
内容：建立excel 首先现在python里面建立一个excel表格，最后当完成对这个表的操作时，保存在文件夹里即可。
## 建立：
```
workbook=xlwt.Workbook(encoding=””)     
worksheet=workbook.add____sheet(sheet_name)
```
## 保存：
```
workbook.save(保存路径)
```
# 对excel进行操作

## 打开excel表格，读取数据

+ 对excel进行内容填充
+ 对excel进行内容修改

## 打开excel表格，读取数据

获取excel
```
Date=xlrd.open__workbook(存储路径)
```
获取工作表，三种方法
```
table = data.sheets()[index_number]
index___numble 表示第几张工作表，这个方式也可以用来迭代
```
通过索引的方式
```
table = data.sheet_by_index(index_number)
```
这个是通过表的名字来获取
```
table = data.sheet_by_name(sheet_name)
```
获取单元格的相关信息

数目：`table.nrows` ,获取行数

`table.ncols ` ,获取列数

获取单元格的内容：

整行整列获取
```
table.row_values(row_index)
table.col_values(col_index)
```
一个一个获取：
```
table.cell(row_index, col_index).value
```
填充单元格内容：
```
worksheet.write(r, c, label='', style=<xlwt.Style.XFStyle object>)
```
## 对excel进行内容修改
其实就是用copy方法将表复制，进行相关的操作，最后保存回去就可以了
```
copyBook = copy(readbook) #拷贝整个excel
copyBook = copy(readSheet) #拷贝第一张表，然后拿到要修改的表
writeSheet = copybook.get_sheet(0)
writeSheet.write(1,0,'asd')#修改表一种的第二行第一个单元格内容
copybook.save('c:\\t2.xls')
```
