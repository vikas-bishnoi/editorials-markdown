# Permutation challenge

In a math class, the teacher writes down a sequence of numbers [1, 2, 3,. , n] on the board. She then tells her students, "This sequence can be arranged in n! unique ways." For example, for N=3, the arrangements are '123', '132', '213', '231', '312', and '321'.
Turning to the class, she asks, "If I give you a specific number k, can you tell me what the kth arrangement in the sorted list would be? "
Your task is to answer the teacher's concise question: What is the kth arrangement for a given N and k?

**Input** :  3 3

**Output** : 213
                
## Approach: Backtracking 

**Intuition**: 

We are given that n <= 9. Typically, problems that ask you to find all of something with low bounds can be solved with backtracking. Also, string can be treated as an array of n elements with a[i] = i .

In backtracking, we generate all solutions one element at a time. This problem asks us to generate all possible permutations, so we will generate permutations one element at a time.

To generate a permutation one element at a time, we will use an array curr representing the current permutation we are building. To start, we add the first element in nums. We have curr = [nums[0]]. We are locking in this first value and will now find all permutations starting with nums[0].

To find all permutations that start with nums[0], we add the next element, nums[1]. We now have curr = [nums[0], nums[1]]. We are locking in this second element, and we will now find all permutations that start with (nums[0], nums[1]).

This continues until we use all elements, i.e. curr.length == nums.length. Let's say that we have finished finding all permutations that start with [nums[0], nums[1]]. Now what? We backtrack by removing the nums[1], and we have curr = [nums[0]] again. Now, we add the second element that comes after nums[0], which is nums[2]. We have curr = [nums[0], nums[2]], and now we need to find all permutations that start with [nums[0], nums[2]].

Once we find all the permutations that start with [nums[0]], we backtrack by removing nums[0] from curr and adding the next element. We have curr = [nums[1]], and now we need to find all permutations that start with nums[1].

This process is very recursive in nature. Each time we add an element, we solve a new version of the problem (find all permutations that start with curr). The initial version of the problem is to find all permutations that start with [], which represents all possible permutations.

To summarize, try all numbers in the first position. For each number in the first position, try all other numbers in the second position. For each pair of numbers in the first and second positions, try all other numbers in the third position, and so on.

**Trees**:
The best way to think about the backtracking process is by modeling it as a tree. You can imagine the solution space as a tree, with each node representing a version of curr. Label each node with a number that represents the last number in curr. Moving to a child is like adding the child's label to curr .

A permutation uses each element exactly once. A node should only have children with labels representing elements not yet used in the current path.

Given nums = [1, 2, 3], here is the backtracking tree:

The root node represents an empty curr []. From the root, every node's curr represents the path taken from the root. The nodes at depth nums.length represent the answer permutations (the leaf nodes).

Solving this problem is equivalent to "traversing" this tree. The easiest way to perform the traversal is by using recursion and passing curr as an argument.

Think of each call to the recursive function as being a node in the tree. In each call, we need to iterate over the numbers that haven't been used yet (not currently in curr).
For each num that isn't already in curr, we add it to curr and then make a recursive call passing curr. Modifying curr and making a recursive call is equivalent to "traversing" to a child node in the tree.

Returning from a function call is equivalent to moving back up the tree (exactly like in a DFS). When we moved from a parent to a child, we added an element to curr. When we move from a child back to its parent, we need to remove the element we added from curr. This is the "backtracking" step.

## Algorithm: 
1. Create a function permute() with parameters as input string, starting index of the string, ending index of the string
2. Call this function with values input string, 0, size of string – 1
3. In this function, if the value of  L and R is the same then print the same string
4. Else run a for loop from L to R and swap the current element in the for loop with the inputString[L]
5. Then again call this same function by increasing the value of L by 1
6. After that again swap the previously swapped values to initiate backtracking

**Example** : 

Input: 3 3

Output: 213

Explanation: The code starts with the string "123," representing the numbers from 1 to                             3.
Strings which will be formed in order are “123”, “132”, “213”, “231”, “312”, “321”.
It enters the permute function, which recursively generate permutations of the string.
The code swaps elements and recursively calls the permute function to explore different     permutations.

Eventually, it reaches the 6th permutation in lexicographically sorted order, which is "321."
When it reaches the target (6th permutation), it prints "321."

So, the output of the code for n=3 and k=6 is indeed "321," which is the 6th permutation when considering permutations in sorted order.

