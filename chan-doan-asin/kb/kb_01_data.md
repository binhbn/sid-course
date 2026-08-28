# kb_01_data.md — Hợp đồng dữ liệu vào

**version:** v2.0 · **cập nhật:** 16/08/2026 · *(đổi tên từ `kb_data_schema.md`)*

## Lấy file ở đâu

Mỗi lần bot đòi dữ liệu, phải kèm đường lấy — không chỉ nói thiếu gì.

| File | Lấy ở đâu | Lưu ý khi export |
|---|---|---|
| **A — Ads Campaigns** | Amazon Ads Console → Campaign manager → chọn khoảng ngày → Export | **Ghi nhớ khoảng ngày đã chọn**, file không lưu lại |
| **B — Cerebro** | Helium 10 → Cerebro → nhập ASIN → Export | Là ảnh chụp thời điểm, không có lịch sử |
| **C — Business Report** | Seller Central → Reports → Business Reports → *Detail Page Sales and Traffic by Child Item* | Chọn đúng bản **by Child Item**, không phải Parent |
| **D — SP Targeting** | Amazon Ads Console → Reports → Sponsored Products → Targeting report | **Lọc theo portfolio của ASIN** và **không chia theo ngày** nếu chỉ cần đếm target — nhẹ hơn khoảng 30 lần |
| Search Term Report *(chưa dùng)* | Ads Console → Reports → Sponsored Products → Search term report | Cần khi muốn tách từ khoá lệch intent |

---

File này dạy bot **đọc file người dùng upload**: nhận diện file gì, cột nào nghĩa là gì, chỗ nào dễ
đọc nhầm, và **cái gì file không trả lời được**.

Bot đọc file này TRƯỚC mọi file KB khác. Đọc xong dữ liệu thì sang `kb_02_logic.md` **Phần I, Cổng
3** để biết với bộ file hiện có thì đánh giá được tới đâu. Chưa qua hai bước đó thì chưa được mở
phễu chẩn đoán ở `kb_02_logic.md` **Phần III**.

---

## 0. Bốn luật tối cao khi đọc dữ liệu

1. **Không suy ra con số không có trong file.** Thiếu chỉ số nào thì ghi "không có dữ liệu" và nêu
   cần file gì để có. Tuyệt đối không ước lượng, không lấy mặt bằng ngành điền vào chỗ trống.
2. **Không đoán cửa sổ thời gian.** Báo cáo Ads không lưu khoảng ngày của số liệu. Chưa có khoảng
   ngày thì **dừng và hỏi**, vì mọi ngưỡng trong `kb_03_thresholds.md` đều gắn với một cửa sổ.
3. **Không trộn hai cửa sổ thời gian khác nhau vào một phép tính.** Xem mục 7 — đây là lỗi âm thầm
   nguy hiểm nhất trong cả hệ thống.
4. **Khai trước khi chẩn đoán.** Đọc xong phải báo lại đã đọc file nào, bao nhiêu dòng, tổng
   spend/sales bao nhiêu (mục 8). Người dùng nhìn con số đó biết ngay bot có đọc đúng file không.

---

## 1. Nhận diện file — bằng tập cột, KHÔNG bằng tên file

Người dùng đổi tên file là chuyện thường. Chỉ được nhận diện bằng hàng tiêu đề.

| Loại | Nhận ra nhờ | Định dạng |
|---|---|---|
| **File A — Ads Campaigns** | có đủ `Campaigns`, `Portfolio`, `Top-of-search IS` | `.csv` |
| **File B — H10 Cerebro** | có đủ `Keyword Phrase`, `Organic Rank`, `Search Volume` | `.csv` / `.xlsx` (sheet `Table`) |
| **File C — Business Report** | có đủ `(Child) ASIN`, `Sessions - Total`, `Unit Session Percentage` | `.csv` |
| **File D — SP Targeting** | có đủ `Targeting`, `Match Type`, `Ad Group Name`, `Date` | `.xlsx` / `.csv` |

Không khớp tập cột nào ở trên → **không đoán**. Báo: "File này không thuộc bốn loại tôi đọc được",
liệt kê 10 tên cột đầu tiên và hỏi người dùng đó là báo cáo gì.

---

## 2. File A — Amazon Ads, export cấp Campaign

37 cột. Chỉ dùng những cột dưới đây, phần còn lại (nhóm Video, NTB, VCPM, CPM) bỏ qua trừ khi
người dùng hỏi thẳng.

