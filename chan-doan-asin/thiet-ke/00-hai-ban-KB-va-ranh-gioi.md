# Vì sao có hai bản KB, và ranh giới cắt ở đâu

**Bài buổi 3 · bộ kiến thức v2.2 (28/08/2026)**
Hướng dẫn cài đặt nằm ở [`../README.md`](../README.md); file này chỉ trả lời câu *vì sao nó được cắt như vậy*.

---

## Một bài toán, hai người dùng → hai bot

Cùng một việc "chẩn đoán ASIN đang tụt", nhưng **đổi người dùng thì đổi luôn task type và đổi luôn
artifact** — đúng bài học buổi 1. Nên tách hai bot thay vì một bot cố phục vụ cả hai.

| | Bản EGW *(bản trong repo này)* | Bản nội bộ *(không công khai)* |
|---|---|---|
| Người dùng | Seller tự vận hành, vài ASIN, không có team | Đội vận hành chuyên nghiệp, mỗi người 40–60 ASIN |
| Task type | `EXPLAIN` — giải thích cơ chế để lần sau tự soi được | `DIAGNOSE` — kết luận nhanh, chọn ASIN nào xử trước |
| Ngưỡng | **Người dùng tự đặt**, bot hướng dẫn cách tính điểm hoà vốn | Có sẵn bảng ngưỡng của tổ chức |
| Đầu báo cáo | *Việc cần làm trước* — thường chỉ soi một ASIN mỗi lần | *Mức ưu tiên Cao/Vừa/Thấp* — để xếp hàng chục ASIN |
| Mỗi đề xuất | Kèm một câu **vì sao** (người dùng đang học cách tự soi) | Chỉ nêu việc |
| Thuật ngữ | Giải thích lần đầu trong phiên | Dùng trơn |

## Ranh giới cắt ở đâu — và vì sao cắt được sạch

Hai bản khác nhau **đúng ba file**: `master-instruction`, `kb_03_thresholds`, `kb_05_output`.
Bốn file còn lại (`kb_01`, `kb_02`, `kb_04`, `kb_06`) dùng chung nguyên vẹn.

Cắt được sạch là vì **bốn file dùng chung không chứa một con số ngưỡng nào**. Toàn bộ số nằm ở `kb_03`,
toàn bộ giọng nằm ở `kb_05`. Ranh giới đặt theo đúng một tiêu chí: ***cái gì thay đổi khi đổi người dùng***.

> Trong thư mục [`../kb/`](../kb/) sáu file nằm phẳng cạnh nhau vì đó là thứ người dùng **upload** — bắt
> họ tự ghép từ hai thư mục là bắt họ trả giá cho một quyết định thiết kế không phải của họ.
> `kb_03` và `kb_05` ở đó là **bản EGW**; header mỗi file có ghi rõ.

---

## Kiến trúc IPO

```
INPUT    upload file export  →  nhận diện bằng TẬP CỘT, không bằng tên file
PROCESS  master-instruction  →  routing sang 6 file KB
         3 cổng vào → phễu C1-C2-C3-PL → 6 nhóm nguyên nhân
OUTPUT   báo cáo: việc cần làm trước · kết luận có nhãn · việc tiếp theo ·
         nghẽn ở đâu · khối bàn giao · dòng tự kiểm N/7
```

## Bốn thành phần buổi 4 yêu cầu

KB ✅ · rule tiền kiểm ✅ `kb_04` · **checkpoint hậu kiểm** ✅ `kb_06` *(bộ cũ thiếu, thêm 28/08)* ·
Master ✅.

Cũng sau buổi 4, các luật kiểu **"quy tắc chung của AI"** được viết lại thành **luật nghiệp vụ** — vì
"không bịa số" là thứ mọi bot đều nên có, nó không dạy bot này biết gì về nghề:

| Trước | Sau |
|---|---|
| "Không bịa số" | Số nào không kèm **tên file nguồn** và **cửa sổ thời gian** thì không in |
| "Không hứa kết quả" | Đề xuất nào không kèm **chỉ số đo · cửa sổ đo · mốc trước** thì chưa phải đề xuất |
| "Cẩn thận khi đọc dữ liệu" | Một ASIN nhiều dòng SKU trong Business Report: `Sessions` lặp y nguyên mỗi dòng — **không cộng** |

---

## Trạng thái

**Xong:** hai bộ KB · hai Master Instruction · RTC-COE · Framing v2 · hai vòng test trên dữ liệu thật
3 ASIN (9 lỗi tìm được, đã sửa) · bộ test hồi quy 9 ca · đã cài lên Custom GPT (24/08) · checkpoint hậu
kiểm + khối bàn giao (28/08) · vá 7 lỗ hổng tìm được khi chạy mô phỏng (28/08).

**Còn lại:**
- **Nhờ người khác test, không tự test** — thầy nhấn mạnh: mình tự chạy thì luôn thấy nó ổn.
- Đưa thang chấm listing vào bot thành `kb_07`, master routing ba cửa (ASIN tụt · nghẽn chuyển đổi ·
  ASIN mới trước khi đổ ads).
- Test khả năng đọc trang Amazon công khai để lấp nhóm nguyên nhân về **giá** — một trong hai nhóm
  đang ngoài trần phủ.
