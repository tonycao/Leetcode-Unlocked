## Maximum Size Subarray Sum Equals K

>**Qustions**

Given an array nums and a target value k, find the maximum length of a subarray that sums to k. If there isn't one, return 0 instead.

>Example 1:

Given `nums = [1, -1, 5, -2, 3]`, `k = 3`,
return 4. (because the subarray `[1, -1, 5, -2]` sums to 3 and is the longest)

>Example 2:

Given `nums = [-2, -1, 2, 1]`, `k = 1`,
return 2. (because the subarray `[-1, 2]` sums to 1 and is the longest)

Follow Up:
Can you do it in O(n) time?


>**Solution**

```c++
class Solution {
public:
    int maxSubArrayLen (vector<int> &nums, int k) {
        if (nums.empty()) {
            return 0;
        }
        int maxLen = 0;
        unordered_map<int, int> summap;
        summap[0] = -1; // important
        int sum = 0;

        for (int i = 0; i < nums.size(); ++i) {
            sum += nums[i];
            if (summap.find(sum) == summap.end()) {
                summap[sum] = i;
            }
            if (summap.find(sum - k) != summap.end()) {
                maxLen = max(maxLen, i - summap[sum - k]);
            }
        }
        return maxLen;
    }
};

```
