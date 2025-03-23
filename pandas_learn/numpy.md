# Numpy数组构造
- 普通构造

```python 
array([1, 2, 3])
```

## 等差数列
```python
  In [28]:  np.linspace(1, 5, 5) #起始点，终止点（含），样本个数

  Out[28]:   array([1., 2., 3., 4., 5.])

  In [29]:   np.arange(1, 5, 1) # 与range()一样不含末端点,起始点，终止点（不含），步长

  Out[29]:   array([1, 2, 3, 4])
```

## 特殊矩阵

### zeros() ones() full()
```python
  In [30]:   # 传入元组表示各维度大小，此例中外层二维，中层三维，内层四维
           np.zeros((2, 3, 4))

  Out[30]:   array([[[0., 0., 0., 0.],
                   [0., 0., 0., 0.],
                   [0., 0., 0., 0.]],
　
                  [[0., 0., 0., 0.],
                   [0., 0., 0., 0.],
                   [0., 0., 0., 0.]]])


  In [31]:   np.ones((2, 1, 2))

  Out[31]:   array([[[1., 1.]],
                  [[1., 1.]]])

  In [32]:   # 传入元组表示各维度大小，10表示填充数值
           np.full((2,3), 10)

  Out[32]:   array([[10, 10, 10],
                  [10, 10, 10]])

  In [33]: np_input = [[1, 2], [3, 4], [5, 6]]
           np.full((2, 2, 3, 2), np_input)

  out[33]:   array([[[[1, 2],
                    [3, 4],
                    [5, 6]],

                   [[1, 2],
                    [3, 4],
                    [5, 6]]],

                  [[[1, 2],
                    [3, 4],
                    [5, 6]],

                   [[1, 2],
                    [3, 4],
                    [5, 6]]]])
```
### zeros_like() ones_like() full_like()
```python
  In [34]:   arr = [[1, 2], [3, 4]] # 给定的矩阵
             np.zeros_like(arr)

  Out[34]:   array([[0, 0],
                  [0, 0]])

  In [35]:   np.ones_like(arr)

  Out[35]:   array([[1, 1],
                  [1, 1]])

  In [36]:   np.full_like(arr, [100, 200])

  Out[36]:   array([[100, 200],
                  [100, 200]])
```
### eye()
```python
  In [37]:   np.eye(3) # 3×3的单位矩阵

  Out[37]:   array([[1., 0., 0.],
                    [0., 1., 0.],
                    [0., 0., 1.]])

  In [38]:   np.eye(3, k=1) #当k≥1时，主对角线上的元素1向上移动k个单位。

  Out[38]:   array([[0., 1., 0.],
                    [0., 0., 1.],
                    [0., 0., 0.]])
```
## 随机数组
### uniform() rand()
使用uniform(a,b,size)能够生成服从U[a,b]且数组维度为size的均匀分布的数组：
```python
  In [39]:   np.random.uniform(-1, 2, 3) # 一维

  Out[39]:   array([ 0.59014468, -0.86907229, -0.21060346])

  In [40]:   np.random.uniform(-1, 2, (3, 3)) # 二维

  Out[40]:   array([[ 0.21562771,  1.79108142, -0.6553781 ],
                  [-0.36814731, -0.12093076,  0.51697754],
                  [-0.00473059, -0.18388791, -0.50126406]]) 
```
特别地，能够使用rand(d1,d2,...,di)来生成服从U [0,1]的均匀分布的数组，其中di是相应维度的维数。
```python
  In [41]:   np.random.rand(3) # 一维

  Out[41]:   array([0.47851726, 0.18083504, 0.75219483])

  In [42]:   np.random.rand(3, 3) # 二维

  Out[42]:   array([[0.04386193, 0.58884209, 0.33904954],
                  [0.33108071, 0.71849998, 0.03185896], 
                  [0.1269714 , 0.1664075 , 0.26441228]])
```
### normal() randn()
使用normal()能够生成服从N[µ,σ]的正态分布的数组：
```python
  In [43]:   mu, sigma = 3, 2.5
           np.random.normal(mu, sigma, 3) # 一维

  Out[43]:   array([3.73778133, 8.41300871, 5.04849452])

  In [44]:   np.random.normal(mu, sigma, (3, 3)) # 二维

  Out[44]:   array([[5.42932796,  2.78689662, -1.45657554],
                  [6.06735553, -0.675319  ,  4.84657806],
                  [6.54138637,  0.21483743,  3.04411302]]) 
```

  特别地，能够使用randn(d1,d2,…)来生成服从N[0,1]的标准正态分布的数组：
