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
## 2302. Count Subarrays With Score Less Than K

# The score of an array is defined as the product of its sum and its length.For example, the score of [1, 2, 3, 4, 5] is (1 + 2 + 3 + 4 + 5) * 5 = 75.Given a positive integer array nums and an integer k, return the number of non-empty subarrays of nums whose score is strictly less than k.A subarray is a contiguous sequence of elements within an array.

```
t(n)= O(N)
s(n)= O(1)

class Solution {
    public long countSubarrays(int[] nums, long k) {
        //using sliding window method..
        int n=nums.length;
        int left=0;
        int right;
        long sum=0;
        long count=0;

        for(right=0;right<n;right++){
            sum+=nums[right]; //sum the array values

            while((sum*(right-left+1))>=k){ //shrink the window size if exceeds >=k
                sum-=nums[left];
                left++;
            }
            count+=(right-left+1); //count the subarrays
        }
        return count;
}
    }
    
```
## 881. Boats to Save People
# You are given an array people where people[i] is the weight of the ith person, and an infinite number of boats where each boat can carry a maximum weight of limit. Each boat carries at most two people at the same time, provided the sum of the weight of those people is at most limit.Return the minimum number of boats to carry every given person.

```
t(n)= O( log n)
s(n)= O(1)

class Solution {
    public int numRescueBoats(int[] people, int limit) {
        Arrays.sort(people); //sort the array
        int count=0;
        //using two pointer approach
        int left=0;
        int right=people.length-1;
        while(left<=right){
            if (people[left]+people[right]<=limit){
                left++;
            }
            right--;
            count++;
        }
        return count;
    }
}

```

## 268. Missing Number

# Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.
```
t(n)= O(n)
s(n)=O(1)

class Solution {
    public int missingNumber(int[] nums) {
        int n=nums.length;
        int sum=0;;
        int val=(n*(n+1))/2; //sum of n numbers
        for(int sum1:nums){
            sum+=sum1;
        }
        return val-sum; //missing number in the array
    }
}


```

## 27. Remove Element

# Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:Change the array nums such that the first k elements of nums contain the elements which are not equal to val. The remaining elements of nums are not important as well as the size of nums.Return k.

```
t(n)= O(n)
s(n)= O(1)

class Solution {
    public int removeElement(int[] nums, int val) {
        //using two pointer approach..
        int k=0;

        for(int i=0;i<nums.length;i++){
            if (nums[i]!=val){ //apply the condition...
                nums[k++]=nums[i];
            }
           
        }
        return k;
    }
}

```

## 283. Move Zeroes

# Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements.Note that you must do this in-place without making a copy of the array.

```
t(n)= O(n)
s(n)= O(1)

class Solution {
    public void moveZeroes(int[] nums) {
        //swap the elements
        //two pointer approach..
        int left=0;
        for(int right=0;right<nums.length;right++){
            if (nums[right]!=0){
                int temp=nums[right];
                nums[right]=nums[left];
                nums[left]=temp;
                left++;
            }
        }
        
    }
}

```

## 239. Sliding Window Maximum

# You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.Return the max sliding window.
```
t(n)=O(n*k)
s(n)=O(1)

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        //brute force approach...
        int n=nums.length;
        if (n==0 || k==0){
            return new int[0]; //base case
        }
         int []res=new int[n-k+1]; //create an empty array with window size

        for(int i=0;i<=n-k;i++){    //control start index of each window...
            int max=nums[i];        //initialise the default values as maximum and then compare
            for(int j=i;j<i+k;j++){    //ensures there is window of k values are traversed...
                if (nums[j]>max){     //compare the max val
                    max=nums[j];
                }
            }
            res[i]=max; //store the max val in the array after every iteration
        }
        return res;

    }
}


t(n)=O(n)
s(n)=O(k)




```
 
## 16. 3Sum Closest
# Given an integer array nums of length n and an integer target, find three integers in nums such that the sum is closest to target.Return the sum of the three integers.You may assume that each input would have exactly one solution.

```
t(n)= 	O(n^2)
s(n)=   O(1)

class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        //using three pointers
        int n=nums.length;
        int closestsum=nums[0]+nums[1]+nums[2]; //sum the values
        //i= first pointer
        for (int i=0;i<n-2;i++){ //go for 3 sum indexes
            int left=i+1; //second pointer
            int right=n-1; //third pointer

            while(left<right){
                int sum=nums[i]+nums[left]+nums[right];
                if (sum==target){
                    return sum;
                }

                if (Math.abs(sum-target)<Math.abs(closestsum-target)){
                    closestsum= sum;
                }

                if (sum<target) left++;
                else right--;
            }

        }
        return closestsum;
        
    }
}

```

## 28. Find the Index of the First Occurrence in a String

