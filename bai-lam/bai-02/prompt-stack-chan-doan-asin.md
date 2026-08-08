# Assignment Buổi 2 — Prompt Stack V1: Xây cây chẩn đoán ASIN

**Học viên:** Bùi Ngọc Bình
**Framing Brief nguồn:** `bai-lam/bai-01/framing-brief-chan-doan-asin.md` (Buổi 1)
**Domain project xuyên khóa:** Vận hành hiệu suất ASIN (chẩn đoán → xử lý → theo dõi).
Scope Buổi 1–3 chỉ nằm ở **nhánh chẩn đoán**.

---

## Ghi chú về loại stack

Stack trong bài này là **stack build-time**: dùng AI để **xây ra** cây chẩn đoán.
Nó không phải prompt mà nhân sự MKT gõ hằng ngày để soi một ASIN — bản đó là sản phẩm cuối
(master instruction của một trợ lý nội bộ), sẽ dựng ở các buổi sau khi đã có cây được kiểm chứng.

Phân biệt này quyết định cách bóc task: nếu viết stack run-time thì bài toán bản chất chỉ là
**một** task, ép thành 6 bước sẽ thành các step vụn không có artifact riêng.

---

# Phần A — Prompt ban đầu

Prompt dưới đây tôi chưa gửi nguyên văn. Nó hợp nhất đúng cách tôi đã thật sự tiếp cận ở hai prompt
đã audit ở Buổi 1 — prompt 09/02/2026 (nhân sự quản 40-60 ASINs, quá tải) và prompt 14/07/2026
(nhân sự loay hoay dù đã đào tạo; tôi phải tự vẽ mindmap rồi follow up 2 tuần). Cùng audience,
cùng cơn đau, cùng kiểu kết bằng động từ mở.

```text
Hiện tại nhân sự MKT của tôi mỗi bạn đang quản lý 40-60 ASINs trên Amazon.
Khi một ASIN hiệu quả không tốt như tụt doanh số hoặc ROAS xuống thấp thì các
bạn khá loay hoay, dù tôi đã đào tạo khá kỹ rồi để các bạn nắm knowhow vận hành
của công ty nhưng khi vào trường hợp thực tế thì các bạn chưa ứng dụng thành
thạo. Vừa rồi tôi phải tự làm 1 mindmap phân tích chi tiết rồi follow up liên
tục 2 tuần thì mới ổn ở trong việc đẩy chiếm rank từ khóa top 10 cho các sản phẩm.

Bạn hãy đứng trên vai trò chuyên gia vận hành Amazon FBA hàng đầu phân tích và
xây dựng cho tôi một quy trình hoặc checklist chẩn đoán đầy đủ: các nguyên nhân
có thể khiến một ASIN tụt, chỉ số cần xem và ngưỡng cảnh báo của từng chỉ số,
thứ tự kiểm tra, cách xử lý tương ứng với mỗi nguyên nhân, và tài liệu hướng dẫn
để tôi đào tạo lại cho team dựa trên kho knowhow của công ty trong Notion và
nguồn best practice bạn tổng hợp được. Lưu ý ưu tiên nguồn knowhow nội bộ trước.

Bạn có thể hỏi thêm nếu cần thông tin.
```

---

# Phần B — RTC-COE Audit

## B.1. Soi theo sáu thành phần

| Thành phần | Prompt hiện tại | Vấn đề | Cách sửa |
|---|---|---|---|
| **Role** | "chuyên gia vận hành Amazon FBA **hàng đầu**" | Có Role nhưng phóng đại. "Hàng đầu" không cung cấp thông tin nào để AI xử lý khác đi — đúng Lỗi 2 của bài. | Chỉ định **góc nhìn** thay vì cấp bậc: "người đã xây quy trình chẩn đoán cho đội vận hành nhiều ASIN cùng lúc". Góc nhìn này mới ép AI ưu tiên tốc độ soi và khả năng giao lại cho người khác. |
| **Task** | "phân tích và xây dựng" + 5 đầu ra | Trộn 5–6 nhiệm vụ khác bản chất trong một lượt. Chi tiết ở B.2. | Tách thành stack; mỗi step một nhiệm vụ nhận thức. |
| **Context** | Quy mô 40-60 ASIN/người · đã đào tạo nhưng không ứng dụng được · ca thật mindmap + 2 tuần follow up · có trỏ nguồn knowhow nội bộ | **Thành phần mạnh nhất của prompt.** Có quy mô, có bằng chứng đã thử và thất bại, có một ca cụ thể, và có ý thức chỉ nguồn. Điểm trừ duy nhất: nguồn trỏ tới cả kho chứ chưa tới đúng page — xem B.4. | Nêu tên page/database cụ thể; bổ sung nhân sự đang có sẵn công cụ nào, dữ liệu lấy ở đâu, và "tụt" nghĩa là giảm bao nhiêu trong bao lâu. |
| **Constraints** | "ưu tiên nguồn knowhow nội bộ trước" | Có đúng **một** constraint và nó kiểm tra được — điểm cộng thật. Nhưng thiếu trần số nguyên nhân, thiếu luật xử lý khi nguồn nội bộ mâu thuẫn với best practice ngoài, thiếu luật khi không có dữ liệu. | Thêm: tối đa 5 nhánh nguyên nhân; khi hai nguồn mâu thuẫn thì nêu cả hai và đánh dấu, không tự chọn; không có căn cứ thì để trống và ghi rõ, không suy đoán. |
| **Output** | "một quy trình **hoặc** checklist" + "tài liệu hướng dẫn để đào tạo lại" | Chữ "hoặc" giao quyền chọn dạng sản phẩm cho AI. Nặng hơn: đang xin **hai artifact cho hai audience khác nhau** trong cùng một lượt. | Gọi đúng tên artifact đã chốt ở Buổi 1: **cây quyết định** có thứ tự soi và ngưỡng. Tài liệu đào tạo là sản phẩm khác, để sau. |
| **Evaluation** | Không có | Không có bất kỳ tiêu chí nào để biết kết quả đạt hay chưa. Câu kết "Bạn có thể hỏi thêm nếu cần" đẩy việc kiểm tra sang phía AI. | Mỗi step có tiêu chí tự kiểm riêng, và checkpoint do tôi quyết trước khi đi tiếp. |

## B.2. Sáu nhiệm vụ đang bị trộn

| # | Nhiệm vụ ẩn trong prompt | Loại tư duy | Cần gì để làm được |
|---|---|---|---|
| 1 | Rút các nguyên nhân có thể khiến ASIN tụt | Analyze | Kho knowhow nội bộ + nguồn ngoài |
| 2 | Chọn chỉ số quan sát cho từng nguyên nhân | Design | Biết nhân sự đọc được số ở đâu |
| 3 | Đặt ngưỡng cảnh báo cho từng chỉ số | Design | **Dữ liệu lịch sử thật** — AI không tự có |
| 4 | Sắp thứ tự kiểm tra | Design | Logic chi phí/tốc độ soi, cần xong 1–3 trước |
| 5 | Đề xuất cách xử lý theo từng nguyên nhân | Design | **Nằm ngoài scope đã chốt ở Buổi 1** |
| 6 | Soạn tài liệu đào tạo lại cho team | Design | Audience khác hẳn, cần cây xong trước |

