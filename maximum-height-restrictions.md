# Maximum Height Restrictions

## Approach: Greedy Solution
### Intuition and Algorithm:
To handle complex scenarios with multiple restrictions, the code employs a two-pass approach. First, it ensures that each restriction is as tight as possible by "propagating" restrictions from left to right. This means that it calculates the maximum allowable height for each position considering only the restrictions to the left of that position. 
Then, it does a similar "propagation" from right to left, considering restrictions to the right.

By doing this two-pass propagation, the code ensures that the height restrictions are correctly applied, even when adjacent restrictions are not the tightest bounds. This approach helps in finding the maximum possible height of buildings while respecting all the given restrictions.
In summary, the code handles scenarios with multiple restrictions by iteratively propagating and updating height restrictions from left to right and then from right to left to ensure that the final height calculation considers all restrictions correctly.

### Example
For example, ```n = 8```, ```restrictions = [[2,1], [5,5], [7,1]]```:

We first propogate from left to right to tighten the restriction ```[5,5]``` to ```[5,4]```

Then going from right to left, we get ```[5,4]``` to ```[5,3]```

Now, we calculate the maximum height between each adjacent restriction pairs to get the overall maximum height.

### Code:
```
class Solution {
public:
    int maxBuilding(int n, vector<vector<int>>& arr) {
        arr.push_back({1, 0});
        arr.push_back({n, n - 1});
        sort(arr.begin(), arr.end());
        int m = arr.size();
        
        for (int i = 1; i < m; ++i)
            arr[i][1] = min(arr[i][1], arr[i-1][1] + arr[i][0] - arr[i-1][0]);
        for (int i = m - 2; i >= 0; --i)
            arr[i][1] = min(arr[i][1], arr[i+1][1] + arr[i+1][0] - arr[i][0]);
        
        int ans = 0, l, h1, r, h2;
        for (int i = 1; i < m; ++i) {
            l = arr[i-1][0], r = arr[i][0], h1 = arr[i-1][1], h2 = arr[i][1];
            ans = max(ans, max(h1, h2) + (r - l - abs(h1 - h2)) / 2);
        }
        return ans;
    }
};
```
### Complexities
**Time Complexity:**

1.	Modifying the input array (adding two elements) takes O(1) time.
2.	Sorting the modified array takes O(m * log(m)) time, where m is the number of elements in the array.
3.	The forward pass and backward pass both iterate over the sorted array once, so they each take O(m) time.
4.	The final loop iterates over the sorted array once, which also takes O(m) time.
So, the overall time complexity of this code is dominated by the sorting step, which is O(m * log(m)). In the worst case, m can be as large as n + 2, so the time complexity is O((n + 2) * log(n + 2)), which can be approximated as O(n * log(n)).

**Space Complexity:**

1.	The modified arr vector has a space complexity of O(m), where m is the number of elements in the array. In the worst case, m can be as large as n + 2, so the space complexity of the modified array is O(n).
2.	The integer variables used for temporary storage and calculations in the loops have a constant space complexity, i.e., O(1).
So, the overall space complexity of this code is O(n).
In summary, the time complexity is O(n * log(n)) and the space complexity is O(n).
