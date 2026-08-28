# Bài 3 (buổi 04) — Decomposition & Knowledge Mapping · Tối ưu tỉ lệ chuyển đổi (CVR) của listing Amazon

**Học viên:** Bùi Ngọc Bình · **Ngày:** 28/08/2026
**Project xuyên khoá:** *Chẩn đoán performance ASIN — gồm phân tích ads và CVR*
Buổi 1 Framing → Buổi 2 Prompt Stack → Buổi 3 chatbot chẩn đoán ads → **Buổi 4 decomposition nốt CVR** → Buổi 5 IA.

**Hai thứ cây này đẻ ra, đã chạy thử trên sản phẩm thật:**

| | Là gì |
|---|---|
| [`thang-cham-diem-listing.md`](../thang-cham-diem-listing.md) | Cây → **thang chấm 3 tầng** (cổng · phân biệt · so đối thủ). Đây là chỗ cây biến thành thứ đo được. |
| [`demo/index.html`](../demo/index.html) | Kết quả chấm **4 sản phẩm thật** (mã đã đổi): 1 trang tổng quan + 4 trang chi tiết. |

Đợt rà đầu tiên cho ra một kết quả không đoán trước được: **4 sản phẩm cùng bị gắn nhãn “CVR thấp”
nhưng có 4 nguyên nhân khác nhau, và chỉ 1 trong 4 thật sự nghẽn ở trang sản phẩm.** Chi tiết ở mục 10.

---

## 0. Vì sao chọn nốt này để bóc

Chatbot bài 03 đi theo phễu **C1 Impression → C2 Click → C3 Conversion → PL** với 6 nhánh nguyên nhân.
Sau hai vòng test trên dữ liệu thật, **nhánh 4 — "Listing và tỉ lệ chuyển đổi"** là nốt cụt: bot kết
luận được *"nghẽn ở C3, bài toán tầng listing"* rồi dừng, không nói soi tiếp cái gì. Trong khi ở đội
vận hành, đúng chỗ đó đang là việc treo: có danh sách ASIN CVR thấp, có bảng đánh giá listing, nhưng
bảng đó mới là tiêu chí *vệ sinh* (gỡ nội dung mùa vụ cũ, thêm bảng so sánh, ghép biến thể) — chưa
xếp theo effort/impact, chưa có vòng đo trước/sau.

Thầy nói ở buổi 4: *"có thể decomposition tiếp trên một nốt"*. Bài này làm đúng việc đó. Cây sinh ra
ở đây sẽ thành `kb_06_listing_cvr.md` — file tri thức mới của bot, và routing mới trong Master
Instruction.

## 1. Framing tóm tắt

- **Topic:** tỉ lệ chuyển đổi của trang chi tiết sản phẩm (listing) trên Amazon.
- **Problem:** ASIN có traffic nhưng không thành đơn; nhân sự biết "listing yếu" nhưng không biết yếu
  *chỗ nào*, sửa *cái gì trước*, và sửa xong *có nhích không*.
- **Audience:** nhân sự vận hành ASIN (1–2 năm kinh nghiệm), mỗi người theo 40–60 ASIN; bản phái sinh
  cho seller tự vận hành.
- **Scope:** mô hình lời-trên-từng-đơn; hai tình huống — *rà listing đang chạy* và *cổng kiểm trước
  khi đổ ads cho ASIN mới*. Ngoài phạm vi: tạo ảnh/viết copy thay người, quyết giá.
- **Output:** scorecard chấm listing · danh sách việc xếp theo effort/impact · brief nghiệm thu cho
  người thực thi · vòng đo trước/sau. **Task type:** `AUDIT` + `DESIGN` (khác `DIAGNOSE` của bài 03).

## 2. Soi ngược project bài 03 qua ba lăng kính

Trước khi bóc nốt mới, đặt project hiện có cạnh ba kỹ thuật để thấy cái gì đã có ngầm, cái gì thiếu:

| Kỹ thuật | Đã có trong bài 03 | Còn thiếu |
|---|---|---|
| **Conceptual** | Phễu C1→C2→C3→PL → 6 nhánh → tín hiệu đo (`kb_02_logic`). Cấp 1 cùng tiêu chí (tầng phễu) | Nhánh 4 là lá cụt; nhánh 5 (giá) và 6 (tồn kho) chưa có nguồn dữ liệu |
| **Functional** | Quy trình 5 bước trong Master Instruction; IPO rõ | Chưa có workflow cho việc *sửa* sau khi chẩn |
| **Stakeholder** | Tách hai bản: seller tự vận hành (`EXPLAIN`) và đội vận hành (`DIAGNOSE`) — chính là stakeholder decomposition | Mới 2 vai; thiếu người thực thi creative, người duyệt, người bảo trì KB |

Bài học rút ra: project đã dùng cả ba kỹ thuật **mà không gọi tên**, nên không tự thấy được chỗ
thiếu. Gọi tên xong thì chỗ thiếu lộ ra ngay.

---

## 3. Phần A — Conceptual (Top-down) Decomposition

**Tiêu chí chia cấp 1:** *vùng trên trang chi tiết mà khách nhìn thấy, theo thứ tự cuộn trên điện
thoại* (60–70% phiên mua đến từ mobile). Một tiêu chí duy nhất cho cả năm nhánh.

**Cố ý loại khỏi cây:** phần *đo lường* (baseline, cửa sổ đo, split test). Nó không phải "vùng trên
trang" — đặt vào là mắc đúng lỗi *"cùng cấp khác loại"*. Đo lường nằm ở Functional map (Phần B).

```
Tối ưu CVR listing — biến lượt bấm thành đơn
│
├── 1. Màn hình đầu (above the fold) — thấy trước khi cuộn
│   ├── 1.1 Ảnh chính
│   │   ├── 1.1.1 Tỷ lệ 5:6 (1000×1200) hiển thị to hơn trên mobile và trang kết quả
│   │   ├── 1.1.2 Render 3D thay ảnh chụp — sửa được, tái dùng cho ảnh phụ
│   │   └── 1.1.3 Nổi bật giữa trang kết quả (curated contrast: màu, góc, badge)
│   ├── 1.2 Tiêu đề
│   │   ├── 1.2.1 Công thức: 5 từ đầu định danh sản phẩm → cho ai → khác gì; 150–180 ký tự
│   │   ├── 1.2.2 Từ khoá chính trong 60 ký tự đầu (phần hiện trên mobile)
│   │   └── 1.2.3 Kiểm reverse-ASIN trước khi sửa để không đánh rơi từ đang rank
│   ├── 1.3 Giá và offer — so với trang 1 đối thủ, coupon/deal, đơn vị tính
│   │       (chưa có nguồn dữ liệu trong bot — phải khai là chưa soi)
│   └── 1.4 Bằng chứng xã hội — điểm sao, số review, review gần nhất, so đối thủ
│
├── 2. Bộ ảnh phụ (image stack) — nơi khách "quyết mua trên điện thoại"
│   ├── 2.1 Mỗi ảnh trả lời đúng MỘT câu hỏi
│   │       (cái gì · làm gì cho tôi · an toàn không · có phải cho tôi · dùng ra sao)
│   ├── 2.2 Chữ đọc được trên mobile
│   │   ├── 2.2.1 Chiều cao chữ ≥5% ảnh · call-out 7% · số nổi bật 8,5% · đoạn A+ 4%
│   │   ├── 2.2.2 Lưới ô 5% đè lên ảnh để kiểm — chữ thấp hơn một ô là trượt
│   │   └── 2.2.3 Phép thử 3 giây: xem 3 giây rồi che, nhớ được gì?
│   ├── 2.3 Sản phẩm đang được dùng — có mặt người đúng chân dung khách, người chỉ/nhìn vào tính năng
│   ├── 2.4 Uy tín và bằng chứng — chứng nhận, giải thưởng, báo chí, người ảnh hưởng
│   ├── 2.5 Bảng so sánh "mình vs họ" (không dùng ảnh thật đối thủ, đổi cả kiểu dáng)
│   └── 2.6 Đủ số lượng — tối thiểu 6, nên 9 ảnh + 1 video
│
├── 3. Chữ trong listing — bullet, mô tả, backend
│   ├── 3.1 Bullet: vài chữ đầu là móc; Promise → Benefit → Feature; lặp điểm bán chính 2–3 chỗ
│   ├── 3.2 Ngôn ngữ của khách — rút cụm từ khách dùng trong review, đưa nguyên văn vào copy
│   ├── 3.3 Năm câu khách hỏi trước khi mua (hỏi Rufus trên listing đối thủ) → đưa CÂU TRẢ LỜI, không đưa câu hỏi
│   ├── 3.4 Ô mô tả → hỏi-đáp ngắn, dưới 2.000 ký tự (vẫn được index dù có A+)
│   └── 3.5 Từ khoá phủ đủ và đúng ý định (7 tầng PRODUCT) — lệch ý định thì traffic vào không mua
│
├── 4. A+ Content và thương hiệu
│   ├── 4.1 Hook → Educate → Persuade
│   ├── 4.2 Kỹ thuật cuộn liền mạch, carousel; chữ trong A+ ≥4%
│   ├── 4.3 Bảng so sánh nội bộ để bán chéo giữa các sản phẩm của mình
│   └── 4.4 Câu chuyện thương hiệu ở cuối — nhân hoá người bán
│
└── 5. Cấu trúc và tuân thủ — khách thấy gián tiếp
    ├── 5.1 Biến thể / ghép ASIN (parent–child) để gom traffic và review
    ├── 5.2 Nội dung mùa vụ đã hết hạn còn sót trên trang
    ├── 5.3 Đúng danh mục — sai danh mục là lỗi âm thầm, sửa là thấy ngay
    └── 5.4 Tuân thủ: không dùng tên thương hiệu đối thủ; ảnh người tạo bằng AI phải gắn nhãn (luật New York, 2026)
```

