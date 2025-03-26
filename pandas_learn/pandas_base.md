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
### unique() nunique()
pandas中有一些函数和数据中元素出现的频次相关。对Series使用unique()和nunique()可以分别得到其唯一值组成的列表和唯一值的个数：
```python
  In [50]:   df['School'].unique()

  Out[50]:   array(['A', 'B', 'C', 'D'], dtype=object)

  In [51]:   df['School'].nunique()

  Out[51]:   4
```
### value_counts()
  通过value_counts()可以得到序列中每个值出现的次数，当设定normalize为True时会进行归一化处理。
```python
  In [52]:   df['School'].value_counts()

  Out[52]: D    69
           A    57
           C    40
           B    34
           Name: School, dtype: int64

  In [53]:   df['School'].value_counts(normalize=True)

  Out[53]: D    0.345
           A    0.285
           C    0.200
           B    0.170
           Name: School, dtype: float64
```
### drop_duplicates() duplicated()
如果想要观察多个列组合的唯一值，可以使用drop_duplicates()。其中的关键参数是keep，默认值first表示保留每个组合第一次出现的所在行，指定为last表示保留每个组合最后一次出现的所在行，指定为False表示把所有组合重复的所在行剔除。
```python
  In [54]:   df_demo = df[['Gender','Transfer','Name']]
           df_demo.drop_duplicates(['Gender', 'Transfer'])

  Out[54]:         Gender Transfer            Name
           0     Female        N    Gaopeng Yang
           1       Male        N  Changqiang You
           12    Female      NaN        Peng You
           21      Male      NaN   Xiaopeng Shen
           36      Male        Y    Xiaojuan Qin
           43    Female        Y      Gaoli Feng

  In [55]:   df_demo.drop_duplicates(['Gender', 'Transfer'], keep='last')

  Out[55]:        Gender Transfer             Name
           147    Male      NaN         Juan You
           150    Male        Y    Chengpeng You
           169  Female        Y    Chengquan Qin
           194  Female      NaN      Yanmei Qian
           197  Female        N   Chengqiang Chu
           199    Male        N      Chunpeng Lv


  #将keep指定为False意味着保留标签或标签组合中只出现过一次的行：
  In [56]:   df_demo.drop_duplicates(['Name', 'Gender'], keep=False).head()

  Out[56]:      Gender Transfer            Name
           0  Female        N    Gaopeng Yang
           1    Male        N  Changqiang You
           2    Male        N         Mei Sun
           4    Male        N     Gaojuan You
           5  Female        N     Xiaoli Qian


 #我们在Series上也可以使用drop_duplicates()：
  In [57]:   df['School'].drop_duplicates()

  Out[57]:   0    A
           1    B
           3    C
           5    D
           Name: School, dtype: object
```

  duplicated()和drop_duplicates()的功能类似，但前者返回关于元素是否为唯一值的布尔列表，而其参数keep的意义与后者一致。duplicated()返回的序列把重复元素设为True，否则为False，drop_duplicates()等价于把duplicated()返回为True的对应行剔除。
  ```python
  In [58]:   df_demo.duplicated(['Gender', 'Transfer']).head()

  Out[58]:   0    False
           1    False
           2     True
           3     True
           4     True
           dtype: bool

  In [59]:   df['School'].duplicated().head()

  Out[59]:   0   False
           1   False
           2    True
           3   False
           4    True
           Name: School, dtype: bool
```
## 替换函数
### replace()
在replace()中，可以通过字典构造或者传入两个列表（分别表示需要替换的值和替换后的值）来进行替换：
```python
  In [60]:   df['Gender'].replace({'Female':0, 'Male':1}).head()

  Out[60]:   0    0
           1    1
           2    1
           3    0
           4    1
           Name: Gender, dtype: int64

  In [61]:   df['Gender'].replace(['Female', 'Male'], [0, 1]).head()

  Out[61]:   0    0
           1    1
           2    1
           3    0
           4    1
           Name: Gender, dtype: int64
```
另外，replace()还可以进行一种特殊的方向替换，指定参数method为ffill时，用前面一个最近的未被替换的值进行替换，参数method为bfill时，则用后面最近的未被替换的值进行替换。从下面的例子可以看到，它们的结果是不同的：
```python
  In [62]:   s = pd.Series(['a', 1, 'b', 2, 1, 1, 'a'])
           s.replace([1, 2], method='ffill')

  Out[62]:   0    a
           1    a
           2    b
           3    b
           4    b
           5    b
           6    a
           dtype: object

  In [63]:   s.replace([1, 2], method='bfill')

  Out[63]:   0    a
           1    b
           2    b
           3    a
           4    a
           5    a
           6    a
           dtype: object
```
### where() mask()
where()在传入条件为False的对应行进行替换，而mask()在传入条件为True的对应行进行替换，当未对二者指定替换值时，将对应行替换为缺失值。
```python
  In [64]:   s = pd.Series([-1, 1.2345, 100, -50])
           s.where(s<0)

  Out[64]:   0     -1.0
           1      NaN
           2      NaN
           3    -50.0
           dtype: float64

  In [65]:   s.where(s<0, 100)

  Out[65]:   0     -1.0
           1    100.0
           2    100.0
           3    -50.0
           dtype: float64

  In [66]:   s.mask(s<0)

  Out[66]:   0         NaN
           1      1.2345
           2    100.0000
           3         NaN
           dtype: float64

  In [67]:   s.mask(s<0, -50)

  Out[67]:   0    -50.0000
           1      1.2345
           2    100.0000
           3    -50.0000
           dtype: float64
```
需要注意的是，传入的条件只需要是布尔序列即可，但其索引应当与被调用的Series索引一致：
```python
  In [68]:   s_condition= pd.Series([True,False,False,True],index=s.index)
           s.mask(s_condition, -50)

  Out[68]:   0    -50.0000
           1      1.2345
           2    100.0000
           3    -50.0000
           dtype: float64
```
### round() abs() clip()
它们分别表示按照给定精度四舍五入、取绝对值和截断：
```python
  In [69]:   s = pd.Series([-1, 1.2345, 100, -50])
           s.round(2)

  Out[69]:   0     -1.00
           1      1.23
           2    100.00
           3    -50.00
           dtype: float64

  In [70]:   s.abs()

  Out[70]:   0    1.0000
           1    1.2345
           2  100.0000
           3   50.0000
           dtype: float64

  In [71]:   s.clip(0, 2) # 前两个数分别表示上下截断边界

  Out[71]:   0    0.0000
           1    1.2345
           2    2.0000
           3    0.0000
           dtype: float64
```
### sort_values() sort_index()
值排序函数sort_values()
默认参数ascending为True表示升序，对身高进行排序,当ascending为False时表示对身高进行降序排列
```python
In [73]:   df_demo.sort_values('Height').head()

In [74]:   df_demo.sort_values('Height', ascending=False).head()
```

