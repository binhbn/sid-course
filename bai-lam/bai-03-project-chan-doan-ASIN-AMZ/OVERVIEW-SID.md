# Overview project theo khung SID — soi lại toàn tuyến

**Mục đích:** đặt cả ba buổi cạnh nhau để thấy project đã đi đúng khung SID chưa, và **chỗ nào đã
lệch khỏi cái mình tự chốt** mà chưa được ghi nhận lại.

Đọc file này trước khi làm buổi 4 (Information Architecture).

> **Lưu ý về tên file:** tài liệu này viết trên bộ KB **v1 (8 file)**. Sau khi soi xong, bộ KB được
> gộp lại còn **v2 (5 file)** và tách làm hai bản. Bảng quy đổi:
> `kb_data_schema` → `kb_01_data` · `kb_causes` + `kb_tree` + `kb_coverage` → `kb_02_logic` ·
> `kb_thresholds` → `kb_03_thresholds` · `kb_rules` → `kb_04_rules` · `template_output` → `kb_05_output`.
> Tên cũ được giữ nguyên trong phần phân tích để không viết lại lịch sử.

---

## 1. Chuỗi artifact xuyên khoá

```
Buổi 1  FRAMING BRIEF        → chốt: bài toán · audience · scope · task type · artifact
   ↓
Buổi 2  PROMPT STACK V1      → stack 6 bước để XÂY RA cây chẩn đoán (build-time)
   ↓                            + Test Log 5 lỗi
Buổi 3  CHATBOT (IPO)        → Master Instruction + 7 file KB (run-time)
   ↓                            + Test Log 9 lỗi trên dữ liệu thật
Buổi 4  INFORMATION ARCH     ← chưa làm
```

Mỗi buổi đều lấy output buổi trước làm input. Đây là điểm mạnh nhất của project: không buổi nào làm
lại từ đầu.

## 2. Sáu thành phần RTC-COE — hiện nằm ở đâu

Ở buổi 3, RTC-COE **không còn là một prompt** nữa mà bị phân rã ra thành kiến trúc phần mềm. Đây là
chỗ đáng ghi nhận nhất về mặt phương pháp:

| Thành phần RTC-COE | Ở buổi 2 nằm đâu | Ở buổi 3 nằm đâu |
|---|---|---|
| **Role** | đầu mỗi prompt trong stack | 2 dòng đầu Master Instruction |
| **Task** | mỗi step một task | quy trình 4 bước bắt buộc trong Master Instruction |
| **Context** | dán vào từng prompt | `kb_01_data.md` + `kb_02_logic.md` — bot tự đọc, không dán |
| **Constraints** | liệt kê trong từng prompt | `kb_04_rules.md` (guardrail) + `kb_03_thresholds.md` (ngưỡng) |
| **Output** | mô tả trong từng prompt | `kb_05_output.md` |
| **Evaluation** | mục tự kiểm cuối prompt | `kb_02_logic.md` Phần I Cổng 3 (mức phủ) + hệ ba nhãn kết luận |

**Điều rút ra:** chuyển từ prompt sang chatbot không phải là "viết prompt dài hơn", mà là **tách sáu
thành phần ra thành sáu file có vòng đời riêng**. Ngưỡng đổi thì sửa `kb_03_thresholds`, không đụng
Master Instruction. Đây đúng ý "component hoá" mà buổi 3 nói tới.

## 3. Kiến trúc IPO

| Tầng | Nội dung | File |
|---|---|---|
| **Input** | Upload 4 loại file export. Nhận diện bằng **tập cột**, không bằng tên file | `kb_01_data.md` |
| **Process** | Master Instruction routing → 5 file KB. Phễu C1→C2→C3→PL | `kb_02_logic.md` |
| **Output** | Báo cáo chẩn đoán có nhãn tin cậy | `kb_05_output.md` |

## 4. ⚠️ Bốn chỗ đã lệch khỏi Framing Brief buổi 1

Đây là phần chính của file này. Framing Brief buổi 1 **chưa được cập nhật** sau hai vòng test, nên
hiện tại nó mô tả một sản phẩm khác với sản phẩm đang có.

