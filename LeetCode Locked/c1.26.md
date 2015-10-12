## Unique Word Abbreviation

>**Question**

An abbreviation of a word follows the form <first letter><number><last letter>. Below are some examples of word abbreviations:

>a) it                      --> it    (no abbreviation)

>     1
>b) d|o|g                   --> d1g

>              1    1  1
>     1---5----0----5--8
>c) i|nternationalizatio|n  --> i18n

>              1
>     1---5----0
>d) l|ocalizatio|n          --> l10n

Assume you have a dictionary and given a word, find whether its abbreviation is unique in the dictionary. A word's abbreviation is unique if no other word from the dictionary has the same abbreviation.

Example:

Given dictionary = [ "deer", "door", "cake", "card" ]

```c++
isUnique("dear") -> false
isUnique("cart") -> true
isUnique("cane") -> false
isUnique("make") -> true
```
>**Solution**

use map to store the abbrev and set of the original string (in case the same string is appeared in the set)

```c++
class ValidWordAbbr {
public:
    ValidWordAbbr(vector<string> &dictionary) {
        for (string& d : dictionary) {
            int n = d.length();
            string abbr = d[0] + to_string(n) + d[n - 1];
            mp[abbr].insert(d);
        }
    }

    bool isUnique(string word) {
        int n = word.length();
        string abbr = word[0] + to_string(n) + word[n - 1];
        return mp[abbr].count(word) == mp[abbr].size();
    }
private:
    unordered_map<string, unordered_set<string>> mp;
};


// Your ValidWordAbbr object will be instantiated and called as such:
// ValidWordAbbr vwa(dictionary);
// vwa.isUnique("hello");
// vwa.isUnique("anotherWord");
```
