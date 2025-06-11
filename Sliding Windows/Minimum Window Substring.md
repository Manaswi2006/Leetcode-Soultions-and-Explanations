# Intuition and Approach
We keep a count variable to see the number of unique variables from the characters required that are seen in current substring. We first find a valid substring then shrink its window until its valid, that is count == number of characters in t.
We use a two pointer approach to find a valid substring -> extend r until all characters needed are in the substring, then to shrink move l until validity remains. Then if current shrinked window has size < min len we record it.
Then from current l and r we again start a search for another valid substring, until r == n.


# Complexity
## - Time complexity: O(n)
Building freq map from t: O(m)
m is at most 26 -> thus constant O(1)
Iterating over s with r and l: O(n)
Each character is visited at most twice: once by r and once by l.


## - Space complexity: O(1)
freq map: stores at most m keys (unique characters of t)
window map: at most m keys because we only care about characters in t
However m is at most 26 -> thus constant.


# Code
```cpp []
class Solution {
public:
    string minWindow(string s, string t) {
        int n = s.size();
        int m = t.size();
        int l = 0;
        int r = 0;
        int minLen = 1e9;
        int sIndex = -1;
        int count = 0;


        unordered_map<char, int> freq;
        for (int i = 0; i < m; i++) {
            freq[t[i]]++;
        }


        unordered_map<char, int> window;


        while (r < n) {
            window[s[r]]++;
            if (freq.count(s[r]) && window[s[r]] <= freq[s[r]]) {
                count++;
            }


            while (count == m) {
                if (r - l + 1 < minLen) {
                    minLen = r - l + 1;
                    sIndex = l;
                }


                window[s[l]]--;
                if (freq.count(s[l]) && window[s[l]] < freq[s[l]]) {
                    count--;
                }
                l++;
            }


            r++;
        }


        return sIndex == -1 ? "" : s.substr(sIndex, minLen);
    }
};


```

