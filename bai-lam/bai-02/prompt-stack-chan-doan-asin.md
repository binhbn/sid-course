# Assignment Buổi 2 — Prompt Stack V1: Xây cây chẩn đoán ASIN

**Học viên:** Bùi Ngọc Bình
**Framing Brief nguồn:** `bai-lam/bai-01/framing-brief-chan-doan-asin.md`
**Domain project xuyên khóa:** Vận hành hiệu suất ASIN (chẩn đoán → xử lý → theo dõi).
Scope từ Buổi 1 đến Buổi 3 em chỉ làm ở nhánh chẩn đoán.

---

## Một lưu ý trước khi vào bài

Stack trong bài này là stack dùng để *xây ra* cây chẩn đoán, không phải prompt mà nhân sự gõ hằng
ngày để soi một ASIN. Bản chạy hằng ngày là sản phẩm cuối, em để lại các buổi sau khi cây đã được
kiểm chứng.

Em phải phân biệt hai thứ này ngay từ đầu vì nó quyết định cách bóc task. Nếu viết stack cho bản
chạy hằng ngày thì bài toán bản chất chỉ là một việc, và ép nó thành sáu bước sẽ ra đúng cái lỗi
mà bài học gọi là "tạo quá nhiều step".

---

# Phần A — Prompt ban đầu

Prompt dưới đây em chưa gửi nguyên văn bao giờ. Nó gộp lại đúng cách em đã thật sự tiếp cận ở hai
prompt đã audit ở Buổi 1: prompt ngày 09/02/2026 (nhân sự mỗi người quản 40-60 ASIN, quá tải) và
prompt ngày 14/07/2026 (nhân sự loay hoay dù đã đào tạo, em phải tự vẽ mindmap rồi theo sát hai
tuần). Cùng một người dùng, cùng một cơn đau, và cùng kiểu kết bằng một động từ mở.

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
| Role | "chuyên gia vận hành Amazon FBA hàng đầu" | Có Role nhưng phóng đại. Chữ "hàng đầu" không thêm thông tin nào để AI xử lý khác đi, đúng lỗi thứ hai mà bài học nêu | Chỉ định góc nhìn thay vì cấp bậc, ví dụ "người đã xây quy trình chẩn đoán cho đội vận hành nhiều ASIN cùng lúc". Góc nhìn đó mới khiến AI ưu tiên tốc độ soi và khả năng giao lại cho người khác |
| Task | "phân tích và xây dựng" kèm năm đầu ra | Trộn năm sáu nhiệm vụ khác bản chất trong một lượt, em tách kỹ ở mục B.2 | Bóc thành stack, mỗi bước một nhiệm vụ |
| Context | Quy mô 40-60 ASIN mỗi người, đã đào tạo mà vẫn không áp dụng được, một ca cụ thể đã xử lý, có trỏ nguồn knowhow nội bộ | Đây là phần mạnh nhất của prompt. Có quy mô, có bằng chứng đã thử và thất bại, có ca thật, và có ý thức chỉ nguồn. Chỗ trừ điểm là nguồn mới trỏ tới cả kho chứ chưa tới đúng phần cần dùng | Nêu tên database cụ thể, bổ sung nhân sự đang có công cụ nào, dữ liệu lấy ở đâu, và "tụt" nghĩa là giảm bao nhiêu trong bao lâu |
| Constraints | "ưu tiên nguồn knowhow nội bộ trước" | Có đúng một constraint và nó kiểm tra được, cái này em nghĩ là điểm cộng thật. Nhưng thiếu trần số nguyên nhân, thiếu luật khi hai nguồn mâu thuẫn, thiếu luật khi không có dữ liệu | Thêm trần năm nhánh, luật nêu cả hai bên khi mâu thuẫn thay vì tự chọn, và luật để trống khi không có căn cứ |
| Output | "một quy trình hoặc checklist" cộng với "tài liệu hướng dẫn để đào tạo lại" | Chữ "hoặc" giao quyền chọn dạng sản phẩm cho AI. Nặng hơn là đang xin hai artifact cho hai người dùng khác nhau trong cùng một lượt | Gọi đúng tên artifact đã chốt ở Buổi 1 là cây quyết định có thứ tự soi và ngưỡng. Tài liệu đào tạo là sản phẩm khác, để sau |
| Evaluation | Không có | Không có tiêu chí nào để biết kết quả đạt hay chưa. Câu kết "bạn có thể hỏi thêm nếu cần" thực chất đẩy việc kiểm tra sang phía AI | Mỗi bước có tiêu chí tự kiểm riêng, và checkpoint do em quyết trước khi đi tiếp |

## B.2. Sáu nhiệm vụ đang bị trộn

| # | Nhiệm vụ ẩn trong prompt | Loại tư duy | Cần gì để làm được |
|---|---|---|---|
| 1 | Rút các nguyên nhân có thể khiến ASIN tụt | Analyze | Kho knowhow nội bộ và nguồn ngoài |
| 2 | Chọn chỉ số quan sát cho từng nguyên nhân | Design | Biết nhân sự đọc được số ở đâu |
| 3 | Đặt ngưỡng cảnh báo cho từng chỉ số | Design | Dữ liệu lịch sử thật, AI không tự có |
| 4 | Sắp thứ tự kiểm tra | Design | Logic chi phí soi, phải xong ba việc trên trước |
| 5 | Đề xuất cách xử lý theo từng nguyên nhân | Design | Nằm ngoài scope em đã chốt ở Buổi 1 |
| 6 | Soạn tài liệu đào tạo lại cho team | Design | Người đọc khác hẳn, cần cây xong trước |

Nhiệm vụ số 3 là chỗ em thấy nguy hiểm nhất. Nếu chạy chung một lượt, AI sẽ điền ngưỡng theo mặt
bằng chung của ngành chứ không theo dữ liệu công ty em, mà nhân sự đọc lại tưởng đó là ngưỡng nội bộ.

## B.3. Đối chiếu ngược với Framing Brief Buổi 1

Đây là phần em thấy có ích nhất khi audit. Prompt không chỉ thiếu thành phần, nó còn phá luôn cái
scope mà chính em đã chốt ở buổi trước.

