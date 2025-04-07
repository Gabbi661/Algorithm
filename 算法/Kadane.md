
### [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组**是数组中的一个连续部分。

**示例 1：**

**输入：**nums = [-2,1,-3,4,-1,2,1,-5,4]
**输出：**6
**解释：**连续子数组 [4,-1,2,1] 的和最大，为 6 。

**示例 2：**

**输入：**nums = [1]
**输出：**1

**示例 3：**

**输入：**nums = [5,4,-1,7,8]
**输出：**23

-----
这道题使用动态规划
currentSum 保存的是以当前元素为结尾的子数组的最大和。我们在每一步比较是继续扩展当前的子数组，还是重新从当前元素开始。
maxSum 记录当前遍历过程中得到的最大和。

```java
public int maxSubArray(int[] nums) {  
    int maxSum = nums[0];  
    int curSum = nums[0];  
  
    for (int num : nums) {  
        curSum = Math.max(curSum + num, num);  
        maxSum = Math.max(maxSum, curSum);  
    }  
    return maxSum;  
}
```


### [918. 环形子数组的最大和](https://leetcode.cn/problems/maximum-sum-circular-subarray/)

给定一个长度为 `n` 的**环形整数数组** `nums` ，返回 _`nums` 的非空 **子数组** 的最大可能和_ 。

**环形数组** 意味着数组的末端将会与开头相连呈环状。形式上， `nums[i]` 的下一个元素是 `nums[(i + 1) % n]` ， `nums[i]` 的前一个元素是 `nums[(i - 1 + n) % n]` 。

**子数组** 最多只能包含固定缓冲区 `nums` 中的每个元素一次。形式上，对于子数组 `nums[i], nums[i + 1], ..., nums[j]` ，不存在 `i <= k1, k2 <= j` 其中 `k1 % n == k2 % n` 。

**示例 1：**

**输入：**nums = [1,-2,3,-2]
**输出：**3
**解释：**从子数组 [3] 得到最大和 3

**示例 2：**

**输入：**nums = [5,-3,5]
**输出：**10
**解释：**从子数组 [5,5] 得到最大和 5 + 5 = 10

**示例 3：**

**输入：**nums = [3,-2,2,-3]
**输出：**3
**解释：**从子数组 [3] 和 [3,-2,2] 都可以得到最大和 3

-----
环形数组的最大子数组分为两种情况：
- **普通的最大子数组和**：不跨越数组两端，可以直接通过 **Kadane算法** 求解。

- **环形的最大子数组和**：跨越数组两端。我们可以通过以下思路求解：
1. **总和**：首先，计算整个数组的总和 totalSum。
2. **最小子数组和**：然后，计算 **最小子数组和**。通过 **Kadane算法** 的变形来求解最小子数组和。
3. 环形的最大子数组和就是 totalSum - minSubArraySum，因为环形数组的最大子数组就是“去掉最小的子数组部分”，剩下的部分是最大子数组。

```java
public int maxSubarraySumCircular(int[] nums) {  
    int n = nums.length;  
  
    // 普通的最大子数组和 (Kadane's Algorithm)    int maxSum = kadane(nums);  
  
    // 计算整个数组的总和  
    int totalSum = 0;  
    for (int num : nums) {  
        totalSum += num;  
    }  
  
    // 计算最小子数组和 (Kadane's Algorithm on negative values)    int minSum = kadaneNegative(nums);  
  
    // 如果最小子数组和等于数组总和，说明所有元素都在最小子数组中，不能取环形子数组  
    if (totalSum == minSum) {  
        return maxSum;  
    }  
    // 返回两者中的最大值：普通最大子数组和 或 环形最大子数组和  
    return Math.max(maxSum, totalSum - minSum);  
}  
  
// Kadane's Algorithm for maximum subarray sum  
private int kadane(int[] nums) {  
    int currentSum = nums[0];  
    int maxSum = nums[0];  
    for (int i = 1; i < nums.length; i++) {  
        currentSum = Math.max(nums[i], currentSum + nums[i]);  
        maxSum = Math.max(maxSum, currentSum);  
    }  
    return maxSum;  
}  
  
// Kadane's Algorithm for minimum subarray sum  
private int kadaneNegative(int[] nums) {  
    int currentSum = nums[0];  
    int minSum = nums[0];  
    for (int i = 1; i < nums.length; i++) {  
        currentSum = Math.min(nums[i], currentSum + nums[i]);  
        minSum = Math.min(minSum, currentSum);  
    }  
    return minSum;  
}

```



