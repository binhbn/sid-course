# kb_02_logic.md — Phạm vi, khung chẩn đoán, nguyên nhân

**version:** v2.0 · **cập nhật:** 16/08/2026
**Thay cho:** `kb_causes.md` + `kb_tree.md` + `kb_coverage.md` (v1, gộp lại vì ba file này luôn phải
sửa cùng nhau).

Bot đọc file này **sau** `kb_01_data.md` (đọc được dữ liệu) và **trước** `kb_03_thresholds.md`.

---

# PHẦN I — BA CỔNG TRƯỚC KHI CHẨN

Không qua đủ ba cổng thì không chạy chẩn đoán.

## Cổng 1 — Dữ liệu

| Kiểm | Không đạt thì |
|---|---|
| Có file Ads không? | Không → **không chạy cây**, chỉ trả lời câu hỏi hẹp trong phạm vi file đang có |
| Đã khai cửa sổ thời gian? | Chưa → hỏi. Không có → chỉ chạy phần **cấu trúc**, đóng dấu "chưa xác nhận cửa sổ" |
| Cửa sổ chạm 3 ngày gần nhất? | Có → gắn nhãn **"số chưa chín"** cho mọi kết luận về ROAS/ACOS/Orders/CVR |
| **Cửa sổ có đủ dài để kết luận cắt?** | Xem Cổng 1.1 |
| Mẫu có đủ để đọc tỉ lệ? | Dưới 50 click **hoặc** dưới 5 đơn → **chỉ kết luận bằng chỉ số cấu trúc**, không dùng tỉ lệ. **Đúng 50 click, hoặc đúng 5 đơn, vẫn tính là chưa đủ** — chạm ngưỡng là áp luật, không phải vượt mới áp |

### Cổng 1.1 — Cửa sổ quyết định loại kết luận được phép đưa ra

*(✅ — cửa sổ 7 ngày chỉ dùng để **phát hiện điểm nóng**; muốn kết luận CẮT hay THA phải đọc 14 hoặc
30 ngày.)*

| Cửa sổ | Được kết luận gì |
|---|---|
| **7 ngày** | Phát hiện điểm nóng · đọc chỉ số cấu trúc · nêu nghi vấn. **KHÔNG kết luận cắt/tha** |
| **14–30 ngày** | Kết luận đầy đủ, kể cả cắt/tha |
| Trong event | Rút về 3 ngày / hôm nay để bắt điểm nóng, cắt lởm bằng tay hằng ngày (✅) |

**Đây là lỗi đã mắc thật:** chạy ngưỡng cắt 15 click trên cửa sổ 7 ngày thì gần như không campaign
nào chạm mốc. Nếu bot không cảnh báo, người đọc hiểu thành *"không có gì đáng cắt"* — trong khi thực
tế chỉ là **cửa sổ ngắn hơn ngưỡng**.

## Cổng 2 — Mô hình kinh doanh của sản phẩm ⚠️ CỔNG CHẶN

**Hỏi trước khi chẩn:** *"ASIN này thuộc mô hình nào — lời trên từng đơn (gifting, quà tặng, đồ dùng
mua một lần), hay khai thác giá trị vòng đời khách hàng (thực phẩm chức năng, đồ tiêu hao, hàng mua
lại định kỳ)?"*

| Mô hình | Bot làm gì |
|---|---|
| **Lời trên từng đơn** | Chẩn bình thường. Đây là mô hình mà toàn bộ knowhow trong bộ KB này được rút ra |
| **Giá trị vòng đời khách hàng** | **DỪNG.** Nói thẳng là ngoài phạm vi, nêu lý do (bên dưới), đề nghị người dùng tự đánh giá hoặc bổ sung knowhow riêng cho mô hình đó |
| Không biết / hỗn hợp | Hỏi lại một lần. Vẫn không rõ → chẩn theo mô hình lời-trên-đơn nhưng **đóng dấu cảnh báo** |

**Vì sao phải chặn.** Với mô hình giá trị vòng đời, bốn giả định nền của bộ KB này đều sai:

| Giả định trong KB | Với mô hình vòng đời |
|---|---|
| ROAS thấp trên đơn đầu = bệnh | Là **bình thường** nếu khách mua lại nhiều lần |
| Nhiều click không ra đơn → cắt | Sai — chu kỳ cân nhắc dài, đơn đến muộn |
| Rank là thước chính khi đẩy | Cạnh tranh còn ở thương hiệu và tỉ lệ đăng ký mua định kỳ |
| CVR là thước chốt đơn | Thiếu hẳn tỉ lệ mua lại và đăng ký định kỳ — **không nguồn nào trong bốn file có** |

Chẩn một ASIN vòng đời bằng bộ luật này sẽ ra kết luận nghe rất hợp lý và **sai về mô hình**. Đó là
dạng sai nguy hiểm nhất vì không có gì báo.

## Cổng 3 — Mức phủ

### 3.1. Sáu nhánh cần nguồn gì

| # | Nhánh nguyên nhân | Nguồn bắt buộc | Nguồn hỗ trợ |
|---|---|---|---|
| 1 | Chưa lên đủ campaign nền | File A | File D |
| 2 | Spend không tiêu được | File A | File D |
| 3 | Spend rải rác / target lệch intent | File A | File D |
| 4 | Listing và tỉ lệ chuyển đổi | **File C** | File A |
| 5 | Giá và vị thế cạnh tranh | *chưa có nguồn* | File B (gián tiếp) |
| 6 | Tồn kho | *chưa có nguồn* | — |
| — | Rank & index *(tín hiệu chéo)* | File B | — |

> **Trần phủ hiện tại: 4/6 nhánh.** Kể cả đủ bốn file, nhánh **giá & vị thế** và **tồn kho** vẫn
> không có nguồn. Mọi báo cáo phải khai điều này.

### 3.2. Ma trận theo bộ file nhận được

| Có trong tay | Kết luận được | Chưa loại trừ được |
|---|---|---|
| Chỉ **A** | Nhánh 1, 2, 3 | 4, 5, 6 |
| **A + B** | 1, 2, 3 + rank/index | 4, 5, 6 |
| **A + C** | 1, 2, 3, 4 | 5, 6 |
| **A + B + C + D** | 1, 2, 3, 4 + rank/index | 5, 6 |
| Không có **A** | — | Không dựng được bức tranh ads |

**File A là xương sống.** Thiếu nó vẫn trả lời được câu hỏi hẹp, nhưng không được gọi là "chẩn đoán".

### 3.3. Cổng xác nhận — in ra rồi dừng chờ

```
MỨC PHỦ CỦA LẦN CHẨN ĐOÁN NÀY
Có: <file>  → soi được: <nhóm nguyên nhân>
Chưa soi được: <nhóm> — vì thiếu <dữ liệu>
Bổ sung <file> sẽ mở thêm: <nhóm>
Chẩn luôn với dữ liệu hiện có, hay bổ sung thêm?
```

Ngoại lệ duy nhất được bỏ qua cổng này: người dùng hỏi một câu hẹp mà dữ liệu hiện có trả lời trọn.
Khi đó trả lời thẳng, ghi rõ *"đây là câu trả lời hẹp, không phải chẩn đoán"*.

---

# PHẦN II — PHẠM VI: CHẨN ĐƯỢC GÌ, KHÔNG CHẨN ĐƯỢC GÌ

Bảng này để **nói trước**, không phải để biện minh sau.

## Chẩn được

| Tình huống | Cần |
|---|---|
| ASIN không được nhìn thấy — impression thấp | File A |
| Campaign bật mà không tiêu được tiền | File A |
| Tiền tiêu rải rác, không nhóm nào ra đơn | File A |
| Quảng cáo kéo về traffic lệch intent | File A + C |
| Listing không chốt được đơn | File C |
| Chưa index / rank yếu ở từ khoá mục tiêu | File B |
| Chưa đủ điều kiện chuẩn bị mùa vụ | File B |
| Cấu trúc target bên trong campaign | File D |

## KHÔNG chẩn được — nói thẳng khi gặp

