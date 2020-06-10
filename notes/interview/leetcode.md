# 常见题
## leetcode_33:搜索旋转排序数组
假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 O(log n) 级别。

示例 1:
```
输入: nums = [4,5,6,7,0,1,2], target = 0
```
输出: 4
示例 2:
```
输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1
```

### 思路
题目要求 O(logN)O(logN) 的时间复杂度，基本可以断定本题是需要使用二分查找，怎么分是关键。  
由于题目说数字了无重复，举个例子：  
1 2 3 4 5 6 7 可以大致分为两类，  
第一类 2 3 4 5 6 7 1 这种，也就是 nums[start] <= nums[mid]。此例子中就是 2 <= 5。  
这种情况下，前半部分有序。因此如果 nums[start] <=target<nums[mid]，则在前半部分找，否则去后半部分找。  
第二类 6 7 1 2 3 4 5 这种，也就是 nums[start] > nums[mid]。此例子中就是 6 > 2。  
这种情况下，后半部分有序。因此如果 nums[mid] <target<=nums[end]，则在后半部分找，否则去前半部分找。  


### 解决方案
```java
public class Solution {
    public int search(int[] nums, int target){
        if(nums.length == 0 | nums == null){
            return -1;
        }
        int start = 0;
        int end = nums.length - 1;
        int mid;

        while(start <= end){
            mid = (start + end)/2;
            if(nums[mid] == target){
                return mid;
            }
            // 前半部分一定是有序的
            if(nums[start] <= nums[mid]){
                if(target >= nums[start] && target < nums[mid]){
                    end = mid -1; // mid已经比较过了，所以end从mid-1开始
                }else {
                    start = mid + 1;// 如果nums[mid]=target，程序就结束了，所以这里从mid+1开始
                }
            }else { // nums[start] > nums[mid]
                // 后半部分一定是有序的
                if(target > nums[mid] && target<= nums[end]){// 此处不是target>=nums[mid]是因为nums[mid] == target最开始已经判断过了
                    start = mid + 1;
                }else {
                    end = mid -1;
                }
            }
        }

        return -1;
    }
}
```

