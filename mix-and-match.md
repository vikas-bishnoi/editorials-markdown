# Mix and Match: Discovering Common Factors

You have a list of positive whole numbers called "nums". Find out how many unique common factors you can get from different choices you make from the "nums" list.

**Note**: A "common factor" is a whole number that can divide each number in a list without leaving a remainder.

For example, in the list [4, 6, 16], the common factor is 2.
You can pick some or all numbers from the "nums" list without changing their order.

## Intuition
1.	We know that a number can only be a valid answer, if some combination of its multiples results in that number being the GCD. 
2.	We also know that if some combination yields this result, then taking all the multiples of this number will also do so.
3.	We also know that we can iterate over all multiples of all the number up to a number x in ```O(x*log(x))``` using Sieve of Eratosthenes.

## Approach

1.	First make an Existence array of size Amax.
2.	For each number till Amax, visit all its multiples that exist and calculate their GCD.
If this GCD is equal to the number, then this number is a possibility. So, add one to our answer.

## Example: 
**Input**:

```
3
6 10 3 
```
**Output**: 
```
5
```

## Explanation: 

GCD of certain numbers cannot be greater than the minimum number. Also, the GCD remains the same or decreases after adding a new number. So, we checked for all the possible GCD’s, if this is possible then add one to the answer. Now, for a specific GCD, if we take all the numbers who are multiple of taken GCD, we got the minimum possible GCD which is multiple of taken GCD.

## Code:
```
#include <bits/stdc++.h> 
using namespace std;


int main() {
    
    int n;
    cin>>n; 
    int nums[n];
    for(int i=0; i<n; i++){
        cin>>nums[i];
    }
    
    
    int a[200001]={0};
    for(int i=0; i < n; i++){
        a[nums[i]]++;
    }
    
    
    int ans=0;
    for(int i=1; i<=200000; i++){
        int curr=0;
        for(int j=i; j<=200000; j+=i){
            if(a[j]){
                curr=__gcd(curr,j);
            }
        }
        if(curr==i){
            ans++; 
        }
    }
    
    
    cout << ans;


    return 0;
}
```


**Time complexity**:  O(N + max(A[i]) * log(max(A[i])))  
According to Sieve of Eratosthenes, X/1 + X/2 + X/3 + ….. + X/X = Xlog(X)

**Space complexity**: O(N + max(A[i]))