

# 最长公共子序列</u>

![image-20200326151343228](D:\leetcode\dp\image-6)

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        n = len(text1) #row
        m = len(text2) #col
        dp = [[0] * (m + 1) for _ in range(n + 1)]
        for i in range(n):
            for j in range(m):
                dp[i][0] = dp[0][j] = 0

        for i in range(1,n + 1):
            for j in range(1,m + 1):
                if text1[i-1] == text2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
        return dp[-1][-1]
```

![image-20200326153313940](D:\leetcode\dp\image-7)

```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        #dp 最长回文子序列个数
        # dp[i][j] represents s[i..j] longest palindromeSubseq
        n = len(s)
        #if not s or n < 2: return n

        dp = [[0] * n for _ in range(n)]
        for i in range(n):  # 从左 下 左下 计算：
            dp[i][i] = 1
        
        for i in range(n-1,-1,-1):  # 不同的状态转移对应不同的loop
            for j in range(i + 1, n): #逆序遍历
                if s[i] == s[j]:
                    dp[i][j] = dp[i+1][j-1] + 2
                else:
                    dp[i][j] = max(dp[i+1][j], dp[i][j-1])
        
        #print(dp)
        return dp[0][-1]
```

![image-20200326155119321](D:\leetcode\dp\image-8)

```python
# dp+ 回溯
# dp用来找出回文字串
# 回溯用来分割字串

class Solution:
    def partition(self, s: str) -> List[List[str]]:
        n = len(s)
        dp = [[0] * n for _ in range(n)]

        for i in range(n):
            for j in range(i + 1):
                if (s[i] == s[j]) and (i - j <= 2 or dp[j + 1][i - 1]):
                    dp[j][i] = 1
        print(dp)
        
        res = []
        def backtrack(tmp, i):
            if i == n:
                res.append(tmp)
            for j in range(i, n):
                if dp[i][j]:
                    backtrack(tmp + [s[i: j + 1]], j + 1)
        
        backtrack([],0)
        return res
```

![image-20200326161439764](D:\leetcode\dp\image-9)

```python
class Solution:
    def minCut(self, s: str) -> int:
        # dp[i] 最小分割次数

        n = len(s)
        if not s or n < 2: return 0
        valid = [[0] * n for _ in range(n)]  # valid 有效的回文串 也是个dp
        dp = [0] * n
        for i in range(n):
            for j in range(i + 1): # j 在 i 之前 s[j...i] s[j+1...i-1]
                if (s[i] == s[j]) and ( i-j <=2 or valid[j + 1][i-1]):
                    valid[j][i] = 1

        #for j in range(n):
        #    for i in range(j + 1):
        #        if (s[i] == s[j]) and (j - i <= 2 or valid[i + 1][j - 1]):
        #            valid[i][j] = 1
        #print(valid)

    # 枚举分割点 找最小dp  在s[0..i]之间枚举j 
        for i in range(1,n):
            if valid[0][i]:
                dp[i] = 0
                continue

            dp[i] = min([dp[j] + 1 for j in range(i) if valid[j + 1][i]])
        return dp[-1]
```

![image-20200326165519878](D:\leetcode\dp\image-10)

```python
class Solution:
    def longestPalindrome(self, s: str) -> int:
        if not s: return 0
        dic = collections.Counter(s)
        count = 0
        length = 0
        for val in dic.values():
            if val % 2 == 0:
                length += val
            else:
                length += val -1
                count = 1
        return length + count
```

![image-20200326172554186](D:\leetcode\dp\image-11)

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        if not nums: return 0
        n = len(nums)
        dp = [1] * n
        for i in range(1,n):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i],dp[j]+1)
        return max(dp)
```

