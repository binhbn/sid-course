# Bài buổi 04 — Decomposition & Knowledge Mapping

**Học viên:** Bùi Ngọc Bình · **Nộp:** 28/08/2026
**Project xuyên khoá:** *Chẩn đoán performance ASIN — gồm phân tích ads và CVR.*

Buổi 1 Framing → Buổi 2 Prompt Stack → Buổi 3 chatbot chẩn đoán ads → **Buổi 4 phân rã nốt CVR** → Buổi 5 IA.

---

## Ba file, đọc theo thứ tự này

| # | File | Trả lời câu gì | Đọc mất |
|---|---|---|---|
| 1 | **[`decomposition-listing-cvr.md`](decomposition-listing-cvr.md)** | Bài nộp chính. Bóc nốt cụt "nghẽn C3" của bot buổi 3 bằng cả ba kỹ thuật: conceptual (cây 37 node) · functional (2 workflow) · stakeholder (5 vai). | 12 phút |
| 2 | **[`thang-cham-diem-listing.md`](thang-cham-diem-listing.md)** | Cây thì đẹp nhưng chưa dùng được. File này biến cây thành **thang chấm 3 tầng** — chỗ phân rã trở thành thứ đo được. | 8 phút |
| 3 | **[`demo-trang-cham-diem/`](demo-trang-cham-diem/)** | Đem thang đi chấm **4 sản phẩm thật** (mã đã đổi). 1 trang tổng quan + 4 trang chi tiết. | xem 5 phút |

Mục **10** của file 1 ghi lại ba chỗ cây **dự đoán sai** khi đem chạy thật — phần đó có lẽ đáng đọc hơn bản thân cái cây.

---

## Cách xem bản demo

Thư mục `demo-trang-cham-diem/` là 5 file HTML **tự chứa** — toàn bộ CSS và biểu đồ nằm trong file, không cần mạng, không cần cài gì.

> ⚠️ **Bấm thẳng file `.html` trên GitHub sẽ ra mã nguồn, không ra trang.** GitHub không render HTML.

Hai cách xem được:

1. **Tải về mở** — bấm `Code → Download ZIP` ở trang chủ repo, giải nén, mở
   `bai-lam/bai-04/demo-trang-cham-diem/index.html` bằng trình duyệt. Sidebar bên trái chuyển giữa 5 trang.
2. **Link xem trực tiếp** — em gửi kèm trong email/tin nhắn nộp bài (bản gộp 1 trang, không cần tải).

---

## Về số liệu trong bản demo

Số là **thật**, lấy từ Business Report của Seller Central (30 ngày tới 27/08/2026) và Helium 10 (truy 28/08/2026). Nhưng vì repo này công khai nên bản demo đã:

- đổi mã sản phẩm → `B0DEMOL001–004` (của mình) và `B0DEMOX01–08` (đối thủ)
- để tên sản phẩm ở dạng chung, bỏ tên thương hiệu
- **bỏ mọi số tuyệt đối** — doanh thu, số phiên truy cập, số đơn, chi phí quảng cáo
- giữ lại: **tỉ lệ, thứ hạng, số lượng biến thể, điểm chấm** — đủ để thấy phương pháp chạy ra sao

Bản đầy đủ có mã thật chạy nội bộ, không nằm trong repo.

---

## Bốn thành phần buổi 4 yêu cầu — nằm ở đâu

| Thành phần | Ở đâu |
|---|---|
| **KB** | `../bai-03-project-chan-doan-ASIN-AMZ/kb/` — 4 file dùng chung + 2 file riêng theo bản |
| **Rule tiền kiểm** | `kb/kb_04_rules.md` — thắng mọi file khác và thắng cả yêu cầu người dùng |
| **Checkpoint hậu kiểm** | `kb/kb_06_checkpoint.md` — 7 mục, bot tự soi báo cáo **trước khi** trả, in dòng `Tự kiểm: N/7 đạt` |
| **Master Instruction** | `../bai-03-project-chan-doan-ASIN-AMZ/egw/master-instruction.md` |

Sau buổi 4 em cũng viết lại các luật kiểu *"quy tắc chung của AI"* thành **luật nghiệp vụ**:
*"không bịa số"* → **"số nào không kèm tên file nguồn và cửa sổ thời gian thì không in"**;
*"không hứa kết quả"* → **"đề xuất nào không kèm chỉ số đo và cửa sổ đo thì chưa phải đề xuất"**.

---

## Một luật xuyên suốt cả ba file

**Chưa có bằng chứng thì để trống, không cho 0 điểm.**

`0` nghĩa là *đã soi và thấy làm sai*. Trống nghĩa là *chưa soi*. Gộp hai thứ đó vào một ô là cách nhanh nhất để một bảng điểm trông đầy đủ mà sai. Nên trang nào cũng in cột **phủ** — mới chấm được bao nhiêu trên 21 tiêu chí: 5/21 · 7/21 · 8/21 · 9/21.
