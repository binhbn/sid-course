# Bản đề xuất tối ưu — chờ anh Bình duyệt

**Ngày:** 16/08/2026 · **Soi trên:** bộ 8 file KB + Master Instruction + output mẫu 3 ASIN.
**Trạng thái:** chưa triển khai gì. Anh tích chọn rồi em làm.

Hai lăng kính chạy độc lập: **người sử dụng** (nhân sự MKT vận hành 40–60 ASIN) và **chuyên gia
thiết kế Custom GPT**.

> **Lưu ý về tên file:** tài liệu này viết trên bộ KB **v1 (8 file)**. Sau khi soi xong, bộ KB được
> gộp lại còn **v2 (5 file)** và tách làm hai bản. Bảng quy đổi:
> `kb_data_schema` → `kb_01_data` · `kb_causes` + `kb_tree` + `kb_coverage` → `kb_02_logic` ·
> `kb_thresholds` → `kb_03_thresholds` · `kb_rules` → `kb_04_rules` · `template_output` → `kb_05_output`.
> Tên cũ được giữ nguyên trong phần phân tích để không viết lại lịch sử.

---

# Phần A — Lăng kính NGƯỜI SỬ DỤNG

Đặt mình vào một bạn MKT sáng thứ Hai, có 6 ASIN đang tụt cần soi, mỗi ASIN muốn xong trong 10 phút.

### A1 · Phải trả lời 3 câu hỏi mới thấy được gì ⚠️ Nặng

Trước khi có một chữ kết quả, bot hỏi: cửa sổ thời gian · ngưỡng per-ASIN · ngưỡng ROAS cắt. Với
người soi 6 ASIN một buổi thì đó là 18 lần trả lời cho những thứ **gần như không đổi giữa các ASIN**.

Kết cục dễ đoán: gõ bừa cho xong, hoặc bỏ tool.

**Đề xuất:** gộp thành **một câu hỏi duy nhất, khai một lần cho cả phiên**, và cho phép trả lời
kiểu *"7 ngày, chưa có ngưỡng riêng, ROAS cắt 1.5"*. Bot nhớ trong phiên, các ASIN sau chỉ hỏi lại
nếu người dùng đổi.

### A2 · "Đường đi của cây" chiếm nhiều chỗ nhất nhưng ít người đọc nhất ⚠️ Vừa

8 dòng liệt kê từng node. Người vận hành cần biết **làm gì tiếp**, không cần xem bot suy nghĩ.
Nhưng khi bot chẩn sai thì đây lại là chỗ duy nhất kiểm được — nên không bỏ hẳn được.

**Đề xuất:** mặc định thu còn **một dòng tóm tắt**, kèm câu *"gõ `chi tiết` để xem đường đi đầy đủ"*.
Người kiểm vẫn có, người dùng hằng ngày không bị ngợp.

### A3 · Báo cáo "chưa kết luận được" đọc như bot bỏ cuộc ⚠️ Nặng

Báo cáo ASIN ██████████ gần như không kết luận gì. Về mặt trung thực thì đúng — mẫu 50 click/2 đơn thật
sự không đọc được tỉ lệ. Nhưng với nhân sự thì nó đọc như *"tôi không biết"*, và phản xạ tiếp theo
là tự suy diễn — đúng cái tool sinh ra để tránh.

**Đề xuất:** khi rơi vào "chưa kết luận được", bot **bắt buộc** đưa một khối **VIỆC TIẾP THEO** đặt
ngay dưới kết luận (không phải cuối báo cáo), gồm 1–3 việc cụ thể làm được ngay hôm nay. Hiện thông
tin này có nhưng nằm ở mục cuối, dễ bị bỏ qua.

### A4 · Không có mức độ ưu tiên ⚠️ Nặng

Nhân sự theo 40–60 ASIN. Câu hỏi thật của họ không phải *"ASIN này bị gì"* mà là ***"trong 6 ASIN
đang tụt, xử cái nào trước"***. Báo cáo hiện không trả lời.

**Đề xuất:** thêm một dòng **MỨC ƯU TIÊN** ngay đầu báo cáo (Cao / Vừa / Thấp) kèm một câu lý do —
ví dụ *"Cao: ads đang tiêu $28,73 vào nhóm không ra đơn"* hoặc *"Thấp: chưa đủ mẫu để kết luận, chờ
thêm dữ liệu"*.

