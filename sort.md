## 一、排序

### 1.1 冒泡排序

#### 1

```java
public static void sort(int[] arr) {
    recursion(arr, arr.length);
}

public static void recursion(int[] arr, int len) {
    if (len == 1) {
        return;
    }
    int x = 0;
    for (int i = 0; i < len - 1; i++) {
        if (arr[i] > arr[i + 1]) {
            swap(arr, i, i + 1);
            x = i;
        }
    }
    recursion(arr, x+1);
}
```

### 1.2 选择排序

### 1.3 插入排序

#### 1

```java
 public static void sort(int[] a) {
        insertion(a, 1);
    }

 private static void insertion(int[] a, int low) {
     if (low == a.length) {
         return;
     }
     int temp = a[low];
     int i = low;
     for (i = low; i > 0 && a[i - 1] > temp; i--) {
         a[i] = a[i - 1];
     }
     a[i] = temp;

     insertion(a, ++low);
 }
```

