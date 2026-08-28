# Bài nộp theo buổi

Cả khoá là **một project xuyên suốt** — *Chẩn đoán performance ASIN, gồm phân tích quảng cáo và tỉ lệ
chuyển đổi listing*. Nên bài nộp của mỗi buổi là **một tầng của cùng một sản phẩm**, không phải bài rời.

Sản phẩm chạy được: [`../chan-doan-asin/`](../chan-doan-asin/)

---

## Buổi nào nộp gì

| Buổi | Nội dung | Bài nộp | Đọc mất |
|---|---|---|---|
| **01** | Framing | [`thiet-ke/01-framing-brief.md`](../chan-doan-asin/thiet-ke/01-framing-brief.md) · [`quiz-buoi-01.md`](quiz-buoi-01.md) | 6' |
| **02** | Prompt Stack (RTC-COE) | [`thiet-ke/02-prompt-stack-rtc-coe.md`](../chan-doan-asin/thiet-ke/02-prompt-stack-rtc-coe.md) · [`thiet-ke/07-rtc-coe-prompt.md`](../chan-doan-asin/thiet-ke/07-rtc-coe-prompt.md) · [`quiz-buoi-02.md`](quiz-buoi-02.md) | 8' |
| **03** | Chatbot có KB | [`chan-doan-asin/`](../chan-doan-asin/) — README cài đặt · [`master-instruction.md`](../chan-doan-asin/master-instruction.md) · [`kb/`](../chan-doan-asin/kb/) 6 file · [`thiet-ke/03-framing-brief-v2.md`](../chan-doan-asin/thiet-ke/03-framing-brief-v2.md) · [`thiet-ke/04-kien-truc-SID.md`](../chan-doan-asin/thiet-ke/04-kien-truc-SID.md) | 15' |
| **04** | Decomposition & Knowledge Mapping | [`thiet-ke/05-decomposition-listing-cvr.md`](../chan-doan-asin/thiet-ke/05-decomposition-listing-cvr.md) · [`thang-cham-diem-listing.md`](../chan-doan-asin/thang-cham-diem-listing.md) · [`demo/`](../chan-doan-asin/demo/) | 20' + xem demo |

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
