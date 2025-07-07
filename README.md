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

## 3307. Find the K-th Character in String Game II

# Alice and Bob are playing a game. Initially, Alice has a string word = "a".You are given a positive integer k. You are also given an integer array operations, where operations[i] represents the type of the ith operation.Now Bob will ask Alice to perform all operations in sequence:If operations[i] == 0, append a copy of word to itself.If operations[i] == 1, generate a new string by changing each character in word to its next character in the English alphabet, and append it to the original word. For example, performing the operation on "c" generates "cd" and performing the operation on "zb" generates "zbac".Return the value of the kth character in word after performing all the operations.Note that the character 'z' can be changed to 'a' in the second type of operation.
```
t(n)= 	O(log k)
s(n)= 	O(log k)

class Solution {
    public char kthCharacter(long k, int[] operations) {
        int ans=0;
        k--; //convert to 0 based indexing..
        String binary_conv=Long.toBinaryString(k); //convert number to binary digit & stored as string
        int index=0;
        for(int i=binary_conv.length()-1;i>=0;i--){
            if (binary_conv.charAt(i)=='1'){
                ans+=operations[index];
            }
            index++;
        }
        return (char)('a'+(ans%26)); //get the kth index in character
    }
}

t(n)=O(log k)
s(n)=O(1)

class Solution {
    public char kthCharacter(long k, int[] operations) {
        int ans=0;
        k--;//0 based indexing
        //using bit manipulations and with optimized one...
        int index=0;//tracks which bit we are checking
        while(k>0){
            if ((k&1)==1){ //check if the last bit of k is 1
                ans+=operations[index]; //if so add it
            }
            k>>=1;//rightshift to remove the 1 and move to next 
            index++;
        }
        return (char)('a'+(ans%26)); 
    }
}

```

## 1394. Find Lucky Integer in an Array
# Given an array of integers arr, a lucky integer is an integer that has a frequency in the array equal to its value.Return the largest lucky integer in the array. If there is no lucky integer return -1.

```
t(n)=O(n)
s(n)=O(n)

class Solution {
    public int findLucky(int[] arr) {
        //frequency of an array refers to how many times each element appears in the array.
        HashMap<Integer,Integer> map=new HashMap<>();
        for(int i:arr){ // Count frequency of each number
            map.put(i,map.getOrDefault(i,0)+1);
        }
        int res=-1;
        for(int num:map.keySet()){ //Check if frequency == number itself
            if (num==map.get(num)){
                res=Math.max(res,num); // take the largest lucky number
            }
        }
        return res;
    }
}

```
## 7. Reverse Integer

# Given a signed 32-bit integer x, return x with its digits reversed. If reversing x causes the value to go outside the signed 32-bit integer range [-231, 231 - 1], then return 0.Assume the environment does not allow you to store 64-bit integers (signed or unsigned).
```
t(n)= O(1)
s(n)= O(1)

class Solution {
    public int reverse(int x) {
        int rev=0;

        while(x!=0){
            int digit=x%10;
            x=x/10;
            //overflow condition...
            if (rev>Integer.MAX_VALUE/10 || rev==Integer.MAX_VALUE && digit>7){
                return 0;
            }
            if (rev<Integer.MIN_VALUE/10|| rev==Integer.MIN_VALUE && digit<-8){
                return 0;
            }
            //.......
            rev=rev*10+digit;
        }
        return rev;
    }
}

```

## Armstrong number
ex; 153= 1^3+5^3+3^3=1+125+27== 153..

```
t(n)=O(d)
s(n)=O(1)

public class ArmstrongRange {

    public static void main(String[] args) {
        System.out.println("Armstrong numbers between 1 and 10000:");
        for (int num = 1; num <= 10000; num++) {
            if (isArmstrong(num)) {
                System.out.println(num);
            }
        }
    }

    public static boolean isArmstrong(int num) {
        int original = num;
        int n = countDigits(num);
        int sum = 0;

        while (num > 0) {
            int digit = num % 10;
            sum += Math.pow(digit, n);
            num /= 10;
        }

        return sum == original;
    }

    public static int countDigits(int num) {
        int count = 0;
        while (num > 0) {
            num /= 10;
            count++;
        }
        return count; // handles 0 as a special case
    }
}


```
## 125. Valid Palindrome