| Buổi 1 em đã chốt | Prompt ở Phần A | Hệ quả |
|---|---|---|
| Artifact là cây quyết định có thứ tự soi và ngưỡng | "quy trình hoặc checklist" | Mất tên artifact đã giành được ở Buổi 1 |
| Tối đa 5 vấn đề trọng tâm | "các nguyên nhân có thể" | Không có trần, nhận về danh sách dài, nhân sự vẫn không biết soi đâu trước |
| Chạy hết cây ra một kết luận nguyên nhân, không phải danh sách khả năng | Không nêu | Mất tiêu chí nghiệm thu quan trọng nhất |
| Ngoài phạm vi: chưa đề xuất cách sửa, đó là phase 2 | "cách xử lý tương ứng với mỗi nguyên nhân" | Kéo nguyên phase 2 vào, làm loãng phần chẩn đoán |
| Người dùng là nhân sự MKT vận hành | Trộn hai người dùng: nhân sự đang soi và em đang giảng lại | Một artifact không phục vụ tốt cho ai |

## B.4. Chỉ nguồn nhưng chưa định vị được nguồn

Cụm "dựa trên kho knowhow của công ty trong Notion".

Về kỹ thuật thì câu này chạy được, workspace của em đã kết nối với AI nên nó đọc được thật. Vấn đề
nằm chỗ khác: kho có nhiều hệ thống, còn câu lệnh chỉ trỏ tới cả cái kho. Em viết ngắn vì ngầm cho
rằng AI đã trao đổi với em nhiều lần nên biết lấy ở đâu.

Cách nghĩ đó chỉ đúng khi đang trong một phiên có sẵn ngữ cảnh. Prompt này là prompt một lượt, nên
nó rơi vào đúng chỗ em đã tự chấm ở Buổi 1. Prompt ngày 14/07 là ô duy nhất trong bảng bốn prompt
được em chấm "Context: Rõ", với lý do ghi nguyên văn là "chỉ đúng ba nguồn cần đọc". So với chính
mình bảy tháng trước, prompt lần này lùi một bước ở đúng thành phần em từng làm tốt nhất.

Hai hệ quả cụ thể. Thứ nhất là em không kiểm được AI đã đọc gì, nên một nguyên nhân trả về không
truy ngược được là đến từ knowhow công ty hay từ kiến thức chung, trong khi chính prompt lại đặt
luật ưu tiên nội bộ. Thứ hai là nó hỏng lặng lẽ khi đổi môi trường: nếu kết nối lỗi hoặc em gửi
prompt này qua một AI khác không có kết nối, AI vẫn trả về đủ bảng, chỉ là làm bằng kiến thức ngoài,
và không có gì báo cho em biết luật ưu tiên đã bị bỏ qua.

Sửa theo ba lớp chứ không phải bỏ nguồn nội bộ đi. Trỏ đúng tên database thay vì nói "kho knowhow".
Trỏ đúng phần trong nguồn được phép dùng, chỗ này em viết riêng ở B.5. Và bắt AI khai chính xác
những gì đã đọc, dừng lại báo nếu không mở được, thay vì lặng lẽ làm bằng kiến thức chung.

## B.5. Nguồn nội bộ đã có sẵn cấu trúc, nhưng prompt không dùng

Kho knowhow của em không phải một đống ghi chép phẳng. Mỗi entry đã được gắn nhãn theo bốn chiều mà
bài toán chẩn đoán cần đến, chỉ là em chưa từng nghĩ tới việc đưa chúng vào prompt.

| Chiều phân loại | Các mức | Vì sao prompt nên dùng |
|---|---|---|
| Độ tin cậy | giả thuyết một lần, đã kiểm chứng, chuẩn đã duyệt, đã bác bỏ | Entry ở mức giả thuyết có ghi rõ là chưa có số và cấm áp đồng loạt. Không lọc thì một giả thuyết chưa kiểm chứng sẽ nằm trong cây và nhân sự tưởng là luật công ty |
| Loại tri thức | ngưỡng có số, quy trình, nguyên lý, anti-pattern, quản trị | Nhóm ngưỡng có số là nguồn ngưỡng đáng tin duy nhất cho bước đặt ngưỡng, còn nhóm anti-pattern là các bẫy chẩn đoán sai |
| Còn hiệu lực hay không | có khoảng hiệu lực và ngày rà lại | Knowhow hết hạn vẫn nằm trong kho, đọc nhầm là chẩn đoán theo luật cũ |
| Cờ nhạy cảm | bật khi entry có thông tin business của khách hoặc học viên | Phải xử lý riêng, nhất là khi kết quả sẽ được đóng gói lại để đào tạo |

Prompt ở Phần A viết "dựa trên kho knowhow của công ty", tức là bảo AI đọc toàn bộ mà không phân
biệt bốn chiều trên. Hệ quả nặng nhất là một giả thuyết chưa kiểm chứng và một quy tắc đã được duyệt
sẽ vào cây với cùng trọng số. Với một artifact mà nhân sự dùng để kết luận nguyên nhân thì em nghĩ
lỗi này còn nghiêm trọng hơn cả việc thiếu Evaluation.

Đây cũng là điều em rút ra rõ nhất khi làm phần audit: Context Framing không dừng ở chuyện chỉ đúng
nguồn, mà là chỉ đúng phần nào trong nguồn được phép dùng làm căn cứ.

## B.6. Vì sao một prompt duy nhất chưa phù hợp

Ba lý do, em xếp theo thứ tự nặng dần.

Thứ nhất, sáu nhiệm vụ khác loại tư duy nằm trong một lượt nên AI chỉ chạm nhẹ mỗi phần, không phần
nào đủ dùng.

Thứ hai, không có checkpoint. Nếu danh sách nguyên nhân ở bước đầu đã thiếu hoặc trùng lặp thì mọi
ngưỡng và thứ tự soi phía sau đều xây trên nền sai, mà em không có chỗ nào để phát hiện.

Thứ ba, nhiệm vụ đặt ngưỡng cần dữ liệu mà em chưa cung cấp, nên prompt một lượt buộc AI phải bịa
ngưỡng để hoàn thành đầu ra. Với bài toán này thì một ngưỡng sai im lặng nguy hiểm hơn một ô bỏ
trống, vì nhân sự sẽ soi theo nó rồi kết luận sai nguyên nhân.

---

# Phần C — Task Decomposition

## C.1. Task Map

