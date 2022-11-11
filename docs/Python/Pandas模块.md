Pandas 是基于 NumPy 的一种数据处理工具，该工具为了解决数据分析任务而创建。Pandas 纳入了大量库和一些标准的数据模型，提供了高效地操作大型数据集所需的函数和方法。

Pandas 的数据结构：Pandas 主要有 Series（一维数组），DataFrame（二维数组），Panel（三维数组），Panel4D（四维数组），PanelND（更多维数组）等数据结构。其中 Series 和 DataFrame 应用的最为广泛。

 - Series 是一维带标签的数组，它可以包含任何数据类型。包括整数，字符串，浮点数，Python 对象等。Series 可以通过标签来定位。

  - DataFrame 是二维的带标签的数据结构。我们可以通过标签来定位数据。这是 NumPy 所没有的。

## 基础部分

```python linenums="1"
import pandas as pd
import numpy as np

# 查看版本
print(pd.__version__)
```

### Series
```python linenums="1"
# 创建 Series
## 1. 从列表创建
arr = [0, 1, 2, 3, 4]
s1 = pd.Series(arr)  # 如果不指定索引，则默认从 0 开始

## 2. 从 Ndarray 创建
n = np.random.randn(5)  # 创建一个随机 Ndarray 数组
index = ['a', 'b', 'c', 'd', 'e']
s2 = pd.Series(n, index=index) # 指定 index

## 3. 从字典创建 Series
d = {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}  # 定义示例字典
s3 = pd.Series(d)

# Series 基本操作
## 1. 修改索引
s1.index = ['A', 'B', 'C', 'D', 'E']

## 2. 纵向拼接
s4 = s3.append(s1)

## 3. 指定索引删除元素
s4 = s4.drop('e')

## 4. 指定索引修改元素
s4['A'] = 6

## 5. 切片操作
s4[:3]

# Series 运算
## 以下都是按照索引对应计算，如果不存在则填充为 NaN
s4.add(s3)
s4.sub(s3)
s4.mul(s3)
s4.div(s3)

s4.median()
s4.sum()
s4.max()
s4.min()
```

### DataFrame
```python linenums="1"
# 创建 DataFrame 数据类型
## 1. 通过 numpy 数组创建
dates = pd.date_range('today', periods = 6)
num_arr = np.random.randn(6, 4)
columns = ['A', 'B', 'C', 'D']
df1 = pd.DataFrame(num_arr, index = dates, columns = columns)

## 2. 通过字典数组创建 DataFrame，字典的 key 将作为列，value 长度必须相同
data = {'animal': ['cat', 'cat', 'snake', 'dog', 'dog', 'cat', 'snake', 'cat', 'dog', 'dog'],
        'age': [2.5, 3, 0.5, np.nan, 5, 2, 4.5, np.nan, 7, 3],
        'visits': [1, 3, 2, 3, 2, 3, 1, 1, 2, 1],
        'priority': ['yes', 'yes', 'no', 'yes', 'no', 'no', 'no', 'yes', 'no', 'no']}

labels = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']
df2 = pd.DataFrame(data, index=labels)

## 查看数据类型
df2.dtypes

# DataFrame 基本操作

## 1. 查看前 5 行
df2.head()

## 2. 查看后 3 行
df2.tail(3)

## 3. 查看 index
df2.index

## 4. 查看 columns
df2.columns

## 5. 查看 dataframe 的数值
df2.values

## 6. 查看统计数据
df2.describe()

## 7. 转置
df2.T

## 8. 对 DataFrame 进行按列排序
df2.sort_values(by = 'age')

## 9. 对 DataFrame 数据切片
df2[1:3]

## 10. 通过便签查询（单列）
df2['age']
df2.age
df2[['age', 'animal']]

## 11. 通过位置查询
df2.iloc[1:3]

## 12. 副本拷贝
df3 = df2.copy()

## 13. 判断 DataFrame 元素是否为空
df3.isnull()

## 14. 添加列数据
num = pd.Series([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], index = df2.index)
df3['No.'] = num

## 15. 根据下标值进行更改
df3.iat[1, 1] = 2

## 16. 根据标签对数据进行修改
df3.loc['f', 'age'] = 1.5

## 17. 求平均值
df3.mean()

## 18. 对任意列做求和操作
df3['visits'].sum()
```