### A5 · Ngôn ngữ của người dựng, không phải của người dùng ⚠️ Vừa

*"Mức phủ 4/6 nhánh"*, *"node tiên quyết"*, *"tiền thật/tiền giả"*, *"kết luận tạm"* — đây là từ
vựng em dựng ra khi thiết kế, nhân sự chưa chắc hiểu.

**Đề xuất:** dịch sang ngôn ngữ nghiệp vụ ở tầng hiển thị, giữ nguyên ở tầng KB:
- "mức phủ 4/6 nhánh" → *"đã soi 4 trong 6 nhóm nguyên nhân; còn thiếu dữ liệu giá và tồn kho"*
- "kết luận tạm" → *"kết luận này còn phụ thuộc dữ liệu chưa có"*

### A6 · Đòi file mà không nói lấy ở đâu ⚠️ Vừa

Bot bảo *"cần Search Term Report"* nhưng không nói lấy ở màn hình nào. Nhân sự 1–2 năm kinh nghiệm
có thể biết, người mới thì không.

**Đề xuất:** mỗi lần đòi dữ liệu phải kèm **đường lấy cụ thể** (công cụ nào, menu nào, chọn cửa sổ
bao nhiêu ngày). Đặt vào `kb_data_schema.md` một bảng "lấy file ở đâu".

### A7 · Bốn cảnh báo đọc số nằm cuối, dễ bị bỏ qua ⚠️ Thấp

Cảnh báo quan trọng nhất — *"số chưa chín, ROAS còn thay đổi"* — nằm ở đáy báo cáo, sau khi người
đọc đã tin vào con số ở trên.

**Đề xuất:** cảnh báo nào **làm thay đổi cách đọc kết luận** thì kéo lên ngay dưới kết luận, dạng
một dòng. Cảnh báo kỹ thuật còn lại giữ ở cuối.

---

# Phần B — Lăng kính CHUYÊN GIA THIẾT KẾ CUSTOM GPT

### B1 · 8 file KB là quá nhiều cho cơ chế retrieval của Custom GPT ⚠️ Nặng

Custom GPT không đọc toàn bộ Knowledge mỗi lượt — nó **tìm theo ngữ nghĩa** rồi nạp phần liên quan.
Càng nhiều file, xác suất nạp thiếu càng cao. Rủi ro lớn nhất: `kb_rules.md` (guardrail) hoặc
`kb_thresholds.md` **không được nạp** đúng lúc cần, mà bot vẫn trả lời như thường — sai im lặng.

**Đề xuất:** gộp còn **5 file** theo vòng đời sửa đổi, không theo chủ đề:

| Gộp thành | Từ | Lý do |
|---|---|---|
| `kb_01_data.md` | data_schema | Sửa khi Amazon đổi report |
| `kb_02_logic.md` | causes + tree + coverage | Sửa khi hiểu biết nghiệp vụ đổi — ba file này luôn sửa cùng nhau |
| `kb_03_thresholds.md` | thresholds | Sửa thường xuyên nhất, phải đứng riêng |
| `kb_04_rules.md` | rules | Guardrail, gần như không đổi |
| `kb_05_output.md` | template_output + styleguide | Sửa khi đổi audience |

Vẫn đúng tinh thần component hoá, nhưng ranh giới đặt theo **cái gì thay đổi cùng nhau**.

### B2 · Chưa có gì đảm bảo bot tính bằng code thay vì đếm bằng mắt ⚠️ Nặng

Với file nhỏ, GPT có thể tự "đọc" nội dung và tự đếm — và đếm sai mà không ai biết. Đây là dạng lỗi
nguy hiểm nhất vì kết quả trông vẫn hợp lý.

**Đề xuất:** Master Instruction thêm luật cứng: **mọi con số phải tính bằng code**, và bot phải in
kèm số dòng đã đọc được của từng file để người dùng đối chiếu. Không có số dòng thì coi như chưa đọc.

### B3 · Không có phiên bản ⚠️ Vừa

Anh sửa ngưỡng hôm nay, nhân sự chạy ngày mai — không ai biết đang dùng bản nào. Khi kết quả khác
lần trước, không truy được vì sao.

**Đề xuất:** mỗi file KB có dòng `version` + ngày ở đầu; bot in một dòng cuối báo cáo:
*"KB v1.2 · cập nhật 16/08/2026"*.

### B4 · Luật file nặng chưa được kiểm bằng hành vi thật ⚠️ Vừa

