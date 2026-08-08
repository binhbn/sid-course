# Bài 02 — Bài làm trắc nghiệm tự kiểm tra (10 câu)

**Học viên:** Bùi Ngọc Bình
**Đề bài:** `kiem-tra/bai-02-quiz.md` — Prompt như giao diện nhận thức, RTC-COE, Task Decomposition, Prompt Stack
**Ngày làm:** 08/08/2026

---

## Bảng đáp án

| Câu | Nội dung câu hỏi | Đáp án chọn |
|---:|---|:---:|
| 1 | Prompt nên được hiểu đúng nhất là gì | **C** |
| 2 | Prompt Engineering được hiểu đúng nhất là | **B** |
| 3 | Thành phần RTC-COE xác định AI cần tạo ra artifact gì | **C** |
| 4 | Ví dụ đúng của phần Output trong RTC-COE | **C** |
| 5 | Vì sao viết đủ RTC-COE rồi vẫn chưa nên dùng một prompt duy nhất | **B** |
| 6 | Mục đích chính của Task Decomposition | **B** |
| 7 | Dấu hiệu cho thấy một yêu cầu nên được Task Decomposition | **B** |
| 8 | Phát biểu đúng nhất về Prompt Stack | **B** |
| 9 | Quan hệ giữa RTC-COE và Prompt Stack | **C** |
| 10 | Thứ tự phản ánh đúng quy trình của Buổi 2 | **B** |

**Đáp án rút gọn:** 1C · 2B · 3C · 4C · 5B · 6B · 7B · 8B · 9C · 10B

---

## Căn cứ trong tài liệu Buổi 2

### Phần đầu — Prompt và Prompt Engineering (câu 1–2)

- **Câu 1 (C).** Mục 2.1–2.2: prompt không chỉ là câu hỏi mà là *"giao diện bằng ngôn ngữ để điều khiển AI tạo ra một đầu ra cụ thể"*. A thu hẹp prompt về câu hỏi; B nhầm sang tiêu chí độ dài, trong khi mục 3.2 nói rõ *"Prompt Engineering tốt không được đo bằng độ dài của prompt"*; D nhầm prompt sang cấu hình kỹ thuật.
- **Câu 2 (B).** Mục 3.2 định nghĩa nguyên văn: *"quá trình thiết kế, kiểm tra và cải tiến prompt để AI hiểu đúng nhiệm vụ và tạo ra đầu ra phù hợp hơn"*. Bài cũng bác thẳng cách hiểu "câu thần chú" (loại C) và không giới hạn cho lập trình viên (loại D).

### Phần RTC-COE (câu 3–5)

- **Câu 3 (C).** Bảng mục 4.1: **Output** trả lời *"kết quả cần được trình bày thành artifact nào"*. Role là góc nhìn, Context là bối cảnh, Constraints là giới hạn.
- **Câu 4 (C).** "Bảng so sánh gồm 5 tiêu chí + khuyến nghị cuối cùng" gọi được tên artifact và đếm được. Ba phương án còn lại — "thật chi tiết", "dễ hiểu", "suy nghĩ kỹ" — đều là yêu cầu về cách làm chứ không phải dạng đầu ra, và mục 3.3 xếp chúng vào nhóm không kiểm tra được.
- **Câu 5 (B).** Mục 7.1 nêu đúng tình huống này: prompt *"không thiếu RTC-COE"* nhưng *"phần Task và Output đang chứa quá nhiều nhiệm vụ nhận thức khác nhau"*. D sai vì bài nói rõ không phải prompt nào cũng cần tách thành stack.

### Phần Task Decomposition (câu 6–7)

- **Câu 6 (B).** Mục 7.2: bóc bài toán thành *"các task nhỏ hơn, có thứ tự và có quan hệ đầu vào–đầu ra rõ ràng"*. A nhầm sang việc trình bày cho dễ đọc; C và D chia nhỏ thành phần của prompt chứ không chia nhỏ bài toán.
- **Câu 7 (B).** Dấu hiệu là **nhiều artifact độc lập + output bước trước là input bước sau** — đúng định nghĩa dependency ở mục 7.4. A là dấu hiệu ngược lại (một artifact thì không cần tách). C và D chỉ nói prompt đã đủ thành phần, không liên quan tới số lượng nhiệm vụ.

### Phần Prompt Stack (câu 8–10)

- **Câu 8 (B).** Mục 8.3 bác thẳng phương án A: chuỗi Clarify → Structure → Design → Critique → Refine *"chỉ là một pattern thường gặp"*, không phải template cứng. Nguyên tắc chốt của mục này: các step phải suy ra từ Task Decomposition của bài toán cụ thể.
- **Câu 9 (C).** Mục 8.4: *"Task Decomposition quyết định stack cần những step nào. RTC-COE quyết định mỗi prompt trong từng step được thiết kế như thế nào."* Hai khung bổ sung nhau, không thay thế nhau.
- **Câu 10 (B).** Mục 1 vẽ luồng học của buổi đúng theo thứ tự: **Prompt → Prompt Engineering → RTC-COE → Task Decomposition → Prompt Stack**. Thứ tự này phản ánh logic của cả buổi — hiểu prompt là gì trước, rồi mới cải tiến nó, rồi mới có khung kiểm tra, rồi mới bóc task, cuối cùng mới nối thành stack.

---

## Ghi chú

Repo tài liệu khóa học không kèm đáp án chính thức. Phần "Căn cứ" ở trên là kết quả tự đối chiếu
với nội dung Buổi 2 (`workshop/bai-02-prompt-stack/bai-02-prompt-stack-cognitive-interface-v2.md`),
không phải bản chấm của giảng viên.
