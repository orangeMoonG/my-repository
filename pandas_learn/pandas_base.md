# 文件的读取和写入
## 文件读取
pandas的文件输入输出模块依赖xlrd、xlwt和openpyxl这3个第三方库，若未安装可使用如下命令安装：
可以使用如下<em>conda</em>命令或<em>pip</em>命令安装
```shell
$ conda install xlrd xlwt openpyxl
$ pip install xlrd xlwt openpyxl”
```
csv、txt和excel文件分别可以用read_csv()、read_table()和read_excel()读取，其中的传入参数为相应文件的绝对路径或相对路径。
```python
In [3]:   df_csv = pd.read_csv('data/ch2/my_csv.csv')

In [4]:   df_txt = pd.read_table('data/ch2/my_table.txt')

In [5]:   df_excel = pd.read_excel('data/ch2/my_excel.xlsx')
```
这些函数有一些公共参数，含义如下：将header设置为None表示第一行不作为列名；index_col表示把某一列或几列作为索引；usecols表示读取列的集合，默认读取所有列；parse_dates表示需要转化为时间的列；nrows表示读取的数据行数。上面这些参数在上述的3个函数里都可以使用。

在读取txt文件时，经常会遇到分隔符非空格的情况，read_table()有一个分割参数sep，它使得用户可以自定义分割符号来进行对txt类型数据的读取。例如，下面读取的表以“||||”为分割符号,可以使用参数sep(正则表达式)，同时需要指定引擎（engine）为Python
```python
In [12]:   pd.read_table(
               'data/ch2/my_table_special_sep.txt',
               sep= '\|\|\|\|',
               engine= 'python'
           )
```
## 数据写入
一般情况下，pandas会在数据写入时包含当前的数据索引，但很多情况下我们并不需要将默认的整数索引（0～n−1，n为行数）包含到输出表中，此时我们可以把index设置为False，该操作能在保存输出表的时候把索引去除。
```python
In [13]:  df_csv.to_csv('data/ch2/my_csv_saved.csv', index=False)
          df_excel.to_excel('data/ch2/my_excel_saved.xlsx', index=False)
```
pandas中没有定义to_table()函数，但是to_csv()函数可以将数据保存为txt文件，并且允许自定义分隔符、常用制表符t分割：
```python
In [14]:   df_txt.to_csv('data/ch2/my_txt_saved.txt', sep='\t', index=False)
```
如果想要把表格快速转换为markdown格式和latex格式，可以使用to_markdown()函数和to_latex()函数，此处需要安装tabulate包。
```python
  pip install tabulate

  In [15]:   print(df_csv.to_markdown())
```
# 基本数据结构
## Series
Series对象中包含4个重要的组成部分，分别是序列的值data、索引index、存储类型dtype和序列的名字name。其中，索引也可以指定名字。
```python
  In [17]:   s = pd.Series(data = [100, 'a', {'dic1':5}],
                         index = pd.Index(['id1', 20, 'third'], name='my_idx'),
                         dtype = 'object', # 常用的dtype还有int、float、string、category
                         name = 'my_name')
           s

  Out[17]:   my_idx
           id1              100
           20                 a
           third    {'dic1': 5}
           Name: my_name, dtype: object
```

对于这些属性等内容，可以通过“.”来获取：
```python
  In [18]:   s.values

  Out[18]:   array([100, 'a', {'dic1': 5}], dtype=object)

  In [19]:   s.index

  Out[19]:   Index(['id1', 20, 'third'], dtype='object', name='my_idx')

  In [20]:   s.dtype

  Out[20]:   dtype('O')

  In [21]:   s.name

  Out[21]:   'my_name'
```

  利用.shape可以获取序列的长度：
```python
  In [22]:   s.shape

  Out[22]:   (3,)
```

- 注解:
  object代表一种混合类型，正如上面的例子中存储了整数、字符串以及Python的字典数据结构。此外，在默认状态下，pandas把纯字符串序列当作一种object类型的序列，但它也可以显式地指定string作为其类型。

