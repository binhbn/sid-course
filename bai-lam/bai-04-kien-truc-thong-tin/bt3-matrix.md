# Bài tập 3 — Bảng hỗ trợ quyết định

**Nguồn dữ liệu:** `bai-04-sample-01.md` (Công ty Minh An Retail, tháng 6/2026)
**Chạy trên:** Claude Opus 5 trong Claude Code — 04/09/2026

---

## 0. Câu phải hoàn thành trước khi viết câu lệnh

> **Nhìn vào bảng này, tôi muốn quyết định** *tháng 7 rót thêm tiền nhập hàng và ngân sách marketing
> vào nhóm sản phẩm nào, ghìm lại ở nhóm nào — trong điều kiện dòng tiền tháng 6 đã âm 43 triệu nên
> không thể rót đều cho cả bốn nhóm.*

Đây là chỗ em phải viết trước, vì nó quyết định luôn hàng và cột. Không có câu này thì cái ra được
là một bảng tổng hợp đẹp mà không dùng để quyết cái gì.

**Vì sao chọn quyết định này:** dữ liệu tháng 6 có một nghịch lý — doanh thu tăng 260 triệu
(3.260 → 3.520) nhưng lợi nhuận trước thuế **giảm** 49 triệu (485 → 436) và dòng tiền thuần âm 43
triệu. Tiền đã cạn thì câu hỏi "rót vào đâu" không còn là câu hỏi tăng trưởng nữa, nó là câu hỏi
phân bổ. Bảng phải trả lời đúng câu đó.

---

## 1. Câu lệnh em tự viết

Khác câu lệnh mẫu §7.5 của thầy ở chỗ: mẫu của thầy cho sẵn danh sách chỉ tiêu và danh sách vai trò
rồi bảo AI điền. Em quy định **ý nghĩa của hàng và cột** (đúng yêu cầu đề bài) nhưng bắt AI tự
**nối hai bảng nguồn** — điều mà cả hai câu lệnh mẫu đều không đụng tới.

```text
### ROLE
Bạn là người chuẩn bị dữ liệu cho một cuộc họp phân bổ ngân sách. Bạn không phải người quyết định,
và bạn không được đề xuất điều gì mà số trong tệp không đỡ nổi.

### TASK
Đọc tệp bai-04-sample-01.md. Dựng MỘT ma trận quyết định để trả lời đúng câu hỏi:
"Tháng 7 rót thêm tiền nhập hàng và marketing vào nhóm sản phẩm nào, ghìm ở nhóm nào?"

### CONTEXT — ý nghĩa của hàng và cột (bắt buộc theo đúng quy định này)
HÀNG = bốn nhóm sản phẩm. Bốn nhóm này xuất hiện ở CẢ §3 (doanh thu) và §8 (tồn kho) với đúng
cùng tên gọi, nên hai bảng đó nối được với nhau theo hàng. Phải nối, không được chỉ dùng một bảng.

CỘT = các chiều để so bốn nhóm với nhau. Mỗi cột phải thuộc đúng một trong ba loại, và phải ghi rõ
nó thuộc loại nào:
  (1) TÍN HIỆU CẦU  — nhóm này đang bán chạy lên hay chậm lại;
  (2) TÍN HIỆU TIỀN — nhóm này đang nhả tiền ra hay giam tiền lại;
  (3) TÍN HIỆU RỦI RO — nhóm này có dấu hiệu gì khiến doanh thu ghi nhận được có thể không thật.

Ràng buộc bắt buộc:
- Phải có ÍT NHẤT một cột so với tháng 5 VÀ ít nhất một cột so với kế hoạch tháng 6.
- Cột cuối cùng là "Tín hiệu quản trị": chuyển các cột số thành MỘT hành động phân bổ cho tháng 7.
  Chỉ được dùng bốn nhãn: RÓT THÊM / GIỮ NGUYÊN / GHÌM VÀ ĐIỀU TRA / DỪNG RÓT THÊM.
- Nếu một chiều cần cho quyết định này mà tệp KHÔNG có dữ liệu, vẫn phải tạo cột đó và ghi
  "Chưa đủ dữ liệu" vào toàn bộ cột. Không được lặng lẽ bỏ cột đi.

### OUTPUT
1. Ma trận Markdown như mô tả trên.
2. Ngay dưới ma trận: bảng "Truy vết phép tính" — mỗi cột dẫn xuất ghi công thức và mục nguồn
   (dạng §3, §8), để người đọc kiểm lại được bằng tay.
3. Mục "Quyết định này chưa chắc ở chỗ nào": nói rõ thiếu dữ liệu gì thì kết luận có thể lật ngược.

### EVALUATION (tự kiểm trước khi trả lời)
1. Cộng lại các cột doanh thu: tổng có khớp với dòng Tổng của §3 không?
2. Mỗi nhãn ở cột "Tín hiệu quản trị" có ít nhất hai cột số đỡ nó không? Nhãn nào chỉ dựa vào
   một cột thì hạ xuống mức thận trọng hơn.
3. Có cột nào bạn đã lặng lẽ bỏ vì thiếu dữ liệu không? Nếu có, thêm lại và ghi "Chưa đủ dữ liệu".
4. Có chỗ nào bạn viết "vì A nên B" mà tệp chỉ cho thấy A và B cùng xảy ra không? Nếu có, đổi thành
   "đi kèm với".
```

