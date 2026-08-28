# Chatbot chẩn đoán hiệu suất ASIN — Bài tập Buổi 3 SID

**Học viên:** Bùi Ngọc Bình · **Nền tảng:** Custom GPT · **Bộ kiến thức:** v2.2 (28/08/2026)
**Project xuyên khoá:** *Chẩn đoán performance ASIN — gồm phân tích ads và CVR.*
Buổi 1 Framing → Buổi 2 Prompt Stack → Buổi 3 chatbot → Buổi 4 decomposition nốt CVR
(`../bai-04/decomposition-listing-cvr.md`).

Chatbot nhận file export thô từ Amazon Ads / Seller Central / Helium 10, định vị nghẽn theo phễu
**C1 → C2 → C3 → PL**, chỉ ra nguyên nhân và đề xuất việc tiếp theo.

---

## Hai bot, ba file dùng chung

Cùng một bài toán nhưng **hai đối tượng khác nhau**, nên tách hai bot thay vì một bot cố phục vụ cả
hai. Lý do: đổi audience thì đổi luôn task type và đổi luôn artifact — đúng bài học Buổi 1.

| | Bản EGW *(bản nộp)* | Bản nội bộ |
|---|---|---|
| Người dùng | Seller tự vận hành | Đội vận hành chuyên nghiệp |
| Task type | `EXPLAIN` — giải thích cơ chế để tự soi được | `DIAGNOSE` — kết luận nhanh |
| Ngưỡng | **Người dùng tự đặt**, bot hướng dẫn cách tính điểm hoà vốn | Có sẵn bảng ngưỡng của tổ chức |
| Mỗi đề xuất | Kèm một câu "vì sao" | Chỉ nêu việc |
| Thuật ngữ | Giải thích lần đầu trong phiên | Dùng trơn |

```
kb/                       ← DÙNG CHUNG cho cả hai bot
  kb_01_data.md             nhận diện 4 nguồn · ý nghĩa cột · bẫy đọc số · lấy file ở đâu
  kb_02_logic.md            3 cổng vào · phạm vi · phễu C1-C2-C3-PL · 6 nhóm nguyên nhân
  kb_04_rules.md            guardrail (tiền kiểm) — thắng mọi file khác
  kb_06_checkpoint.md       hậu kiểm 7 mục — bot tự soi báo cáo trước khi trả (thêm 28/08)

egw/                      ← BẢN NỘP THẦY
  master-instruction.md     4.336 ký tự (trần Custom GPT 8.000)
  kb_03_thresholds.md       hướng dẫn tự đặt ngưỡng
  kb_05_output.md           giọng văn + mẫu báo cáo cho seller tự vận hành
```

Bản nội bộ nằm ngoài repo, cùng cấu trúc, khác đúng ba file: `master-instruction`, `kb_03`, `kb_05`,
cộng một phụ lục ký hiệu riêng của tổ chức.

**Vì sao tách được sạch:** bốn file dùng chung **không chứa một con số ngưỡng nào**. Toàn bộ số nằm ở
`kb_03`, toàn bộ giọng nằm ở `kb_05`. Ranh giới đặt theo *cái gì thay đổi khi đổi người dùng*.

**Khối BÀN GIAO (28/08).** Báo cáo kết thúc bằng một bảng copy được vào sổ theo dõi: mỗi việc kèm
đối tượng + ngưỡng · cái gì kích hoạt · bằng chứng · chỉ số đo · cửa sổ đo · mốc trước khi đổi. Lý do
thêm: không có khối này thì không đo được việc đã làm có tác dụng không, và **không có dữ liệu để sửa
bộ kiến thức** — người dùng bỏ một việc mà không ai biết vì sao. Dòng `BỎ — lý do` được coi là dữ liệu
quý nhất, không phải điểm trừ; khối này **không dùng để chấm người**.

**Bốn thành phần thầy yêu cầu (buổi 4)** — KB ✅ · rule tiền kiểm ✅ `kb_04` · **checkpoint hậu kiểm**
✅ `kb_06` (thiếu trước 28/08) · Master ✅. Sau buổi 4 cũng viết lại các luật kiểu "quy tắc chung
của AI" ("không bịa số", "không hứa kết quả") thành luật nghiệp vụ: *số nào không kèm tên file + cửa
sổ thì không in*, *đề xuất nào không kèm chỉ số đo + cửa sổ đo thì chưa phải đề xuất*.

## Kiến trúc IPO

