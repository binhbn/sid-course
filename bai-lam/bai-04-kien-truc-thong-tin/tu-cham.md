# Tự chấm — Buổi 4, Kiến trúc thông tin

---

## 1. Đối chiếu 4 bài tập với yêu cầu của thầy

Mỗi bài thầy đòi đủ ba phần: câu lệnh tự viết · kết quả AI tạo · tự đánh giá 3–5 ý.

| Bài | Yêu cầu bắt buộc | Thực tế | Đạt |
|---|---|---|---|
| **BT1** [`bt1-taxonomy.md`](bt1-taxonomy.md) | Bảng Markdown · 4–6 nhóm đồng đẳng · mỗi bảng một nhóm chính · 3 cột bắt buộc · đánh dấu trường hợp mơ hồ | 6 nhóm · 8 bảng nguồn, mỗi bảng một nhóm · đủ 3 cột · 4 trường hợp mơ hồ có giải thích | ✅ |
| **BT2** [`bt2-hierarchy.md`](bt2-hierarchy.md) | 4–6 mục cấp I · ≥2 mục cấp II mỗi mục cấp I · ghi nguồn cạnh từng mục · thứ tự hợp người đọc · chưa viết nội dung | 5 cấp I · 15 cấp II · 11 cấp III · mục cấp I ít nhất vẫn có 3 cấp II · mọi mục có nguồn · không lọt số liệu | ✅ |
| **BT3** [`bt3-matrix.md`](bt3-matrix.md) | Hoàn thành câu "tôi muốn quyết định…" · ý nghĩa hàng/cột quy định trong câu lệnh · ≥1 phép so với tháng trước hoặc kế hoạch · cột tín hiệu quản trị · thiếu ghi `Chưa đủ dữ liệu` · truy ngược được | Câu quyết định ở mục 0 · hàng/cột quy định trong khối CONTEXT · có **cả hai** phép so · cột tín hiệu 4 nhãn · 2 cột ghi `Chưa đủ dữ liệu` · có bảng truy vết phép tính | ✅ |
| **BT4** [`bt4-mermaid.md`](bt4-mermaid.md) | Một câu lệnh ra 2 sơ đồ · flowchart ≥2 rẽ nhánh + 1 nhánh quay lại · concept map 10–16 khái niệm, ≥3 loại quan hệ có tên · 2 sơ đồ không trùng chức năng · 2 ý giải thích khác nhau · gắn `Giả định` | Một câu lệnh · flowchart 4 nút quyết định + 2 nhánh quay lại · concept map 16 khái niệm, 4 loại quan hệ · 2 ý so sánh · 4 mục gắn `Giả định` | ✅ |

---

## 2. Bốn cái bẫy trong `sample-02` — bắt được hay không

| # | Bẫy | Chỗ xử lý trong [`report.md`](report.md) |
|---|---|---|
| 1 | Cộng số HS dưới 5,0 của các môn để suy ra số HS duy nhất | Mục 5.1 có ô cảnh báo in đậm: 20 + 12 + 30 + 18 **không phải** số HS cần hỗ trợ. Số đúng là **16**, lấy từ `[§4]` và dòng tổng `[§6]`. Nhắc lại ở mục 7.2. |
| 2 | Biến chênh lệch quan sát thành nhân quả | Toàn báo cáo chỉ dùng "có liên hệ với", "đi kèm với", "chênh lệch quan sát". Kết quả quét tự động ở mục 4 dưới đây. |
| 3 | Suy đoán giới tính, hoàn cảnh kinh tế | Không xuất hiện ở bất kỳ đâu. Nêu thẳng ở mục 7.7 là dữ liệu không có và không được suy đoán. |
| 4 | Thiếu dữ liệu thì ghi `Chưa đủ dữ liệu` | Xuất hiện ở mục 2.1 (bối cảnh lớp), 7.2, 7.4, 7.5. Trong BT3 có hai cột nguyên vẹn ghi `Chưa đủ dữ liệu`. |

**Một bẫy thứ năm không nằm trong danh sách của thầy nhưng em thấy khi đọc kỹ:** `[§9]` ghi khảo sát
đầu năm đưa **74 em** vào danh sách theo dõi, `[§4]` ghi **16 em** cần hỗ trợ cuối năm. Rất dễ viết
"giảm từ 74 xuống 16" — nhưng hai con số này đo hai thứ khác nhau (lỗ hổng kiến thức đầu năm so với
điểm trung bình chung dưới 5,0 cuối năm). Xử lý ở mục 7.3.

