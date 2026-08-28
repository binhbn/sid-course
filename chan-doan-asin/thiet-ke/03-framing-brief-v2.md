# Framing Brief v2 — Chẩn đoán hiệu suất ASIN

**Học viên:** Bùi Ngọc Bình · **Ngày:** 16/08/2026
**Thay cho:** `bai-lam/bai-01/framing-brief-chan-doan-asin.md` (v1, 30/07/2026)

Bản v1 **không sửa đè** — giữ nguyên để thấy được quá trình. Bản này ghi nhận những gì đã thay đổi
sau hai vòng test trên dữ liệu thật, và **giải thích vì sao**.

---

## 1. Chủ đề

Hiệu suất sản phẩm (ASIN) trong kinh doanh Amazon.

## 2. Bài toán và mục tiêu

Khi hiệu suất một ASIN không tốt (ROAS thấp, đơn ít), nhân sự không biết vấn đề nằm ở đâu nên không
tập trung được vào giải pháp phù hợp.

**Bổ sung ở v2:** bài toán thật của người soi 40–60 ASIN không dừng ở *"ASIN này bị gì"* mà là
***"trong mấy ASIN đang tụt, xử cái nào trước"***. Vì vậy đầu ra phải có mức ưu tiên, không chỉ có
kết luận nguyên nhân.

## 3. Audience

Nhân sự MKT vận hành sản phẩm trên Amazon, 1–2 năm kinh nghiệm, mỗi người theo 40–60 ASIN.

**Không đổi so với v1.** Việc giữ đúng một audience xuyên khoá là quyết định có chủ đích: bản cho
học viên EGW là **sản phẩm phái sinh**, làm sau khi bản nội bộ đã được kiểm chứng.

## 4. Bối cảnh

Nhân sự đã được đào tạo knowhow nội bộ nhưng chưa áp dụng được vào ca cụ thể. Họ có Amazon Ads
Console, Seller Central và công cụ ranking từ khoá.

**Bổ sung ở v2 — điều kiện vận hành thật:**
- Dữ liệu vào là **file export thô**, không phải số đã chuẩn hoá.
- Bốn nguồn dữ liệu mang **bốn cửa sổ thời gian khác nhau**.
- Nhân sự **không phải lúc nào cũng nộp đủ** — công cụ phải chạy được với dữ liệu thiếu.

## 5. Scope

### Trong phạm vi

- Chẩn đoán ASIN thuộc mô hình **lời trên từng đơn** (gifting, đồ dùng mua một lần).
- Định vị nghẽn theo phễu **C1 Impression → C2 Click → C3 Conversion → PL**.
- Sáu nhánh nguyên nhân treo dưới phễu.
- **Đề xuất hướng xử lý** sau khi đã ra kết luận nguyên nhân.
- Cảnh báo điều kiện chuẩn bị mùa vụ.

### Ngoài phạm vi

- **Mô hình khai thác giá trị vòng đời khách hàng** (thực phẩm chức năng, đồ tiêu hao, hàng mua lại
  định kỳ) — có cổng chặn riêng, bot phải từ chối chẩn.
- So sánh nhiều ASIN để xếp thứ tự xử lý (nhiệm vụ `COMPARE`, bài toán riêng).
- Dự đoán hiệu quả tương lai.
- Kênh ngoài Amazon.
- Quyết định thay người dùng (cắt ASIN, dừng campaign).

## 6. Output và Task Type

- **Task type chính:** `DIAGNOSE`
- **Task type phụ:** `DESIGN` — phần đề xuất hướng xử lý, chỉ mở sau khi có kết luận nguyên nhân.
- **Đầu ra:** báo cáo chẩn đoán gồm mức ưu tiên · kết luận có nhãn tin cậy · việc tiếp theo · tầng
  nghẽn · nguyên nhân gốc kèm nguyên nhân phụ thuộc và quan hệ giữa chúng · danh sách nhánh chưa
  loại trừ được.

## 7. Rủi ro nếu framing sai

Kết quả chung chung không dùng được, hoặc — nguy hiểm hơn — **kết luận sai một cách tự tin**. Nhân
sự tin và đi sửa sai chỗ, không ai biết vì báo cáo trông vẫn gọn gàng.

---

## 8. Năm thay đổi so với v1, và vì sao