```python
  In [45]:   np.random.randn(3) # 一维

  Out[45]:   array([-1.38498657,   0.70879419, -0.31567701])

  In [46]:   np.random.randn(3, 3) # 二维

  Out[46]:   array([[-1.24285359,  0.08074262, -0.127839  ],
                  [-1.16307485, -0.80570142, -0.66898915],
                  [1.12926891 , -0.684949  ,  0.97122595]])
```
### randint()
使用randint()可以生成在给定整数范围内的呈离散均匀分布的数组，其中参数high对应的整数值不包含在内：
```python
  In [47]:   low, high, size = 5, 15, (3, 3) # 生成5到14的随机整数

  In [48]:   np.random.randint(low, high, size)

  Out[48]:   array([[ 5, 6, 13],
                  [10, 9,  7],
                  [14, 9,  5]])”
```

### choice() permutation()
使用choice()可以从给定的列表中，以一定概率和方式抽取元素，当不指定概率时为等概率抽样。默认抽样方式为有放回抽样，此时replace=True，即同一个元素可能会被重复抽取：
```python
  In [49]:   my_list = ['a', 'b', 'c', 'd']

  In [50]:   np.random.choice(
               my_list,
               3,
               replace=False, 
               p=[0.1,0.7,0.1,0.1] 
           )# 结果一定不同
           # <U1类型指不超过一个字符长度的字符串

  Out[50]:   array(['b', 'c', 'd'], dtype='<U1')

  In [51]:   # 此时，指定replace=False报错
           np.random.choice(my_list, (3, 3))

  Out[51]:   array([['d', 'a', 'c'], 
                  ['b', 'c', 'a'], 
                  ['b', 'd', 'c']],dtype='<U1') 
```