| Tình huống | Vì sao |
|---|---|
| **Mô hình giá trị vòng đời khách hàng** | Toàn bộ knowhow rút từ mô hình lời-trên-đơn (Cổng 2) |
| Giá và vị thế cạnh tranh | Không nguồn nào cấp giá, Buy Box |
| Tồn kho | Không có nguồn |
| **Rank đã tụt bao nhiêu** | Nguồn rank là ảnh chụp một thời điểm, không có lịch sử |
| Diễn biến theo thời gian | Không có chuỗi thời gian |
| **So sánh nhiều ASIN để chọn xử trước** | Là loại nhiệm vụ khác (`COMPARE`), ngoài phạm vi |
| ASIN mới, chưa đủ dữ liệu | Mẫu quá nhỏ — xem Cổng 1 |
| Sản phẩm nhiều biến thể parent-child | Số liệu gộp theo ASIN cha làm sai lệch |
| Kênh ngoài Amazon | Cơ chế xếp hạng và ngưỡng khác hẳn |
| Dự đoán hiệu quả tương lai | Bot đọc chuyện đã xảy ra |

---

# PHẦN III — KHUNG CHẨN ĐOÁN: PHỄU C1 → C2 → C3 → PL

*(📕 Chuẩn, xác nhận 4 lần: chẩn đoán ASIN theo bốn chỉ số C1 Impression · C2 Clicks · C3 Conversions
· PL rồi **mới** chọn giải pháp — và bắt buộc quay lại nghiệm thu.)*

## Vì sao đi theo phễu

Phễu **tự nó đã mang thứ tự nhân quả**: phải được nhìn thấy mới có click, có click mới có đơn, có
đơn mới nói chuyện lợi nhuận. Nên không cần node nào có "quyền tiên quyết" đặc biệt — thứ tự nằm sẵn
trong cấu trúc.

Đây là điều chỉnh quan trọng so với bản v1. Bản cũ xếp thứ tự soi theo *chi phí soi* rồi phải thêm
node tiên quyết để chữa, và chính node đó lại chặn cây, gây bỏ sót. Phễu giải quyết cả hai.

**Luật đọc:** đi từ **C1 xuống**, dừng ở tầng đầu tiên có bất thường rõ, nhưng **vẫn ghi nhận** các
tầng dưới thay vì bỏ qua. Bất thường ở tầng trên làm cho số ở tầng dưới **mất ý nghĩa**, không phải
làm chúng biến mất.

## C1 — Impression: ASIN có được nhìn thấy không?

**Đo:** tổng impression · số campaign ENABLED có spend · Top-of-search IS · tình trạng index (File B).

> **Đọc Top-of-search Impression Share cho đúng.** Chỉ số này là **của từng campaign**, không phải của
> ASIN. Không cộng, không lấy trung bình cộng giữa các campaign — campaign 12 impression và campaign
> 40.000 impression không có cùng trọng số. Cách dùng đúng: nêu TOS IS của **campaign đang mang phần
> lớn impression**, kèm tên campaign đó. File A trả chuỗi dạng `<5%` (đọc là "dưới 5%", không quy
> thành 5), File D trả số — **không so hai file với nhau**. Không có cột này thì nói không có, đừng
> suy TOS IS từ impression.

| Bất thường | Nhánh nguyên nhân |
|---|---|
| Rất ít campaign đang chạy | **Nhánh 1** — nhưng phân biệt "chưa lên" với "đã lên rồi tắt", xem III.1 |
| Campaign bật mà không tiêu được tiền | **Nhánh 2** |
| Không index ở từ khoá mục tiêu | Nhánh rank/index — *(🧪: spend đều vài ngày mà vẫn chưa index thì nghi listing, không phải lỗi campaign)* |

### III.1. "Chưa lên bao giờ" khác "đã lên rồi tắt"

| Dấu hiệu | Đọc là | Xử lý |
|---|---|---|
| Ít campaign, hầu như không có PAUSED | **Chưa lên** — thiếu thật | Báo nhánh 1 |
| Nhiều campaign, phần lớn PAUSED | **Đã tắt** — quyết định vận hành | **Không báo thiếu.** Nêu quan sát và hỏi |

Bot **không đề xuất "bật lại campaign đã tắt"** như hướng mặc định — kho yêu cầu bật lại phải nêu
được ít nhất hai yếu tố đã đổi cộng một đòn bẩy mới (✅).

## C2 — Click: có impression rồi thì người ta có bấm không?

**Đo:** CTR · tương quan impression với click.

| Bất thường | Hướng |
|---|---|
| CTR thấp dù impression nhiều | Vấn đề ở **sức hấp dẫn**: ảnh chính, giá hiển thị, tiêu đề — hoặc từ khoá lệch intent |
| CTR bình thường | Xuống C3 |

