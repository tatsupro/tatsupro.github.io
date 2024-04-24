# Giải thuật

> ⚠️ Trang còn sơ sài, cần cập nhật

Algorithms are the ideas behind computer programs to _**solve a general, specified problem**_.
An algorithmic problem is specified by _**describing the set of instances**_ it must work on, and what desired properties _**the output must have**_.

## Big O Notation

Có thể hiểu là sự tương quan giữa thời lượng của thuật toán và độ lớn của dữ liệu. Thông thường người ta hay sử dụng biểu đồ như thế này (Big O Notation) để biểu thị độ phức tạp của thuật toán:

![](/assets/algo-1.png)

Ví dụ:

- **O(1)**: Dù dữ liệu có nhiều lên tới mức nào thì thời gian chạy vẫn không tăng.
- **O(n)**: Dữ liệu càng nhiều thì tốc độ càng chậm.

_Có nhiều loại notations khác ngoài Big-O (Big Theta Notation, Big Omega Notation), đề cập tới để biết chứ không cần tìm hiểu._

### Common Runtimes

...

## Thuật toán sắp xếp

### Selection Sort

**Yêu cầu**

Sắp xếp một danh sách các số nguyên bằng Selection Sort. Đây là một trong những thuật toán sắp xếp cơ bản.

Độ phức tạp: O(n^2), không phù hợp để sắp xếp mảng lớn.

**Test Case**

1. Mảng đã sắp xếp
2. Mảng ngược thứ tự
3. Mảng có phần tử trùng nhau
4. Mảng rỗng

```
Input: []
Output: []
```

5. Mảng chỉ có 1 phần tử

```
Input: [42]
Output: [42]
```

6. Mảng chứa các số âm

**Cách giải quyết**

Bắt đầu từ phần tử đầu tiên của mảng, duyệt tìm phần tử nhỏ nhất và đổi chỗ cho phần tử đầu. Tiếp tục từ phần tử tiếp theo (bỏ qua phần tử đã đầu tiên vừa đổi chỗ), tìm phần tử nhỏ nhất trong mảng còn lại và tiếp tục đổi chỗ cho tới khi mảng được sắp xếp.

**Code**

```python
def selection_sort(arr):
    n = len(arr)

    for i in range(n):
        min_idx = i

        for j in range(i+1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        # swap
        arr[i], arr[min_idx] = arr[min_idx], arr[i]

    return arr
```

### Merge Sort

![](/assets/algo-2.gif)

### Quick Sort

![](/assets/algo-3.gif)

## Thuật toán tìm kiếm

### Tìm kiếm nhị phân

![](/assets/algo-4.gif)

**Yêu cầu**

Tìm 1 số trong một danh sách đã sắp xếp.

**Test case**

1.  Danh sách rỗng

```
Input: []
Output: -1
```

2. Không có trong mảng

```
Input: [2, 4, 6, 8, 10]; Target: 5
Output: -1
```

**Cách giải quyết**

Thuật toán này sử dụng kĩ thuật chia để trị.

- Nếu giá trị ở giữa = giá trị cần tìm, thuật toán sẽ trả về chỉ số của phần tử này.
- Nếu giá trị ở giữa > giá trị cần tìm, thuật toán sẽ tìm kiếm nửa bên trái.
- Nếu giá trị ở giữa < giá trị cần tìm, thuật toán sẽ tìm kiếm nửa bên phải.

Độ phức tạp của thuật toán: O(log n).
Các trường hợp không nên áp dụng thuật toán:

- Mảng chưa được sắp xếp
- Mảng quá ngắn
- Có các phần tử trùng lặp

**Code**

```python
def binary_search(arr, x):
    left = 0
    right = len(arr) - 1

    while left <= right:
        mid = (left + right) // 2

        if arr[mid] == x:
            return mid

        elif arr[mid] < x:
            left = mid + 1

        else:
            right = mid - 1

    return -1
```

### Tìm số bị xóa khỏi mảng

**Yêu cầu**
Cho 1 mảng các số nguyên khác nhau, lấy 1 phần tử bất kì trong mảng. Tìm phần tử bị lấy ra đó.

**Định hướng**
Cho 1 mảng các số nguyên, sau khi lấy ra 1 phần tử bất kì sẽ tạo ra một mảng hoàn toàn mới.

**Test case**

1. Trường hợp mảng rỗng hoặc có 1 phần tử duy nhất

```
Input: []
Output: None
```

2. Mảng có 2 phần tử trở lên

```
Input: [1, 2, 3, 4, 5]; Target: 3
Output: 3
```

**Cách giải quyết**
Tính tổng mảng cũ và mảng mới, lấy tổng mảng cũ trừ đi mảng mới là ra được số bị lấy ra.

```python
class Solution(object):
	def twoSum(self, nums, target):
            hashmap = {}
            for i in range(len(nums)):
                complement = target - nums[i]
                if complement in hashmap:
                    return [i, hashmap[complement]]
                    hashmap[nums[i]] = i|
```

### Tìm số lớn thứ 2

**Yêu cầu**

Tìm số lớn thứ 2 trong mảng các số nguyên

**Test case**

1. Rỗng hoặc chỉ chứa 1 phần tử

```
Input: []
Output: None
```

```
Input: [5]
Output: None
```

2. Chứa 2 phần tử

```
Input: [4, 7]
Output: 4
```

3. Chứa nhiều phần tử, số cần tìm ở giữa danh sách

```
Input: [3, 7, 1, 9, 4, 5]
Output: 7
```

4. Chứa nhiều phần tử, số cần tìm ở cuối danh sách

```
Input: [1, 4, 6, 2, 8, 10]
Output: 8
```

5. Chứa nhiều phần tử, các phần tử đều bằng nhau

```
Input: [2, 2, 2, 2, 2]
Output: 2
```

6. Chứa nhiều phần tử, các phần tử đều âm

```
Input: [-3, -5, -2, -1, -4]
Output: -2
```

**Cách giải quyết**

1. Kiểm tra danh sách có ít nhất 2 phần tử
2. Tìm số lớn nhất trong danh sách
3. Xóa số lớn nhất trong mảng
4. Tìm số lớn nhất trong mảng bị xóa (chính là số lớn thứ 2 trong mảng cũ)
5. Trả về số lớn thứ 2

**Code**

```python
def find_second_largest(numbers):
    # Kiểm tra danh sách có ít nhất hai phần tử
    if len(numbers) < 2:
        return None

    # Tìm số lớn nhất trong danh sách
    max_number = float('-inf')
    for number in numbers:
        if number > max_number:
            max_number = number

    # Xóa số lớn nhất khỏi danh sách
    numbers.remove(max_number)

    # Tìm số lớn nhất thứ hai trong danh sách đã xóa
    second_max_number = float('-inf')
    for number in numbers:
        if number > second_max_number:
            second_max_number = number

    # Trả về số lớn nhất thứ hai
    return second_max_number
```

### Breadth First Search

![](/assets/algo-5.gif)

### Depth First Search

![](/assets/algo-6.gif)

## Memoization

![](/assets/algo-7.png)

## Phép toán thao tác bit (Bitwise operation)

## Đệ quy (Recursion)

![](/assets/algo-8.png)