## Code:
```
#include <bits/stdc++.h>
using namespace std;


void permute(string& a, int l, int r, int& curr, int k)
{   
    string x = a;
    sort( a.begin() + l, a.begin() + r + 1);
    if (l == r){
        curr++;
        if(curr == k){
            cout << a << endl;
        }
    }
    else {
        for (int i = l; i <= r; i++) {


            swap(a[l], a[i]);


            permute(a, l + 1, r, curr, k);


            swap(a[l], a[i]);
        }
    }


    a = x; 
    
}


int main()
{
    int n;
    cin >> n;


    int k;
    cin >> k;


    string str;
    for(int i = 1; i <= n; i++){
        str +='0' + i;
    }
    
    int curr=0;
    permute(str, 0, n - 1, curr, k);


    return 0;
}

```


## Time Complexity: O(N*N!)

The outer loop in the permute function runs 'n' times because it iterates over all characters in the string.

For each character, there is a recursive call to permute, which generates (n-1)! permutations for the remaining characters.

Therefore, the total number of recursive calls is n * (n-1) * (n-2) * ... * 1, which is n!. Hence, the time complexity is O(n * n!).


## Space Complexity: O(N)

The primary space usage is for the string 'x’, which is used to store a copy of 'a' for backtracking.

The space complexity is also O(n!), as in the worst case, it needs to store n! permutations, each with 'n' characters.# Permutation challenge

In a math class, the teacher writes down a sequence of numbers [1, 2, 3,. , n] on the board. She then tells her students, "This sequence can be arranged in n! unique ways." For example, for N=3, the arrangements are '123', '132', '213', '231', '312', and '321'.
Turning to the class, she asks, "If I give you a specific number k, can you tell me what the kth arrangement in the sorted list would be? "
Your task is to answer the teacher's concise question: What is the kth arrangement for a given N and k?

**Input** :  3 3

**Output** : 213
                
## Approach: Backtracking 

**Intuition**: 

We are given that n <= 9. Typically, problems that ask you to find all of something with low bounds can be solved with backtracking. Also, string can be treated as an array of n elements with a[i] = i .

In backtracking, we generate all solutions one element at a time. This problem asks us to generate all possible permutations, so we will generate permutations one element at a time.

To generate a permutation one element at a time, we will use an array curr representing the current permutation we are building. To start, we add the first element in nums. We have curr = [nums[0]]. We are locking in this first value and will now find all permutations starting with nums[0].

To find all permutations that start with nums[0], we add the next element, nums[1]. We now have curr = [nums[0], nums[1]]. We are locking in this second element, and we will now find all permutations that start with (nums[0], nums[1]).

This continues until we use all elements, i.e. curr.length == nums.length. Let's say that we have finished finding all permutations that start with [nums[0], nums[1]]. Now what? We backtrack by removing the nums[1], and we have curr = [nums[0]] again. Now, we add the second element that comes after nums[0], which is nums[2]. We have curr = [nums[0], nums[2]], and now we need to find all permutations that start with [nums[0], nums[2]].

Once we find all the permutations that start with [nums[0]], we backtrack by removing nums[0] from curr and adding the next element. We have curr = [nums[1]], and now we need to find all permutations that start with nums[1].

This process is very recursive in nature. Each time we add an element, we solve a new version of the problem (find all permutations that start with curr). The initial version of the problem is to find all permutations that start with [], which represents all possible permutations.

To summarize, try all numbers in the first position. For each number in the first position, try all other numbers in the second position. For each pair of numbers in the first and second positions, try all other numbers in the third position, and so on.

**Trees**:
The best way to think about the backtracking process is by modeling it as a tree. You can imagine the solution space as a tree, with each node representing a version of curr. Label each node with a number that represents the last number in curr. Moving to a child is like adding the child's label to curr .

A permutation uses each element exactly once. A node should only have children with labels representing elements not yet used in the current path.

Given nums = [1, 2, 3], here is the backtracking tree:

The root node represents an empty curr []. From the root, every node's curr represents the path taken from the root. The nodes at depth nums.length represent the answer permutations (the leaf nodes).

Solving this problem is equivalent to "traversing" this tree. The easiest way to perform the traversal is by using recursion and passing curr as an argument.

Think of each call to the recursive function as being a node in the tree. In each call, we need to iterate over the numbers that haven't been used yet (not currently in curr).
For each num that isn't already in curr, we add it to curr and then make a recursive call passing curr. Modifying curr and making a recursive call is equivalent to "traversing" to a child node in the tree.

Returning from a function call is equivalent to moving back up the tree (exactly like in a DFS). When we moved from a parent to a child, we added an element to curr. When we move from a child back to its parent, we need to remove the element we added from curr. This is the "backtracking" step.