---

## 2. Kết quả AI tạo ra

### Ma trận phân bổ ngân sách tháng 7 — theo nhóm sản phẩm

| Nhóm sản phẩm | Doanh thu T6 *(quy mô)* | vs T5 **(cầu)** | vs Kế hoạch T6 **(cầu)** | Số ngày tồn kho **(tiền)** | Vốn giam trong kho **(tiền)** | Tỷ lệ hoàn hàng **(rủi ro)** | Biên lợi nhuận nhóm **(tiền)** | Chi marketing theo nhóm **(tiền)** | **Tín hiệu quản trị** |
|---|---:|---:|---:|---:|---:|---:|---|---|---|
| Chăm sóc cá nhân | 780 | **+80 (+11,4%)** | **+20 (+2,6%)** | **31 ngày** | 210 | **2,4%** | Chưa đủ dữ liệu | Chưa đủ dữ liệu | **RÓT THÊM** |
| Thiết bị gia dụng | 1.260 | +80 (+6,8%) | −40 (−3,1%) | 42 ngày | 480 | 3,2% | Chưa đủ dữ liệu | Chưa đủ dữ liệu | **GIỮ NGUYÊN** |
| Đồ dùng nhà bếp | 940 | +80 (+9,3%) | −40 (−4,1%) | 58 ngày | 390 | **5,8%** | Chưa đủ dữ liệu | Chưa đủ dữ liệu | **GHÌM VÀ ĐIỀU TRA** |
| Phụ kiện | 540 | +20 (+3,8%) | −20 (−3,6%) | **67 ngày** | 260 | 4,1% | Chưa đủ dữ liệu | Chưa đủ dữ liệu | **DỪNG RÓT THÊM** |
| **Tổng** | **3.520** | **+260** | **−80** | — | **1.340** | — | — | — | — |

*(cầu) = tín hiệu cầu · (tiền) = tín hiệu tiền · (rủi ro) = tín hiệu rủi ro. Số in đậm là giá trị
tốt nhất hoặc xấu nhất của cột.*

### Truy vết phép tính

| Cột | Công thức | Nguồn | Kiểm chéo |
|---|---|---|---|
| Doanh thu T6 | lấy thẳng | §3 | 1.260 + 940 + 780 + 540 = 3.520 = dòng Tổng §3 ✓ |
| vs T5 | `T6 − T5`, `%` = chênh ÷ T5 | §3 | tổng chênh 260 = 3.520 − 3.260 ✓ |
| vs Kế hoạch T6 | `T6 − KH`, `%` = chênh ÷ KH | §3 | tổng chênh −80 = 3.520 − 3.600 ✓ |
| Số ngày tồn kho | lấy thẳng | §8 | — |
| Vốn giam trong kho | lấy thẳng cột Giá trị tồn kho | §8 | 480 + 390 + 210 + 260 = 1.340 = dòng Tổng §8 ✓ |
| Tỷ lệ hoàn hàng | lấy thẳng | §3 | — |
| Biên lợi nhuận nhóm | không tính được | — | §2 chỉ có giá vốn **tổng** (2.096), không tách theo nhóm |
| Chi marketing theo nhóm | không tính được | — | §5 chỉ có marketing **tổng** (188), không tách theo nhóm |

### Bốn tín hiệu dựa trên cái gì

| Nhóm | Nhãn | Hai cột đỡ nhãn đó |
|---|---|---|
| Chăm sóc cá nhân | RÓT THÊM | Nhóm **duy nhất vượt kế hoạch** (+2,6%) · quay vòng nhanh nhất (31 ngày) · hoàn hàng thấp nhất (2,4%) — cả ba chiều cùng chiều tốt |
| Thiết bị gia dụng | GIỮ NGUYÊN | Quy mô lớn nhất (36% doanh thu) và vẫn tăng, nhưng hụt kế hoạch và tồn 42 ngày — §8 xếp "Theo dõi". Không có tín hiệu nào đủ mạnh để đổi hướng |
| Đồ dùng nhà bếp | GHÌM VÀ ĐIỀU TRA | Tăng 9,3% *đi kèm với* tỷ lệ hoàn hàng cao nhất (5,8%) và tồn 58 ngày. §9 ghi rõ hàng hoàn nhóm này **chưa có phân tích nguyên nhân** — rót thêm khi chưa biết vì sao khách trả hàng là rót vào chỗ chưa hiểu |
| Phụ kiện | DỪNG RÓT THÊM | Tăng chậm nhất (+3,8%), hụt kế hoạch, và tồn **67 ngày — chậm nhất**, §8 xếp "Chậm luân chuyển". 260 triệu đang nằm im trong khi công ty âm dòng tiền |