### 2.1. Cột định danh

| Cột | Ý nghĩa | Bẫy |
|---|---|---|
| `Campaigns` | Tên campaign | Chứa nhiều thông tin nghiệp vụ, đọc theo mục 2.4 |
| `Portfolio` | Nhóm campaign theo sản phẩm | **Nguồn đáng tin nhất để xác định ASIN chủ thể** (mục 2.3) |
| `State` | `ENABLED` / `PAUSED` | PAUSED vẫn có thể có spend nếu nó chạy phần đầu kỳ rồi mới tắt |
| `Type` | `SP` / `SB2` / `SD` | `SB2` = Sponsored Brands. Ba loại là **ba tầng khác nhau**, không cộng gộp để so sánh |
| `Targeting` | `AUTOMATIC` / `MANUAL` | Auto vs Manual **ở cấp campaign**, không phải match type |
| `Campaign bidding strategy` | `Dynamic bidding (down only)` / `(up and down)` / `Fixed bids` / rỗng | Rỗng ở campaign không phải SP |
| `Start date` | Ngày bắt đầu, định dạng **M/D/YY** | **KHÔNG phải cửa sổ số liệu** |
| `End date` | Thường rỗng | Rỗng = chưa đặt ngày kết thúc, không phải lỗi |
| `Budget(USD)` | Ngân sách ngày | Là trần, không phải tiền đã tiêu |

### 2.2. Cột chỉ số — đơn vị phải đọc đúng

| Cột | Đơn vị thật | Đọc sai thành |
|---|---|---|
| `Impressions`, `Clicks`, `Orders` | số nguyên | — |
| `CTR` | **thập phân**: `0.0085` = 0,85% | tưởng 0,0085% |
| `ACOS` | **thập phân**: `0.434` = 43,4% | tưởng 0,434% |
| `ROAS` | bội số: `2.3044` = doanh thu gấp 2,3 lần chi phí ads | — |
| `Spend(USD)`, `Sales(USD)`, `CPC(USD)` | USD | — |
| `DPV` | lượt xem trang chi tiết | Nhiều dòng = 0 kể cả khi có click |
| `Top-of-search IS` | **CHUỖI, không phải số**. Ba dạng: `12.5%`, `<5%`, rỗng | Ép `<5%` thành số là bịa. Giữ nguyên chuỗi; rỗng là "không có dữ liệu". **Khác kiểu dữ liệu với cột cùng tên ở File D** |
| `Cost type` | `CPC` hoặc `VCPM` | Dòng `VCPM` tính tiền theo lượt hiển thị — **không so ACOS/ROAS chung với dòng CPC** |

**Tự tính được** (ghi rõ là số tự tính, không phải số Amazon trả về):
- CVR ads = `Orders / Clicks` — chỉ tính khi `Clicks >= 10`, ít hơn thì ghi "mẫu quá nhỏ".
- Tỉ lệ campaign có spend = số dòng `Spend > 0` / số dòng `State = ENABLED`.

### 2.3. Xác định ASIN chủ thể — làm theo đúng thứ tự này

1. **Đọc `Portfolio` trước.** Portfolio thường chứa mã ASIN và là nguồn đáng tin nhất — trong bộ
   dữ liệu tham chiếu, 100% số dòng có ASIN chủ thể ở cột này, còn tên campaign chỉ khoảng 96%.
2. Không thấy ở Portfolio thì tìm mã dạng `B0` + 8 ký tự trong `Campaigns`, lấy mã **xuất hiện ở
   nhiều dòng nhất**.
3. Vẫn không xác định được → **hỏi người dùng ASIN cần chẩn đoán**, không tự chọn.

> **BẪY NẶNG NHẤT — mã ASIN khác trong tên campaign không phải sản phẩm của mình.**
> Tên campaign thường chứa hàng chục mã `B0…` khác: đó là **product targeting nhắm ASIN đối thủ**.
> Luật: **ASIN chủ thể = ASIN trong Portfolio. Mọi mã B0 khác trong tên campaign = target đối thủ.**
> Có campaign mang hai mã cùng lúc (`B09XXXXXXX & B0BXXXXXXX_…`) — vẫn theo Portfolio mà xét.

Một file có thể lẫn campaign `Mutil…` chạy nhiều sản phẩm cùng lúc. Những dòng đó **không quy hết
cho ASIN chủ thể** — tách riêng và nói rõ là campaign dùng chung.

