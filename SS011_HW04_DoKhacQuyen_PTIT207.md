# Bài 4: Chiến Lược Sinh Dữ Liệu Giả Lập & Test Case

## 1. Mục tiêu:

Đánh giá tư duy thiết kế kịch bản kiểm thử (Test Cases), xử lý ngoại lệ (Edge Cases) và năng lực dùng Mega-Prompt để AI sinh đồng thời cả dữ liệu giả lập lẫn mã nguồn Unit Test.

## 2. Mega-Prompt sử dụng:

```text
Bạn là một Senior Java Developer và kiêm Test Architect.
Mục tiêu: Sinh một bộ Mock Data JSON và mã nguồn JUnit 5 Test Case để kiểm thử phương thức login(String username, String password) của lớp UserAuthentication.
Ngữ cảnh: Hệ thống chưa có database test, nên phải dùng dữ liệu giả lập trực tiếp.
Yêu cầu:
1. Sinh một file JSON mock gồm 5 user cho các kịch bản:
   - user hợp lệ với thông tin username và password đúng,
   - user với mật khẩu sai,
   - user bị khóa (locked),
   - user chứa payload SQL Injection trong username hoặc password,
   - user có mật khẩu rỗng.
2. Sinh mã nguồn JUnit 5 Test Case bằng Java để đọc bộ JSON mock data và test phương thức login().
3. Không cần cung cấp source code hàm login; AI tự giả định logic login dựa vào tên hàm và kịch bản test.
4. Test Case phải bao gồm các assert phù hợp cho từng kịch bản.
5. Trả về hai phần rõ ràng: phần JSON mock dữ liệu và phần mã Java JUnit 5 trong code block tương ứng.

### Định dạng đầu ra mong muốn:
- Đầu tiên là khối JSON chứa mock data.
- Sau đó là khối Java chứa mã JUnit 5.
- Không giải thích thêm.
```

## 3. Mock Data JSON:

```json
[
  {
    "username": "john.doe",
    "password": "SecurePass123",
    "status": "active"
  },
  {
    "username": "jane.doe",
    "password": "WrongPassword",
    "status": "active"
  },
  {
    "username": "locked.user",
    "password": "LockedPass",
    "status": "locked"
  },
  {
    "username": "admin' OR '1'='1",
    "password": "password123",
    "status": "active"
  },
  {
    "username": "empty.password",
    "password": "",
    "status": "active"
  }
]
```

## 4. JUnit 5 Test Case Java:

```java
import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

import static org.junit.jupiter.api.Assertions.*;

class UserAuthenticationTest {
    private static List<UserRecord> mockUsers;
    private static UserAuthentication auth;

    @BeforeAll
    static void setup() throws IOException {
        ObjectMapper mapper = new ObjectMapper();
        try (InputStream stream = UserAuthenticationTest.class.getResourceAsStream("/mock-users.json")) {
            mockUsers = mapper.readValue(stream, new TypeReference<>() {});
        }
        auth = new UserAuthentication();
    }

    @Test
    void testValidCredentials() {
        UserRecord user = mockUsers.get(0);
        assertTrue(auth.login(user.getUsername(), user.getPassword()));
    }

    @Test
    void testInvalidPassword() {
        UserRecord user = mockUsers.get(1);
        assertFalse(auth.login(user.getUsername(), user.getPassword()));
    }

    @Test
    void testLockedUser() {
        UserRecord user = mockUsers.get(2);
        assertFalse(auth.login(user.getUsername(), user.getPassword()));
    }

    @Test
    void testSqlInjectionUsername() {
        UserRecord user = mockUsers.get(3);
        assertFalse(auth.login(user.getUsername(), user.getPassword()));
    }

    @Test
    void testEmptyPassword() {
        UserRecord user = mockUsers.get(4);
        assertFalse(auth.login(user.getUsername(), user.getPassword()));
    }

    private static class UserRecord {
        private String username;
        private String password;
        private String status;

        public String getUsername() {
            return username;
        }

        public String getPassword() {
            return password;
        }

        public String getStatus() {
            return status;
        }
    }
}
```