## Algorithm: 
1. Create a function permute() with parameters as input string, starting index of the string, ending index of the string
2. Call this function with values input string, 0, size of string – 1
3. In this function, if the value of  L and R is the same then print the same string
4. Else run a for loop from L to R and swap the current element in the for loop with the inputString[L]
5. Then again call this same function by increasing the value of L by 1
6. After that again swap the previously swapped values to initiate backtracking

**Example** : 

Input: 3 3

Output: 213

Explanation: The code starts with the string "123," representing the numbers from 1 to                             3.
Strings which will be formed in order are “123”, “132”, “213”, “231”, “312”, “321”.
It enters the permute function, which recursively generate permutations of the string.
The code swaps elements and recursively calls the permute function to explore different     permutations.

Eventually, it reaches the 6th permutation in lexicographically sorted order, which is "321."
When it reaches the target (6th permutation), it prints "321."

So, the output of the code for n=3 and k=6 is indeed "321," which is the 6th permutation when considering permutations in sorted order.

## Code:
```
#include <bits/stdc++.h>
using namespace std;


void permute(string& a, int l, int r, int& curr, int k)
{   
    string x = a;
    sort( a.begin() + l, a.begin() + r + 1);
    if (l == r){
        curr++;
        if(curr == k){
            cout << a << endl;
        }
    }
    else {
        for (int i = l; i <= r; i++) {


            swap(a[l], a[i]);


            permute(a, l + 1, r, curr, k);


            swap(a[l], a[i]);
        }
    }


    a = x; 
    
}


int main()
{
    int n;
    cin >> n;


    int k;
    cin >> k;


    string str;
    for(int i = 1; i <= n; i++){
        str +='0' + i;
    }
    
    int curr=0;
    permute(str, 0, n - 1, curr, k);


    return 0;
}

```


## Time Complexity: O(N*N!)

The outer loop in the permute function runs 'n' times because it iterates over all characters in the string.

For each character, there is a recursive call to permute, which generates (n-1)! permutations for the remaining characters.

Therefore, the total number of recursive calls is n * (n-1) * (n-2) * ... * 1, which is n!. Hence, the time complexity is O(n * n!).


## Space Complexity: O(N)

The primary space usage is for the string 'x’, which is used to store a copy of 'a' for backtracking.

The space complexity is also O(n!), as in the worst case, it needs to store n! permutations, each with 'n' characters.
# Permutation challenge

In a math class, the teacher writes down a sequence of numbers [1, 2, 3,. , n] on the board. She then tells her students, "This sequence can be arranged in n! unique ways." For example, for N=3, the arrangements are '123', '132', '213', '231', '312', and '321'.
Turning to the class, she asks, "If I give you a specific number k, can you tell me what the kth arrangement in the sorted list would be? "
Your task is to answer the teacher's concise question: What is the kth arrangement for a given N and k?

**Input** :  3 3

**Output** : 213
                
## Approach: Backtracking 

**Intuition**: 

We are given that n <= 9. Typically, problems that ask you to find all of something with low bounds can be solved with backtracking. Also, string can be treated as an array of n elements with a[i] = i .

In backtracking, we generate all solutions one element at a time. This problem asks us to generate all possible permutations, so we will generate permutations one element at a time.

To generate a permutation one element at a time, we will use an array curr representing the current permutation we are building. To start, we add the first element in nums. We have curr = [nums[0]]. We are locking in this first value and will now find all permutations starting with nums[0].

To find all permutations that start with nums[0], we add the next element, nums[1]. We now have curr = [nums[0], nums[1]]. We are locking in this second element, and we will now find all permutations that start with (nums[0], nums[1]).

This continues until we use all elements, i.e. curr.length == nums.length. Let's say that we have finished finding all permutations that start with [nums[0], nums[1]]. Now what? We backtrack by removing the nums[1], and we have curr = [nums[0]] again. Now, we add the second element that comes after nums[0], which is nums[2]. We have curr = [nums[0], nums[2]], and now we need to find all permutations that start with [nums[0], nums[2]].

Once we find all the permutations that start with [nums[0]], we backtrack by removing nums[0] from curr and adding the next element. We have curr = [nums[1]], and now we need to find all permutations that start with nums[1].

This process is very recursive in nature. Each time we add an element, we solve a new version of the problem (find all permutations that start with curr). The initial version of the problem is to find all permutations that start with [], which represents all possible permutations.

To summarize, try all numbers in the first position. For each number in the first position, try all other numbers in the second position. For each pair of numbers in the first and second positions, try all other numbers in the third position, and so on.