| Step | Task | Câu hỏi chính | Input | Expected output | Phụ thuộc |
|---:|---|---|---|---|---|
| 1 | Rút danh sách nguyên nhân | Một ASIN có thể tụt vì những nguyên nhân nào? | Framing Brief và knowhow nội bộ | Cause Inventory có ghi nguồn | — |
| 2 | Gom và cắt còn tối đa 5 nhánh | Năm nhánh nào đáng soi nhất và không chồng nhau? | Cause Inventory | Cause Shortlist | Step 1 |
| 3 | Gán chỉ số quan sát cho mỗi nhánh | Nhìn số nào, đọc ở đâu thì biết nhánh đó có vấn đề? | Cause Shortlist | Signal Map | Step 2 |
| 4 | Đặt ngưỡng cho từng chỉ số | Đến mức nào thì coi là bất thường? | Signal Map và dữ liệu lịch sử | Threshold Table | Step 3 |
| 5 | Sắp thứ tự soi và dựng cây | Soi nhánh nào trước thì loại trừ được nhiều nhất? | Threshold Table | Decision Tree v1 | Step 4 |
| 6 | Test và sửa | Cây có chạy đúng trên ca thật không? | Decision Tree v1 và 3 ASIN thật | Reviewed Decision Tree | Step 5 |

## C.2. Dependency

```text
Cause Inventory
→ Cause Shortlist
→ Signal Map
→ Threshold Table
→ Decision Tree v1
→ Reviewed Decision Tree
```

Có ba quan hệ em không đảo được. Chưa cắt còn năm nhánh thì chưa nên gán chỉ số, vì gán chỉ số cho
mười lăm nguyên nhân là công vô ích khi hơn nửa sẽ bị loại ở bước sau. Chưa có chỉ số thì chưa đặt
được ngưỡng, vì ngưỡng luôn là ngưỡng của một chỉ số cụ thể. Và chưa có ngưỡng thì chưa sắp được
thứ tự soi, vì thứ tự phụ thuộc vào việc nhánh nào loại trừ được nhiều nhất và rẻ nhất, mà chỉ biết
được khi đã biết phải nhìn số gì.

## C.3. Vì sao dừng ở sáu bước

Em tách Step 1 khỏi Step 2 vì hai việc này ngược chiều nhau. Step 1 là thu thập, càng đủ càng tốt.
Step 2 là cắt bỏ, càng gọn càng tốt. Trộn vào một lượt thì AI sẽ tự kiểm duyệt ngay lúc liệt kê và
bỏ sót những nguyên nhân hiếm.

Ngược lại, Step 5 em gộp việc sắp thứ tự với việc dựng cây, vì cả hai cùng tạo ra một artifact.
Tách ra sẽ thành bước vụn không có đầu ra riêng.

Phần "cách xử lý theo từng nguyên nhân" và "tài liệu đào tạo lại cho team" trong prompt gốc thì em
để hẳn ngoài stack này, đúng scope đã chốt ở Buổi 1. Chúng thuộc phase 2 và chỉ chạy được khi cây
chẩn đoán đã qua test.

---

# Phần D — Prompt Stack V1

## Step 1 — Rút danh sách nguyên nhân

**Purpose:** Mở rộng hết mức các nguyên nhân có thể trước khi cắt. Đây là bước duy nhất được phép
rộng, các bước sau đều thu hẹp dần.

**Input:** Framing Brief Buổi 1 và knowhow nội bộ. Nguồn nội bộ đưa vào theo một trong hai cách, trỏ
tên database cụ thể nếu chạy trong môi trường có kết nối, hoặc dán trích nếu không. Cả hai cách đều
phải nêu tên nguồn cụ thể chứ không nói chung là "kho knowhow".

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
- Trước khi làm: mở database knowhow nội bộ nêu bên dưới và báo lại số entry đọc
  được, phân theo trạng thái tin cậy. Nếu không mở được, dừng lại và báo cho tôi.
  Không tự làm tiếp bằng kiến thức chung, làm vậy là phá luật ưu tiên nội bộ mà
  tôi không biết.

- Lọc theo trạng thái tin cậy của từng entry:
  · chuẩn đã duyệt và đã kiểm chứng thì dùng làm căn cứ chính;
  · giả thuyết một lần chưa có số thì liệt kê ở mục riêng, đánh dấu chưa kiểm
    chứng, không trộn vào danh sách chính;
  · đã bác bỏ thì không dùng làm nguyên nhân, nhưng đọc và tóm tắt riêng để biết
    cái gì đã thử và sai.

- Bỏ qua entry đã hết hiệu lực. Với entry còn nhãn vòng đời cũ, quy đổi sang hệ
  vòng đời hiện hành và ghi rõ đã quy đổi.
- Entry có cờ nhạy cảm vẫn dùng được làm căn cứ, nhưng đánh dấu rõ để không đưa
  ra ngoài khi đóng gói tài liệu.
- Đọc kỹ nhóm anti-pattern: đây là các cách làm đã biết là sai, dùng để cảnh báo
  bẫy chẩn đoán, không phải để làm nguyên nhân.
- Mỗi nguyên nhân ghi rõ nguồn là nội bộ kèm tên entry, hay là kiến thức ngoài.
- Ưu tiên liệt kê hết từ nguồn nội bộ trước, rồi mới bổ sung nguồn ngoài.
- Khi hai entry nội bộ cùng mức tin cậy nói ngược nhau, nêu điều kiện áp dụng của
  từng bên thay vì chọn một bên.
- Khi knowhow nội bộ mâu thuẫn với kiến thức chung, nêu cả hai và đánh dấu mâu
  thuẫn, không tự chọn.
- Không đề xuất cách xử lý ở bước này.
- Không gộp hai nguyên nhân khác cơ chế thành một dòng.

[Output]
1. Báo cáo nguồn: số entry đọc được tách theo trạng thái tin cậy, entry bị loại
   vì hết hiệu lực, entry được đánh dấu nhạy cảm.
2. Bảng nguyên nhân, chỉ lấy từ entry đã duyệt hoặc đã kiểm chứng và nguồn ngoài:
   a. Nguyên nhân
   b. Cơ chế, vì sao nó làm ASIN tụt
   c. Nguồn, kèm tên entry
   d. Trạng thái tin cậy của entry nguồn
   e. Dấu hiệu quan sát được đầu tiên
   f. Mức phổ biến theo đánh giá của bạn, kèm lý do
3. Mục riêng cho giả thuyết chưa kiểm chứng: nguyên nhân, entry nguồn, và cần số
   gì để nâng lên mức đã kiểm chứng.
4. Mục riêng cho những cái đã thử và sai, tóm tắt từ nhóm bác bỏ và anti-pattern.

