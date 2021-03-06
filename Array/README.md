# Array.java

## 243. Shortest Word Distance (Easy)
Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.

For example,
Assume that words = ["practice", "makes", "perfect", "coding", "makes"].

Given word1 = “coding”, word2 = “practice”, return 3.
Given word1 = "makes", word2 = "coding", return 1.

#### Solution
1. Solution 1: Very Naiive Soluion.
- Put indexes where word1 appears in array list 1.
- Put indexes where word2 appears in array list 2.
- Use two for loop to count minimal distance between indexes in array list 1 and indexes in array list 2.
2. Solution 2: Keep two the latest indexes where word1 and word2 appear.

## 244. Shortest Word Distance II (Medium)
This is a follow up of Shortest Word Distance. The only difference is now you are given the list of words and your method will be called repeatedly many times with different parameters. How would you optimize it?

Design a class which receives a list of words in the constructor, and implements a method that takes two words word1 and word2 and return the shortest distance between these two words in the list.

For example,
Assume that words = ["practice", "makes", "perfect", "coding", "makes"].

Given word1 = “coding”, word2 = “practice”, return 3.
Given word1 = "makes", word2 = "coding", return 1.

Note:
You may assume that word1 does not equal to word2, and word1 and word2 are both in the list.

#### Solution
Use a HashMap<String, List<Integer>> to store the indexes of each string.
When calling shortest(), get the indexes list of two strings, and calculate the shortest distance between the two lists.

## 245. Shortest Word Distance III
This is a follow up of Shortest Word Distance. The only difference is now word1 could be the same as word2.

Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.

word1 and word2 may be the same and they represent two individual words in the list.

For example,
Assume that words = ["practice", "makes", "perfect", "coding", "makes"].

Given word1 = “makes”, word2 = “coding”, return 1.
Given word1 = "makes", word2 = "makes", return 3.

Note:
You may assume word1 and word2 are both in the list.

#### Solution
When word1 equals to word2, add the code below based on 243. Shortest Word Distance (Easy):
~~~~
if (str.equals(word1) && str.equals(word2)) {
  if (pos1 <= pos2) {
    pos1 = i;
  }  else {
    pos2 = i;
  }
}
~~~~

## 73. Set Matrix Zeroes
Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in place.

click to show follow up.

Follow up:
Did you use extra space?
A straight forward solution using O(mn) space is probably a bad idea.
A simple improvement uses O(m + n) space, but still not the best solution.
Could you devise a constant space solution?

#### Solution
Use two for loop to scan the board.
When encountering a element with value 0, set the first element of the corresponding column and row as 0.
**Corner case** like [[1,1,1],[0,1,2]], need special handling of the board[0][0] element.

## 220. Contains Duplicate III
Given an array of integers, find out whether there are two distinct indices i and j in the array such that the absolute difference between nums[i] and nums[j] is at most t and the absolute difference between i and j is at most k.

#### Solution
1. Naiive Solution O(nk) will get time out.
2. Top solution Using bucket from leetcode discussion O(n).
3. O(nlogk) using TreeSet.