### 2.4. Đọc tên campaign — nhận diện bằng từ khoá, KHÔNG tách theo dấu

Trong thực tế có ít nhất ba hệ đặt tên chạy song song trong cùng một tài khoản:

```
ASIN_Tên SP_SP KW_Exact_01_Người_Mục đích_Mã kỳ    ← hệ gạch dưới
ASIN - Exact - <ký hiệu nội bộ> - ngày - từ khoá   ← hệ gạch ngang
Danh mục ASIN - <ký hiệu> - Broad - +từ +khoá      ← do công cụ sinh
Multi_Tên SP_Display__01_Người_Audiences           ← campaign nhiều sản phẩm
```

→ **Không tách tên theo dấu `_` hay `-` để lấy trường theo vị trí.** Chỉ dò từ khoá, không phân biệt
hoa thường:

| Dò thấy | Hiểu là | Ghi chú |
|---|---|---|
| `exact` / `broad` / `phrase` | match type của campaign keyword | Đối chiếu chéo với `Match Type` ở File D nếu có |
| `auto` | campaign Auto | Đối chiếu chéo với `Targeting = AUTOMATIC` |
| `pt` / `asin` / `product page` | campaign product targeting | |
| `video` | Sponsored Brands Video hoặc SP Video | |
| ký hiệu riêng của tổ chức | Nhiều đội đặt ký hiệu riêng trong tên campaign để đánh dấu vai trò | **Không suy ra số target từ nhãn.** Đã gặp ca thật: campaign mang nhãn "một cụm từ khoá" đúng là 1 target, nhưng campaign đông target nhất lại nằm ở nhóm tên khác hẳn. Muốn biết số target thì đếm ở File D. Có bảng ký hiệu riêng thì nạp thành file phụ lục, đừng đoán |
| `mutil` | campaign nhiều sản phẩm | Tách riêng, không quy cho một ASIN |

> **Tên campaign là ý định, File D là sự thật.** Tên nói campaign đó *định* làm gì; chỉ File D mới
> cho biết bên trong nó *thật sự* có bao nhiêu target. Có file D thì luôn tin File D.

Từ khoá xung đột nhau (vừa `exact` vừa `broad`) hoặc không dò thấy gì → xếp vào **"chưa phân loại"**
và nêu rõ số lượng. Không đoán bừa.

### 2.5. File A KHÔNG trả lời được những gì

- **Số target trong một campaign** → cần File D.
- **Từ khoá nào đang tiêu tiền** → cần File D hoặc Search Term Report.
- **Cửa sổ thời gian của số liệu** → phải hỏi người dùng (mục 7).
- **Sessions, tỉ lệ chuyển đổi toàn ASIN** → cần File C. Số ở File A là số ads, không phải số của
  cả listing.
- **Giá bán, Buy Box, tồn kho** → không nguồn nào trong bốn file này có.

---

## 3. File B — Helium 10 Cerebro (rank từ khoá)

21 cột, mỗi dòng là một keyword phrase. `.xlsx` thì dữ liệu nằm ở sheet **`Table`**.

| Cột | Ý nghĩa | Bẫy |
|---|---|---|
| `Keyword Phrase` | Từ khoá | |
| `Search Volume` | Lượng tìm kiếm tháng | |
| `Organic Rank` | Thứ hạng tự nhiên | **Số nhỏ = tốt.** Rỗng hoặc `-` = **không index / ngoài bảng xếp hạng**, KHÔNG phải rank 0 |
| `Sponsored Rank` | Thứ hạng quảng cáo | `-` rất phổ biến = không thấy quảng cáo ở lần quét đó, không kết luận "không chạy ads" |
| `Competing Products` | Số sản phẩm cạnh tranh | Hay chạm trần `1000` → đọc là "từ 1000 trở lên" |
| `CPR` | Số đơn H10 ước tính để lên trang 1 | **Ước tính của H10**, không phải số Amazon |
| `H10 PPC Sugg. Bid` (+ Min/Max) | Bid gợi ý | Gợi ý của công cụ, không phải bid thật đang chạy |
| `ABA Total Click Share` / `Conv. Share` | Thị phần theo Amazon Brand Analytics | **Số phần trăm dạng số**: `34.3` = 34,3%. Rất nhiều dòng rỗng |
| Các cờ `Organic`, `Sponsored Product`, `Amazon Choice`… | 0/1 — vị trí đó có xuất hiện không | Cờ hiện diện, không phải thứ hạng |
| `Search Volume Trend` | Số có dấu: `77`, `-28` | **Chưa xác minh đơn vị.** Chỉ đọc chiều tăng/giảm, **không dùng làm căn cứ cho bất kỳ ngưỡng nào** |

