https://www.javatpoint.com/linear-search

# 1. LRU Design:

	LRU, or Least Recently Used, is a popular algorithm used in computer science to manage cache memory. 
	It works on the principle of discarding the least recently used items first when the cache becomes full.

	When designing an LRU cache, there are a few key steps to consider:
	Choose a data structure: Typically, a hash table and a doubly linked list are used to implement an LRU cache. 
    The hash table is used to store key-value pairs, while the linked list is used to maintain the order in which the 
    items were accessed.
	
	Define the maximum cache size: This is the maximum number of items that the cache can hold at any given time. 
    When the cache reaches its maximum size, the least recently used item will be discarded to make room for the new item.
	
	Implement the operations: The LRU cache typically supports two operations: get and put. When an item is retrieved 
    using the get operation, it is moved to the front of the linked list to indicate that it has been accessed recently. 
    When a new item is added using the put operation, it is added to the front of the linked list and the least recently 
    used item is removed from the end of the linked list.
	
	Handle edge cases: When designing an LRU cache, it is important to consider edge cases such as cache hits, 
    cache misses, and what happens when the cache size is smaller than the number of items that need to be cached.

	Overall, designing an LRU cache requires careful consideration of data structures, maximum cache size, 
    and cache management operations.


# 2. Two Number Sum Problem Statement

	Given an array of integers, return the indices of the two numbers whose sum is equal to a given target.

	You may assume that each input would have exactly one solution, and you may not use the same element twice.
	
	Example: Given nums = [2, 7, 11, 15], target = 22.
	
	The output should be [0, 1]. 
	
	Because nums[0] + nums[1] = 2 + 7 = 9.

## (https://github.com/techquestsoft/CoreJava_Interviews/tree/main/src/com/hi/problems)

# 3. Product of array except self
    Given an array nums of n integers where n > 1, return an array output such that output[i] is 
    equal to the product of all the elements of nums except nums[i].

# Example:
	Input: [1,2,3,4]
	Output: [24,12,8,6]

**Note:** Please solve it wihout division and in O(n).

**Follow up:**
    Could you solve it with constant space complexity? (The output array 
    does not count as extra space for the purpose of the space complexity analysis.)

## Discussed Solution approaches
	Brute force solution using nested loops
	Using extra space and loop : left and right product array
	Efficient in-place solution using single array

https://github.com/techquestsoft/CoreJava_Interviews/blob/main/src/com/hi/problems/ProductOfArrayExceptItself.java

https://www.enjoyalgorithms.com/blog/product-of-array-except-self

https://www.youtube.com/watch?v=tSRFtR3pv74

# 4. Rearrange an array in maximum minimum form using Two Pointer Technique
    Given a sorted array of positive integers, rearrange the array alternately i.e first element should be a maximum value, 
    at second position minimum value, at third position second max, at fourth position second min, and so on. 
**Examples:**

Input: arr[] = {1, 2, 3, 4, 5, 6, 7} 

Output: arr[] = {7, 1, 6, 2, 5, 3, 4}

Input: arr[] = {1, 2, 3, 4, 5, 6}

Output: arr[] = {6, 1, 5, 2, 4, 3} 

https://github.com/techquestsoft/CoreJava_Interviews/blob/main/src/com/hi/problems/RearrangeSortedArrayInMaximumMinimumForm.java

https://www.geeksforgeeks.org/rearrange-array-maximum-minimum-form/

# 5. Difference Btw Arrays.sort() and Collections.sort() ?
**Arrays.sort():**
Arrays.sort() is a method residing in Arrays class. It is used to sort the Array passed to it. It can be integer array, float array, String array, Array of objects etc.
The time complexity for this method is O(nlogn) as it runs TimSort in background. TimSort algorithm makes use of the Insertion sort and the MergeSort algorithms.
sort() method is best optimized, so if you use this method instead of writing your own, you'll get best results.

**Collections.sort():**
Collections.sort() is used to sort an object which extends List interface. ArrayList and LinkedList extend List interface, so we can sort them using Collections.sort.
Collections.sort() has a time complexity of O(nlogn) as it run merge sort in background

**Overview:**
Many developers are concerned about the performance difference between java.util.Array.sort() & java.util.Collections.sort() methods. Both methods have same algorithm the only difference is type of input to them.
Collections.sort() has a input as List so it does a translation of List to array and vice versa which is an additional step while sorting. So this should be used when you are trying to sort a list.
Arrays.sort is for arrays so the sorting is done directly on the array. So clearly it should be used when you have a array available with you and you want to sort it.

# 6. Linear Search complexity
Now, let's see the time complexity of linear search in the best case, average case, and worst case. We will also see the space complexity of linear search.

Time Complexity
      |Case	|Time Complexity|
      |-------------------------|
      |Best Case	|O(1)|
      |Average Case	|O(n)|
      |Worst Case	|O(n)|
      Best Case Complexity - In Linear search, best case occurs when the element we are finding is at the first position of the array. The best-case time complexity of linear search is O(1).
      Average Case Complexity - The average case time complexity of linear search is O(n).
      Worst Case Complexity - In Linear search, the worst case occurs when the element we are looking is present at the end of the array. The worst-case in linear search could be when the target element is not present in the given array, and we have to traverse the entire array. The worst-case time complexity of linear search is O(n).
      The time complexity of linear search is O(n) because every element in the array is compared only once.

2. Space Complexity
   |Space Complexity	|O(1)|
   |------------------------|
   The space complexity of linear search is O(1).