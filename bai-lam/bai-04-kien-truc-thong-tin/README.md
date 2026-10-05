# Buổi cuối — Kiến trúc thông tin và cách biểu diễn

Bài `bai-04-information-architect` trong repo tài liệu của thầy.

**Vì sao bài này nằm riêng, không gộp vào project `chan-doan-asin/` như các buổi trước:** đề bài
**khoá cứng nguồn dữ liệu** — bài tập 1–4 bắt buộc dùng `bai-04-sample-01.md` (báo cáo kinh doanh
Công ty Minh An Retail) và bài cuối bắt buộc dùng **duy nhất** `bai-04-sample-02.md` (kết quả học
tập THCS Hoà Bình). Không cho tự chọn domain, nên không nối được vào mạch chẩn đoán ASIN.

---

## Nộp gì

| Phần | File | Nguồn dữ liệu | Đọc mất |
|---|---|---|---|
| BT1 — Taxonomy | [`bt1-taxonomy.md`](bt1-taxonomy.md) | sample-01 | 4' |
| BT2 — Hierarchy | [`bt2-hierarchy.md`](bt2-hierarchy.md) | sample-01 | 4' |
| BT3 — Bảng hỗ trợ quyết định | [`bt3-matrix.md`](bt3-matrix.md) | sample-01 | 5' |
| BT4 — Flowchart + Concept Map | [`bt4-mermaid.md`](bt4-mermaid.md) | sample-01 | 5' |
| **Bài cuối** — câu lệnh tổng hợp | [`cau-lenh-tong-hop.md`](cau-lenh-tong-hop.md) | sample-02 | 3' |
| **Bài cuối** — báo cáo | [`report.md`](report.md) | sample-02 | 10' |
| Tự chấm | [`tu-cham.md`](tu-cham.md) | — | 4' |

Mỗi bài tập có đủ ba phần thầy yêu cầu: **câu lệnh em tự viết · kết quả AI tạo ra · tự đánh giá**.
Không bài nào chép lại câu lệnh mẫu trong tài liệu — mỗi bài có một mục nói rõ đã đổi gì so với mẫu
và vì sao.

---

## Nếu thầy chỉ có 8 phút

1. **[`report.md`](report.md) mục 7** — bảy hạn chế dữ liệu. Đây là chỗ em giữ tay nhiều nhất:
   khuyến nghị số 4 **từ chối đánh giá** chương trình phụ đạo thay vì đưa kết luận nửa vời.
2. **[`bt1-taxonomy.md`](bt1-taxonomy.md) mục 2** — em tìm được một chỗ `sample-01` tự mâu thuẫn:
   tổng chi phí vận hành §5 ghi **926**, trong khi §2 cho **797**, dù chú thích nói hai con số cùng
   phạm vi. Lệch **129** ở tháng 6 và **100** ở tháng 5. Bài không chọn số nào làm số đúng.
3. **[`tu-cham.md`](tu-cham.md) mục 4** — kết quả quét tự động chữ mang nghĩa nhân quả trên
   `report.md`, soi từng lượt bị bắt.

**Tự chấm: 36/40.**

---

## Đối chiếu với bài mẫu của thầy

Em làm xong bài ngày 04/09/2026, lúc đó repo chưa có đáp án tham khảo. Thầy up
[`report.md`](https://github.com/rooneyhoi/SID-course-v2/blob/main/workshop/bai-04-information-architect/report.md)
ngày 01/10/2026, em đối chiếu lại như sau.

**Khớp:** mọi con số chính đều trùng với bài mẫu — điểm trung bình 7,2 (+0,6) · 73,8% mức Khá trở
lên · 16 em cần hỗ trợ · Tiếng Anh thấp nhất 6,9 · lớp 9A cao nhất 7,8 · lớp 8B thấp nhất 6,6 với
chuyên cần 91,8% · chênh lệch quan sát +1,2 điểm (bài tập đúng hạn) và +1,0 điểm (chuyên cần).

**Khác:**

| | Bài mẫu | Bài em |
|---|---|---|
| Nguồn số liệu | Không gắn | Mỗi con số gắn mục nguồn `[§n]` của `sample-02` |
| Hạn chế dữ liệu | 5 ý | 7 ý — thêm 3 chỗ dễ đọc sai: cộng số em dưới 5,0 của các môn ≠ số em cần hỗ trợ · 74 em đầu năm và 16 em cuối năm là hai thước đo khác nhau · lớp B thấp hơn lớp A ở cả bốn khối nhưng tệp không nói cách xếp lớp |
| Chương trình phụ đạo | Đánh giá bằng điểm trước–sau, kiểm soát đầu vào | Cùng hướng, nhưng nói rõ: với dữ liệu hiện có thì **chưa đánh giá được theo chiều nào** |
| Độ dài | 143 dòng | 350 dòng — dài hơn mức một Ban giám hiệu đọc được trong một lượt. Em đã tự trừ điểm chỗ này ở [`tu-cham.md`](tu-cham.md) mục 3 |