Nhiệm vụ 3 là chỗ nguy hiểm nhất: nếu chạy chung một lượt, AI sẽ điền ngưỡng theo mặt bằng chung
của ngành chứ không theo dữ liệu của công ty tôi — mà nhân sự lại tưởng đó là ngưỡng nội bộ.

## B.3. Đối chiếu ngược với Framing Brief Buổi 1

Đây là phần đáng giá nhất của audit: prompt không chỉ thiếu thành phần, nó **vi phạm chính cái
scope tôi đã chốt ở buổi trước**.

| Buổi 1 đã chốt | Prompt Phần A | Hệ quả |
|---|---|---|
| Artifact = **cây quyết định** có thứ tự soi và ngưỡng | "quy trình hoặc checklist" | Mất tên artifact đã giành được ở Buổi 1 |
| **Tối đa 5** vấn đề trọng tâm | "các nguyên nhân có thể" | Không có trần → nhận về danh sách dài, nhân sự vẫn không biết soi đâu trước |
| Chạy hết cây ra **một kết luận nguyên nhân**, không phải danh sách khả năng | Không nêu | Mất tiêu chí nghiệm thu quan trọng nhất |
| Ngoài phạm vi: **chưa đề xuất cách sửa** — đó là phase 2 (DESIGN) | "cách xử lý tương ứng với mỗi nguyên nhân" | Kéo nguyên phase 2 vào, làm loãng phần chẩn đoán |
| Audience = nhân sự MKT vận hành | Trộn 2 audience: nhân sự đang soi + tôi đang giảng lại | Một artifact không phục vụ tốt cho ai |

## B.4. Chỉ nguồn nhưng chưa định vị được nguồn

Cụm *"dựa trên kho knowhow của công ty trong Notion"*.

Về mặt kỹ thuật, câu này chạy được: workspace Notion của tôi đã kết nối với AI qua MCP và CLI, nên
AI đọc được thật. Vấn đề nằm chỗ khác — **workspace có nhiều hệ thống, còn câu lệnh chỉ trỏ tới cả
cái kho.** Tôi viết ngắn vì ngầm giả định AI đã trao đổi với tôi nhiều lần nên biết lấy ở đâu.

Giả định đó chỉ đúng trong một phiên đang có ngữ cảnh. Prompt này là prompt one-shot, nên nó rơi vào
đúng chỗ tôi đã tự chấm ở Buổi 1: prompt 14/07/2026 là ô **duy nhất** trong bảng 4 prompt được chấm
**"Context: Rõ"**, với lý do ghi nguyên văn *"điểm mạnh: chỉ đúng ba nguồn cần đọc"*. So với chính
mình bảy tháng trước, prompt lần này **lùi một bước** ở đúng thành phần tôi từng làm tốt nhất.

Hai hệ quả cụ thể:

1. **Không kiểm được AI đã đọc gì.** Không nêu nguồn thì output trả về không truy ngược được — tôi
   không biết một nguyên nhân đến từ knowhow công ty hay từ kiến thức chung, trong khi chính prompt
   đặt luật "ưu tiên nội bộ trước".
2. **Hỏng im lặng khi đổi môi trường.** Nếu kết nối Notion lỗi, hoặc tôi gửi prompt này qua một AI
   khác không có MCP, AI vẫn trả về đủ bảng — chỉ là làm bằng kiến thức ngoài. Không có gì báo cho
   tôi biết luật ưu tiên đã bị bỏ qua.

Sửa theo ba lớp, không phải bỏ Notion đi:

- **Trỏ đúng nguồn:** nêu tên database cụ thể thay vì "kho knowhow", đúng như tôi đã làm ở
  prompt 14/07.
- **Trỏ đúng trường trong nguồn:** đây là lớp tôi chưa từng nghĩ tới. Database knowhow của tôi có
  sẵn các trường phân loại độ tin cậy và phạm vi áp dụng — nhưng prompt không dùng đến chúng, nên AI
  sẽ đọc mọi dòng như nhau. Chi tiết ở B.6.
- **Bắt khai nguồn:** yêu cầu AI liệt kê chính xác những gì đã đọc được, và **dừng lại báo** nếu
  không mở được nguồn nội bộ, thay vì lặng lẽ làm bằng kiến thức chung.

## B.6. Nguồn nội bộ đã có sẵn cấu trúc, nhưng prompt không dùng

Kho knowhow của tôi không phải một đống ghi chép phẳng. Mỗi entry đã được gắn nhãn theo bốn chiều
mà bài toán chẩn đoán cần đến:

| Chiều phân loại | Các mức | Vì sao prompt phải dùng |
|---|---|---|
| **Độ tin cậy** | giả thuyết (n=1) · đã kiểm chứng · chuẩn (đã duyệt) · đã bác bỏ | Entry giả thuyết ghi rõ *"chưa có số, cấm áp đồng loạt"*. Nếu prompt không lọc, một giả thuyết chưa kiểm chứng sẽ nằm trong cây và nhân sự tưởng là luật công ty |
| **Loại tri thức** | ngưỡng có số · quy trình · nguyên lý · anti-pattern · quản trị | Nhóm "ngưỡng có số" là nguồn ngưỡng duy nhất đáng tin cho bước đặt ngưỡng; nhóm "anti-pattern" là các bẫy chẩn đoán sai |
| **Còn hiệu lực hay không** | có khoảng thời gian hiệu lực + ngày rà tiếp | Knowhow hết hạn vẫn nằm trong kho; đọc nhầm là chẩn đoán theo luật cũ |
| **Cờ nhạy cảm** | bật khi entry chứa thông tin business của khách/học viên | Phải loại trừ, nhất là khi kết quả sẽ được đóng gói lại để đào tạo |

Prompt ở Phần A viết "dựa trên kho knowhow của công ty" — tức yêu cầu AI đọc **toàn bộ**, không phân
biệt bốn chiều trên. Hệ quả nặng nhất: **một giả thuyết chưa kiểm chứng và một quy tắc đã được duyệt
sẽ vào cây với cùng trọng số.** Với artifact mà nhân sự dùng để kết luận nguyên nhân, đây là lỗi
nghiêm trọng hơn cả việc thiếu Evaluation.

Đây cũng là điều tôi rút ra lớn nhất khi làm bài này: **Context Framing không dừng ở "chỉ đúng
nguồn", mà là "chỉ đúng phần nào trong nguồn được phép dùng làm căn cứ".**

## B.5. Vì sao một prompt duy nhất chưa phù hợp

Ba lý do, theo thứ tự nặng dần:

