##Longest Substring with At Most Two Distinct Characters

> **Question**

Given a string S, find the length of the longest substring T that contains at most two distinct characters.

For example,
Given `S = “eceba”`,
T is ``"ece"` which its length is 3.


> **Solution**

First, we could simplify the problem by assuming that S contains two or more distinct characters, which means T must contain exactly two distinct characters.
The brute force approach is O(n3) where n is the length of S. We can form every possible substring, and for each substring insert all characters into a Set which the Set’s size indicating the number of distinct characters. This could be easily improved to O(n2) by reusing the same Set when adding a character to form a new substring.

The trick is to maintain a sliding window that always satisfies the invariant where there are always at most two distinct characters in it. When we add a new character that breaks this invariant, how can we move the begin pointer to satisfy the invariant? Using the above example, our first window is the substring “abba”. When we add the character ‘c’ into the sliding window, it breaks the invariant. Therefore, we have to readjust the window to satisfy the invariant again. The question is which starting point to choose so the invariant is satisfied.

Let’s look at another example where S = “abaac”. We found our first window “abaa”. When we add ‘c’ to the window, the next sliding window should be “aac”.
This method iterates n times and therefore its runtime complexity is O(n). We use three pointers: i, j, and k.

```java
public int lengthOfLongestSubstringTwoDistinct(String s) {
   int i = 0, j = -1, maxLen = 0;
   for (int k = 1; k < s.length(); k++) {
      if (s.charAt(k) == s.charAt(k - 1)) continue;
      if (j >= 0 && s.charAt(j) != s.charAt(k)) {
         maxLen = Math.max(k - i, maxLen);
i = j + 1; }
j = k - 1; }
   return Math.max(s.length() - i, maxLen);
}
```

```c++
#include <iostream>
#include <cmath>
using namespace std;

class Solution {
public:
	int lengthOfLongestSubstringTwoDistinct(string s) {
		int i = 0, j = -1, maxLen = 0;
		for (int k = 1; k < s.length(); k++) {
			if (s[k] == s[k - 1]) continue;
			if (j >= 0 && s[j] != s[k]) {
				maxLen = max(k - i, maxLen);
				i = j + 1;
			}
			j = k - 1;
		}
		return max((int)(s.length() - i), maxLen);
	}
};


int main() {
	Solution sol;
	string s("eceba");
	cout << sol.lengthOfLongestSubstringTwoDistinct(s) << endl;
	return 0;
}
```

> **Further Thoughts**

Although the above method works fine, it could not be easily generalized to the case where T contains at most k distinct characters.
The key is when we adjust the sliding window to satisfy the invariant, we need a counter of the number of times each character appears in the substring.

```java
public int lengthOfLongestSubstringTwoDistinct(String s) {
   int[] count = new int[256];
   int i = 0, numDistinct = 0, maxLen = 0;
   for (int j = 0; j < s.length(); j++) {
      if (count[s.charAt(j)] == 0) numDistinct++;
      count[s.charAt(j)]++;
      while (numDistinct > 2) {
         count[s.charAt(i)]--;
         if (count[s.charAt(i)] == 0) numDistinct--;
         i++;
}
      maxLen = Math.max(j - i + 1, maxLen);
   }
   return maxLen;
}
```