### 3.0. Search Volume KHÔNG phải thứ tự cơ hội

Từ khoá lượng tìm lớn nhất thường là từ **chung chung** — người tìm chưa biết mình muốn gì. Từ đúng
**ý định** của sản phẩm thường nhỏ hơn nhiều mà lại ra đơn.

Nên khi xếp cơ hội từ khoá, **không xếp theo `Search Volume`**. Đọc theo cặp:

| Dấu hiệu | Đọc là |
|---|---|
| SV lớn · rank rất xa hoặc rỗng · từ chỉ mô tả loại sản phẩm chung | Từ chung chung — chưa phải cơ hội, và cũng là chỗ ads dễ đốt tiền |
| SV vừa · từ có **thuộc tính hoặc dịp** của đúng sản phẩm này · rank rỗng | **Đây mới là khoảng trống đáng đẩy** |
| SV bất kỳ · đã có rank tốt | Không phải khoảng trống — đây là chỗ cần giữ |

Bot **không tự phán** từ nào đúng ý định khi tên sản phẩm không đủ để suy ra. Không chắc thì liệt kê
kèm nhãn *"chưa xác định được ý định"* và hỏi người dùng, đừng xếp hạng bừa.

> Cerebro lấy theo **ASIN cha**: từ khoá của biến thể khác cũng nằm trong bảng. Lọc bằng tên/thuộc
> tính của đúng sản phẩm đang chẩn trước khi kết luận "chưa index".

### 3.1. File B là ảnh chụp một thời điểm

Cerebro cho **rank hiện tại**, không có lịch sử.

- Trả lời được: "ASIN đang đứng ở đâu với từ khoá nào", "từ khoá nào chưa index".
- **Không** trả lời được: "rank đã tụt bao nhiêu so với trước" — cần Keyword Tracker theo thời gian
  hoặc hai lần export cách nhau.

---

## 4. File C — Business Report (Seller Central)

Mỗi dòng là một **SKU**, không phải một ASIN. Đây là nguồn duy nhất cho traffic và chuyển đổi của
cả listing (không chỉ phần ads).

| Cột | Ý nghĩa | Bẫy |
|---|---|---|
| `(Parent) ASIN` / `(Child) ASIN` | ASIN cha / con | Lọc theo **Child ASIN**. ASIN cha gộp nhiều biến thể |
| `SKU` | Mã SKU | Chìa khoá của bẫy 4.1 |
| `Sessions - Total` | Số phiên truy cập | **KHÔNG cộng dồn** — xem 4.1 |
| `Page Views - Total` | Lượt xem trang | Cũng không cộng dồn |
| `Units Ordered` | Số sản phẩm bán được | Cộng dồn được |
| `Unit Session Percentage` | Tỉ lệ chuyển đổi của listing | Chuỗi có `%`: `27.00%`. Phải tính lại khi có nhiều SKU |
| `Ordered Product Sales` | Doanh số | Chuỗi có `$` và dấu phẩy: `$3,032.89` → bỏ `$` và `,` mới ép về số |
| `Total Order Items` | Số đơn hàng | Khác `Units Ordered` khi một đơn mua nhiều cái |

### 4.1. Bẫy nặng nhất — một ASIN có thể có nhiều dòng

Một child ASIN bán bằng nhiều SKU sẽ có **nhiều dòng**, và Amazon **lặp lại y nguyên số Sessions ở
mỗi dòng** vì phiên truy cập tính ở cấp ASIN chứ không cấp SKU.

Trong bộ dữ liệu tham chiếu: 85 dòng nhưng chỉ 64 child ASIN duy nhất, và một ASIN có 2 dòng cùng
`Sessions = 694` với `Units Ordered` là 27 và 13.

**Luật xử lý khi một ASIN có nhiều dòng:**
- `Units Ordered`, `Ordered Product Sales`, `Total Order Items` → **cộng lại**.
- `Sessions - Total`, `Page Views - Total` → **lấy giá trị của một dòng**, không cộng. Cộng vào là
  thổi phồng traffic lên gấp đôi và kéo tỉ lệ chuyển đổi xuống còn một nửa.
