

##Reverse Words in a String II

>**Question**

Given an input string, reverse the string word by word. A word is defined as a sequence of non-space characters.
The input string does not contain leading or trailing spaces and the words are always separated by a single space.

For example,
Given s = "the sky is blue",
return "blue is sky the".
Could you do it in-place without allocating extra space?

>**Idea** Reverse twice, both in place.


>**Time** O(n) Space: O(1)

```c++
class Solution {
public:
    void reverseWords(string s) {
        if (s.size()==0 ) return ;
        reverse(s, 0, s.size() - 1);

        while (s.size() > 0 && s[0] == ' ') {
            s.erase(0, 1);
        }

        int begin = 0;
        for (int i = 0; i < s.size(); ++i) {
            if (i == s.size() || s[i] == ' ') {
                reverse(s, begin, i - 1);
                while (i < s.size() - 1 && s[i+1] == ' ') s.erase(i,1);
                if (i == s.size() - 1) s.erase(i, 1);
                begin = i + 1;
            }
        }
    }
  private:
    void reverse(string &s, int begin, int end) {
        while (begin < end) {
            char tmp = s[begin];
            s[begin] = s[end];
            s[end] = tmp;
        }
    }
};

```
