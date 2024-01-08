## [1815. 得到新鲜甜甜圈的最多组数](https://leetcode.cn/problems/maximum-number-of-groups-getting-fresh-donuts/)

### 方法一：状压DP

!!! note ""
    有一个甜甜圈商店，每批次都烤 batchSize 个甜甜圈。这个店铺有个规则，就是在烤一批新的甜甜圈时，之前 所有 甜甜圈都必须已经全部销售完毕。给你一个整数 batchSize 和一个整数数组 groups ，数组中的每个整数都代表一批前来购买甜甜圈的顾客，其中 groups[i] 表示这一批顾客的人数。每一位顾客都恰好只要一个甜甜圈。

    当有一批顾客来到商店时，他们所有人都必须在下一批顾客来之前购买完甜甜圈。如果一批顾客中第一位顾客得到的甜甜圈不是上一组剩下的，那么这一组人都会很开心。

    你可以随意安排每批顾客到来的顺序。请你返回在此前提下，最多 有多少组人会感到开心。

    数据范围：

    - $1 \le batchSize \le 9$
    - $1 \le groups.length \le 30$
    - $1\le groups[i] \le 10^9$

本题极具技巧性，注意到每组人数可以直接对 $batchSize$ 取模，然后 $cnt[x]$ 表示人数为 $x$ 的组数。这时最多只会有 $9$ 种不同人数的组。

我们可以从这 $batchSize$ 种组里面选一种，作为当前的最后一组。构造这个状态需要 $batchSize$ 维数组。这样很麻烦，所以将其压缩，每种最多 $30$ 种，所以使用 $5$ 位二进制数字来表示个数。

然后对于 $0$ 的我们不需要管它，所以这时最多 $40$ 位二进制数字。进行状压即可。

```cpp linenums="1"
class Solution {
public:
    using ll = long long;
    int maxHappyGroups(int batchSize, vector<int>& groups) {
        vector<int> cnt(batchSize);
        for (auto x : groups) {
            cnt[x % batchSize]++;
        }

        ll mask = 0;
        for (int i = batchSize - 1; i >= 1; i--) {
            mask = (mask << BITWIDTH) | cnt[i];
        }
        unordered_map<ll, int> d;
        function<int(ll)> dfs = [&](ll mask) -> int {
            if (mask == 0) {
                return 0;
            }
            if (d.count(mask)) {
                return d[mask];
            }
            int sum = 0; 
            for (int i = 1; i < batchSize; i++) {
                int count = (mask >> ((i - 1) * BITWIDTH)) & BITMASK;
                sum += i * count;
            }
            int ret = 0;
            for (int i = 1; i < batchSize; i++) {
                int count = (mask >> ((i - 1) * BITWIDTH)) & BITMASK;
                if (count > 0) {
                    int sub = dfs(mask - (1ll << (i - 1) * BITWIDTH));
                    if ((sum - i) % batchSize == 0) {
                        ret = max(ret, sub + 1);
                    } else {
                        ret = max(ret, sub);
                    }
                }
            }
            return d[mask] = ret;
        };
        return dfs(mask) + cnt[0];
    }
private:
    static constexpr int BITWIDTH = 5;
    static constexpr int BITMASK = (1 << BITWIDTH) - 1;
};
```
**复杂度分析**

我们并不会将所有的 40 位二进制数字都遍历到，因为组数最多只有 30，均匀分到 8 个组里面，乘起来之后可以得到状态数最多约为 $2\times 10^5$ 左右。