# A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.Given a string s, return true if it is a palindrome, or false otherwise.
```
t(n)=O(n)
s(n)=O(n)

class Solution {
    public boolean isPalindrome(String s) {
        //using two pointer 
        s=s.toLowerCase().replaceAll("[^a-z0-9]",""); //convert all char to lowercases
        int start=0;
        int end=s.length()-1;
        while(start<end){
            if(s.charAt(start)!=s.charAt(end)){
                return false;
            }
            start++;
            end--;
        }
        return true;
    }
}

t(n)=O(n)
s(n)=O(1)

class Solution {
    public boolean isPalindrome(String s) {
        int start = 0;
        int end = s.length() - 1;

        while (start < end) {
            // Skip non-alphanumeric characters from start
            while (start < end && !Character.isLetterOrDigit(s.charAt(start))) {
                start++;
            }

            // Skip non-alphanumeric characters from end
            while (start < end && !Character.isLetterOrDigit(s.charAt(end))) {
                end--;
            }

            // Compare characters case-insensitively
            if (Character.toLowerCase(s.charAt(start)) != Character.toLowerCase(s.charAt(end))) {
                return false;
            }

            start++;
            end--;
        }

        return true;
    }
}


```
## 509. Fibonacci Number
# The Fibonacci numbers, commonly denoted F(n) form a sequence, called the Fibonacci sequence, such that each number is the sum of the two preceding ones, starting from 0 and 1. That is,
```
t(n)= O(n)
s(n)= O(1)

class Solution {
    public int fib(int n) {
        if (n==0) return 0;
        if (n==1) return 1;
        if (n==2) return 1;
        int num1=1;
        int num2=1;
        for(int i=3;i<=n;i++){
            int num3=num1+num2;
            num1=num2;
            num2=num3;
        }
        return num2;
    }
}

```
## 1838. Frequency of the Most Frequent Element

# The frequency of an element is the number of times it occurs in an array.You are given an integer array nums and an integer k. In one operation, you can choose an index of nums and increment the element at that index by 1.Return the maximum possible frequency of an element after performing at most k operations.

```
t(n)= O(nlogn)
s(n)= O(1)

class Solution {
    public int maxFrequency(int[] nums, int k) {
        //using sorting+sliding window
        Arrays.sort(nums);    //sort the array
        int left=0;
        long total=0;
        int max_freq=0;
        for(int right=0;right<nums.length;right++){
            total+=nums[right]; //sumup the array elements
            //find the rightmost element sum with sliding window shell

            long req_sum=(long) nums[right] *(right-left+1);

            //shrink the window

            while(req_sum-total>k){
                total-=nums[left]; 
                left++;
                //update the req_sum after shrinking the window
                req_sum=(long) nums[right]*(right-left+1); 
            }

            max_freq=Math.max(max_freq,right-left+1); //find max frequency value..

        }
        return max_freq;
    }
}

```

## 349. Intersection of Two Arrays

# Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in the result must be unique and you may return the result in any order.

```
t(n)=O(n+m)
s(n)=O(n)

class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        HashSet<Integer> map1=new HashSet<>();
        HashSet<Integer> map2=new HashSet<>();

        for (int num1:nums1){
            map1.add(num1);
        }
        for(int num2:nums2){
            if (map1.contains(num2)){
                map2.add(num2);
            }
        }
        int arr[]=new int[map2.size()];
        int ind=0;
        for(int num2:map2){
            arr[ind++]=num2;
        }
        return arr;

    }
}

```

## 560. Subarray Sum Equals K

# Given an array of integers nums and an integer k, return the total number of subarrays whose sum equals to k.A subarray is a contiguous non-empty sequence of elements within an array.

```
t(n)=	O(n²)
s(n)=  O(1)

class Solution {
    public int subarraySum(int[] nums, int k) {
        int n=nums.length;
        int ind1,ind2;
        int count=0;
        for(ind1=0;ind1<n;ind1++){
            int sum=0;
            for(ind2=ind1;ind2<n;ind2++){
                sum+=nums[ind2];

                if (sum==k){
                    count++;
                }
            }
        }
        return count;
    }
}

t(n)=O(n)
s(n)=O(n)

class Solution {
    public int subarraySum(int[] nums, int k) {
// using hashmap + prefixsum...

        HashMap<Integer,Integer> map=new HashMap<>();
        map.put(0,1); //base condition..for default values...
        int sum=0;
        int count=0;

        for (int num1:nums){
            sum+=num1;

            if (map.containsKey(sum-k)){
                count+=map.get(sum-k); //count the subarray
            }
            map.put(sum,map.getOrDefault(sum,0)+1); //update the map values with the input..
        }
        return count;
    }
}

```
## 

