File Targeting 11 MB / 117.596 dòng gần như chắc chắn vượt khả năng xử lý trong một lượt. KB đã có
luật báo thẳng, nhưng chưa ai kiểm bot có tuân không — nó có thể đọc một phần rồi kết luận.

**Đề xuất:** bot **in kích thước và số dòng trước khi tính**, và tự so với mức trần. Đây là cách
duy nhất phát hiện được nó đọc thiếu.

### B5 · Không có bộ test hồi quy ⚠️ Vừa

Mỗi lần sửa KB là một lần có thể làm hỏng chỗ khác — đúng như buổi 3 đã chứng minh: sửa lỗi của buổi
2 sinh ra lỗi nặng hơn.

**Đề xuất:** đóng gói **3 ASIN này thành bộ test chuẩn**, ghi kết quả mong đợi của từng cái. Sau mỗi
lần sửa KB thì chạy lại 3 ca, lệch thì biết ngay.

### B6 · Custom GPT không có state giữa phiên ⚠️ Thấp — ghi nhận, chưa xử

Chẩn ASIN hôm nay, tuần sau chẩn lại thì không so được với lần trước. Đây là giới hạn nền tảng.

**Đề xuất:** chưa làm gì. Ghi vào phần giới hạn để người dùng biết. Muốn xử thật thì cần database
ngoài — vượt phạm vi bài tập.

### B7 · Thiếu `kb_styleguide.md` ⚠️ Vừa

Bộ KB đang thiếu đúng file mà demo Profile Curator nhấn mạnh. Giọng văn và mức chi tiết hiện nằm rải
trong `template_output.md` và Master Instruction — mà đó chính là hai thứ **phải đổi khi chuyển sang
bản EGW**.

**Đề xuất:** tách ra thành file riêng ngay từ bây giờ, không đợi tới lúc làm bản EGW.

---

# Phần C — Danh sách để anh tích duyệt

Sắp theo **giá trị / công sức**.

| # | Việc | Vai | Mức | Công | Duyệt |
|---|---|---|---|---|---|
| 1 | Thêm dòng **MỨC ƯU TIÊN** đầu báo cáo | A4 | Nặng | Thấp | ☐ |
| 2 | Gộp 3 câu hỏi thành 1, nhớ trong phiên | A1 | Nặng | Thấp | ☐ |
| 3 | Khối **VIỆC TIẾP THEO** khi chưa kết luận được | A3 | Nặng | Thấp | ☐ |
| 4 | Luật **tính bằng code + in số dòng đã đọc** | B2 | Nặng | Thấp | ☐ |
| 5 | Thu gọn "đường đi của cây", mở rộng theo yêu cầu | A2 | Vừa | Thấp | ☐ |
| 6 | Kéo cảnh báo đổi-cách-đọc lên đầu | A7 | Thấp | Thấp | ☐ |
| 7 | Dịch thuật ngữ sang ngôn ngữ nghiệp vụ | A5 | Vừa | Vừa | ☐ |
| 8 | Bảng "lấy file ở đâu" | A6 | Vừa | Thấp | ☐ |
| 9 | Thêm `kb_styleguide.md` | B7 | Vừa | Vừa | ☐ |
| 10 | **Gộp 8 file KB còn 5** | B1 | Nặng | Vừa | ☐ |
| 11 | Version + ngày trên mỗi file KB | B3 | Vừa | Thấp | ☐ |
| 12 | Bot in kích thước file trước khi tính | B4 | Vừa | Thấp | ☐ |
| 13 | Bộ test hồi quy 3 ASIN | B5 | Vừa | Vừa | ☐ |
| 14 | **Artifact mẫu báo cáo, áp brand** | việc 5 | — | Cao | ☐ |
| 15 | Framing Brief v2 (4 thay đổi ở Overview mục 4) | Overview | Vừa | Vừa | ☐ |

**Gợi ý của em nếu anh muốn gọn:** duyệt **1–6** trước (đều tác động lớn, công sức thấp, làm xong
trong một lượt), rồi tính tiếp. Riêng **10** nên làm trước khi cài lên Custom GPT, vì cài xong mới
gộp thì phải upload lại toàn bộ.

**Việc 14 (artifact)** em để cuối vì nó chỉ có nghĩa sau khi nội dung báo cáo đã chốt — làm đẹp một
bố cục sắp thay đổi là làm hai lần.