[Evaluation]
Tự kiểm:
- có nguyên nhân nào thực chất là triệu chứng của nguyên nhân khác không;
- có entry đã duyệt nào chưa được phản ánh vào bảng không;
- có giả thuyết nào lọt vào bảng chính không, nếu có thì chuyển xuống mục 3;
- tỉ lệ dòng nội bộ so với nguồn ngoài có hợp lý không. Nếu gần như toàn nguồn
  ngoài thì khả năng cao bạn chưa đọc được nguồn nội bộ, hãy báo lại thay vì
  tiếp tục;
- có nguyên nhân nào bạn suy đoán mà không có căn cứ không, nếu có thì đánh dấu.

Framing Brief:
[DÁN FRAMING BRIEF BUỔI 1]

Nguồn knowhow nội bộ:
[TÊN DATABASE KHO KNOWHOW VẬN HÀNH NỘI BỘ]
```

**Expected output:** Cause Inventory.

---

## Step 2 — Gom và cắt còn tối đa 5 nhánh

**Purpose:** Biến danh sách dài thành số nhánh mà một người soi được trong một ca làm việc. Bước này
cũng là chỗ ép prompt tuân thủ ràng buộc tối đa năm nhánh mà em đã chốt ở Buổi 1.

**Input:** Cause Inventory từ Step 1.

```text
[Role]
Đóng vai người thiết kế quy trình vận hành, ưu tiên thứ dùng được hằng ngày hơn
thứ đầy đủ về lý thuyết.

[Task]
Gom Cause Inventory thành tối đa 5 nhánh nguyên nhân không chồng lấn.

[Context]
Năm nhánh này sẽ trở thành 5 nhánh chính của một cây quyết định mà nhân sự chạy
khi một ASIN tụt. Chạy hết cây phải ra được một kết luận nguyên nhân.

[Constraints]
- Tối đa 5 nhánh.
- Hai nhánh bất kỳ không được cùng đúng trên một ca. Nếu có thể cùng đúng thì
  phải nêu rõ cách phân định.
- Mỗi nguyên nhân trong Cause Inventory phải được xếp vào đúng một nhánh, hoặc
  ghi vào mục loại kèm lý do. Không được bỏ im lặng.
- Không tạo nhánh "khác" hoặc "nguyên nhân còn lại".

[Output]
1. Bảng 5 nhánh: tên nhánh, định nghĩa, các nguyên nhân thuộc về nó, ranh giới
   phân biệt với nhánh gần nhất.
2. Danh sách loại: nguyên nhân bị bỏ kèm lý do.
3. Ghi chú về những chỗ ranh giới còn mờ.

[Evaluation]
Tự kiểm:
- năm nhánh có phủ được các ca thường gặp không;
- có cặp nhánh nào dễ bị nhầm khi soi thực tế không;
- danh sách loại có bỏ sót nguyên nhân nghiêm trọng nào không.

Cause Inventory:
[DÁN OUTPUT STEP 1]
```

**Expected output:** Cause Shortlist.

**Checkpoint 1 — em quyết trước khi đi tiếp:**

- Đúng 5 nhánh hoặc ít hơn.
- Không có cặp nhánh nào có thể cùng đúng mà không phân định được.
- Mọi nguyên nhân bị loại đều có lý do, không cái nào biến mất lặng lẽ.
- Năm nhánh này phủ được ca mindmap hai tuần em đã tự xử lý. Nếu ca đó không rơi
  vào nhánh nào thì quay lại Step 1, chưa được đi tiếp.

---

## Step 3 — Gán chỉ số quan sát cho mỗi nhánh

**Purpose:** Biến nhánh nguyên nhân, vốn là một khái niệm, thành thứ nhân sự nhìn được trên màn hình.

**Input:** Cause Shortlist từ Step 2.

```text
[Role]
Đóng vai người vận hành Amazon quen đọc số trên Ads Manager và Seller Central.

[Task]
Với mỗi nhánh trong Cause Shortlist, xác định các chỉ số quan sát cho biết nhánh
đó đang có vấn đề.

[Context]
Người đọc là nhân sự MKT phải soi nhiều ASIN mỗi ngày, nên mỗi nhánh cần ít chỉ
số nhưng phải lấy được nhanh.

[Constraints]
- Mỗi nhánh tối đa 3 chỉ số.
- Mỗi chỉ số phải nêu rõ lấy ở đâu: màn hình hoặc báo cáo nào, của công cụ nào.
- Chỉ dùng chỉ số nhân sự lấy được bằng công cụ đang có, không giả định công cụ mới.
- Không điền ngưỡng ở bước này, ngưỡng thuộc bước sau.
- Nếu một nhánh không có chỉ số nào quan sát trực tiếp được, ghi rõ là không đo
  trực tiếp được và đề xuất dấu hiệu gián tiếp, thay vì bịa ra một chỉ số.

[Output]
Bảng gồm:
1. Nhánh
2. Chỉ số
3. Lấy ở đâu, công cụ và màn hình
4. Chỉ số này tăng hay giảm thì đáng ngờ
5. Cửa sổ thời gian nên nhìn
6. Độ tin cậy của chỉ số, kèm lý do

[Evaluation]
Tự kiểm:
- có chỉ số nào dùng chung cho nhiều nhánh đến mức không phân biệt được nhánh nào không;
- có chỉ số nào nhân sự thực tế không lấy được không;
- nhánh nào đang yếu nhất về khả năng quan sát.

Cause Shortlist:
[DÁN OUTPUT STEP 2]
```

**Expected output:** Signal Map.

---

## Step 4 — Đặt ngưỡng cho từng chỉ số

**Purpose:** Cho nhân sự một mốc để quyết thay vì làm theo cảm tính. Đây là bước phụ thuộc dữ liệu
nặng nhất, và cũng là bước em lo AI bịa số nhất.

**Input:** Signal Map từ Step 3 và dữ liệu lịch sử của một nhóm ASIN.

```text
[Role]
Đóng vai người phân tích dữ liệu vận hành, có thói quen phân biệt rõ đâu là số
đọc được từ dữ liệu và đâu là ước đoán.

[Task]
Đề xuất ngưỡng cảnh báo cho từng chỉ số trong Signal Map.

[Context]
Ngưỡng này sẽ được nhân sự dùng để kết luận một ASIN có vấn đề ở nhánh nào. Kết
luận sai dẫn tới xử lý sai chỗ và mất thêm thời gian.