1. **Sáu nhiệm vụ khác loại tư duy trong một lượt** → AI chạm nhẹ mỗi phần, không phần nào đủ dùng.
2. **Không có checkpoint** → nếu danh sách nguyên nhân ở bước đầu đã thiếu hoặc trùng lặp, mọi
   ngưỡng và thứ tự soi phía sau đều xây trên nền sai, mà tôi không có chỗ nào để phát hiện.
3. **Nhiệm vụ 3 cần dữ liệu tôi chưa cung cấp** → prompt một lượt buộc AI phải bịa ngưỡng để hoàn
   thành đầu ra. Với bài toán này, một ngưỡng sai im lặng nguy hiểm hơn một câu trả lời bỏ trống:
   nhân sự sẽ soi theo nó và kết luận sai nguyên nhân.

---

# Phần C — Task Decomposition

## C.1. Task Map

| Step | Task | Câu hỏi chính | Input | Expected output | Phụ thuộc |
|---:|---|---|---|---|---|
| 1 | Rút danh sách nguyên nhân | Một ASIN có thể tụt vì những nguyên nhân nào? | Framing Brief + trích knowhow nội bộ | **Cause Inventory** (có ghi nguồn) | — |
| 2 | Gom và cắt còn tối đa 5 nhánh | Năm nhánh nào đáng soi nhất và không chồng nhau? | Cause Inventory | **Cause Shortlist** | Step 1 |
| 3 | Gán chỉ số quan sát cho mỗi nhánh | Nhìn số nào, đọc ở đâu thì biết nhánh đó có vấn đề? | Cause Shortlist | **Signal Map** | Step 2 |
| 4 | Đặt ngưỡng cho từng chỉ số | Đến mức nào thì coi là bất thường? | Signal Map + dữ liệu lịch sử | **Threshold Table** | Step 3 |
| 5 | Sắp thứ tự soi và dựng cây | Soi nhánh nào trước thì loại trừ được nhiều nhất với công sức ít nhất? | Threshold Table | **Decision Tree v1** | Step 4 |
| 6 | Test và sửa | Cây có chạy đúng trên ca thật không? | Decision Tree v1 + 3 ASIN thật | **Reviewed Decision Tree** | Step 5 |

## C.2. Dependency

```text
Cause Inventory
→ Cause Shortlist
→ Signal Map
→ Threshold Table
→ Decision Tree v1
→ Reviewed Decision Tree
```

Ba quan hệ bắt buộc, không đảo được:

- **Chưa cắt còn 5 nhánh thì chưa gán chỉ số.** Gán chỉ số cho 15 nguyên nhân là công vô ích, vì
  hơn nửa sẽ bị loại ở bước sau.
- **Chưa có chỉ số thì chưa đặt được ngưỡng.** Ngưỡng luôn là ngưỡng *của một chỉ số cụ thể*.
- **Chưa có ngưỡng thì chưa sắp được thứ tự soi.** Thứ tự soi phụ thuộc vào việc nhánh nào loại trừ
  được nhiều nhất và rẻ nhất — chỉ biết được khi đã biết phải nhìn số gì.

## C.3. Vì sao dừng ở 6 step

Step 1 và 2 tách riêng vì khác bản chất: step 1 là **thu thập** (đọc nguồn, càng đủ càng tốt),
step 2 là **cắt bỏ** (ra quyết định ưu tiên, càng gọn càng tốt). Trộn hai việc ngược chiều nhau vào
một lượt thì AI sẽ tự kiểm duyệt lúc liệt kê và bỏ sót nguyên nhân hiếm.

Step 5 gộp "sắp thứ tự" và "dựng cây" vì cả hai cùng tạo ra **một** artifact. Tách ra sẽ thành step
vụn không có đầu ra riêng.

Phần "cách xử lý theo từng nguyên nhân" và "tài liệu đào tạo lại cho team" trong prompt gốc **không
nằm trong stack này** — đúng scope đã chốt ở Buổi 1. Chúng là phase 2, và chỉ chạy được khi cây
chẩn đoán đã qua test.

---

# Phần D — Prompt Stack V1

## Step 1 — Rút danh sách nguyên nhân

**Purpose:** Mở rộng hết mức các nguyên nhân có thể, trước khi cắt. Đây là bước duy nhất được phép
"rộng"; các bước sau đều thu hẹp.

**Input:** Framing Brief Buổi 1 + knowhow nội bộ. Nguồn nội bộ đưa vào theo một trong hai cách —
trỏ tên page/database cụ thể nếu chạy trong môi trường có kết nối Notion, hoặc dán trích nếu chạy ở
môi trường không có kết nối. Cả hai cách đều phải nêu **tên nguồn cụ thể**, không nói "kho knowhow".

```text
[Role]
Đóng vai người đã xây quy trình chẩn đoán cho đội vận hành phải theo nhiều ASIN
cùng lúc trên Amazon.

[Task]
Rút ra danh sách các nguyên nhân có thể khiến một ASIN tụt doanh số hoặc tụt ROAS.

[Context]
- Người sẽ dùng kết quả: nhân sự MKT vận hành, mỗi người theo 40-60 ASIN.
- Họ đã được đào tạo knowhow nội bộ nhưng chưa áp dụng được vào ca cụ thể.
- Kết quả bước này sẽ được cắt còn tối đa 5 nhánh ở bước sau, nên ở bước này cần
  đủ trước, gọn sau.

[Constraints]
- TRƯỚC KHI LÀM: mở database knowhow nội bộ nêu bên dưới và báo lại số entry đọc
  được, phân theo trạng thái tin cậy. Nếu KHÔNG mở được, DỪNG LẠI và báo cho tôi.
  Không tự làm tiếp bằng kiến thức chung — làm vậy là phá luật ưu tiên nội bộ mà
  tôi không biết.

- Lọc theo trạng thái tin cậy của từng entry:
  · CHUẨN (đã duyệt) và ĐÃ KIỂM CHỨNG → dùng làm căn cứ chính;
  · GIẢ THUYẾT (n=1, chưa có số) → liệt kê ở mục riêng, đánh dấu CHƯA KIỂM CHỨNG.
    KHÔNG trộn vào danh sách chính;
  · ĐÃ BÁC BỎ → không dùng làm nguyên nhân, nhưng đọc và tóm tắt riêng để biết
    cái gì đã thử và sai.

- Bỏ qua entry đã hết hiệu lực. Với entry còn nhãn vòng đời cũ (legacy), quy đổi
  sang hệ vòng đời hiện hành và ghi rõ đã quy đổi.
- Bỏ qua mọi entry được đánh cờ nhạy cảm.
- Đọc kỹ nhóm ANTI-PATTERN: đây là các cách làm đã biết là sai, dùng để cảnh báo
  bẫy chẩn đoán, không phải để làm nguyên nhân.
- Mỗi nguyên nhân ghi rõ nguồn: NỘI BỘ (kèm tên entry) hay NGOÀI (kiến thức chung).
- Ưu tiên liệt kê hết từ nguồn NỘI BỘ trước, rồi mới bổ sung nguồn NGOÀI.
- Khi knowhow nội bộ mâu thuẫn với kiến thức chung, nêu cả hai và đánh dấu MÂU THUẪN.
  Không tự chọn bên nào.
- Không đề xuất cách xử lý ở bước này.
- Không gộp hai nguyên nhân khác cơ chế thành một dòng.

[Output]
1. Báo cáo nguồn: số entry đọc được, tách theo trạng thái tin cậy; entry bị loại vì
   hết hiệu lực hoặc có cờ nhạy cảm.
2. Bảng nguyên nhân (chỉ từ entry CHUẨN / ĐÃ KIỂM CHỨNG + nguồn NGOÀI):
   a. Nguyên nhân
   b. Cơ chế (vì sao nó làm ASIN tụt)
   c. Nguồn (NỘI BỘ + tên entry / NGOÀI / MÂU THUẪN)
   d. Trạng thái tin cậy của entry nguồn
   e. Dấu hiệu quan sát được đầu tiên
   f. Mức phổ biến theo đánh giá của bạn (cao/trung bình/thấp) + lý do
3. Mục riêng — GIẢ THUYẾT CHƯA KIỂM CHỨNG: nguyên nhân + entry nguồn + cần số gì
   để nâng lên mức đã kiểm chứng.
4. Mục riêng — ĐÃ THỬ VÀ SAI: tóm tắt từ nhóm bác bỏ và anti-pattern.

[Evaluation]
Tự kiểm:
- có nguyên nhân nào thực chất là triệu chứng của nguyên nhân khác không;
- có entry CHUẨN nào chưa được phản ánh vào bảng không;
- có giả thuyết nào lọt vào bảng chính không — nếu có, chuyển xuống mục 3;
- tỉ lệ dòng NỘI BỘ so với NGOÀI có hợp lý không. Nếu gần như toàn NGOÀI thì khả
  năng cao bạn chưa đọc được nguồn nội bộ; báo lại thay vì tiếp tục;
- có nguyên nhân nào bạn suy đoán mà không có căn cứ không — nếu có, đánh dấu rõ.

Framing Brief:
[DÁN FRAMING BRIEF BUỔI 1]

Nguồn knowhow nội bộ:
[TÊN DATABASE KHO KNOWHOW VẬN HÀNH NỘI BỘ — hoặc dán trích nếu không có kết nối]
```

