# Integer to English Words

You will be given an integer, your task is to convert it to English word representation.

## Approach: Divide and Conquer 

### Intuition:
We can visualize our number to be divided in chunks of size 3. Then all the chunks will behave in the same manner, but will just have an extra prefix. This is due to the number being in the International number system.

### Example: 
**Input**:
```156321156```

**Output**: 
```One Hundred Fifty Six Million Three Hundred Twenty One Thousand One Hundred Fifty Six```

### Explanation:
Let's simplify the problem by representing it as a set of simple sub-problems. One could split the initial integer 1234567890 on the groups containing not more than three digits 1.234.567.890. That results in representation 1 Billion 234 Million 567 Thousand 890 and reduces the initial problem to how to convert a 3-digit integer to English words. One could split further 234 -> 2 Hundred 34 into two sub-problems : convert 1-digit integer and convert 2-digit integer. The first one is trivial. The second one could be reduced to the first one for all 2-digit integers but the ones from 10 to 19 which should be considered separately.

### Code:
```
#include<bits/stdc++.h>

using namespace std ;

string first_twenty[] = { "", "One", "Two", "Three", "Four", "Five", "Six", "Seven",
                         "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen",
                         "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen",
                         "Nineteen", "Twenty" };

string tens[] = { "" , "" , "Twenty" , "Thirty" , "Forty" , "Fifty" , "Sixty" , "Seventy" , "Eighty" , "Ninety" };

string places[] = { "", " Thousand ", " Million ", " Billion " };

string three_digit ( int a )
{
    string an = "" ;

    if ( a > 99 )
    {
        an = first_twenty[a/100] + " Hundred " ; 
        a %= 100 ;
    }
    
    if ( a < 1 ) an += "" ;
    else if ( a < 21 ) an += first_twenty[a] ;  
    else an += tens[a/10] + ( ( a%10 ) ? ' ' + first_twenty[a%10] : "" ) ;

    if ( an.size() and an[an.size()-1] == ' ' ) an.pop_back() ;

    return an ;
}

int main()
{
    
    int n ; cin >> n ;
    
    if ( n == 0 )
    {
        cout << "Zero" ;
        return 0 ;
    }
    
    string ans = "" ;
    
    for ( int i = 0 ; i < 4 ; ++ i )
    {
        if ( n == 0 ) break ;
        if ( n%1000 != 0 ) ans = three_digit( n % 1000 ) + places[i] + ans ;
        n /= 1000 ;
    }
    
    if ( ans[ans.size()-1] == ' ' ) ans.pop_back() 
    cout << ans ;
    return 0 ;
 
}
```

**Time complexity**: O(logN), we are iterating on digits of a given number.

**Space complexity**: O(1)
