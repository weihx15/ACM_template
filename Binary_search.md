# 代码实现

## 基本的二分查找
### 查找区间为\[left, right]的写法

```C++
int search(vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size() - 1; // 定义target在左闭右闭的区间里[left, right]
    while (left <= right) { // 当left==right，区间[left, right]依然有效，所以用 <=
        int mid = left + ((right - left) / 2);// 防止溢出 等同于(left + right) / 2
        if (nums[mid] > target) {
            right = mid - 1; // target 在左区间，所以[left, mid - 1]
        } else if (nums[mid] < target) {
            left = mid + 1; // target 在右区间，所以[mid + 1, right]
        } else { // nums[mid] == target
            return mid; // 数组中找到目标值，直接返回下标
        }
    }
    // 未找到目标值
    return -1;
}
```

### 查找区间为\[left, right)的写法

```C++
int search(vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size(); // 定义target在左闭右开的区间里，即：[left, right)
    while (left < right) { // 因为left == right的时候，在[left, right)是无效的空间，所以使用 <
        int mid = left + ((right - left) / 2);
        if (nums[mid] > target) {
            right = mid; // target 在左区间，在[left, mid)中
        } else if (nums[mid] < target) {
            left = mid + 1; // target 在右区间，在[mid + 1, right)中
        } else { // nums[mid] == target
            return mid; // 数组中找到目标值，直接返回下标
        }
    }
    // 未找到目标值
    return -1;
}
```

## 寻找一个数的左右边界

### 查找左侧边界

```C++
int lower_bound(vector<int> &nums, int k)
{
    int left = 0, right = nums.size() - 1;
    while (left < right)
    {
        int mid = left + ((right - left) >> 1);
        if (nums[mid] >= k)
            right = mid;
        else
            left = mid + 1;
    }
    if (nums[left] < k)
        left++;
    return left;
}
```

### 查找右侧边界

```C++
int upper_bound(vector<int> &nums, int k)
{
    int left = 0, right = nums.size() - 1;
    while (left < right)
    {
        int mid = left + ((right - left) >> 1);
        if (nums[mid] > k)
            right = mid;
        else
            left = mid + 1;
    }
    if (nums[left] <= k)
        left++;
    return left;
}
```

## \*寻找重复值的左右边界

寻找一个数在单调不递减序列中第一次出现的位置（左侧边界）和最后一次出现的位置（右侧边界），如果不存在该数则返回 `-1`

### 寻找左侧边界

#### 查找区间为\[left, right]的写法

```C++
int left_bound(vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size() - 1;
    // 搜索区间为[left, right]
    while (left <= right) {
        int mid = left + ((right - left) / 2);
        if (nums[mid] < target) {
            // 搜索区间变为[mid + 1, right]
            left = mid + 1;
        } else if (nums[mid] > target) {
            // 搜索区间变为[left, mid - 1]
            right = mid - 1;
        } else {
            // 收缩右侧边界
            // 如果存在target，那right最后必定是target左边界的位置减1
            right = mid - 1;
        }
    }
    // 检查出界情况
    // left == right + 1跳出循环
    if (left >= nums.size())
        return -1;
    return nums[left] == target ? left : -1;
}
```

#### 查找区间为\[left, right)的写法

```C++
int left_bound(vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size(); // 注意
    // 搜索区间为[left, right)
    while (left < right) { // 注意
        int mid = ((left + right) / 2);
        if (nums[mid] == target) {
            right = mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid; // 注意
        }
    }
    // target比所有数都大
    if (left == nums.size())
        return -1;
    // 类似之前算法的处理方式
    return nums[left] == target ? left : -1;
}
```

### 寻找右侧边界

#### 查找区间为\[left, right]的写法

```C++
int right_bound(vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size() - 1;
    
    while (left <= right) {
        int mid = left + ((right - left) / 2);
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            // 如果存在target，那right最后就是右边界
            // 如果不存在target，right是第一个小于target的值
            right = mid - 1;  
        } else {
            // 这里改成收缩左侧边界即可
            left = mid + 1;
        }
    }
    if (left <= 0)
        return -1;
    return nums[left - 1] == target ? left - 1 : -1;
}
```

#### 查找区间为\[left, right)的写法

```C++
int right_bound(vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size();

    while (left < right) {
        int mid = left + ((right - left) / 2);
        if (nums[mid] == target) {
            left = mid + 1; // 注意
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            // 如果存在target, 那right最后是target右边界加1的位置
            right = mid;
        }
    }
    
    if (left == 0)
        return -1;
    return nums[left - 1] == target ? left - 1 : -1;
}
```


# STL 使用

## binary_search()

```C++
std::binary_search(begin, end, value);
```

- binary_search()函数用于查找指定的元素，找到返回**true**，否则返回**false**。

## lower_bound()

```C++
std::lower_bound(begin, end, value);
```

- lower_bound()函数返回数组中**大于等于value的第一个元素的地址**，若数组中的元素均小于value则返回**尾后地址**。
## upper_bound()

```C++
std::upper_bound(begin, end, value);
```

- upper_bound()函数返回数组中**大于value的第一个元素的地址**，若数组中的元素均小于等于value则返回**尾后地址**。

## 使用 greater\<type\>() 重载

```C++
std::upper_bound(begin, end, value, greater<int>())
```

- 在**单调不递增**的数组中，在数组的 **\[begin, end)** 区间中二分查找**小于value的第一个元素的地址**，若数组中的元素均大于等于value则返回**尾后地址**。

```C++
std::lower_bound(begin, end, value, greater<int>())
```

- 在**单调不递增**的数组中，在数组的 **\[begin, end)** 区间中二分查找**小于等于value的第一个元素的地址**，若数组中的元素均大于value则返回**尾后地址**。