**Expected output:** Cause Inventory.

---

## Step 2 — Gom và cắt còn tối đa 5 nhánh

**Purpose:** Biến danh sách dài thành số nhánh mà một người soi được trong một ca làm việc. Đây là
bước ép prompt tuân thủ ràng buộc "tối đa 5" đã chốt ở Buổi 1.

**Input:** Cause Inventory từ Step 1.

```text
[Role]
Đóng vai người thiết kế quy trình vận hành, ưu tiên thứ dùng được hằng ngày hơn
thứ đầy đủ về lý thuyết.

[Task]
Gom Cause Inventory thành tối đa 5 nhánh nguyên nhân không chồng lấn.

[Context]
Năm nhánh này sẽ trở thành 5 nhánh chính của một cây quyết định mà nhân sự chạy
khi một ASIN tụt. Chạy hết cây phải ra được MỘT kết luận nguyên nhân.

[Constraints]
- Tối đa 5 nhánh. Không được 6.
- Hai nhánh bất kỳ không được cùng đúng trên một ca — nếu có thể cùng đúng, phải
  nêu rõ cách phân định.
- Mỗi nguyên nhân trong Cause Inventory phải được xếp vào đúng một nhánh, hoặc
  ghi vào mục LOẠI kèm lý do. Không được bỏ im lặng.
- Không tạo nhánh "khác" hoặc "nguyên nhân còn lại".

[Output]
1. Bảng 5 nhánh: tên nhánh · định nghĩa · các nguyên nhân thuộc về nó · ranh giới
   phân biệt với nhánh gần nhất.
2. Danh sách LOẠI: nguyên nhân bị bỏ + lý do.
3. Ghi chú về những chỗ ranh giới còn mờ.

[Evaluation]
Tự kiểm:
- năm nhánh có phủ được các ca thường gặp không;
- có cặp nhánh nào dễ bị nhầm khi soi thực tế không;
- danh sách LOẠI có bỏ sót nguyên nhân nghiêm trọng nào không.

Cause Inventory:
[DÁN OUTPUT STEP 1]
```

**Expected output:** Cause Shortlist.

**⛳ Checkpoint 1 — quyết định trước khi đi tiếp:**

- [ ] Đúng 5 nhánh hoặc ít hơn.
- [ ] Không có cặp nhánh nào có thể cùng đúng mà không phân định được.
- [ ] Mọi nguyên nhân bị loại đều có lý do, không có cái nào biến mất lặng lẽ.
- [ ] Năm nhánh này phủ được **ca mindmap 2 tuần** tôi đã tự xử lý — nếu ca đó không
      rơi vào nhánh nào thì quay lại Step 1, chưa được đi tiếp.

---

## Step 3 — Gán chỉ số quan sát cho mỗi nhánh

**Purpose:** Biến nhánh nguyên nhân (khái niệm) thành thứ nhân sự nhìn được trên màn hình.

**Input:** Cause Shortlist từ Step 2.

```text
[Role]
Đóng vai người vận hành Amazon quen đọc số trên Ads Manager và Seller Central.

[Task]
Với mỗi nhánh trong Cause Shortlist, xác định các chỉ số quan sát cho biết nhánh
đó đang có vấn đề.

[Context]
Người đọc là nhân sự MKT phải soi nhiều ASIN mỗi ngày, nên mỗi nhánh cần ít chỉ số
nhưng phải lấy được nhanh.

[Constraints]
- Mỗi nhánh tối đa 3 chỉ số.
- Mỗi chỉ số phải nêu rõ lấy ở đâu: màn hình/báo cáo nào, của công cụ nào.
- Chỉ dùng chỉ số nhân sự lấy được bằng công cụ đang có; không giả định công cụ mới.
- KHÔNG điền ngưỡng ở bước này — ngưỡng thuộc bước sau.
- Nếu một nhánh không có chỉ số nào quan sát trực tiếp được, ghi rõ "KHÔNG ĐO TRỰC
  TIẾP ĐƯỢC" và đề xuất dấu hiệu gián tiếp, thay vì bịa ra một chỉ số.

[Output]
Bảng gồm:
1. Nhánh
2. Chỉ số
3. Lấy ở đâu (công cụ + màn hình/báo cáo)
4. Chỉ số này tăng hay giảm thì đáng ngờ
5. Cửa sổ thời gian nên nhìn
6. Độ tin cậy của chỉ số (cao/trung bình/thấp) + lý do

[Evaluation]
Tự kiểm:
- có chỉ số nào dùng chung cho nhiều nhánh đến mức không phân biệt được nhánh nào không;
- có chỉ số nào nhân sự thực tế không lấy được không;
- nhánh nào đang yếu về khả năng quan sát nhất.

Cause Shortlist:
[DÁN OUTPUT STEP 2]
```