---

## 3. Bảng tiêu chí tự chấm của thầy — /40

| Tiêu chí | Mô tả | Điểm | Căn cứ |
|---|---|---:|---|
| Mức độ rõ ràng của câu lệnh | Nêu rõ vai trò AI, nhiệm vụ, nguồn, giới hạn, cấu trúc báo cáo | **4**/5 | Có đủ ROLE · TASK 7 phần · CONTEXT nguồn duy nhất · 5 luật cứng · 7 bước tự kiểm. **Trừ 1:** không ràng buộc độ dài, nên báo cáo dài hơn mức một Ban giám hiệu đọc được trong một lượt — chính câu lệnh có LUẬT 5 "ưu tiên ra quyết định, không chép dữ liệu" mà lại không cho công cụ để ép mình gọn. |
| Độ chính xác của thông tin | Số liệu và phép so khớp dữ liệu mẫu 02 | **5**/5 | Chạy 42 phép kiểm tự động (21 phép trên `sample-02`, 21 phép trên `sample-01`) đối chiếu mọi con số dẫn xuất với dòng tổng của tệp nguồn — khớp toàn bộ. Chi tiết ở mục 5. |
| Cấu trúc thông tin | Mạch rõ từ kết luận đến bằng chứng và hành động | **4**/5 | Đúng bảy phần, mạch tóm tắt → yếu tố → sơ đồ → tổng kết → khuyến nghị → hạn chế. **Trừ 1:** mục 5.3 lặp lại phần lớn bảng chuyên cần `[§6]`, đúng chỗ LUẬT 5 cấm. |
| Phân tích các yếu tố | Phân nhóm rõ, đủ các môn, không nhầm liên hệ với nhân quả | **5**/5 | Ba nhóm yếu tố chia theo *mức nhà trường can thiệp được* · phân tích đủ bốn môn, mỗi môn có điểm riêng không lặp môn khác · thêm hai quan sát cắt ngang, trong đó có một **phản ví dụ** (lớp 9B phá mẫu hình chuyên cần ↔ kết quả). |
| Bản đồ khái niệm | Thời gian, can thiệp, hành vi, kết quả nối bằng quan hệ có ý nghĩa | **4**/5 | 4 loại quan hệ có tên, không loại nào mang nghĩa nhân quả · phát hiện được hai khoảng trống thật (chỉ 2/6 can thiệp chạm hành vi; không can thiệp nào nhắm vào tự học). **Trừ 1:** phải thêm nút "Mục tiêu kiến thức nền" nằm ngoài bốn loại thực thể đề bài nêu — trung thực với dữ liệu nhưng lệch khung đề. |
| Quy trình đánh giá | Bước, rẽ nhánh, nhánh sửa lỗi, người phê duyệt thể hiện rõ | **5**/5 | 9 bước lấy thẳng `[§11]` · 3 điểm rẽ nhánh · 2 nhánh quay lại · người phê duyệt có thật trong tệp (khác `sample-01` phải giả định) · 3 chỗ suy ra đều gắn `Giả định` kèm lý do. |
| Bảng tổng kết | Chọn đúng chỉ số giúp Ban giám hiệu quyết định | **4**/5 | 4 bảng, mỗi bảng trả lời một câu hỏi khác nhau · bảng 5.3 gộp bốn chỉ số vào một khung nên đọc ra ngay 8B đứng cuối cả bốn cột · có ô cảnh báo không cộng. **Trừ 1:** cùng lý do với tiêu chí cấu trúc — chọn lọc chưa đủ tay. |
| Kiểm soát độ tin cậy | Nêu rõ dữ liệu thiếu, giới hạn, không tự tạo thông tin | **5**/5 | 7 hạn chế, mỗi hạn chế chỉ đích danh mục nguồn · bắt được cả bẫy thứ năm thầy không liệt kê · khuyến nghị số 4 **từ chối đánh giá** chương trình phụ đạo thay vì đưa kết luận nửa vời. |
| **Tổng** | | **36**/40 | |

---

## 4. Quét tự động chữ mang nghĩa nhân quả

LUẬT 2 trong câu lệnh liệt kê chữ cấm để quét được bằng máy. Chạy quét trên `report.md`:

```
grep -n -o -E "khiến|dẫn đến|gây ra|nhờ …|tác động|hiệu quả của|do …|vì …" report.md
```

