## 二、查找

### 2.1 二分查找

#### 1

二分查找普通版：

```java
public static int binarySearchNormal(int[] arr, int target) {
    int l = 0;
    int r = arr.length - 1;
    while (l <= r) {
        int mid = l + ((r - l) >> 1);
        if (target < arr[mid]) {
            r = mid - 1;
        } else if (target > arr[mid]) {
            l = mid + 1;
        } else {
            return mid;
        }
    }
    return -(l + 1);
}
```

#### 2

在数组中有重复元素，求数组的最左（右）边目标i元素时，要对代码的if条件进行更改

```java
public static int binarySearchLeftMost2(int[] arr, int target) {
    int l = 0;
    int r = arr.length - 1;
    int index = -1;
    while (l <= r) {
        int mid = l + ((r - l) >> 1);
        if (target < arr[mid]) {
            r = mid - 1;
        } else if (target > arr[mid]) {
            l = mid + 1;
        } else {
            index = mid;
            r = mid - 1;
        }
    }
    return index;
}
```

在找到指定元素时，将不能直接返回索引，需要先添加补充一个变量存储该元素的索引，将有边界减小为mid-1（与目标元素的在中点的左边所处的情况一致），只是多了一步存储操作

#### 3

在边界的值无法值无法直接不参与查找时，循环的边界条件要进一步更改

```java
public static int binarySearch(int[] a, int target) {
    int i = 0, j = a.length;
    while (i < j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {			// 在左边
            j = m;
        } else if (a[m] < target) {		// 在右边
            i = m + 1;
        } else {
            return m;
        }
    }
    return -1;
}
```

上述代码的有边界j所处的值无法取到，所以在二分时j不能在中点的基础上减一；

