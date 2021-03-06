## [1. Two Sum](https://leetcode.com/problems/two-sum/#/description)
### Problem Description
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

### Solution
O(n) solution:
Maintainance a map, store `(target - element) : element_index` for each element. Traverse input array, for each element if (target - element) existing, return index.

```
var twoSum = function(nums, target) {
  var l = nums.length;
  var map = {};
  for (var i = 0; i < l; i++) {
    var cur = nums[i];
    var tgt = target - cur;
    var j = map[cur];
    if (map[cur] !== undefined) {
      return [j, i];
    }
    map[tgt] = i;   // store current element's target and current index
  }
  return [];
}
```

Using Javascript built-in Map implement:
```
var twoSum = function(nums, target) {
  var myMap = new Map();
  var res = [];
  nums.some(function(val, index) {
    var num = target - val;
    if (myMap.has(val)) {
      res = [myMap.get(val), index];
      return true;
    }
    myMap.set(num, index);
  });
  return res;
}
```

## [167. Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/#/description)
### Problem Description
Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.

You may **assume** that each input would have _exactly_ one solution and you may _not use the same element twice_.
```
Input: numbers={2, 7, 11, 15}, target=9
Output: index1=1, index2=2
```

### Solution
```
var twoSum = function(numbers, target) {
  let i=0,j=numbers.length-1;
  while(numbers[i]+numbers[j]!==target){
    if(numbers[i]+numbers[j]>target) j--;
    else if(numbers[i]+numbers[j]<target) i++;
  }
  return [i+1,j+1];
}
```

## [15. 3Sum](https://leetcode.com/problems/3sum/#/description)
### Problem Description
Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note: The solution set must not contain duplicate triplets.
```
For example, given array S = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```
### Solution
Basic Idea: 
- for each i-th element of array, find the two sum satisfied target = -nums[i] in the array except ith element.
- first sort the input array, to conveniently skip repeated elements.
- use two pointers method to fine two sum in the subarray which is right of ith element.
```
class Solution {
  public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    Arrays.sort(nums);
    for (int i = 0; i < nums.length - 2; i++) {
      if (nums[i] > 0)   break;
      if (i > 0 && nums[i] == nums[i-1])   continue;    // skip same value
            
      int target = -nums[i];
      int j = i+1;
      int k = nums.length-1;
      while (j < k) {
        int sum = nums[j] + nums[k];
        if (sum == target) {
          res.add(Arrays.asList(nums[i], nums[j], nums[k]));
          j++;
          k--;
          while (j < k && nums[j] == nums[j-1])  j++;   // skip same value
          while (j < k && nums[k] == nums[k+1])  k--;   // skip same value
        } else if (sum < target) {
          j++;                    
        } else {
          k--;
        }                
      }
    }
    return res;
  }
}
```
### Java knowledge used
- Arrays.sort() : a java.util.Arrays class method, to sort an array of integers in _ascending_ order.

  Syntax:
  ```
  public static void sort(int[] arr, int from_Index, int to_Index)

  arr - the array to be sorted. 
  from_Index - the index of the first element, inclusive, to be sorted
  to_Index - the index of the last element, exclusive, to be sorted
  
  In-place sort. No value returned.
  ```
  
  Example usage
  ```
  int[] arr = {13, 7, 6, 45, 21, 9, 101, 102};
  Arrays.sort(arr);
  // Modified arr[] : [6, 7, 9, 13, 21, 45, 101, 102]
  ```
- Arrays.asList() : acts as bridge between array-based and collection-based APIs, returns a _**fixed-size list**_ backed by the specified array. The list returned by Arrays.asList cannot extend size but can use all other methods of List.

  Syntax:
  ```
  public static <T> List<T> asList(T... a)
  ```

  Example:
  ```
  List<String> list=  Arrays.asList("A","B","C","D","E");

  String a[] = new String[]{"abc","klm","xyz","pqr"};
  List list1 = Arrays.asList(a);
  ```