### 字符串操作

```python linenums="1"
## 将字符串转换为小写字母
string = pd.Series(['A', 'B', 'C', 'Aaba', 'Baca',
                    np.nan, 'CABA', 'dog', 'cat'])
print(string)
string.str.lower()

## 将字符串转换为大写字母
string.str.upper()
```

### DataFrame缺失值操作

```python linenums="1"
## 对缺失值进行填充
df4 = df3.copy()
df4.fillna(value=3)

## 删除存在缺失值的行
df5 = df3.copy()
df5.dropna(how='any')

## DataFrame 按指定列对齐，类似于 join 操作
left = pd.DataFrame({'key': ['foo1', 'foo2'], 'one': [1, 2]})
right = pd.DataFrame({'key': ['foo2', 'foo3'], 'two': [4, 5]})

# 按照 key 列对齐连接，只存在 foo2 相同，所以最后变成一行
pd.merge(left, right, on='key')
```

### DataFrame 文件操作

```python linenums="1"
## csv 文件写入
df3.to_csv('animal.csv')

## csv 读入
df_animal = pd.read_csv('animal.csv')

## excel 写入
df3.to_excel('animal.xlsx', sheet_name = 'Sheet1')

## excel 读入
pd.read_excel('animal.xlsx', 'Sheet1', index_col = None, na_values = ['NA'])
```


## 进阶部分

### 时间序列索引

```python linenums="1"
## 建立一个以 2018 年每一天为索引，值为随机数的 Series
dti = pd.date_range(start = '2018-01-01', end = '2018-12-31', freq = 'D')
s = pd.Series(np.random.rand(len(dti)), index = dti)

## 统计 s 中每一个周三对应值的和
s[s.index.weekday == 2].sum()

## 统计 s 中每个月的平均值
s.resample('M').mean()

## 将 Series 中的时间进行转换（秒转分钟）
s = pd.date_range('today', periods = 100, freq = 'S')
ts = pd.Series(np.random.randint(0, 500, len(s)), index = s)

ts.resample('Min').sum()

## UTC 世界时间标准
s = pd.date_range('today', periods = 1, freq = 'D')
ts = pd.Series(np.random.randn(len(s)), s)
ts_utc = ts.tz_localize('UTC')

## 转为上海所在时区
ts_utc.tz_convert('Asia/Shanghai')

## 不同时间表示方式的转换
rng = pd.date_range('1/1/2018', periods = 5, freq = 'M')
ts = pd.Series(np.random.randn(len(rng)), index = rng)
ps = ts.to_period()
ps.to_timestamp()
```

### Series 多重索引

```python linenums="1"
## 创建多重索引 Series
letters = ['A', 'B', 'C']
numbers = list(range(10))
mi = pd.MultiIndex.from_product([letters, numbers])
s = pd.Series(np.random.rand(30), index = mi)

## 多重索引 Series 查询
s.loc[:, [1, 3, 6]]

## 多重索引 Series 切片
s.loc[pd.IndexSlice[:'B', 5:]]
```

### DataFrame 多重索引

```python linenums="1"
frame = pd.DataFrame(np.arange(12).reshape(6, 2),
                     index=[list('AAABBB'), list('123123')],
                     columns=['hello', 'world'])

## 索引名称设置
frame.index.names = ['first', 'second']

## DataFrame 多重索引分组求和
frame.groupby('first').sum()
```