**Expected output:** Signal Map.

---

## Step 4 — Đặt ngưỡng cho từng chỉ số

**Purpose:** Cho nhân sự một mốc để quyết, thay vì cảm tính. Đây là bước phụ thuộc dữ liệu nặng nhất
và là bước dễ bị AI bịa nhất.

**Input:** Signal Map từ Step 3 + dữ liệu lịch sử của một nhóm ASIN.

```text
[Role]
Đóng vai người phân tích dữ liệu vận hành, có thói quen phân biệt rõ đâu là số đọc
được từ dữ liệu và đâu là ước đoán.

[Task]
Đề xuất ngưỡng cảnh báo cho từng chỉ số trong Signal Map.

[Context]
Ngưỡng này sẽ được nhân sự dùng để kết luận một ASIN có vấn đề ở nhánh nào. Kết luận
sai dẫn tới xử lý sai chỗ và mất thêm thời gian.

[Constraints]
- Thứ tự ưu tiên nguồn ngưỡng, không được đảo:
  1. Entry knowhow nội bộ thuộc loại NGƯỠNG CÓ SỐ, trạng thái CHUẨN hoặc ĐÃ KIỂM
     CHỨNG, và có trường bằng-chứng-số không trống;
  2. Dữ liệu lịch sử tôi dán bên dưới;
  3. Mặt bằng chung của ngành — chỉ khi hai nguồn trên không có, và phải gắn nhãn
     NGUỒN NGOÀI kèm lý do vì sao vẫn đáng tham khảo.
- Entry loại NGƯỠNG CÓ SỐ nhưng trạng thái GIẢ THUYẾT: ghi vào mục ỨNG VIÊN, KHÔNG
  đưa vào bảng ngưỡng chính.
- Chỉ số nào không có đủ căn cứ từ cả ba nguồn thì ghi "CHƯA ĐỦ DỮ LIỆU" và nêu cần
  thu thập gì. TUYỆT ĐỐI không điền một con số ước lượng vào ô đó.
- Mỗi ngưỡng lấy từ nội bộ phải ghi kèm version và khoảng hiệu lực của entry nguồn.
- Ngưỡng phải kèm cửa sổ thời gian; không có ngưỡng nào đứng một mình.

[Output]
Bảng gồm:
1. Nhánh
2. Chỉ số
3. Ngưỡng đề xuất
4. Cửa sổ thời gian
5. Nguồn của ngưỡng (NỘI BỘ + tên entry + version / DỮ LIỆU DÁN / NGOÀI / CHƯA ĐỦ DỮ LIỆU)
6. Độ chắc chắn + điều gì sẽ làm ngưỡng này sai

Kèm hai mục riêng:
- ỨNG VIÊN: ngưỡng từ entry còn ở mức giả thuyết, cần số gì để dùng được.
- THIẾU DỮ LIỆU: chỉ số chưa đặt được ngưỡng và cần thu thập gì.

[Evaluation]
Tự kiểm:
- có ô ngưỡng nào bạn điền mà không truy được về một entry hoặc một dòng dữ liệu không;
- có ngưỡng nào lấy từ entry giả thuyết mà lọt vào bảng chính không;
- có ngưỡng nào lấy từ entry đã hết hiệu lực không;
- có ngưỡng nào khiến gần như mọi ASIN đều báo động không;
- có ngưỡng nào chặt đến mức gần như không ASIN nào chạm không.

Signal Map:
[DÁN OUTPUT STEP 3]

Dữ liệu lịch sử:
[DÁN DỮ LIỆU HOẶC GHI: CHƯA CÓ]
```

**Expected output:** Threshold Table.

**⛳ Checkpoint 2 — quyết định trước khi đi tiếp:**

- [ ] Mọi ngưỡng đều truy được về nguồn; không ô nào là số không rõ từ đâu ra.
- [ ] Các ô "CHƯA ĐỦ DỮ LIỆU" được để trống thật, không bị điền đại.
- [ ] Số nhánh còn đủ ngưỡng để chẩn đoán được ≥ 3/5 — nếu ít hơn, dừng stack và đi
      thu thập dữ liệu trước, không dựng cây trên nền trống.
- [ ] Ngưỡng không lỏng quá (mọi ASIN đều đỏ) cũng không chặt quá (không ASIN nào đỏ).

---

## Step 5 — Sắp thứ tự soi và dựng cây quyết định

**Purpose:** Ra artifact chính của Buổi 1: cây quyết định có thứ tự soi và ngưỡng, chạy hết ra một
kết luận nguyên nhân.

**Input:** Threshold Table từ Step 4.

```text
[Role]
Đóng vai người thiết kế quy trình chẩn đoán, tối ưu cho việc loại trừ nhanh chứ
không cho việc đầy đủ.

[Task]
Sắp thứ tự kiểm tra 5 nhánh và dựng thành một cây quyết định hoàn chỉnh.

[Context]
Nhân sự chạy cây này khi phát hiện một ASIN tụt. Mục tiêu là ra được MỘT kết luận
nguyên nhân, không phải một danh sách khả năng.

[Constraints]
- Thứ tự soi ưu tiên nhánh loại trừ được nhiều nhất với công sức ít nhất; nêu rõ lý
  do xếp thứ tự cho từng nhánh.
- Mỗi node phải là một câu hỏi trả lời được bằng số trong Threshold Table.
- Mỗi nhánh rẽ phải dẫn tới hoặc một node tiếp theo, hoặc một kết luận. Không có
  nhánh cụt.
- Cây phải có đường ra cho trường hợp không nhánh nào chạm ngưỡng.
- Nhánh nào thiếu ngưỡng thì đặt ở cuối và ghi rõ là nhánh chưa đủ căn cứ.
- KHÔNG đề xuất cách xử lý sau khi ra kết luận. Cây dừng ở tên nguyên nhân.

[Output]
1. Bảng thứ tự soi: thứ tự · nhánh · lý do đặt ở vị trí này · chi phí soi (nhanh/vừa/chậm)
2. Cây quyết định dạng văn bản phân cấp, mỗi node là một câu hỏi kèm ngưỡng
3. Danh sách các kết luận nguyên nhân mà cây có thể ra
4. Đường xử lý cho ca "không nhánh nào chạm ngưỡng"

[Evaluation]
Tự kiểm:
- chạy thử cây trên một ASIN giả định có nhiều vấn đề cùng lúc — nó có ra được một
  kết luận duy nhất không;
- có nhánh cụt nào không;
- có câu hỏi nào nhân sự không tự trả lời được bằng số không.

Threshold Table:
[DÁN OUTPUT STEP 4]
```

**Expected output:** Decision Tree v1.

---

## Step 6 — Test trên ca thật và sửa

**Purpose:** Đánh giá cây bằng khả năng dẫn tới kết luận đúng, không bằng việc nó trông hợp lý.

**Input:** Decision Tree v1 + 3 ASIN thật đã biết nguyên nhân.