**Đếm:** 5 nhánh cấp 1 · 23 node cấp 2 · 9 node cấp 3 = **37 node**, 4 tầng.

**Checkpoint tự kiểm:**
- Cấp 1 cùng loại? — Có, đều là "vùng trên trang". Nhánh 5 yếu nhất vì "gián tiếp" — ghi nhận ở rubric.
- Trùng ý? — 1.3 (giá) và 2.5 (so sánh) chạm nhau về "vị thế cạnh tranh" nhưng khác vùng trang và
  khác nguồn dữ liệu, giữ tách. 3.3 và 3.4 cùng dùng câu hỏi của khách, nhưng 3.3 là *nguồn* còn 3.4 là
  *chỗ đặt* — giữ.
- Dạy được 3–5 buổi? — Mỗi nhánh cấp 1 là một buổi thực hành: ảnh chính + title · bộ ảnh · copy ·
  A+ · vệ sinh cấu trúc.

---

## 4. Phần B — Functional Decomposition

Hai workflow, vì hai tình huống có đầu vào khác nhau: một bên *đã có số CVR để so*, một bên *chưa có
số nào*.

### B1 · Rà listing đang chạy (CVR thấp)

```
Workflow: Rà và sửa listing có CVR thấp

INPUT  danh sách ASIN + CVR 30 ngày (đơn/lượt bấm) + mốc so sánh
       (từ bot chẩn đoán kết luận "nghẽn C3", hoặc từ bảng sức khoẻ ASIN)
  ↓
1. Chốt bằng chứng CVR thấp
   1.1 Cửa sổ đo đủ dài (≥14 ngày), mẫu đủ (không đọc tỉ lệ trên vài chục lượt bấm)
   1.2 Tách CVR quảng cáo với CVR toàn listing — hai số này khác nhau, không trộn
   1.3 So với mốc: trung vị danh mục trong Seller Central, hoặc trung vị danh mục nội bộ
  ↓
2. Chấm listing theo cây Phần A — xem trên MOBILE, vai khách hàng
   Mỗi node cấp 2: Đạt / Yếu / Trượt (Trượt phải ghi rõ vì sao)
  ↓
3. Lấy tiếng nói của khách
   3.1 Review của mình + đối thủ → cụm từ khách dùng
   3.2 Rufus trên listing đối thủ: "5 câu khách hỏi trước khi mua" → câu nào listing mình chưa trả lời?
  ↓
4. Xếp việc theo effort/impact và chọn MỘT biến
   Thứ tự mặc định: tiêu đề/copy (rẻ, nhanh) → ảnh chính → bộ ảnh phụ → A+ (đắt, tác động lớn nhất)
   Ngoại lệ: node nào Trượt quá xa so với đối thủ thì sửa trước
  ↓
5. Brief cho người thực thi, kèm tiêu chí nghiệm thu khách quan
   (luật 5% · phép thử 3 giây · công thức title · PBF) — hết cãi "đẹp hay chưa đẹp"
  ↓
6. Đổi một biến, ghi ngày đổi vào sổ thử nghiệm
  ↓
7. Đo sau: tiêu đề/ảnh chính 7 ngày · bullet 14 ngày · bộ ảnh/A+ 14–21 ngày
   Giữ hay quay lại bản cũ → sang biến kế tiếp
  ↓
OUTPUT scorecard + việc đã đổi + số trước/sau + biến kế tiếp
```

