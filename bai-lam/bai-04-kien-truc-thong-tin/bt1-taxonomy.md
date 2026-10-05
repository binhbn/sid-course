# Bài tập 1 — Taxonomy

**Nguồn dữ liệu:** `SID-course-v2/workshop/bai-04-information-architect/bai-04-sample-01.md`
(báo cáo kinh doanh tháng 6/2026, Công ty Minh An Retail)
**Chạy trên:** Claude Opus 5 trong Claude Code — 04/09/2026

---

## 1. Câu lệnh em tự viết

Em viết theo khung RTC-COE của buổi 02, và cố ý **không dùng lại bộ nhóm mẫu của thầy**
(Doanh thu / Chi phí / Tiền / Công nợ / Tồn kho / Kiểm soát dữ liệu). Bộ đó phân loại theo *vai trò
kế toán* của dữ liệu. Em đổi sang tiêu chí *câu hỏi quản trị mà bảng đó trả lời* — lý do ở mục 3.

```text
### ROLE
Bạn là kiến trúc sư thông tin, chuyên chuẩn bị dữ liệu thô trước khi dựng báo cáo quản trị.
Bạn không phải kế toán và không được tự hoà giải số liệu mâu thuẫn.

### TASK
Phân loại TOÀN BỘ 8 bảng dữ liệu và 1 danh sách trong tệp bai-04-sample-01.md vào một hệ phân loại
phẳng, theo ĐÚNG MỘT tiêu chí duy nhất:

    "Bảng này trả lời câu hỏi quản trị nào của người điều hành?"

Không dùng bất kỳ tiêu chí nào khác (không phân theo nguồn chứng từ, không phân theo vai trò kế
toán, không phân theo mức nhạy cảm). Nếu bạn thấy mình đang dùng tiêu chí thứ hai để tách nhóm,
dừng lại và gộp về tiêu chí trên.

### CONTEXT
- Kỳ báo cáo: tháng 6/2026. Đơn vị: triệu đồng.
- Người đọc cuối: CEO và kế toán trưởng.
- Hệ phân loại này dùng để quyết định mỗi bảng sẽ chảy vào phần nào của báo cáo, TRƯỚC khi bắt đầu
  tính toán bất kỳ con số nào.

### OUTPUT
Một bảng Markdown duy nhất, mỗi dòng là một bảng dữ liệu nguồn, đúng các cột theo thứ tự:

| Bảng nguồn | Nhóm | Lý do phân loại | Phần báo cáo sử dụng | Trạng thái dữ liệu |

Ràng buộc:
- Số nhóm: 4 đến 6. Các nhóm phải đồng đẳng — đọc tên nhóm lên phải thấy chúng cùng là "một câu
  hỏi quản trị", không được có nhóm nào là "một loại chứng từ".
- Mỗi bảng nguồn chỉ được gán MỘT nhóm chính.
- Cột "Lý do phân loại": đúng một câu, phải nêu được câu hỏi quản trị mà bảng đó trả lời.
- Cột "Trạng thái dữ liệu": chỉ được nhận một trong ba giá trị — SẴN SÀNG / CẦN XÁC MINH /
  CHƯA ĐỦ DỮ LIỆU.

Sau bảng chính, thêm mục "Trường hợp mơ hồ": liệt kê những CỘT (không phải bảng) mà bạn thấy nằm
vắt giữa hai nhóm. Với mỗi trường hợp, ghi rõ nó vắt giữa hai nhóm nào và vì sao bạn KHÔNG ép nó
vào một nhóm.

### EVALUATION (tự kiểm trước khi trả lời)
1. Đọc to tên 4-6 nhóm: có nhóm nào lạc tiêu chí không? Nếu có, sửa rồi mới trả lời.
2. Có bảng nguồn nào bị bỏ sót hoặc bị gán hai nhóm không?
3. Có con số nào trong tệp mâu thuẫn với con số khác trong chính tệp đó không? Nếu có, đánh
   CẦN XÁC MINH và ghi rõ hai con số đang chọi nhau là con số nào, chênh bao nhiêu.
   TUYỆT ĐỐI không tự chọn một trong hai con số làm số đúng.
4. Không bổ sung bất kỳ dữ liệu nào ngoài tệp.
```

---

## 2. Kết quả AI tạo ra