## 其他
```python linenums="1"
## DataFrame 行列名称互换
frame.stack()

## 条件查询
data = {'animal': ['cat', 'cat', 'snake', 'dog', 'dog', 'cat', 'snake', 'cat', 'dog', 'dog'],
        'age': [2.5, 3, 0.5, np.nan, 5, 2, 4.5, np.nan, 7, 3],
        'visits': [1, 3, 2, 3, 2, 3, 1, 1, 2, 1],
        'priority': ['yes', 'yes', 'no', 'yes', 'no', 'no', 'no', 'yes', 'no', 'no']}

labels = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']
df = pd.DataFrame(data, index=labels)

### 查找 age 大于 3 的全部信息
df[df['age'] > 3]


## 根据行列索引切片
df.iloc[2:4, 1:3]

## 多重条件查询
df = pd.DateFrame(data, index = labels)
df[(df['animal'] == 'cat') & (df['age'] < 3)]

## DataFrame 按关键字查询：

df3[df3['animal'].isin(['cat', 'dog'])]

## DataFrame 按标签及列名查询
df.loc[df2.index[[3, 4, 8]], ['animal', 'age']]

## DataFrame 的多条件排序
df.sort_values(by = ['age', 'visits'], ascending = [False, True])

## 多值替换
df['priority'].map({'yes': True, 'no': False})

## DataFrame 分组求和
df.groupby('animal').sum()

## 使用列表拼接多个 DataFrame
temp_df1 = pd.DataFrame(np.random.randn(5, 4))  # 生成由随机数组成的 DataFrame 1
temp_df2 = pd.DataFrame(np.random.randn(5, 4))  # 生成由随机数组成的 DataFrame 2
temp_df3 = pd.DataFrame(np.random.randn(5, 4))  # 生成由随机数组成的 DataFrame 3

pieces = [temp_df1, temp_df2, temp_df3]
pd.concat(pieces)

## 找到 DataFrame 表中和最小的列
df = pd.DataFrame(np.random.random(size=(5, 10)), columns=list('abcdefghij'))
print(df)
df.sum().idxmin()  # idxmax(), idxmin() 为 Series 函数返回最大最小值的索引值

## DataFrame 中每个元素减去每一行的平均值
df = pd.DataFrame(np.random.random(size=(5, 3)))
df.sub(df.mean(axis = 1), axis = 0)

## DataFrame 分组，并得到每一组最大的三个数之和：
df = pd.DataFrame({'A': list('aaabbcaabcccbbc'),
                   'B': [12, 345, 3, 1, 45, 14, 4, 52, 54, 23, 235, 21, 57, 3, 87]})
print(df)
df.groupby('A')['B'].nlargest(3).sum(level=0)
```

### 透视表

当分析庞大的数据时，为了更好的发掘数据特征之间的关系，且不破坏原数据，就可以利用透视表 pivot_table 进行操作。

```python linenums="1"
df = pd.DataFrame({'A': ['one', 'one', 'two', 'three'] * 3,
                   'B': ['A', 'B', 'C'] * 4,
                   'C': ['foo', 'foo', 'foo', 'bar', 'bar', 'bar'] * 2,
                   'D': np.random.randn(12),
                   'E': np.random.randn(12)})

pd.pivot_table(df, index=['A', 'B'])

## 按指定列聚合：
### 将该 DataFrame 的 D 列聚合，按照 A,B 列为索引进行聚合，聚合方式默认为求均值
pd.pivot_table(df, values = ['D'], index = ['A', 'B'])

### aggfunc 可以指定聚合函数
pd.pivot_table(df, values = ['D'], index = ['A', 'B'], aggfunc = [np.sum, len])

### 利用额外列进行辅助分割
pd.pivot_table(df, values = ['D'], index = ['A', 'B'], columns = ['C'], aggfunc = np.sum)

### 缺失值处理
pd.pivot_table(df, values=['D'], index=['A', 'B'], columns=['C'], aggfunc=np.sum, fill_value=0)
```

### 绝对类型

在数据的形式上主要包括数量型和性质型，数量型表示着数据可数范围可变，而性质型表示范围已经确定不可改变，绝对型数据就是性质型数据的一种。