### B2 · Cổng kiểm trước khi đổ ads cho ASIN mới (pre-launch gate)

```
Workflow: Pre-launch gate

INPUT  ASIN mới, chưa có dữ liệu bán
  ↓
1. Bộ từ khoá theo 7 tầng PRODUCT + reverse-ASIN 3–5 đối thủ đang thắng
2. Copy dựng từ ngôn ngữ khách của ĐỐI THỦ (review + Rufus) — vì mình chưa có review
3. Bộ ảnh dựng theo checklist nhánh 2 (mỗi ảnh một câu hỏi, 5%, mặt người, đủ 9 + video)
4. A+ theo nhánh 4
5. CỔNG: chấm scorecard — mọi node cấp 2 phải Đạt mới được đổ tiền quảng cáo
   (đẩy traffic vào listing chưa sẵn sàng = trả giá cao cho lưu lượng không chuyển đổi)
6. Ghi ngày launch làm mốc; đo CVR lần đầu sau 14–21 ngày
  ↓
OUTPUT scorecard Đạt toàn bộ + mốc đo
```

**Checkpoint:** đủ khâu input→output — có. Bước 4 (B1) là bước lớn nhất, đã bóc thành thứ tự mặc
định + ngoại lệ. Không bước nào lặp ý — bước 3 và bước 5 đều chạm "ngôn ngữ khách" nhưng một bên là
*thu thập*, một bên là *giao việc*.

---

## 5. Phần C — Stakeholder Decomposition

| Stakeholder | Kỳ vọng | Sợ | Cần | Dùng AI để |
|---|---|---|---|---|
| **Nhân sự vận hành ASIN** (owner 40–60 ASIN) | Biết ASIN nào sửa trước, sửa cái gì, trong 15 phút | Sửa xong không nhích, không biết tại sao; bị hỏi "sao chọn cái này" | Scorecard chấm nhanh, thứ tự effort/impact, cách đo trước/sau | Chấm listing theo cây, sinh brief, nhắc lịch đo |
| **Người thực thi creative** (designer / người viết copy) | Brief rõ, tiêu chí nghiệm thu không cảm tính | Làm đi làm lại vì "chưa đẹp"; bị ép nhét quá nhiều chữ | Lưới 5%, phép thử 3 giây, danh sách ảnh kèm mục đích từng ảnh | Kiểm ảnh theo luật % trước khi gửi; sinh biến thể để test |
| **Người duyệt** (quản lý / chủ doanh nghiệp) | Thấy tiền đang treo ở đâu và việc nào đáng làm | Đội sửa nhiều mà CVR không đổi; không đo được đóng góp | Tóm một dòng/ASIN: CVR trước → sau, biến đã đổi | Tổng hợp nhật ký thử nghiệm thành báo cáo tuần |
| **Người bảo trì KB** (chủ doanh nghiệp + Claude) | Sửa tri thức một chỗ, mọi bot dùng theo | Tri thức lỗi thời vẫn được bot dùng như chuẩn; sửa KB làm hỏng phần khác | Phiên bản + ngày trên file, bộ test hồi quy, nhãn tin cậy | Đối chiếu KB với nguồn mới, chạy lại test hồi quy |
| **Seller tự vận hành** (học viên — bản phái sinh) | Hiểu vì sao listing mình không bán, tự sửa được | Áp máy móc ngưỡng của người khác; mất tiền thuê designer sai chỗ | Giải thích cơ chế ở mỗi node, ngưỡng tự đặt | Hỏi "vì sao", được hướng dẫn tự chấm |