**Tiêu chí phân loại:** *câu hỏi quản trị mà bảng đó trả lời*.
**Sáu nhóm:** A. Kết quả kỳ · B. Nguồn tăng trưởng · C. Hiệu quả chi tiêu · D. Thanh khoản ·
E. Vốn đang bị giam · F. Kiểm soát độ tin cậy.

| Bảng nguồn | Nhóm | Lý do phân loại | Phần báo cáo sử dụng | Trạng thái dữ liệu |
|---|---|---|---|---|
| §2. Báo cáo kết quả kinh doanh | **A. Kết quả kỳ** | Bảng duy nhất trả lời "tháng này lãi bao nhiêu, so với tháng trước và so với kế hoạch thì hơn kém thế nào" | Tóm tắt điều hành · Phần I Kết quả kỳ | **CẦN XÁC MINH** |
| §3. Doanh thu theo nhóm sản phẩm | **B. Nguồn tăng trưởng** | Trả lời "phần doanh thu tăng thêm đến từ dòng hàng nào, dòng nào hụt kế hoạch" | Phần II Phân tích doanh thu | SẴN SÀNG |
| §4. Doanh thu theo kênh bán | **B. Nguồn tăng trưởng** | Cùng câu hỏi với §3 nhưng cắt theo chiều kênh: "kênh nào kéo tăng trưởng, kênh nào đang tụt" | Phần II Phân tích doanh thu | SẴN SÀNG |
| §5. Chi phí vận hành | **C. Hiệu quả chi tiêu** | Trả lời "tiêu vượt kế hoạch ở khoản nào, khoản nào là chi một lần không lặp lại" | Phần III Phân tích chi phí | **CẦN XÁC MINH** |
| §6. Dòng tiền | **D. Thanh khoản** | Trả lời "có tiền thật để chi tháng sau không" — khác hẳn câu hỏi lãi/lỗ của nhóm A | Phần IV Dòng tiền | SẴN SÀNG |
| §7. Công nợ phải thu cuối tháng 6 | **E. Vốn đang bị giam** | Trả lời "tiền của mình đang kẹt ở khách hàng bao nhiêu và kẹt bao lâu rồi" | Phần V Công nợ và rủi ro | SẴN SÀNG |
| §8. Tồn kho cuối tháng 6 | **E. Vốn đang bị giam** | Cùng câu hỏi với §7, chỉ khác chỗ kẹt: tiền kẹt ở hàng chưa bán được | Phần V Công nợ và rủi ro | SẴN SÀNG |
| §9. Các điểm cần xác minh | **F. Kiểm soát độ tin cậy** | Không phải số để tính; trả lời "chỗ nào trong báo cáo này chưa được phép kết luận" | Phần VII Hạn chế dữ liệu | SẴN SÀNG (5 mục) |

### Chi tiết hai dòng CẦN XÁC MINH

**§2 và §5 chọi nhau về tổng chi phí vận hành.**

| Nguồn | Cách tính | Tháng 5 | Tháng 6 |
|---|---|---:|---:|
| §2 | Chi phí bán hàng + Chi phí quản lý | 410 + 320 = **730** | 455 + 342 = **797** |
| §5 | Dòng "Tổng" của bảng chi phí vận hành | **830** | **926** |
| | **Chênh** | **100** | **129** |

Chú thích ngay dưới §5 ghi *"Tổng chi phí vận hành gồm chi phí bán hàng và chi phí quản lý; không
gồm giá vốn và lãi vay"* — tức hai con số này lẽ ra phải bằng nhau. Chúng không bằng nhau ở cả hai
tháng, và mức chênh cũng không cố định (100 rồi 129), nên không giải thích được bằng một khoản phân
bổ đứng yên.

Không chọn số nào làm số đúng. Mọi phép tính chi phí trong báo cáo phải khai rõ đang lấy từ §2 hay §5.

**§5 còn một điểm mờ thứ hai:** khoản 18 triệu sửa kho nằm trong dòng "Chi phí khác" (71 triệu tháng
6) nhưng chưa xác nhận là chi một lần hay định kỳ — §9 cũng nêu lại. Chưa gỡ được điểm này thì không
được kết luận chi phí khác "tăng bất thường".

### Trường hợp mơ hồ — đánh dấu, không ép nhóm

