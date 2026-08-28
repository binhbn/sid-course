# Thực hành 1 — Chẩn đoán prompt cũ

**Dữ liệu thật:** 4 prompt trong lịch sử hội thoại AI của tôi, tháng 02/2026 và tháng 07/2026 —
chọn những lượt tôi kỳ vọng nhận được sản phẩm dùng ngay, nhưng kết quả trả về hoặc thừa, hoặc
không giao được, hoặc phải sửa qua nhiều vòng.

> **Ghi chú về dữ liệu:** phần "Nguyên văn" chép lại từ lịch sử hội thoại, có thể lệch đôi chỗ ở
> đầu/cuối đoạn. Một vài tên tài liệu nội bộ và đường link đã được thay bằng dấu ngoặc vuông.

---

## Prompt 1 — Yêu cầu "template hoặc quy trình" mà không nói dùng để làm gì

**Nguyên văn:**

> Hiện tại tôi đang trực tiếp cầm một số sản phẩm (ASINs) để vận hành, mục tiêu là để có cảm nhận
> trực tiếp (xem vấn đề ở đâu, tại sao nhân sự chưa triển khai vận hành hiệu quả hoặc có gì có thể
> tối ưu thêm từ knowhow hiện tại), để nắm bắt knowhow mới khi có - test thử tính năng mới (điều mà
> tôi thấy nhân sự vận hành thường không để ý mà chỉ tập trung vào knowhow đã chốt)...
> Hiện tại tôi đang nghĩ nên có hướng ghi nhận dữ liệu ở bước đầu như nào, và tracking mỗi lần tôi
> thao tác tối ưu để so sánh.
>
> Bạn hãy đề xuất cho tôi template hoặc quy trình triển khai phù hợp.

**Ngày:** 16/02/2026

**Kết quả nhận về:** AI mở đầu bằng việc tự diễn giải lại mục tiêu của tôi thành ba nhánh — dấu
hiệu nó phải đoán ý. Cuối lượt nhận được nguyên một bộ bốn file kèm hướng dẫn lưu trữ, ba lời nhắc
định kỳ và một checklist tự chấm. Lượt kết thúc bằng câu AI hỏi ngược *"Bạn muốn tôi customize
thêm không?"* — tức chính nó cũng không chắc bản giao có trúng hay không.

| Yếu tố | Trạng thái | Ghi chú lỗi |
|---|---|---|
| Topic | Có | Tracking thao tác tối ưu ASIN do chính tôi cầm |
| Problem | Trộn nhiều bài toán | Ba mục tiêu khác bản chất bị gộp: chẩn đoán nhân sự, học knowhow mới, so sánh trước/sau tối ưu |
| Audience | Không có | Không nói template này ai điền — chỉ mình tôi, hay sau này nhân sự dùng lại |
| Context | Thiếu | Không nêu đang dùng công cụ gì, dữ liệu lấy ở đâu, đã có hệ nào chưa |
| Scope | Không có | Không giới hạn số ASIN, thời gian chạy thử, hay độ nặng chấp nhận được |
| Output | Chỉ yêu cầu "template hoặc quy trình" | Chữ "hoặc" giao quyền chọn dạng sản phẩm cho AI → nhận về cả bộ bốn file |
| Task | Nhiều nhiệm vụ trộn | Thiết kế cách ghi baseline + thiết kế cách tracking + đề xuất quy trình vận hành |

**Viết lại — Framing Brief rút gọn:** Tôi là CEO đang tự tay vận hành vài ASIN để kiểm chứng knowhow
trước khi chuẩn hoá cho team; tôi cần **một bảng log duy nhất, mỗi lần ghi dưới 2 phút**, để so sánh
chỉ số trước và sau mỗi lần tôi can thiệp. Chỉ làm phần ghi nhận và so sánh của riêng tôi — không
thiết kế quy trình cho nhân sự, không dựng bộ tài liệu hướng dẫn, không đề xuất nhịp họp hay lịch nhắc.

---

## Prompt 2 — Đưa ba hiện trạng, hỏi "tối ưu như nào" mà không nói tối ưu theo tiêu chí gì

**Nguyên văn:**