多列排序：在体重相同的情况下，对身高进行排序，并且保持身高降序排列，体重升序排列
```python
In [75]:   df_demo.sort_values(['Weight','Height'],ascending=[True,False]).head()

```
索引排序函数sort_index()
索引排序的用法和值排序几乎完全一致，只不过元素的值在索引中，此时需要指定索引层的名字或者层号，用参数level表示。
```python
In [76]:   # 对年级按升序排列，对名字按降序排列,grade,name为索引
           df_demo.sort_index(level=['Grade','Name'],ascending=[True,False]).head()
```
### rank()
返回每个元素在整个序列中的排名，参数ascending表示升序排序，pct表示是否返回元素对应的分位数。
```python
  In [77]:   s = pd.Series(list("ebcad")) 
           s.rank(ascending=True, pct=False)

  Out[77]:   0  5.0
           1  2.0
           2  3.0
           3  1.0
           4  4.0
           dtype: float64
```
参数method用于控制元素相等时的排名处理方式，默认为“average”，取均值。取“min”和“max”时分别表示取最小的可能排名和最大的可能排名，取“first”时表示按照序列中元素出现的先后顺序排名，取“dense”时表示相邻大小的元素排名相差1。从下面的例子中可以看出它们的区别：
```python
  In [78]:   s = pd.Series(list("abac"))
           df_rank = pd.DataFrame()
           for method in ["average", "min", "max", "first", "dense"]:
               df_rank[method] = s.rank(method=method)
           df_rank

  Out[78]:       average       min       max     first     dense
           0       1.5       1.0       2.0       1.0       1.0
           1       3.0       3.0       3.0       3.0       2.0
           2       1.5       1.0       2.0       2.0       1.0
           3       4.0       4.0       4.0       4.0       3.0
```