## [16. 3Sum Closest](https://leetcode.com/problems/3sum-closest/description/)
### Problem Description
Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.
```
  For example, given array S = {-1 2 1 -4}, and target = 1.

  The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

### Solution
- idea

  Sort + two pointers
```
  Suppose sorted input array is: v[0] v[1] v[2] ... v[i] .... v[j] ... v[k] ... v[n-2] v[n-1],
  v[i]  <=  v[j]  <= v[k] always, because we sorted our array. 
  Now, for each number v[i], we look for pairs v[j] & v[k] such that absolute value of 
  (target - (v[i] + v[j] + v[k]) is minimised. If the sum of the triplet is greater than
  the target it implies, we need to reduce our sum, so we do k--, that is we reduce the sum
  by taking a smaller number.
  Simillarly if sum of the triplet is less than the target, then we increase out sum 
  by taking a larger number, i.e. j++.
```
- Code O(n^2)
```
  class Solution {
    public int threeSumClosest(int[] nums, int target) {
      Arrays.sort(nums);

      int ans = nums[0]+nums[1]+nums[2];
      for (int i = 0; i < nums.length - 2; i++) {
        int j = i + 1, k = nums.length -1;
        while (j < k) {
          int sum = nums[i]+nums[j]+nums[k];
          if (Math.abs(target - ans) > Math.abs(target - sum)) {
            ans = sum;
            if (ans == target)    return ans;
          }
          if (sum > target)  k--;
          else j++;
        }
      }
      return ans;
    }
  }
```

## [?. 3Sum Smaller](https://leetcode.com/problems/3sum-smaller) locked
### Problem Description
Given an array of n integers nums and a target, find the number of index triplets i, j, k with 0 <= i < j < k < n that satisfy the condition nums[i] + nums[j] + nums[k] < target.
```
For example, given nums = [-2, 0, 1, 3], and target = 2.

Return 2. Because there are two triplets which sums are less than 2:

[-2, 0, 1]
[-2, 0, 3]
```
Follow up:
Could you solve it in O(n^2) runtime?

### Solution
Sort array + two pointers.
```
class Solution {
  public int threeSumSmaller(int[] nums, int target) {
    Arrays.sort(nums);
    int i, j, k, t;
    int res = 0;
    for (i = 0; i < nums.length-2; i++) {
      t = target - nums[i];
      j = i + 1;
      k = nums.length - 1;
      while (j < k) {
        if (nums[j] + nums[k] >= t)     k--;
        else {
          res += (k - j);
          j++;
        }
      }
    }
    return res;
  }
}
```

## [18. 4Sum](https://leetcode.com/problems/4sum/#/description)
### Problem Description
Given an array S of n integers, are there elements a, b, c, and d in S such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

Note: The solution set must not contain duplicate quadruplets.
```
For example, given array S = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

### Solution
idea similiar to 3Sum and 2Sum, sort + two pointers.
```
class Solution {
  public List<List<Integer>> fourSum(int[] num, int target) {
    ArrayList<List<Integer>> ans = new ArrayList<>();
    if(num.length<4)  return ans;
    Arrays.sort(num);
    for(int i=0; i<num.length-3; i++) {
      //first candidate too large, search finished
      if(num[i]+num[i+1]+num[i+2]+num[i+3]>target)  break; 

      //first candidate too small
      if(num[i]+num[num.length-1]+num[num.length-2]+num[num.length-3]<target)   continue; 

      if(i>0&&num[i]==num[i-1])continue; //prevents duplicate result in ans list
      for(int j=i+1; j<num.length-2; j++){
        if(num[i]+num[j]+num[j+1]+num[j+2]>target)  break; //second candidate too large

        //second candidate too small
        if(num[i]+num[j]+num[num.length-1]+num[num.length-2]<target)  continue; 
        if(j>i+1&&num[j]==num[j-1])  continue; //prevents duplicate results in ans list
        int low=j+1, high=num.length-1;
        while (low<high) {
          int sum = num[i]+num[j]+num[low]+num[high];
          if (sum == target) {
            ans.add(Arrays.asList(num[i], num[j], num[low], num[high]));
            while (low<high&&num[low]==num[low+1])  low++; //skipping over duplicate on low
            while (low<high&&num[high]==num[high-1])  high--; //skipping over duplicate on high
            low++;
            high--;
          }
          //move window
          else if(sum<target)  low++; 
          else high--;
        }
      }
    }
    return ans;
  }
}
```
Use threeSum and twoSum subfunction.
```
class Solution {
  public List<List<Integer>> fourSum(int[] nums, int target) {
    ArrayList<List<Integer>> res = new ArrayList<List<Integer>>();
    int len = nums.length;
    if (nums == null || len < 4)  return res;

    Arrays.sort(nums);

    int max = nums[len - 1];
    if (4 * nums[0] > target || 4 * max < target)  return res;

    int i, z;
    for (i = 0; i < len; i++) {
      z = nums[i];
      if (i > 0 && z == nums[i - 1])// avoid duplicate
        continue;
      if (z + 3 * max < target) // z is too small
        continue;
      if (4 * z > target) // z is too large
        break;
      if (4 * z == target) { // z is the boundary
        if (i + 3 < len && nums[i + 3] == z)
          res.add(Arrays.asList(z, z, z, z));
        break;
      }
      threeSumForFourSum(nums, target - z, i + 1, len - 1, res, z);
    }
    return res;
  }

  /*
  * Find all possible distinguished three numbers adding up to the target
  * in sorted array nums[] between indices low and high. If there are,
  * add all of them into the ArrayList fourSumList, using
  * fourSumList.add(Arrays.asList(z1, the three numbers))
  */
  public void threeSumForFourSum(int[] nums, int target, int low, int high, 
                                 ArrayList<List<Integer>> fourSumList, int z1) {
    if (low + 1 >= high)  return;

    int max = nums[high];
    if (3 * nums[low] > target || 3 * max < target)  return;

    int i, z;
    for (i = low; i < high - 1; i++) {
      z = nums[i];
      if (i > low && z == nums[i - 1]) // avoid duplicate
        continue;
      if (z + 2 * max < target) // z is too small
        continue;

      if (3 * z > target) // z is too large
        break;

      if (3 * z == target) { // z is the boundary
        if (i + 1 < high && nums[i + 2] == z)
          fourSumList.add(Arrays.asList(z1, z, z, z));
        break;
      }
      twoSumForFourSum(nums, target - z, i + 1, high, fourSumList, z1, z);
    }
  }

  /*
  * Find all possible distinguished two numbers adding up to the target
  * in sorted array nums[] between indices low and high. If there are,
  * add all of them into the ArrayList fourSumList, using
  * fourSumList.add(Arrays.asList(z1, z2, the two numbers))
  */
  public void twoSumForFourSum(int[] nums, int target, int low, int high, 
                               ArrayList<List<Integer>> fourSumList, int z1, int z2) {
    if (low >= high)  return;
    if (2 * nums[low] > target || 2 * nums[high] < target)  return;

    int i = low, j = high, sum, x;
    while (i < j) {
      sum = nums[i] + nums[j];
      if (sum == target) {
        fourSumList.add(Arrays.asList(z1, z2, nums[i], nums[j]));

        x = nums[i];
        while (++i < j && x == nums[i]) // avoid duplicate
          ;
        x = nums[j];
        while (i < --j && x == nums[j]) // avoid duplicate
          ;
      }
      if (sum < target)
        i++;
      if (sum > target)
        j--;
    }
    return;
  }
}
```

## [454. 4Sum II](https://leetcode.com/problems/4sum-ii/description/)
### Problem Description
Given four lists A, B, C, D of integer values, compute how many tuples (i, j, k, l) there are such that A[i] + B[j] + C[k] + D[l] is zero.

To make problem a bit easier, all A, B, C, D have same length of N where 0 ≤ N ≤ 500. All integers are in the range of -228 to 228 - 1 and the result is guaranteed to be at most 231 - 1.
```
Example:

Input:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

Output:
2

Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
```
### Solution
Idea:

Take the arrays A and B, and compute all the possible sums of two elements. Put the sum in the Hash map, 
and increase the hash map value if more than 1 pair sums to the same value.

Compute all the possible sums of the arrays C and D. If the hash map contains the opposite value of the current sum, 
increase the count of four elements sum to 0 by the counter in the map.
```
class Solution {
  public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
    Map<Integer, Integer> map = new HashMap<>();
    int len = A.length;   // 4 list have same length
    int sum, res = 0, i, j;
    for (i = 0; i < len; i++) {
      for (j = 0; j < len; j++) {
        sum = A[i] + B[j];
        map.put(sum, map.getOrDefault(sum, 0)+1);
      }
    }
        
    for (i = 0; i < len; i++) {
      for (j = 0; j < len; j++) {
        sum = -(C[i] + D[j]);
        res += map.getOrDefault(sum, 0);
      }
    }
    return res;
  }
}
```
### Java Knowledge
- map.getOrDefault : The getOrDefault() method returns the value to which the specified key is mapped, 
or defaultValue if this map contains no mapping for the key.

	Syntax : `public V getOrDefault(Object key,V defaultValue)`

	Example:
	```
	public class HashMapGetOrDefaultExample {
		public static void main(String[] args) throws InterruptedException {
			int idNum = 9756;
			HashMap<Integer, String> map = init();
			System.out.println("Student with id number " + idNum + " is "
					+ map.getOrDefault(idNum, "John Doe"));
		}

		private static HashMap<Integer, String> init() {
			// declare the hashmap
			HashMap<Integer, String> mapStudent = new HashMap<>();
			// put contents to our HashMap
			mapStudent.put(73654, "Shyra Travis");
			mapStudent.put(98712, "Sharon Wallace");
			return mapStudent;
		}
	}
	```

## [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/#/description)
### Problem Description
Given an array of integers and an integer k, you need to find the total number of continuous subarrays whose sum equals to k.
```
Example 1:
Input:nums = [1,1,1], k = 2
Output: 2
```
Note:
The length of the array is in range [1, 20,000].
The range of numbers in the array is [-1000, 1000] and the range of the integer k is [-1e7, 1e7].

### Solution
- Solution 1

  Brute force. We just need two loops (i, j) and test if SUM[i, j] = k. Time complexity O(n^2), Space complexity O(1). I bet this solution will TLE.

- Solution 2
  
  From solution 1, we know the key to solve this problem is SUM[i, j]. So if we know SUM[0, i - 1] and SUM[0, j], then we can easily get SUM[i, j]. To achieve this, we just need to go through the array, calculate the current sum and save number of all seen PreSum to a HashMap. Time complexity O(n), Space complexity O(n).
	```
    class Solution {
      public int subarraySum(int[] nums, int k) {
      // we can get any Sum[i, j] from Sum[0, i-1] and Sum[0, j].
      // so just store Sum[0, i] in map : <sum, occurrence>
      int sum = 0, res = 0;
      Map<Integer, Integer> preSum = new HashMap<>();
      preSum.put(0, 1);
      for (int i = 0; i < nums.length; i++) {
          sum += nums[i];
          if (preSum.containsKey(sum - k)) {
              res += preSum.get(sum - k);
          }
          preSum.put(sum, preSum.getOrDefault(sum, 0)+1);
      }
      return res;
    }
  }
	```
  
  run faster version:
  ```
  class Solution {
    public int subarraySum(int[] nums, int k) {
      int count = 0;
      for (int i = 1; i < nums.length; i++) {
        nums[i] += nums[i - 1];
      }
      HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
      map.put(0, 1);
      for (int num : nums) {
        if (map.containsKey(num - k)) {
          int cur = map.get(num - k);
          count += cur;
        }            
        //count += map.getOrDefault(num-k, 0);
            
        map.put(num, map.getOrDefault(num, 0) + 1);
      }    
      return count;
    }
  }
  ```
  
## [523. Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/description/)
### Problem Description
Given a list of non-negative numbers and a target integer k, write a function to check if the array has a continuous subarray of size at least 2 that sums up to the multiple of k, that is, sums up to n*k where n is also an integer.
```
Example 1:
Input: [23, 2, 4, 6, 7],  k=6
Output: True
Explanation: Because [2, 4] is a continuous subarray of size 2 and sums up to 6.

Example 2:
Input: [23, 2, 6, 4, 7],  k=6
Output: True
Explanation: Because [23, 2, 6, 4, 7] is an continuous subarray of size 5 and sums up to 42.
```
Note:
The length of the array won't exceed 10,000.
You may assume the sum of all the numbers is in the range of a signed 32-bit integer.

### Solution
We iterate through the input array exactly once, keeping track of the running sum mod k of the elements in the process. 
If we find that a running sum value at index j has been previously seen before in some earlier index i in the array, 
then we know that the sub-array (i,j] contains a desired sum. `i.e., if Sum[0,i] % k == Sum[0,j] % k, then Sum(i,j] % k == 0`.
Note : 
- any number except 0 mod 0 is undefined. So `nums = [23,2,4], k = 0` should return false.
- 0 mod 0 is 0. So `nums = [0, 0, 0], k = 0` should return true.

```
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        Map<Integer, Integer>  map = new HashMap<>();
        map.put(0, -1);    //  nums = [0, 0], k = 0
        int acc = 0;
        for (int i = 0; i < nums.length; i++) {
            acc += nums[i];
            if (k != 0) acc %= k;
            Integer prev = map.get(acc);
            if (prev != null)  {
                if (i - prev > 1)   return true;
            } else {
                map.put(acc, i);
            }
        }
        return false;
    }
}
```

## [713. Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/description/)
### Problem Description
Your are given an array of positive integers nums.

Count and print the number of (contiguous) subarrays where the product of all the elements in the subarray is less than k.
```
Example 1:
Input: nums = [10, 5, 2, 6], k = 100
Output: 8
Explanation: The 8 subarrays that have product less than 100 are: 
             [10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6].
Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.
```
Note:

0 < nums.length <= 50000.
0 < nums[i] < 1000.
0 <= k < 10^6.

### solution
- Basic Idea : two pointer slide window, product of (window) subarray.
```
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        int res = 0, product = 1, left = 0;
        for (int i = 0; i < nums.length; i++) {
            int curr = nums[i];
            if (curr >= k) {
                left = i + 1;
                product = 1;
                continue;
            }
            product *= curr;
            if(product < k) {
                res += i - left + 1;
            } else {
                while (product >= k && left < i) {
                    product /= nums[left];
                    ++left;
                }
                res += i - left + 1;
            }
        }
        return res;
    }
}
```


## [152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/description/)
### Problem Description
Find the contiguous subarray within an array (containing at least one number) which has the largest product.

For example, given the array [2,3,-2,4], the contiguous subarray [2,3] has the largest product = 6.

### Solution
```
class Solution {
  public int maxProduct(int[] nums) {
    // store the result that is the max we have found so far
    int res = nums[0];
        
    // max/min stores the max/min product of
    // subarray that ends with the current number nums[i]
    for (int i = 1, max = res, min = res; i < nums.length; i++) {
      // multiplied by a negative makes big number smaller, small number bigger
      // so we redefine the extremums by swapping them            
      if (nums[i] < 0)  {
        int tmp = max;
        max = min;
        min = tmp;
      }
            
      // max/min product for the current number is either the current number itself
      // or the max/min by the previous number times the current one            
      max = Math.max(nums[i], max*nums[i]);
      min = Math.min(nums[i], min*nums[i]);
            
      // the newly computed max value is a candidate for our global result
      res = Math.max(max, res);
    }
    return res;
  }
}
```

## [?. Maximum Size Subarray Sum Equals k](https://www.programcreek.com/2014/10/leetcode-maximum-size-subarray-sum-equals-k-java/) locked
### Problem Description
Given an array nums and a target value k, find the maximum length of a subarray that sums to k. 
If there isn't one, return 0 instead.

Note:
The sum of the entire nums array is guaranteed to fit within the 32-bit signed integer range.
```
Example 1:
Given nums = [1, -1, 5, -2, 3], k = 3,
return 4. (because the subarray [1, -1, 5, -2] sums to 3 and is the longest)
```

## [689. Maximum Sum of 3 Non-Overlapping Subarrays](https://leetcode.com/problems/maximum-sum-of-3-non-overlapping-subarrays/description/)
### Problem Description
In a given array nums of positive integers, find three non-overlapping subarrays with maximum sum.

Each subarray will be of size k, and we want to maximize the sum of all 3*k entries.

Return the result as a list of indices representing the starting position of each interval (0-indexed). 
If there are multiple answers, return the lexicographically smallest one.

Example:
```
Input: [1,2,1,2,6,7,5,1], 2
Output: [0, 3, 5]
Explanation: Subarrays [1, 2], [2, 6], [7, 5] correspond to the starting indices [0, 3, 5].
We could have also taken [2, 1], but an answer of [1, 3, 5] would be lexicographically larger.
```
Note:
nums.length will be between 1 and 20000.
nums[i] will be between 1 and 65535.
k will be between 1 and floor(nums.length / 3).

### Solution

# Knowledge Used
#### [Javascript built-in objects : Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)
- `var myMap = new Map([iterable])`

Properties | Description
---------- | ------------------------
Map.prototype.size | Returns the number of key/value pairs in the Map object.

Methods | Description
------- | ---------------------------
Map.prototype.set(key, value) | Sets the value for the key in the Map object. Returns the Map object.
Map.prototype.get(key) | Returns the value associated to the key, or undefined if there is none.
Map.prototype.delete(key) | Removes any value associated to the key and returns the value that Map.prototype.has(key) would have previously returned. Map.prototype.has(key) will return false afterwards.
Map.prototype.clear() | Removes all key/value pairs from the Map object.
Map.prototype.has(key) | Returns a boolean asserting whether a value has been associated to the key in the Map object or not.
Map.prototype.keys() | Returns a new Iterator object that contains the keys for each element in the Map object in insertion order.
Map.prototype.values() | Returns a new Iterator object that contains the values for each element in the Map object in insertion order.
Map.prototype.entries() | Returns a new Iterator object that contains an array of [key, value] for each element in the Map object in insertion order.
Map.prototype.forEach(callbackFn[, thisArg]) | Calls callbackFn once for each key-value pair present in the Map object, in insertion order. If a thisArg parameter is provided to forEach, it will be used as the this value for each callback.
Map.prototype[@@iterator]() | Returns a new Iterator object that contains an array of [key, value] for each element in the Map object in insertion order.