> Khi chạy, tôi dán mã ASIN thật và số liệu thật. Trong bản nộp này, ba ca được gọi là **ASIN A / B / C**
> — mã sản phẩm tra được công khai trên Amazon nên không đưa vào tài liệu công khai. Điều thầy chấm là
> cây có dẫn tới đúng kết luận hay không, không phụ thuộc vào mã sản phẩm.

```text
[Role]
Đóng vai reviewer độc lập, nhiệm vụ là tìm chỗ cây sai chứ không phải khen cây tốt.

[Task]
Chạy Decision Tree v1 trên 3 ca thật bên dưới và chỉ ra chỗ cây cần sửa.

[Context]
Ba ca này tôi đã biết nguyên nhân thật. Việc của bạn là xem cây có dẫn tới đúng
kết luận đó không, và nếu không thì hỏng ở node nào.

[Constraints]
- Với mỗi ca, đi từng node và ghi lại đường đi thực tế, không tóm tắt.
- Phân biệt rõ ba loại lỗi: sai ngưỡng · sai thứ tự soi · thiếu nhánh.
- Chỉ đề xuất sửa dựa trên bằng chứng từ 3 ca này, không sửa theo cảm giác chung.
- Không viết lại toàn bộ cây; chỉ nêu các chỉnh sửa cụ thể theo node.

[Output]
1. Ba bảng đường đi, mỗi ca: node · số thực tế · rẽ nhánh nào · kết luận cây đưa ra ·
   nguyên nhân thật · khớp hay không
2. Bảng lỗi: node · loại lỗi · bằng chứng · cách sửa
3. Danh sách 3-5 chỉnh sửa ưu tiên
4. Kết luận: dùng được / cần sửa trước khi giao cho nhân sự

[Evaluation]
Tự kiểm:
- mọi nhận xét có dẫn được về một ca cụ thể không;
- có lỗi nào bạn phát hiện nhưng chưa xếp được vào ba loại trên không.

Decision Tree v1:
[DÁN OUTPUT STEP 5]

Ba ca thật:
[DÁN MÃ ASIN + SỐ LIỆU + NGUYÊN NHÂN THẬT CỦA TỪNG CA]
```

**Expected output:** Reviewed Decision Tree.

**⛳ Checkpoint 3 — checkpoint cuối:**

- [ ] Cây ra đúng kết luận trên ít nhất 2/3 ca thật.
- [ ] Mọi lỗi phát hiện đều xếp được vào một trong ba loại (ngưỡng / thứ tự / thiếu nhánh).
- [ ] Không còn nhánh cụt.
- [ ] Nếu sai ở ≥ 2/3 ca: quay lại **Step 2** (nhánh chưa đúng), không phải Step 5.
      Sai kết luận thường là lỗi phân nhánh, không phải lỗi vẽ cây.

---

# Phần E — Test Log

## E.0. Điều kiện của lần chạy này

| | |
|---|---|
| Ngày chạy | 08/08/2026 |
| Nguồn nội bộ | Database knowhow vận hành, kết nối trực tiếp, **120 entry** |
| Ba ca thật | ASIN A · B · C, cùng một người vận hành |
| Ground truth | **Chốt TRƯỚC khi chạy cây.** Hai nguyên nhân do tôi xác định từ trước: (1) chưa đẩy spend nhóm campaign chạy đà đủ mạnh nên ASIN không lên được rank; (2) lên campaign 1-keyword nhưng nhồi nhiều target nên spend rải rác, không chiếm được rank của từ khóa nào, kéo ROAS xuống |
| Mức chạy được | Step 1–5 chạy đầy đủ trên nguồn thật. **Step 6 mới chạy ở mức đối chiếu logic**: so đường đi của cây với nguyên nhân đã biết, chưa chạy trên số liệu từng ASIN vì tôi chưa trích số tại thời điểm tụt |

> Ghi rõ giới hạn trên vì nó ảnh hưởng tới độ tin cậy của kết luận E.2: các lỗi tìm được là lỗi
> **logic của cây**, chưa phải lỗi **ngưỡng**. Ngưỡng chỉ kiểm được khi chạy trên số thật.

## E.1. Kết quả theo step

### Step 1 — Cause Inventory ✅ ra đúng artifact

Đọc được 120 entry. Phân bố theo trạng thái tin cậy: 27 chuẩn (đã duyệt) · 70 đã kiểm chứng ·
22 giả thuyết · 1 đã bác bỏ. Riêng nhóm **ngưỡng có số**: 18 entry ở mức dùng được, **8 entry còn ở
mức giả thuyết** — tức luật lọc có việc thật để làm, không phải luật cho vui.

Luật khai nguồn hoạt động đúng: AI báo số entry trước khi làm, nên tôi kiểm được nó thực sự đọc
nguồn nội bộ chứ không làm bằng kiến thức chung.

**🔴 Lỗi 1 — constraint "bỏ qua mọi entry có cờ nhạy cảm" quá tay.**
Chạy thật mới thấy **9 trên 18** entry ngưỡng dùng được đang bật cờ này. Loại hết là mất một nửa
nguồn ngưỡng, trong đó có các ngưỡng cốt lõi nhất về bid và về ROAS.
Nguyên nhân sai: tôi đọc cờ đó là *"không được dùng"*, trong khi ý nghĩa thật của nó là
*"không được xuất ra ngoài"*. Hai chuyện khác nhau — cây chẩn đoán dùng nội bộ thì vẫn được dùng.

### Step 2 — Cause Shortlist ⚠️ ra artifact nhưng vướng ràng buộc

**🔴 Lỗi 2 — trần "tối đa 5 nhánh" chật hơn thực tế.**
Các nhóm nguyên nhân tự nhiên tách thành **sáu**: chưa lên đủ bộ campaign nền · spend không tiêu
được · cấu trúc target sai · listing và tỉ lệ chuyển đổi · giá và vị thế cạnh tranh · tồn kho.
Ép xuống 5 buộc phải gộp hai nhóm khác cơ chế — vi phạm chính constraint "hai nhánh không được
cùng đúng trên một ca".

Trần 5 là ràng buộc tôi tự chốt ở Buổi 1. Test cho thấy nó đúng về tinh thần (nhân sự không soi nổi
15 nhánh) nhưng chật về số. **Đây là lần đầu một ràng buộc từ Buổi 1 bị chính dữ liệu phản bác.**

**🔴 Lỗi 3 — thiếu luật xử lý mâu thuẫn nội bộ ↔ nội bộ.**
Step 1 chỉ có luật cho mâu thuẫn *nội bộ ↔ kiến thức ngoài*. Chạy thật gặp hai entry **cùng ở mức
đã duyệt** nói ngược nhau về số target trên một campaign: một bên gom cụm nhiều keyword cùng nhóm,
một bên yêu cầu đúng một target khi vận hành bằng công cụ tự động. Không phải mâu thuẫn thật — là
hai nhánh có điều kiện áp dụng khác nhau — nhưng prompt không có chỗ nào bắt AI phát hiện và nêu
điều kiện.