> Hiện tại tôi đang thấy có 1 vấn đề với các nhân sự MKT là họ quản lý nhiều ASINs quá 40-60 ASINs.
> => nhân sự nếu rà soát lần lượt các ASINs sẽ bị quá tải. Tôi hiện tại đang có 3 loại báo cáo để
> nhân sự MKT check. Tôi đang suy nghĩ nên điều chỉnh / tối ưu như nào phù hợp. Bạn hãy đề xuất cho tôi:
>
> 1. Báo cáo top 10-15 ASINs trọng điểm nhân sự đang follow up — [tên báo cáo nội bộ] update hàng tuần.
> 2. Báo cáo MKT daily để xem performance của toàn bộ các ASINs được update hàng ngày.
> 3. [tên báo cáo nội bộ] để lọc ra toàn bộ những sản phẩm có performance không tốt (ROAS thấp, CTR, CVR thấp...)

**Ngày:** 09/02/2026

**Kết quả nhận về:** AI **không giao được sản phẩm nào** ở lượt đầu. Nó mở bằng *"Để đưa ra đề xuất
chính xác, tôi cần làm rõ một số điểm"* rồi hỏi ngược bốn nhóm câu hỏi. Thứ duy nhất nhận được là
một bảng so sánh ba báo cáo — tức mô tả lại chính hiện trạng tôi vừa kể, chưa phải giải pháp.

| Yếu tố | Trạng thái | Ghi chú lỗi |
|---|---|---|
| Topic | Có | Bộ ba báo cáo MKT đang dùng |
| Problem | Mơ hồ | "Nhân sự quá tải" là triệu chứng; chưa nói quá tải ở khâu nào — đọc số, ra quyết định, hay ghi báo cáo |
| Audience | Mơ hồ | Có nhắc "nhân sự MKT" nhưng không nói ai đọc bản đề xuất và ai có quyền đổi báo cáo |
| Context | Thiếu | Không đính kèm nội dung ba báo cáo, không nói cái nào đang thực sự được dùng |
| Scope | Quá rộng | "Điều chỉnh / tối ưu" mở toang: gộp, bỏ, đổi tần suất, đổi chỉ số, hay đổi cả quy trình đều lọt |
| Output | Chỉ yêu cầu "đề xuất" | Không gọi tên sản phẩm: bản so sánh, phương án gộp, hay bộ báo cáo mới |
| Task | Một nhiệm vụ | Điểm mạnh duy nhất của prompt này — chỉ có một việc |

**Viết lại — Framing Brief rút gọn:** Tôi là CEO công ty Amazon FBA, mỗi marketer đang phải theo
hàng chục ASIN và hiện có ba báo cáo chồng chéo nhau. Tôi cần **một bản đề xuất gộp hoặc cắt xuống
còn tối đa hai báo cáo**, nêu rõ mỗi báo cáo trả lời câu hỏi ra quyết định nào và tần suất bao lâu.
Chỉ làm ở tầng cấu trúc báo cáo — không thiết kế template chi tiết, không đề xuất công cụ mới,
không bàn tới quy trình họp.

---

## Prompt 3 — Nhờ AI viết prompt cho chính AI, kèm ba nguồn phải tự đi đọc

**Nguyên văn:**

> Hiện tại tôi đang muốn xây dựng Playbook hoặc tài liệu hướng dẫn vận hành cho [team nội bộ], đội
> ngũ nhân sự MKT vận hành sản phẩm nội bộ.
>
> Tôi đã có những buổi đào tạo rất chi tiết rồi nhưng thấy nhân sự vẫn loay hoay trong quá trình
> triển khai. Bạn có thể kiểm tra kho knowhow trong Notion và các meeting note cũng như vừa rồi tôi
> phải làm 1 mindmap phân tích chi tiết như này để hỗ trợ nhân sự triển khai đẩy rank các sản phẩm
> lên Growth nhưng cũng phải follow up liên tục trong 2 tuần mới ok [...link nội bộ...]
>
> Bạn hãy phân tích và tạo 1 prompt tối ưu để tôi gửi cho bạn hỗ trợ tôi trong việc này. Bạn có thể
> hỏi tôi thêm các thông tin nếu cần để triển khai công việc một cách tốt nhất.

**Ngày:** 14/07/2026

**Kết quả nhận về:** Đây là prompt tốn nhiều vòng sửa nhất trong bốn bản. Chuỗi diễn ra: AI đọc ba
nguồn, phản biện rằng nếu output lại là thêm một tài liệu know-how nữa thì tôi lặp lại đúng cái bẫy
cũ; ra bản prompt nháp; hỏi tôi ba quyết định; tự đổi vai đánh giá lại chính nó hai lần và tìm thêm
lỗi mỗi lần. Giữa chừng tôi phải đóng khung lại toàn bộ sản phẩm. Hội thoại kết thúc mà vẫn còn năm
mục treo chưa chốt được.

