# Leetcode
# 1.两数之和 
[leetcode链接](https://leetcode-cn.com/problems/two-sum/)

```python3
class Solution:
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        dict = {}
        for i in range(nums.__len__()):
            if nums[i] in dict:
                return [dict[nums[i]], i]
            else:
                dict[target - nums[i]] = i
```
#### 提示：
1.使用一个字典去记录nums数组中每个元素离target的差值和index（键值为差值，存放的是该元素的index），然后检测数组里的元素是否存在字典的键值里，若存在则其加和可以构成target。

# 2.两数相加
[leetcode链接](https://leetcode-cn.com/problems/add-two-numbers/)

```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        l3 = ListNode(0)
        temp = l3
        carry = 0
        while l1 != None or l2 != None or carry !=0:
            a = 0
            b = 0
            if l1 != None:
                a = l1.val
                l1 = l1.next
            if l2 != None:
                b = l2.val
                l2 = l2.next
            c = a + b + carry
            if c > 9:
                carry = 1
            else:
                carry = 0
            temp.next = ListNode(c % 10)
            temp = temp.next
            
        return l3.next
```
#### 提示：
1.同时遍历两个链表，同时注意是否有进位。

# 3.无重复字符的最长子串 
[leetcode链接](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

```python3
class Solution:
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        if s == '':
            return 0
        str_length = s.__len__()
        current_words = {}
        i = 0
        j = 1
        current_words[s[i]] = 0
        max_length = current_words.__len__()
        while j != str_length:
            if s[j] not in current_words:
                current_words[s[j]] = 0
                j += 1
                length = current_words.__len__()
                max_length = max(max_length, length)
            else:
                current_words.pop(s[i])
                i += 1
        return max_length
```
#### 提示：
1.使用[i, j++]滑动窗口，同时利用一个字典去记录当前滑动窗口的单词，因为字典的查找时间效率为O（1）。
2.若当前s[j]已经存在字典中，则从字典中删除s[i]元素，直到s[j]不存在字典中，这样来保证不重复子串的属性。

# 4.寻找两个有序数组的中位数
[leetcode链接](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

```python3
class Solution:
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        nums1.extend(nums2)
        nums1.sort()
        nums = nums1
        mi = nums.__len__() // 2
        if nums.__len__() % 2 != 0:
            return nums[mi]
        else:
            return (nums[mi] + nums[mi - 1]) / 2
```
#### 提示：
1.把两个数组有序合并在同一个数组中。

# 5. 最长回文子串
[leetcode链接](https://leetcode-cn.com/problems/longest-palindromic-substring/)

```python3
class Solution:
    def expand(self, s, left, right):
        while left >= 0 and right < s.__len__() and s[left] == s[right]:
            left -= 1
            right += 1
        return left, right, right - left - 1
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        max_len = 0
        right = 0
        left = 0
        for i in range(s.__len__()):
            left1, right1, gap1 = self.expand(s, i, i)
            left2, right2, gap2 = self.expand(s, i, i + 1)
            if gap2 > gap1:
                if gap2 > max_len:
                    right = right2
                    left = left2
                    max_len = gap2
            else:
                if gap1 > max_len:
                    right = right1
                    left = left1
                    max_len = gap1
        return s[left + 1 : right]
```
#### 提示：
1.在s的每一个位置运行expand函数，检查以该点为中心的回文长度。