当返回的元素个数与原列表的元素个数相同时，不放回抽样等价于使用permutation()函数，即打散原列表：
```python
  In [52]:   np.random.permutation(my_list)

  Out[52]:   array(['b', 'd', 'c', 'a'], dtype='<U1')
```
# Numpy数组的变形
## 1．由元素组织方式变化导致的变形
### transpose() T操作
```python
  In [55]:   carbon = np.random.rand(360, 180, 3)  # 这里用随机数字替代
           res = carbon.transpose(2, 0, 1)       # 最后一个维度前置，其他维度向后顺移
           res.shape

  Out[55]:   (3, 360, 180)

  In [56]:   res = carbon.T # 等价于carbon.transpose(2, 1, 0)
           res.shape

  Out[56]:   (3, 180, 360)
```
### reshape()
```python
  In [58]:   my_matrix = np.arange(8) # 默认初始值为0，步长为1
           my_matrix.reshape(2, 4)

  Out[58]:   array([[0, 1, 2, 3],
                  [4, 5, 6, 7]])

  In [59]:   my_matrix.reshape((2, 4), order="C") #默认以行的顺序或内层优先的顺序

  Out[59]:   array([[0, 1, 2, 3],
                  [4, 5, 6, 7]])

  In [60]:   my_matrix.reshape((2, 4), order="F") #以列的顺序填充

  Out[60]:   array([[0, 2, 4, 6],
                  [1, 3, 5, 7]])
```
### expend_dims() squeeze()
```python
  In [66]:   target = np.ones((2, 2))
           expanded_target = np.expand_dims(target, (0, 2)) # 在第1和第3维度插入
           expanded_target.shape

  Out[66]:   (1, 2, 1, 2)

  In [67]:   np.squeeze(expanded_target, (0, 2)) # 压缩第1和第3维度

  Out[67]:   array([[1., 1.],
                  [1., 1.]])
```
## 2．由合并或拆分导致的变形
### 合并函数 stack() concatenate()
stack()会产生新的维度而concatenate()不会，同时 stack()拼接的数组的维度必须完全一致，concatenate()只需在非拼接的维度上匹配。
```python
  In [68]: pop_man = np.random.rand(20, 6, 3)                    # 男性（随机生成）
           pop_women = np.random.rand(20, 6, 3)                  # 女性（随机生成）
           res = np.stack([pop_man, pop_women], axis=2)          # 新维度为新数组的第三维
           res.shape

  Out[68]:   (20, 6, 2, 3)
```
```python
  In [69]:   pop_1_6 = np.random.rand(20, 6, 3)                     # 1月～6月
           pop_7_12 = np.random.rand(20, 6, 3)                    # 7月～12月
           res = np.concatenate([pop_1_6, pop_7_12], axis=1)      # 在月份维度拼接
           res.shape

  Out[69]:   (20, 12, 3)
```
### 分割函数 split()
```python
  In [71]:   res = np.split(pop_1_6, indices_or_sections=3, axis=1)
           for i in res:
               print(i.shape)

  Out[71]:   (20, 2, 3)
           (20, 2, 3)
           (20, 2, 3)

  In [72]:   res = np.split(pop_1_6, indices_or_sections=[1, 4], axis=1)
           for i in res:
               print(i.shape)

  Out[72]:   (20, 1, 3)
           (20, 3, 3)
           (20, 2, 3)
```
### repeat()
```python
  In [73]:   a = np.arange(3) # [0,1,2]
           np.repeat(a, repeats=2) # 每个元素重复两遍

  Out[73]:   array([0, 0, 1, 1, 2, 2])

  In [74]:   np.repeat(a, repeats=a+1) # 第i个元素重复i遍

  Out[74]:   array([0, 1, 1, 2, 2, 2])
```
对二维数组而言，还可以指定发生重复的维度：
```python
  In [75]:   np.array([[1,2],[3,4]])
           np.repeat(a, repeats=[1,2], axis=1) # 在最内层维度，第i个元素重复i遍

  Out[75]:   array([[1, 2, 2],
                 [3, 4, 4]])

  In [76]:   # 在最外层维度，第i个元素重复i遍，注意这里的元素也是一个NumPy数组
           np.repeat(a, repeats=[1,2], axis=0)

  Out[76]:   array([[1, 2],
                  [3, 4],
                  [3, 4]])
```
## NumPy数组的切片
### 利用[]
```python
  In [77]:   target = np.arange(24).reshape(4, 2, 3)
           target

  Out[77]:   array([[[ 0,  1,  2],
                   [ 3,  4,  5]],

                  [[ 6,  7,  8],
                   [ 9, 10, 11]],

                  [[12, 13, 14],
                   [15, 16, 17]],

                  [[18, 19, 20],
                   [21, 22, 23]]])

  In [78]:   target[0:3, :, 1:3]

  Out[78]:   array([[[ 1,  2],
                   [ 4,  5]],

                  [[ 7,  8],
                   [10, 11]], 

                  [[13, 14],
                   [16, 17]]])
```
当[]符号对应维度的输入值都是长度相同的列表（或一维NumPy数组）时，其返回的结果并不是子数组，此时输入值分别代表元素在各个维度的索引值，例如下面的例子等价于取出target[0,0,0]和target[1,1,1]，而不是一个大小为2×2×2的子数组：
```python
  In [79]:   target[[0, 1], [0, 1], [0, 1]] # 单个元素逐个取出

  Out[79]:   array([ 0, 10])

  In [80]:   target[0:2, 0:2, 0:2]          # 维度为2×2×2的子数组

  Out[80]:   array([[[0,  1],
                   [3,  4]],

                  [[6,  7], 
                   [9, 10]]]) 
```
上述代码中的0:3等价于slice(0,3,1)，即起始点为0，终止点为3（不包含3），步长为1。一般的slice对象若具有slice(p,q,r)的形式，则等价于p:q:r，当p为0时可省略p，当q为对应数组维度的维数时可省略q，当步长为1时可省略:r。

在实际使用中，利用布尔数组进行切片也是一种常见操作，其使用方法为：若要保留某一个维度的若干维数，只需传入相应位置为True且其余位置为False的布尔数组。例如，保留外层维度中维数为0和2的子数组：
```python
  In [81]:   target[[True, False, True, False], :, :]

  Out[81]:   array([[[ 0,  1,  2],
                   [ 3,  4,  5]], 

                  [[12, 13, 14], 
                   [15, 16, 17]]]) 
```
对于连续多个出现在最后几个维度的全体切片“:”，可以省略：
```python
  In [82]:   target[[True, False, True, False]].shape

  Out[82]:   (2, 2, 3)
```
  对于连续多个出现在最初几个维度的全体切片“:”，可以使用...来简写：