**Luật quan trọng ở tầng này:** nhiều click không ra đơn **nhưng search term vẫn sát sản phẩm** thì
**giảm bid, không negative**. Negative chỉ dành cho từ lệch intent (📕).

## C3 — Conversion: có click rồi thì có chốt được đơn không?

**Đo hai lớp:**

### C3.1 — CVR của listing (File C)

CVR listing thấp so với mặt bằng ngành hàng → **nhánh 4**.

> **Hai lớp chặn bắt buộc.** Một: CVR thấp **là hệ quả bình thường** khi ASIN chưa có rank — chỉ kết
> luận nhánh 4 sau khi C1 và C2 đã sạch. Hai: bảng CVR theo ngành hàng đang ở mức 🧪 và chính người
> chốt đã gắn cờ cần xác nhận lại → **không viết "listing kém"**, phải viết *"CVR thấp hơn mặt bằng
> tham khảo, mà mặt bằng đó chưa được xác nhận"*.

### C3.2 — Đối chiếu CVR ads với CVR listing

Phép so rẻ nhất trong cả khung, và là cách **phân định "quảng cáo kém" với "listing kém"**:

| Tương quan | Đọc là | Đường đi |
|---|---|---|
| CVR ads **thấp hơn nhiều** | Ads kéo về traffic lệch intent; listing vẫn tốt | **Nhánh 3**, không phải nhánh 4 |
| Hai bên xấp xỉ | Không thêm thông tin | Xuống PL |
| CVR ads **cao hơn** | Ads chọn đúng từ; traffic tự nhiên mới kéo tỉ lệ xuống | Xem lại từ khoá đang index |

**Hai con số này KHÔNG cùng một phép chia** — đây là bẫy nặng hơn chuyện khác cửa sổ:

| | Mẫu số | Nguồn |
|---|---|---|
| CVR ads | **lượt bấm** vào quảng cáo | File A / File D |
| CVR listing | **phiên truy cập** trang sản phẩm (Sessions) | File C |

Một phiên có thể xem nhiều lần, một lượt bấm quảng cáo cũng tính là một phiên. Nên **cấm trừ hai số
cho nhau, cấm chia hai số cho nhau**, và cấm nói "chênh 1,2 điểm phần trăm". Chỉ được đọc **chiều**:
bên nào cao hơn hẳn.

"Cao hơn hẳn" = **gấp rưỡi trở lên**. Dưới mức đó → ghi *"hai bên xấp xỉ"*, không diễn giải thêm.

Hai file khác cửa sổ → nói rõ hai cửa sổ ngay tại chỗ so, và hạ kết quả xuống **tín hiệu định hướng**,
không phải bằng chứng kết luận. Muốn dùng làm bằng chứng thì export lại hai file **cùng khoảng ngày**.

## PL — Lợi nhuận: có đơn rồi thì có lời không?

**Đo:** ROAS/ACOS theo vai campaign · phân bố spend · tỉ trọng spend vào nhóm không ra đơn.

| Bất thường | Nhánh |
|---|---|
| Spend rải đều nhiều campaign, không nhóm nào ra đơn | **Nhánh 3** |
| ROAS dưới kỳ vọng **của đúng vai campaign đó** | Xem Phần IV |
| Có lời nhưng tồn kho lệch | **Nhánh 6** — chưa có nguồn |

**Luật nghiệm thu (📕):** chẩn xong, chọn giải pháp xong thì **bắt buộc quay lại kiểm** — "gãi xong
có đỡ ngứa không". Mỗi hướng xử lý phải kèm chỉ số theo dõi và mốc thời gian.

---

# PHẦN IV — KHÔNG CHẤM MỌI CAMPAIGN BẰNG MỘT THƯỚC

*(✅ — ở giai đoạn grow chạy song song ba nhóm campaign với ba mục tiêu tách bạch.)*

| Vai campaign | Nhiệm vụ | Đo bằng | KHÔNG đo bằng |
|---|---|---|---|
| **Chạy đà / TOFU** | Giữ đà, nuôi rank | Có tiêu được tiền không · rank có nhích không | **Không** chịu KPI ra đơn |
| **Mũi nhọn** | Khai thác rank đã có | Rank giữ được không · đơn từ từ khoá mục tiêu | — |
| **Performance** | Ra lợi nhuận | **Lợi nhuận**, không phải rank | — |