**Checkpoint:** 5 vai, khác nhau thật — khác ở *câu hỏi họ cần trả lời* (sửa gì / làm thế nào / đáng
không / còn đúng không / vì sao). Không vai nào copy ý vai khác.

---

## 6. Phần D — So sánh ba kỹ thuật trên cùng chủ đề

| | Conceptual | Functional | Stakeholder |
|---|---|---|---|
| **Nhìn rõ điều gì** | Listing gồm những vùng nào, mỗi vùng có tiêu chí gì | Sửa theo thứ tự nào, đo lúc nào, dừng ở đâu | Ai cần output dạng nào; tiêu chí nghiệm thu của ai |
| **Đổ vào bot ở đâu** | `kb_06_listing_cvr.md` — mỗi nhánh cấp 1 một mục, mỗi node cấp 2 một tiêu chí chấm | Routing + quy trình trong Master Instruction; luật cửa sổ đo trong rule | Mẫu output theo vai trong `kb_05_output`; conversation starter riêng cho "ASIN mới" |
| **Phù hợp khi** | Dựng KB, dựng buổi dạy, dựng scorecard | Viết SOP, giao việc, đặt lịch đo | Thiết kế báo cáo, thiết kế bản phái sinh |
| **Hạn chế** | Không mang thứ tự làm, không mang ngưỡng; dễ nhồi vào cây cả thứ AI đã biết (lỗi thầy cảnh báo buổi 4) | Giả định tuyến tính — thực tế lặp lại nhiều vòng; không nói node nào quan trọng hơn | Không sinh tri thức mới; dễ viết các vai giống nhau nếu không ép tìm điểm khác |
| **Trần thật** | Trần là hiểu biết của người dựng: cây này chỉ sâu bằng 5 nguồn đã đọc | Cửa sổ đo (7/14/21 ngày) là kinh nghiệm từ nguồn, chưa kiểm trên dữ liệu của mình | Vai "người duyệt" và "bảo trì" đang là cùng một người — chưa test được xung đột |

**Khuyến nghị thứ tự:** Conceptual trước (không có cây thì không có gì để chấm) → Functional
(biến cây thành việc có thứ tự và mốc đo) → Stakeholder (định dạng output theo người nhận). Với bài
toán này, Stakeholder là bước nhẹ nhất vì bài 03 đã tách hai vai chính từ trước.

---

## 7. Lý giải lựa chọn (≈220 từ)

Em chọn tiêu chí cấp 1 là *vùng trên trang theo thứ tự cuộn trên điện thoại* vì hai lý do. Một, nó
là tiêu chí duy nhất mà cả người chấm lẫn khách hàng cùng nhìn thấy: nhân sự mở listing trên điện
thoại, cuộn từ trên xuống, chấm từng vùng — cây khớp với hành vi chấm nên dùng được ngay, không cần
dịch. Hai, nó tách sạch khỏi phễu của bài 03: phễu C1–C2–C3 chia theo *hành vi của khách* (thấy →
bấm → mua), còn cây này chia theo *cấu tạo của trang* — hai trục vuông góc, ghép vào nhau không trùng.

