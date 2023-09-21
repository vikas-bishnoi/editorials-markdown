# Find Minimum in Rotated Sorted Array II

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0, 1, 4, 4, 5, 6, 7] might become:
1. [4, 5, 6, 7, 0, 1, 4],  if it was rotated 4 times.
2. [0, 1, 4, 4, 5, 6, 7] , if it was rotated 7 times.
Notice that rotating an array [a[0], a[1], a[2],. , a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2],. , a[n-2]].
Given the sorted rotated array nums that may contain duplicates, return the minimum element of this array.
You must decrease the overall operation steps as much as possible.

## Approach: Variant of Binary Search

**Intuition** : Given a sorted array in ascending order (denoted as L[i]), the array is then rotated over a certain unknown pivot, (denoted as L'[i]). We are asked to find the minimum value of this sorted and rotated array, which is to find the value of the first element in the original array, i.e. L[0].

The problem resembles a common problem of finding a given value from a sorted array, to which problem one could apply the binary search algorithm. Intuitively, one might wonder if we could apply a variant of binary search algorithm to solve our problem here.
Indeed, this is the right intuition, though the tricky part is to figure out a concise solution that could work for all cases.

To illustrate the algorithm, we draw the array in a 2D dimension in the following graph, where the X axis indicates the index of each element in the array and the Y axis indicates the value of the element.

The main structure of our algorithm remains the same as the classical binary search algorithm. As a reminder, we summarize it briefly as follows:
We keep two pointers, i.e. low, high which point to the lowest and highest boundary of our search scope.

We then reduce the search scope by moving either of pointers, according to various situations. Usually we shift one of pointers to the midpoint between low and high, (i.e. pivot = (low+high)/2), which reduces the search scope down to half. This is also where the name of the algorithm comes from.

The reduction of the search scope would stop, either we find the desired element or the two pointers converge (i.e. low == high).
it.

**Algorithm**: In the classical binary search algorithm, we would compare the pivot element (i.e. nums[pivot]) with the value that we would like to locate. In our case, however, we would compare the pivot element to the element pointed by the upper bound pointer (i.e. nums[high]).

Following the structure of the binary search algorithm, the essential part remains to design the cases on how to update the two pointers.

Here we give one example on how we can break it down concisely into three cases. Note that given the array, we consider the element pointed by the low index to be on the left-hand side of the array, and the element pointed by the high index to be on the right-hand side.

***Case 1.*** nums[pivot] < nums[high]
The pivot element resides in the same half as the upper bound element.
Therefore, the desired minimum element should reside to the left-hand side of pivot element. As a result, we then move the upper bound down to the pivot index, i.e. high = pivot.

***Case 2.*** nums[pivot] > nums[high]
The pivot element resides in the different half of the array as the upper bound element.
Therefore, the desired minimum element should reside to the right-hand side of the pivot element. As a result, we then move the lower bound up next to the pivot index, i.e. low = pivot + 1.

***Case 3.*** nums[pivot] == nums[high]
In this case, we are not sure which side of the pivot that the desired minimum element would reside.

To further reduce the search scope, a safe measure would be to reduce the upper bound by one (i.e. high = high - 1), rather than moving aggressively to the pivot point.

The above strategy would prevent the algorithm from stagnating (i.e. endless loop). More importantly, it maintains the correctness of the procedure, i.e. we would not end up with skipping the desired element.

To summarize, this algorithm differs to the classical binary search algorithm in two parts:

We use the upper bound of search scope as the reference for the comparison with the pivot element, while in the classical binary search the reference would be the desired value.

When the result of comparison is equal (i.e. Case #3), we further move the upper bound, while in the classical binary search normally we would return the value immediately.

# #Code:
```
#include <bits/stdc++.h> 
using namespace std;


int main() {
    
    vector<int> nums;


    int x; 


    while(cin >> x){
        nums.push_back(x);
    }


    int n = nums.size();


    int low = 0, high = n-1;


    while (low < high) {


      int pivot = (high + low) / 2;


      if (nums[pivot] < nums[high])
        high = pivot;
      else if (nums[pivot] > nums[high])
        low = pivot + 1;
      else
        high -= 1;


    }


    cout << nums[low]; 


    return 0;
}
```

### Example:
             
**Input:** 3 2 1

**Output:** 1
             
**Explanation:** The code starts by declaring a vector nums to store the input integers and initializes integer variables x, low, and high.

It enters a while loop that reads integers from cin and pushes them into the nums vector until the end-of-file (EOF) is reached.

In this case, it reads the integers 3, 2, and 1 from the input.
After reading the input, the code calculates the size of the nums vector, which is 3 in this case.

Then, it enters another while loop to find the minimum element in the rotated sorted array using a modified binary search algorithm.

The binary search is performed by comparing the middle element (pivot) with the last element (nums[high]). Depending on the comparison, it updates the low and high indices.
In this specific input, the binary search logic will reduce the search range to find the minimum element.

Finally, it prints the minimum element, which is the result of the search.



**Time complexity:** O(logN), where N is the array's length since it is generally a binary search algorithm. 

However, in the worst case where the array contains identical elements (i.e. case #3 nums[pivot]==nums[high] ), the algorithm would deteriorate to iterate each element; as a result, the time complexity becomes O(N).

**Space Complexity:** O(1), it's a constant space solution.