- `Unit Session Percentage` → **tính lại**: tổng `Units Ordered` ÷ `Sessions`. Không cộng các ô %.
- Nếu các dòng có `Sessions` **khác nhau** thì dừng lại, báo người dùng và hỏi, đừng tự chọn dòng nào.

Ví dụ đúng: hai dòng 27 và 13 units trên 694 sessions → CVR = 40/694 = **5,76%**, không phải 3,89%
(chỉ một dòng) và cũng không phải 694+694 ở mẫu số.

### 4.2. File C là báo cáo cả tài khoản

File chứa mọi ASIN đang bán, không riêng ASIN cần soi. **Bắt buộc lọc theo Child ASIN trước.**
ASIN cần soi không có trong file → nói thẳng là không có, đừng lấy dòng gần giống.

---

## 5. File D — Sponsored Products Targeting report

Đây là nguồn duy nhất cho biết **bên trong một campaign có bao nhiêu target**. 25 cột, mỗi dòng là
một target trong một ngày.

| Cột | Ý nghĩa | Bẫy |
|---|---|---|
| `Date` | Ngày của dòng số liệu | Trong `.xlsx` là **số serial Excel** (`46239.0`), phải quy đổi với mốc **30/12/1899**. In ra `46239` là vô nghĩa với người đọc |
| `Portfolio name` | Nhóm sản phẩm | Dùng để lọc ASIN chủ thể, ưu tiên hơn tên campaign |
| `Campaign Name` / `Ad Group Name` | Tên campaign / ad group | Cùng ba hệ đặt tên ở mục 2.4 |
| `Targeting` | **Từ khoá hoặc ASIN được nhắm** | Chính là thứ dùng để đếm số target |
| `Match Type` | `EXACT` / `BROAD` / `PHRASE` / dạng target tự động | Đây mới là match type thật, tên campaign chỉ là ý định |
| `Impressions`, `Clicks`, `Spend`, `Cost Per Click (CPC)` | số | Kiểu số thật, không phải chuỗi |
| `Click-Thru Rate (CTR)`, `Total ACOS`, `Total ROAS`, `7 Day Conversion Rate` | tỉ lệ | **Rỗng khi không có đơn** — rỗng không phải 0 |
| `Top-of-search Impression Share` | số | **Khác kiểu với cột cùng tên ở File A** (bên đó là chuỗi `<5%`). Là chỉ số **của campaign**, không phải của ASIN — không cộng, không trung bình cộng giữa các campaign |
| `7 Day Total Sales / Orders / Units` | Kết quả trong cửa sổ quy đổi 7 ngày của Amazon | "7 Day" ở đây là **attribution window**, KHÔNG phải khoảng ngày của report |
| `7 Day Advertised SKU …` vs `Other SKU …` | Doanh số của chính SKU quảng cáo vs SKU khác | Chênh lệch lớn giữa hai cột là tín hiệu đáng xem riêng |

### 5.1. Ba luật bắt buộc với File D

**a. Quy đổi `Date` rồi mới nói khoảng ngày.** Báo lại cho người dùng dạng "17/07/2026 → 15/08/2026,
30 ngày", không bao giờ in số serial.

**b. Đây là báo cáo cả tài khoản, dòng theo ngày.** Bộ dữ liệu tham chiếu có 117.596 dòng cho toàn
tài khoản, trong đó chỉ 8.880 dòng thuộc 3 ASIN cần soi. **Lọc theo ASIN trước khi tính bất cứ thứ
gì.** Và vì mỗi target lặp lại mỗi ngày, **đếm số target phải đếm giá trị duy nhất của `Targeting`
trong phạm vi một campaign**, không đếm số dòng.

**c. File này rất nặng — và đây là lỗi phải to tiếng.** Bản tham chiếu 11 MB, bung ra hơn 140 MB
khi xử lý, 117.596 dòng; thư viện đọc Excel thông thường không mở nổi.

Luật xử lý, theo đúng thứ tự:

1. Thử đọc. Đọc được trọn vẹn → làm bình thường.
2. Đọc không nổi, hoặc chỉ đọc được một phần → **dừng và báo thẳng**:
   *"File Targeting quá lớn (<n> MB), tôi không đọc được trọn vẹn nên sẽ không dùng nó."*
   Rồi hướng dẫn export lại theo mục 5.3.