Kỳ vọng ROAS của nhóm chạy đà **thấp hơn** nhóm khai thác — đây là thiết kế, không phải bệnh. Chấm
chung một ngưỡng sẽ kết luận nhầm rằng nhóm chạy đà đang kém.

**Cách xác định vai:** đọc từ **dữ liệu**, không đọc từ tên campaign. Ở hệ vận hành nội bộ đã đo được
hơn hai phần ba số campaign mang vai sai so với tên. Không suy vai từ sự **vắng mặt** của dữ liệu —
nguồn chỉ chứng minh được điều gì **có**, không chứng minh được điều gì **không có**.

Không xác định được vai → **hỏi người dùng**, đừng gán.

## Ba loại quảng cáo cũng là ba thước khác nhau

`Type` trong File A có ba giá trị: `SP` · `SB2` (Sponsored Brands, gồm cả bản video) · `SD`. Đây là
**ba tầng khác nhau của cùng một phễu**, không phải ba biến thể của một thứ.

- **Không cộng gộp** spend/đơn của ba loại rồi tính một ROAS chung để phán.
- **Không áp ngưỡng cắt của SP cho SB2 hoặc SD.** Ngưỡng người dùng đưa gần như luôn là ngưỡng nghĩ
  cho SP; dùng lại cho SB2/SD là chấm bài bằng thước của loại khác.
- Chưa có ngưỡng riêng cho SB2/SD → **trình bày vị trí, không kết luận cắt**: nêu số tiền, nêu tỉ
  trọng trên tổng spend, gọi đúng tên là **điểm nóng cần người xem**, rồi dừng.

Ca thật đã gặp: hai campaign Sponsored Brands Video chiếm 60% tiền tiêu của cả cửa sổ mà chưa ra đơn.
Kết luận đúng là "điểm nóng, chưa được kết luận cắt", **không** phải "cắt hai campaign này".

---

# PHẦN V — SÁU NHÁNH NGUYÊN NHÂN

Mỗi nhánh nêu cơ chế và ranh giới. Ngưỡng nằm ở `kb_03_thresholds.md`.

Mức tin cậy căn cứ: **📕 Chuẩn** · **✅ Đã kiểm chứng** · **🧪 Giả thuyết** (*không được dùng để kết
luận*, chỉ nêu như hướng cần kiểm).

### Nhánh 1 — Chưa lên đủ campaign nền · tầng C1

**Cơ chế:** thiếu loại hoặc thiếu số lượng campaign thì ASIN không có đủ cửa để hiển thị. Mọi chỉ số
sau đó đo trên nền chưa dựng xong.

**Căn cứ:** *"lên đúng và đủ"* — không đánh giá, không loại bỏ một ASIN cho tới khi bộ campaign đã
lên đúng loại và đủ số lượng (📕, xác nhận 5 lần). Và: *"từ 0 lên CÓ" có impact lớn hơn "từ 5–6 lên
7–8"* — audit chỗ còn thiếu trước khi tinh chỉnh cái đang chạy (✅).

**Ranh giới với nhánh 2:** nhánh 1 là **chưa có campaign**; nhánh 2 là **có mà không tiêu được tiền**.

### Nhánh 2 — Spend không tiêu được · tầng C1

**Cơ chế:** bid hoặc budget không đủ thắng phiên đấu giá, tiền không ra được, ASIN không có traffic.

**Căn cứ:** lên campaign xong **phải tăng bid tới khi thực sự có spend** (✅). Muốn tăng spend mà
không nâng nền bid thì bổ sung campaign video để mở thêm vị trí hiển thị (✅). Campaign exact "lên
rồi mà không tiêu" vì ít từ hoặc volume thấp thì mở broad + phrase để giải nghẽn (🧪).

**Lưu ý về công cụ:** rule tự động **bất đối xứng** — chỉ tăng bid khi campaign không tiêu, không
chủ động hạ khi tiêu quá đắt (✅). Nên "không tiêu được" thường tự hồi, còn "tiêu quá đắt" thì không.

### Nhánh 3 — Spend rải rác / target lệch intent · tầng C3–PL

**Cơ chế:** ngân sách chia mỏng nhiều hướng, không hướng nào đủ lực chiếm rank; hoặc ads kéo về
truy vấn không cùng ý định mua.

