# Hướng đối tượng

## 4 tính chất

### Tính đóng gói (Encapsulation)

Là cách che dấu các thuộc tính bên trong đối tượng không thể bị tác động... trừ khi thay đổi nó qua một method nào đó.

```cs
public class Rectangle {
	private int height;
	private int width;

	public Rectangle(int newHeight, int newWidth) {
		height = newHeight;
		width = newWidth;
	}
}
```

Nếu có một đối tượng được tạo từ đoạn code trên, thì không thể gọi `height = 12` ra trực tiếp để sửa được. Phải gán giá trị cho height qua method `Rectangle()`.

### Tính kế thừa (Inheritance)

Là cách kế thừa các thuộc tính và method mà không cần phải khai báo lại chúng. Hoặc nếu cần khai báo lại thì có thể tái định nghĩa lại (override).

```cs
class Vehicle {
	public string brand = "Ford";

	public void honk() {
		Console.WriteLine("Tuut, tuut!");
	}
}

class Car: Vehicle {
	public string modelName = "Mustang";
}
```

### Tính đa hình (Polymorphism)

Tính đa hình cho phép một hành động có thể được thực hiện bằng nhiều cách khác nhau.

Có 2 cách vận dụng tính đa hình:

- Overload (đa hình khi biên dịch - compile time): Trong 1 class có nhiều method cùng tên nhưng khác params.

```cs
public class BankAccount() {
	public object GetAccountInformation(int bankId) {}
	public object GetAccountInformation(string bankName) {}
    public object GetAccountInformation(string bankName, string address){}
}
```

- Override (đa hình khi thực thi - runtime): Các method kế thừa của class con từ class cha tùy vào từng class sẽ thực hiện như thế nào. Chỉ trong lúc runtime ta mới biết nó sử dụng method nào.

```cs
public class BankAccount {
	public decimal GetTotalPrice() {}
}

public class CreditCard: BankAccount {
    public decimal GetTotalPrice() {}
}

public class DebitCard: BankAccount {
    public decimal GetTotalPrice() {}
}
```

### Tính trừu tượng (Abstraction)

> Tính trừu tượng cho phép tổng quát hóa một đối tượng.

Nghĩa là ẩn đi những thông tin chi tiết bên trong, chỉ thể hiện ra những thông tin bên ngoài. Qua thông tin bên ngoài đó ta có thể hiểu được đối tượng đó làm gì.

**Ẩn chi tiết, thể hiện tổng quan.**

Tính chất này được thể hiện qua việc sử dụng **interface** hoặc **abstract class**.

```cs
public Interface Checkout {
	Bool ValidateAccount(object BankAccount);
	Decimal CaculateTotalPrice(object BankAccount);
	int Checkout(object BankAccount));
}
```