Lỗi này nặng vì **chính nó là địa hạt của nguyên nhân thật số 2**: sai ở đây là chẩn đoán sai đúng
cái đang xảy ra.

### Step 3 — Signal Map ✅ ra đúng artifact

Không phát hiện lỗi. Ràng buộc "tối đa 3 chỉ số mỗi nhánh" và "không điền ngưỡng ở bước này" được
tuân thủ; không có hiện tượng bước này làm thay việc của Step 4.

### Step 4 — Threshold Table ✅ luật chống bịa hoạt động

Thứ tự ưu tiên nguồn ba bậc chạy đúng. Ngưỡng lấy được từ entry nội bộ đều truy được về tên entry và
version. Các chỉ số không có căn cứ được ghi "chưa đủ dữ liệu" thay vì điền số ước lượng — **luật
chống bịa đứng vững**, đây là điều tôi lo nhất trước khi chạy.

Tám entry ngưỡng ở mức giả thuyết đều rơi đúng vào mục ứng viên, không lọt vào bảng chính.

### Step 5 — Decision Tree v1 ❌ ra artifact nhưng sai logic

**🔴 Lỗi 4 — nặng nhất — thứ tự soi sắp theo tiêu chí sai.**

Constraint tôi viết: *"ưu tiên nhánh loại trừ được nhiều nhất với công sức ít nhất"*. Theo tiêu chí
này, cây xếp các nhánh đọc nhanh lên trước: tồn kho → listing và index → tỉ lệ chuyển đổi → rồi mới
tới cấu trúc campaign và phân bổ spend.

Đối chiếu với ground truth thì hỏng: **cả hai nguyên nhân thật đều nằm ở hai node cuối cùng.** Tệ
hơn, node "tỉ lệ chuyển đổi thấp" đứng trước sẽ **bắt tín hiệu và dừng cây sớm** — vì khi ASIN chưa
có rank thì tỉ lệ chuyển đổi thấp là chuyện đương nhiên. Cây sẽ kết luận "vấn đề ở listing" trong
khi nguyên nhân thật là chưa đẩy đủ spend để có rank.

Kho nội bộ đã có sẵn cảnh báo đúng cái bẫy này trong nhóm anti-pattern: *sửa lỗi kỹ thuật chỉ trả nợ
về mốc bình thường, đừng báo cáo nó như giải pháp cải thiện hiệu suất* — kèm một ca thật trong đó
nhân sự đề xuất sửa listing trước, trong khi hiệu suất đã kém từ nhiều tuần trước sự cố đó.

Và kho cũng có sẵn câu trả lời đúng, ở mức **chuẩn, 5 lần xác nhận**: *không đánh giá hay loại bỏ
một ASIN cho tới khi bộ campaign đã lên đúng loại và đủ số lượng.* Đây là **điều kiện tiên quyết**,
không phải một nhánh ngang hàng với các nhánh khác.

> Nói cách khác: tôi sắp cây theo **chi phí soi**, đúng về hiệu quả vận hành nhưng **sai về logic
> nhân quả**. Nguồn nội bộ đã có sẵn thứ tự đúng — stack chỉ đơn giản là không hỏi tới nó.

### Step 6 — đối chiếu với ca thật ❌ cây v1 không đạt

| Ca | Nguyên nhân thật | Cây v1 dẫn tới đâu | Khớp? |
|---|---|---|---|
| ASIN A | Chưa đẩy spend nhóm chạy đà đủ mạnh → rank yếu | Có node bắt được, nhưng nằm sau node tỉ lệ chuyển đổi → **rủi ro dừng sớm và kết luận nhầm sang listing** | ⚠️ đúng nhánh, sai thứ tự |
| ASIN B | Campaign 1-keyword nhồi nhiều target → spend rải rác | Node phân bổ spend nằm **cuối cây**; thêm nữa Lỗi 3 làm nhánh này định nghĩa chưa chuẩn | ❌ |
| ASIN C | Cùng hai nguyên nhân trên | Cây thiết kế để ra **một** kết luận, nhưng ca thật có **hai** nguyên nhân đồng thời ở hai nhánh khác nhau | ❌ |

**🔴 Lỗi 5 — giả định "một ASIN một nguyên nhân" không đúng với thực tế.**
Framing Brief Buổi 1 tôi viết *"chạy hết cây phải ra được một kết luận nguyên nhân, không phải một
danh sách khả năng"*. Mục đích đúng — chặn AI trả về danh sách khả năng vô dụng. Nhưng ba ca thật
cho thấy hai nguyên nhân **cùng tồn tại và có quan hệ nhân quả với nhau**: không đẩy đủ spend nền
và rải spend sai cấu trúc là hai mặt của cùng một vấn đề phân bổ.

Sửa đúng không phải bỏ ràng buộc, mà đổi từ *"một kết luận"* sang *"một nguyên nhân gốc, kèm các
nguyên nhân phụ thuộc nếu có, và phải nêu quan hệ giữa chúng"*.

## E.2. Trả lời các câu hỏi tự đặt trước khi chạy

| Câu hỏi | Trả lời |
|---|---|
| Step nào làm thay việc của step sau? | Không có. Ràng buộc "không làm việc của bước sau" ở từng step hoạt động tốt |
| Step 4 có tôn trọng luật "chưa đủ dữ liệu" không? | **Có.** Không có ô ngưỡng nào bị điền số ước lượng |
| Step nào nên gộp? | Không có |
| Step nào nên tách? | Không có. Quyết định tách Step 1 khỏi Step 2 được chứng minh đúng: 22 entry giả thuyết đã bị lọc ở Step 2 chứ không bị bỏ sót ngay từ Step 1 |
| Ca mindmap 2 tuần có rơi đúng nhánh không? | Rơi vào nhánh cấu trúc campaign — **cùng nhánh với hai nguyên nhân thật**. Điều này xác nhận nhánh đó là nhánh quan trọng nhất, và càng cho thấy việc xếp nó xuống cuối cây là sai |

## E.3. Checkpoint đã hoạt động như thiết kế chưa

| Checkpoint | Kết quả |
|---|---|
| CP1 (sau Step 2) | ⚠️ **Bắt được lỗi nhưng không chặn được.** Điều kiện "không cặp nhánh nào cùng đúng" bị vi phạm do trần 5 nhánh, nhưng chính trần đó lại là ràng buộc cứng — checkpoint không có đường xử lý cho tình huống hai ràng buộc đánh nhau |
| CP2 (sau Step 4) | ✅ Đạt. Mọi ngưỡng truy được nguồn; số nhánh có ngưỡng vượt mức tối thiểu |
| CP3 (cuối) | ✅ **Hoạt động đúng như thiết kế.** Cây sai ở ≥2/3 ca → chỉ đường quay về Step 2 chứ không phải Step 5. Và đúng vậy thật: gốc của Lỗi 4 nằm ở cách phân nhánh, không nằm ở cách vẽ cây |

