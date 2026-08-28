# Chẩn đoán performance ASIN — chatbot + thang chấm listing

Bạn upload file export thô từ **Amazon Ads · Seller Central · Helium 10**. Bot định vị nghẽn theo phễu
**Hiển thị → Bấm → Chuyển đổi → Lợi nhuận**, nói rõ *vì sao là tầng đó chứ không phải tầng khác*, và
hướng dẫn bạn tự đặt ngưỡng thay vì áp số của người khác.

Kèm theo là **thang chấm listing 3 tầng** dùng khi bot kết luận nghẽn ở tầng chuyển đổi.

> Đây là bài tập khoá **SID (Structured Intelligence Design)** — nhưng đóng gói để **tải về là dùng được**,
> không phải để đọc cho biết. Tài liệu thiết kế nằm ở [`thiet-ke/`](thiet-ke/).

---

## Cài trong 5 phút

Nền tảng: **Custom GPT** (ChatGPT Plus trở lên). Không cần cài gì trên máy.

| # | Việc | Chi tiết |
|---|---|---|
| 1 | Tạo GPT mới | ChatGPT → *Explore GPTs* → *Create* |
| 2 | Đặt tên | `Chẩn đoán ASIN` |
| 3 | Dán **Instructions** | Toàn bộ nội dung [`master-instruction.md`](master-instruction.md) |
| 4 | Upload **Knowledge** | Cả **6 file** trong [`kb/`](kb/) |
| 5 | Bật **Code Interpreter & Data Analysis** | ⚠️ **Bắt buộc.** Không bật thì bot không đọc được CSV/XLSX — lỗi kinh điển, dễ tưởng nhầm là "GPT không đọc được file" |

Thêm 4 câu mở đầu (mỗi câu đã có nhánh xử lý riêng trong Master Instruction):

```
Chẩn đoán sản phẩm đang tụt
Tôi có file này thì chẩn được đến đâu?
Cách đặt ngưỡng cho sản phẩm của tôi
Bot cần dữ liệu gì?
```

Ô **Description** (dòng hiển thị dưới tên) — bot không đọc ô này, nó chỉ để người dùng đọc:

```
Bạn upload file export từ Amazon Ads, Business Report hoặc Helium 10. Bot định vị nghẽn
theo phễu Hiển thị → Bấm → Chuyển đổi → Lợi nhuận, nói rõ vì sao là tầng đó chứ không
phải tầng khác, và hướng dẫn bạn tự đặt ngưỡng thay vì áp số của người khác.
```

---

## Cấu trúc

```
master-instruction.md        ← dán vào ô Instructions (~4.300 ký tự, trần Custom GPT 8.000)
kb/                          ← upload cả 6 file vào Knowledge
  kb_01_data.md                nhận diện 4 nguồn · ý nghĩa từng cột · bẫy đọc số · lấy file ở đâu
  kb_02_logic.md               3 cổng vào · phễu C1-C2-C3-PL · 6 nhóm nguyên nhân
  kb_03_thresholds.md          hướng dẫn tự đặt ngưỡng (bot KHÔNG đặt hộ)
  kb_04_rules.md               guardrail tiền kiểm — thắng mọi file khác
  kb_05_output.md              giọng văn · mẫu báo cáo · khối bàn giao
  kb_06_checkpoint.md          hậu kiểm 7 mục — bot tự soi báo cáo TRƯỚC khi trả

thang-cham-diem-listing.md   ← dùng tay, khi bot kết luận nghẽn ở tầng chuyển đổi
demo/                        ← 5 trang HTML: chấm thật 4 sản phẩm (mã đã đổi)
thiet-ke/                    ← vì sao nó được xây như vậy (bài nộp khoá SID)
```

---

## Dùng nó ra sao

**Bốn nguồn dữ liệu và trần phủ.** Bot nhận diện file bằng **tập cột**, không bằng tên file — đổi tên
file vẫn chạy.

