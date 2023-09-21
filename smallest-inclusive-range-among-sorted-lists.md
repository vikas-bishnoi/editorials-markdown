# Smallest Inclusive Range among Sorted Integer Lists

You have k lists of sorted integers in non- decreasing order. Find the smallest range that includes at least one number from each of the k lists. We define the range [a, b] is smaller than range [c, d] if b - a < d - c or a < c if b - a == d – c.

**Input:** ```nums = [[4,10,15,24,26],[0,9,12,20],[5,18,22,30]]```

**Output:**  ```[20,24]```

**Explanation:**
```
List 1: [4, 10, 15, 24,26], 24 is in range [20,24].
List 2: [0, 9, 12, 20], 20 is in range [20,24].
List 3: [5, 18, 22, 30], 22 is in range [20,24].
```

## Approach 1: Brute Force [Time Limit Exceeded]

### Intuition and Algorithm:
Initialize the minimum range to be ```INT_MAX```.

Iterate over all pairs of elements, ```nums[i][j]``` and ```nums[k][l]``` where ```0 <= i < j < k < l < k```.

For each pair, consider the range formed by these elements.

Traverse over all the lists to determine if at least one element from these lists can be included in the current range.
If so, and the current range is less than the minimum range found so far, update the minimum range to be the current range, [x, y].
Return the minimum range found, [x, y].

### Example

Suppose we have four lists of integers:
```
List 1: [2, 5, 8]
List 2: [4, 9]
List 3: [1, 7, 10]
List 4: [3, 6]
```
We want to find the minimum range by selecting elements from these lists.

Initialize min_range to INT_MAX.

```min_range = INT_MAX```

Iterate over all pairs of elements, i.e., (i, j) and (k, l) where 0 <= i < j < k < l < k.

(i, j) can be (0, 1), (0, 2), (0, 3), (1, 2), (1, 3), (2, 3)
For each pair, consider the range formed by these elements.

Let's start with (i, j) = (0, 1), so we consider the range between ```nums[0][0]``` and ```nums[1][1]```, which is [2, 9].

Traverse over all the lists to determine if at least one element from these lists can be included in the current range.

For the range [2, 9], we can include elements 2, 5, 4, 9, 1, 7, 3, and 6.
Calculate the current range, and if it's less than min_range, update min_range.

The current range is [1, 9] (minimum: 1, maximum: 9), which is less than min_range, so we update min_range to [1, 9].

Repeat steps 3-5 for all pairs of elements.

For (0, 2), the range is [2, 8], and the current range is [1, 8], which is less than [1, 9], so we update min_range to [1, 8].

For (0, 3), the range is [2, 3], and the current range is [1, 3], which is less than [1, 8], so we update min_range to [1, 3].

For (1, 2), the range is [5, 8], and the current range is [4, 8], which is not less than [1, 3].

For (1, 3), the range is [5, 6], and the current range is [4, 6], which is not less than [1, 3].

For (2, 3), the range is [8, 3], and the current range is [1, 8], which is not less than [1, 3].

After iterating over all pairs, the minimum range found is [1, 3].

Return the minimum range, which is [1, 3].

So, the minimum range that can be formed by selecting elements from the given lists is [1, 3].

### Code:
```
class Solution {
public:
  std::vector<int> smallestRange(std::vector<std::vector<int>>& nums) {
    int minx = 0, miny = INT_MAX;
    for (int i = 0; i < nums.size(); i++) {
      for (int j = 0; j < nums[i].size(); j++) {
        for (int k = i; k < nums.size(); k++) {
          for (int l = (k == i ? j : 0); l < nums[k].size(); l++) {
            int min = std::min(nums[i][j], nums[k][l]);
            int max = std::max(nums[i][j], nums[k][l]);
            int n, m;
            for (m = 0; m < nums.size(); m++) {
              for (n = 0; n < nums[m].size(); n++) {
                if (nums[m][n] >= min && nums[m][n] <= max)
                  break;
              }
              if (n == nums[m].size())
                break;
            }
            if (m == nums.size()) {
              if (miny - minx > max - min ||
                  (miny - minx == max - min && minx > min)) {
                miny = max;
                minx = min;
              }
            }
          }
        }
      }
    }
    return {minx, miny};
  }
};
```

### Complexities

**Time complexity:** ```O(n^3)```