3. **Tuyệt đối không đọc một phần rồi tính như thể đã đọc hết.** Đếm target trên một nửa dữ liệu sẽ
   ra "campaign này có 40 target" trong khi thật ra là 146 — sai theo hướng làm nhẹ vấn đề đi.
4. Không có File D dùng được → coi như **không có File D**, tính lại mức phủ theo `kb_02_logic.md` (Phần I, Cổng 3)
   và nói rõ nhánh cấu trúc target chưa loại trừ được.

### 5.3. Hướng dẫn export lại cho nhẹ

Khi cần bảo người dùng export lại, đưa đúng ba điều chỉnh này:

- **Lọc theo portfolio của đúng ASIN cần soi**, đừng xuất cả tài khoản — riêng bước này đã cắt được
  phần lớn dung lượng (bản tham chiếu: 8.880 dòng có ích trên tổng 117.596, tức khoảng 7%).
- **Không chia theo ngày** khi mục đích chỉ là đếm target và xem phân bổ spend. Dạng tổng cho cả kỳ
  là đủ, và nhẹ hơn khoảng 30 lần.
- **Xuất CSV thay vì XLSX** nếu công cụ cho chọn.

Chỉ giữ dạng chia theo ngày khi thật sự cần nhìn diễn biến theo thời gian — và khi đó thì càng phải
lọc chặt theo một ASIN.

### 5.2. Chỉ số quan trọng nhất rút ra từ File D

**Số target duy nhất trên mỗi campaign** — đây là chỉ số mà cả ba file kia không có.

Cách tính: nhóm theo `Campaign Name`, đếm số giá trị duy nhất của `Targeting`. Xuất bảng phân bố:
1 target · 2–5 · 6–20 · trên 20, kèm tên các campaign đông target nhất.

Ý nghĩa nghiệp vụ nằm ở `kb_02_logic.md` **Phần V**, nhánh cấu trúc target. Ở đây chỉ tính và trình bày, **không
kết luận** — vì một campaign nhiều target là bình thường hay bất thường còn tuỳ loại campaign, việc
đó thuộc về cây chẩn đoán.

---

## 6. Cửa sổ thời gian — chỗ dễ sai âm thầm nhất

Bốn file có thể mang bốn khoảng ngày khác nhau, và **không file nào tự khai đầy đủ**:

| File | Cửa sổ nằm ở đâu |
|---|---|
| A — Ads Campaigns | **Không có trong file.** Người dùng chọn lúc export → **phải hỏi** |
| B — Cerebro | Ảnh chụp tại thời điểm export, không có khoảng |
| C — Business Report | Thường có trong tên file, nhưng **tên file không đáng tin** → hỏi lại |
| D — SP Targeting | **Tính được** từ cột `Date` (min → max) |

### 6.1. Hai câu hỏi bắt buộc trước khi chẩn đoán

Trước khi mở `kb_03_thresholds.md`, bot phải hỏi và nhận được câu trả lời cho **cả hai**:

> **1.** "Mỗi file đang lấy số liệu của khoảng ngày nào? Riêng file Ads Campaigns bắt buộc phải có,
> vì khoảng ngày không được lưu trong file."
>
> **2.** "ASIN này có ngưỡng riêng không (ROAS sàn, trần TACOS) — đọc từ hệ báo cáo nội bộ? Nếu có
> thì cho tôi con số; không có thì tôi dùng mốc chung theo giai đoạn vòng đời."
>
> **3.** "Ngưỡng ROAS cắt cho ASIN này là bao nhiêu? Mỗi ngách một khác, và còn tuỳ chiến lược đang
> chạy, nên tôi không tự đặt."

**Không trả lời câu 1** → phần **cấu trúc** vẫn chạy được (có bao nhiêu campaign, loại gì, bao nhiêu
cái đang bật, bao nhiêu target mỗi campaign), nhưng mọi kết luận dựa trên ngưỡng phải đánh dấu
**"chưa xác nhận cửa sổ thời gian"**.

**Không trả lời câu 2** → dùng mốc chung, và mọi kết luận đóng dấu **"chưa dùng ngưỡng per-ASIN"**.
Ngưỡng đáng tin nhất luôn tính riêng cho từng ASIN theo dư địa biên lợi nhuận của chính nó; mốc
chung chỉ là sàn tham chiếu.

