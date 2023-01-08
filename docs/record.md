

<script src="https://cdn.jsdelivr.net/npm/echarts@5.4.0/dist/echarts.min.js"></script>

<!--  
 错误提交图标：:lady_beetle:  
-->

=== "LeetCode"

    <div id="main" style="width: 800px;height:300px;"></div>

    | Name      | rank | T1 | T2 | T3 | T4|
    | :---------- | :--- | :--- | :--- | :--- | :--- |
    | [第 327 场周赛](https://leetcode.cn/contest/weekly-contest-327/ranking/) | 79(185) | 0:01:06 | 0:04:09 | 0:12:45 :lady_beetle: 2 | 0:55:02 :lady_beetle: 1 |
    | [第 326 场周赛](https://leetcode.cn/contest/weekly-contest-326/ranking/) | 92(152) | 0:01:03 | 0:02:23  | 0:05:28 | 0:11:45 :lady_beetle: 1 |
    | [第 325 场周赛](https://leetcode.cn/contest/weekly-contest-325/ranking/) | 87(153) | 0:03:17 | 0:34:12 :lady_beetle: 3 |0:20:34 | 0:25:45 |
    | [第 324 场周赛](https://leetcode.cn/contest/weekly-contest-324/ranking/) | 625(1611) | 0:04:56 :lady_beetle: 1| 0:07:32 :lady_beetle: 1|| 0:27:47 :lady_beetle: 1 |
    | [第 323 场周赛](https://leetcode.cn/contest/weekly-contest-323/ranking/) | 41(82) | 0:02:12 | 0:06:02 | 0:30:07 | 0:24:57 |
    | [第 322 场周赛](https://leetcode.cn/contest/weekly-contest-322/ranking/) | 17(37) | 0:01:51 | 0:06:52 | 0:18:57 | 0:30:56 |
    | [第 321 场周赛](https://leetcode.cn/contest/weekly-contest-321/ranking/) | 74(158) | 0:01:20 | 0:02:43 | 0:06:53 | 0:17:36 :lady_beetle: 1 |
    | [第 320 场周赛](https://leetcode.cn/contest/weekly-contest-320/ranking/) | 38(90) | 0:02:13 | 0:06:47 | 0:11:35 | 0:38:29 |
    | [第 91 场双周赛](https://leetcode.cn/contest/biweekly-contest-91/) | virtual | * | * | * | * |
    | [第 319 场周赛](https://leetcode.cn/contest/weekly-contest-319/ranking/) | 130(274) | 0:00:50 | 0:02:33 | 0:14:21 | 0:30:13 :lady_beetle: 1|
    | [第 318 场周赛](https://leetcode.cn/contest/weekly-contest-318/ranking/) | 50(87) | 0:01:57 | 0:04:15 | 0:10:45 | 0:32:58 :lady_beetle: 1 |
    | [第 317 场周赛](https://leetcode.cn/contest/weekly-contest-317/ranking/) | 84(166) | 0:01:18 | 0:07:50  :lady_beetle: 1 | 0:29:37 | 0:47:23 |
    | [第 316 场周赛](https://leetcode.cn/contest/weekly-contest-316/ranking/) | 138(291) | 0:03:45 | 0:05:20 |0:23:15 | 0:36:52 |
    | [第 315 场周赛](https://leetcode.cn/contest/weekly-contest-315/ranking/) | 315(695) | 0:01:39 | 0:04:37 | 0:12:43 :lady_beetle: 5 | 0:18:22  |
    | [第 314 场周赛](https://leetcode.cn/contest/weekly-contest-314/ranking/) | 25(48) | 0:03:17 | 0:04:55 | 0:17:02 :lady_beetle: 1 | 0:09:43  |
    | [第 88 场双周赛](https://leetcode.cn/contest/biweekly-contest-88/ranking/) | 14(virtual) | 	0:02:43 | 0:05:15 | 0:07:46 | 0:12:33 |
    | [第 313 场周赛](https://leetcode.cn/contest/weekly-contest-313/ranking/) | 13(27) | 0:00:43 | 0:03:17 | 0:06:21 | 0:13:14 |
    | [第 312 场周赛](https://leetcode.cn/contest/weekly-contest-312/ranking/) | 2(7) | 0:01:40	| 0:03:30 |	0:06:06 | 0:14:47 |
    | [第 311 场周赛](https://leetcode.cn/contest/weekly-contest-311/ranking/) | 2(7) | 0:00:53 | 0:02:45 |	0:05:40 | 0:09:20 |
    | [第 310 场周赛](https://leetcode.cn/contest/weekly-contest-310/ranking/) | 14(38) |0:04:39 | 0:04:42 |	0:09:29 | 0:14:19 |
    | [第 309 场周赛](https://leetcode.cn/contest/weekly-contest-309/ranking/) | 39(68) |	 0:02:59 | 0:05:49 | 0:11:30	| 0:23:17 :lady_beetle: 2|
    | [第 308 场周赛](https://leetcode.cn/contest/weekly-contest-308/ranking/) | 214(439) |	0:21:50 | 0:23:33	| 0:27:31	| 0:36:13|

=== "AtCoder"

    | Name      | rank | A | B | C | D | E | F | G | Ex |
    | :---------- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
    | [ABC 267](https://atcoder.jp/contests/abc267/tasks) | 406 | 1:57 | 7:16 | 11:31 | 15:23 | 26:34 | 84:35 (5) | | |


<script type="text/javascript">
    // 基于准备好的dom，初始化echarts实例
    var myChart = echarts.init(document.getElementById('main'));

    // 指定图表的配置项和数据
    var option = {
        title: {
            text: 'LeetCode 周赛排名曲线'
        },
        tooltip: {},
        legend: {
            data: ['美服排名', '国服排名']
        },
        xAxis: {
            data: [...Array(323 - 308).keys()].map((el, i) => 308 + i),
            name: '周赛'
        },
        yAxis: {
            type: 'value',
            name: '排名'
        },
        series: [
            {
                name: '美服排名',
                type: 'line',
                data: [
                    439, 68, 38, 7, 7, 27, 48, 695, 291, 166, 87, 274, 90, 158, 37, 82
                ],
                areaStyle: {}
            },
            {
                name: '国服排名',
                type: 'line',
                data: [
                    214, 39, 14, 2, 2, 13, 25, 315, 138, 84, 50, 130, 38, 74, 17, 41
                ],
                areaStyle: {}
            } 
        ]
    };

    // 使用刚指定的配置项和数据显示图表。
    myChart.setOption(option);
</script>