The time complexity of the algorithm is ```O(n^3)```, where n is the total number of elements in all the lists. This is because the algorithm needs to consider every possible pair of elements from all the lists, and for each pair, it needs to traverse over all the lists to determine if at least one element from these lists can be included in the current range.

**Space complexity:** ```O(1)```

The space complexity of the algorithm is ```O(1)```, because it only uses constant extra space, such as for the variables minx and miny.

Here is a more detailed explanation of the time complexity:

The algorithm considers every possible pair of elements from all the lists. This takes ```O(n^2)``` time, because we need to iterate over all the elements in all the lists.
For each pair of elements, the algorithm needs to traverse over all the lists to determine if at least one element from these lists can be included in the current range. This takes O(n) time in the worst case, because we may need to iterate over all the elements in all the lists.
Therefore, the overall time complexity of the algorithm is O(n^2) * O(n) = O(n^3).
Here is a more detailed explanation of the space complexity:

The algorithm only uses constant extra space, such as for the variables minx and miny.
Therefore, the overall space complexity of the algorithm is O(1).

## Approach 2: Using Pointers

### Intuition and Algorithm:

This algorithm uses an array of pointers, next, where each element ```next[i]``` indicates the element that needs to be considered next in the i-th list. The purpose of this will become clearer as we go through the process.
Here's a simplified explanation of the algorithm:

1.	Begin by initializing all elements of the next array to 0. This means we are currently considering the first (minimum) element in each list.

2.	Find the index of the list that contains the maximum (max_i) and minimum (min_i) elements among the elements pointed to by the next array. The range between these maximum and minimum elements surely contains at least one element from each list.

3.	However, our goal is to minimize this range. To achieve this, we have two options: either decrease the maximum value or increase the minimum value.

4.	The maximum value can't be reduced any further because it already corresponds to the minimum value in one of the lists. Reducing it further would exclude all the elements from that list.
5.	Therefore, the only option left is to try to increase the minimum value. To do this, we consider the next element in the list that contains the last minimum value. We increment the corresponding entry in the next array to indicate that the next element in this list should now be considered.
6.	At each step, we identify the current maximum and minimum values, update the next array accordingly, and determine the range formed by these maximum and minimum values to find the smallest range that meets the given criteria.
7.	During this process, if any list becomes completely exhausted (no more elements to consider), it means we can't increase the minimum value any further without excluding all the elements in at least one of the lists. Therefore, we can stop the search process when any list gets exhausted.
8.	We can also stop the process when all the elements in the given lists have been exhausted.

In summary, the algorithm follows these steps:

•	Initialize the next array (pointers) with all 0's.

•	Find the indices of the lists containing the minimum and maximum elements among the elements pointed to by the next array.

•	If the range formed by the maximum and minimum elements found above is larger than the previous maximum range, update the boundary values used for the maximum range.

•	Increment the pointer in the list that contains the minimum element.

•	Repeat steps 2 to 4 until any of the lists gets exhausted.

### Example

Suppose we have four lists of integers:
```
List 1: [2, 5, 8]
List 2: [4, 9] 
List 3: [1, 7, 10] 
List 4: [3, 6]
```

1.	Initialize the next array with all 0's to keep track of the current element being considered in each list.

    •  	next = [0, 0, 0, 0] (Initially, we are considering the first element in each list.)
2.	Find the indices of the lists containing the minimum (min_i) and maximum (max_i) elements among the elements pointed to by the next array.

    •	min_i = 2 (List 3 contains the minimum element, which is 1.)
    
    •	max_i = 1 (List 2 contains the maximum element, which is 9.)
3.	Calculate the range formed by the maximum and minimum elements found above and check if it's smaller than the previous maximum range.

    •	Current range: [1, 9] (Minimum from List 3, Maximum from List 2)
    
    •	Update the maximum range to be [1, 9].
4.	Increment the pointer in the list that contains the minimum element (min_i).
    •	Increment next[2] from 0 to 1.
