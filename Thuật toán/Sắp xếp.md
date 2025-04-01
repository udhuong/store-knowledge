https://codepumpkin.com/selection-sort-algorithms/

## 1. **Bubble Sort (Sắp xếp nổi bọt)**

* **Nguyên lý** : Lặp đi lặp lại việc hoán đổi các phần tử liền kề nếu chúng không đúng thứ tự.
* **Độ phức tạp** :
  * **Trung bình & Tệ nhất** : O(n2)O(n^2)**O**(**n**2**)**
  * **Tốt nhất (danh sách đã sắp xếp)** : O(n)O(n)**O**(**n**)
* **Ưu điểm** : Dễ hiểu, dễ triển khai.
* **Nhược điểm** : Chậm, không hiệu quả cho dữ liệu lớn.

VD:

```less
Lượt 1: [5, 3, 8, 4, 2] → [3, 5, 8, 4, 2] → [3, 5, 8, 4, 2] → [3, 5, 4, 8, 2] → [3, 5, 4, 2, 8]
Lượt 2: [3, 5, 4, 2, 8] → [3, 5, 4, 2, 8] → [3, 4, 5, 2, 8] → [3, 4, 2, 5, 8]
Lượt 3: [3, 4, 2, 5, 8] → [3, 4, 2, 5, 8] → [3, 2, 4, 5, 8]
Lượt 4: [3, 2, 4, 5, 8] → [2, 3, 4, 5, 8] 
```

**Kết quả cuối cùng:** [2, 3, 4, 5, 8]

---

## 2. **Selection Sort (Sắp xếp chọn)**

* **Nguyên lý** : Tìm phần tử nhỏ nhất và hoán đổi nó với phần tử đầu tiên, lặp lại cho phần còn lại.
* **Độ phức tạp** : O(n2)O(n^2)**O**(**n**2**)** trong mọi trường hợp.
* **Ưu điểm** : Ít hoán đổi hơn Bubble Sort.
* **Nhược điểm** : Vẫn chậm và không hiệu quả với dữ liệu lớn.

VD:

```less
Bước 1: [5, 3, 8, 4, 2] → Chọn 2 nhỏ nhất → Đổi chỗ với 5 → [2, 3, 8, 4, 5]
Bước 2: [2, 3, 8, 4, 5] → Chọn 3 nhỏ nhất → Không đổi
Bước 3: [2, 3, 8, 4, 5] → Chọn 4 nhỏ nhất → Đổi chỗ với 8 → [2, 3, 4, 8, 5]
Bước 4: [2, 3, 4, 8, 5] → Chọn 5 nhỏ nhất → Đổi chỗ với 8 → [2, 3, 4, 5, 8]
```

---

## 3. **Insertion Sort (Sắp xếp chèn)**

* **Nguyên lý** : Chèn từng phần tử vào vị trí đúng trong danh sách đã sắp xếp.
* **Độ phức tạp** :
  * **Tệ nhất** : O(n2)O(n^2)**O**(**n**2**)**
  * **Tốt nhất (danh sách đã sắp xếp sẵn)** : O(n)O(n)**O**(**n**)
* **Ưu điểm** : Hiệu quả với danh sách gần như đã sắp xếp.
* **Nhược điểm** : Vẫn chậm với dữ liệu lớn.

VD:

```less
Bước 1: [5, | 3, 8, 4, 2] → Chèn 3 vào đúng vị trí → [3, 5, | 8, 4, 2]
Bước 2: [3, 5, | 8, 4, 2] → Không cần chèn
Bước 3: [3, 5, 8, | 4, 2] → Chèn 4 vào đúng vị trí → [3, 4, 5, 8, | 2]
Bước 4: [3, 4, 5, 8, | 2] → Chèn 2 vào đúng vị trí → [2, 3, 4, 5, 8]
```

---

## 4. **Merge Sort (Sắp xếp trộn)**

* **Nguyên lý** : Chia danh sách thành các phần nhỏ hơn, sắp xếp từng phần, rồi gộp lại.
* **Độ phức tạp** : O(nlog⁡n)O(n \log n)**O**(**n**log**n**) trong mọi trường hợp.
* **Ưu điểm** : Ổn định, hiệu quả với dữ liệu lớn.
* **Nhược điểm** : Cần bộ nhớ phụ để lưu trữ danh sách đã chia.

```less
Chia: [5, 3, 8, 4, 2] → [5, 3] và [8, 4, 2] 
Chia tiếp: [5, 3] → [5], [3]  | [8, 4, 2] → [8], [4, 2] → [4], [2]

Gộp: [5], [3] → [3, 5]  
Gộp: [4], [2] → [2, 4]  
Gộp: [8], [2, 4] → [2, 4, 8]  
Gộp cuối: [3, 5], [2, 4, 8] → [2, 3, 4, 5, 8]  
```