**Không trả lời câu 3** → **không kết luận cắt hay giữ campaign nào**. Bot chỉ được trình bày
campaign nào đang nằm dưới sàn tham chiếu nào, và nói rõ chưa có ngưỡng cắt riêng. Không tự chọn
một con số để cắt hộ.

Trong cả ba trường hợp, bot **không được tự tính lại** ngưỡng từ dữ liệu trong file — hai chỗ cùng
tính một con số là chắc chắn lệch, và lệch âm thầm.

### 6.2. Số ads gần đây CHƯA CHÍN — bẫy im lặng

Amazon quy đơn theo cửa sổ trễ và **sửa lại số liệu trong khoảng 10–15 ngày sau đó**. Nghĩa là:

- **3 ngày gần nhất luôn là số tạm.** Đơn sẽ còn được cộng thêm vào các ngày này.
- Cửa sổ được coi là **đã chín** là khoảng **từ 9 ngày trước đến 3 ngày trước**.
- Một file "7 ngày gần nhất" export hôm nay thì **toàn bộ nằm trong vùng chưa chín** — ROAS, ACOS,
  Orders trong đó đều sẽ thay đổi.

**Luật xử lý:**

1. Cửa sổ người dùng khai có chạm 3 ngày gần nhất → gắn nhãn **"số chưa chín"** vào mọi kết luận
   dùng ROAS / ACOS / Orders / CVR, và nói rõ một câu: *"các chỉ số này còn thay đổi khi Amazon quy
   đơn lại."*
2. Khi kết luận là **"ROAS dưới sàn"** trên dữ liệu chưa chín → hạ xuống mức **kết luận tạm**, kèm
   khuyến nghị đọc lại bằng cửa sổ đã chín.
3. Chỉ số **cấu trúc** (số campaign, trạng thái, số target, phân bố spend) **không bị ảnh hưởng** —
   vẫn kết luận bình thường. Đây là lý do phần cấu trúc đáng tin hơn phần hiệu quả khi dữ liệu mới.
4. Không tự ý điều chỉnh số để "bù" cho phần chưa chín. Gắn nhãn, không sửa số.

**Luật cấm trộn.** Không được đưa số từ hai cửa sổ khác nhau vào cùng một phép tính hay một phép so
sánh. Ví dụ sai điển hình: lấy doanh số ads 7 ngày chia cho doanh số toàn ASIN 30 ngày rồi kết luận
"ads chỉ đóng góp 8% doanh số". Con số đó vô nghĩa nhưng trông rất thuyết phục — đúng kiểu sai âm
thầm mà cả hệ thống này được dựng ra để chặn.

Buộc phải so hai cửa sổ khác nhau thì: quy về **trung bình ngày** trước, và ghi rõ đã quy đổi.

---

## 7. Báo cáo đọc file — in ra trước khi chẩn đoán

```
ĐÃ ĐỌC
- File A (Ads Campaigns): <tên file> — <N> campaign
  · ASIN chủ thể: <ASIN> (nhận từ cột Portfolio)
  · Cửa sổ thời gian: <người dùng khai / CHƯA KHAI>
  · Theo loại: SP <n> · SB <n> · SD <n>
  · Trạng thái: ENABLED <n> · PAUSED <n>
  · Có phát sinh spend: <n>/<số ENABLED> campaign
  · Tổng: spend $<x> · sales $<y> · <i> impressions · <c> clicks · <o> orders
  · Chưa phân loại được theo tên: <n> campaign
- File B (Cerebro): <tên file> — <M> từ khoá
  · Có Organic Rank: <n> · không index: <n>
- File C (Business Report): <tên file>
  · Dòng khớp ASIN: <n> (nếu >1: đã gộp units, giữ nguyên sessions)
  · Sessions <s> · Units <u> · CVR tính lại <p>% · Doanh số $<v>
- File D (SP Targeting): <tên file> — <R> dòng, <r> dòng thuộc ASIN chủ thể
  · Khoảng ngày: <dd/mm/yyyy> → <dd/mm/yyyy> (<k> ngày)
  · <c> campaign · <t> target duy nhất
  · Phân bố target/campaign: 1 target <a> · 2–5 <b> · 6–20 <c> · trên 20 <d>

CHƯA CÓ
- <theo kb_02_logic.md, Phần I Cổng 3>
```

Con số nào lệch với cái người dùng biết → dừng lại làm rõ, đừng chẩn đoán tiếp.
