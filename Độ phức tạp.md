## Độ phức tạp của thuật toán là gì?

Độ phức tạp của thuật toán là một cách đo lường hiệu suất của thuật toán khi kích thước đầu vào tăng lên. Nó giúp ta đánh giá thuật toán nào nhanh hơn hoặc sử dụng ít tài nguyên hơn.

Có  **hai loại độ phức tạp chính** :

1. **Độ phức tạp thời gian (Time Complexity)** – Đo lường số bước tính toán cần thực hiện.
2. **Độ phức tạp không gian (Space Complexity)** – Đo lường lượng bộ nhớ cần sử dụng.

## Độ phức tạp thời gian (Time Complexity)

Là số phép toán cần thực hiện theo kích thước đầu vào nn**n**.

Một số ký hiệu phổ biến:

* O(1)O(1)**O**(**1**) – **Thời gian hằng số** (không phụ thuộc vào nn**n**)
* O(log⁡n)O(\log n)**O**(**lo**g**n**) – **Thời gian logarit**
* O(n)O(n)**O**(**n**) – **Thời gian tuyến tính**
* O(nlog⁡n)O(n \log n)**O**(**n**log**n**) – **Thời gian log tuyến tính**
* O(n2)O(n^2)**O**(**n**2**)** – **Thời gian bậc hai**
* O(2n)O(2^n)**O**(**2**n**)** – **Thời gian lũy thừa**
* O(n!)O(n!)**O**(**n**!) – **Thời gian giai thừa**

## Độ phức tạp không gian (Space Complexity)

Là lượng bộ nhớ (RAM) mà thuật toán cần sử dụng.

* O(1)O(1)**O**(**1**) – Sử dụng bộ nhớ cố định.
* O(n)O(n)**O**(**n**) – Cần bộ nhớ tỷ lệ với đầu vào.
* O(n2)O(n^2)**O**(**n**2**)** – Cần bộ nhớ gấp đôi khi kích thước tăng.

## Ví dụ tính độ phức tạp

### Tính tổng mảng – O(n) (Tuyến tính)

```java
public class SumArray {
    public static int sum(int[] arr) {
        int total = 0;
        for (int num : arr) {  // O(n)
            total += num;  // O(1)
        }
        return total;
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5};
        System.out.println("Tổng mảng: " + sum(arr));
    }
}
```

Vòng lặp chạy **n** lần → O(n)

### Tìm phần tử trùng nhau –  O(n^2) (Bậc hai)

```java
public class DuplicateFinder {
    public static boolean hasDuplicate(int[] arr) {
        for (int i = 0; i < arr.length; i++) {  // O(n)
            for (int j = i + 1; j < arr.length; j++) {  // O(n)
                if (arr[i] == arr[j]) {
                    return true;
                }
            }
        }
        return false;
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 2};
        System.out.println("Có phần tử trùng không? " + hasDuplicate(arr));
    }
}
```

Hai vòng lặp lồng nhau → O(n x n) = O(n^2)

### Tìm kiếm nhị phân – O(log ⁡n) (Logarithmic)

```java
import java.util.Arrays;

public class BinarySearch {
    public static int binarySearch(int[] arr, int target) {
        int left = 0, right = arr.length - 1;
        while (left <= right) {  // O(log n)
            int mid = left + (right - left) / 2;
            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5, 6, 7, 8, 9};
        int target = 6;
        System.out.println("Phần tử ở vị trí: " + binarySearch(arr, target));
    }
}

```

Mỗi lần  **cắt mảng làm đôi** , chạy tối đa log 2n -> O (log n)

### Sắp xếp nhanh (Quick Sort) – O(n log ⁡n)

```java
import java.util.Arrays;

public class QuickSort {
    public static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            int pivotIndex = partition(arr, low, high);
            quickSort(arr, low, pivotIndex - 1);
            quickSort(arr, pivotIndex + 1, high);
        }
    }

    private static int partition(int[] arr, int low, int high) {
        int pivot = arr[high];
        int i = low - 1;
        for (int j = low; j < high; j++) {
            if (arr[j] < pivot) {
                i++;
                swap(arr, i, j);
            }
        }
        swap(arr, i + 1, high);
        return i + 1;
    }

    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    public static void main(String[] args) {
        int[] arr = {5, 3, 8, 4, 2};
        quickSort(arr, 0, arr.length - 1);
        System.out.println("Mảng sau sắp xếp: " + Arrays.toString(arr));
    }
}

```

* **Chia đôi mảng** mỗi lần O(log ⁡n)
* **Sắp xếp từng phần** mất O(n)
* Tổng hợp lại:  **O(nlog⁡n)**