```python linenums="1"
df = pd.DataFrame({"id": [1, 2, 3, 4, 5, 6], "raw_grade": [
                  'a', 'b', 'b', 'a', 'a', 'e']})
df["grade"] = df["raw_grade"].astype("category")

## 对绝对型数据进行重命名
df["grade"].cat.categories = ["very good", "good", "very bad"]

## 重新排列绝对型数据并补充相应的缺省值
df["grade"] = df["grade"].cat.set_categories(["very bad", "bad", "medium", "good", "very good"])

## 对绝对型数据进行排序
df.sort_values(by = 'grade')

## 对绝对型数据进行分组
df.groupby('grade').size()
```

### 数据清洗

```python linenums="1"
## 缺失值拟合, 按照 10 增长：
df = pd.DataFrame({'From_To': ['LoNDon_paris', 'MAdrid_miLAN', 'londON_StockhOlm',
                               'Budapest_PaRis', 'Brussels_londOn'],
                   'FlightNumber': [10045, np.nan, 10065, np.nan, 10085],
                   'RecentDelays': [[23, 47], [], [24, 43, 87], [13], [67, 32]],
                   'Airline': ['KLM(!)', '<Air France> (12)', '(British Airways. )',
                               '12. Air France', '"Swiss Air"']})
df['FlightNumber'] = df['FlightNumber'].interpolate().astype(int)

## 数据列拆分
temp = df.From_To.str.split('_', expand=True)
temp.columns = ['From', 'To']

## 字符标准化
### 首字母大小等
temp['From'] = temp['From'].str.capitalize()
temp['To'] = temp['To'].str.capitalize()

## 删除坏数据，加入整理好的数据
df = df.drop('From_To', axis=1)
df = df.join(temp)

## 使用正则过滤字符数据
df['Airline'] = df['Airline'].str.extract('([a-zA-Z\s]+)', expand=False).str.strip()

## 格式规范
### 规定 RecentDelays 的长度为 3，拆成 3 项，缺失的用 NaN 补全
delays = df['RecentDelays'].apply(pd.Series)

delays.columns = ['delay_{}'.format(n)
                  for n in range(1, len(delays.columns)+1)]

df = df.drop('RecentDelays', axis=1).join(delays)

```

### 数据预处理

```python linenums="1"
## 信息区间划分
df = pd.DataFrame({'name': ['Alice', 'Bob', 'Candy', 'Dany', 'Ella',
                            'Frank', 'Grace', 'Jenny'],
                   'grades': [58, 83, 79, 65, 93, 45, 61, 88]})


def choice(x):
    if x > 60:
        return 1
    else:
        return 0


df.grades = pd.Series(map(lambda x: choice(x), df.grades))

## 数据去重
df = pd.DataFrame({'A': [1, 2, 2, 3, 4, 5, 5, 5, 6, 7, 7]})
df.loc[df['A'].shift() != df['A']]

## 数据归一化 (Min-Max 归一化)
def normalization(df):
    numerator = df.sub(df.min())
    denominator = (df.max()).sub(df.min())
    Y = numerator.div(denominator)
    return Y

df = pd.DataFrame(np.random.random(size=(5, 3)))
print(df)
normalization(df)
```

### Pandas 绘图操作

```python linenums="1"
## Series 可视化
ts = pd.Series(np.random.randn(100), index=pd.date_range('today', periods=100))
ts = ts.cumsum()
ts.plot()

## DataFrame 折线图
df = pd.DataFrame(np.random.randn(100, 4), index=ts.index,
                  columns=['A', 'B', 'C', 'D'])
df = df.cumsum()
df.plot()

## DataFrame 散点图
df = pd.DataFrame({"xs": [1, 5, 2, 8, 1], "ys": [4, 2, 1, 9, 6]})
df = df.cumsum()
df.plot.scatter("xs", "ys", color='red', marker="*")

## DataFrame 柱形图
df = pd.DataFrame({"revenue": [57, 68, 63, 71, 72, 90, 80, 62, 59, 51, 47, 52],
                   "advertising": [2.1, 1.9, 2.7, 3.0, 3.6, 3.2, 2.7, 2.4, 1.8, 1.6, 1.3, 1.9],
                   "month": range(12)
                   })

ax = df.plot.bar("month", "revenue", color="yellow")
df.plot("month", "advertising", secondary_y=True, ax=ax)
```