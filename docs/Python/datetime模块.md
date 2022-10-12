time 模块用于取得 Unix 纪元时间戳并加以处理。但是如果要更方便的格式显示日期，或者对日期进行算术运算，应该使用 datetime 模块。

datetime 模块主要包括以下类：

|   类名    |             功能说明             |
| :-------: | :------------------------------: |
| datetime  |           日期时间对象           |
|   date    |             日期对象             |
|   time    |             时间对象             |
| timedelta | 时间间隔，即两个时间点之间的长度 |

### datetime

datetime 模块中有自己的 datetime 类，它表示一个特定的时刻。datetime 类可以看做是 date 和 time 的合体。

```python
>>> import datetime
>>> datetime.datetime.now()
datetime.datetime(2022, 10, 12, 21, 12, 7, 215172)
>>> dt = datetime.datetime(2022, 3, 17, 18, 0, 0)
>>> dt.year, dt.month, dt.day
(2022, 3, 17)
>>> dt.hour, dt.minute, dt.second
(18, 0, 0)
```

Unix 纪元时间戳可以通过 `datetime.datetime.fromtimestamp()` 转换为 datetime 对象。该转换将根据本地时区转换。

```python
>>> import time
>>> datetime.datetime.fromtimestamp(0)
datetime.datetime(1970, 1, 1, 8, 0)
>>> datetime.datetime.fromtimestamp(time.time())
datetime.datetime(2022, 10, 12, 21, 14, 16, 698691)
```

datetime 对象可以直接用比较操作符进行比较，时间更靠后的 datetime 值更大。

### timedelta

timedelta 数据类型表示一段时间，而不是一个时刻。

```python
>>> delta = datetime.timedelta(days=11, hours=10, minutes=9, seconds=8)
>>> delta.days, delta.seconds, delta.microseconds
(11, 36548, 0)
>>> delta.total_seconds()
986948.0
>>> str(delta)
'11 days, 10:09:08'
```

delta 接受关键字 `weeks, days, hours, minutes, seconds, milliseconds, microseconds`，但是没有 `year, month` 关键字。在内部，它使用 `days, seconds, microseconds` 来保存数据。

`total_seconds()` 返回以 `second` 为单位的时间。

算术运算符可以用于对 datetime 类型的值进行日期运算。

```python
>>> dt = datetime.datetime.now()
>>> thousandDays = datetime.timedelta(days=1000)
>>> dt + thousandDays
datetime.datetime(2025, 7, 8, 21, 21, 11, 351880)
>>> dt + thousandDays / 2
datetime.datetime(2024, 2, 24, 21, 21, 11, 351880)
```

### 将 datetime 对象转换为字符串

Unix 纪元时间戳和 datetime 对象对人类来说都不是很友好可读。利用 strftime() 方法，可以将 datetime 对象显示为字符串。（ `f` 表示 format）。

```python
>>> oct12st = datetime.datetime(2022, 10, 12, 21, 31, 0)
>>> oct12st.strftime('%Y-%m-%d %H:%M:%S')
'2022-10-12 21:31:00'
>>> oct12st.strftime('%I:%M %P')
'09:31 P'
>>> oct12st.strftime('%I:%M %p')
'09:31 PM'
>>> oct12st.strftime('%B of \'%y')
"October of '22"
```

| strftime 指令 | 含义                                |
| ------------- | ----------------------------------- |
| %Y            | 带世纪的年份，2022                  |
| %y            | 不带世纪的年份，00 ~ 99             |
| %m            | 数字表示的月份，01 ~ 22             |
| %B            | 完整的月份，'November'              |
| %b            | 简写的月份，'Nov'                   |
| %d            | 一月中的第几天，01 ~ 31             |
| %j            | 一年中的第几天，001 ~ 366           |
| %w            | 一周中的第几天，0(周日) ~ 6（周六） |
| %A            | 完整的周几，'Monday'                |
| %a            | 简写的周几，'Mon'                   |
| %H            | 小时（24小时），00 ~ 23             |
| %I            | 小时（12小时），01 ~ 12             |
| %M            | 分钟，00 ~ 59                       |
| %S            | 秒，00 ~ 59                         |
| %p            | 'AM' 或 'PM'                        |
| %%            | 转义字符 %                          |

与之相反的，strptime() 可以将字符串解析为 datetime 对象。（p 表示 parse）。

```python
>>> datetime.datetime.strptime('October 12, 2022', '%B %d, %Y')
datetime.datetime(2022, 10, 12, 0, 0)
```

### 参考资料

- [Python datetime模块详解、示例 [CSDN]](https://blog.csdn.net/cmzsteven/article/details/64906245)
- 《Python 编程快速上手》第 15 章