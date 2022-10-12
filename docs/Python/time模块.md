内置的 time 模块可以读取系统时钟的当前时间。在 time 模块中，`time.time()` 和 `time.sleep()` 是最有用的模块。除此之外，`time.mktime()` 也很有用。

### time.time()

返回自 1970 年 1 月 1 日 0 点以来的秒数，是一个浮点数，被称为 UNIX 纪元时间戳。例如：

```python
>>> import time
>>> time.time()
1665579384.791162
```

1665579384.791162 是 2020 年 10 月 12 日晚上 20 点 56 分的时间戳。

### time.sleep() 

该函数可以使程序暂停，参数单位是秒。

该函数会阻塞，直到结束才会继续运行后面的程序。

一个秒表程序示例如下：

=== "超级秒表"
    ```python linenums="1"
    #! python3
    # stopwatch.py - A simple stopwatch program.

    import time

    # Display the program's instructions
    print('Press ENTER to begin. Afterwards, press ENTER to "click" the stopwatch. Press Ctrl-C to quit.')
    input()                     # press ENTER to begin
    print('Started.')           # get the first lap's start time
    startTime = time.time()
    lastTime = startTime
    lapNum = 0

    # Start tracking the lap times.
    try:
        while True:
            input()
            lapTime = round(time.time() - lastTime, 2)
            totalTime = round(time.time() - startTime, 2)
            print('Lap #%s: %s (%s)' % (lapNum, totalTime, lapTime), end='')
            lapNum += 1
            lastTime = time.time() # reset the last lap time
    except KeyboardInterrupt:
        # Handle the Ctrl-C exception to keep its error message from displaying
        print('\nDone')
    ```

### time.mktime()

待更新[2022-10-12]
