# Bài tập cuối Buổi 4 — Câu lệnh tổng hợp

**Nguồn dữ liệu:** `bai-04-sample-02.md` (Trường THCS Hoà Bình, năm học 2025–2026) — **duy nhất**
**Sản phẩm:** [`report.md`](report.md)
**Chạy trên:** Claude Opus 5 trong Claude Code — 04/09/2026

---

## Câu lệnh

```text
### ROLE
Bạn là chuyên viên phân tích dữ liệu giáo dục, viết báo cáo cho Ban giám hiệu một trường THCS.
Bạn viết để Ban giám hiệu RA QUYẾT ĐỊNH phân bổ nguồn lực hè và năm học tới, không viết để lưu hồ sơ.

### TASK
Đọc tệp bai-04-sample-02.md và tạo MỘT báo cáo Markdown hoàn chỉnh, đúng bảy phần dưới đây,
đúng thứ tự này:

1. Tóm tắt điều hành — 5 đến 8 ý. Mỗi ý phải nêu được một trong bốn thứ: kết quả, mức tiến bộ,
   rủi ro, hoặc việc cần ưu tiên. Không ý nào chỉ nhắc lại một con số mà không nói nó nghĩa là gì.
2. Các yếu tố liên quan — phân loại các yếu tố thành nhóm, rồi phân tích theo TỪNG MÔN
   (đủ cả bốn môn: Toán, Ngữ văn, Tiếng Anh, Khoa học tự nhiên).
3. Bản đồ khái niệm theo thời gian — Mermaid.
4. Sơ đồ quy trình kiểm tra, đánh giá — Mermaid.
5. Bảng tổng kết thành tích — điểm toàn trường, phân bố thành tích, môn và lớp nổi bật,
   nhóm cần hỗ trợ.
6. Khuyến nghị hành động — 3 đến 5 đề xuất, mỗi đề xuất gắn thẳng vào một con số cụ thể trong tệp.
7. Hạn chế dữ liệu — nêu rõ điều gì chưa thể kết luận.

### CONTEXT — nguồn và giới hạn
- Nguồn dữ liệu DUY NHẤT là tệp bai-04-sample-02.md. Không tra Internet. Không lấy chuẩn ngành,
  không lấy số liệu trường khác, không dùng kiến thức nền về giáo dục để bổ sung số.
- Trường có 8 lớp, 320 học sinh, thang điểm 10, ngưỡng cần hỗ trợ là điểm trung bình môn dưới 5,0.
- Không suy đoán giới tính, hoàn cảnh kinh tế, hay bất kỳ thông tin cá nhân nào. Tệp không có
  những dữ liệu đó và mục 12 của tệp cấm suy đoán chúng.

### NĂM LUẬT CỨNG — vi phạm một luật là hỏng cả báo cáo
LUẬT 1 — Cấm cộng chéo để suy ra số học sinh duy nhất.
  Bảng "Học sinh cần hỗ trợ theo môn" đếm theo từng môn và một học sinh có thể nằm ở nhiều môn.
  Cộng bốn hàng đó lại KHÔNG ra số học sinh cần hỗ trợ của trường. Số học sinh duy nhất phải lấy
  từ bảng phân bố thành tích và bảng chuyên cần.
LUẬT 2 — Cấm biến chênh lệch quan sát thành nguyên nhân.
  Chỉ được dùng các cụm: "có liên hệ với", "đi kèm với", "chênh lệch quan sát là".
  Cấm tuyệt đối các chữ: "do", "vì", "khiến", "dẫn đến", "gây ra", "nhờ", "tác động", "hiệu quả của".
LUẬT 3 — Mọi con số phải truy vết được.
  Mỗi con số xuất hiện trong báo cáo phải ghi mục nguồn dạng [§6] ngay cạnh nó, hoặc nằm trong một
  bảng có cột nguồn. Số nào bạn tự tính thì phải ghi cả phép tính.
LUẬT 4 — Thiếu dữ liệu thì ghi "Chưa đủ dữ liệu".
  Không được lấp bằng suy đoán, không được lặng lẽ bỏ mục đó đi.
LUẬT 5 — Ưu tiên khả năng ra quyết định, không chép lại dữ liệu.
  Không bê nguyên các bảng của tệp vào báo cáo. Chỉ đưa con số nào đỡ cho một nhận định
  hoặc một khuyến nghị.

### YÊU CẦU RIÊNG CHO HAI SƠ ĐỒ
Phần 3 — bản đồ khái niệm theo thời gian:
  Nối bốn loại thực thể: mốc thời gian, biện pháp can thiệp, hành vi học tập, kết quả quan sát.
  Mọi cạnh phải được GỌI TÊN. Tên quan hệ không được mang nghĩa nhân quả — dùng những tên như
  "diễn ra tại", "nhắm vào", "đi kèm với", "được đo tại".
  Đây là bản đồ QUAN HỆ, không phải dòng thời gian xếp thẳng hàng.
Phần 4 — sơ đồ quy trình:
  Dựng theo quy trình 9 bước có sẵn ở mục 11 của tệp, cộng các mốc ở mục 10.
  Bắt buộc có ít nhất hai điểm rẽ nhánh theo điều kiện và ít nhất một nhánh quay lại bước trước
  (đường xử lý trường hợp bất thường).
  Bước nào tệp không nói mà bạn suy ra thì gắn chữ "Giả định" trên nhãn.

### EVALUATION — chạy đủ bảy bước này trước khi trả lời
1. Quét toàn bộ bản nháp tìm các chữ cấm ở LUẬT 2. Còn chữ nào thì viết lại câu đó.
2. Tìm mọi phép cộng trong bài. Với mỗi phép cộng, tự hỏi: các số hạng có đếm cùng một tập
   đối tượng không? Nếu không chắc, bỏ phép cộng đó.
3. Kiểm chéo: những con số bạn tự tính có khớp với dòng "Toàn trường" của tệp không?
4. Đếm số ý ở phần 1 (phải 5-8) và số khuyến nghị ở phần 6 (phải 3-5).
5. Phần 2 đã phân tích đủ cả bốn môn chưa?
6. Có con số nào chưa có mục nguồn không?
7. Có chỗ nào bạn viết một nhận định mạnh mà mục 12 của tệp đã cảnh báo là không đủ căn cứ không?
   Nếu có, hạ giọng và ghi rõ mục 12 cảnh báo gì.
```

