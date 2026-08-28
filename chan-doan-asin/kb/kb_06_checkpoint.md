# kb_06_checkpoint.md — Hậu kiểm trước khi trả báo cáo

**version:** v1.1 · **cập nhật:** 28/08/2026 · **dùng chung** cho mọi bản

Bot chạy file này **sau khi soạn xong báo cáo, trước khi in ra**. `kb_04_rules` là tiền kiểm (luật
để làm đúng); file này là hậu kiểm (soi cái đã làm có đúng luật không). Mục nào không đạt thì **sửa
báo cáo rồi mới trả**, không trả kèm lời xin lỗi.

Cuối báo cáo in đúng một dòng: `Tự kiểm: N/7 đạt` — và nếu có mục phải sửa, ghi mục nào.

---

## 1. Kiểm nguồn số

- Mỗi con số trong báo cáo có kèm **tên file** (hoặc "người dùng cung cấp" / "bot tính từ …") và
  **cửa sổ thời gian** chưa?
- Số dòng đã đọc của từng file có in ở đầu báo cáo chưa?
- Số nào thiếu một trong hai thứ trên → **xoá số đó**, thay bằng "chưa có dữ liệu" và nêu lấy ở đâu
  (`kb_01`).

## 2. Kiểm cổng

- Đã hỏi gộp bốn thông tin đầu phiên (cửa sổ · biên lợi nhuận · mục tiêu · ngưỡng cắt)? Người dùng
  chưa trả lời mục nào thì báo cáo có ghi rõ **giới hạn do thiếu mục đó** không?
- Đã hỏi mô hình kinh doanh? Nếu là mô hình mua lại nhiều lần → báo cáo này **không được tồn tại**,
  chỉ có thông báo ngoài phạm vi.
- Mức phủ đã in ra và người dùng đã chọn "chẩn luôn" hay "bổ sung" chưa?

## 3. Kiểm ngưỡng — bot có tự đặt ngưỡng không?

- Có kết luận nào dựa trên một con số ngưỡng mà **người dùng chưa cung cấp** và `kb_03` không có?
  → Bỏ phán xét, chuyển thành **trình bày vị trí**: nêu con số, nhắc đối chiếu với điểm hoà vốn.
- Có ngưỡng nào lấy từ kho tri thức mà thiếu nhãn "cần đối chiếu với ngưỡng hiện hành" không?

## 4. Kiểm cửa sổ thời gian

- Cửa sổ 7 ngày mà báo cáo có chữ "cắt", "dừng", "tha" campaign? → Hạ về **"điểm nóng, cần đọc
  14–30 ngày mới kết luận"**.
- Cửa sổ chạm 3 ngày gần nhất mà kết luận ROAS/ACOS/đơn không mang nhãn "số chưa chốt"?
- Có phép tính nào trộn hai file có hai cửa sổ khác nhau (ví dụ chia số 7 ngày cho số 30 ngày)?
- Mẫu dưới ~50 lượt bấm hoặc ~5 đơn mà báo cáo vẫn đọc tỉ lệ (CTR, CVR, ROAS)?

## 5. Kiểm logic chẩn đoán

- Tầng nghẽn đã nói rõ **vì sao là tầng đó mà không phải tầng khác** chưa?
- Nêu hai nguyên nhân thì đã nói cái nào gốc, cái nào hệ quả chưa? Liệt kê song song = chưa đạt.
- Nhánh không có dữ liệu có ghi đúng chữ **"không có dữ liệu"** — không được để người đọc hiểu là
  "không có vấn đề"?
- Mã ASIN khác xuất hiện trong tên campaign có bị đọc nhầm thành sản phẩm của người dùng không?
- Có kết luận "ASIN ổn" trong khi mới soi được một phần các nhánh không? → Cấm.

## 6. Kiểm đầu ra

- Đầu báo cáo có **dòng định hướng của đúng bản đang dùng**, kèm một câu lý do **có số**?
  Bản dùng cho đội vận hành nhiều ASIN: `MỨC ƯU TIÊN: Cao / Vừa / Thấp`.
  Bản dùng cho người tự vận hành một ASIN: `VIỆC CẦN LÀM TRƯỚC`.
  Lấy nhầm dòng của bản kia là **không đạt** — xem `kb_05_output.md` của bản đang chạy.
- Mỗi kết luận có **một trong ba nhãn**: đã loại trừ đủ · kết luận tạm · chưa kết luận được?
- Có khối **việc tiếp theo** 1–3 việc đặt ngay dưới kết luận — kể cả khi chưa kết luận được?
- Mỗi việc đề xuất có kèm: **vì sao** · chỉ số để biết có tác dụng · cửa sổ đo?
- Mỗi việc có **đối tượng gọi đích danh** và **một ngưỡng**? "Tối ưu listing" là chủ đề, không phải việc.
- Mỗi việc có nói **cái gì kích hoạt** nó — tín hiệu cụ thể trong dữ liệu, khác với cơ chế "vì sao"?
- Có khối **BÀN GIAO** ở cuối, đủ cột, và **mốc trước là số đã đo** — không phải số kỳ vọng?
- Có con số tiền hay phần trăm nào **kỳ vọng thu về sau khi làm** bị in ra không? → Xoá.
- Đề xuất có đứng **sau** kết luận nguyên nhân, không trộn vào lúc chẩn?
- Đường đi đầy đủ của phễu có bị in ra khi người dùng chưa gõ `chi tiết` không?
- Dòng cuối có phiên bản bộ kiến thức + ngày chưa?

## 7. Kiểm phạm vi và giọng

- Báo cáo có nói tới số liệu của ASIN khác ngoài ASIN đang chẩn không? → Xoá.
- Có câu nào quy lỗi cho cá nhân (tên người trong tên campaign / portfolio)? → Đổi sang mức việc.
- Có từ nào trong danh sách từ cấm của `kb_05`, hoặc câu hứa kết quả? → Viết lại theo bảng thay thế.
- Thuật ngữ tiếng Anh xuất hiện lần đầu trong phiên đã có cụm giải thích ngắn chưa (bản dành cho
  seller tự vận hành)?
