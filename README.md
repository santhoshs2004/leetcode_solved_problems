# leetcode_solved_problems

## 9. Palindrome Number
# Given an integer x, return true if x is a palindrome, and false otherwise.
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

## 367. Valid Perfect Square

# Given a positive integer num, return true if num is a perfect square or false otherwise.A perfect square is an integer that is the square of an integer. In other words, it is the product of some integer with itself. You must not use any built-in library function, such as sqrt.

```
1)
t(n)=O(1)
s(n)=O(1)
class Solution {
    public boolean isPerfectSquare(int num) {
        if ((int)Math.sqrt(num)*Math.sqrt(num)==num){
            return true;
        }
        return false;
    }
}

2)
t(n)= O(√n)
s(n)= O(1)
class Solution {
    public boolean isPerfectSquare(int num) {
        if (num==1) return true;
        for(int ind=1;ind<=num/ind;ind++){  //It loops from 1 to sqrt(num) (because ind <= num / ind is equivalent to ind <= sqrt(num))
            if (ind*ind==num){
                return true;
            }
        }
        return false;
    }
}

3) using binary search (best)
t(n)= O(logn)
s(n)= O(1)
class Solution {
    public boolean isPerfectSquare(int num) {
        //using binary search 
        if (num==1) return true;
        long low=1;
        long high=num;
        while (low<=high){
            long mid=low+(high-low)/2;
            if (mid*mid==num){
                return true;
            }
            else if (mid*mid<num){
                low=mid+1; //right search prior to current mid value
            }
            else{
                high=mid-1; //left search
            }
        }
        return false;
    }
}

```

## 1. Two Sum
# Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.You may assume that each input would have exactly one solution, and you may not use the same element twice.You can return the answer in any order.

```
1)
t(n)= O(n²)
s(n)= O(1)

class Solution {
    public int[] twoSum(int[] nums, int target) {
        int i,j;   //using simple brute force method....
        int n=nums.length;
        for(i=0;i<n;i++){
            for(j=i+1;j<n;j++){
                if (nums[j]==target-nums[i]){
                    return new int[]{i,j};
                }
            }
        }
        return new int[]{};
    }
}

2)

t(n)= O(n)
s(n)= O(n)
class Solution {
    public int[] twoSum(int[] nums, int target) {
        //using hashMap....technique
        HashMap<Integer,Integer> map=new HashMap<>();
        int n=nums.length;
        for (int i=0;i<n;i++){
            int diff=target-nums[i];
            if (map.containsKey(diff)){
                return new int[]{map.get(diff),i};
            }
            else{
                map.put(nums[i],i);
            }
        }
        return new int[]{};
    }
}

```

## 328. Odd Even Linked List

# Given the head of a singly linked list, group all the nodes with odd indices together followed by the nodes with even indices, and return the reordered list.The first node is considered odd, and the second node is even, and so on.Note that the relative order inside both the even and odd groups should remain as it was in the input.You must solve the problem in O(1) extra space complexity and O(n) time complexity.
```
t(n)=O(n)
s(n)=O(1)

class Solution {
    public ListNode oddEvenList(ListNode head) {
        // using two pointers
        if (head==null || head.next==null) return head; //base case
        ListNode odd= head;
        ListNode even = head.next;
        ListNode evenhead=even;
        while (even!=null && even.next!=null){
            odd.next=even.next;
            odd=odd.next; //moving the pointer to new one
            even.next=odd.next;
            even=even.next; //moving the pointer to new one
        }
        odd.next=evenhead; //connecting odd and even 
        return head;
    }
}

```

## 53. Maximum Subarray
# Given an integer array nums, find the subarray with the largest sum, and return its sum.
```
t(n)=O(n)
s(n)=O(1)

class Solution {
    public int maxSubArray(int[] nums) {
        //using kadanes algorithm...
        int curr_sum=nums[0];
        int maxsum=nums[0];
        for (int i=1;i<nums.length;i++){
            curr_sum=Math.max(nums[i],curr_sum+nums[i]);
            maxsum=Math.max(maxsum,curr_sum);
        }
        return maxsum;
    }
}

```
