[Constraints]
- Thứ tự ưu tiên nguồn ngưỡng, không được đảo:
  1. Entry knowhow nội bộ thuộc loại ngưỡng có số, ở mức đã duyệt hoặc đã kiểm
     chứng, và có trường bằng chứng số không trống;
  2. Dữ liệu lịch sử tôi dán bên dưới;
  3. Mặt bằng chung của ngành, chỉ khi hai nguồn trên không có, và phải gắn nhãn
     nguồn ngoài kèm lý do vì sao vẫn đáng tham khảo.
- Entry loại ngưỡng có số nhưng còn ở mức giả thuyết thì ghi vào mục ứng viên,
  không đưa vào bảng ngưỡng chính.
- Chỉ số nào không có đủ căn cứ từ cả ba nguồn thì ghi rõ là chưa đủ dữ liệu và
  nêu cần thu thập gì. Tuyệt đối không điền một con số ước lượng vào ô đó.
- Mỗi ngưỡng lấy từ nội bộ phải ghi kèm version và khoảng hiệu lực của entry nguồn.
- Ngưỡng phải kèm cửa sổ thời gian, không có ngưỡng nào đứng một mình.

[Output]
Bảng gồm:
1. Nhánh
2. Chỉ số
3. Ngưỡng đề xuất
4. Cửa sổ thời gian
5. Nguồn của ngưỡng, kèm tên entry và version nếu là nội bộ
6. Độ chắc chắn, và điều gì sẽ làm ngưỡng này sai

Kèm hai mục riêng:
- Ứng viên: ngưỡng từ entry còn ở mức giả thuyết, cần số gì để dùng được.
- Thiếu dữ liệu: chỉ số chưa đặt được ngưỡng và cần thu thập gì.

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
[DÁN DỮ LIỆU HOẶC GHI RÕ LÀ CHƯA CÓ]
```

**Expected output:** Threshold Table.

**Checkpoint 2 — em quyết trước khi đi tiếp:**

- Mọi ngưỡng đều truy được về nguồn, không ô nào là số không rõ từ đâu ra.
- Các ô ghi chưa đủ dữ liệu được để trống thật, không bị điền đại.
- Số nhánh còn đủ ngưỡng để chẩn đoán được từ ba trên năm trở lên. Nếu ít hơn thì
  dừng stack và đi thu thập dữ liệu trước, không dựng cây trên nền trống.
- Ngưỡng không lỏng quá đến mức mọi ASIN đều báo động, cũng không chặt quá đến
  mức không ASIN nào chạm.

---

## Step 5 — Sắp thứ tự soi và dựng cây quyết định

**Purpose:** Ra artifact chính mà Buổi 1 đã chốt, là cây quyết định có thứ tự soi và ngưỡng, chạy
hết ra một kết luận nguyên nhân.

**Input:** Threshold Table từ Step 4.

```text
[Role]
Đóng vai người thiết kế quy trình chẩn đoán, tối ưu cho việc loại trừ nhanh chứ
không cho việc đầy đủ.

[Task]
Sắp thứ tự kiểm tra 5 nhánh và dựng thành một cây quyết định hoàn chỉnh.

[Context]
Nhân sự chạy cây này khi phát hiện một ASIN tụt. Mục tiêu là ra được một kết luận
nguyên nhân, không phải một danh sách khả năng.

[Constraints]
- Trước khi xếp thứ tự, rút từ knowhow các điều kiện tiên quyết. Node tiên quyết
  đứng trước mọi node khác, không phụ thuộc vào việc nó soi nhanh hay chậm.
- Sau đó mới xếp các nhánh còn lại theo nguyên tắc loại trừ được nhiều nhất với
  công sức ít nhất, và nêu rõ lý do xếp thứ tự cho từng nhánh.
- Mỗi node phải là một câu hỏi trả lời được bằng số trong Threshold Table.
- Mỗi nhánh rẽ phải dẫn tới hoặc một node tiếp theo, hoặc một kết luận. Không có
  nhánh cụt.
- Cây phải có đường ra cho trường hợp không nhánh nào chạm ngưỡng.
- Nhánh nào thiếu ngưỡng thì đặt ở cuối và ghi rõ là nhánh chưa đủ căn cứ.
- Không đề xuất cách xử lý sau khi ra kết luận, cây dừng ở tên nguyên nhân.

[Output]
1. Bảng thứ tự soi: thứ tự, nhánh, đây có phải node tiên quyết không, lý do đặt
   ở vị trí này, chi phí soi.
2. Cây quyết định dạng văn bản phân cấp, mỗi node là một câu hỏi kèm ngưỡng.
3. Danh sách các kết luận nguyên nhân mà cây có thể ra.
4. Đường xử lý cho ca không nhánh nào chạm ngưỡng.

[Evaluation]
Tự kiểm:
- chạy thử cây trên một ASIN giả định có nhiều vấn đề cùng lúc, nó có ra được một
  kết luận duy nhất không;
- có node nào bắt tín hiệu là hệ quả của nguyên nhân khác rồi dừng sớm không;
- có nhánh cụt nào không;
- có câu hỏi nào nhân sự không tự trả lời được bằng số không.

Threshold Table:
[DÁN OUTPUT STEP 4]
```

**Expected output:** Decision Tree v1.

---

## Step 6 — Test trên ca thật và sửa

**Purpose:** Đánh giá cây bằng khả năng dẫn tới kết luận đúng, không bằng việc nó trông có vẻ hợp lý.

**Input:** Decision Tree v1 và 3 ASIN thật đã biết nguyên nhân.

Khi chạy thì em dán mã ASIN và số liệu thật. Trong bản nộp này em gọi ba ca là ASIN A, B, C, vì mã
sản phẩm tra được công khai trên Amazon nên em không đưa vào tài liệu công khai. Phần cần đánh giá
là cây có dẫn tới đúng kết luận hay không, việc này không phụ thuộc vào mã sản phẩm.

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
- Phân biệt rõ ba loại lỗi: sai ngưỡng, sai thứ tự soi, thiếu nhánh.
- Chỉ đề xuất sửa dựa trên bằng chứng từ 3 ca này, không sửa theo cảm giác chung.
- Không viết lại toàn bộ cây, chỉ nêu các chỉnh sửa cụ thể theo node.

[Output]
1. Ba bảng đường đi, mỗi ca gồm: node, số thực tế, rẽ nhánh nào, kết luận cây đưa
   ra, nguyên nhân thật, khớp hay không.
2. Bảng lỗi: node, loại lỗi, bằng chứng, cách sửa.
3. Danh sách 3 đến 5 chỉnh sửa ưu tiên.
4. Kết luận: dùng được, hay cần sửa trước khi giao cho nhân sự.

[Evaluation]
Tự kiểm:
- mọi nhận xét có dẫn được về một ca cụ thể không;
- có lỗi nào bạn phát hiện nhưng chưa xếp được vào ba loại trên không.

Decision Tree v1:
[DÁN OUTPUT STEP 5]

Ba ca thật:
[DÁN MÃ ASIN, SỐ LIỆU VÀ NGUYÊN NHÂN THẬT CỦA TỪNG CA]
```

