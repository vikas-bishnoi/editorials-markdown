# Matrix Queries

Given a N x M integer matrix A and Q queries of form X1 Y1 X2 Y2. Print the sum of A[i][j], for X1 <= i <= X2 and Y1 <= j <= Y2.

## Example
Sample Input:
```
2 2
1 5
2 3
3
1 1 1 1
1 1 1 2
1 1 2 2
```

Sample Output:
```
1
6
11
```
Explanation:
```
Query 1: 1
Query 2: 1 + 5 = 6
Query 3: 1 + 5 + 2 + 3 = 11
```


## Approach 1: Prefix Sums
### Intuition:
We used a cumulative sum array in the 1D version. We notice that the cumulative sum is computed with respect to the origin at index 0. Extending this analogy to the 2D case, we could pre-compute a cumulative region sum with respect to the origin at (0,0)
Note that the region ```Sum(OA)``` is covered twice by both ```Sum(OB)``` and ```Sum(OC)``` We could use the principle of inclusion-exclusion to calculate ```Sum(ABCD)``` as following:
``` Sum(ABCD)=Sum(OD)−Sum(OB)−Sum(OC)+Sum(OA) ```
### Code:
```
class NumMatrix {
private:
  std::vector<std::vector<int>> dp;

public:
  NumMatrix(const std::vector<std::vector<int>>& matrix) {
    if (matrix.empty() || matrix[0].empty()) return;
    dp.resize(matrix.size() + 1, std::vector<int>(matrix[0].size() + 1, 0));
    for (int r = 0; r < matrix.size(); r++) {
      for (int c = 0; c < matrix[0].size(); c++) {
        dp[r + 1][c + 1] = dp[r + 1][c] + dp[r][c + 1] + matrix[r][c] - dp[r][c];
      }
    }
  }

  int sumRegion(int row1, int col1, int row2, int col2) {
    return dp[row2 + 1][col2 + 1] - dp[row1][col2 + 1] - dp[row2 + 1][col1] +
           dp[row1][col1];
  }
};
```

### Complexity Analysis

**Time complexity**: O(1) time per query, O(m * n) time pre-computation.
The pre-computation in the constructor takes O(m * n) time. Each sumRegion query takes O(1).

**Space complexity**: O(m * n)
The algorithm uses O(m * n) space to store the cumulative region sum.
