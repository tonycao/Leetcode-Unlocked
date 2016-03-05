## Zigzag Iterator

>**Questions**

Given two 1d vectors, implement an iterator to return their elements alternately.

For example, given two 1d vectors:

`v1 = [1, 2]`

`v2 = [3, 4, 5, 6]`

By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: `[1, 3, 2, 4, 5, 6]`.

>Follow up:

What if you are given k 1d vectors? How well can your code be extended to such cases?

>**Solutions**

from [here](http://www.cnblogs.com/jcliBlogger/p/4807033.html)

The idea is as follows: keep the two beginning iterators and the two end iterators of v1 and v2into two arrays of type vector<int>::iterator with size 2. Then we keep a variable p(initialized to be 0) to record which iterator should be used: p takes values ranging from 0 to1. Each time we call next, update p by p = (p + 1) % 2 to circulate it to point to the next vector.

The code is as follows.

```c++
class ZigzagIterator {
public:
    ZigzagIterator(vector<int>& v1, vector<int>& v2) {
        bs[0] = v1.begin(), bs[1] = v2.begin();
        es[0] = v1.end(), es[1] = v2.end();
        p = 0;
    }

    int next() {
        int elem;
        if (bs[0] == es[0]) elem = *bs[1]++;
        else if (bs[1] == es[1]) elem = *bs[0]++;
        else {
            elem = *bs[p]++;
            p = (p + 1) % 2;
        }
        return elem;
    }

    bool hasNext() {
        return bs[0] != es[0] || bs[1] != es[1];
    }
private:
    int p;
    vector<int>::iterator bs[2], es[2];
};
```
