# Total Maximum Apples Collected

## Approach: Sliding Window

### Intuition and Algorithm:
Algorithm is described as follows:

1.	**Initialization**:
```
•	Initialize l as the left boundary of the range, initially set to pos.
•	Initialize r as the right boundary of the range, initially set to the minimum of 200000 and pos + k.
•	Initialize rem to keep track of the remaining distance you can move within the range, initially set to k - (r - l).
•	Initialize cursum to store the current sum of apples within the range.
•	Initialize ans to store the maximum sum of apples.
```
2.	**Forward Pass**:
```
•	Calculate the initial sum of apples within the range from l to r and update cursum by iterating from l to r and adding the apples at each position to cursum.
•	Update ans with the maximum of the current ans and cursum.
```
3.	**While Loop (Forward Pass)**:
```
•	The while loop continues as long as it's possible to expand the right boundary r to the right without violating the constraint that r must not exceed pos + k.
•	In each iteration of the while loop, it adjusts l and r to the left by 1 position (effectively moving the window to the left).
•	It updates cursum by removing the apples at positions r and r - 1 from it and adding the apple at position l - 1 to it.
•	It decrements rem to account for the movement of the window.
•	If there's still room to move the right boundary to the right (i.e., rem > 0 and r hasn't reached its maximum limit), it moves r to the right and adds the apples at the new position to cursum.
•	In each iteration, it updates ans with the maximum of the current ans and cursum.
```
4.	**Backward Pass**:
```
•	After the forward pass, the code re-initializes l and r for a backward pass.
•	l is set to the maximum of (pos - k) and 0.
•	r is set to pos, the starting position.
•	rem is adjusted based on the new l and r.
•	The initial sum of apples within the new range (l to r) is calculated and stored in cursum.
•	ans is updated with the maximum of the current ans and cursum.
```
5.	**While Loop (Backward Pass)**:
```
•	Similar to the forward pass, the while loop continues as long as it's possible to expand the left boundary l to the left without violating the constraint.
•	In each iteration, it adjusts l and r to the right by 2 positions (effectively moving the window to the right).
•	It updates cursum by removing the apples at positions l and l + 1 from it and adding the apple at position r + 1 to it.
•	It increments rem to account for the movement of the window.
•	If there's still room to move the left boundary to the left (i.e., rem > 0 and l hasn't reached its minimum limit), it moves l to the left and adds the apples at the new position to cursum.
•	In each iteration, it updates ans with the maximum of the current ans and cursum.
```
6.	Finally, the code outputs the maximum sum of apples (ans).
This sliding window approach efficiently calculates the maximum sum of apples within the specified range and takes into account the distance you can move from the starting position while sliding the window.

### Example:

**Input**:
```
n = 3 (number of positions)
pos = 5 (starting position)
k = 4 (maximum distance from starting position)
positions = [2, 6, 8] (positions where apples are available)
amount = [8, 3, 6] (number of apples at each position)
```
**Initialization:**
```
ans = 0
l = 5, r = 9, rem = 0
cursum = 0
```
**Forward Pass:**
```
cursum = 9 (apples at positions 5, 6, 7, 8)
ans = 9
```
**Backward Pass:**
```
l = 2, r = 5, rem = 0
cursum = 8 (apples at positions 1, 2, 3, 4, 5)
ans = 9
```
**Output:**
```
Maximum sum of apples that can be collected within the range: 9
```

### Code:
```
#include <bits/stdc++.h> // header file includes every Standard library
using namespace std;

int main() {
  int n, pos, k;
    cin >> n >> pos >> k;
    vector<int> positions(n);
    vector<int> amount(n);
    vector<int> apples(200001);
    for(int i = 0; i < n; i++){
        cin >> positions[i] >> amount[i];
        apples[positions[i]] = amount[i];
    }

    int ans = 0;
    int l = pos, r = min(200000, pos + k), rem = k - (r - l);
    int cursum = 0;
    for(int i = l; i <= r; i++){
        cursum += apples[i];
    }
    ans = max(ans, cursum);
    while(r - 2 >= pos && l - 1 >= 0){
        cursum -= (apples[r] + apples[r - 1]);
        cursum += apples[l - 1];
        l -= 1;
        r -= 2;
        while(rem > 0 && r < 200000){
            rem--;
            cursum += apples[++r];
        }
        ans = max(ans, cursum);
    }
    
    l = max(pos - k, 0), r = pos, cursum = 0, rem = k - (r - l);
    for(int i = l; i <= r; i++){
        cursum += apples[i];
    }
    ans = max(ans, cursum);
    while(l + 2 <= pos && r + 1 <= 200000){
        cursum -= (apples[l] + apples[l + 1]);
        cursum += apples[r + 1];
        r += 1;
        l += 2;
        while(rem > 0 && l > 0){
            rem--;
            cursum += apples[--l];
        }
        ans = max(ans, cursum);
    }

    cout << ans << '\n';

    return 0;
}
```

### Complexities

**Time Complexity**: The time complexity of the code is O(n + k) where 'n' is the number of positions, and 'k' is the maximum distance you can move from the starting position. In practice, if 'k' is significantly smaller than 'n', the time complexity can be approximated as O(n).

**Space Complexity**: The space complexity of the code is O(n) primarily due to the input vectors positions and amount. The space used by the fixed-size apples array does not significantly contribute to the space complexity.

