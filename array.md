## 四、数组

### 4.1 合并有序数组

#### 1

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int len = m + n - 1;
        for (int i = len; i >= 0; i--) {
            if (m == 0) {
                for (int j = 0; j < n; j++) {
                    nums1[j] = nums2[j];
                }
                break;
            }
            if (n == 0) {
                break;
            }
            nums1[i] =
                nums1[m - 1] < nums2[n - 1] ? nums2[n-- - 1] : nums1[m-- - 1];
        }
    }
};
```

反向输入

### 4.2 找到数组中第k大的元素

[215. 数组中的第K个最大元素 - 力扣（LeetCode）](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

#### 1

```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        int n = nums.size();
        return quickSelect(nums, 0, n - 1, n - k);
    }

    int quickSelect(vector<int> nums, int left, int right, int k) {

        int mid = partition(nums, left, right);
        if (mid == k) {
            return nums[mid];
        }
        if (mid < k) { // 到右边找
            return quickSelect(nums, mid + 1, right, k);
        } else { // 到左边找
            return quickSelect(nums, left, mid, k);
        }
    }

    int partition(vector<int>& nums, int left, int right) {
        int base = nums[left];
        int i = left + 1;
        int j = right;
        while (i <= j) {
            while (i <= j && nums[i] < base) {
                i++;
            }
            while (i <= j && nums[j] > base) {
                j--;
            }
            if (i <= j) {
                swap(nums, i, j);
                i++;
                j--;
            }
        }
        swap(nums, left, j);
        return j;
    }

    void swap(vector<int>& nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
};
```

