# RTC-COE của project — prompt dựng chatbot chẩn đoán ASIN

**Học viên:** Bùi Ngọc Bình
**Project xuyên khóa:** Vận hành hiệu suất ASIN — nhánh chẩn đoán.
**Kế thừa:** Framing Brief (Buổi 1) → Prompt Stack V1 (Buổi 2) → chatbot (Buổi 3).

---

## Một lưu ý về vai trò của prompt này

Prompt dưới đây là prompt **build-time**: dùng để dựng ra Master Instruction và bộ KB, không phải
prompt nhân sự gõ hằng ngày. Bản chạy hằng ngày chính là con chatbot.

Em ghi rõ vì ở Buổi 2 em đã phân biệt hai thứ này, và ranh giới đó vẫn đúng ở đây.

---

## Prompt

```text
[Role]
Đóng vai người thiết kế chatbot nghiệp vụ, quen với việc biến một quy trình chẩn đoán
đã có sẵn thành phần mềm hội thoại cho nhiều người dùng khác nhau sử dụng.

[Task]
Dựng Master Instruction và bộ file knowledge base cho một chatbot chẩn đoán hiệu suất
ASIN trên Amazon.

[Context]
- Người dùng cuối: nhân sự MKT vận hành, 1-2 năm kinh nghiệm, mỗi người theo 40-60 ASIN.
  Họ đã được đào tạo knowhow nội bộ nhưng chưa áp dụng được vào ca cụ thể.
- Bài toán: khi một ASIN tụt doanh số hoặc tụt ROAS, họ không biết soi từ đâu.
- Đã có sẵn từ các buổi trước: cây chẩn đoán 6 nhánh, thứ tự soi có node tiên quyết,
  và một kho knowhow nội bộ 126 entry đã gắn nhãn độ tin cậy.
- Nền tảng triển khai: Custom GPT. Ô instruction có giới hạn ký tự, nên Master
  Instruction phải gọn, mọi chi tiết đẩy xuống file KB.
- Người dùng nộp dữ liệu bằng cách upload file export thô từ Amazon Ads, Seller Central
  và công cụ ranking từ khoá. Không phải lúc nào cũng nộp đủ.

[Constraints]
- Master Instruction chỉ giữ ba việc: routing sang file KB, luật tối cao, và mẫu output.
  Mọi chi tiết nghiệp vụ nằm ở file KB.
- Mỗi file KB một nhiệm vụ, đặt tiền tố kb_ và đuôi .md, không file nào quá dài.
- Bot không được bịa số. Thiếu dữ liệu thì nói thiếu và nêu cần file gì.
- Bot không được tự đặt ngưỡng. Ngưỡng đến từ nguồn có thứ tự ưu tiên rõ ràng, và
  ngưỡng nào phụ thuộc ngách hoặc chiến lược thì phải hỏi người dùng.
- Bot phải chạy được với dữ liệu thiếu: có nguồn nào thì kết luận trong phạm vi nguồn
  đó, và nói rõ nhánh nào chưa loại trừ được.
- Bot không kết luận thay người dùng những việc thuộc thẩm quyền của họ.
- Mỗi conversation starter phải có một nhánh xử lý tương ứng trong Master Instruction.

[Output]
1. Master Instruction, viết vừa giới hạn ký tự của nền tảng.
2. Bộ file KB, mỗi file nêu rõ nhiệm vụ và quan hệ với các file khác.
3. Mẫu output cho báo cáo chẩn đoán.
4. Danh sách conversation starter kèm nhánh xử lý của từng cái.

[Evaluation]
Tự kiểm trước khi giao:
- có chi tiết nghiệp vụ nào còn nằm trong Master Instruction đáng lẽ phải ở file KB không;
- có chỗ nào bot được phép kết luận khi thiếu dữ liệu không;
- có ngưỡng nào bị viết cứng trong KB mà thực chất phụ thuộc ngách hoặc chiến lược không;
- một người dùng chỉ nộp một file thì bot có chạy được không, và có nói rõ giới hạn không;
- mỗi conversation starter có nhánh xử lý riêng chưa.
```

---

## Prompt này sinh được gì, và không sinh được gì

Đây là phần em thấy đáng ghi lại nhất khi làm bài.

**Sinh được:** khung Master Instruction, danh sách file KB và nhiệm vụ từng file, mẫu output, bộ
conversation starter, và phần lớn luật trình bày.

**Không sinh được — phải đi lấy từ thực tế:**

| Thứ | Vì sao prompt không sinh được |
|---|---|
| Schema bốn nguồn dữ liệu | Phải mở file thật mới biết tên cột, đơn vị, và các bẫy như `CTR` là thập phân hay phần trăm |
| Luật đọc `Portfolio` thay vì tên campaign để xác định ASIN | Chỉ lộ ra khi đếm trên dữ liệu thật: 100% so với 96% |
| Bẫy Business Report lặp Sessions theo SKU | Phải thấy hai dòng cùng `Sessions = 694` mới phát hiện |
| Toàn bộ bảng ngưỡng | Nằm trong kho nội bộ, và một phần đã lỗi thời — phải đối chiếu mới biết |
| Quyết định "số target không còn là tiêu chí bệnh" | Là quyết định vận hành của người có thẩm quyền, không suy ra được từ tài liệu |

Nói cách khác: prompt dựng được **cái vỏ**, còn **business logic thì phải do người có nghiệp vụ cung
cấp**. Đúng câu chốt của buổi học — AI là công cụ tăng tốc, không phải chuyên gia.

Cụ thể hơn: nếu em chỉ chạy prompt này rồi giao cho nhân sự, con bot sẽ đọc sai cột, cộng nhầm
Sessions, dùng ngưỡng đã bị thay thế, và báo lỗi cấu trúc target ở chỗ không còn là lỗi. Bốn cái sai
đó đều **im lặng** — bot vẫn trả về báo cáo trông rất gọn gàng.
