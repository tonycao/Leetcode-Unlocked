## Meeting Rooms I, II

>**Question 1**

Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...] (si < ei)`, determine if a person could attend all meetings.

For example,

Given `[[0, 30],[5, 10],[15, 20]]`,  
return false.

>**Solution**

The idea is pretty simple: first we sort the intervals in the ascending order of start; then we check for the overlapping of each pair of neighboring intervals. If they do, then return false; after we finish all the checks and have not returned false, just return true.

Sorting takes O(nlogn) time and the overlapping checks take O(n) time, so this idea is O(nlogn) time in total.

The code is as follows.
```c++
/**
 * Definition for an interval.
 * struct Interval {
 *     int start;
 *     int end;
 *     Interval() : start(0), end(0) {}
 *     Interval(int s, int e) : start(s), end(e) {}
 * };
 */
class Solution {
public:
    bool canAttendMeetings(vector<Interval>& intervals) {
        // Time: O(NlogN)
        sort(intervals.begin(), intervals.end(), compare);
        int n = intervals.size();
        for (int i = 0; i < n - 1; i++)
            if (overlap(intervals[i], intervals[i + 1]))
                return false;
        return true;
    }
private:
    static bool compare(Interval& interval1, Interval& interval2) {
        return interval1.start < interval2.start;
    }
    bool overlap(Interval& interval1, Interval& interval2) {
        return interval1.end > interval2.start;
    }
};
```

>**Question 2**

Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...] (si < ei)`, find the minimum number of conference rooms required.

For example,

Given `[[0, 30],[5, 10],[15, 20]]`,  
return `2`.

>**Solution**

The idea is to group those non-overlapping meetings in the same room and then count how many rooms we need. You may refer to this [link](https://leetcode.com/discuss/50783/java-ac-code-using-comparator).


```c++
bool myComp(const Interval &a, const Interval &b){
    return (a.start<b.start);
}
class Solution {
public:
    int minMeetingRooms(vector<Interval>& intervals) {
        int rooms = 0;
        priority_queue<int> pq;//prioritize earlier ending time
        sort(intervals.begin(), intervals.end(), myComp);
        for(int i=0; i<intervals.size(); ++i){
            while(!pq.empty() && -pq.top()<intervals[i].start) pq.pop();
            pq.push(-intervals[i].end);
            rooms = max(rooms, (int)pq.size());
        }
        return rooms;
    }
};

```

another solution: for each group of non-overlapping intervals, we just need to store the last added one instead of the full list. So we could use a vector < Interval > instead of vector < vector < Interval >> in C++. The code is now as follows.

```c++
class Solution {
public:
    int minMeetingRooms(vector<Interval>& intervals) {
        sort(intervals.begin(), intervals.end(), compare);
        vector<Interval> rooms;
        int n = intervals.size();
        for (int i = 0; i < n; i++) {
            int idx = findNonOverlapping(rooms, intervals[i]);
            if (rooms.empty() || idx == -1)
                rooms.push_back(intervals[i]);
            else rooms[idx] = intervals[i];
        }
        return (int)rooms.size();
    }
private:
    static bool compare(Interval& interval1, Interval& interval2) {
        return interval1.start < interval2.start;
    }
    int findNonOverlapping(vector<Interval>& rooms, Interval& interval) {
        int n = rooms.size();
        for (int i = 0; i < n; i++)
            if (interval.start >= rooms[i].end)
                return i;
        return -1;
    }
};
```

another smart solution: [here](https://leetcode.com/discuss/58720/c-solution-using-a-map-total-11-lines)

```c++
class Solution {
public:
    int minMeetingRooms(vector<Interval>& intervals){
        // Time O(NlogN)
        map<int, int> mp; // key:time, val:+1 start, -1 end
        for (int i = 0; i < intervals.size(); ++i) {
            mp[intervals[i].start]++;
            mp[intervals[i].end]--;
        }
        int cnt = 0; maxcnt = 0;
        for (auto it = mp.begin(); it != mp.end(); ++it) {
            cnt += it->second;
            maxcnt = max(maxcnt, cnt);
        }
        return maxcnt;
    }
};
```
