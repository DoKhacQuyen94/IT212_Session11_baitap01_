# Bài 5: Xây Dựng Workflow Tích Hợp

## 1. Thiết kế luồng (Input / Output / Sơ đồ xử lý)

### Input cần có:

- `orderAmountVnd`: tổng giá trị đơn hàng tính bằng VNĐ.
- `orderDate`: ngày đặt hàng để xác định có thuộc cuối tuần hay không.
- `vipCustomer`: cờ boolean xác định khách hàng VIP.

### Output trả về:

- `pointsEarned`: số điểm thưởng sau khi tính toán theo business rule.

### Logic tính toán:

1. Tính điểm cơ bản: mỗi 100.000 VNĐ được 1 điểm.
2. Kiểm tra ngày cuối tuần: nếu là Thứ 7 hoặc Chủ Nhật thì nhân đôi tổng điểm.
3. Nếu khách VIP thì cộng thêm 20% vào điểm đã có sau các ưu đãi khác.
4. Với giá trị đơn hàng âm hoặc dữ liệu không hợp lệ, ném `IllegalArgumentException`.

### Sơ đồ xử lý ASCII:

```
[Start]
   |
   v
[Nhập orderAmountVnd, orderDate, vipCustomer]
   |
   v
[Validate orderAmountVnd >= 0]
   |
   v
[basePoints = floor(orderAmountVnd / 100000)]
   |
   v
[isWeekend?]--yes-->[points = basePoints * 2]
   |                       |
   no                      v
   |                   [points = basePoints]
   v                       |
[isVIP?]--yes-->[points = round(points * 1.2)]
   |                       |
   no                      v
   |                   [Return points]
   v
[Return points]
   |
   v
[End]
```

## 2. Workflow / Mega-Prompt sử dụng:

```text
Bạn là một AI Solution Architect và Senior Java Developer.
Mục tiêu: Sinh mã nguồn Java cho tính năng Loyalty Points theo yêu cầu TechShop, thêm Javadoc và viết Unit Test JUnit 5 bao phủ cả edge cases.
Ngữ cảnh: Business Analyst chỉ cung cấp ba quy tắc sau:
- Cứ tiêu 100.000 VNĐ thì được 1 điểm thưởng.
- Nếu mua hàng vào cuối tuần (Thứ 7, Chủ Nhật) thì tổng số điểm được nhân đôi.
- Khách hàng VIP luôn được cộng thêm 20% vào số điểm cuối cùng (sau khi đã tính các ưu đãi khác).
Yêu cầu:
1. Sinh một lớp Java `LoyaltyPointsCalculator` với phương thức `calculatePoints(long orderAmountVnd, LocalDate orderDate, boolean vipCustomer)`.
2. Logic phải:
   - Tính điểm cơ bản bằng cách lấy `orderAmountVnd / 100000` và làm tròn xuống.
   - Nhân đôi điểm nếu ngày đặt hàng là cuối tuần.
   - Cộng thêm 20% nếu là VIP, sau khi đã áp dụng ưu đãi cuối tuần.
3. Trả về số điểm cuối cùng dưới dạng `int`.
4. Nếu `orderAmountVnd` âm, ném `IllegalArgumentException`.
5. Sinh thêm Javadoc cho lớp và phương thức.
6. Viết một lớp JUnit 5 test đầy đủ, bao gồm các trường hợp:
   - orderAmount đúng chuẩn, không phải cuối tuần, không VIP,
   - orderAmount vào cuối tuần,
   - khách VIP,
   - orderAmount âm (nên ném ngoại lệ),
   - orderAmount bằng 0.
7. Trả về toàn bộ mã Java trong một code block duy nhất, không giải thích thêm.
```

## 3. Kết quả AI sinh và Text Log kiểm chứng:

### Text Log:

User:

```text
Bạn là một AI Solution Architect và Senior Java Developer.
Mục tiêu: Sinh mã nguồn Java cho tính năng Loyalty Points theo yêu cầu TechShop, thêm Javadoc và viết Unit Test JUnit 5 bao phủ cả edge cases.
Ngữ cảnh: Business Analyst chỉ cung cấp ba quy tắc sau:
- Cứ tiêu 100.000 VNĐ thì được 1 điểm thưởng.
- Nếu mua hàng vào cuối tuần (Thứ 7, Chủ Nhật) thì tổng số điểm được nhân đôi.
- Khách hàng VIP luôn được cộng thêm 20% vào số điểm cuối cùng (sau khi đã tính các ưu đãi khác).
Yêu cầu:
1. Sinh một lớp Java `LoyaltyPointsCalculator` với phương thức `calculatePoints(long orderAmountVnd, LocalDate orderDate, boolean vipCustomer)`.
2. Logic phải:
   - Tính điểm cơ bản bằng cách lấy `orderAmountVnd / 100000` và làm tròn xuống.
   - Nhân đôi điểm nếu ngày đặt hàng là cuối tuần.
   - Cộng thêm 20% nếu là VIP, sau khi đã áp dụng ưu đãi cuối tuần.
3. Trả về số điểm cuối cùng dưới dạng `int`.
4. Nếu `orderAmountVnd` âm, ném `IllegalArgumentException`.
5. Sinh thêm Javadoc cho lớp và phương thức.
6. Viết một lớp JUnit 5 test đầy đủ, bao gồm các trường hợp:
   - orderAmount đúng chuẩn, không phải cuối tuần, không VIP,
   - orderAmount vào cuối tuần,
   - khách VIP,
   - orderAmount âm (nên ném ngoại lệ),
   - orderAmount bằng 0.
7. Trả về toàn bộ mã Java trong một code block duy nhất, không giải thích thêm.
```

AI:

```java
import java.math.BigDecimal;
import java.math.RoundingMode;
import java.time.DayOfWeek;
import java.time.LocalDate;

/**
 * LoyaltyPointsCalculator computes loyalty points based on order amount, order date, and VIP status.
 */
public class LoyaltyPointsCalculator {
    /**
     * Calculates loyalty points using TechShop business rules.
     *
     * @param orderAmountVnd total order amount in VND
     * @param orderDate order date used to determine weekend bonus
     * @param vipCustomer true when the customer is VIP
     * @return calculated loyalty points
     * @throws IllegalArgumentException when orderAmountVnd is negative
     */
    public int calculatePoints(long orderAmountVnd, LocalDate orderDate, boolean vipCustomer) {
        if (orderAmountVnd < 0) {
            throw new IllegalArgumentException("Order amount must not be negative");
        }

        long basePoints = orderAmountVnd / 100000;
        long pointsAfterWeekendBonus = isWeekend(orderDate) ? basePoints * 2 : basePoints;

        if (vipCustomer) {
            BigDecimal vipMultiplier = BigDecimal.valueOf(pointsAfterWeekendBonus).multiply(BigDecimal.valueOf(1.2));
            return vipMultiplier.setScale(0, RoundingMode.HALF_UP).intValueExact();
        }

        return Math.toIntExact(pointsAfterWeekendBonus);
    }

    private boolean isWeekend(LocalDate orderDate) {
        DayOfWeek dayOfWeek = orderDate.getDayOfWeek();
        return dayOfWeek == DayOfWeek.SATURDAY || dayOfWeek == DayOfWeek.SUNDAY;
    }
}

import org.junit.jupiter.api.Test;
import java.time.LocalDate;

import static org.junit.jupiter.api.Assertions.*;

class LoyaltyPointsCalculatorTest {
    private final LoyaltyPointsCalculator calculator = new LoyaltyPointsCalculator();

    @Test
    void testNormalWeekdayNonVip() {
        int points = calculator.calculatePoints(250000, LocalDate.of(2026, 7, 1), false);
        assertEquals(2, points);
    }

    @Test
    void testWeekendNonVip() {
        int points = calculator.calculatePoints(300000, LocalDate.of(2026, 7, 4), false);
        assertEquals(6, points);
    }

    @Test
    void testVipAfterWeekendBonus() {
        int points = calculator.calculatePoints(300000, LocalDate.of(2026, 7, 4), true);
        assertEquals(7, points);
    }

    @Test
    void testOrderAmountZero() {
        int points = calculator.calculatePoints(0, LocalDate.of(2026, 7, 2), false);
        assertEquals(0, points);
    }

    @Test
    void testNegativeOrderAmountThrows() {
        assertThrows(IllegalArgumentException.class, () ->
                calculator.calculatePoints(-1000, LocalDate.of(2026, 7, 2), false));
    }
}
```