# Given two strings needle and haystack, return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.
```
t(n)= O((n - m + 1) * m)
s(n)=O(1)

class Solution {
    public int strStr(String haystack, String needle) {
        //calculate the length of the each string
        int n=haystack.length();
        int m=needle.length();

        for(int i=0;i<=n-m;i++){
            //inbuilt operations....for comparing the string....
            if (haystack.substring(i,i+m).equals(needle)){
                return i;
            }
        }
        return -1;
    }
}

```
## 206. Reverse Linked List

# Given the head of a singly linked list, reverse the list, and return the reversed list.
```
t(n)=O(n)
s(n)=O(n)

class Solution {
    public ListNode reverseList(ListNode head) {
        //RECURSIVE FUNCTION
        if (head==null || head.next==null) return head; //base cases...
        ListNode newhead= reverseList(head.next); //recursive call...
        head.next.next=head; //changes to reverse flow
        head.next=null; //deletes the old chain
        return newhead; //return the new head...

    }
}

```
## 88. Merge Sorted Array

# You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.Merge nums1 and nums2 into a single array sorted in non-decreasing order.The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.
```
t(n)= O((m+n) log (m+n)) 
s(n)= O(m+n) 

class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        //brute force method
        int [] temp=new int[m+n];
        for(int i=0;i<m;i++){
            temp[i]=nums1[i];
        }
        for(int j=0;j<n;j++){
            temp[j+m]=nums2[j];
        }
        Arrays.sort(temp);

        for(int i=0;i<n+m;i++){
            nums1[i]=temp[i];
        }
        
    }
}



```

## 387. First Unique Character in a String

# Given a string s, find the first non-repeating character in it and return its index. If it does not exist, return -1.

```
t(n)=O(n)
s(n)=O(1)

class Solution {
    public int firstUniqChar(String s) {
        //using hashmap...
        HashMap<Character,Integer> map=new HashMap<>();
        int n=s.length();
        for(int i=0;i<n;i++){ //store the character in the map..
            char ch=s.charAt(i);
            map.put(ch,map.getOrDefault(ch,0)+1);
        }
        for(int i=0;i<n;i++){
            if (map.get(s.charAt(i))==1){//find the unique character as the count is 1 in map..
                return i;
            }
        }
        return -1;


    }
}

```

## 452. Minimum Number of Arrows to Burst Balloons
# There are some spherical balloons taped onto a flat wall that represents the XY-plane. The balloons are represented as a 2D integer array points where points[i] = [xstart, xend] denotes a balloon whose horizontal diameter stretches between xstart and xend. You do not know the exact y-coordinates of the balloons.Arrows can be shot up directly vertically (in the positive y-direction) from different points along the x-axis. A balloon with xstart and xend is burst by an arrow shot at x if xstart <= x <= xend. There is no limit to the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.Given the array points, return the minimum number of arrows that must be shot to burst all balloons.
```
t(n)=O (nlog n)
s(n)=O(1)

class Solution {
    public int findMinArrowShots(int[][] points) {
        //sort the array
        Arrays.sort(points,(a,b)->Integer.compare(a[1],b[1]));
        int n=points.length;
        //get default arrow as 1;
        int min_arrow=points[0][1];
        int arrows=1; //minimum one arrow required to burst the balloons
        for(int i=1;i<n;i++){
            if (points[i][0]>min_arrow){
                arrows++;
                min_arrow=points[i][1];
            }
        }
        return arrows;
    }
}

```
##  4. Median of Two Sorted Arrays

# Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.The overall run time complexity should be O(log (m+n)).
```
t(n)= O(log(n+m))
s(n)= O(1)

class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int n=nums1.length;
        int m=nums2.length;
        int []arr=new int[m+n];
        int k=0;
        for(int i=0;i<n;i++){
            arr[k++]=nums1[i];
        }
        for(int i=0;i<m;i++){
            arr[k++]=nums2[i];
        }
        Arrays.sort(arr);
        int len=arr.length;
        if (len%2==1){
            return (double) arr[len/2];
        }
        else{
            int middle1= arr[len/2-1];
            int middle2=arr[len/2];
            return (double)(((double) middle1+(double) middle2)/2);
        }
    }
}

```
## 160. Intersection of Two Linked Lists

# Given the heads of two singly linked-lists headA and headB, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return null.For example, the following two linked lists begin to intersect at node c1:
```
t(n)=O(n+m)
s(n)=O(1)


public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA==null ||headB==null) return null;
        //using two pointer...approach....
        ListNode a=headA;
        ListNode b=headB;

        while(a!=b){
            a=a==null?headB:a.next;
            b=b==null?headA:b.next;
        }
        return a;
    }
}
```
## 338. Counting Bits

# Given an integer n, return an array ans of length n + 1 such that for each i (0 <= i <= n), ans[i] is the number of 1's in the binary representation of i.

```
t(n)=O(n)
s(n)=O(n)

class Solution {
    public int[] countBits(int n) {
        int []arr=new int[n+1];

        for(int i=0;i<=n;i++){
            //i>>1 - right shift..
            //i&1 - bitwise and operation...
            arr[i]=arr[i>>1]+(i&1); 
        }
        return arr;
    }
}
```
## 34. Find First and Last Position of Element in Sorted Array