| Yếu tố | Trạng thái | Ghi chú lỗi |
|---|---|---|
| Topic | Có | Playbook vận hành cho team MKT |
| Problem | Mơ hồ | "Nhân sự vẫn loay hoay" — không phân biệt loay hoay vì thiếu kiến thức, thiếu ưu tiên, hay thiếu kỷ luật. Chính khoảng mơ hồ này làm mất mấy vòng để chẩn đoán lại |
| Audience | Mơ hồ | "Nhân sự MKT" nhưng không nói trình độ, ai là chủ tài liệu, ai bảo trì |
| Context | Rõ | Điểm mạnh: chỉ đúng ba nguồn cần đọc |
| Scope | Quá rộng | Không giới hạn giai đoạn vòng đời nào, nên về sau phải quay lại chốt |
| Output | Chỉ yêu cầu "chi tiết" | Sản phẩm được định nghĩa vòng vo: "Playbook **hoặc** tài liệu hướng dẫn" → chuyển thành "tạo 1 prompt" → cuối cùng mới lộ ra là cẩm nang kèm workbook. Bản chất sản phẩm chỉ rõ ở lượt thứ N |
| Task | Nhiều nhiệm vụ trộn | Đọc và tổng hợp ba nguồn + chẩn đoán nguyên nhân + viết prompt cho chính mình. Ba việc, ba loại tư duy |

**Viết lại — Framing Brief rút gọn:** Nhân sự MKT của tôi đã được đào tạo đủ know-how nhưng vẫn
không tự chạy được chiến dịch đẩy rank, khiến tôi phải theo sát thủ công. Tôi cần **một checklist
"một tuần của một ASIN" dài tối đa hai trang**, mỗi bước có một tiêu chí "thế nào là xong" đo được,
trỏ ngược về tài liệu đào tạo đã có. Không viết lại know-how, không thiết kế bộ chỉ số hay dashboard
ở lượt này, và không viết prompt hộ tôi — làm thẳng sản phẩm.

---

## Prompt 4 — Khai "ba bản nội dung" nhưng chỉ liệt kê hai, rồi cộng thêm hai việc ở cuối

**Nguyên văn:**

> Hiện tại anh đang muốn nghiên cứu hệ thống OKRs chuẩn từ [kênh YouTube về OKR].
>
> Anh muốn tổng hợp các kiến thức quan trọng như các định nghĩa, best practice, case study, cách
> triển khai, hướng dẫn triển khai với một người nắm cơ bản về định nghĩa rồi nhưng đã nhiều lần
> triển khai chưa thành công như bản thân anh và team. Anh muốn áp dụng luôn cho nửa cuối năm.
>
> Output sẽ là 3 bản nội dung:
>
> 1. Những kiến thức, định nghĩa, hướng dẫn, case study chung tổng hợp từ các video từ kênh => lưu
>    lại vào Notion kho kiến thức để sau này anh có thể tham khảo lại, làm tài liệu đào tạo cho team...
> 2. Bản hướng dẫn triển khai phù hợp với anh để anh áp dụng cho nửa cuối năm.
>
> Em hãy đề xuất xem các yêu cầu trên đã đủ tối ưu? chưa ok thì hãy viết lại thành một prompt tối ưu
> để xây dựng thành 1 plan rồi anh sẽ copy paste qua công cụ khác triển khai tiếp.
>
> Em cũng đề xuất thêm cho anh lần sau với việc nghiên cứu các kênh như này thì quy trình triển khai
> nên như nào, với prompt cụ thể từng bước, bộ agents (skills) tạo thêm nếu cần.

**Ngày:** 13/07/2026

**Kết quả nhận về:** AI không làm việc được ngay mà mở đầu bằng một mục phản biện sáu điểm, trong
đó ba điểm rơi đúng vào lỗi framing: (a) khai "3 bản nội dung" nhưng chỉ liệt kê hai, nên nó phải
để trống bản thứ ba thay vì đoán; (b) "áp dụng luôn cho nửa cuối năm" mâu thuẫn thời điểm — kỳ đó
đã đang chạy và OKR đã chốt, nên bài toán thật là *vận hành* OKR chứ không phải *set* OKR; (c) phạm
vi nguồn không giới hạn, "tổng hợp các video từ kênh" nghĩa là hàng trăm video nên nó phải tự đặt trần.