| # | Buổi 1 chốt | Thực tế buổi 3 | Đánh giá |
|---|---|---|---|
| 1 | **Ngoài phạm vi: chưa đề xuất phương án sửa** — đó là phase 2 (DESIGN) | Bot **có** mục "Hướng xử lý" | **Đã mở rộng có chủ đích.** Anh Bình yêu cầu ngay từ đầu buổi 3: bot phải "đưa ra vấn đề và đề xuất hướng xử lý". Cần ghi nhận chính thức, không để framing nói một đằng sản phẩm một nẻo |
| 2 | **Tối đa 5 vấn đề trọng tâm** | **6 nhánh** | Đã nới ở buổi 2 (Lỗi 2) vì nhóm nguyên nhân tự nhiên tách thành 6. Ràng buộc cũ bị chính dữ liệu phản bác |
| 3 | **Chạy hết cây ra MỘT kết luận nguyên nhân** | Nguyên nhân gốc **+ phụ thuộc + quan hệ** | Đã sửa ở buổi 2 (Lỗi 5): ba ca thật đều có hai nguyên nhân cùng tồn tại. Giữ tinh thần chống "danh sách khả năng", bỏ ràng buộc "đúng một" |
| 4 | Task type: **`DIAGNOSE`** | `DIAGNOSE` + một phần `DESIGN` + một mục cảnh báo mùa vụ | Cảnh báo mùa vụ **không thuộc DIAGNOSE** — nó trả lời "có kịp chuẩn bị không", không phải "vì sao tụt". Đã tách ra thành mục riêng ngoài cây, nhưng framing chưa có chỗ cho nó |

**Đề xuất:** viết **Framing Brief v2** cho buổi 4, ghi nhận cả bốn thay đổi trên. Không sửa đè bản
buổi 1 — giữ bản gốc để thấy được quá trình.

## 5. Phân rã task — còn đúng không?

Stack 6 bước ở buổi 2 là **build-time** (dùng để xây cây), chatbot là **run-time**. Hai thứ khác
nhau, không mâu thuẫn.

| Step buổi 2 | Sinh ra artifact | Nay nằm ở |
|---|---|---|
| 1. Rút nguyên nhân | Cause Inventory | (trung gian) |
| 2. Gom còn ≤5 nhánh | Cause Shortlist | `kb_causes.md` |
| 3. Gán chỉ số | Signal Map | `kb_causes.md` (mục "đo bằng") |
| 4. Đặt ngưỡng | Threshold Table | `kb_thresholds.md` |
| 5. Sắp thứ tự + dựng cây | Decision Tree | `kb_tree.md` |
| 6. Test và sửa | Reviewed Tree | Test Log buổi 3 |

**Nhận xét:** phân rã vẫn đúng, **nhưng thiếu một bước**. Giữa Step 3 và Step 4 lẽ ra phải có một
bước *"kiểm chỉ số này có đo được từ dữ liệu thật không"*. Thiếu bước đó nên mới sinh ra lỗi ngưỡng
"4 campaign Auto tách + 1 gộp" — một ngưỡng **không đo được** từ file có trong tay.

→ Nếu làm lại stack, thêm Step 3.5: **Feasibility check** — mỗi chỉ số phải chỉ ra được lấy từ cột
nào của file nào.

## 6. Hai vòng test — cây tiến hoá thế nào

| | Buổi 2 | Buổi 3 |
|---|---|---|
| Kiểm được | logic cây | logic + **ngưỡng + dữ liệu thật** |
| Lỗi tìm được | 5 | **9** |
| Lỗi nặng nhất | thứ tự node sai — dừng sớm ở node **dễ soi** | node tiên quyết **chặn tuyệt đối** — dừng sớm ở đầu kia |
| Sửa chính | thêm node tiên quyết | node tiên quyết chuyển từ **chặn** sang **cảnh báo** |

**Điểm đáng suy nghĩ nhất của cả project:** cái sửa ở buổi 2 chính là cái sinh ra lỗi nặng nhất ở
buổi 3. Không phải vì sửa sai, mà vì **một node có quyền tuyệt đối luôn tạo ra điểm mù** — dù đặt ở
đầu nào của cây.

Và cả hai lần, lỗi đều **không phải thiếu dữ liệu**. Dữ liệu nằm sẵn trong nguồn. Lỗi là **cây không
hỏi tới**.

## 7. Chỗ mạnh nhất và yếu nhất hiện tại

**Mạnh:**
- Guardrail chống bịa số đứng vững qua hai vòng test thật.
- Hệ ba nhãn tin cậy (📕 / ✅ / 🧪) chặn được entry chưa kiểm chứng lọt vào kết luận.
- Cơ chế mức phủ cho phép chạy với dữ liệu thiếu mà vẫn trung thực.
- Phát hiện được kho tri thức nội bộ có entry lỗi thời — và đã sửa kho.