**Expected output:** Reviewed Decision Tree.

**Checkpoint 3 — checkpoint cuối:**

- Cây ra đúng kết luận trên ít nhất hai trên ba ca thật.
- Mọi lỗi phát hiện đều xếp được vào một trong ba loại.
- Không còn nhánh cụt.
- Nếu sai ở từ hai trên ba ca trở lên thì quay lại Step 2 chứ không phải Step 5,
  vì sai kết luận thường là lỗi phân nhánh chứ không phải lỗi vẽ cây.

---

# Phần E — Test Log

## E.0. Điều kiện của lần chạy này

Em chạy ngày 08/08/2026, trên database knowhow vận hành nội bộ với 120 entry, kết nối trực tiếp
chứ không dán trích. Ba ca test là ba ASIN do cùng một người vận hành.

Nguyên nhân thật của ba ca em chốt trước khi chạy cây, gồm hai cái: một là chưa đẩy spend nhóm
campaign chạy đà đủ mạnh nên ASIN không lên được rank, hai là lên campaign một keyword nhưng nhồi
nhiều target nên spend rải rác, không chiếm được rank của từ khóa nào và kéo ROAS xuống.

Có một giới hạn em phải nói rõ. Step 1 đến Step 5 chạy đầy đủ trên nguồn thật, nhưng Step 6 mới
chạy ở mức đối chiếu logic, tức là so đường đi của cây với nguyên nhân đã biết, chứ chưa chạy trên
số liệu từng ASIN vì em chưa trích số tại thời điểm tụt. Vì vậy các lỗi tìm được dưới đây là lỗi
logic của cây, chưa phải lỗi ngưỡng. Phần ngưỡng chỉ kiểm được khi chạy trên số thật.

## E.1. Kết quả theo từng bước

### Step 1 — ra đúng artifact

Đọc được 120 entry, trong đó 27 ở mức chuẩn đã duyệt, 70 đã kiểm chứng, 22 còn là giả thuyết và 1
đã bị bác bỏ. Riêng nhóm ngưỡng có số thì có 18 entry ở mức dùng được và 8 entry còn là giả thuyết,
nên luật lọc mà em viết có việc thật để làm chứ không phải luật cho vui.

Luật khai nguồn chạy đúng như em mong. AI báo số entry trước khi làm nên em kiểm được là nó thực sự
đọc nguồn nội bộ chứ không làm bằng kiến thức chung.

**Lỗi 1 — constraint về cờ nhạy cảm em viết quá tay.** Bản đầu em viết là bỏ qua mọi entry có cờ
nhạy cảm. Chạy thật mới thấy 9 trên 18 entry ngưỡng dùng được đang bật cờ này, trong đó có mấy
ngưỡng cốt lõi nhất. Loại hết là mất một nửa nguồn. Em đọc nhầm ý nghĩa của cờ đó: nó có nghĩa là
không xuất ra ngoài, chứ không phải không được dùng. Hai chuyện khác nhau, và cây chẩn đoán dùng
nội bộ thì vẫn dùng được bình thường.

### Step 2 — ra artifact nhưng vướng ràng buộc

**Lỗi 2 — trần năm nhánh chật hơn thực tế.** Các nhóm nguyên nhân tự nhiên tách thành sáu: chưa lên
đủ bộ campaign nền, spend không tiêu được, cấu trúc target sai, listing và tỉ lệ chuyển đổi, giá và
vị thế cạnh tranh, tồn kho. Ép xuống năm thì buộc phải gộp hai nhóm khác cơ chế, mà như vậy lại vi
phạm chính constraint "hai nhánh không được cùng đúng trên một ca".

Trần năm là ràng buộc em tự chốt ở Buổi 1. Test cho thấy nó đúng về tinh thần, vì nhân sự không soi
nổi mười lăm nhánh, nhưng chật về con số. Đây là lần đầu một ràng buộc từ Buổi 1 bị chính dữ liệu
phản bác, nên em nghĩ đáng ghi lại.

**Lỗi 3 — thiếu luật xử lý mâu thuẫn giữa hai nguồn nội bộ với nhau.** Bản đầu em chỉ viết luật cho
trường hợp nội bộ mâu thuẫn với kiến thức ngoài. Chạy thật thì gặp hai entry cùng ở mức đã duyệt
nói ngược nhau về số target trên một campaign, một bên gom cụm nhiều keyword cùng nhóm, một bên yêu
cầu đúng một target khi vận hành bằng công cụ tự động. Thực ra không phải mâu thuẫn, mà là hai
nhánh có điều kiện áp dụng khác nhau, nhưng prompt không có chỗ nào bắt AI phát hiện và nêu điều
kiện đó ra.

Lỗi này em thấy nặng vì nó rơi đúng vào địa hạt của nguyên nhân thật thứ hai. Sai ở đây tức là chẩn
sai đúng cái đang xảy ra.

### Step 3 — ra đúng artifact

Không phát hiện lỗi. Ràng buộc tối đa ba chỉ số mỗi nhánh và không điền ngưỡng ở bước này đều được
tuân thủ, không có hiện tượng bước này làm thay việc của bước sau.

### Step 4 — luật chống bịa đứng vững

Thứ tự ưu tiên nguồn ba bậc chạy đúng. Ngưỡng lấy từ entry nội bộ đều truy được về tên entry và
version. Các chỉ số không có căn cứ được ghi là chưa đủ dữ liệu thay vì điền số ước lượng. Đây là
chỗ em lo nhất trước khi chạy nên cũng là chỗ em thấy nhẹ người nhất.

Tám entry ngưỡng ở mức giả thuyết đều rơi đúng vào mục ứng viên, không cái nào lọt vào bảng chính.

### Step 5 — ra artifact nhưng sai logic

