# Bài 2: Tối Ưu Prompt Sinh Docstring & Giải Thích Logic

## 1. Phân tích lỗ hổng của prompt thô sơ:
- Prompt quá chung chung, không có vai trò người nhận (role) nên AI không biết cần trả lời với phong cách kỹ thuật chuyên nghiệp.
- Không có ngữ cảnh nghiệp vụ rõ ràng, chỉ hỏi “Giải thích hàm này làm gì”, dẫn AI dễ trả lời chung chung, thiếu liên kết với hệ thống e-commerce, chiết khấu và freeship.
- Không xác định rõ mapping biến `a`, `b`, `c`, nên AI có thể đoán sai ý nghĩa nghiệp vụ.
- Không yêu cầu Javadoc hay comment inline cụ thể, nên AI sẽ không chú trọng viết tài liệu bảo trì.
- Không có định dạng đầu ra cụ thể, làm đầu ra dễ bị lẫn giải thích chung và khó sử dụng trực tiếp.

## 2. Prompt tối ưu mới:
```text
Đóng vai Senior Java Developer kiêm Technical Writer.
Mục tiêu: Phân tích đoạn code legacy sau và viết lại nó bằng Java với Javadoc rõ ràng và comment inline cho mọi nhánh logic phức tạp.
Ngữ cảnh: Đây là phần tính chiết khấu trong hệ thống e-commerce. Biến nguyên thủy cần map như sau:
- a = order subtotal
- b = membership tier (1 = Silver, 2 = Gold)
- c = freeship voucher (1 = freeship voucher áp dụng, 0 = không áp dụng)
Ràng buộc:
- Giữ nguyên logic tính toán hiện tại.
- Đổi tên biến thành ý nghĩa, ví dụ orderSubtotal, membershipTier, freeShipVoucher.
- Thêm Javadoc cho lớp và phương thức.
- Thêm comment inline cho các điều kiện và bước tính toán quan trọng.
- Trả về duy nhất mã Java trong một code block, không giải thích thêm ngoài code.
Đoạn code cần phân tích:
public class DiscountCalculator {
    public static double cD(double a, int b, int c) {
        double d = 0;
        if (a >= 500) {
            d = a * 0.2;
        } else if (a >= 200) {
            d = a * 0.15;
        } else if (a >= 100) {
            d = a * 0.1;
        } else {
            d = a * 0.05;
        }
        if (b == 2) {
            d += 20;
        } else if (b == 1) {
            d += 10;
        }
        if (c == 1) {
            d += 5;
        }
        return d;
    }
}
```

## 3. Kết quả AI sinh hồi cứu:
```java
public class DiscountCalculator {
    /**
     * Calculates the total discount amount based on order subtotal, membership tier, and freeship voucher.
     *
     * @param orderSubtotal order subtotal before discount
     * @param membershipTier membership level where 1 = Silver, 2 = Gold
     * @param freeShipVoucher freeship voucher flag where 1 = freeship voucher applied, 0 = no voucher
     * @return the total discount amount in the same currency as the order subtotal
     */
    public static double calculateDiscount(double orderSubtotal, int membershipTier, int freeShipVoucher) {
        // Determine base discount percentage from subtotal tiers
        double discountAmount;
        if (orderSubtotal >= 500) {
            discountAmount = orderSubtotal * 0.2;
        } else if (orderSubtotal >= 200) {
            discountAmount = orderSubtotal * 0.15;
        } else if (orderSubtotal >= 100) {
            discountAmount = orderSubtotal * 0.1;
        } else {
            discountAmount = orderSubtotal * 0.05;
        }

        // Apply membership bonus discount: Gold gets 20, Silver gets 10
        if (membershipTier == 2) {
            discountAmount += 20;
        } else if (membershipTier == 1) {
            discountAmount += 10;
        }

        // Apply freeship voucher benefit as fixed shipping savings
        if (freeShipVoucher == 1) {
            discountAmount += 5;
        }

        return discountAmount;
    }
}
```