**Trees**:
The best way to think about the backtracking process is by modeling it as a tree. You can imagine the solution space as a tree, with each node representing a version of curr. Label each node with a number that represents the last number in curr. Moving to a child is like adding the child's label to curr .

A permutation uses each element exactly once. A node should only have children with labels representing elements not yet used in the current path.

Given nums = [1, 2, 3], here is the backtracking tree:

The root node represents an empty curr []. From the root, every node's curr represents the path taken from the root. The nodes at depth nums.length represent the answer permutations (the leaf nodes).

Solving this problem is equivalent to "traversing" this tree. The easiest way to perform the traversal is by using recursion and passing curr as an argument.

Think of each call to the recursive function as being a node in the tree. In each call, we need to iterate over the numbers that haven't been used yet (not currently in curr).
For each num that isn't already in curr, we add it to curr and then make a recursive call passing curr. Modifying curr and making a recursive call is equivalent to "traversing" to a child node in the tree.

Returning from a function call is equivalent to moving back up the tree (exactly like in a DFS). When we moved from a parent to a child, we added an element to curr. When we move from a child back to its parent, we need to remove the element we added from curr. This is the "backtracking" step.

## Algorithm: 
1. Create a function permute() with parameters as input string, starting index of the string, ending index of the string
2. Call this function with values input string, 0, size of string – 1
3. In this function, if the value of  L and R is the same then print the same string
4. Else run a for loop from L to R and swap the current element in the for loop with the inputString[L]
5. Then again call this same function by increasing the value of L by 1
6. After that again swap the previously swapped values to initiate backtracking

**Example** : 

Input: 3 3

Output: 213

Explanation: The code starts with the string "123," representing the numbers from 1 to                             3.
Strings which will be formed in order are “123”, “132”, “213”, “231”, “312”, “321”.
It enters the permute function, which recursively generate permutations of the string.
The code swaps elements and recursively calls the permute function to explore different     permutations.

Eventually, it reaches the 6th permutation in lexicographically sorted order, which is "321."
When it reaches the target (6th permutation), it prints "321."

So, the output of the code for n=3 and k=6 is indeed "321," which is the 6th permutation when considering permutations in sorted order.

## Code:
```
#include <bits/stdc++.h>
using namespace std;


void permute(string& a, int l, int r, int& curr, int k)
{   
    string x = a;
    sort( a.begin() + l, a.begin() + r + 1);
    if (l == r){
        curr++;
        if(curr == k){
            cout << a << endl;
        }
    }
    else {
        for (int i = l; i <= r; i++) {


            swap(a[l], a[i]);


            permute(a, l + 1, r, curr, k);


            swap(a[l], a[i]);
        }
    }


    a = x; 
    
}


int main()
{
    int n;
    cin >> n;


    int k;
    cin >> k;


    string str;
    for(int i = 1; i <= n; i++){
        str +='0' + i;
    }
    
    int curr=0;
    permute(str, 0, n - 1, curr, k);


    return 0;
}

```


## Time Complexity: O(N*N!)

The outer loop in the permute function runs 'n' times because it iterates over all characters in the string.

For each character, there is a recursive call to permute, which generates (n-1)! permutations for the remaining characters.

Therefore, the total number of recursive calls is n * (n-1) * (n-2) * ... * 1, which is n!. Hence, the time complexity is O(n * n!).


## Space Complexity: O(N)

The primary space usage is for the string 'x’, which is used to store a copy of 'a' for backtracking.

The space complexity is also O(n!), as in the worst case, it needs to store n! permutations, each with 'n' characters.
# Permutation challenge

In a math class, the teacher writes down a sequence of numbers [1, 2, 3,. , n] on the board. She then tells her students, "This sequence can be arranged in n! unique ways." For example, for N=3, the arrangements are '123', '132', '213', '231', '312', and '321'.
Turning to the class, she asks, "If I give you a specific number k, can you tell me what the kth arrangement in the sorted list would be? "
Your task is to answer the teacher's concise question: What is the kth arrangement for a given N and k?

**Input** :  3 3

**Output** : 213
                
## Approach: Backtracking 

**Intuition**: 

We are given that n <= 9. Typically, problems that ask you to find all of something with low bounds can be solved with backtracking. Also, string can be treated as an array of n elements with a[i] = i .

In backtracking, we generate all solutions one element at a time. This problem asks us to generate all possible permutations, so we will generate permutations one element at a time.

To generate a permutation one element at a time, we will use an array curr representing the current permutation we are building. To start, we add the first element in nums. We have curr = [nums[0]]. We are locking in this first value and will now find all permutations starting with nums[0].

