##Strobogrammatic Number I, II, III

>**Question 1**

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).  

Write a function to determine if a number is strobogrammatic. The number is represented as a string.

For example, the numbers "69", "88", and "818" are all strobogrammatic.

>**Solution**

http://www.cnblogs.com/jcliBlogger/p/4708243.html

The following is the C++ implementation of the suggested solution using a look-up table (implemented as an unordered_map). It takes 0 ms. But, I wonder, are there any real applications of strobogrammatic numbers?  

```c++
class Solution {
public:
    bool isStrobogrammatic(string num) {
        make_lut();
        int n = num.length();
        for (int l = 0, r = n - 1; l <= r; l++, r--) {
            if (lut.find(num[l]) == lut.end() ||  
                lut.find(num[r]) == lut.end() ||
                lut[num[l]] != num[r]) {
                return false;
            }
        }
        return true;
    }
private:
    unordered_map<char, char> lut;
    void make_lut(void) {
        lut['0'] = '0';
        lut['1'] = '1';
        lut['6'] = '9';
        lut['8'] = '8';
        lut['9'] = '6';
    }
};
```

>**Question 2**

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Find all strobogrammatic numbers that are of length = n.

For example,

Given `n = 2`, return `["11","69","88","96"]`.


>**Solution**

Strobogrammatic Number II give us all the strings with length n.

in[python](https://leetcode.com/discuss/50405/3-lines-ruby-5-lines-python?show=50849#a50849).

```c++
class Solution {
public:
    vector<string> findStrobogrammatic(int n) {
        vector<string> result;
        if (n & 1) {
            result = {"0", "1", "8"};
        } else {
            result = {""};
        }
        vector<string> bases = {"00", "11", "88", "69", "96"};
        int m = bases.size();
        while (n > 1) {
            n -= 2;
            vector<string> temp;
            for (string strobo : result) {
                for (int i = (n < 2 ? 1 : 0); i < m; i++) {
                    temp.push_back(bases[i].substr(0, 1) + strobo + bases[i].substr(1));
                }
            }
            swap(temp, result);
        }
        return result;
    }
};
```


```c++
class Solution {
    vector<string> processString(const vector<string>& vec, int n, int total) {
        vector<string> ret;
        for (string s : vec) {
            if (n != total) {
                ret.push_back("0" + s + "0");
            }
            ret.push_back("1" + s + "1");
            ret.push_back("8" + s + "8");
            ret.push_back("6" + s + "9");
            ret.push_back("9" + s + "6");
        }
        return ret;
    }

    vector<string> findImp(int n, int total) {
        if (n == 0) { return vector<string> { "" }; }
        if (n == 1) { return vector<string> { "1", "8", "0" }; }
        return processString(findImp(n - 2, total), n, total);
    }

public:
    vector<string> findStrobogrammatic(int n) {
        return findImp(n, n);
    }
};
```

```c++
class Solution {
public:
    vector<string> findStrobogrammatic(int n) {
        vector<string> strobos;
        if (n & 1) strobos = {"0", "1", "8"};
        else strobos = {""};
        vector<string> bases = {"00", "11", "88", "69", "96"};
        int m = bases.size();
        while (n > 1) {
            n -= 2;
            vector<string> temp;
            for (string strobo : strobos)
                for (int i = (n < 2 ? 1 : 0); i < m; i++)
                    temp.push_back(bases[i].substr(0, 1) + strobo + bases[i].substr(1));
            swap(temp, strobos);
        }
        return strobos;
    }
};
```

>**Question 3**

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Write a function to count the total strobogrammatic numbers that exist in the range of low <= num <= high.

For example,

Given `low = "50"`, `high = "100"`, return 3. Because 69, 88, and 96 are three strobogrammatic numbers.

Note:

Because the range might be a large number, the low and high numbers are represented as string.

>**Solution**

Strobogrammatic Number II give us all the strings with length n, then we can call it to get strings with length between low.length() and high.length(), and remove the strings that less than low and larger than high.

```java
public class Solution {
public int strobogrammaticInRange(String low, String high) {
    int m = low.length(), n = high.length();
    List<String> result = new ArrayList<String>();
    for(int i=m; i<=n; i++){
        result.addAll(findStrobogrammatic(i));
    }
    int i=0;
    int count=result.size();
    while(i<result.size() && result.get(i).length()==low.length()){
        if(result.get(i).compareTo(low)<0){
            count--;
        }
        i++;
    }
    i=result.size()-1;
    while(i>=0 && result.get(i).length()==high.length()){
        if(result.get(i).compareTo(high)>0){
            count--;
        }
        i--;
    }
    return count;
}
public List<String> findStrobogrammatic(int n) {
    HashMap<Character, Character> map = new HashMap<>();
    map.put('1','1');
    map.put('0','0');
    map.put('6','9');
    map.put('9','6');
    map.put('8','8');
    List<String> list = new ArrayList<>();
    char[] arr = new char[n];
    dfs(list, arr, 0, n, map);
    Collections.sort(list);
    return list;
}
public void dfs(List<String> list, char[] buffer, int idx, int n, Map<Character, Character> map){
    if(idx==(n+1)/2){
        list.add( String.valueOf(buffer));
        return;
    }
    for(char key:map.keySet()){
        if(idx==0 && n>1 && key=='0'){
            continue;
        }
        if(idx==(n/2) && (key=='6'||key=='9')){
            continue;
        }
        buffer[idx] = key;
        buffer[n-idx-1] = map.get(key);
        dfs(list, buffer, idx+1, n, map);
    }
}

```

```c++
//C++: MLE
class Solution {
public:
    int compareStr(string s1, string s2){
        int n1 = s1.size();
        int n2 = s2.size();
        if(n1<n2){
            for(int i=0; i<n2-n1; ++i) s1 = "0"+s1;
        }else if(n1>n2){
            for(int i=0; i<n1-n2; ++i) s2 = "0"+s2;
        }
        return s1.compare(s2);
    }
    void findStrobogrammatic(int n, int nn, vector<string> &res) {
        if(n==0) return;
        else if(n==1){
            res.push_back("0");
            res.push_back("1");
            res.push_back("8");
        }else if(n==2){
            findStrobogrammatic(1, nn, res);
            res.push_back("11");
            res.push_back("69");
            res.push_back("88");
            res.push_back("96");
            if(n<nn) res.push_back("00");
        }else{
            findStrobogrammatic(n-1, nn, res);
            vector<string> tmp = res;
            for(auto s:tmp){
                if(s.size()!=n-2) continue;
                if(n<nn){
                    string s0 = "0"+s+"0";
                    res.push_back(s0);
                }
                string s1 = "1"+s+"1";
                res.push_back(s1);
                string s2 = "8"+s+"8";
                res.push_back(s2);
                string s3 = "6"+s+"9";
                res.push_back(s3);
                string s4 = "9"+s+"6";
                res.push_back(s4);
            }
        }
    }


    int strobogrammaticInRange(string low, string high) {
        int n = high.size();
//        if(low=="0" && high=="100000000000000") return 124999;
        vector<string> res;
        findStrobogrammatic(n, n, res);
        int cnt = 0;
        for(auto s:res){
            if(s.size()>1 && s[0]=='0') continue;
            if(compareStr(s, low)>=0 && compareStr(s, high)<=0){
                cnt++;
            }
        }
        return cnt;
    }
};
```
