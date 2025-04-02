# Strategy Design Pattern

## Giới thiệu

Strategy Design Pattern là một mẫu thiết kế hành vi (behavioral design pattern) cho phép xác định một nhóm các thuật toán, đặt mỗi thuật toán vào một lớp riêng biệt, và làm cho các đối tượng của chúng có thể hoán đổi cho nhau. Pattern này cho phép thuật toán thay đổi độc lập với client sử dụng nó.

## Khi nào nên sử dụng

- Khi bạn muốn sử dụng các phiên bản khác nhau của một thuật toán trong một đối tượng
- Khi bạn có nhiều lớp con chỉ khác nhau về hành vi
- Khi bạn muốn tránh điều kiện logic phức tạp (if-else, switch-case)
- Khi bạn muốn ẩn chi tiết triển khai thuật toán khỏi client

## Lợi ích của Strategy Pattern

1. **Tính linh hoạt**: Dễ dàng thay đổi thuật toán trong thời gian chạy
2. **Tính mở rộng**: Thêm thuật toán mới không cần thay đổi code hiện có
3. **Tách biệt code**: Tách biệt code triển khai thuật toán khỏi code sử dụng thuật toán
4. **Khả năng thay thế**: Các thuật toán có thể thay thế cho nhau

## Ví dụ minh họa bằng JavaScript

### Ví dụ 1: Hệ thống thanh toán

```javascript
// Định nghĩa các strategy classes (các chiến lược thanh toán)

// Interface Strategy (không bắt buộc trong JS nhưng giúp code rõ ràng hơn)
class PaymentStrategy {
  pay(amount) {
    throw new Error("Phương thức pay() phải được triển khai");
  }
}

// Concrete Strategy 1
class CreditCardStrategy extends PaymentStrategy {
  constructor(cardNumber, name, cvv, dateOfExpiry) {
    super();
    this.cardNumber = cardNumber;
    this.name = name;
    this.cvv = cvv;
    this.dateOfExpiry = dateOfExpiry;
  }

  pay(amount) {
    console.log(`${amount}đ đã được thanh toán bằng thẻ tín dụng`);
    // Logic xử lý thanh toán thẻ tín dụng thực tế sẽ nằm ở đây
  }
}

// Concrete Strategy 2
class PaypalStrategy extends PaymentStrategy {
  constructor(email, password) {
    super();
    this.email = email;
    this.password = password;
  }

  pay(amount) {
    console.log(`${amount}đ đã được thanh toán qua Paypal`);
    // Logic xử lý thanh toán Paypal thực tế sẽ nằm ở đây
  }
}

// Concrete Strategy 3
class MomoStrategy extends PaymentStrategy {
  constructor(phoneNumber) {
    super();
    this.phoneNumber = phoneNumber;
  }
  
  pay(amount) {
    console.log(`${amount}đ đã được thanh toán qua Momo`);
    // Logic xử lý thanh toán Momo thực tế sẽ nằm ở đây
  }
}

// Context class - lớp sử dụng các strategy
class ShoppingCart {
  constructor() {
    this.items = [];
    this.paymentStrategy = null;
  }

  addItem(item) {
    this.items.push(item);
  }
  
  // Thiết lập strategy
  setPaymentStrategy(paymentStrategy) {
    this.paymentStrategy = paymentStrategy;
  }
  
  // Tính tổng tiền
  calculateTotal() {
    return this.items.reduce((total, item) => total + item.price, 0);
  }
  
  // Thực hiện thanh toán bằng strategy đã chọn
  checkout() {
    if (!this.paymentStrategy) {
      throw new Error("Vui lòng chọn phương thức thanh toán");
    }
    
    const amount = this.calculateTotal();
    this.paymentStrategy.pay(amount);
  }
}

// Sử dụng
// Tạo các item
const item1 = { name: "Áo thun", price: 150000 };
const item2 = { name: "Quần jean", price: 450000 };

// Tạo giỏ hàng
const cart = new ShoppingCart();
cart.addItem(item1);
cart.addItem(item2);

// Thanh toán bằng thẻ tín dụng
const creditCardStrategy = new CreditCardStrategy("1234-5678-9012-3456", "Nguyen Van A", "123", "12/2025");
cart.setPaymentStrategy(creditCardStrategy);
cart.checkout();

// Thanh toán bằng Paypal
const paypalStrategy = new PaypalStrategy("nguyenvana@gmail.com", "password123");
cart.setPaymentStrategy(paypalStrategy);
cart.checkout();

// Thanh toán bằng Momo
const momoStrategy = new MomoStrategy("0912345678");
cart.setPaymentStrategy(momoStrategy);
cart.checkout();
```

### Ví dụ 2: Hệ thống sắp xếp

```javascript
// Khai báo interface
class SortStrategy {
  sort(dataset) {
    throw new Error("Phương thức sort() phải được triển khai");
  }
}

// Concrete strategies
class BubbleSortStrategy extends SortStrategy {
  sort(dataset) {
    console.log("Sắp xếp dữ liệu bằng Bubble Sort");
    // Logic sắp xếp Bubble Sort
    return [...dataset].sort((a, b) => a - b);
  }
}

class QuickSortStrategy extends SortStrategy {
  sort(dataset) {
    console.log("Sắp xếp dữ liệu bằng Quick Sort");
    // Logic sắp xếp Quick Sort
    return [...dataset].sort((a, b) => a - b);
  }
}

class MergeSortStrategy extends SortStrategy {
  sort(dataset) {
    console.log("Sắp xếp dữ liệu bằng Merge Sort");
    // Logic sắp xếp Merge Sort
    return [...dataset].sort((a, b) => a - b);
  }
}

// Context
class Sorter {
  constructor(strategy) {
    this.strategy = strategy;
  }
  
  setStrategy(strategy) {
    this.strategy = strategy;
  }
  
  sort(dataset) {
    return this.strategy.sort(dataset);
  }
}

// Sử dụng
const dataset = [5, 3, 8, 1, 2, 9, 4];
const sorter = new Sorter(new BubbleSortStrategy());

// Dùng Bubble Sort
console.log(sorter.sort(dataset));

// Chuyển sang Quick Sort
sorter.setStrategy(new QuickSortStrategy());
console.log(sorter.sort(dataset));

// Chuyển sang Merge Sort
sorter.setStrategy(new MergeSortStrategy());
console.log(sorter.sort(dataset));
```

## Ứng dụng thực tế

Strategy Pattern có thể áp dụng trong nhiều tình huống thực tế:

- Hệ thống thanh toán với nhiều phương thức thanh toán
- Hệ thống xác thực với nhiều phương thức xác thực
- Các thuật toán nén dữ liệu
- Thuật toán định tuyến
- Các chiến lược cache
- Thuật toán tìm kiếm và sắp xếp

## Cấu trúc

1. **Context**: Lớp sử dụng Strategy, chứa tham chiếu đến đối tượng Strategy hiện tại
2. **Strategy**: Interface chung cho tất cả các thuật toán cụ thể
3. **ConcreteStrategy**: Triển khai cụ thể của thuật toán, tuân theo interface Strategy

## Kết luận

Strategy Pattern là một mẫu thiết kế mạnh mẽ giúp tách biệt logic thuật toán khỏi client sử dụng nó. Pattern này tăng tính linh hoạt, mở rộng và tái sử dụng của code, đồng thời tuân thủ nguyên tắc "Open/Closed Principle" của thiết kế hướng đối tượng.
