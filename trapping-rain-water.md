# Trapping Rain Water

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute volume of water it is able to trap after raining.

## Example

**Input**: height = [0,1,0,2,1,0,1,3,2,1,2,1]

**Output**: 6

**Explanation**: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.

## Approach 1: Brute force 
### Intuition
Do as directed in the question. For each element in the array, we find the maximum level of water it can trap after the rain directly above it, which is equal to the minimum of the maximum height of bars on both sides of the element, minus its own height.
### Algorithm:
Initialize the answer variable, ans, to 0.
Iterate over the array from left to right.
For each element in the array:
Initialize left_max and right_max to 0.
Iterate from the current element to the beginning of the array, updating left_max to the maximum height of the buildings to the left of the current element.
Iterate from the current element to the end of the array, updating right_max to the maximum height of the buildings to the right of the current element.
Add the difference between the minimum of left_max and right_max and the height of the current element to the answer variable, ans.
Return ans.
### Code:
```
class Solution {
public:
    int trap(vector<int>& height)
    {
        int ans = 0;
        int size = height.size();
        for (int i = 1; i < size - 1; i++) {
            int left_max = 0, right_max = 0;
            for (int j = i; j >= 0; j--) { //Search the left part for max bar size
                left_max = max(left_max, height[j]);
            }
            for (int j = i; j < size; j++) { //Search the right part for max bar size
                right_max = max(right_max, height[j]);
            }
            ans += min(left_max, right_max) - height[i];
        }
        return ans;
    }
};
```

### Complexity Analysis

**Time complexity** : O(n * n), For each element of array, we iterate the left and right parts.

**Space complexity** : O(1) extra space.


## Approach 2: Dynamic Programming
### Intuition
In brute force, we iterate over the left and right parts again and again just to find the highest bar size upto that index. But, this could be stored, hence Dynamic Programming.
### Algorithm:
The algorithm works by first finding the maximum height of the buildings to the left and right of each element in the array. This is done by iterating over the array from left to right and from right to left, respectively, and updating the left_max and right_max arrays accordingly.
Once the left_max and right_max arrays have been populated, the algorithm iterates over the height array and adds the difference between the smaller of left_max[i] and right_max[i] and the height of the current element, height[i], to the answer variable, ans. This is because the difference represents the amount of rainwater that can be trapped between the current element and the higher of its two neighbors.
### Code:

```
class Solution {
public:
    int trap(vector<int>& height)
    {
        if(height.empty())
            return 0;
        int ans = 0;
        int size = height.size();
        vector<int> left_max(size), right_max(size);
        left_max[0] = height[0];
        for (int i = 1; i < size; i++) {
            left_max[i] = max(height[i], left_max[i - 1]);
        }
        right_max[size - 1] = height[size - 1];
        for (int i = size - 2; i >= 0; i--) {
            right_max[i] = max(height[i], right_max[i + 1]);
        }
        for (int i = 1; i < size - 1; i++) {
            ans += min(left_max[i], right_max[i]) - height[i];
        }
        return ans;
    }
};
```

### Complexity Analysis

**Time complexity**: O(n). We store the maximum heights upto a point using 2 iterations of O(n) each. We finally update ans using the stored values in O(n)

**Space complexity**: O(n) extra space. Additional O(n) space for left_max and right_max arrays

## Approach 3: 2 Pointers
### Intuition
As in Approach 2, instead of computing the left and right parts separately, we may think of some way to do it in one iteration.
From the figure in dynamic programming approach, notice that as long as right_max[i] > left_max[i], the water trapped depends upon the left_max, and similar is the case when left_max[i] > right_max[i].
So, we can say that if there is a larger bar at one end (say right), we are assured that the water trapped would be dependent on height of bar in current direction (from left to right). As soon as we find the bar at other end (right) is smaller, we start iterating in opposite direction (from right to left).
We must maintain left_max and right_max during the iteration, but now we can do it in one iteration using 2 pointers, switching between the two.
### Algorithm:
The algorithm works by maintaining two pointers, left and right, which point to the leftmost and rightmost elements in the array, respectively. The algorithm also maintains two variables, left_max and right_max, which represent the maximum heights of the buildings to the left and right of the current element, respectively.

At each iteration of the algorithm, the algorithm compares the heights of the buildings at left and right. If the height of the building at left is smaller than the height of the building at right, then the algorithm adds the difference between left_max and the height of the building at left to the answer variable, ans. This is because the difference represents the amount of rainwater that can be trapped between the building at left and the higher of the two buildings at left_max and right_max. The algorithm then increments left.

If the height of the building at right is smaller than the height of the building at left, then the algorithm adds the difference between right_max and the height of the building at right to the answer variable, ans. This is because the difference represents the amount of rainwater that can be trapped between the building at right and the higher of the two buildings at left_max and right_max. The algorithm then decrements right.

The algorithm continues in this way until the two pointers meet, at which point the algorithm returns the value of the answer variable, ans.

### Code:

```
class Solution {
public:
    int trap(vector<int>& height)
    {
        int left = 0, right = height.size() - 1;
        int ans = 0;
        int left_max = 0, right_max = 0;
        while (left < right) {
            if (height[left] < height[right]) {
                height[left] >= left_max ? (left_max = height[left]) : ans += (left_max - height[left]);
                ++left;
            }
            else {
                height[right] >= right_max ? (right_max = height[right]) : ans += (right_max - height[right]);
                --right;
            }
        }
        return ans;
    }
};
```

### Complexity analysis

**Time complexity** : O(n), Single iteration of O(n).

**Space complexity** : O(1) extra space. Only constant space required for left, right, left_max and right_max.