To find all permutations that start with nums[0], we add the next element, nums[1]. We now have curr = [nums[0], nums[1]]. We are locking in this second element, and we will now find all permutations that start with (nums[0], nums[1]).

This continues until we use all elements, i.e. curr.length == nums.length. Let's say that we have finished finding all permutations that start with [nums[0], nums[1]]. Now what? We backtrack by removing the nums[1], and we have curr = [nums[0]] again. Now, we add the second element that comes after nums[0], which is nums[2]. We have curr = [nums[0], nums[2]], and now we need to find all permutations that start with [nums[0], nums[2]].

Once we find all the permutations that start with [nums[0]], we backtrack by removing nums[0] from curr and adding the next element. We have curr = [nums[1]], and now we need to find all permutations that start with nums[1].

This process is very recursive in nature. Each time we add an element, we solve a new version of the problem (find all permutations that start with curr). The initial version of the problem is to find all permutations that start with [], which represents all possible permutations.

To summarize, try all numbers in the first position. For each number in the first position, try all other numbers in the second position. For each pair of numbers in the first and second positions, try all other numbers in the third position, and so on.

**Trees**:
The best way to think about the backtracking process is by modeling it as a tree. You can imagine the solution space as a tree, with each node representing a version of curr. Label each node with a number that represents the last number in curr. Moving to a child is like adding the child's label to curr .

A permutation uses each element exactly once. A node should only have children with labels representing elements not yet used in the current path.

Given nums = [1, 2, 3], here is the backtracking tree:

The root node represents an empty curr []. From the root, every node's curr represents the path taken from the root. The nodes at depth nums.length represent the answer permutations (the leaf nodes).

Solving this problem is equivalent to "traversing" this tree. The easiest way to perform the traversal is by using recursion and passing curr as an argument.

Think of each call to the recursive function as being a node in the tree. In each call, we need to iterate over the numbers that haven't been used yet (not currently in curr).
For each num that isn't already in curr, we add it to curr and then make a recursive call passing curr. Modifying curr and making a recursive call is equivalent to "traversing" to a child node in the tree.

Returning from a function call is equivalent to moving back up the tree (exactly like in a DFS). When we moved from a parent to a child, we added an element to curr. When we move from a child back to its parent, we need to remove the element we added from curr. This is the "backtracking" step.

## Algorithm: 
1. Create a function permute() with parameters as input string, starting index of the string, ending index of the string
2. Call this function with values input string, 0, size of string – 1
3. In this function, if the value of  L and R is the same then print the same string
4. Else run a for loop from L to R and swap the current element in the for loop with the inputString[L]
5. Then again call this same function by increasing the value of L by 1
6. After that again swap the previously swapped values to initiate backtracking

**Example** : 

Input: 3 3

Output: 213

Explanation: The code starts with the string "123," representing the numbers from 1 to                             3.
Strings which will be formed in order are “123”, “132”, “213”, “231”, “312”, “321”.
It enters the permute function, which recursively generate permutations of the string.
The code swaps elements and recursively calls the permute function to explore different     permutations.

Eventually, it reaches the 6th permutation in lexicographically sorted order, which is "321."
When it reaches the target (6th permutation), it prints "321."

So, the output of the code for n=3 and k=6 is indeed "321," which is the 6th permutation when considering permutations in sorted order.

## Code:
```
#include <bits/stdc++.h>
using namespace std;


void permute(string& a, int l, int r, int& curr, int k)
{   
    string x = a;
    sort( a.begin() + l, a.begin() + r + 1);
    if (l == r){
        curr++;
        if(curr == k){
            cout << a << endl;
        }
    }
    else {
        for (int i = l; i <= r; i++) {


            swap(a[l], a[i]);


            permute(a, l + 1, r, curr, k);


            swap(a[l], a[i]);
        }
    }


    a = x; 
    
}


int main()
{
    int n;
    cin >> n;


    int k;
    cin >> k;


    string str;
    for(int i = 1; i <= n; i++){
        str +='0' + i;
    }
    
    int curr=0;
    permute(str, 0, n - 1, curr, k);


    return 0;
}

```


## Time Complexity: O(N*N!)

The outer loop in the permute function runs 'n' times because it iterates over all characters in the string.

For each character, there is a recursive call to permute, which generates (n-1)! permutations for the remaining characters.

Therefore, the total number of recursive calls is n * (n-1) * (n-2) * ... * 1, which is n!. Hence, the time complexity is O(n * n!).


## Space Complexity: O(N)

The primary space usage is for the string 'x’, which is used to store a copy of 'a' for backtracking.

The space complexity is also O(n!), as in the worst case, it needs to store n! permutations, each with 'n' characters.