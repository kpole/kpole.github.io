## [442.数组中重复的数据](https://leetcode.cn/problems/find-all-duplicates-in-an-array/description/)


!!! note ""
    给你一个长度为 n 的整数数组 nums ，其中 nums 的所有整数都在范围 [1, n] 内，且每个整数出现 一次 或 两次 。请你找出所有出现 两次 的整数，并以数组形式返回。

    你必须设计并实现一个时间复杂度为 O(n) 且仅使用常量额外空间的算法解决此问题。

有很多种方法可以解决本题，但是思路本质上都是类似的。「原地修改」，指在输入的数据上直接做修改，可以避免额外的空间复杂度，同时原地修改的方法最好还可以还原原数组。

### 方法一 交换

每个数字都在 $[1, n]$，设对应位置 $[0, n-1]$，每次遇到一个数字 $nums[i] = x$，就将其交换到 $x - 1$ 的位置上，如果 $nums[x - 1]$ 已经等于 $x$，就说明 $x$ 是一个重复出现的数字。如果不等于，交换后 $nums[i]$ 被换成了一个新的数字，那就继续交换，直到相等。

### 方法二 取反

交换的方法会导致无法还原原数组，具有破坏性。但是如果仅仅是将 $x - 1$ 位置上的数字变为负数，那么最终是可以还原的。

### 方法三 增加 n

大同小异，由于数据范围仅有 $n$，所以每次遇到 $x$，就将 $x - 1$ 位置上的数字增加 $n$，这样最后数值大于 $2n$ 的元素就是重复出现两次的。


=== "方法一"
    ```c++ linenums="1"
    class Solution {
    public:
        vector<int> findDuplicates(vector<int>& nums) {
            int n = nums.size();
            for (int i = 0; i < n; ++i) {
                while (nums[i] != nums[nums[i] - 1]) {
                    swap(nums[i], nums[nums[i] - 1]);
                }
            }
            vector<int> ans;
            for (int i = 0; i < n; ++i) {
                if (nums[i] - 1 != i) {
                    ans.push_back(nums[i]);
                }
            }
            return ans;
        }
    };
    ```

=== "方法二"
    ```c++ linenums="1"
    class Solution {
    public:
        vector<int> findDuplicates(vector<int>& nums) {
            int n = nums.size();
            vector<int> res;
            for (auto x : nums) {
                x = abs(x);
                if (nums[x - 1] < 0) {
                    res.push_back(x);
                } else {
                    nums[x - 1] *= -1;
                }
            }
            return res;
        }
    };
    ```

=== "方法三"
    ```c++ linenums="1"
    class Solution {
    public:
        vector<int> findDuplicates(vector<int>& nums) {
            int n = nums.size();
            for (auto x : nums) {
                // 注意有些 x 已经变化，所以要取模
                nums[(x - 1) % n] += n;
            }
            vector<int> res;
            for (int i = 0; i < n; i++) {
                if (nums[i] > 2 * n) {
                    res.push_back(i + 1);
                }
            }
            return res;
        }
    };
    ```


**复杂度分析**

- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
