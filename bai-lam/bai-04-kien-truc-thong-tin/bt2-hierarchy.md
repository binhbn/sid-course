# Bài tập 2 — Hierarchy

**Nguồn dữ liệu:** `bai-04-sample-01.md` (Công ty Minh An Retail, tháng 6/2026)
**Người đọc em chọn:** **Tổng giám đốc**
**Chạy trên:** Claude Opus 5 trong Claude Code — 04/09/2026

---

## 1. Câu lệnh em tự viết

Khác biệt cố ý so với câu lệnh mẫu §6.3 của thầy: câu lệnh mẫu **cho sẵn tên 5 mục cấp I**. Làm vậy
thì AI chỉ còn việc điền cấp II — phần khó nhất (chọn tiêu chí chia cấp I) đã bị người viết prompt
làm hộ. Em bắt AI **suy cấp I ra từ hệ phân loại của BT1**, rồi tự bảo vệ thứ tự sắp xếp.

```text
### ROLE
Bạn là kiến trúc sư thông tin. Việc của bạn là dựng SƯỜN báo cáo, không phải viết báo cáo.

### TASK
Từ tệp bai-04-sample-01.md và hệ phân loại đã chốt ở bước trước (sáu nhóm: A. Kết quả kỳ,
B. Nguồn tăng trưởng, C. Hiệu quả chi tiêu, D. Thanh khoản, E. Vốn đang bị giam,
F. Kiểm soát độ tin cậy), dựng mục lục 3 tầng cho báo cáo quản trị tháng 6/2026.

Không tự đặt trước tên các mục cấp I. Phải suy chúng ra từ sáu nhóm trên: gộp hoặc tách nhóm nếu
cần, nhưng phải nói được mỗi mục cấp I gánh nhóm nào.

### CONTEXT
Người đọc: TỔNG GIÁM ĐỐC, không phải kế toán.
Ba đặc điểm của người đọc này chi phối thứ tự các phần:
- đọc để RA QUYẾT ĐỊNH, không đọc để đối chiếu sổ sách;
- thường chỉ đọc hết mục cấp I, cấp II đọc lướt, cấp III chỉ mở khi bị nghi ngờ;
- câu hỏi sống còn của người này là "công ty có đủ tiền chạy tiếp không", câu hỏi đó KHÔNG
  đồng nghĩa với "tháng này lãi hay lỗ".

### OUTPUT
Mục lục Markdown đánh số I / I.1 / I.1.1. Ràng buộc:
- 4 đến 6 mục cấp I.
- MỖI mục cấp I có TỐI THIỂU 2 mục cấp II.
- Chỉ mở cấp III ở chỗ thật sự cần tách; không mở đều cho đẹp.
- Sau mỗi mục cấp II và cấp III, ghi trong ngoặc vuông tên bảng nguồn trong tệp mẫu,
  dạng [§3] hoặc [§2, §5]. Mục nào không có bảng nguồn trực tiếp thì ghi [tổng hợp].
- TUYỆT ĐỐI không viết số liệu, không viết câu phân tích, không viết kết luận.
  Chỉ có tiêu đề mục và nguồn.

Sau mục lục, thêm hai phần ngắn:
(a) "Vì sao thứ tự này" — mỗi mục cấp I một dòng, nói rõ vì sao nó đứng ở vị trí đó
    ĐỐI VỚI TỔNG GIÁM ĐỐC.
(b) "Sáu nhóm đi đâu" — bảng đối chiếu nhóm A-F sang mục cấp I, để kiểm tra không nhóm nào
    rơi mất và không nhóm nào bị xé ra hai chỗ.

### EVALUATION (tự kiểm trước khi trả lời)
1. Có mục cấp I nào chỉ có 1 mục cấp II không? Nếu có, sửa.
2. Có nội dung nào xuất hiện ở hai chỗ khác nhau trong mục lục không?
3. Đọc riêng danh sách các mục cấp I: người đọc có nắm được bức tranh lớn mà không cần
   mở cấp II không?
4. Có lọt số liệu hay câu phân tích nào vào mục lục không? Nếu có, xoá.
```

---

## 2. Kết quả AI tạo ra

### Mục lục 3 tầng — báo cáo quản trị tháng 6/2026, người đọc: Tổng giám đốc