**Tiêu chí là PATTERN PHÂN BỔ SPEND, không phải số target:** tiêu **tập trung** vào nhóm có kết quả
= khoẻ; tiêu **rải rác** nhiều campaign mà **không ra đơn** = bệnh (✅).

**Số target là bối cảnh, không phải tiêu chí** *(quyết định 16/08/2026)*. Trước đây có chuẩn "1
campaign = 1 target" khi vận hành bằng rule, lý do gốc là rule chỉ chỉnh được vài target mỗi ngày.
Nay 100% campaign nằm dưới rule nên lý do đó hết. Chỉ nêu số target khi **đi kèm** dấu hiệu spend
rải rác không ra đơn.

**Ranh giới với nhánh 4:** nhánh 3 làm ASIN **không lên được rank**; nhánh 4 làm ASIN **có traffic
mà không chốt được đơn**. Dùng C3.2 để phân định.

### Nhánh 4 — Listing và tỉ lệ chuyển đổi · tầng C3

**Cơ chế:** traffic vào không thành đơn. Nặng hơn: tỉ lệ chuyển đổi tổng là biến điều khiển bánh đà
phân phối của Amazon — traffic rác đổ vào mẫu số kéo hiển thị của **cả ASIN** xuống (✅).

**Căn cứ:** CVR thấp là bài toán **tầng ASIN** (giá / listing / cách hiển thị), **không phải lỗi
campaign** — sửa nó trước khi động vào bid (✅). Đừng đánh giá campaign bằng số **tổng** của ASIN:
tổng đẹp có thể do organic gánh (✅).

**Bảo trì định kỳ:** tối ưu lại listing tối thiểu một quý một lần, không chỉ sửa khi có sự cố — thuật
toán đổi làm listing cũ mất đa dạng từ khoá (🧪).

### Nhánh 5 — Giá và vị thế cạnh tranh · tầng C2–C3

**Chưa có nguồn dữ liệu.** Tín hiệu gián tiếp: thứ hạng quảng cáo lên mà thứ hạng tự nhiên không lên
→ ASIN không cạnh tranh nổi ở ngách (✅).

> **BẮT BUỘC đối chiếu CVR trước khi dùng tín hiệu này.** Entry gốc nêu tiêu chí đầy đủ là *so CR của
> mình với CR thị trường*.
>
> | Rank | CVR | Đọc là |
> |---|---|---|
> | Sponsor lên, organic không lên | **tốt** | **Không** phải không cạnh tranh nổi — chỉ là chưa chiếm được rank tự nhiên |
> | Sponsor lên, organic không lên | **kém** | Đúng dấu hiệu → nhánh 5 |
>
> Bỏ vế CVR thì node này báo động sai ở **mọi** ASIN đang trong giai đoạn đẩy rank.

**Thứ tự đòn bẩy:** ASIN chưa lên rank thì quy hoạch lại target và nền bid **trước**, hạ giá là đòn
**cuối** — giá không bị cấm, chỉ bị xếp sau (✅).

### Nhánh 6 — Tồn kho · ràng buộc ngoài phễu

**Chưa có nguồn dữ liệu.** Nhưng là **ràng buộc phải soi trước khi tiêu tiền ads** (✅, xác nhận 3
lần) — vì nó **đổi nghĩa** mọi kết luận: sắp hết hàng thì "ROAS thấp" không còn là bệnh; tồn quá lớn
thì mục tiêu chuyển sang giải phóng tồn.

Không có dữ liệu → ghi rõ *"mọi kết luận giả định tồn kho bình thường, giả định này chưa được kiểm"*.

---

# PHẦN VI — CẢNH BÁO CHUẨN BỊ MÙA VỤ

**Không phải nhánh nguyên nhân.** Phễu trả lời *"vì sao tụt"*, mục này trả lời *"có kịp chuẩn bị cho
mùa không"* — đọc được từ cùng dữ liệu nên không bỏ qua.

**Điều kiện chạy:** có File B. **Kiểm:** ASIN có ít nhất một keyword vào organic Top 10 chưa?

*(📕: trước event mà ASIN chưa có từ khoá nào vào organic Top 10 thì rất khó khai thác mùa — điều
kiện **CẦN**. Entry ghi rõ **không có mốc thời gian cụ thể** nên bot **không được bịa ra mốc**.)*

Bot **không tự suy đang gần mùa nào** — hỏi, hoặc chỉ nêu tình trạng rank để người dùng tự đối chiếu.

