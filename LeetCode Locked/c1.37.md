## Generalized Abbreviation

>**Question**

Write a function to generate the generalized abbreviations of a word.

**Example:**
Given `word = "word"`, return the following list (order does not matter):
```
["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2",
"1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
```

>**Solution**

bit mask

```c++
class Solution {
public:
    vector<string> generateAbbreviations(string word) {
        vector<string> sol;
        int len = word.length();
        for (int i = 0; i < (1 << len); i++) {
            int count = 0;
            string one = "";
            for (int j = 0; j < len; j++) {
                if (i & (1 << j)) {
                    count++;
                } else {
                    if (count) {
                        one += to_string(count);
                        count = 0;
                    }
                    one += word[j];
                }
            }
            if (count) {
                one += to_string(count);
            }
            sol.push_back(one);
        }
        return sol;
    }
};
```

[DFS](https://segmentfault.com/a/1190000004187690)

```c++
class Solution {
public:
    vector<string> generateAbbreviations(string word) {
        //time: O(2^n), space: O(n)
        vector<string> res;
        dfs(res, "", 0, word);
        return res;
    }

private:
    void dfs(vector<string>& res, string curr, int start, string s) {
        // stop recursion
        if (start > s.size()) {
            return ;
        }

        // record valid result
        res.push_back(curr + s.substr(start));

        // restart
        int i = 0;

        //
        if (start > 0) {
            i = start + 1;
        }

        for (; i < s.size(); i++) {
            for (int j = 1; j <= s.size(); j++) {
                // append substr before Number
                dfs(res, curr + s.substr(start, i - start) + to_string(j), i + j, s);
            }
        }
    }

};
```