---

## 5. **Quick Sort (Sắp xếp nhanh)**

* **Nguyên lý** : Chọn một phần tử làm "chốt" (pivot), chia danh sách thành hai phần nhỏ hơn và lớn hơn pivot, sau đó đệ quy sắp xếp từng phần.
* **Độ phức tạp** :
  * **Trung bình & Tốt nhất** : O(nlog⁡n)O(n \log n)**O**(**n**log**n**)
  * **Tệ nhất (khi chọn pivot không tốt, danh sách đã sắp xếp sẵn)** : O(n2)O(n^2)**O**(**n**2**)**
* **Ưu điểm** : Rất nhanh, hiệu quả trên hầu hết các bộ dữ liệu.
* **Nhược điểm** : Không ổn định, có thể tốn thêm bộ nhớ với cách triển khai đệ quy.

VD:

```less
Chọn pivot = 4, chia mảng: [3, 2] (nhỏ hơn pivot) và [5, 8] (lớn hơn pivot)  
Sắp xếp [3, 2] → [2, 3]  
Sắp xếp [5, 8] → [5, 8]  
Kết hợp lại: [2, 3, 4, 5, 8]
```

---

## 6. **Heap Sort (Sắp xếp bằng Heap)**

* **Nguyên lý** : Xây dựng một **Heap** (cây nhị phân) từ danh sách và trích xuất phần tử lớn nhất dần dần.
* **Độ phức tạp** : O(nlog⁡n)O(n \log n)**O**(**n**log**n**) trong mọi trường hợp.
* **Ưu điểm** : Không cần bộ nhớ bổ sung nhiều.
* **Nhược điểm** : Không ổn định, chậm hơn Quick Sort.

---

## 7. **Counting Sort (Sắp xếp đếm)**

* **Nguyên lý** : Đếm số lần xuất hiện của từng giá trị và dùng thông tin đó để sắp xếp.
* **Độ phức tạp** : O(n+k)O(n + k)**O**(**n**+**k**) với kk**k** là phạm vi giá trị trong danh sách.
* **Ưu điểm** : Cực nhanh với tập dữ liệu có giá trị nhỏ.
* **Nhược điểm** : Không dùng được cho dữ liệu có giá trị quá lớn hoặc dạng số thực.

---

## 8. **Radix Sort (Sắp xếp theo cơ số)**

* **Nguyên lý** : Sắp xếp từng chữ số của số theo thứ tự từ thấp đến cao.
* **Độ phức tạp** : O(nk)O(nk)**O**(**nk**) với kk**k** là số chữ số lớn nhất.
* **Ưu điểm** : Hiệu quả với dữ liệu số nguyên có phạm vi hẹp.
* **Nhược điểm** : Không phù hợp với dữ liệu phức tạp (chuỗi, số thực).

---

## 9. **Bucket Sort (Sắp xếp theo nhóm)**

* **Nguyên lý** : Chia các phần tử vào các "xô" (bucket), sau đó sắp xếp từng bucket và gộp lại.
* **Độ phức tạp** : O(n+k)O(n + k)**O**(**n**+**k**) trong điều kiện lý tưởng.
* **Ưu điểm** : Hiệu quả với dữ liệu phân bố đều.
* **Nhược điểm** : Không hiệu quả nếu dữ liệu phân bố không đồng đều.

## **So sánh các thuật toán**

| Thuật toán   | Độ phức tạp trung bình                                 | Độ phức tạp tệ nhất                                   | Ổn định | Dùng tốt khi nào?                 |
| -------------- | ----------------------------------------------------------- | ----------------------------------------------------------- | ---------- | ------------------------------------ |
| Bubble Sort    | O(n2)O(n^2)**O**(**n**2**)**                    | O(n2)O(n^2)**O**(**n**2**)**                    | ✔         | Dữ liệu rất nhỏ                  |
| Selection Sort | O(n2)O(n^2)**O**(**n**2**)**                    | O(n2)O(n^2)**O**(**n**2**)**                    | ✘         | Khi cần ít hoán đổi             |
| Insertion Sort | O(n2)O(n^2)**O**(**n**2**)**                    | O(n2)O(n^2)**O**(**n**2**)**                    | ✔         | Dữ liệu gần sắp xếp             |
| Merge Sort     | O(nlog⁡n)O(n \log n)**O**(**n**log**n**) | O(nlog⁡n)O(n \log n)**O**(**n**log**n**) | ✔         | Khi cần ổn định                  |
| Quick Sort     | O(nlog⁡n)O(n \log n)**O**(**n**log**n**) | O(n2)O(n^2)**O**(**n**2**)**                    | ✘         | Khi cần tốc độ cao               |
| Heap Sort      | O(nlog⁡n)O(n \log n)**O**(**n**log**n**) | O(nlog⁡n)O(n \log n)**O**(**n**log**n**) | ✘         | Khi cần bộ nhớ ít                |
| Counting Sort  | O(n+k)O(n + k)**O**(**n**+**k**)          | O(n+k)O(n + k)**O**(**n**+**k**)          | ✔         | Khi phạm vi giá trị nhỏ          |
| Radix Sort     | O(nk)O(nk)**O**(**nk**)                         | O(nk)O(nk)**O**(**nk**)                         | ✔         | Khi làm việc với số nguyên lớn |
| Bucket Sort    | O(n+k)O(n + k)**O**(**n**+**k**)          | O(n2)O(n^2)**O**(**n**2**)**                    | ✔         | Khi dữ liệu phân bố đều        |



