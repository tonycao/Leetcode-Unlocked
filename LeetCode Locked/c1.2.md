##Two Sum II, III
> **Question 1**

Array is sorted.

>**Solution**

tow pointers one from beginning and one from end of the array. move pointers based on the two sum results.

```c++
vector<int> twosum(vector<int> &numbers, int target) {
    //vector<int> res;
    int left = 0, right = nums.size() - 1;
    while (left < right) {
        int sum = numbers[left] + numbers[right];
        if (sum == target) {
            return {left + 1, right + 1};
        } else if (sum > target){
            right--;
        } else {
            left++;
        }
    }
    return {};
}

```

> **Question 2**

Design and implement a TwoSum class. It should support the following operations: add and find.

`add(input)` – Add the number input to an internal data structure.

`find(value)` – Find if there exists any pair of numbers which sum is equal to the value.

For example,
`add(1); add(3); add(5); find(4)->true; find(7)->false`


>**Solution**

1. **add – O(n) runtime, find – O(1) runtime, O(n2) space– Store pair sums in hash table:**
We could store all possible pair sums into a hash table. The extra space needed is in the order of O(n2). You would also need an extra O(n) space to store the list of added numbers. Each add operation essentially go through the list and form new pair sums that go into the hash table. The find operation involves a single hash table lookup in O(1) runtime.
This method is useful if the number of find operations far exceeds the number of add operations.
2. **add – O(log n) runtime, find – O(n) runtime, O(n) space – Binary search + Two pointers:**
Maintain a sorted array of numbers. Each add operation would need O(log n) time to insert it at the correct position using a modified binary search (See Question [48. Search Insert Position]). For find operation we could then apply the [Two pointers] approach in O(n) runtime.
3. **add – O(1) runtime, find – O(n) runtime, O(n) space – Store input in hash table:**
A simpler approach is to store each input into a hash table. To find if a pair sum exists, just iterate through the hash table in O(n) runtime. Make sure you are able to handle duplicates correctly.

```c++
class TwoSum {
public:

    void add(int x) {
        umap[x]++;
    }

    bool find(int target) {
        for (auto i : umap) {
            if (umap.find(target - i.first) != umap.end()) {
                if (target - i.first != i.first) return true;
                else if (umap[i.first] >= 2) return true;
            }
        }
        return false;
    }
private:
    unordered_map<int, int> umap;
};
```
