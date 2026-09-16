# Bài 3: Đọc Hiểu, Dò Lỗi & Tái Cấu Trúc Mã Nguồn

## 1. Mục tiêu:

Nâng cao khả năng phân tích lỗi nghiệp vụ, điều hướng AI để tìm ra lỗi logic ẩn (không phải lỗi cú pháp) và tái cấu trúc (Refactoring) mã nguồn theo tiêu chuẩn Clean Code.

## 2. Giải thích lý do prompt cũ thất bại:

- Prompt cũ "Tìm lỗi và sửa code giúp tôi" không cung cấp bối cảnh nghiệp vụ, nên AI chỉ phân tích theo cú pháp chung và không biết đâu là điều kiện freeship đúng.
- AI thường ưu tiên tối ưu cấu trúc code thay vì kiểm tra chính xác business rule, vì thiếu dữ liệu để đối chiếu.
- Do đó, lỗi toán tử `>` thay vì `>=` bị bỏ qua, vì đây là sai lệch nghiệp vụ chứ không phải lỗi cú pháp hoặc lỗi cấu trúc dễ thấy.

## 3. Prompt nâng cao thiết kế:

```text
Bạn là Senior Java Developer của dự án TechShop.
Mục tiêu: Kiểm tra và sửa hàm tính điều kiện freeship dựa trên luật nghiệp vụ rõ ràng.
Ngữ cảnh: Theo Business Rule của TechShop, "Đơn hàng từ 500.000 VNĐ trở lên (>=) mới được freeship".
Đoạn code hiện tại đang dùng `if (order.getTotalAmount() > 500000)` và cần được refactor.
Yêu cầu:
- Xác định lỗi logic hiện tại liên quan đến toán tử so sánh.
- Sửa lỗi thành điều kiện đúng `>= 500000`.
- Refactor toàn bộ hàm để loại bỏ if-else thừa và viết code sạch, rõ ràng.
- Giữ nguyên chức năng freeship nhưng tối ưu biểu thức và cấu trúc.
- Trả về mã Java duy nhất trong code block, không giải thích thêm.
```

## 4. Kết quả Refactor của AI:

```java
public class ShippingPolicy {
    /**
     * Determines whether an order qualifies for freeship.
     *
     * @param order the order to evaluate
     * @return true if the order total amount is greater than or equal to 500000, otherwise false
     */
    public boolean qualifiesForFreeShipping(Order order) {
        return order.getTotalAmount() >= 500000;
    }
}
```
