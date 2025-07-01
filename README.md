# leetcode_solved_problems

## 9. Palindrome Number
## Given an integer x, return true if x is a palindrome, and false otherwise.
```
t(n)= O(logx)
s(n)=O(1)
class Solution {
    public boolean isPalindrome(int x) {
        //using pure math for checking palindrome number
        if(x<0){
            return false;
        }
        int rev=0;
        int xcopy=x;
        while(x>0){
            int n=x%10;
            rev=rev*10+n;
            x=x/10;
        }
        return rev==xcopy;
    }
}

```