Không có `khiến` / `dẫn đến` / `gây ra` / `nhờ`. Tám lượt còn lại đã soi từng chỗ:

| Dòng | Cụm bị bắt | Giữ hay sửa |
|---|---|---|
| 35 | "không đọc được như **mức hiệu quả của** chương trình phụ đạo" | **Giữ** — đang phủ định chính cách đọc nhân quả |
| 49, 51 | "Nhà trường **tác động** được không" | **Giữ** — nói về khả năng can thiệp của trường, không gán kết quả cho yếu tố |
| 67 | "không tính được **vì** nhóm đối chiếu là…" | **Giữ** — giải thích một quyết định phương pháp |
| 199, 235, 332 | "**vì** dữ liệu không đỡ", "**vì sao**" | **Giữ** — câu meta về chính báo cáo |
| 323, 328 | "**vì** một em có thể nằm ở nhiều môn", "danh sách theo dõi **vì** lỗ hổng kiến thức" | **Giữ** — câu đầu giải thích phép đếm, câu sau thuật lại tiêu chí chọn của `[§9]` |

Không chỗ nào gán kết quả học tập cho một yếu tố. Nếu thầy soi kỹ hơn, hai chỗ đáng bàn nhất là
dòng 49 và 51 — đó là chữ "tác động" duy nhất còn lại, và em giữ vì nó mô tả **quyền can thiệp của
nhà trường**, không phải một quan hệ đã được chứng minh.

---

## 5. Cách em kiểm số

Không đọc bằng mắt. Em viết một script đối chiếu **mọi con số dẫn xuất** trong bài với dòng tổng của
tệp nguồn — 42 phép, gồm:

- Tổng theo cột phải khớp dòng "Toàn trường" / "Tổng" của tệp (phân bố thành tích, lượt nghỉ, số HS
  cần hỗ trợ theo lớp, số HS theo bậc tự học, tồn kho, công nợ, doanh thu theo nhóm).
- Mọi tỷ lệ phần trăm em tự tính (73,8% · 26,3% · 28,6% · 68,8% · 37,5% và 8 tỷ lệ chênh lệch của BT3).
- Mọi phép quy đổi tỷ lệ ra số học sinh (15,0% × 40 = 6 · 20,0% × 40 = 8 · 12,5% × 40 = 5).
- Mâu thuẫn 926 vs 797 của `sample-01` — kiểm ở cả hai tháng để chắc chắn nó không phải một khoản
  phân bổ cố định.

Kết quả: **42/42 khớp.** Đây là chỗ em học được từ buổi 3 — cây phân rã chạy trên sản phẩm thật thì
sai ở ba chỗ, và cả ba chỗ đều chỉ lộ ra khi đem số đi đối chiếu chứ không lộ khi đọc lại tài liệu.

---

## 6. Ba điều em rút ra sau buổi 4

1. **Ràng buộc đặt ở phần "nhiệm vụ" thì yếu, đặt ở phần "tự kiểm" mới có hiệu lực.** Cả BT3 lẫn bài
   cuối đều gặp: AI đọc luật chống suy diễn nhân quả từ đầu, rồi vẫn viết trôi theo mạch văn, và chỉ
   bị chặn ở bước quét lại. Nguyên tắc trừu tượng ("không kết luận nhân quả") gật đầu xong vẫn vi
   phạm; danh sách chữ cấm cụ thể thì **quét được bằng máy**.

2. **Cách biểu diễn không phải chuyện trình bày, nó quyết định phát hiện được gì.** Cùng bộ dữ liệu:
   xếp thành bảng thì thấy 8B đứng cuối cả bốn cột; vẽ thành bản đồ khái niệm mới thấy **không can
   thiệp nào trong năm nhắm vào thời lượng tự học** — thứ mà đọc bảng bao nhiêu lần cũng không lộ,
   vì nó là một *chỗ trống*, mà bảng thì không hiển thị chỗ trống.

3. **Chỗ khó nhất của concept map là đặt tên quan hệ, không phải chọn khái niệm.** Ở BT4 em suýt gắn
   nhãn "dẫn tới" cho cạnh Doanh thu → Lợi nhuận, trong khi đó là quan hệ **số học** (`cấu thành`),
   không phải quan hệ nguyên nhân. Mũi tên không tên là chỗ suy diễn nhân quả lọt vào dễ nhất, vì
   người đọc tự điền nghĩa cho nó.