Em cân nhắc và bỏ hai tiêu chí khác. Chia theo *đòn bẩy effort/impact* (title → ảnh → A+) thì cấp 1
mang sẵn thứ tự làm, nhưng thứ tự đó là kinh nghiệm của một agency trên ngành supplement, không chắc
đúng với ngành mình — để nó ở Functional map (có thể đổi) an toàn hơn để ở cây (khó đổi). Chia theo
*CTR vs CVR* thì chỉ có hai nhánh, quá nông.

Cây này giúp gì cho buổi 5 và project: mỗi node cấp 2 sẽ thành một dòng trong scorecard của
`kb_06`, và taxonomy ở buổi 5 sẽ gắn hai facet lên từng node — *vùng trang* (từ cây này) và *tầng
phễu* (từ bài 03) — để bot chẩn đoán ads và module CVR tra cùng một bảng.

## 8. Cây này đổ vào kiến trúc bot thế nào

Quyết định kiến trúc dài hạn: **gộp ở cửa vào, tách ở tri thức.**

- **Một bot**, Master Instruction routing ba cửa: *ASIN đang tụt* → chẩn đoán phễu (bài 03) · *nghẽn
  C3 / rà listing* → workflow B1 · *ASIN sắp launch* → workflow B2. Hai bot thì nhân sự phải chép tay
  kết luận từ bot này sang bot kia — chỗ rơi thông tin.
- **Tách file:** `kb_01–05` giữ nguyên; thêm `kb_06_listing_cvr.md` (cây Phần A + scorecard). Tri thức
  listing đổi khi Amazon đổi thuật toán; ngưỡng ads đổi hằng tháng — hai vòng đời khác nhau, không
  nhét chung.
- **Chiêu nhỏ** (luật 5%, hỏi Rufus 5 câu, phép thử 3 giây) — trên Custom GPT là mục trong `kb_06`;
  khi chuyển sang nền tảng có skill thật thì tách thành skill, bot gọi. Đúng phân biệt Skill ≠ Agent
  của buổi 4.
- **Thứ tự triển khai:** dựng module CVR như bot *dev* riêng để test trên ASIN thật → ổn mới gộp vào
  bot chính bằng routing → chạy lại bộ test hồi quy 9 ca của bài 03 để chắc phần chẩn đoán không vỡ.
- **Rủi ro chưa đo:** khả năng retrieval của Custom GPT với 6–7 file. Bài 03 đã phải gộp 8 file còn
  5 vì lý do này. Phải test, không đoán.

## 9. Tự chấm theo rubric

| Tiêu chí | Điểm | Lý do |
|---|---:|---|
| Logic cấp 1 | 4/5 | Cùng tiêu chí "vùng trang"; trừ vì nhánh 5 là vùng "gián tiếp", yếu hơn bốn nhánh kia |
| Breadth | 4/5 | Phủ đủ 5 nguồn chuyên gia + khung CVR Health Check; thiếu hẳn nhánh *giá* vì chưa có nguồn dữ liệu — đã khai |
| Rõ cấp 2–3 | 5/5 | 37 node, đã soi hai cặp gần trùng và giữ có lý do |
| Map bổ sung | 5/5 | Hai workflow dùng được ngay: một cho ASIN đang chạy, một cho ASIN mới; kèm cửa sổ đo |
| Tính dạy được | 4/5 | Mỗi nhánh cấp 1 = một buổi thực hành; chưa có bài tập kèm |
| Clarity | 4/5 | Người ngoài đọc theo được; phần 8 hơi kỹ thuật với người mới |
| **Tổng** | **26/30** | Mức "tốt, mang sang buổi 5" |

Không tự chấm cao hơn vì cây mới được *dựng*, chưa được *chạy*: chưa nhân sự nào chấm thử một
listing bằng nó. Bài 03 đã dạy rằng cây đúng trên giấy vẫn có thể sai trên dữ liệu thật.

## 10. Cây này chạy thật thì ra gì — đợt rà đầu tiên

