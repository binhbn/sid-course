# Bài nộp theo buổi

Cả khoá là **một project xuyên suốt** — *Chẩn đoán performance ASIN, gồm phân tích quảng cáo và tỉ lệ
chuyển đổi listing*. Nên bài nộp của mỗi buổi là **một tầng của cùng một sản phẩm**, không phải bài rời.

Sản phẩm chạy được: [`../chan-doan-asin/`](../chan-doan-asin/)

---

## Buổi nào nộp gì

> **Lưu ý số hiệu:** số buổi học của lớp **lệch một nhịp** so với số bài trong repo tài liệu của thầy.
> Bài `bai-03-decomposition` của thầy được học ở **buổi 04** của lớp. Cột thứ hai dưới đây là để thầy
> đối chiếu đúng bài, khỏi lần.

| Buổi lớp | Bài trong repo thầy | Nội dung | Bài nộp | Đọc mất |
|---|---|---|---|---|
| **01** | `bai-01-framing` | Framing | [`thiet-ke/01-framing-brief.md`](../chan-doan-asin/thiet-ke/01-framing-brief.md) · [`quiz-buoi-01.md`](quiz-buoi-01.md) | 6' |
| **02** | `bai-02-prompt-stack` | Prompt Stack (RTC-COE) | [`thiet-ke/02-prompt-stack-rtc-coe.md`](../chan-doan-asin/thiet-ke/02-prompt-stack-rtc-coe.md) · [`thiet-ke/07-rtc-coe-prompt.md`](../chan-doan-asin/thiet-ke/07-rtc-coe-prompt.md) · [`quiz-buoi-02.md`](quiz-buoi-02.md) | 8' |
| **03** | — (repo thầy không có file bài riêng) | Chatbot có KB | [`chan-doan-asin/`](../chan-doan-asin/) — README cài đặt · [`master-instruction.md`](../chan-doan-asin/master-instruction.md) · [`kb/`](../chan-doan-asin/kb/) 6 file · [`thiet-ke/03-framing-brief-v2.md`](../chan-doan-asin/thiet-ke/03-framing-brief-v2.md) · [`thiet-ke/04-kien-truc-SID.md`](../chan-doan-asin/thiet-ke/04-kien-truc-SID.md) | 15' |
| **04** | `bai-03-decomposition` | Decomposition & Knowledge Mapping | [`thiet-ke/05-decomposition-listing-cvr.md`](../chan-doan-asin/thiet-ke/05-decomposition-listing-cvr.md) · [`thang-cham-diem-listing.md`](../chan-doan-asin/thang-cham-diem-listing.md) · [`demo/`](../chan-doan-asin/demo/) | 20' + xem demo |
| **cuối** | `bai-04-information-architect` | Kiến trúc thông tin & cách biểu diễn | [`bai-04-kien-truc-thong-tin/`](bai-04-kien-truc-thong-tin/) — 4 bài tập + báo cáo cuối | 15' |

---

## Bài 3 của thầy (Decomposition) — 5 mục yêu cầu nằm ở đâu

Bài này em không tách file riêng, nó nằm trong tài liệu thiết kế của project. Bảng dưới trỏ thẳng
tới từng mục thầy yêu cầu trong [`bai-03-decomposition-knowledge-mapping-v2.md`](https://github.com/rooneyhoi/SID-course-v2/blob/main/workshop/bai-03-decomposition/bai-03-decomposition-knowledge-mapping-v2.md) §10:

File: [`chan-doan-asin/thiet-ke/05-decomposition-listing-cvr.md`](../chan-doan-asin/thiet-ke/05-decomposition-listing-cvr.md)

| Thầy yêu cầu | Ngưỡng tối thiểu | Nằm ở mục | Thực tế |
|---|---|---|---|
| 1. Framing tóm tắt | 3–5 dòng | §1 | ✅ |
| 2. Decomposition Tree | ≥3 nhánh cấp 1 · ≥20 node · 3–4 tầng | §3 | 5 nhánh · 37 node · 4 tầng |
| 3. Functional **hoặc** Stakeholder map | chọn 1 | §4 và §5 | làm cả hai |
| 4. Lý giải lựa chọn | 150–250 từ | §7 | ~220 từ |
| 5. Tự chấm theo rubric | /30 | §9 | 26/30 |

Phần thầy không bắt buộc mà em làm thêm: §6 so sánh ba kỹ thuật trên cùng chủ đề, §10 ba chỗ cây
phân rã **dự đoán sai** khi đem chạy trên sản phẩm thật.

---

## Nếu thầy chỉ có 10 phút

Đọc đúng ba thứ, theo thứ tự:

1. **[`thiet-ke/05-decomposition-listing-cvr.md`](../chan-doan-asin/thiet-ke/05-decomposition-listing-cvr.md)** — bài buổi 4. Đặc biệt **mục 10**: ba chỗ cây phân rã **dự đoán sai** khi đem chạy trên sản phẩm thật.
2. **[`thang-cham-diem-listing.md`](../chan-doan-asin/thang-cham-diem-listing.md) mục 0** — vì sao thang phải có 3 tầng chứ không phải một bảng điểm phẳng. Đây là chỗ em sai rồi sửa: suýt xoá nhóm tiêu chí "đếm số lượng" vì suy từ **mẫu lệch**.
3. **[`demo/index.html`](../chan-doan-asin/demo/index.html)** — 4 sản phẩm cùng bị gắn nhãn "chuyển đổi thấp", **bốn nguyên nhân khác nhau**, chỉ một con thật sự nghẽn ở listing.

---

## Bốn thành phần buổi 4 yêu cầu — nằm ở đâu

| Thành phần | File |
|---|---|
| **KB** | [`chan-doan-asin/kb/`](../chan-doan-asin/kb/) — 6 file |
| **Rule tiền kiểm** | [`kb/kb_04_rules.md`](../chan-doan-asin/kb/kb_04_rules.md) — thắng mọi file khác, thắng cả yêu cầu người dùng |
| **Checkpoint hậu kiểm** | [`kb/kb_06_checkpoint.md`](../chan-doan-asin/kb/kb_06_checkpoint.md) — 7 mục, bot tự soi trước khi trả, in `Tự kiểm: N/7 đạt` |
| **Master Instruction** | [`chan-doan-asin/master-instruction.md`](../chan-doan-asin/master-instruction.md) |

Sau buổi 4 em cũng viết lại các luật kiểu *"quy tắc chung của AI"* thành **luật nghiệp vụ**:
*"không bịa số"* → **"số nào không kèm tên file nguồn và cửa sổ thời gian thì không in"**;
*"không hứa kết quả"* → **"đề xuất nào không kèm chỉ số đo và cửa sổ đo thì chưa phải đề xuất"**.