```
INPUT    upload file export  →  nhận diện bằng TẬP CỘT, không bằng tên file
PROCESS  master-instruction  →  routing sang 5 file KB
         3 cổng vào → phễu C1-C2-C3-PL → 6 nhóm nguyên nhân
OUTPUT   báo cáo: việc cần làm trước · kết luận · việc tiếp theo · nghẽn ở đâu
         + khối BÀN GIAO copy vào sổ theo dõi (đo lại sau, sửa KB từ phản hồi)
```

## Cài lên Custom GPT

**Ô `Name`:**

```
Chẩn đoán ASIN — ECom GreenWay
```

**Ô `Description`** (dòng hiển thị dưới tên, người dùng đọc trước khi bấm vào):

```
Bạn upload file export từ Amazon Ads, Business Report hoặc Helium 10. Bot định vị nghẽn
theo phễu Hiển thị → Bấm → Chuyển đổi → Lợi nhuận, nói rõ vì sao là tầng đó chứ không
phải tầng khác, và hướng dẫn bạn tự đặt ngưỡng thay vì áp số của người khác.
```

Description **không** phải chỗ nhét thêm luật — bot không đọc ô này, nó chỉ hiển thị cho người
dùng. Toàn bộ hành vi nằm ở `Instructions` và các file KB.

Rồi mới đến phần cấu hình:

1. Dán `master-instruction.md` của đúng bản vào ô **Instructions**.
2. Upload vào **Knowledge**: `kb/*.md` (4 file chung) + `kb_03` và `kb_05` của bản đó — 6 file.
3. **Bật `Code Interpreter & Data Analysis`.** Không bật thì bot không đọc được CSV/XLSX — lỗi kinh
   điển, dễ tưởng nhầm là "GPT không đọc được file".
4. Thêm 4 conversation starter, mỗi cái đã có nhánh xử lý riêng trong Master Instruction.

## Bốn nguồn dữ liệu và mức phủ

| Nguồn | Mở ra |
|---|---|
| Ads Campaigns **(bắt buộc)** | Campaign nền · tiền có tiêu được không · cách phân bổ tiền |
| Business Report | Listing và tỉ lệ chuyển đổi |
| H10 Cerebro | Thứ hạng, index, chuẩn bị mùa vụ |
| SP Targeting | Bối cảnh cấu trúc target |

**Trần phủ: 4/6 nhóm nguyên nhân.** Nhóm **giá & vị thế cạnh tranh** và **tồn kho** chưa có nguồn —
bot luôn phải khai, không được im lặng bỏ qua.

## Ba cổng chặn trước khi chẩn

1. **Dữ liệu** — có file Ads chưa · cửa sổ thời gian · mẫu có đủ để đọc tỉ lệ không.
2. **Mô hình kinh doanh** — lời trên từng đơn hay khách mua lại nhiều lần. Nếu mua lại nhiều lần
   (thực phẩm chức năng, đồ tiêu hao) thì **dừng, báo ngoài phạm vi**: toàn bộ knowhow nền rút từ
   mô hình lời-trên-đơn, bốn giả định gốc không áp được.
3. **Mức phủ** — in ra, hỏi chẩn luôn hay bổ sung, dừng chờ.

## Tài liệu kèm

| File | Nội dung |
|---|---|
| `framing-brief-v2.md` | 5 thay đổi so với Framing Brief Buổi 1, kèm lý do |
| `OVERVIEW-SID.md` | Soi toàn tuyến theo khung SID · RTC-COE nằm ở đâu sau khi phân rã |
| `DE-XUAT-TOI-UU.md` | Findings hai lăng kính review + 20 đề xuất |
| `rtc-coe-project.md` | Prompt RTC-COE dùng để dựng bộ này (bài tập số 2) |

## Không đưa vào repo

Repo bài nộp là repo công khai. Chặn bằng `.gitignore`:

- `data/` — file export thật
- `_noi-bo-KHONG-NOP/` — bản nội bộ, bảng ngưỡng, khảo sát dữ liệu, test log, bộ test hồi quy
- `_v1-KHONG-NOP/` — bản KB v1 giữ để đối chiếu

## Trạng thái

**Xong:** hai bộ KB, hai Master Instruction, RTC-COE, Framing v2, hai vòng test trên dữ liệu thật
3 ASIN (9 lỗi tìm được, đã sửa), bộ test hồi quy 9 ca, đã cài lên Custom GPT (24/08), thêm
checkpoint hậu kiểm (28/08).

**Còn lại:** **nhờ người khác test, không tự test** (thầy nhấn mạnh: mình tự chạy thì luôn thấy nó
ổn) · module CVR từ cây bài 04 (`kb_06_listing_cvr`, routing ba cửa) · test khả năng đọc trang Amazon
công khai để lấp nhóm nguyên nhân về giá.
