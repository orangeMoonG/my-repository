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
