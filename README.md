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

## 3304. Find the K-th Character in String Game I
# Alice and Bob are playing a game. Initially, Alice has a string word = "a".You are given a positive integer k.Now Bob will ask Alice to perform the following operation forever:Generate a new string by changing each character in word to its next character in the English alphabet, and append it to the original word.For example, performing the operation on "c" generates "cd" and performing the operation on "zb" generates "zbac".Return the value of the kth character in word, after enough operations have been done for word to have at least k characters.Note that the character 'z' can be changed to 'a' in the operation.

```
t(n)=O(k)
s(n)=O(k)

class Solution {
    public char kthCharacter(int k) {
        //have stringbuilder with initialising the value as a with the word length n as 1
        StringBuilder word=new StringBuilder("a");
        int n=1;

        while(n<k){ //boundary check untill n is lesser than k to execute the code
            n=word.length();//update the length for each iteration
            for(int i=0;i<n;i++){
                char ch=word.charAt(i); //traverse and get the each character
                if (ch=='z'){
                    word.append('a'); //if z append the word with a
                }
                else{
                    word.append((char)(ch+1)); //if it is a-y append the word with +1 ch
                }
            }
        }
        return word.charAt(k-1);//get the kth character(due to 0 based indexing)
    }
}

```

## 5. Longest Palindromic Substring
# Given a string s, return the longest palindromic substring in s.

```
t(n)=O(n^3)
s(n)=O(1)

class Solution {
    public String longestPalindrome(String s) {
        int length,start;
        for(length=s.length();length>0;length--){ //start from the longestrange
            for(start=0;start<=s.length()-length;start++){ //boundary check
                if (check(start,start+length,s)){
                    return s.substring(start,start+length);
                }
            }
        }
        return "";
    }

    public boolean check (int i, int j, String s){
        int left=i; 
        int right=j-1;
        //using two pointer

        while(left<right){
            if (s.charAt(left)!=s.charAt(right)){
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}

```
## 2. Add Two Numbers

# You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.You may assume the two numbers do not contain any leading zero, except the number 0 itself.

```
t(n)= O(max(n, m))
s(n)= O(max(n, m))

class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        return add_numbers(l1,l2,0); //recursive call function
    }

    public ListNode add_numbers(ListNode l1,ListNode l2,int carry){
        //base condition
        if (l1==null && l2==null && carry==0) return null;
        int sum=carry;
        if (l1!=null){//add l1 values
            sum+=l1.val;
            l1=l1.next;
        }
        if (l2!=null){ //add l2 values
            sum+=l2.val;
            l2=l2.next;
        }
        ListNode head=new ListNode(sum%10); //store the last digit
        head.next=add_numbers(l1,l2,sum/10); //use the first digit to sum up 
        return head; //return the new linkedlist
    }
}

```





