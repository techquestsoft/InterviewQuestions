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
	Using extra space and loop : prefix and suffix product array
	Efficient in-place solution using single loop

https://github.com/techquestsoft/CoreJava_Interviews/blob/main/src/com/hi/problems/ProductOfArrayExceptItself.java

https://www.enjoyalgorithms.com/blog/product-of-array-except-self

https://www.youtube.com/watch?v=tSRFtR3pv74



