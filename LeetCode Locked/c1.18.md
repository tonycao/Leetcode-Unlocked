## Palindrome Permutation I, II

>**Question 1**

Given a string, determine if a permutation of the string could form a palindrome.

For example,  
`"code" -> False`, `"aab" -> True`, `"carerac" -> True`.

Hint:

1. Consider the palindromes of odd vs even length. What difference do you notice?
2. Count the frequency of each character.
3. If each character occurs even number of times, then it must be a palindrome. How about character which occurs odd number of times?

>**Solution**


count number of chars, and odd char can only be one

```c++

class Solution {
public:
    bool canPermutePalindrome(string s) {
        vector<int> smap(256, 0);
        for (auto c : s) {
            smap[c]++;
        }
        int oddCount = 0;
        for (int i = 0; i < 256; i++) {
            if (cmap[i] % 2 != 0) oddCount++;
        }
        return oddCount <= 1;
    }
};

```

another solution [here](https://leetcode.com/discuss/53180/1-4-lines-python-ruby-c-c-java)

```c++
bool canPermutePalindrome(string s) {
bitset<256> b;
for (char c : s)
    b.flip(c);
return b.count() < 2;
}
```



>**Question 2**

Given a string s, return all the palindromic permutations (without duplicates) of it. Return an empty list if no palindromic permutation could be form.

For example:

Given `s = "aabb"`, return `["abba", "baab"]`.

Given `s = "abc"`, return [].

Hint:

If a palindromic permutation exists, we just need to generate the first half of the string.
To generate all distinct permutations of a (half of) string, use a similar approach from: Permutations II or Next Permutation.

>**Solution**

```c++
class Solution {
public:
    vector<string> generatePalindromes(string s) {
        vector<string> palindromes;
        unordered_map<char, int> count;
        for (char c : s) count[c]++;
        int odd = 0; char mid; string half;
        for (auto p: count) {
            if (p.second & 1) {
                odd++, mid = p.first;
            }
            half += string(p.second / 2, p.first);
        }
        palindromes = permutations(half);
        for (string & p : palindromes) {
            string t(p);
            reverse(t.begin(), t.end());
            if (odd) t = mid + t;
            p += t;
        }
        return palindromes;
    }
private:
    vector<string> permutations(string & s) {
        vector<string> perms;
        string t(s);
        do {
            perms.push_back(s);
            next_permutation(s.begin(), s.end());
        } while (s!= t);
        return perms;
    }

};
```
