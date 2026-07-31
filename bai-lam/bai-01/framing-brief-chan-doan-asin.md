# Thực hành 1 — Chẩn đoán prompt cũ

**Dữ liệu thật:** 5 prompt trong transcript Claude Code của tôi, 07–20/07/2026 — chọn những lượt
tôi kỳ vọng cao nhưng phải làm lại nhiều vòng.

| # | Ngày | Prompt (trích) | Hậu quả đo được |
|---|---|---|---|
| P1 | 12/07 | "regen và deploy ads-actions.html… anh nên đưa bao nhiêu task vào 1 lần chat?… liệu playbook như này đã đủ tốt chưa" | Phần playbook không ra kết luận; phải bổ sung ngưỡng qua **3 lượt dài** sau đó |
| P2 | 09/07 | "báo cáo tuần anh đang ưng ý rồi… hướng tạo báo cáo daily thì em thấy sao?… viết lại prompt trên cho tối ưu" | Ra SPEC dùng được, nhưng **lượt kế tiếp** phải bù 3 ràng buộc, nặng nhất là "đang có report Ads ở hội thoại khác" |
| P3 | 17/07 | "đánh giá xem hệ thống report đã giải quyết được… Em có đề xuất gì tối ưu UI/UX hoặc tính năng?" | Nhận về **bản chấm điểm** trong khi tôi cần plan để làm; mất **3 lượt** mới ra thứ triển khai được |
| P4 | 20/07 | "bạn anh hướng dẫn 1 mẹo… cứ khi đạt 45% token thì tự động compact… Em cũng triển khai" | Mẹo vốn không chạy được; đôi bên sai số, tôi phải bắt lỗi và chỉnh lại giữa chừng |
| P5 | 07/07 | "1. muốn nhìn xu hướng theo ngày… 2. sắp xếp lagging/leading… 3. bổ sung dữ liệu từ tháng 1.2027, làm tab dữ liệu theo tháng" | **Chuỗi rework dài nhất tháng (8 lượt)**; %(COGS+FBA) ra 36–41% vs team tính tay 46–47% (coverage COGS T1 chỉ 61%) |

### Tự chấm 7 yếu tố

| Yếu tố | P1 | P2 | P3 | P4 | P5 |
|---|---|---|---|---|---|
| Topic | Có (3 topic rời) | Có | Có | Có | Có |
| Problem | Trộn nhiều bài toán | Mơ hồ | Trộn nhiều bài toán | Mơ hồ (nêu giải pháp, không nêu cơn đau) | Trộn nhiều bài toán |
| Audience | Mơ hồ | Không có | Mơ hồ | Rõ | Rõ |
| Context | Rõ | Thiếu | Rõ | Thiếu | Thiếu (giấu mốc 46–47%) |
| Scope | Không có | Quá rộng | Quá rộng | Không có | Quá rộng |
| Output | Chỉ "đánh giá" | Sản phẩm rõ | Chỉ "đánh giá/đề xuất" | Chỉ "triển khai" | Trộn lệnh-làm và xin-đề-xuất |
| Task | Nhiều nhiệm vụ trộn | Một nhiệm vụ | Nhiều nhiệm vụ trộn | Một nhiệm vụ | Nhiều nhiệm vụ trộn |

### Ba lỗi lặp lại

1. **Gộp nhiều mạch việc vào một lượt** (P1, P3, P5) — lệnh thực thi + câu hỏi tư vấn + yêu cầu
   thiết kế nằm cạnh nhau, nên bài toán thật bị chôn ở cuối và trả về nửa vời.
2. **Output dừng ở "đánh giá / đề xuất"** (P1, P3, P4) — không nói ra file gì, dạng gì, nên nhận về
   bản chấm điểm thay vì thứ bắt tay làm được.
3. **Không có ngưỡng nghiệm thu và luật xử lý khi thiếu data** (P4, P5) — đây là chỗ đắt nhất:
   ca P5 sai số vì code âm thầm lấy giá từ file plan thay vì báo thiếu. Lỗi fallback im lặng là lỗi
   của AI, nhưng phần thuộc cách giao việc thì có thật — prompt không nêu mốc đối chiếu, cũng không
   nói phải dừng khi thiếu data. Bài học này về sau thành luật "thiếu data → DỪNG" và gate COGS ≥95%.

### Viết lại — Framing Brief rút gọn

- **P1:** Tôi là CEO Pukido, đã có playbook vận hành ASIN nhưng nhân sự MKT hiểu sai luật chuyển
  Growth 1→Growth 2 nên chọn nhầm bộ từ khoá. Cần một trang tra cứu 1 mặt giấy cho nhân sự MKT: mỗi
  stage = dấu hiệu nhận biết + mục tiêu + 3 hành động + ngưỡng số (Growth 2: KW volume ≥1.000 vào
  top 10 organic, sàn %PL3 ≥10%). Lượt này không đụng deploy, không bàn chuyện gõ bao nhiêu topic.
- **P2:** Đã có báo cáo tuần chạy tốt; cần một trang daily đọc trong 30 giây mỗi sáng cho chính tôi,
  chỉ hiện chỉ số dẫn + cảnh báo cần xử lý ngay, cửa sổ 7 ngày, kế thừa loader và cache của báo cáo
  tuần. Không đặt PL3/PL5 lên daily (nguồn fee thật chỉ về theo tuần), không quét Cerebro, không
  chồng lấn report Ads đang dựng ở hội thoại khác — nếu chạm thì dừng hỏi.
- **P3:** report.pukido.com hiện quá nhiều thông tin nên nhân sự MKT không biết nhìn đâu trước. Cần
  một plan triển khai để bắt tay ngay trong tuần: bỏ/gộp khối gây rối, nêu rõ mỗi trang trả lời câu
  hỏi nào của ai. Không chấm điểm hệ thống, không bàn MCP-Ads hay lớp follow-up trong lượt này.
- **P4:** Tôi dùng Claude Code cả ngày và sợ mất số liệu/ràng buộc đã chốt khi phiên dài — đó mới là
  cơn đau, không phải con số "45%". Hãy kiểm chứng mẹo tự-compact có thật không trước khi làm, rồi
  đề xuất cách thật kèm nguồn tra được. Không sửa settings global cho tới khi tôi duyệt.
- **P5:** Số trên báo cáo này dùng để chấm COM nhân sự nên sai là sai tiền thật. Lượt này chỉ dựng
  tab dữ liệu theo tháng (Jan 2026 → nay), dùng đúng bộ chỉ số lagging/leading của tab tổng quan, và
  đối chiếu %(COGS+FBA) với mốc team tính tay 46–47% — lệch quá 2 điểm % thì dừng, báo tôi, không
  xuất. Không dùng file plan thay giá vốn thật, không đẩy Lark, không đụng xu hướng ngày.


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