| Nguồn | Mở ra được gì |
|---|---|
| Ads Campaigns **(bắt buộc)** | Campaign nền · tiền có tiêu được không · cách phân bổ tiền |
| Business Report | Listing và tỉ lệ chuyển đổi |
| H10 Cerebro | Thứ hạng, index, chuẩn bị mùa vụ |
| SP Targeting | Bối cảnh cấu trúc target |

**Trần phủ: 4/6 nhóm nguyên nhân.** Nhóm *giá & vị thế cạnh tranh* và *tồn kho* chưa có nguồn — bot
**luôn phải khai ra**, không được im lặng bỏ qua.

**Ba cổng chặn trước khi chẩn.**

1. **Dữ liệu** — có file Ads chưa · cửa sổ thời gian nào · mẫu có đủ để đọc tỉ lệ không.
2. **Mô hình kinh doanh** — lời trên từng đơn, hay khách mua lại nhiều lần? Nếu mua lại nhiều lần
   (thực phẩm chức năng, đồ tiêu hao) thì bot **dừng, báo ngoài phạm vi**: toàn bộ tri thức nền rút từ
   mô hình lời-trên-đơn, bốn giả định gốc không áp được.
3. **Mức phủ** — in ra, hỏi *chẩn luôn hay bổ sung*, rồi dừng chờ.

---

## Ba luật đáng chú ý nhất

Chúng là lý do bộ này khác một prompt "phân tích giúp tôi":

- **Số nào không kèm tên file nguồn và cửa sổ thời gian thì không in.** Thiếu một trong hai → ghi
  "chưa có dữ liệu" và nêu lấy ở đâu.
- **Đề xuất nào không kèm đối tượng + ngưỡng, chỉ số đo, cửa sổ đo và mốc trước thì chưa phải đề xuất.**
  Cuối mỗi báo cáo bot in một **khối bàn giao** đủ các cột đó để chép vào sổ theo dõi.
- **Chưa có bằng chứng thì để trống, không cho 0.** `0` nghĩa là *đã soi và thấy sai*; trống nghĩa là
  *chưa soi*. Gộp hai thứ là cách nhanh nhất để một bảng trông đầy đủ mà sai.

Bot tự chạy [`kb_06_checkpoint.md`](kb/kb_06_checkpoint.md) trên chính báo cáo của nó trước khi trả, và
in dòng cuối `Tự kiểm: N/7 đạt`.

---

## Xem demo

[`demo/index.html`](demo/index.html) — kết quả chấm **4 sản phẩm thật** bằng thang 3 tầng: 1 trang tổng
quan + 4 trang chi tiết, sidebar bên trái để chuyển.

> ⚠️ **Bấm thẳng file `.html` trên GitHub sẽ ra mã nguồn, không ra trang** — GitHub không render HTML.
> Tải repo về (`Code → Download ZIP`) rồi mở `chan-doan-asin/demo/index.html` bằng trình duyệt.
> File tự chứa toàn bộ CSS và biểu đồ, không cần mạng, không cần cài gì.

Số trong demo là thật (Business Report 30 ngày tới 27/08/2026 · Helium 10 truy 28/08/2026), nhưng vì
repo công khai nên đã đổi mã sản phẩm, để tên ở dạng chung và **bỏ mọi số tuyệt đối** — giữ lại tỉ lệ,
thứ hạng, số biến thể và điểm chấm.

---

## Giới hạn — nói trước cho đỡ mất công

- **Chưa được người ngoài test.** Người dựng tự chạy thì luôn thấy nó ổn.
- **Trọng số thang chấm chưa hiệu chuẩn** — là lựa chọn thiết kế, không phải kết quả đo. Mỗi tiêu chí
  có ghi rõ căn cứ là *đo được* hay *giả định*.
- **Custom GPT không nhớ gì giữa các phiên.** Sổ theo dõi phải nằm ngoài bot.
- **Chỉ đúng cho mô hình lời-trên-từng-đơn.** Sản phẩm bán theo mua lại nhiều lần thì bot từ chối chẩn —
  cố ý, không phải thiếu sót.
