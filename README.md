# 01 Binary Search + Remove Element

LeetCode 704 - 二分查找

1. 题目描述：给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

示例:
输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4

2. 解题思路：
2.1. 有序数组 + 无重复 ——> 二分法。
2.2 二分的循环退出，以及边界怎么不断缩小是难点。
2.2.1 左闭右闭[]，就是left = right，对应的边界减少时，left = mid+1, 或者right = mid-1， 因为mid已经判断过了不是一个有意义的数，没必要再进入下一次循环，我们用的闭合就代表我们要的一个有意义的数（可能是答案的数）。
2.2.2 左闭右开[)，就是left ！= right，对应的边界减少时，left = mid+1, 或者right = mid，开区间代表最后一位取不到，所以要把mid加到区间最后，因为mid已经判断过了不是一个有意义的数（不是可能的答案）。

3. 代码实现：
3.1. 二分法第一种写法-左闭右闭[left,right]
   class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length;
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] == target)
                return mid;
            else if (nums[mid] < target)
                left = mid + 1;
            else if (nums[mid] > target)
                right = mid;
        }
        return -1;
    }
}

3.2.  二分法第一种写法-左闭右闭[left,right)
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] == target)
                return mid;
            else if (nums[mid] < target)
                left = mid + 1;
            else if (nums[mid] > target)
                right = mid - 1;
        }
        return -1;
    }
}

总结：
在看视频之前，我确实对这个left<right or left <= right,和 mid or mid+1, 没有过多思考，每次都是各种试验然后碰运气。
现在明白了是一个区间的问题，选择了什么样范围的区间，就贯通的使用下去，left <= right, left = mid + 1, right = mid - 1; /left < right, left = mid + 1, right = mid;
另外还注意一下mid = left + (right - left )/2, 这样是最保险的，如果直接写(right + left )/2，如果left跟right都很大，相加可能会超过2147483647，那相加的结果将会溢出，可能变成负数。
以后遇到这类题目，还是要画一个数简易数组在旁边，然后写上开闭，模拟分类情况以后再写代码。

LeetCode 27-移除元素

1. 题目描述：给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

2.示例：
nums = [3,2,2,3], val = 3
输出：2, nums = [2,2]
解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案

2.解题思路：
要在一个数组上直接修改数据，两个for循环 ——> 双指针（快慢指针）， 指针都是index。
快指针：去找到要保留下来的数值，快指针一直往前走。for循环的index就是快指针。
慢指针：把快指针找到的值保存到慢指针的位置，然后慢指针往前移动一位，用于保存下一个值。

3. 代码实现：
class Solution {
    public int removeElement(int[] nums, int val) {
        // 快慢指针
        int slowIndex = 0;
        for (int fastIndex = 0; fastIndex < nums.length; fastIndex++) {
            if (nums[fastIndex] != val) {
                nums[slowIndex] = nums[fastIndex];
                slowIndex++;
            }
        }
        return slowIndex;
    }
}
总结：
做双指针的时候还是要画图比较好理解一些，把快慢标记出来，一步一步跑。

Leetcode 977.有序数组的平方

1. 题目描述：给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。

示例：

输入：nums = [-4,-1,0,3,10]
输出：[0,1,9,16,100]
解释：平方后，数组变为 [16,1,0,9,100]，排序后，数组变为 [0,1,9,16,100]

2. 解题思路：
   虽然不是单调递增、递减的数组，但也有规律，从两头往中间减少。因此可以考虑到使用双指针（left，right），每次比较left 和 right的大小，存储的时候从右边往左边存。谁大谁先存。

3. 代码实现：
class Solution {
    public int[] sortedSquares(int[] nums) {
        int right = nums.length - 1;
        int left = 0;
        int[] result = new int[nums.length];
        int index = result.length - 1;
        while (left <= right) {
            if (nums[left] * nums[left] > nums[right] * nums[right]) {
                // 正数的相对位置是不变的， 需要调整的是负数平方后的相对位置
                result[index] = nums[left] * nums[left];
                index--
                left++;
            } else {
                result[index] = nums[right] * nums[right];
                index--;
                right--;
            }
        }
        return result;
    }
}

Leetcode 209.长度最小的子数组

1. 题目描述：给定一个含有 n 个正整数的数组和一个正整数 s ，找出该数组中满足其和 ≥ s 的长度最小的 连续 子数组，并返回其长度。如果不存在符合条件的子数组，返回 0。

示例：

输入：s = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。

2. 解题思路：
   要求的是数组的连续片段，或者范围。类似一个窗口范围，因此考虑用滑动窗口的方法。
   滑动窗口的两个指针，right指针是先走的用于锁定终点位置，在满足题目条件时，暂停下来，然后left指针开始逐步走，每次走都判断一下题目条件是否继续满足。一直走到不能满足为止，这是窗口范围就是right- left+1；执行完一轮，fast又开始继续走。因此right在for循环里（i——>length)，left则在while循环里while(sum >= 2)。

3. 代码实现：
class Solution {

    // 滑动窗口
    public int minSubArrayLen(int s, int[] nums) {
        int left = 0;
        int sum = 0;
        int result = Integer.MAX_VALUE;
        for (int right = 0; right < nums.length; right++) {
            sum += nums[right];
            while (sum >= s) {
                result = Math.min(result, right - left + 1);
                sum -= nums[left];
                left++
            }
        }
        return result == Integer.MAX_VALUE ? 0 : result;
    }
}

Leetcode 59.螺旋矩阵II

1. 题目描述：给定一个正整数 n，生成一个包含 1 到 n^2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

示例:

输入: 3 输出: [ [ 1, 2, 3 ], [ 8, 9, 4 ], [ 7, 6, 5 ] ]

2. 解题思路：
   螺旋矩阵主要是要确定横向遍历跟纵向遍历的区间问题，我们统计按照i:0 ——> length - 1，这样可以保证四边都是一样的规则，遍历第一个，不遍历最后一个。
   走完上横向行的时候，j已经到了最后一列，也就是右纵向列，以此类推。 1.column: 0 ——> n-1-圈数, nums[圈数][column] .2.row: 0 ——> n-1-圈数，nums[row][column]. 3. column: n-1-圈数 ——> 圈数, nums[row][column], 4.row: n-1-圈数 ——> 圈数，nums[row][column].
   遍历的圈数就是 n/2, 另外要考虑奇偶数问题，如果是基数就会落下最后一个数字n^2没遍历，最后加上就行了。

3. 代码实现：
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] matrix = new int[n][n];
        int circle = 0;
        // int end = 1;
        int count = 1;
        // int circle = n / 2;
        int i = 0;
        int j = 0;
        while(circle < n/2){
            // 模拟上侧从左到右
            for(j = circle; j < n - circle - 1; j++){
                matrix[circle][j] = count;
                count++;
            }
            // 模拟右侧从上到下
            for(i = circle; i < n - circle - 1; i++){
                matrix[i][j] = count;
                count++;
            }
            // 模拟下侧从右到左
            for(j = j; j > circle; j--){
                matrix[i][j] = count;
                count++;
            }
            // 模拟左侧从下到上
            for(i = i; i > circle; i--){
                matrix[i][j] = count;
                count++;
            }
            circle++;
        }
        if(n % 2 == 1){
            matrix[circle][circle] = n*n;
        }
        return matrix;
    }
}
