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

代码实现：
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