| # | v1 chốt | v2 | Vì sao đổi |
|---|---|---|---|
| 1 | **Ngoài phạm vi: chưa đề xuất phương án sửa**, đó là phase 2 | Có mục **Việc tiếp theo** và **Hướng xử lý** | Người dùng cần cầm về được việc làm ngay. Giữ nguyên nguyên tắc tách phase: chỉ mở phần xử lý **sau khi** có kết luận nguyên nhân |
| 2 | **Tối đa 5 vấn đề trọng tâm** | **6 nhánh** | Các nhóm nguyên nhân tự nhiên tách thành 6; ép xuống 5 buộc phải gộp hai nhóm khác cơ chế, vi phạm chính ràng buộc "hai nhánh không cùng đúng" |
| 3 | **Chạy hết cây ra MỘT kết luận** | Nguyên nhân gốc **+ phụ thuộc + quan hệ** | Ba ca thật đều có hai nguyên nhân cùng tồn tại và có quan hệ nhân quả. Giữ tinh thần chống "danh sách khả năng", bỏ ràng buộc "đúng một" |
| 4 | Không nêu mô hình kinh doanh | **Cổng chặn mô hình** ở đầu | Toàn bộ knowhow rút từ mô hình lời-trên-đơn. Chẩn một ASIN vòng đời bằng bộ luật này ra kết luận nghe hợp lý mà sai về mô hình — không có gì báo |
| 5 | Đầu ra là **cây quyết định** | Đầu ra là **báo cáo chẩn đoán**, cây là cơ chế bên trong | Artifact bàn giao cho người dùng là báo cáo, không phải cái cây. Cây là thứ bot dùng, người dùng chỉ thấy khi gõ `chi tiết` |

## 9. Một thay đổi về phương pháp, không nằm trong bảng trên

Ở v1 em xếp **thứ tự soi theo chi phí soi** — nhánh nào nhìn nhanh nhất thì đặt trước. Test buổi 2
cho thấy sai: thứ dễ soi nhất (tỉ lệ chuyển đổi, listing) lại chính là **hệ quả**, nên cây dừng sớm
và kết luận nhầm.

Buổi 2 em chữa bằng cách thêm **node tiên quyết** đứng trước mọi node khác. Test buổi 3 trên dữ liệu
thật cho thấy cách chữa đó **đẻ ra lỗi nặng hơn**: node tiên quyết có quyền chặn tuyệt đối nên cây
bỏ sót đúng ASIN có vấn đề lớn nhất.

v2 bỏ hẳn khái niệm "node tiên quyết" và chuyển sang **phễu C1-C2-C3-PL** — khung đã có sẵn trong
kho knowhow nội bộ ở mức cao nhất. Phễu **tự nó mang thứ tự nhân quả** nên không node nào cần quyền
đặc biệt nữa.

**Điều rút ra:** một node có quyền tuyệt đối luôn tạo ra điểm mù, dù đặt ở đầu nào của cây. Và khung
đúng thì đã nằm sẵn trong kho — em chỉ không hỏi tới nó.

---

## Rubric tự chấm

| Tiêu chí | v1 | v2 | Lý do đổi điểm |
|---|---:|---:|---|
| Problem clarity | 5/5 | 5/5 | — |
| Goal/Output frame | 4/5 | 5/5 | Đầu ra nay là báo cáo có mức ưu tiên và việc tiếp theo — dùng được ngay, không cần diễn giải thêm |
| Audience fit | 5/5 | 5/5 | — |
| Scope quality | 4/5 | 5/5 | Đã có bảng chẩn-được / không-chẩn-được, và cổng chặn mô hình kinh doanh |
| Task definition | 5/5 | 4/5 | **Trừ điểm**: nay có hai task type (`DIAGNOSE` + `DESIGN`) cộng một mục cảnh báo mùa vụ không thuộc cả hai. Sạch hơn nếu tách hẳn, nhưng người dùng cần chúng trong cùng một báo cáo |
| Clarity & usability | 4/5 | 5/5 | Người khác đọc brief này dựng lại được hệ |
| **Tổng** | **27/30** | **29/30** | |

Em không tự chấm 30 vì mục Task definition thật sự có lấn — và em chọn lấn một cách có ý thức, đổi
độ sạch của framing lấy tính dùng được.