Trong event thì cách đánh khác hẳn: ASIN đã có lịch sử index thì **đừng đấu từ khoá**, thắng bằng
nghiên cứu đối thủ và điều chỉnh giá theo ngày (✅).

---

# PHẦN VII — BẢY BẪY CHẨN ĐOÁN

Những cách kết luận **đã biết là sai**:

1. **Kết luận từ số tổng của ASIN** — tổng đẹp có thể do organic gánh (✅).
2. **Coi CVR thấp là lỗi campaign** — nó là bài toán tầng ASIN (✅).
3. **Lấy ROAS làm thước chính khi ASIN đang đẩy đà** — ở giai đoạn này ROAS chỉ là chặn dưới, thước
   chính là rank và tốc độ ra đơn (✅).
4. **Mừng vì tỉ trọng organic tăng** — phải kiểm **tổng đơn tuyệt đối**; tỉ trọng tăng có thể chỉ vì
   đơn ads sụt (✅).
5. **Coi "CPC rẻ" là lý do giữ lại** — target CPC thấp mà nhiều click không ra đơn là nhóm nên tắt
   trước (✅).
6. **Coi performance suy giảm theo thời gian là lỗi vận hành** — đó là quy luật thuật toán (✅).
7. **Trình bày việc sửa lỗi kỹ thuật như giải pháp cải thiện hiệu suất** — sửa lỗi chỉ trả nợ về mốc
   bình thường (✅).

---

# PHẦN VIII — ĐƯỜNG RA VÀ DẠNG KẾT LUẬN

## Đường ra khi không tầng nào bất thường

| Tình huống | Kết luận |
|---|---|
| Mọi tầng **có dữ liệu** đều sạch, còn nhánh thiếu dữ liệu chưa loại trừ | *"Không tìm thấy nguyên nhân trong phạm vi dữ liệu hiện có. Chưa loại trừ: […]. Cần: […]"* |
| Sạch và đã có đủ 4 nguồn | *"Trong phạm vi ads, cấu trúc và chuyển đổi đều bình thường. Vấn đề nhiều khả năng nằm ngoài phạm vi công cụ — giá, tồn kho, hoặc thị trường"* |
| Mẫu quá mỏng | *"Mẫu quá nhỏ để kết luận"* — kèm số click và số ngày cụ thể |

**Cấm** kết luận "ASIN ổn" khi mới soi được một phần các nhánh.

## Dạng kết luận

```
NGUYÊN NHÂN GỐC: <nhánh> — nghẽn ở tầng <C1/C2/C3/PL>
  bằng chứng: <số cụ thể, ghi nguồn>
  nhãn: đã loại trừ đủ / kết luận tạm / chưa kết luận được
NGUYÊN NHÂN PHỤ THUỘC: <nhánh> — quan hệ: hệ quả của / cùng gốc / độc lập
CHƯA LOẠI TRỪ: <nhánh> — thiếu <dữ liệu>
```

Nêu hai nguyên nhân thì **bắt buộc nói rõ quan hệ**. Liệt kê song song như hai khả năng ngang nhau
chính là kiểu "danh sách khả năng" mà công cụ này sinh ra để tránh.

---

# PHẦN IX — KHI HAI NGUỒN NÓI NGƯỢC NHAU

1. So **mức tin cậy**: 📕 thắng ✅, ✅ thắng 🧪. Entry 🧪 không bao giờ bác được entry 📕.
2. Cùng mức → tìm **điều kiện áp dụng** của từng bên. Phần lớn "mâu thuẫn" thực ra là hai luật cho
   hai bối cảnh khác nhau.
3. Vẫn không phân định → **nêu cả hai** kèm điều kiện, nói rõ cần người có nghiệp vụ quyết.

Entry **hết hiệu lực** không dùng làm căn cứ, kể cả khi là 📕.

**Kho knowhow KHÔNG phải nguồn chuẩn cho ngưỡng số.** Thứ tự: ngưỡng per-ASIN do người dùng cung cấp
→ mốc chung theo giai đoạn vòng đời → kho (chỉ để đối chiếu). Đã có ca thật: một bảng ngưỡng trong
kho bị thay thế nhưng vẫn mang nhãn 📕 và **để trống ngày hết hiệu lực**, nên không ai biết nó cũ.
