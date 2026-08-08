# Bài 01 — Bài làm trắc nghiệm tự kiểm tra (12 câu)

**Học viên:** Bùi Ngọc Bình
**Đề bài:** `kiem-tra/bai-01-quiz.md` — Kỹ thuật Định khung (Framing) và mô hình 5W-O
**Ngày làm:** 08/08/2026

---

## Bảng đáp án

| Câu | Nội dung câu hỏi | Đáp án chọn |
|---:|---|:---:|
| 1 | Khác biệt cốt lõi giữa "Topic" và "Problem Framing" | **B** |
| 2 | Vì sao Framing được gọi là "Cognitive Control" | **C** |
| 3 | Câu lệnh chỉ có Topic thường dẫn đến kết quả nào | **C** |
| 4 | Xác định "Where" và "Task" trong ví dụ cửa hàng đồ uống | **B** |
| 5 | Ý nghĩa của Width/Depth với In-scope và Out-of-scope | **B** |
| 6 | Vì sao Output Framing được xem là quan trọng nhất | **B** |
| 7 | Dấu hiệu rõ nhất cho thấy Goal/Output Framing chưa xong | **C** |
| 8 | Artifact tối ưu cho nhân viên tổng đài đang nghe máy | **B** |
| 9 | Artifact phù hợp cho buổi đào tạo nội bộ 2 tiếng | **B** |
| 10 | Artifact hữu ích cho lãnh đạo quyết định thay nhân sự bằng AI | **C** |
| 11 | Vì sao prompt "phân tích chi tiết về CSKH..." bị xem là tồi | **B** |
| 12 | Yếu tố ngăn AI đề xuất giải pháp quá tầm | **B** |

**Đáp án rút gọn:** 1B · 2C · 3C · 4B · 5B · 6B · 7C · 8B · 9B · 10C · 11B · 12B

---

## Căn cứ trong tài liệu Buổi 1

Phần này ghi lại chỗ trong tài liệu Buổi 1 mà mỗi lựa chọn bám vào, dùng để tự đối chiếu.

### Phần 1 — Kiến thức nền tảng (câu 1–3)

- **Câu 1 (B).** Mục 5.1: *"Topic là 'đang nói về cái gì'. Problem là 'đang cần làm gì với cái đó'."* Phương án A đảo ngược hai khái niệm, C nhầm sang Audience và Output, D phủ nhận sự khác biệt.
- **Câu 2 (C).** Framing được đặt tên "Cognitive Control" vì nó **điều hướng phạm vi tư duy** của AI, không phải vì cung cấp thêm dữ liệu (A), sửa lỗi (B) hay tăng tốc độ (D). Hệ quả của việc không định khung là câu trả lời "đúng nhưng không dùng được".
- **Câu 3 (C).** Prompt chỉ có Topic khiến AI phải tự đoán nhiệm vụ nhận thức, nên nó chọn phương án an toàn nhất là tóm tắt kiến thức chung. A và B mô tả kết quả của prompt đã được định khung tốt, tức là ngược lại.

### Phần 2 — Mô hình 5W-O (câu 4–7)

- **Câu 4 (B).** Bảng 5W-O ở mục 4 định nghĩa **Where = Context = "ngành, quy mô, thời gian, ngân sách và ràng buộc thực tế"** → ngân sách 15 triệu/tháng và quy mô cửa hàng nhỏ. Task là **Design** vì sản phẩm cần nhận là một Campaign Playbook. Ba phương án còn lại đều gán sai task type (Analyze / Compare / Explain).
- **Câu 5 (B).** Scope Design trả lời *"phần nào cần làm, phần nào chưa làm"*. Độ dài văn bản (A), số lượng artifact (C) và trình độ người đọc (D) thuộc về Output và Audience.
- **Câu 6 (B).** Output Framing là thứ chuyển yêu cầu mơ hồ thành artifact gọi được tên — checklist, script, rubric. Phương án A mô tả Goal Framing; C là điều cần tránh vì để AI tự chọn định dạng chính là bỏ trống Output.
- **Câu 7 (C).** Dấu hiệu là **chưa gọi tên được sản phẩm cần nhận**, không phải thiếu thuật ngữ (A), thiếu ngân sách (B — đó là Context) hay prompt ngắn (D — độ dài không phải tiêu chí).

### Phần 3 — Artifact theo Audience (câu 8–10)

Ba câu này kiểm tra cùng một nguyên tắc: **đổi audience và tình huống sử dụng thì đổi luôn artifact.**

- **Câu 8 (B).** Người dùng đang nghe máy, cần thứ đọc được trong vài giây → checklist bước kiểm tra kèm script thoại. Syllabus (A), workflow 15 bước liên phòng ban (C) và concept note (D) đều không dùng được khi đang trực tiếp với khách.
- **Câu 9 (B).** Ràng buộc là một buổi 2 tiếng → cần khung nội dung chia theo khung giờ. Decision tree (A) và comparison table (C) không phải dạng tài liệu đào tạo; script đọc nguyên 2 tiếng (D) không ai dùng được.
- **Câu 10 (C).** Nhiệm vụ nhận thức ở đây là **ra quyết định giữa hai phương án**, nên artifact đúng là bảng so sánh. A, B, D đều là tài liệu triển khai — hữu ích sau khi đã quyết, không giúp việc quyết.

### Phần 4 — Phân tích và cải thiện prompt (câu 11–12)

- **Câu 11 (B).** Prompt chỉ nêu Topic, thiếu Audience, Scope và Artifact. Vấn đề không nằm ở chữ "chi tiết" (C) hay ở việc thiếu ví dụ (D).
- **Câu 12 (B).** "Giải pháp quá tầm" là hệ quả của việc AI không biết người dùng có nguồn lực gì và đâu là ranh giới không được vượt → cần **Audience rõ + Out-of-scope**. Đóng vai chuyên gia hàng đầu (A) làm vấn đề nặng thêm.

---

## Ghi chú

Repo tài liệu khóa học không kèm đáp án chính thức. Phần "Căn cứ" ở trên là kết quả tự đối chiếu
với nội dung Buổi 1 (`workshop/bai-01-framing/bai-01-framing-cognitive-control-v1.1.a.md`), không
phải bản chấm của giảng viên.