```markdown
I. Tóm tắt điều hành và việc cần quyết
  I.1. Bức tranh tháng 6 trong ba dòng  [tổng hợp]
  I.2. Những chỗ số liệu chưa khớp và nó chặn kết luận nào  [§2, §5, §9]
  I.3. Việc cần Tổng giám đốc quyết trong tháng 7  [tổng hợp]

II. Thanh khoản — công ty có đủ tiền chạy tháng tới không
  II.1. Dòng tiền thuần và số dư cuối kỳ  [§6]
  II.2. Các khoản kéo dòng tiền xuống  [§6]
    II.2.1. Chi cho nhà cung cấp  [§6]
    II.2.2. Chi lương và vận hành  [§6]
    II.2.3. Chi mua thiết bị  [§6]
  II.3. Tiền đã bán nhưng chưa thu về  [§7]
    II.3.1. Cơ cấu tuổi nợ  [§7]
    II.3.2. Nhóm nợ quá hạn trên 60 ngày  [§7, §9]

III. Kết quả kỳ — lãi thật là bao nhiêu và hụt kế hoạch ở đâu
  III.1. Doanh thu, lợi nhuận gộp, lợi nhuận trước thuế  [§2]
  III.2. Khoảng cách giữa thực tế và kế hoạch  [§2]
  III.3. Cảnh báo: hai cách tính chi phí đang cho hai kết quả  [§2, §5]

IV. Tăng trưởng đến từ đâu và tiền rò ở đâu
  IV.1. Doanh thu theo nhóm sản phẩm  [§3]
    IV.1.1. Nhóm kéo tăng trưởng và nhóm hụt kế hoạch  [§3]
    IV.1.2. Tỷ lệ hoàn hàng theo nhóm  [§3, §9]
  IV.2. Doanh thu theo kênh bán  [§4]
    IV.2.1. Dịch chuyển tỷ trọng giữa các kênh  [§4]
    IV.2.2. Giá trị đơn trung bình theo kênh  [§4]
  IV.3. Chi phí vận hành so với kế hoạch  [§5]
    IV.3.1. Các khoản vượt kế hoạch  [§5]
    IV.3.2. Khoản chi chưa rõ một lần hay định kỳ  [§5, §9]
  IV.4. Vốn nằm trong tồn kho  [§8]

V. Phụ lục kiểm soát
  V.1. Nguồn dữ liệu của từng mục  [§2-§8]
  V.2. Các điểm chưa xác minh  [§9]
  V.3. Những điều báo cáo này chưa kết luận được  [tổng hợp]
```

**5 mục cấp I · 15 mục cấp II · 11 mục cấp III · mọi mục cấp I đều có ≥ 2 mục cấp II.**

### (a) Vì sao thứ tự này — đối với Tổng giám đốc

| Mục | Vì sao đứng ở đây |
|---|---|
| **I. Tóm tắt điều hành** | Người này có thể chỉ đọc đúng mục I. Nếu mục I không đủ để quyết thì cả báo cáo hỏng. Đặt luôn "việc cần quyết" ở I.3 chứ không giấu xuống cuối. |
| **II. Thanh khoản** | Đặt **trước** lãi lỗ, khác mạch kế toán thông thường. Với người điều hành, hết tiền là chết ngay còn lỗ một tháng thì chưa. Tháng 6 dòng tiền thuần âm trong khi doanh thu vẫn tăng — đúng loại nghịch lý phải nói sớm, để dưới mục III thì nó bị chôn. |
| **III. Kết quả kỳ** | Sau khi biết còn tiền hay không mới hỏi tới lãi bao nhiêu. Cảnh báo hai cách tính chi phí (III.3) phải nằm ngay đây, cạnh con số lợi nhuận mà nó ảnh hưởng, không đẩy xuống phụ lục. |
| **IV. Tăng trưởng và chỗ rò** | Đây là phần *giải thích* cho mục III. Người đọc chỉ mở khi muốn biết vì sao. Bốn mục cấp II của nó là bốn chiều bóc: sản phẩm, kênh, chi phí, tồn kho. |
| **V. Phụ lục kiểm soát** | Không phục vụ quyết định, phục vụ việc truy ngược. Để cuối. |

### (b) Sáu nhóm của BT1 đi đâu