Chấm 4 sản phẩm thật, mỗi sản phẩm so với 2 đối thủ. Ba thứ cây **dự đoán sai hoặc chưa nghĩ tới**:

**a. Tiêu chí đếm số lượng suýt bị bỏ nhầm.** Cả 9 listing đã rà đều đạt 8,9–10 điểm chất lượng của
công cụ, nên bản nháp đầu định bỏ nhóm tiêu chí "đủ 7 ảnh, đủ bullet, có video" khỏi trọng số. Đó là
**suy luận từ mẫu lệch**: cả 9 đều của người bán chuyên nghiệp. Sửa lại thành **tầng cổng** — 0 điểm
nhưng chặn. Bài học vượt ra ngoài listing: *xây thước đo bằng cách quan sát người giỏi thì sẽ xoá đúng
những tiêu chí mà người kém trượt.*

**b. Node 5.1 (biến thể) xếp sai vùng.** Trong cây nó nằm ở nhánh 5 "cấu trúc — khách thấy gián tiếp".
Thực tế: người dẫn đầu ở **cả 4 ngách** đều có 7–27 mẫu trong khi mình có 2–4, và khách thấy dãy mẫu
**ngay màn hình đầu**. Đã nâng lên nhóm 1 của thang chấm.

**c. Cây thiếu một cổng chặn ở tầng trên nó.** 3 trong 4 sản phẩm "CVR thấp" hoá ra không thua ở trang
sản phẩm — hai con thua từ trang kết quả (có thứ hạng ở 4/894 và 141/2.163 cụm), một con thắng cụm lõi
nhưng ngách lõi quá nhỏ. Nghĩa là **cây này không được chạy một mình**: nó phải nằm sau cổng hiển thị
của bot bài 03. Đây chính là lý do kiến trúc chọn *gộp ở cửa vào, tách ở tri thức* (mục 8).

Một chi tiết nhỏ mà đắt: danh sách "khoảng trống từ khoá" của sản phẩm **tặng bà** có cụm đứng đầu là
cụm **cho ông** — vì công cụ gộp cả biến thể của đối thủ. Xếp cơ hội theo lượt tìm kiếm là đi tối ưu
nhầm sản phẩm. Node 3.5 đã được bổ sung một tiêu chí riêng cho việc lọc này.

---

## 11. Nguồn tri thức đã dùng để dựng cây

Chỉ đưa vào cây những gì **đặc thù** (số đo, công thức, thứ tự) — không chép lại framework AI đã
biết, theo đúng lời thầy buổi 4.

| Nguồn | Đóng góp vào node |
|---|---|
| Nghiên cứu Cambridge–Unilever về mobile readiness (qua George Reed, H10 Elite 10/2025) | 2.2 luật chữ 5/7/8,5/4%, lưới kiểm, phép thử 3 giây; 18 loại ảnh |
| Jon Derkits — khung IMPACT (H10 Elite 07/2026) | 1.1 tỷ lệ 5:6, 2.3 mặt người, 2.4 uy tín, 2.5 so sánh, 2.6 số lượng ảnh + video |
| Daniela — 192 split test (H10 Elite 05/2026) | Thứ tự effort/impact ở B1 bước 4; 2.1 mỗi ảnh một câu hỏi; 3.2 ngôn ngữ khách |
| Will Haire (H10 Elite 10/2025) | 1.2 công thức tiêu đề; 3.4 mô tả → hỏi-đáp; 5.3 danh mục; nhịp rà soát và mốc tụt 10–15% |
| Leo Sgovio (H10 Elite 12/2025) | 3.3 hỏi Rufus trên listing đối thủ, đưa câu trả lời |
| Khung CVR Health Check — chương trình Profit Maximizer (ECom GreenWay, 2026) | 5 đòn bẩy, PBF/HEP, cửa sổ đo 7/14/21 ngày, cổng pre-ads cho sản phẩm mới, luật "một biến một lần" |
| Bảng đánh giá listing nội bộ (2026) | 4.3, 5.1, 5.2 — các mục vệ sinh |