| Cột | Vắt giữa | Vì sao không ép |
|---|---|---|
| §3 · *Tỷ lệ hoàn hàng tháng 6* | **B. Nguồn tăng trưởng** ↔ **F. Kiểm soát độ tin cậy** | Nó nằm trong bảng doanh thu nhưng không trả lời câu hỏi "tăng đến từ đâu". Nó là tín hiệu chất lượng sản phẩm/vận hành — và §9 đã gọi tên riêng chuyện hàng hoàn nhóm Đồ dùng nhà bếp (5,8%). Ép vào B là giấu mất một cảnh báo. |
| §8 · *Số ngày tồn kho* | **E. Vốn đang bị giam** ↔ **C. Hiệu quả chi tiêu** | Giá trị tồn kho là tiền bị giam (E), nhưng số ngày tồn kho là thước đo hiệu quả vận hành (C). Hai cột cùng bảng nhưng trả lời hai câu hỏi khác nhau. |
| §5 · cột *Ghi chú* | **C. Hiệu quả chi tiêu** ↔ **F. Kiểm soát độ tin cậy** | Phần lớn ghi chú giải thích nguyên nhân chi (thuộc C), riêng "Có 18 triệu sửa kho" là một nghi vấn cần xác minh (thuộc F). |
| §6 · *Mua thiết bị −260* | **D. Thanh khoản** ↔ **C. Hiệu quả chi tiêu** | Đây là khoản chi lớn nhất kéo dòng tiền tháng 6 xuống âm, nhưng nó là chi đầu tư, không nằm trong bảng chi phí vận hành §5. Tính nó vào C là trộn hai loại chi. |

---

## 3. Tự đánh giá

1. **Đáp ứng được yêu cầu "một tiêu chí duy nhất".** Sáu tên nhóm đọc lên đều là một câu hỏi quản
   trị, không có nhóm nào tụt về "loại chứng từ". Chỗ dễ trượt nhất là §7 và §8 — theo phản xạ kế
   toán thì đó là hai khoản mục khác nhau (công nợ · tồn kho), nhưng theo tiêu chí đang dùng thì
   chúng trả lời **cùng một câu hỏi**: tiền đang bị giam ở đâu. Gộp chung là đúng tiêu chí.

2. **Đổi tiêu chí so với bộ mẫu của thầy là có lý do, không phải khác cho khác.** Bộ mẫu phân theo
   vai trò kế toán, hợp khi mục tiêu là *rót dữ liệu vào đúng ô báo cáo*. Nhưng bài tập 3 của buổi
   này đòi bảng phải dẫn tới **một quyết định quản trị cụ thể**. Phân theo câu hỏi quản trị ngay từ
   BT1 thì BT2 (mục lục cho CEO) và BT3 (bảng ra quyết định) kế thừa được luôn, không phải xếp lại
   từ đầu.

3. **Còn thiếu: câu lệnh chưa buộc AI xếp thứ tự ưu tiên giữa các nhóm.** Kết quả trả về sáu nhóm
   ngang hàng nhau, nhưng khi dựng báo cáo thì nhóm A và D phải lên trước nhóm E. Bảng hiện tại
   không nói được điều đó. Sửa câu lệnh: thêm cột *"Thứ tự xuất hiện trong báo cáo"*, hoặc bắt AI
   xếp các nhóm theo mức khẩn trước khi kẻ bảng.

4. **Còn thiếu: ràng buộc "một bảng một nhóm" làm mất thông tin ở cấp cột.** Cả bốn trường hợp mơ
   hồ đều là *cột* nằm lệch nhóm của *bảng* chứa nó. Phải thêm hẳn một mục phụ mới cứu được, nghĩa
   là ràng buộc trong câu lệnh hơi thô. Sửa: cho phép đơn vị phân loại là *bảng hoặc cột*, và chỉ
   bắt "một nhóm chính" ở cấp đơn vị đã chọn.

5. **Phần chạy đúng nhất là mục EVALUATION số 3 trong câu lệnh.** Nếu không có dòng bắt AI dò số
   chọi số **ngay trong bước phân loại**, mâu thuẫn 926 vs 797 sẽ lọt tới tận lúc viết báo cáo mới
   lộ — lúc đó các phép tính lợi nhuận đã dựa lên nó rồi. Bắt kiểm tính nhất quán ở bước xếp loại,
   tức là trước khi tính, là chỗ đáng giữ lại cho mọi bài sau.
