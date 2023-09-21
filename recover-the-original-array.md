# Recover the Original Array

Alice had a 0-indexed array arr consisting of n positive integers. She chose an arbitrary positive integer k and created two new 0-indexed integer arrays lower and higher in the following manner:

lower[i] = arr[i] - k, for every index i where 0 <= i < n

higher[i] = arr[i] + k, for every index i where 0 <= i < n

Unfortunately, Alice lost all three arrays. However, she remembers the integers that were present in the arrays lower and higher, but not the array each integer belonged to. Help Alice and recover the original array.

Given an array nums consisting of 2n integers, where exactly n of the integers were present in lower and the remaining in higher, return the original array arr. In case the answer is not unique, return any valid array.

Note: The test cases are generated such that there exists at least one valid array arr.

## Approach: Sorting and Greedy

**Intuition:** 
1. If we sort the given array and we are given a specific value of k, we can check validity of k by taking smallest element which is unvisited and visit its pair i.e. a[i] + 2*k. If it doesn’t exist for any unvisited element, that specific value of k is invalid. Similarly, we can check for every possible value of k by taking every element as a pair of a[0];

**Algorithm:** 

Read integers from standard input and store them in a vector until the end-of-file (EOF) is reached.

Sort the elements in vector a in ascending order using std::sort.

Calculate the size of the vector a and store it in the variable n.

Iterate through the elements of the vector a starting from the second element (index 1) to the last element (index n-1).

For each element a[i], calculate the value k as half of the difference between a[i] and the first element a[0]. This k represents the expected difference between the two elements in each pair.

Check if k is zero or if the difference between a[i] and the first element a[0] is not evenly divisible by 2 (i.e., (a[i] - a[0]) % 2 != 0). If either condition is true, continue to the next iteration because it's impossible to find valid pairs for this value of k.

Initialize a multiset s to store the elements of vector a. A multiset is used because it allows for efficient insertion and removal of elements while maintaining sorted order.
Initialize an empty vector ans to store the values of elements of required array.


While the multiset s is not empty (i.e., there are still elements to consider):
a. Check if s contains an element that is equal to the current element plus 2*k. If not, break out of the loop because it's not possible to find a valid pair with this value of k.

b. If such an element exists in the multiset, add the value *s.begin() + k (the candidate element) to the ans vector.

c. Remove the candidate element and the current element from the multiset s.

After the loop, check if the multiset s is empty. If it is, it means that valid pairs have been found for the current value of k, and the ans vector contains the elements forming these pairs.

Finally, print the elements in the ans vector, which represent the pairs that meet the specified conditions.

The code repeats this process for each possible pair of a[0] starting from the second element in the sorted array. When it finds a valid solution for a particular value of k, it prints the resulting pairs and breaks out of the loop.


## Example:

**Input:** 2 10 6 4 8 12

**Output:** 3 7 11 

After sorting, array becomes 2 4 6 8 10 12

The code finds every possible value of k i.e. (a[i] - a[0])/2, where i>0 and looks for pairs with the same difference. In this case, it will find the pairs (2, 4), (4, 6), and (6, 8).

The code adds half of the difference back to each pair to get the original elements:

(2 + 1) and (4 - 1) become (3, 3)

(4 + 1) and (6 - 1) become (5, 5)

(6 + 1) and (8 - 1) become (7, 7)

So, the expected output for the input "2 10 6 4 8 12" is "3 7 11".

**Code:**
```
#include <bits/stdc++.h> 
using namespace std;


int main() {
    
    vector<int> a;
    
    int x;
    while(cin >> x){
        a.push_back(x);
    }
    
    sort( a.begin(), a.end() ); 
    int n = a.size();


    for(int i = 1; i < n; i++){


        int k = (a[i] - a[0])/2;
        if( k == 0 || (a[i] - a[0])%2 )continue;


        multiset<int> s;
        vector<int> ans;


        for(int j = 0; j < n; j++){
            s.insert( a[j] );
        }
        
        while(s.size() > 0){


            if(s.find( *s.begin() + 2*k ) == s.end())break;
            ans.push_back( *s.begin() + k );


            s.erase( s.find( *s.begin() + 2*k ) );
            s.erase( s.begin() );


        }
        if(s.size() == 0){


            for( auto val: ans){
                cout << val << " ";
            }
            break;


        }
    }


    return 0;
}
```

### Time Complexity: O(N^2)
We are iterating on every possible element which can be a pair of 1st element and checking for validity of that element is O(N).
So, overall time complexity is O(N*N) 

### Space Complexity: O(N^2)
In worst case, ans vector is initialized and N times and max  number of elements it will get are N/2. So, overall space complexity is O(N^2)