# Given an array of integers nums sorted in non-decreasing order, find the starting and ending position of a given target value.If target is not found in the array, return [-1, -1].You must write an algorithm with O(log n) runtime complexity.
```
t(n)= O(n)
s(n)= O(1)

class Solution {
    public int[] searchRange(int[] nums, int target) {
        //linear search.....
        int ptr1=-1;
        int ptr2=-1;

        for(int i=0;i<nums.length;i++){
            if (nums[i]==target){
                if (ptr1==-1) ptr1=i;
                ptr2=i;
            }
        }
        return new int[]{ptr1,ptr2};
    }
}
```

## 771. Jewels and Stones

# You're given strings jewels representing the types of stones that are jewels, and stones representing the stones you have. Each character in stones is a type of stone you have. You want to know how many of the stones you have are also jewels.Letters are case sensitive, so "a" is considered a different type of stone from "A".

```
t(n)= O(m+n)
s(n)= O(m)

class Solution {
    public int numJewelsInStones(String jewels, String stones) {
        //using hashset...approach....
        int n=jewels.length();
        int m=stones.length();

        HashSet<Character> jewelset=new HashSet<>(); //hashset

        for(char ch:jewels.toCharArray()){
            jewelset.add(ch); //add jewels character in the set
        }
        int count=0;

        for(char st:stones.toCharArray()){
            if (jewelset.contains(st)){ //if jewel character is in stones,increment the count..
                count++;
            }
        }
        return count;

    }
}


t(n)=O(m*n)
s(n)=O(1)


class Solution {
    public int numJewelsInStones(String jewels, String stones) {
        //using array...
        int m=jewels.length();
        int n=stones.length();
        int count=0;
        for(int i=0;i<m;i++){
            char ch=jewels.charAt(i);
            for(int j=0;j<n;j++){
                if (stones.charAt(j)==ch){
                    count++;
                }
            }
        }
        return count;
    }
}

```

## 524. Longest Word in Dictionary through Deleting

# Given a string s and a string array dictionary, return the longest string in the dictionary that can be formed by deleting some of the given string characters. If there is more than one possible result, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.

```
t(n)= O(n * k)
s(n)= O(1)

class Solution {
    public String findLongestWord(String s, List<String> dictionary) {
        String res="";
        for(String word:dictionary){
            if (issubsequence(word,s)){
                if (word.length()>res.length()||(word.length()==res.length() && word.compareTo(res)<0)){
                    res=word;
                }
            }
        }
        return res;
    }

    private boolean issubsequence(String word,String s){
        int i=0,j=0;
        while(i<word.length() && j<s.length()){
            if (word.charAt(i)==s.charAt(j)){
                i++;
            }
            j++;
        }
        return i==word.length();
    }
}

```
## 1346. Check If N and Its Double Exist

# Given an array arr of integers, check if there exist two indices i and j such that :
# i != j
# 0 <= i, j < arr.length
# arr[i] == 2 * arr[j] 
```
t(n)= O(n²)
s(n)= O(1)

class Solution {
    public boolean checkIfExist(int[] arr) {
        for(int i=0;i<arr.length;i++){
            for(int j=0;j<arr.length;j++){
                if (i!=j && arr[i]==2*arr[j]){
                    return true;
                }
            }
        }
        return false;
    }
}

```

## 151. Reverse Words in a String

# Given an input string s, reverse the order of the words.A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.Return a string of the words in reverse order concatenated by a single space.Note that s may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

```
t(n)=O(n)
s(n)=O(n)

class Solution {
    public String reverseWords(String s) {
        String [] words=s.trim().split("\\s+"); //trim and split the string
        StringBuilder res=new StringBuilder();
        for(int i= words.length-1;i>=0;i--){ //reverse the traversal
            res.append(words[i]);
            if (i!=0){  //give space only between the words..
                res.append(" ");
            }

        }
        return res.toString(); //return the stringbuilder res as string 
    }
}

```

## 18. 4Sum

# Given an array nums of n integers, return an array of all the unique quadruplets [nums[a], nums[b], nums[c], nums[d]] such that:
# 0 <= a, b, c, d < n
# a, b, c, and d are distinct.
# nums[a] + nums[b] + nums[c] + nums[d] == target
# You may return the answer in any order.

```
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        //brute force method
        HashSet<List<Integer>> res=new HashSet<>();
        int n=nums.length;
        Arrays.sort(nums);
        for(int i=0;i<n-3;i++){
            for(int j=i+1;j<n-2;j++){
                for(int k=j+1;k<n-1;k++){
                    for(int l=k+1;l<n;l++){
                        long sum=nums[i]+nums[j]+nums[k]+nums[l];
                        if(sum==target){
                            res.add(Arrays.asList(nums[i],nums[j],nums[k],nums[l]));
                        }
                            
                    }
                }
            }
        }
        return new ArrayList<>(res);
    }
}

```
## 