**Lỗi 4, và đây là lỗi em thấy đáng nói nhất.** Constraint em viết ban đầu là "ưu tiên nhánh loại
trừ được nhiều nhất với công sức ít nhất". Theo tiêu chí này, cây xếp các nhánh đọc nhanh lên trước:
tồn kho, rồi listing và index, rồi tỉ lệ chuyển đổi, sau đó mới tới cấu trúc campaign và phân bổ spend.

Đối chiếu với nguyên nhân thật thì hỏng. Cả hai nguyên nhân đều nằm ở hai node cuối cùng. Tệ hơn là
node tỉ lệ chuyển đổi đứng trước sẽ bắt được tín hiệu và làm cây dừng sớm, vì khi ASIN chưa có rank
thì tỉ lệ chuyển đổi thấp là chuyện đương nhiên. Cây sẽ kết luận vấn đề ở listing, trong khi nguyên
nhân thật là chưa đẩy đủ spend để có rank.

Điều làm em nhớ lâu là kho nội bộ đã có sẵn cảnh báo đúng cái bẫy này trong nhóm anti-pattern, đại
ý là sửa lỗi kỹ thuật chỉ trả nợ về mốc bình thường nên đừng báo cáo nó như một giải pháp cải thiện
hiệu suất. Entry đó còn kèm một ca thật mà nhân sự đề xuất sửa listing trước, trong khi hiệu suất
đã kém từ nhiều tuần trước khi có sự cố.

Và kho cũng có sẵn câu trả lời đúng, ở mức đã duyệt và được xác nhận năm lần, đại ý là không đánh
giá hay loại bỏ một ASIN cho tới khi bộ campaign đã lên đúng loại và đủ số lượng. Đây là một điều
kiện tiên quyết chứ không phải một nhánh ngang hàng với các nhánh khác.

Nói cách khác, em sắp cây theo chi phí soi. Tiêu chí đó đúng nếu xét về hiệu quả vận hành, nhưng
sai về logic nhân quả. Nguồn nội bộ đã có sẵn thứ tự đúng, chỉ là stack của em không hỏi tới nó.

### Step 6 — cây v1 chưa đạt

| Ca | Nguyên nhân thật | Cây v1 dẫn tới đâu | Khớp không |
|---|---|---|---|
| ASIN A | Chưa đẩy spend nhóm chạy đà đủ mạnh nên rank yếu | Có node bắt được, nhưng nằm sau node tỉ lệ chuyển đổi nên có rủi ro dừng sớm và kết luận nhầm sang listing | Đúng nhánh nhưng sai thứ tự |
| ASIN B | Campaign một keyword nhồi nhiều target nên spend rải rác | Node phân bổ spend nằm cuối cây, thêm nữa Lỗi 3 làm nhánh này định nghĩa chưa chuẩn | Không |
| ASIN C | Cùng hai nguyên nhân trên | Cây thiết kế để ra một kết luận, nhưng ca thật có hai nguyên nhân đồng thời ở hai nhánh khác nhau | Không |

**Lỗi 5 — giả định một ASIN một nguyên nhân không đúng với thực tế.** Ở Framing Brief Buổi 1 em viết
là chạy hết cây phải ra được một kết luận nguyên nhân chứ không phải một danh sách khả năng. Mục
đích thì đúng, để chặn AI trả về danh sách vô dụng. Nhưng ba ca thật cho thấy hai nguyên nhân cùng
tồn tại và có quan hệ nhân quả với nhau, vì không đẩy đủ spend nền và rải spend sai cấu trúc thực ra
là hai mặt của cùng một vấn đề phân bổ.

Cách sửa em nghĩ không phải bỏ ràng buộc, mà đổi từ "một kết luận" sang "một nguyên nhân gốc, kèm
các nguyên nhân phụ thuộc nếu có, và phải nêu quan hệ giữa chúng".

## E.2. Trả lời các câu hỏi em đặt ra trước khi chạy

Không có bước nào làm thay việc của bước sau, ràng buộc này hoạt động tốt ở cả sáu bước.

Ở Step 4 thì AI tôn trọng luật chưa đủ dữ liệu, không có ô ngưỡng nào bị điền số ước lượng.

Không có bước nào nên gộp, và cũng không có bước nào nên tách thêm. Quyết định tách Step 1 khỏi
Step 2 hóa ra đúng, vì 22 entry giả thuyết đã bị lọc ở Step 2 chứ không bị bỏ sót ngay từ Step 1.

Ca mindmap hai tuần em từng xử lý rơi vào nhánh cấu trúc campaign, tức là cùng nhánh với hai nguyên
nhân thật. Điều này vừa xác nhận nhánh đó là nhánh quan trọng nhất, vừa cho thấy việc em xếp nó
xuống cuối cây là sai.

## E.3. Checkpoint có hoạt động như thiết kế không

Checkpoint 1 bắt được lỗi nhưng không chặn được. Điều kiện "không cặp nhánh nào cùng đúng" bị vi
phạm do trần năm nhánh, nhưng chính trần đó lại là ràng buộc cứng, nên checkpoint rơi vào thế bí:
nó không có đường xử lý cho tình huống hai ràng buộc đánh nhau.

Checkpoint 2 đạt. Mọi ngưỡng truy được nguồn, và số nhánh có ngưỡng vượt mức tối thiểu em đặt ra.

Checkpoint 3 hoạt động đúng như thiết kế. Cây sai ở từ hai trên ba ca nên nó chỉ đường quay về
Step 2 chứ không phải Step 5. Và đúng như vậy thật, gốc của Lỗi 4 nằm ở cách phân nhánh chứ không
nằm ở cách vẽ cây.

Đây là phần em hài lòng nhất trong cả bài. Nếu không có checkpoint này thì phản xạ tự nhiên của em
sẽ là đi sửa thứ tự node ở Step 5, tức là chữa triệu chứng.

## E.4. Phiên bản sau khi sửa, Stack V1.1

