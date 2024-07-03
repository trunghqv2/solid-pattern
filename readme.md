
# Nguyên tắc SOLID

Việc áp dụng các nguyên tắc SOLID giúp mã nguồn trở nên dễ bảo trì, dễ mở rộng và dễ hiểu hơn.

SOLID là một tập hợp năm nguyên tắc thiết kế trong lập trình hướng đối tượng, giúp tăng cường tính linh hoạt và bảo trì của mã nguồn. Các nguyên tắc SOLID bao gồm:

## 1. Single Responsibility Principle (SRP) - Nguyên tắc Trách nhiệm Đơn lẻ

**Định nghĩa:** Một lớp chỉ nên có một lý do để thay đổi, nghĩa là nó chỉ nên có một nhiệm vụ hoặc trách nhiệm duy nhất.

**Ví dụ:**
```java
class Invoice {
    private List<Item> items;
    
    // Tính tổng tiền hóa đơn
    public double calculateTotal() {
        // Tính tổng tiền của các mặt hàng
    }

    // In hóa đơn
    public void printInvoice() {
        // Logic in hóa đơn
    }

    // Lưu hóa đơn vào cơ sở dữ liệu
    public void saveToDatabase() {
        // Logic lưu vào cơ sở dữ liệu
    }
}
```

## 2. Open/Closed Principle (OCP) - Nguyên tắc Mở/Đóng

**Định nghĩa**: Các thực thể phần mềm (lớp, module, hàm, v.v.) nên được mở để mở rộng nhưng đóng để sửa đổi.

**Ví dụ:**

```java
abstract class Shape {
    public abstract double area();
}

class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}

class Rectangle extends Shape {
    private double width, height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double area() {
        return width * height;
    }
}
```
## 3. Liskov Substitution Principle (LSP) - Nguyên tắc Thay thế Liskov

**Định nghĩa**: Các đối tượng của một lớp con có thể thay thế cho các đối tượng của lớp cha mà không làm thay đổi tính đúng đắn của chương trình.

**Ví dụ:**

```java

class Bird {
    public void fly() {
        // Logic để bay
    }
}

class Ostrich extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Ostriches can't fly");
    }
}


```

Lớp Ostrich không tuân theo LSP vì nó thay đổi hành vi của phương thức fly() từ lớp Bird. Để tuân theo LSP, chúng ta có thể thiết kế lại các lớp như sau:

```java

abstract class Bird {
    // Không có phương thức fly
}

class FlyingBird extends Bird {
    public void fly() {
        // Logic để bay
    }
}

class Ostrich extends Bird {
    // Không có phương thức fly
}

```

## 4. Interface Segregation Principle (ISP) - Nguyên tắc Phân chia Interface
**Định nghĩa**: Một lớp không nên bị buộc phải triển khai các interface mà nó không sử dụng.

**Ví dụ:**

```java
interface Worker {
    void work();
    void eat();
}

class HumanWorker implements Worker {
    public void work() {
        // Logic để làm việc
    }

    public void eat() {
        // Logic để ăn
    }
}

class RobotWorker implements Worker {
    public void work() {
        // Logic để làm việc
    }

    public void eat() {
        throw new UnsupportedOperationException("Robots don't eat");
    }
}
```

Lớp RobotWorker không nên bị buộc phải triển khai phương thức eat(). Thay vào đó, chúng ta nên tách interface Worker thành hai interface riêng biệt:

```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

class HumanWorker implements Workable, Eatable {
    public void work() {
        // Logic để làm việc
    }

    public void eat() {
        // Logic để ăn
    }
}

class RobotWorker implements Workable {
    public void work() {
        // Logic để làm việc
    }
}

```
## 5. Dependency Inversion Principle (DIP) - Nguyên tắc Đảo ngược Phụ thuộc

**Định nghĩa**: Các module cấp cao không nên phụ thuộc vào các module cấp thấp. Cả hai nên phụ thuộc vào các abstraction. Abstraction không nên phụ thuộc vào chi tiết. Chi tiết nên phụ thuộc vào abstraction.

**Ví dụ:**

```java
class LightBulb {
    public void turnOn() {
        // Logic bật đèn
    }

    public void turnOff() {
        // Logic tắt đèn
    }
}

class Switch {
    private LightBulb lightBulb;

    public Switch(LightBulb lightBulb) {
        this.lightBulb = lightBulb;
    }

    public void operate() {
        // Logic vận hành công tắc
    }
}

```
Lớp Switch phụ thuộc vào lớp LightBulb, vi phạm DIP. Để tuân theo DIP, chúng ta nên sử dụng một interface để trừu tượng hóa hành vi của đèn:

```java

interface Switchable {
    void turnOn();
    void turnOff();
}

class LightBulb implements Switchable {
    public void turnOn() {
        // Logic bật đèn
    }

    public void turnOff() {
        // Logic tắt đèn
    }
}

class Switch {
    private Switchable switchableDevice;

    public Switch(Switchable switchableDevice) {
        this.switchableDevice = switchableDevice;
    }

    public void operate() {
        // Logic vận hành công tắc
    }
}