```python
  In [83]:   target[:, :, 0::2].shape == target[..., 0::2].shape

  Out[83]:   True
```
最后来介绍np.newaxis，它结合切片可以完成与expand_dims()类似的操作，每一个全体切片“:”代表原数组中的一个维度：
```python
  In [84]:   target[:, np.newaxis, np.newaxis].shape # 后两个“:”省略，第一个“:”对应最外层

  Out[84]:   (4, 1, 1, 2, 3)

  In [85]:   target[:, np.newaxis, :, np.newaxis].shape # 最内层的“:”省略

  Out[85]:   (4, 1, 2, 1, 3)

  In [86]:   # 通过“...”省略前两个维度，用最后一个“:”匹配最内层维度
           target[..., np.newaxis, :, np.newaxis].shape  

  Out[86]:   (4, 2, 1, 3, 1)
  ```
  ## 广播机制、round()
  ```python
  In [87]:   # 假设顾客购买某件商品的件数为0～4 
           piece = np.random.randint(0, 5, (5, 3))
           # 商品价格为10～100元
           price = np.random.uniform(10, 100, 3)
           # 使用round()函数能够保留n位小数，此处保留两位
           price = price.round(2)

  In [88]:   res = np.array([i*price for i in piece])
           res

  Out[88]:   array([[ 44.09,  90.85, 193.56],
                  [ 88.18, 363.4 , 387.12],
                  [132.27, 363.4 , 193.56],
                  [ 44.09,   0.  , 193.56],
                  [ 88.18,  90.85, 193.56]])
```
在NumPy中，只有维度完全一样的数组才能进行逐元素运算，即标量之间的直接运算，包括但不限于加、减、乘、除、整除、取余数。正如上面例子的循环中的i*price，*两侧都是大小一样的数组，因此这些数组之间的乘法就是逐元素的乘法，结果的维度与原来数组的维度一致。
 ```python
  In [89]:   piece * price

  Out[89]:   array([[ 44.09,  90.85, 193.56],
                  [ 88.18, 363.4 , 387.12],
                  [132.27, 363.4 , 193.56],
                  [ 44.09,   0.  , 193.56],
                  [ 88.18,  90.85, 193.56]])
```
广播的一般规则，假设数组A的维度为dp * ... * d1，数组B的维度为dq * ... * d1，设r = max(p,q)。当p≥q时，数组B需要在的维度前补充p−q个维度，相应的维数都为1；当q≥p时，数组A需要在的维度前补充q−p个维度，相应的维数同样都为1。在补充维度后，重记数组A的维度为dr *...* d1，数组B的维度为dr *...* d1。此时依次比较各个维度的值，对维度i而言，当dAi = dBi时，数组保持不变；当dAi != dBi且dAi=1时，A将沿着该维度进行复制，将该维度大小扩充至dBi；当dAi != dBi且dBi=1时，B将沿着该维度进行复制，将该维度大小扩充至dAi；当上述3种情况都不满足时，报错。

piece数组的维度为5×3，price数组的维度为3，根据维度数量相同的规则，price数组的维度将会被变形为1×3，即在最外层维度前补充维数为1的维度，从而与piece数组对齐。接着依次比较各个维度的维数：首先看外层，由于piece外层维度不等于price外层维度且price外层维度=1，与规则中的第三种情况相符合，因此将price数组沿着该维度重复5次；接着看内层，由于两者相等，故数组保持不变。此时两个数组的大小都为5×3，可以使用逐元素的运算。这个简单的例子就反映了广播的整个过程，其中包括的两个子过程分别为维度对齐与维数比较。

##  常用函数
### 计算函数
max()、min()、mean()、median()、std()、var()、sum()和quantile()，它们分别计算最大值、最小值、平均值、中位数、标准差、方差、总和和分位数。
```python
  In [92]:   my_matrix = np.random.randint(20, 40, 24).reshape(2, 3, 4)
           # 维度分别代表学校、年级、班级
           my_matrix

  Out[92]:   array([[[21, 25, 37, 31],
                   [39, 32, 36, 34],
                   [37, 27, 31, 24]],

                  [[37, 38, 22, 33],
                   [30, 33, 26, 33],
                   [20, 22, 21, 35]]])

  In [93]:   my_matrix.sum()

  Out[93]:   724
  ```
计算某一个或某几个维度的值，例如计算每个学校的学生总数，可以使用参数axis
```python
  In [94]:   my_matrix.sum(axis=(1, 2)) # 把年级和班级的维度聚合起来，只保留学校

  Out[94]:   array([374, 350])
```