| Nhóm (BT1) | Vào mục | Ghi chú |
|---|---|---|
| A. Kết quả kỳ | III | Nguyên vẹn |
| B. Nguồn tăng trưởng | IV.1, IV.2 | Nguyên vẹn |
| C. Hiệu quả chi tiêu | IV.3 | Nguyên vẹn |
| D. Thanh khoản | II | Được **đẩy lên vị trí 2** so với trật tự kế toán |
| E. Vốn đang bị giam | II.3 (công nợ) và IV.4 (tồn kho) | **Nhóm duy nhất bị tách đôi** — lý do ở mục 3 |
| F. Kiểm soát độ tin cậy | I.2, III.3, V | Cố ý xuất hiện ba chỗ: cảnh báo ở đầu, chi tiết cạnh chỗ bị ảnh hưởng, lưu vết ở cuối |

---

## 3. Tự đánh giá

1. **Đạt yêu cầu định lượng.** 5 mục cấp I (trong dải 4–6), mục cấp I ít cấp II nhất là mục V với 3
   mục — vẫn trên ngưỡng 2. Mọi mục cấp II và cấp III đều có nguồn trong ngoặc vuông. Mục lục không
   lọt con số nào.

2. **Chỗ vi phạm chính nguyên tắc "mỗi nội dung một vị trí": nhóm E bị tách đôi.** Công nợ nằm ở
   II.3, tồn kho nằm ở IV.4, trong khi BT1 xếp cả hai vào cùng một nhóm "vốn đang bị giam". Em cân
   nhắc rồi vẫn tách, vì với Tổng giám đốc thì công nợ là *tiền sắp về hay không về* — thuộc câu
   chuyện thanh khoản; còn tồn kho là *hàng bán chậm* — thuộc câu chuyện vận hành. Nhưng phải nói
   thẳng: đây là chỗ mục lục **lệch khỏi taxonomy**, và nếu đọc riêng mục lục thì không ai biết hai
   mục đó vốn cùng một nhóm. Cách sửa sạch hơn là gộp thành một mục cấp I "Vốn bị giam" — đổi lại
   mục II mất một chân.

3. **Còn thiếu trong câu lệnh: chưa ràng buộc độ dài dự kiến từng mục.** Mục lục hiện có 31 đầu mục.
   Nếu bung ra viết thì Tổng giám đốc — người mà chính câu lệnh mô tả là "chỉ đọc hết mục cấp I" —
   sẽ nhận một tài liệu quá dài so với cách đọc của họ. Sửa: thêm ràng buộc *"mỗi mục cấp II ghi
   kèm số dòng dự kiến, tổng toàn báo cáo không quá N dòng"*, để sườn tự ép mình gọn lại.

4. **Trả lời câu tự kiểm của thầy — đổi người đọc sang kế toán thì đổi gì?** Ba chỗ đổi, không phải
   đổi chữ mà đổi cấu trúc:
   - **Thứ tự lật lại.** Kế toán đọc theo mạch chứng từ → sổ → báo cáo, nên mục III (kết quả kỳ)
     phải lên vị trí 2 và mục II (thanh khoản) lùi xuống, vì dòng tiền với họ là *kết quả của* các
     bút toán chứ không phải câu hỏi mở đầu.
   - **Phụ lục kiểm soát đổi vai.** Với Tổng giám đốc nó là mục V để cuối, đọc khi nghi ngờ. Với kế
     toán nó là **vùng làm việc chính** — phải lên cấp I ở vị trí cao, và phải bung thêm cấp III cho
     từng phép tính (công thức, kỳ lấy số, người nhập).
   - **Mục I biến mất hoặc teo lại.** "Việc cần quyết" không phải việc của kế toán. Thay bằng
     "Những chênh lệch cần đối chiếu trước khi khoá sổ" — cùng dữ liệu, khác hẳn tiêu đề và khác
     hẳn mục đích.

5. **Điểm mạnh nhất của câu lệnh là khối CONTEXT mô tả *cách đọc* chứ không mô tả *chức danh*.**
   Nếu chỉ viết "người đọc là Tổng giám đốc", AI sẽ suy diễn theo khuôn mẫu chung. Ba gạch đầu dòng
   về hành vi đọc (chỉ đọc cấp I, đọc để quyết, sợ hết tiền hơn sợ lỗ) mới là thứ ép được mục II
   lên trước mục III — chỗ khác biệt thật sự giữa hai bản mục lục.
