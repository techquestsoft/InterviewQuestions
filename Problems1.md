## Problem Solving Excercies
| Complexity Level | Question                                                                                                                                                                                                                                                                                                                        | Comments                                                                                                                                                             |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Easy             | Given an input string, reverse the string word by word. Example below. <br> Input: "the sky is blue" <br> Output: "blue is sky the"                                                                                                                                                                                             |
| Easy             | Given an array with 0s, 1s, and 2s - sort the array without actually doing sorting and any built-in functions                                                                                                                                                                                                                   |
| Easy             | Write a program that will take a string input and generate the following series. <br> Sample Input:: a3b4c2 <br> Output: aaabbbbcc <br>                                                                                                                                                                                         | Make this moderately difficult by asking to consider multiple digit integers following an alphabet.                                                                  |
| Medium           | The Two Sum Problem - You are given an array of n integers and a number k. Determine whether there is a pair of elements in the array that sums to exactly k. For example, given the array [1,3,7] and k = 8 the answer is "yes", but given k = 6 the answer is "no"                                                            | If the candidate provides O(N^2), look for efficient approaches O(N log N) OR linear time solution                                                                   |
| Medium           | Design an algorithm and write code to remove the duplicate characters in a string without using any additional buffer. Note: One or Thow additional variables fine. An extra copy of the array is not.                                                                                                                          |
| Medium           | Given a sentence, sort the words of a sentence according to the their individual lengths. If one or more words have same length then sort the words by first character of the word. <br> Input: The quick brown fox jumps over the lazy dog  <br> Output: dog fox the The lazy over brown jumps quick                           |
| Hard             | Given a pair of words, return the minimum number of characters you need to remove in order to make them into an anagram. Assume input has only lower case alphabets. <br> read, dare => 0 (already anagram) <br> reading, dare => 3 (need to remove i, n and g) <br> reading, darefff => 6 (need to remove i, n, g, f, f and f) ||
| Hard             | Given an array of numbers, arrange them in a way that yields the largest value. Fox <br/>example, if the given numbers are {54, 546, 548, 60}, the arrangement 6054854654 gives the largest value. And if the given numbers are {1, 34, 3, 98, 9, 76, 45,4}, then the arrangement 998764543341, gives the largest value.        | If the problem seems tricky for the candidate to grasp in first attempt, start by asking how they can find the characters that differ between the two given strings. |

## Miscellaneous Programming
1. Imagine we operated a social network (e.g., twitter, reddit, google plus) where people can 
   share links:
   
   Alice shared: ["http://www.abc.com", "https://reddit.com", "http://google.com", "http://yahoo.
   com"]

   Bob shared: ["http://www.abc.com", "https://reddit.com", "http://google.com", "http://wikipedia.
   com"]

   Eve shared: ["http://yahoo.com", "http://nos.com"]

   we need to build a "follow" feature adn we want to offer some recommendations(so that the 
   feature gets more traction). E.g. Alice might want to follow Bob, since they have something 
   common (shared same links)

   For every user A we recommend one other such user B, that A and B have maximum number of 
   shared links in common. In our short example Bob has 3 shares in common with Alice, while Eve 
   has only 1.
   
   The task is to implement the recommendation function:

   **recommend(string userA, Map<string, string[]> shares)**
   
   **output -> string userB**
2. 