| # | Sửa gì | Ở bước nào | Bằng chứng từ lần chạy |
|---:|---|---|---|
| 1 | Đổi cờ nhạy cảm từ loại bỏ sang gắn nhãn, vẫn dùng làm căn cứ nhưng đánh dấu không xuất ra ngoài và lược bỏ khi đóng gói tài liệu đào tạo | Step 1 | 9 trên 18 ngưỡng dùng được đang bật cờ, loại hết là mất nửa nguồn |
| 2 | Thêm luật cho mâu thuẫn giữa hai nguồn nội bộ: khi hai entry cùng mức tin cậy nói ngược nhau thì nêu điều kiện áp dụng của từng bên thay vì chọn một bên | Step 1 | Hai entry cùng mức đã duyệt nói ngược nhau về số target trên một campaign |
| 3 | Nới trần từ năm lên sáu nhánh, kèm điều kiện là nhánh thứ sáu chỉ được thêm khi chứng minh được nó có cơ chế khác hẳn năm nhánh kia | Step 2 | Nhóm nguyên nhân tự nhiên tách thành sáu, ép xuống năm làm hai nhánh chồng nhau |
| 4 | Thêm khái niệm node tiên quyết. Trước khi xếp thứ tự theo chi phí soi thì phải rút từ knowhow các điều kiện tiên quyết, và node tiên quyết đứng trước mọi node khác bất kể soi nhanh hay chậm | Step 5 | Lỗi 4. Kho có entry ở mức đã duyệt, xác nhận năm lần, nêu đúng điều kiện tiên quyết mà stack không hỏi tới |
| 5 | Đổi tiêu chí đầu ra của cây từ một kết luận sang một nguyên nhân gốc kèm các nguyên nhân phụ thuộc và quan hệ giữa chúng | Step 5 và Step 6 | Ba ca thật đều có hai nguyên nhân đồng thời, có quan hệ nhân quả với nhau |
| 6 | Bổ sung đường xử lý cho Checkpoint 1 khi hai ràng buộc mâu thuẫn: ghi rõ ràng buộc nào nhường, và ghi lại để rà lại Framing Brief | Checkpoint 1 | Checkpoint 1 bắt được lỗi nhưng bế tắc |

Bốn sửa số 1, 2, 4 và phần constraint của Step 5 em đã cập nhật vào Phần D ở trên. Ba sửa còn lại
liên quan tới trần số nhánh và tiêu chí đầu ra thì em giữ nguyên bản cũ trong Phần D, vì chúng đụng
tới ràng buộc đã chốt ở Framing Brief Buổi 1 nên em muốn rà lại framing trước rồi mới sửa.

## E.5. Điều em rút ra

Stack này không hỏng vì thiếu thành phần RTC-COE. Cả sáu prompt đều có đủ Role, Task, Context,
Constraints, Output và Evaluation. Nó hỏng ở một constraint nghe rất hợp lý mà lại sai về logic, đó
là câu "ưu tiên nhánh loại trừ được nhiều nhất với công sức ít nhất".

Câu đó tối ưu cho người đi soi. Nhưng chẩn đoán thì phải tối ưu theo thứ tự nhân quả, tức là điều
kiện tiên quyết trước rồi mới tới hệ quả. Hai thứ tự này ngược nhau ở đúng bài toán của em, vì thứ
dễ soi nhất như listing hay tỉ lệ chuyển đổi lại chính là thứ bị ảnh hưởng bởi nguyên nhân chứ
không phải nguyên nhân.

Điều em thấy đáng suy nghĩ nhất là câu trả lời đúng đã nằm sẵn trong kho knowhow của công ty em, ở
mức đã duyệt và được xác nhận năm lần. Stack không thiếu dữ liệu, nó thiếu một câu hỏi. Khoảng cách
giữa "prompt chưa đủ thông tin" và "prompt chưa đúng cấu trúc" nằm ở đó, và em chỉ nhìn ra được sau
khi chạy thật.

---

# Rubric tự chấm

| Tiêu chí | 1 — Yếu | 3 — Đạt | 5 — Tốt | Điểm | Lý do |
|---|---|---|---|---:|---|
| Framing alignment | Stack lệch Framing Brief | Phần lớn bám framing | Mọi step đều phục vụ đúng mục tiêu | 5 | Sáu bước đều phục vụ một artifact, và mục B.3 đối chiếu ngược từng dòng với Buổi 1 |
| RTC-COE quality | Prompt mơ hồ | Có task, context và output | Thành phần được dùng có chủ đích | 4 | Đủ và có chủ đích, nhưng Step 1 ban đầu thiếu luật mâu thuẫn giữa hai nguồn nội bộ |
| Task decomposition | Các step tùy ý | Có thứ tự tương đối rõ | Mỗi step có vai trò và dependency rõ | 4 | Dependency chặt và được test xác nhận, trừ điểm vì trần năm nhánh ở Step 2 chật |
| Output chaining | Output rời rạc | Một số bước có nối | Output trước trở thành input có ý nghĩa | 5 | Chạy thật thì output mỗi bước là input bước sau, không bước nào phải làm lại từ đầu |
| Checkpoint | Không có | Có kiểm tra cơ bản | Checkpoint quyết định rõ đi tiếp hay quay lại | 5 | Ba checkpoint, và Checkpoint 3 chỉ đúng chỗ cần quay lại trước khi em tự nhận ra |
| Evaluation | Chỉ dựa vào cảm giác | Có một bước critique | Có tiêu chí và sửa dựa trên bằng chứng | 4 | Sáu sửa đều có bằng chứng từ lần chạy, trừ điểm vì chưa kiểm được ngưỡng trên số thật |
| Practicality | Khó dùng trong thực tế | Có thể thử nghiệm | Tạo được artifact gần với công việc thật | 4 | Chạy được trên nguồn thật và bắt được năm lỗi, nhưng cây v1 chưa giao cho nhân sự được |
| **Tổng** | | | | **31/35** | |

Theo thang của bài thì 25 đến 31 là stack tốt, có thể sử dụng và tiếp tục test. Em chấm 31 chứ không
cao hơn vì cây v1 chưa đạt ở Step 6. Stack thì ổn nhưng sản phẩm cuối chưa dùng được, cần chạy lại
V1.1 với sáu chỉnh sửa ở mục E.4 và test trên số liệu thật của ba ASIN.

---

# Việc tiếp theo

Em dự định chạy lại stack V1.1 với sáu chỉnh sửa, và trích số liệu thật của ba ASIN tại thời điểm
tụt để kiểm phần ngưỡng, vì đó là thứ lần chạy này chưa kiểm được.

Cause Shortlist và Threshold Table sẽ được đưa sang Buổi 3 làm nguyên liệu cho Decomposition Tree
của miền tri thức.

Nhánh xử lý, tức phase 2 với task type DESIGN, em vẫn giữ ngoài phạm vi đúng như đã chốt ở Buổi 1.
