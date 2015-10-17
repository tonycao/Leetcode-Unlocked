## Flip Game

>**Question**

You are playing the following Flip Game with your friend: Given a string that contains only these two characters: `+` and `-`, you and your friend take turns to flip two consecutive "++" into "--". The game ends when a person can no longer make a move and therefore the other person will be the winner.

Write a function to compute all possible states of the string after one valid move.

For example, given s = "++++", after one move, it may become one of the following states:

```c++
[
  "--++",
  "+--+",
  "++--"
]
```


If there is no valid move, return an empty list `[]`.

>**Solution**
```c++
class Solution {
public:
    vector<string> generatePossibleNextMoves(string s) {
        vector<string> moves;
        int n = s.length();
        for (int i = 0; i < n - 1; i++) {
            if (s[i] == '+' && s[i + 1] == '+') {
                s[i] = s[i + 1] = '-';
                moves.push_back(s);
                s[i] = s[i + 1] = '+';
            }
        }
        return moves;
    }
};
```

## Flip Game II

>**Question**

You are playing the following Flip Game with your friend: Given a string that contains only these two characters: `+` and `-`, you and your friend take turns to flip two consecutive "++" into "--". The game ends when a person can no longer make a move and therefore the other person will be the winner.

Write a function to determine if the starting player can guarantee a win.

For example, given s = "++++", return true. The starting player can guarantee a win by flipping the middle "++" to become "+--+".

Follow up:
Derive your algorithm's runtime complexity.

>**Solution**

short answer from [here](https://leetcode.com/discuss/64350/short-java-%26-ruby?show=64350#q64350).

```c++
class Solution {
public:
    bool canWin(string s) {
        for (int i = -1; (i = s.find("++", i + 1)) >= 0; )
            if (!canWin(s.substr(0, i) + '-' + s.substr(i+2)))
                return true;
        return false;
    }
};
```

or [here](https://leetcode.com/discuss/64344/theory-matters-from-backtracking-128ms-to-dp-0ms)