5.	Repeat steps 2 to 4 until any of the lists gets exhausted.

    •	**Iteration 2**:
    
        •	min_i = 0 (List 1 contains the minimum element, which is 2.)
        •	max_i = 1 (List 2 still contains the maximum element, which is 9.)
        •	Current range: [2, 9]
        •	The current range is not smaller than the maximum range [1, 9].
        •	Increment next[0] from 1 to 2.
    
    •	**Iteration 3**:
    
        •	min_i = 0 (List 1 still contains the minimum element, which is 5.)
        •	max_i = 1 (List 2 still contains the maximum element, which is 9.)
        •	Current range: [5, 9]
        •	The current range is not smaller than the maximum range [1, 9].
        •	Increment next[0] from 2 to 3.
    
    
    •	**Iteration 4**:
    
        •	min_i = 3 (List 4 contains the minimum element, which is 3.)
        •	max_i = 1 (List 2 still contains the maximum element, which is 9.)
        •	Current range: [3, 9]
        •	The current range is not smaller than the maximum range [1, 9].
        •	Increment next[3] from 0 to 1.
    •	**Iteration 5**:
    
        •	min_i = 3 (List 4 still contains the minimum element, which is 6.)
        •	max_i = 1 (List 2 still contains the maximum element, which is 9.)
        •	Current range: [6, 9]
        •	The current range is not smaller than the maximum range [1, 9].
        •	Increment next[3] from 1 to 2.
    •	**Iteration 6**:
    
        •	min_i = 0 (List 1 still contains the minimum element, which is 8.)
        •	max_i = 1 (List 2 still contains the maximum element, which is 9.)
        •	Current range: [8, 9]
        •	The current range is not smaller than the maximum range [1, 9].
        •	Increment next[0] from 3 to 4.
    •	**Iteration 7**:
    
        •	min_i = 0 (List 1 still contains the minimum element, which is 8.)
        •	max_i = 3 (List 4 contains the maximum element, which is 9.)
        •	Current range: [8, 9]
        •	The current range is not smaller than the maximum range [1, 9].
        •	Increment next[0] from 4 to 5.
    •	**Iteration 8**:
    
        •	min_i = 0 (List 1 still contains the minimum element, which is 8.)
        •	max_i = 3 (List 4 still contains the maximum element, which is 6.)
        •	Current range: [8, 6]
        •	The current range is smaller than the maximum range [1, 9].
        •	Increment next[0] from 5 to 6.
    •	**Iteration 9 (Exit)**:
    
        •	List 1 is exhausted (no more elements).
        •	Terminate the process.
6.	The algorithm stops when any list gets exhausted or when all elements in the given lists have been exhausted.
7.	The minimum range found is [6, 8].
8.	Return the minimum range, which is [6, 8].
So, the minimum range that can be formed by selecting elements from the given lists is [8, 6].

### Code:
```
#include <vector>
#include <climits>

class Solution {
public:
    std::vector<int> smallestRange(std::vector<std::vector<int>>& nums) {
        int minx = 0;
        int miny = INT_MAX;
        std::vector<int> next(nums.size(), 0);
        bool flag = true;
        for (int i = 0; i < nums.size() && flag; i++) {
            for (int j = 0; j < nums[i].size() && flag; j++) {
                int min_i = 0, max_i = 0;
                for (int k = 0; k < nums.size(); k++) {
                    if (nums[min_i][next[min_i]] > nums[k][next[k]])
                        min_i = k;
                    if (nums[max_i][next[max_i]] < nums[k][next[k]])
                        max_i = k;
                }
                if (miny - minx > nums[max_i][next[max_i]] - nums[min_i][next[min_i]]) {
                    miny = nums[max_i][next[max_i]];
                    minx = nums[min_i][next[min_i]];
                }
                next[min_i]++;
                if (next[min_i] == nums[min_i].size()) {
                    flag = false;
                }
            }
        }
        return {minx, miny};
    }
};
```

### Complexity Analysis:


**Time complexity** of the algorithm is ```O(n*m)```, where n is the total number of elements in all the lists and m is the number of lists. This is because the algorithm needs to traverse over the next array, which is of length m, for all the elements of the given lists.

**Space complexity** of the algorithm is ```O(m)```, because it needs to store the next array, which is of size m.

Here is a more detailed explanation of the time complexity:

In the worst case, the algorithm will need to traverse over the next array for all the elements of the given lists. This is because the minimum element in the next array may be the last element in the list.

The number of times the algorithm needs to traverse over the next array is equal to the number of elements in the next array, which is m.

The number of times the algorithm needs to traverse over the next array for all the elements of the given lists is equal to the product of the number of elements in the next array and the number of elements in all the lists, which is m*n.

Therefore, the time complexity of the algorithm is ```O(m*n)```.

Here is a more detailed explanation of the space complexity:

The algorithm needs to store the next array, which is of size m.

Therefore, the space complexity of the algorithm is ```O(m)```.
