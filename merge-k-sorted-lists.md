# Merge K Sorted Arrays

You are given two sorted arrays arr1 and arr2 of sizes m and n, respectively. Your task is to merge these two arrays into a single sorted array, such that the resultant array is also sorted.

## Example
**Input**
```
4
1 3 5 7
4
2 4 6 8
```
**Output**
```
1 2 3 4 5 6 7 8
```
Upon merging the two arrays, we get the new sorted array as above.
## Approach 1: Brute Force
### Intuition:
A naive approach would be to simply write the values from nums2 into the end of nums1, and then sort nums1. Remember that we do not need to return a value, as we should modify nums1 in-place. While straightforward to code, this approach has a high time complexity as we're not taking advantage of the existing sorting.

### Code:
```
void merge(int* nums1, int m, int* nums2, int n) {
  for (int i = 0; i < n; i++) {
    nums1[i + m] = nums2[i];
  }
  std::sort(nums1, nums1 + m + n);
}
```
### Complexity Analysis

**Time complexity**: O((n+m) * log(n+m))

The cost of sorting a list of length x using a built-in sorting algorithm is O(xlogx). Because in this case we're sorting a list of length m+n, we get a total time complexity of O((n+m) * log(n+m)).

**Space complexity**: O(n), but it can vary.

Most programming languages have a built-in sorting algorithm that uses O(n) space.

## Approach 2: Merge Sort
### Intuition:
Because each array is already sorted, we can achieve an O(n+m) time complexity with the help of the two-pointer technique.
### Algorithm:

The algorithm you described is a simple and efficient way to merge two sorted arrays. It works by maintaining two read pointers, p1 and p2, which point to the current elements in the two input arrays, respectively. It also maintains a write pointer, p, which points to the current element in the output array.

At each iteration of the algorithm, the algorithm compares the elements at p1 and p2. If the element at p1 is smaller than or equal to the element at p2, then the algorithm writes the element at p1 to the output array and increments p1. Otherwise, the algorithm writes the element at p2 to the output array and increments p2. The algorithm then increments p.

The algorithm terminates when p reaches the end of the output array. At this point, all of the elements in the two input arrays have been merged into the output array in sorted order.

### Code:
```
void merge(int* nums1, int m, int* nums2, int n) {
  // Make a copy of the first m elements of nums1.
  int* nums1Copy = new int[m];
  for (int i = 0; i < m; i++) {
    nums1Copy[i] = nums1[i];
  }

  // Read pointers for nums1Copy and nums2 respectively.
  int p1 = 0;
  int p2 = 0;

  // Compare elements from nums1Copy and nums2 and write the smallest to nums1.
  for (int p = 0; p < m + n; p++) {
    // We also need to ensure that p1 and p2 aren't over the boundaries
    // of their respective arrays.
    if (p2 >= n || (p1 < m && nums1Copy[p1] < nums2[p2])) {
      nums1[p] = nums1Copy[p1++];
    } else {
      nums1[p] = nums2[p2++];
    }
  }

  // Delete the temporary array.
  delete[] nums1Copy;
}
```
### Complexity Analysis

**Time complexity**: O(n+m)

We are performing ```n + 2 * m``` reads and ```n + 2 * m``` writes. Because constants are ignored in Big O notation, this gives us a time complexity of O(n+m).

Space complexity: O(m)

We are allocating an additional array of length m.