| Yếu tố | Trạng thái | Ghi chú lỗi |
|---|---|---|
| Topic | Có | Hệ thống OKR |
| Problem | Trộn nhiều bài toán | "Nhiều lần triển khai chưa thành công" là dữ liệu quý nhất nhưng không được mô tả — AI phải hỏi ngược mới có. Bài toán thật (vận hành OKR đang chạy) bị phát biểu nhầm thành nghiên cứu lý thuyết |
| Audience | Mơ hồ | "Một người nắm cơ bản về định nghĩa rồi" — không rõ là tôi, là team, hay cả hai; trong khi tài liệu số 1 lại nhắm cho team đọc sau này |
| Context | Thiếu | Không nêu OKR hiện có đang như thế nào, hỏng ở đâu |
| Scope | Quá rộng | Nguồn không giới hạn số lượng; phạm vi áp dụng cũng không giới hạn |
| Output | Sản phẩm rõ nhưng **đếm sai** | Khai ba bản mà chỉ mô tả hai — AI buộc phải đoán hoặc hỏi lại. Đây là lỗi nặng nhất của prompt này |
| Task | Nhiều nhiệm vụ trộn | Bốn việc trong một lượt: nghiên cứu nguồn + soạn tài liệu + đánh giá và viết lại chính prompt này + thiết kế quy trình tái sử dụng cho lần sau |

**Viết lại — Framing Brief rút gọn:** OKR kỳ này của công ty tôi đã chốt và đang chạy, nhưng các chu
kỳ trước đều chết giữa đường. Tôi cần **một playbook vận hành OKR dài tối đa ba trang** cho chu kỳ
đang chạy: nhịp check-in, cách chấm điểm, cách xử lý khi kết quả then chốt đi lệch. Không tổng hợp
lý thuyết OKR tổng quát, không viết lại prompt hộ tôi, và không thiết kế quy trình nghiên cứu cho
lần sau — đó là bài riêng.

---

## Ma trận tổng hợp bốn prompt

| Yếu tố | Prompt 1 | Prompt 2 | Prompt 3 | Prompt 4 |
|---|---|---|---|---|
| Topic | Có | Có | Có | Có |
| Problem | Trộn nhiều bài toán | Mơ hồ | Mơ hồ | Trộn nhiều bài toán |
| Audience | Không có | Mơ hồ | Mơ hồ | Mơ hồ |
| Context | Thiếu | Thiếu | **Rõ** | Thiếu |
| Scope | Không có | Quá rộng | Quá rộng | Quá rộng |
| Output | Chỉ "template hoặc quy trình" | Chỉ "đề xuất" | Chỉ "chi tiết" | Rõ nhưng đếm sai |
| Task | Nhiều nhiệm vụ trộn | **Một nhiệm vụ** | Nhiều nhiệm vụ trộn | Nhiều nhiệm vụ trộn |

Đọc theo cột thì thấy từng prompt hỏng ở đâu. Đọc theo hàng thì thấy thứ đáng chú ý hơn:
**Audience và Scope hỏng ở cả bốn prompt, không có ngoại lệ.**

## Ba lỗi lặp lại

1. **Kết prompt bằng một động từ mở.** "Đề xuất cho tôi", "tối ưu như nào", "template hoặc quy
   trình" — không gọi tên sản phẩm cần nhận. Hậu quả chia hai hướng trái ngược nhau: Prompt 1 khiến
   AI đoán rồi giao thừa cả bộ bốn file, Prompt 2 khiến AI không dám giao gì và hỏi ngược bốn nhóm
   câu hỏi. Cùng một lỗi, hai kiểu thất bại.

2. **Nhồi nhiều loại việc khác bản chất vào một lượt.** Prompt 3 và 4 đều trộn ba đến bốn việc, và
   cùng mắc một biến thể đặc biệt: *nhờ AI viết prompt hộ mình rồi mới làm*. Việc này kéo dài chuỗi
   thêm vài vòng trước khi chạm được sản phẩm thật, trong khi bản thân nó cũng là một task riêng cần
   được định khung.

3. **Không nêu audience và không cắt phạm vi.** Đây là lỗi duy nhất xuất hiện ở cả bốn prompt. Không
   có audience thì AI không biết viết cho ai đọc, nên chọn mức chi tiết theo cảm tính. Không có
   phạm vi thì mọi hướng mở rộng đều hợp lệ, nên nó mở rộng.