如果想要取出单个索引对应的值，可以通过[index_item]取出，其中index_item是索引的标签。
```python
  In [23]:   s['third']

  Out[23]:   {'dic1': 5}
```
## DataFrame
一个DataFrame可以由二维的data与行列索引来构造：
```python
  In [24]:   data = [[1, 'a', 1.2], [2, 'b', 2.2], [3, 'c', 3.2]]
           df = pd.DataFrame(data=data,
                             index=['row_%d'%i for i in range(3)],  # 行索引
                             columns=['col_0', 'col_1', 'col_2'])   # 列索引
           df

  Out[24]:         col_0 col_1   col_2
           row_0     1     a     1.2
           row_1     2     b     2.2
           row_2     3     c     3.2

#但更多的时候会采用从列索引名到数据的映射来构造DataFrame，再加上行索引：

  In [25]:   df = pd.DataFrame(data = {'col_0': [1,2,3], 'col_1':list('abc'),
                                     'col_2': [1.2, 2.2, 3.2]},
                             index = ['row_%d'%i for i in range(3)])
           df

  Out[25]:         col_0 col_1   col_2
           row_0     1     a     1.2
           row_1     2     b     2.2
           row_2     3     c     3.2
```
由于这种映射关系，在DataFrame中可以用[col_name]与[col_list]来取出相应的列与由多个列组成新的DataFrame，结果分别为Series和DataFrame：
```python
  In [26]:   df['col_0']

  Out[26]:   row_0    1
           row_1    2
           row_2    3
           Name: col_0, dtype: int64

  In [27]:   df[['col_0', 'col_1']]

  Out[27]:         col_0 col_1
           row_0     1     a
           row_1     2     b
           row_2     3     c
```
使用to_frame()函数可以把序列转换为列数为1的DataFrame：
```python
  In [28]:   df['col_0'].to_frame()

  Out[28]:        col_0
           row_0    1
           row_1    2
           row_2    3
```
与Series类似，在DataFrame中同样可以取出相应的属性：
```python
  In [29]:   df.values

  Out[29]:   array([[1, 'a', 1.2],
                  [2, 'b', 2.2],
                  [3, 'c', 3.2]], dtype=object)

  In [30]:   df.index

  Out[30]:   Index(['row_0', 'row_1', 'row_2'], dtype='object')

  In [31]:   df.columns

  Out[31]:   Index(['col_0', 'col_1', 'col_2'], dtype='object')

  In [32]:   df.dtypes # 返回的是值为相应列数据类型的Series

  Out[32]:   col_0      int64
           col_1     object
           col_2    float64
           dtype: object

  In [33]:   df.shape # 返回一个元组

  Out[33]:   (3, 3)
```

  通过“.T”可以把DataFrame的行列进行转置：
```python
  In [34]:   df.T

  Out[34]:          row_0  row_1  row_2
           col_0      1      2      3
           col_1      a      b      c
           col_2    1.2    2.2    3.2
```

当想要对列进行修改或者新增一列时，可以直接使用df[col_name]的方式：

```python
  In [35]:   df["col_0"] = df['col_0'].values[::-1] # 颠倒顺序
           df["col_2"] *= 2
           df["col_3"] = ["apple",banaa", "cat"] 
           df

  Out[35]:         col_0 col_1   col_2   col_3
           row_0     3     a     2.4   apple
           row_1     2     b     4.4  banana
           row_2     1     c     6.4     cat
```

  当想要删除某一个列时，可以使用drop方法：
```python
  In [36]:   df.drop(["col_3"], axis=1)

  Out[36]:         col_0 col_1   col_2 
           row_0     3     a     2.4 
           row_1     2     b     4.4 
           row_2     1     c     6.4  
```

  当axis取值为1时为删除列，而当axis取值为0时为删除行：
```python
  In [37]:   df.drop(["row_1"], axis=0)
           df

  Out[37]:         col_0 col_1   col_2   col_3
           row_0     3     a     2.4   apple
           row_2     1     c     6.4     cat
```
  

  - 注解
  Series或DataFrame的绝大多数方法在默认参数下都不会改变原表，而是返回一个临时拷贝。当真正需要在df上删除时，使用赋值语句df=df.drop(...)即可。
  

  同时，利用[col_list]的方式来选出需要的列可以做到如上的等价筛选：
  ```python
  In [38]:   df = df[df.columns[:-1]]
           df

  Out[38]:         col_0 col_1   col_2 
           row_0     3     a     2.4 
           row_1     2     b     4.4 
           row_2     1     c     6.4  
```
# 常用基本函数
## 汇总函数
### head() tail()
分别返回表或者序列的前n行和后n行信息，其中n默认为5
```python
  In [41]:   df.head(2)

  In [42]:   df.tail(3)
```
### info() describe()

info()函数和describe()函数分别返回表的信息概况和表中数值列对应的主要统计量：
```python
df.info()

df.describe()
```
## 特征统计函数
### sum()、mean()、median()、var()、std()、max()、min()
### quantile()、count()、idxmax()、idxmin()
quantile()、count()和idxmax()这3个函数，它们分别返回的是分位数、非缺失值个数和最大值对应的索引，idxmin()返回的是最小值对应的索引

由于上述所有函数对每一个序列进行操作后返回的结果是标量（单个值），因此它们又被称为聚合函数，它们有一个公共参数axis，默认值为0，代表逐列聚合，如果设置为1则表示逐行聚合：
## 频次函数
