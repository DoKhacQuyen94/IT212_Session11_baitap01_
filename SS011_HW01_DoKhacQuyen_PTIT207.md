# Bài 1: Phân Tích & Lựa Chọn Phương Án Tạo Mã Nguồn (Lesson 01)

## 1. Mục tiêu:

Đánh giá năng lực thiết kế và lựa chọn Prompt chuẩn cấu trúc để yêu cầu AI sinh mã nguồn chính xác, đầy đủ ràng buộc trong môi trường dự án.

## 2. Lựa chọn phương án tốt nhất:

**Phương án B** là lựa chọn tối ưu nhất.

## 3. Giải thích lý do chọn Phương án B:

- **Rõ ràng vai trò**: Prompt yêu cầu AI “Đóng vai Senior Java Developer”, giúp AI hiểu được phong cách và chất lượng mong muốn.
- **Đầy đủ bối cảnh**: Nêu rõ ngữ cảnh hệ thống E-commerce và mục tiêu validate trước khi gửi API thanh toán.
- **Ràng buộc kỹ thuật**: Yêu cầu Java 17 và thuật toán Luhn giúp đảm bảo mã nguồn phù hợp tiêu chuẩn dự án.
- **Xử lý ngoại lệ**: Bắt buộc ném `IllegalArgumentException` khi thẻ rỗng hoặc chứa chữ cái là yêu cầu cụ thể, giảm rủi ro AI sinh code thiếu kiểm tra.
- **Định dạng đầu ra**: Yêu cầu chỉ trả mã nguồn Java trong code block và không giải thích giúp tập trung vào kết quả đầu ra sạch, dễ sao chép.

## 4. Phân tích cấu trúc Prompt theo 5 thành phần:

1. **Người nhận (Role)**:
   - “Đóng vai Senior Java Developer” làm rõ AI cần mô phỏng người có chuyên môn, từ đó tăng khả năng sinh code chất lượng.
2. **Nhiệm vụ (Task)**:
   - “Viết class PaymentValidator kiểm tra tính hợp lệ của thẻ tín dụng” tập trung trực tiếp vào chức năng cần tạo.
3. **Ngữ cảnh (Context)**:
   - “Hệ thống E-commerce, cần validate trước khi gửi API thanh toán” giúp AI hiểu môi trường sử dụng và mục tiêu bảo mật, chính xác.
4. **Ràng buộc (Constraints)**:
   - “Dùng Java 17, triển khai thuật toán Luhn, ném ra ngoại lệ IllegalArgumentException nếu thẻ rỗng hoặc chứa chữ cái” đảm bảo code tuân thủ tiêu chuẩn kỹ thuật dự án và xử lý lỗi hợp lý.
5. **Định dạng đầu ra (Format)**:
   - “Chỉ trả về mã nguồn Java trong một code block, không giải thích” giúp đầu ra gọn, không cần lọc thông tin thừa.

## 5. Phân tích 2 phương án bị loại trừ:

### Phương án A:

- Prompt quá ngắn, thiếu bối cảnh dự án và thiếu ràng buộc kỹ thuật.
- Yêu cầu “Code ngắn gọn thôi nhé” có thể khiến AI tối giản quá mức, bỏ qua xử lý ngoại lệ hoặc các điều kiện kiểm tra đầu vào.
- Không chỉ rõ phiên bản Java, không yêu cầu code block, dễ dẫn đến đầu ra không đồng nhất và có thể kèm giải thích không cần thiết.
- Có nguy cơ AI sinh code chỉ chạy được cơ bản mà không đủ chuẩn cho môi trường sản phẩm.

### Phương án C:

- Yêu cầu không liên quan hoặc mở rộng quá mức so với bài toán: thêm database SQL và hướng dẫn cài đặt JDBC.
- Việc thêm lưu thông tin thẻ tín dụng trong cơ sở dữ liệu có thể vi phạm bảo mật và quy định lưu trữ thẻ.
- Nhiều khả năng AI sẽ rời xa mục tiêu chính là validate thẻ bằng thuật toán Luhn và sinh đầu ra quá phức tạp.
- Có khả năng tạo ra thông tin thừa, sai ngữ cảnh với hệ thống E-commerce hiện tại và không đáp ứng đúng yêu cầu dự án.

## 6. Kết luận:

- Chọn: **Phương án B**
- Lý do: Prompt có cấu trúc đầy đủ, rõ ràng, có vai trò, ngữ cảnh, ràng buộc kỹ thuật và định dạng đầu ra cụ thể.
- Phương án A bị loại vì thiếu chi tiết và ràng buộc, dễ dẫn đến kết quả không đủ chuẩn.
- Phương án C bị loại vì mở rộng quá mức, lệch mục tiêu và tiềm ẩn sai sót ngữ cảnh, bảo mật.