**Điều tôi rút ra:** giữa tháng 02 và tháng 07, phần bối cảnh trong prompt của tôi đã tốt lên rõ
(Prompt 3 là prompt duy nhất được chấm "Context: Rõ"). Nhưng Audience và Scope thì không cải thiện
chút nào — vì tôi vẫn nghĩ chúng là thứ "AI tự suy ra được". Đúng như bài học chỉ ra, đó chính là
hai chỗ AI không bao giờ tự suy ra đúng.

# Thực hành 2 — Một chủ đề, nhiều bài toán

## CHỦ ĐỀ: Hiệu suất sản phẩm (ASIN) trong kinh doanh Amazon.

## Bài toán

| # | Bài toán | Task type chính | Output chính |
|---|---|---|---|
| 1 | Chẩn đoán một ASIN đang giảm performance bị nghẽn ở khâu nào | `DIAGNOSE` | Cây quyết định: thứ tự soi + ngưỡng ở mỗi nhánh, chạy hết ra một kết luận nguyên nhân |
| 2 | Phân bổ nguồn lực giữa các ASIN khi quản lý nhiều sản phẩm cùng lúc | `COMPARE` | Ma trận ưu tiên ASIN kèm tiêu chí dồn lực / giữ nguyên / cắt |
| 3 | Tách bạch "quảng cáo kém" và "listing không chuyển đổi" | `DIAGNOSE` | Checklist phân tách traffic và conversion, mỗi bước gắn một ngưỡng đọc được từ Ads Manager / Seller Central |
| 4 | Đề xuất phương án tăng hiệu quả kinh doanh cho một ASIN khi đã biết nguyên nhân | `DESIGN` | Playbook xử lý theo từng nhóm nguyên nhân, mỗi nhóm nêu việc cần làm và thứ tự |
| 5 | Chọn cách đẩy quảng cáo phù hợp với ASIN theo từng giai đoạn vòng đời (Launch, Growth, Maturity, Declining) | `DESIGN` | Bảng chiến lược quảng cáo bốn giai đoạn: mục tiêu, loại campaign ưu tiên, chỉ số theo dõi |

**Quan hệ giữa các bài toán:** bài 1 và 3 là chẩn đoán (tìm nguyên nhân), bài 4 và 5 là thiết kế
(tạo giải pháp), bài 2 là so sánh để ra quyết định ưu tiên. Bài 4 chỉ chạy được khi bài 1 đã có
kết quả — đúng nguyên tắc tách phase ở mục 9: output của phase trước là input của phase sau.



# Thực hành 3 — Đổi audience, đổi toàn bộ cách làm

Giữ nguyên topic của Thực hành 2 — **Hiệu suất sản phẩm (ASIN) trong kinh doanh Amazon** — và viết
ba framing cho ba audience khác nhau. Ba audience giữ đúng ba nguyên mẫu của đề (người ít kinh
nghiệm · người đã có nghề · người ra quyết định chiến lược), chỉ đặt vào bối cảnh Amazon để dùng
được thật.

### Audience A — Chủ shop Amazon nhỏ, tự vận hành, mới bán

- **Mục tiêu:** biết ASIN của mình đang khoẻ hay đang có vấn đề. Chưa cần biết vì sao.
- **Phạm vi:** 3–4 chỉ số đọc thẳng được trên Seller Central. Ngoài phạm vi: không dùng Helium10
  hay công cụ trả phí, không phân tích cấu trúc campaign, không thuật ngữ PPC.
- **Đầu ra:** bảng "đèn giao thông" một trang — mỗi chỉ số kèm ngưỡng xanh/vàng/đỏ và một câu
  giải thích chỉ số đó nói lên điều gì.
- **Task type:** `EXPLAIN`

### Audience B — Nhân sự MKT vận hành, đã làm 1–2 năm

- **Mục tiêu:** khi một ASIN tụt, soi đúng thứ tự để không mất thời gian và không bỏ sót.
- **Phạm vi:** đủ 5 nhánh nguyên nhân, dùng cả Ads Manager, Seller Central và công cụ ranking từ
  khoá. Ngoài phạm vi: không bao gồm cách xử lý sau khi tìm ra nguyên nhân.
- **Đầu ra:** cây quyết định chẩn đoán, mỗi nhánh gắn một ngưỡng cụ thể, chạy hết cây ra một
  kết luận nguyên nhân duy nhất.
- **Task type:** `DIAGNOSE`