Đây là phần tôi hài lòng nhất: **checkpoint 3 chẩn đoán đúng chỗ cần quay lại trước khi tôi kịp
nghĩ ra.** Nếu không có nó, phản xạ tự nhiên sẽ là sửa thứ tự node ở Step 5 — chữa triệu chứng.

## E.4. Phiên bản sau khi sửa — Stack V1.1

| # | Sửa gì | Ở step nào | Bằng chứng từ lần chạy |
|---:|---|---|---|
| 1 | Đổi cờ nhạy cảm từ **loại bỏ** sang **gắn nhãn**: vẫn dùng làm căn cứ, nhưng đánh dấu "không xuất ra ngoài" và lược bỏ khi đóng gói tài liệu đào tạo | Step 1 | 9/18 ngưỡng dùng được đang bật cờ; loại hết là mất nửa nguồn |
| 2 | Thêm luật **mâu thuẫn nội bộ ↔ nội bộ**: khi hai entry cùng mức tin cậy nói ngược nhau, phải nêu **điều kiện áp dụng** của từng bên thay vì chọn một bên | Step 1 | Hai entry cùng mức đã duyệt nói ngược nhau về số target/campaign |
| 3 | Nới trần từ **5 lên 6 nhánh**, kèm điều kiện: nhánh thứ 6 chỉ được thêm khi chứng minh được nó có cơ chế khác hẳn 5 nhánh kia | Step 2 | Nhóm nguyên nhân tự nhiên tách thành 6; ép xuống 5 làm hai nhánh chồng nhau |
| 4 | **Thêm khái niệm node tiên quyết.** Trước khi xếp thứ tự theo chi phí soi, phải rút từ knowhow các **điều kiện tiên quyết** — node tiên quyết đứng trước mọi node khác, bất kể soi nhanh hay chậm | Step 5 | Lỗi 4. Kho có entry mức chuẩn, 5 lần xác nhận, nêu đúng điều kiện tiên quyết mà stack không hỏi tới |
| 5 | Đổi tiêu chí đầu ra của cây: từ **"một kết luận"** sang **"một nguyên nhân gốc + các nguyên nhân phụ thuộc + quan hệ giữa chúng"** | Step 5, Step 6 | Ba ca thật đều có hai nguyên nhân đồng thời, có quan hệ nhân quả với nhau |
| 6 | Bổ sung đường xử lý cho CP1 khi hai ràng buộc mâu thuẫn: ghi rõ ràng buộc nào nhường, và ghi lại để rà lại Framing Brief | Checkpoint 1 | CP1 bắt được lỗi nhưng bế tắc |

## E.5. Điều rút ra lớn nhất

Stack không hỏng vì thiếu thành phần RTC-COE. Cả sáu prompt đều đủ Role, Task, Context, Constraints,
Output, Evaluation. Nó hỏng ở **một constraint nghe rất hợp lý mà sai về logic**: *"ưu tiên nhánh
loại trừ được nhiều nhất với công sức ít nhất"*.

Câu đó tối ưu cho **người soi**. Nhưng chẩn đoán phải tối ưu cho **thứ tự nhân quả** — điều kiện
tiên quyết trước, hệ quả sau. Hai thứ tự này ngược nhau ở đúng bài toán của tôi, vì thứ dễ soi nhất
(listing, tỉ lệ chuyển đổi) lại chính là thứ **bị ảnh hưởng bởi** nguyên nhân thật chứ không phải
nguyên nhân.

Và điều đáng nói nhất: **câu trả lời đúng đã nằm sẵn trong kho knowhow của tôi**, ở mức đã duyệt,
được xác nhận 5 lần. Stack không thiếu dữ liệu — nó thiếu **một câu hỏi**. Đó là khác biệt giữa
"prompt chưa đủ thông tin" và "prompt chưa đúng cấu trúc", và tôi chỉ nhìn ra nó sau khi chạy thật.

---

# Rubric tự chấm

| Tiêu chí | 1 — Yếu | 3 — Đạt | 5 — Tốt | Điểm | Lý do |
|---|---|---|---|---:|---|
| Framing alignment | Stack lệch Framing Brief | Phần lớn bám framing | Mọi step đều phục vụ đúng mục tiêu | **5** | Sáu step đều phục vụ một artifact; B.3 đối chiếu ngược từng dòng với Buổi 1 |
| RTC-COE quality | Prompt mơ hồ | Có task, context và output | Thành phần được dùng có chủ đích | **4** | Đủ và có chủ đích, nhưng Step 1 thiếu luật mâu thuẫn nội bộ ↔ nội bộ (Lỗi 3) |
| Task decomposition | Các step tùy ý | Có thứ tự tương đối rõ | Mỗi step có vai trò và dependency rõ | **4** | Dependency chặt và được test xác nhận; trừ điểm vì trần 5 nhánh ở Step 2 chật (Lỗi 2) |
| Output chaining | Output rời rạc | Một số bước có nối | Output trước trở thành input có ý nghĩa | **5** | Chạy thật: output mỗi bước là input bước sau, không bước nào phải làm lại từ đầu |
| Checkpoint | Không có | Có kiểm tra cơ bản | Checkpoint quyết định rõ đi tiếp hay quay lại | **5** | Ba checkpoint; CP3 chỉ đúng chỗ cần quay lại (Step 2, không phải Step 5) trước khi tôi tự nhận ra |
| Evaluation | Chỉ dựa vào cảm giác | Có một bước critique | Có tiêu chí và sửa dựa trên bằng chứng | **4** | Sáu sửa đều có bằng chứng từ lần chạy; trừ điểm vì chưa kiểm được ngưỡng trên số thật |
| Practicality | Khó dùng trong thực tế | Có thể thử nghiệm | Tạo được artifact gần với công việc thật | **4** | Chạy được trên nguồn thật và bắt được 5 lỗi; nhưng cây v1 chưa dùng được cho nhân sự, cần chạy lại V1.1 |
| **Tổng** | | | | **31/35** | |

Theo thang của bài: 25–31 là *"stack tốt, có thể sử dụng và tiếp tục test"*. Tôi chấm 31 chứ không
cao hơn vì **cây v1 chưa đạt ở Step 6** — stack tốt nhưng sản phẩm cuối chưa dùng được, cần chạy
lại V1.1 với sáu chỉnh sửa ở E.4 và test trên số liệu thật của ba ASIN.

---

# Việc tiếp theo

1. Chạy lại stack V1.1 với sáu chỉnh sửa, trích số liệu thật của ba ASIN tại thời điểm tụt để kiểm
   phần **ngưỡng** — thứ lần chạy này chưa kiểm được.
2. Đưa Cause Shortlist và Threshold Table sang Buổi 3 làm nguyên liệu cho Decomposition Tree miền
   tri thức.
3. Nhánh xử lý (phase 2 — `DESIGN`) vẫn giữ ngoài phạm vi, đúng như đã chốt ở Buổi 1.