类似地，我们可以分别计算各学校所有班级中最大的学生总数：
```python
  In [96]:   my_matrix.max(axis=(1, 2)) 

  Out[96]:   array([39, 38])
```
median()函数与quantile()函数并不能直接通过数组调用，而必须使用np.median()与np.quantile()来实现，并且后者具有参数q表示分位数。

```python
  In [97]:   np.quantile(my_matrix, axis=-1, q=0.5) # -1表示倒数第一维

  Out[97]:   array([[28. , 35. , 29. ],
                  [35. , 31.5, 21.5]])

  In [98]:   np.median(my_matrix, axis=-1)
           # 由于quantile()的分位数为0.5，因此结果一致

  Out[98]:   array([[28. , 35. , 29. ],
                  [35. , 31.5, 21.5]])”
```

如果在数组中聚合的维度具有缺失值，那么结果中的对应维度也会被设为缺失值。如果想要忽略缺失值进行计算，可以使用以nan开头的聚合函数，例如max()可被替换为nanmax()：
```python
  In [99]:   my_matrix = my_matrix.astype("float")
           # np.nan是一种特殊的浮点类型，故在赋值前需要更改数组类型
           # 关于数据中缺失值的各种性质与处理方法将在第7章中讨论
           my_matrix[0][0][0] = np.nan
           my_matrix.max(axis=(1, 2))

  Out[99]:   array([nan, 38.])

  In [100]:   np.nanmax(my_matrix, axis=(1, 2))

  Out[100]:   array([39., 38.])
```
- 注解：一般而言，NumPy数组中的元素必须是统一类型的，常用类型包括int32(int)、int64、float32、float64(float)和bool。正如上文给出的例子，类型之间的转换可以通过astype完成。其中，bool的转换规则为：原数组位置元素等于0时设为False，否则设为True。

## 逻辑函数
```python
  In [106]:   salary = np.array([[9000, 11000], [11000, 12000]])
            (salary>10000).all(axis=1)

  Out[106]:   array([False,   True])

  In [107]:   (salary>10000).any(axis=1)

  Out[107]:   array([ True,  True])
```
一般来说，NumPy有以下几种常见的生成逻辑数组的方式：

  ● 通过比较运算符，如<、>、<=、>=、!=和==等；

  ● 通过内置函数，如isnan()，isinf()等，当然也包括上面介绍的any()和all()；

  ● 通过逻辑运算符，这里主要指或运算符|、与运算符&以及非运算符～。

这里我们举一个例子来说明逻辑运算符的使用方法。假设现在随机构造一个1000×3矩阵，其中的元素服从[0,1]的均匀分布，筛选出行元素之和不超过1.5或者行内第一个元素与第三个元素都超过0.5的所在行。
```python
  In [108]:   my_array = np.random.rand(1000, 3)
            condition1 = ～(my_array.sum(1)>1.5)
            condition2 = (my_array[:, 0]>0.5)&(my_array[:, 2]>0.5))
            res = my_array[condition1|condition2, :]
```


  下面进行筛选结果的验证：
```python
  In [109]:   ((res.sum(1)<=1.5)|(res[:, [0, 2]]>0.5).all(1)).all()

  Out[109]:   True
```
### where()
np.where(bool_array, fill_array_for_true, fill_array_for_false)
```python
  In [110]:   a = np.arange(4).reshape(-1, 2)
            a

  Out[110]:   array([[0, 1],
                   [2, 3]])

  In [111]:   np.where(a>1, a.sum(0), a.sum(1))

  Out[111]:   array([[1, 5],
                   [2, 4]])
```
where()的第一个参数是一个逻辑数组，其中的元素都为布尔值，第二个与第三个参数是待填充的值。从图1.1中可以看到，如果后两个元素与第一个数组维度不匹配且符合广播条件，则会被广播至相应维度，输出的数组与逻辑数组的大小一致。如果布尔值为True，则用fill_array_for_true的相应位置值填充；如果布尔值为False，则用fill_array_for_false的相应位置值填充。

## 索引函数

本部分提及的索引函数并不是用来进行索引的函数，而是返回值为索引的函数，它们包括nonzero()、argmax()和argmin()。其中，nonzero()返回非零数的索引，argmax()、argmin()分别返回最大数和最小数的索引。
```python
  In [112]:   a = np.array([0,-5,0,1,3,-1])
            np.nonzero(a)

  Out[112]:   (array([1, 3, 4, 5], dtype=int64),)

  In [113]:   a.argmax()

  Out[113]:   4

  In [114]:   a.argmin()

  Out[114]:   1
```