## Kết hợp thuật toán

### 1. **Hybrid Sort (Kết hợp Quick Sort và Insertion Sort)**

* **Ý tưởng** : Quick Sort hoạt động hiệu quả với mảng lớn, nhưng khi mảng nhỏ (thường < 10-20 phần tử), Insertion Sort có thể nhanh hơn do tối ưu hoá bộ nhớ cache.
* **Cách làm** :
  * Dùng Quick Sort để chia mảng thành các phần nhỏ.
  * Khi kích thước mảng con nhỏ hơn một ngưỡng nhất định (ví dụ 10 phần tử), thay vì tiếp tục dùng Quick Sort, ta sử dụng Insertion Sort để sắp xếp phần nhỏ đó.
* **Ví dụ** : **Timsort** (sử dụng trong Python, Java) kết hợp giữa Merge Sort và Insertion Sort.

---

### 2. **Timsort (Kết hợp Merge Sort và Insertion Sort)**

* **Được dùng trong** : Python (`sorted()`), Java (`Arrays.sort()`).
* **Ý tưởng** :
  * Chia mảng thành nhiều phần nhỏ (run) và dùng **Insertion Sort** cho từng phần nhỏ.
  * Sau đó, dùng **Merge Sort** để gộp các phần đã sắp xếp lại.
* **Lợi ích** :
  * **Nhanh với dữ liệu có trật tự** (vì tận dụng được Insertion Sort).
  * **Ổn định** và hiệu quả với dữ liệu thực tế.

---

### 3. **Introsort (Kết hợp Quick Sort, Heap Sort và Insertion Sort)**

* **Được dùng trong** : `std::sort()` của C++
* **Ý tưởng** :
  * Ban đầu sử dụng **Quick Sort** để sắp xếp.
  * Nếu đệ quy quá sâu (có nguy cơ rơi vào trường hợp xấu nhất của Quick Sort O(n2)O(n^2)**O**(**n**2**)**), chuyển sang **Heap Sort** để đảm bảo O(nlog⁡n)O(n \log n)**O**(**n**log**n**).
* Khi danh sách nhỏ, dùng **Insertion Sort** để tối ưu tốc độ.
* **Lợi ích** :
  * Không rơi vào trường hợp tệ nhất của Quick Sort.
  * Tận dụng ưu điểm của nhiều thuật toán khác nhau.

---

### 4. **Dual-Pivot Quick Sort (Quick Sort 2 pivot)**

* **Được dùng trong** : Java 7+ (`Arrays.sort()` cho mảng nguyên thủy).
* **Ý tưởng** :
  * Thay vì chọn 1 pivot, ta chọn **2 pivot** để chia mảng thành 3 phần: nhỏ hơn pivot 1, nằm giữa pivot 1 & 2, và lớn hơn pivot 2.
* **Lợi ích** :
  * Ít hoán đổi hơn Quick Sort truyền thống.
  * Thực tế nhanh hơn Quick Sort thông thường.

---

### 5. **Bucket Sort + Quick Sort hoặc Insertion Sort**

* **Ý tưởng** :
  * Dùng **Bucket Sort** để chia dữ liệu vào nhiều nhóm nhỏ.
  * Nếu số lượng phần tử trong mỗi nhóm nhỏ, dùng **Insertion Sort** để sắp xếp.
  * Nếu nhóm có quá nhiều phần tử, dùng **Quick Sort** hoặc  **Merge Sort** .
* **Lợi ích** :
  * Hiệu quả với dữ liệu có phân bố đều.
  * Giảm độ phức tạp nếu các bucket nhỏ.