### Audience C — Người ra quyết định phân bổ vốn cho cả danh mục

- **Mục tiêu:** quyết định dồn nguồn lực vào ASIN nào và dừng ASIN nào trong kỳ tới.
- **Phạm vi:** nhìn toàn danh mục theo nhóm. Ngoài phạm vi: không đi vào chẩn đoán từng ASIN,
  không bàn chi tiết campaign hay từ khoá.
- **Đầu ra:** ma trận danh mục theo hai trục (hiệu quả hiện tại × tiềm năng còn lại) kèm tiêu chí
  dồn lực / giữ nguyên / cắt, và danh sách ASIN cần quyết ngay trong kỳ.
- **Task type:** `COMPARE`

### Điều rút ra

Cùng một topic, nhưng đổi audience thì **đổi luôn task type và đổi luôn artifact** — A cần được
giải thích, B cần được chẩn đoán, C cần được so sánh. Nếu bỏ trống audience và chỉ viết "phân tích
hiệu suất ASIN", AI sẽ mặc định trả về một bài tổng quan không phục vụ ai trong ba người này.

Đáng chú ý: cùng một dữ liệu đầu vào (Ads Manager, Seller Central, công cụ ranking) mà ra ba sản
phẩm hoàn toàn khác nhau. Cái quyết định không phải là dữ liệu, mà là người đọc.

# Thực hành 4 - Bài tập cuối buổi - Framing Brief — Chẩn đoán ASIN tụt doanh số - Bùi Ngọc Bình

---

## 1. Chủ đề

Hiệu suất sản phẩm (ASIN) trong kinh doanh Amazon.

## 2. Bài toán và mục tiêu

Khi hiệu suất (Performance) sản phẩm không tốt (ROAS thấp, đơn ít) thì nhân sự không biết rõ vấn đề ở đâu nên muốn có sự hỗ trợ để từ đó tập trung tìm giải pháp phù hợp.

## 3. Audience

Nhân sự vận hành, quảng bá sản phẩm bán hàng trực tiếp trên website TMĐT Amazon.

## 4. Bối cảnh

Nhân sự Marketing vận hành sản phẩm, bán hàng trên Amazon. Họ chưa có nhiều kinh nghiệm, kiến thức, công cụ hỗ trợ và kỹ năng để nhận định nhanh chóng và chính xác vấn đề.

## 5. Scope: trong và ngoài phạm vi

- **Trong phạm vi:** Nhận định đánh giá các sản phẩm bán hàng trên sàn thương mại điện tử Amazon dựa trên các số liệu từ AMZ Ads Manager, Seller Central, các công cụ ranking từ khóa như Helium10, SellerSprite (hoặc tương đương).
- **Ngoài phạm vi:** chưa đề xuất phương án sửa — vì mỗi nguyên nhân cần một
  playbook riêng và phải lấy kết quả chẩn đoán làm đầu vào. Đó là **phase 2 (DESIGN)**. Không đánh giá performance của sản phẩm ở các kênh khác ngoài Amazon.

## 6. Output và Task Type

- **Task type chính:** `DIAGNOSE`
- **Đầu ra ưu tiên:** cây quyết định có **thứ tự soi** và **ngưỡng** ở mỗi nhánh. Tối đa 5 vấn đề trọng tâm nhất —
  chạy hết cây phải ra được một kết luận nguyên nhân, không phải một danh sách khả năng.

## 7. Rủi ro nếu framing sai

Kết quả trả về chung chung, không có ứng dụng thực tế. Hoặc định hướng chưa chính xác vấn đề khiến cho kết quả không đúng.

---

## Rubric tự chấm (làm sau khi viết xong — ngưỡng đạt 20/30)

| Tiêu chí | Câu hỏi kiểm tra | Điểm |
|---|---|---:|
| Problem clarity | Đã phân biệt topic và problem chưa? | 5/5 |
| Goal/Output frame | Đầu ra có phải sản phẩm cụ thể và dùng được không? | 4/5 |
| Audience fit | Audience có nền tảng, mục tiêu và bối cảnh rõ không? | 5/5 |
| Scope quality | Trong và ngoài phạm vi có rõ không? | 4/5 |
| Task definition | Có một nhiệm vụ chính hay đang trộn quá nhiều việc? | 5/5 |
| Clarity & usability | Người khác đọc brief có thể bắt đầu làm ngay không? | 4/5 |
| **Tổng** | | **27/30** |
