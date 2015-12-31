## 3Sum Smaller

>**Question**

Given an array of `n` integers `nums` and a target, find the number of index triplets i, j, k with `0 <= i < j < k < n` that satisfy the condition `nums[i] + nums[j] + nums[k] < target`.

For example, given `nums = [-2, 0, 1, 3]``, and `target = 2`.

Return 2. Because there are two triplets which sums are less than 2:

`[-2, 0, 1]`  
`[-2, 0, 3]`  

Follow up:
Could you solve it in O(n2) runtime?

>**Solutuion**  

sort and two pointers

```c++
class Solution {
public:
    int threeSumSmaller(vector<int>& nums, int target) {
        if (nums.size() < 3) return 0;
        sort(nums.begin(), nums.end());
        int sz = nums.size(), count = 0;
        for (int ind = 0; ind < sz - 2; ind++) {
            int left = ind + 1, right = sz - 1;
            while(left < right) {
                int sum = nums[ind] + nums[left] + nums[right];
                if (sum < target) {
                    count += right - left;
                    left++;
                } else {
                    right--;
                }
            }
        }
        return count;
    }
}
```
