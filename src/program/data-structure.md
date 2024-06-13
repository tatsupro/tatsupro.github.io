# Cấu trúc dữ liệu

> A data structure is a particular way storing and organizing data in a computer for efficient access and modification. Data structures are designed for a specific purpose. Examples include arrays, linked lists, and classes.

Cấu trúc dữ liệu chỉ cách máy tính lưu trữ và tổ chức dữ liệu. Các cấu trúc dữ liệu được thiết kế cho một mục đích cụ thể. Các ví dụ bao gồm mảng, danh sách liên kết và lớp (class).

### Array

![](/assets/data-structure-array.png)

Mảng là cấu trúc dữ liệu chứa một loạt những phần tử có cùng kiểu ở cạnh nhau.

- Khi lấy một phần tử bất kì trong mảng thì độ phức tạp chỉ là O(1).
- Nếu mảng chưa đầy thì thêm một phần tử cũng chỉ là O(1).
- Mảng phải khai báo số lượng phần tử cho trước và chỉ được fix cứng như vậy. Ví dụ cho mảng có độ dài 5 phần tử thì chỉ có thể lưu tối đa 5 phần tử, muốn thêm phần tử thứ 6 thì phải tạo một mảng mới và copy mảng gốc sang, độ phức tạp là O(n).
- Xóa một phần tử thì phải xóa rồi dịch các phần tử phía sau phần tử bị xóa về trước, độ phức tạp sẽ là O(n).

### Linked List

![](/assets/data-structure-1.png)

Gần giống mảng, nhưng thay vì để dữ liệu cạnh nhau trên bộ nhớ thì danh sách liên kết đơn sẽ lưu các phần tử thành các node: mỗi node chứa dữ liệu và con trỏ tới dữ liệu tiếp theo.

- Điểm hay của cấu trúc dữ liệu này, giả sử nếu muốn xóa node thứ 2 trong hình thì chỉ cần điều chỉnh lại con trỏ ở node thứ 1 và xóa node 2 là xong. Độ phức tạp là O(1).
- Nhược điểm: Khi tìm một phần tử bất kì thì phải chạy qua từng node một, độ phức tạp là O(n).

### Stack và Queue

![](/assets/data-structure-2.png)

Stack giống như một ngăn xếp, thêm dữ liệu mới vào ngăn xếp được gọi là push và bỏ dữ liệu ra khỏi ngăn xếp gọi là pop. Dữ liệu thêm và bỏ theo thứ tự FILO (First in Last Out), thằng nào vào trước thì bỏ ra sau hoặc LIFO (First in Last Out) thằng nào vào sau thì ra trước.
Queue giống như một hàng đợi, dữ liệu được thêm và bỏ theo thứ tự FIFO (First in First Out) thằng nào vào trước thì ra trước hoặc vào sau thì ra sau.

Độ phức tạp của thuật toán push, pop, queue và dequeue đều là O(1).

### Hash Table

![](/assets/data-structure-3.png)

- Độ phức tạp khi thêm phần tử mới: O(1).
- Độ phức tạp khi tìm kiếm phần tử (trong trường hợp bình thường): O(1).

Hash Table lưu trữ dữ liệu dưới dạng Key-Value để khi cần tìm dữ liệu ta sẽ gọi nó ra theo Key của dữ liệu đó. Ví dụ: Key "Lisa" sẽ cho ra số điện thoại của Lisa như hình trên.

- CTDL này có 1 thuật toán hash để khi thêm một key mới thì nó sẽ hash thành một mã lưu ở bucket trong bộ nhớ (mỗi bucket đều chứa Linked List). Giả như: Key "Lisa" được cho mã hash là 001 như hình trên.
- Đôi khi thuật toán hash sẽ gặp phải trường hợp Hash Collision, đó là khi 2 key được cho cùng một mã hash. Như "Tom" và "Mark" đều có mã hash là 011, thì giờ đây nó sẽ có thêm đường dẫn số của Mark với Tom. Khi gặp trường hợp này, độ phức tạp sẽ tăng lên O(n).

### Set

![](/assets/data-structure-4.png)

Set là một tập hợp dữ liệu không trùng nhau, under the hood thì Set có thể sử dụng Hash (Set) hoặc Tree (Set) để biểu diễn dữ liệu.

### Graph

![](/assets/data-structure-5.png)

Đồ thị là tập hợp các node nối với nhau bởi các cạnh, đồ thị có thể có hướng hoặc không hướng. Trên thực tế thì thường áp dụng vào làm bản đồ.

### Tree

![](/assets/data-structure-6.png)

Đồ thị dạng cây chỉ có một node cha và rất nhiều node con, các node con có thể có nhiều node con nữa.
