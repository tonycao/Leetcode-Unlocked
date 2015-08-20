## Flatten 2D Vector

>**Question**

Implement an iterator to flatten a 2d vector.

For example,
Given 2d `vector =  
    [  
    [1,2],  
    [3],  
    [4,5,6]  
    ]`

By calling `__next__` repeatedly until `__hasNext__` returns false, the order of elements returned by next should be: ``[1, 2, 3, 4, 5, 6]`.

>**Solution**

The idea is very simple. We keep two variables row and col for the range of rows and cols. Specifically, row is the number of rows of vec2d and col is the number of columns of the current 1d vector in vec2d. We also keep two variables r and c to point to the current element.

In the constructor, we initialize row and col as above and initialize both r and c to be 0(pointing to the first element).

In hasNext(), we just need to check whether r and c are still in the range limited by row andcol.

In next(), we first record the current element, which is returned later. Then we update the running indexes and possibly the range if the current element is the last element of the current 1d vector.

A final and important note, since in next(), we record the current element, we need to guarantee that there is an element. So we implement a helper function skipEmptyVector() to skip the empty vectors. It is also important to handle the case that vec2d is empty (in this case, we set col = -1).

The time complexity of hasNext() is obviously O(1) and the time complexity of next is alsoO(1) in an amortized sense.

The code is as follows.


```c++
//C++: 20ms
class Vector2D {
    vector<vector<int>>::iterator row_it, row_end;
    vector<int>::iterator col_it;
public:
    Vector2D(vector<vector<int>>& vec2d) {
        row_it = vec2d.begin();
        row_end = vec2d.end();
        while(row_it!=row_end && row_it->empty()) row_it++;
        if(row_it!=row_end) col_it = row_it->begin();
    }

    int next() {
        int v = *col_it;
        col_it++;
        if(col_it==row_it->end()){
            row_it++;
            while(row_it!=row_end && row_it->empty()) row_it++; // skip empty rows
            if(row_it!=row_end) col_it = row_it->begin();
        }
        return v;
    }

    bool hasNext() {
       if(row_it==row_end) return false;
       return true;
    }
};

/**
 * Your Vector2D object will be instantiated and called as such:
 * Vector2D i(vec2d);
 * while (i.hasNext()) cout << i.next();
 */
```

another [solution](https://leetcode.com/discuss/50292/8-9-lines-added-c-o-1-space).