---

## Ghi chú về cách viết câu lệnh này

**Vì sao năm luật cứng nằm riêng một khối, không trộn vào phần mô tả nhiệm vụ.** Khi làm bài tập 3,
em thấy AI đọc ràng buộc chống suy diễn nhân quả ở phần nhiệm vụ rồi vẫn viết trôi theo mạch văn —
vì lúc viết, nó đang bám theo *mạch lập luận*, không bám theo *danh sách ràng buộc* đã đọc từ đầu.
Tách thành khối riêng, đánh số, rồi nhắc lại ở mục EVALUATION dưới dạng **việc phải quét lại**, thì
ràng buộc mới có hiệu lực ở bước cuối chứ không chỉ ở bước đầu.

**Vì sao LUẬT 2 liệt kê chữ cấm thay vì nói nguyên tắc.** "Không kết luận nhân quả" là một nguyên
tắc trừu tượng, AI gật đầu rồi vẫn vi phạm. Danh sách chữ cấm cụ thể (`do`, `vì`, `khiến`, `nhờ`…)
biến nó thành thứ **quét được bằng mắt** ở bước tự kiểm.

**Vì sao LUẬT 1 nói rõ phải lấy số ở đâu.** Chỉ cấm "đừng cộng" là chưa đủ — AI vẫn cần một con số
để viết. Không chỉ chỗ lấy thay thế thì nó sẽ tự xoay ra một cách tính khác cũng sai. Nên luật vừa
cấm đường sai vừa chỉ đường đúng.