### Quyết định này chưa chắc ở chỗ nào

Hai cột "Chưa đủ dữ liệu" không phải chi tiết phụ, chúng có thể lật ngược cả bảng:

1. **Không có biên lợi nhuận theo nhóm.** Toàn bộ xếp hạng ở trên đang đo *tốc độ tiền quay lại*,
   không đo *tiền lãi bao nhiêu*. Nếu Phụ kiện có biên cao gấp đôi Chăm sóc cá nhân thì 67 ngày tồn
   vẫn có thể đáng, và nhãn DỪNG RÓT THÊM là sai. Cần bảng giá vốn tách theo nhóm mới chốt được.
2. **Không có chi marketing theo nhóm.** Marketing tháng 6 vượt kế hoạch 28 triệu (§5, §9 xác nhận)
   nhưng không biết 28 triệu đó đổ vào nhóm nào. Có thể chính nhóm Chăm sóc cá nhân vượt kế hoạch
   là nhờ khoản chi thêm đó — nếu vậy thì "vượt kế hoạch" là kết quả đã mua bằng tiền, không phải
   cầu tự nhiên, và mức RÓT THÊM phải xem lại.
3. **Tỷ lệ hoàn hàng chỉ có của tháng 6.** Không biết 5,8% của Đồ dùng nhà bếp là mới xấu đi hay
   vốn vẫn thế. Đây là lý do nhãn là GHÌM VÀ ĐIỀU TRA chứ không phải DỪNG.

---

## 3. Tự đánh giá

1. **Đáp ứng đủ năm tiêu chí bắt buộc của đề.** Ý nghĩa hàng/cột được quy định ngay trong câu lệnh
   (khối CONTEXT, ba loại tín hiệu); có cả so với tháng 5 lẫn so với kế hoạch; có cột "Tín hiệu quản
   trị" chuyển số thành hành động; hai cột thiếu dữ liệu ghi "Chưa đủ dữ liệu"; mọi phép tính có
   bảng truy vết về §3 và §8.

2. **Chỗ có giá trị nhất là ràng buộc "phải nối §3 với §8".** Bốn nhóm sản phẩm xuất hiện ở hai bảng
   khác nhau với đúng cùng tên gọi, nhưng không câu lệnh mẫu nào trong bài giảng nối chúng lại. Nối
   xong mới lộ ra điều mà đọc riêng từng bảng không thấy: **Phụ kiện vừa tăng chậm nhất vừa giam vốn
   lâu nhất**, còn **Chăm sóc cá nhân tốt đều cả ba chiều**. Đây mới là thứ đỡ được một quyết định
   phân bổ.

3. **Chỗ còn yếu: ép chỉ dùng bốn nhãn là hơi thô.** Đồ dùng nhà bếp và Phụ kiện đang bị xếp vào
   hai nhãn khác nhau, nhưng lý do thì khác hẳn nhau về bản chất — một bên là *chưa hiểu* (hoàn hàng
   cao chưa rõ nguyên nhân), một bên là *đã hiểu và thấy chậm* (tồn 67 ngày). Bốn nhãn không tải nổi
   sự khác biệt đó, phải viết thêm cả một bảng giải thích bên dưới mới đủ. Sửa câu lệnh: tách cột
   tín hiệu thành hai — *hành động* và *độ chắc chắn của hành động*.

4. **Chỗ suýt sai: quan hệ nhân quả.** Bản chạy đầu tiên viết "tỷ lệ hoàn hàng cao **làm** doanh thu
   nhóm Đồ dùng nhà bếp không đạt kế hoạch". Tệp không hề đỡ câu đó — nó chỉ cho thấy hai chuyện cùng
   xảy ra ở một nhóm. Mục EVALUATION số 4 trong câu lệnh bắt được và đổi thành "đi kèm với". Bài học:
   dòng chống suy diễn nhân quả phải nằm trong **phần tự kiểm** của prompt, chứ để ở phần mô tả nhiệm
   vụ thì AI đọc xong rồi vẫn viết trôi theo mạch văn.

5. **Còn thiếu: bảng không nói được rót thêm *bao nhiêu*.** Nó xếp hạng được thứ tự ưu tiên nhưng
   không ra được con số ngân sách, vì thiếu biên lợi nhuận theo nhóm. Đúng phạm vi dữ liệu cho phép
   thì dừng ở đây là đúng — nhưng phải nói thẳng với người đọc rằng bảng này trả lời "ưu tiên ai
   trước", không trả lời "chia bao nhiêu", kẻo họ cầm bảng đi chia tiền.