**Yếu:**
- Chưa có `kb_styleguide.md` — bộ KB thiếu đúng file mà demo Profile Curator của thầy nhấn mạnh.
- Chưa chạy thật trên Custom GPT, mới chạy mô phỏng.
- Chưa có nhân sự nào test — vẫn là tự mình đánh giá mình.
- Hai nhánh (giá, tồn kho) không có nguồn dữ liệu, trần phủ chỉ 4/6.
- Ngưỡng cắt cầm máu **không ghi cửa sổ**, nên chạy trên cửa sổ 7 ngày gần như vô hiệu.

## 8. Đường đi nội bộ → EGW: file nào swap được, file nào không

**Định hướng (16/08/2026):** pilot ở đội vận hành nội bộ trước, sau đó áp dụng cho EGW — dạy cái
mình làm. Khớp với nguyên tắc đã chốt từ buổi 2: **kiểm chứng ở nội bộ, đóng gói cho EGW.**

Điều này **đổi kiến trúc, không chỉ đổi brand**. Theo bài học buổi 1: đổi audience → đổi task type →
đổi artifact.

| | Nội bộ (pilot) | EGW (phái sinh) |
|---|---|---|
| Audience | Nhân sự MKT vận hành, 40–60 ASIN | Học viên — seller tự vận hành |
| Task type | `DIAGNOSE` | `EXPLAIN` (theo Framing buổi 1, Audience A) |
| Ngưỡng | Ngưỡng của tổ chức | Học viên **phải tự đặt ngưỡng của họ** |
| Giọng | Ngắn, giả định đã biết nghề | Giải thích vì sao, ít thuật ngữ |

**Phân loại file theo khả năng dùng lại:**

| File | Dùng chung | Ghi chú |
|---|---|---|
| `kb_data_schema.md` | ~90% | Schema Amazon giống nhau. **Nhưng** phần ký hiệu nội bộ trong tên campaign là quy ước riêng của tổ chức → phải tách ra thành phụ lục |
| `kb_coverage.md` | 100% | Logic mức phủ không phụ thuộc audience |
| `kb_tree.md` | 100% | Thứ tự nhân quả là kiến thức ngành |
| `kb_causes.md` | ~80% | Cơ chế dùng chung; các căn cứ trích từ kho knowhow nội bộ phải trung tính hoá |
| `kb_rules.md` | ~85% | Luật chống bịa số dùng chung; mục bảo mật nội bộ thì khác |
| `kb_thresholds.md` | **0%** | **Swap hoàn toàn.** Bản EGW là *hướng dẫn tự đặt ngưỡng*, không phải bảng số |
| `template_output.md` | **0%** | `DIAGNOSE` và `EXPLAIN` ra hai artifact khác nhau |
| `kb_styleguide.md` | **0%** | Chưa có — xem mục 9 |
| `master-instruction.md` | ~70% | Phần Role và audience phải viết lại |

**Hệ quả thiết kế — làm ngay từ bây giờ, không đợi:**

1. **Mọi thứ phụ thuộc audience phải nằm ở file riêng**, không nhúng vào Master Instruction. Hiện
   Master Instruction đang mô tả audience ngay dòng đầu — chấp nhận được vì nó ngắn, nhưng giọng văn
   và mức chi tiết thì **phải tách ra `kb_styleguide.md`**.
2. **Không nhúng số nội bộ vào file dùng chung.** Việc này đã làm đúng: `kb_causes.md` không có số
   nào, toàn bộ số nằm ở `kb_thresholds.md`. Nhờ vậy 80% `kb_causes` dùng lại được.
3. **Artifact báo cáo phải tách tokens brand** (màu, font, logo) khỏi cấu trúc, để đổi từ bản nội bộ sang
   EGW mà không dựng lại layout.

Đây chính là bài học từ demo Profile Curator: **style guide tách file riêng để customize theo từng
khách** — banking thì trang trọng, agency thì phóng khoáng. Ở đây bản nội bộ thì ngắn gọn, EGW thì
giải thích.

---

## 9. Việc cho buổi 4 (Information Architecture)

1. **Framing Brief v2** — ghi nhận 4 thay đổi ở mục 4.
2. **Thêm `kb_styleguide.md`** — tách giọng văn và định dạng ra khỏi `template_output.md`.
3. **Xem lại kiến trúc thông tin của báo cáo** — hiện phần "đường đi của cây" chiếm nhiều chỗ nhất
   nhưng là phần ít người đọc nhất.
4. **Gắn cửa sổ cho mọi ngưỡng** trong `kb_thresholds.md`.
