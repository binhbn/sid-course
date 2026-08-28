# Thang chấm điểm listing — bộ công cụ đo từ cây Phần A

**Bản v0.9 · 28/08/2026 · phụ lục của [`thiet-ke/05-decomposition-listing-cvr.md`](thiet-ke/05-decomposition-listing-cvr.md)**

Cây decomposition trả lời câu *"listing gồm những gì"*. File này trả lời câu tiếp theo — **"cái nào
đang yếu, và sửa cái nào trước"** — bằng một thang chấm mà hai người chấm cùng một listing phải ra
gần cùng một điểm.

> **Chưa hiệu chuẩn.** Trọng số dưới đây là **lựa chọn thiết kế**, không phải kết quả đo. Chúng được
> đặt theo bằng chứng từ một đợt rà 4 sản phẩm / 8 đối thủ — mẫu quá nhỏ để nói trọng số nào đúng.
> Mỗi tiêu chí đều ghi rõ căn cứ đến từ **đo được** hay **giả định**. Hiệu chuẩn lại sau 3 đợt rà.

---

## 0. Vì sao ba tầng, không phải một bảng điểm phẳng

Bản nháp đầu của thang này suýt **xoá các tiêu chí đếm số lượng** — đủ 7 ảnh, đủ bullet, có video —
với lý do: chấm 9 listing thì cả 9 đều đạt, vậy tiêu chí không phân biệt được ai thắng.

Lập luận đó **sai vì mẫu lệch**. Cả 9 listing đều của người bán chuyên nghiệp. Một tiêu chí mà cả
nhóm đã qua không chứng minh tiêu chí vô dụng — nó chỉ chứng minh **nhóm này đã qua cổng đó**. Người
mới, hoặc nhân sự chưa quen, trượt ngay ở đó.

Bài học chung, không riêng gì listing: **xây thước đo bằng cách quan sát người giỏi thì sẽ xoá đúng
những tiêu chí mà người kém trượt.** Nên tách hai vai:

| Tầng | Câu hỏi nó trả lời | Cách chấm | Ảnh hưởng điểm |
|---|---|---|---|
| **1 · CỔNG** | Listing này đã đủ tư cách để đem đi so chưa? | Đạt / Trượt | 0 điểm — nhưng **chặn**, trượt thì dừng |
| **2 · PHÂN BIỆT** | Trong nhóm đã đủ chuẩn, cái gì tách người thắng khỏi phần còn lại? | 0 / 1 / 2 điểm | 100 điểm |
| **3 · SO ĐỐI THỦ** | Mình hơn hay kém họ ở đúng tiêu chí đó? | Hơn / Bằng / Kém | quyết **thứ tự làm** |

Tầng 1 không cho điểm là có chủ ý: **cộng điểm cho việc "có đủ 7 ảnh" sẽ đẻ ra listing 7 ảnh vô
nghĩa.** Đủ số lượng là điều kiện cần, không phải thành tích.

---

## 1. TẦNG 1 — Cổng: trượt thì dừng, đừng chấm tiếp

Chấm bằng mắt, trên điện thoại, mất khoảng 2 phút. **Trượt bất kỳ mục nào → không chấm tầng 2.**
Việc cần làm là đưa listing qua cổng đã, chấm điểm lúc này chỉ tạo ra một bảng số vô nghĩa.

| # | Cổng | Đạt khi | Node |
|---|---|---|---|
| G1 | **Đủ ảnh** | ≥ 6 ảnh (nên 9) | 2.6 |
| G2 | **Có video trên trang sản phẩm** | Có ít nhất 1 video ở khối ảnh | 2.6 |
| G3 | **Đủ bullet** | 5 bullet, không bullet nào bỏ trống hoặc lặp | 3.1 |
| G4 | **Có A+ Content** | Có, và không phải bản mặc định của mẫu | 4.1 |
| G5 | **Title đủ dài và đủ ý** | 150–180 ký tự, 5 từ đầu định danh được sản phẩm | 1.2.1 |
| G6 | **Ô mô tả không bỏ trống** | Có nội dung, dưới 2.000 ký tự | 3.4 |
| G7 | **Không còn nội dung mùa vụ hết hạn** | Không còn chữ của mùa/năm đã qua trên ảnh hoặc copy | 5.2 |
| G8 | **Đúng danh mục** | Danh mục khớp sản phẩm | 5.3 |
| G9 | **Tuân thủ** | Không dùng tên thương hiệu đối thủ; ảnh người tạo bằng AI có gắn nhãn | 5.4 |

**Ghi chú về G2 (video).** Video là **cổng, không phải đòn bẩy**. Bằng chứng: trong đợt rà đầu tiên,
sản phẩm bán chạy nhất một ngách (ước tính hơn 1.200 đơn/tháng) **không có video** trong khi listing
của mình có. Đặt video vào tầng 2 sẽ khiến người vận hành đi quay video trong lúc vấn đề thật nằm ở
chỗ khác. Nhưng bỏ hẳn nó ra khỏi thang thì người mới sẽ để trống ô đó — nên nó nằm ở cổng.

---

## 2. TẦNG 2 — Phân biệt: 100 điểm, 5 nhóm

Mỗi tiêu chí chấm **0 · 1 · 2**:

- **2 — Đạt:** làm đúng, không cần sửa trong vòng này.
- **1 — Yếu:** có làm nhưng chưa tới; sửa được bằng một việc nhỏ.
- **0 — Trượt:** chưa làm, hoặc làm sai hướng. **Bắt buộc ghi một câu vì sao**, không được để trống.

Điểm nhóm = (tổng điểm đạt ÷ tổng điểm tối đa của nhóm) × trọng số nhóm.

### Nhóm 1 · Được nhìn thấy và được chọn ở trang kết quả — **30 điểm**

Đây là nhóm nặng nhất vì nó quyết định khách có **bấm vào** hay không; mọi thứ bên trong trang chỉ
có giá trị sau khi khách đã bấm.

| # | Tiêu chí | Đạt (2) khi | Cách đo | Node | Căn cứ |
|---|---|---|---|---|---|
| A1 | **Tỷ lệ ảnh chính** | Khung đứng 5:6 (1000×1200) | Xem thuộc tính ảnh gốc | 1.1.1 | đo được — ảnh dọc chiếm nhiều diện tích hơn trên điện thoại |
| A2 | **Nổi bật giữa trang kết quả** | Đặt ảnh chính cạnh 10 ảnh đối thủ trên cùng một màn: vẫn phân biệt được ngay | Chụp màn hình trang kết quả, nhìn 3 giây | 1.1.3 | đo được — ba ngách đã rà đều có người thắng nhờ tương phản màu/bố cục |
| A3 | **Số biến thể** | Có ≥ 5 mẫu, hoặc bằng đối thủ dẫn đầu | Đếm trên trang | 5.1 | **đo được, mạnh nhất** — người dẫn đầu ở cả 4 ngách đã rà đều có 7–27 mẫu; mình có 2–4 |
| A4 | **Từ khoá chính trong 60 ký tự đầu title** | Cụm khách thật sự gõ nằm trong phần hiện trên điện thoại | Cắt title ở ký tự 60, đọc lại | 1.2.2 | giả định — chưa tách được ảnh hưởng riêng |
| A5 | **Bằng chứng xã hội** | Điểm sao và số review không thấp hơn hẳn hai đối thủ | So trực tiếp | 1.4 | đo được — nhưng là **kết quả**, không phải việc sửa được ngay |

> **A3 được nâng lên nhóm 1** so với cây gốc (ở đó nó nằm ở nhánh 5 "cấu trúc, gián tiếp"). Lý do:
> khách nhìn thấy dãy biến thể **ngay màn hình đầu**, và đây là tiêu chí phân biệt rõ nhất trong cả
> đợt rà. Xếp nó vào "gián tiếp" là xếp sai vùng trang.

### Nhóm 2 · Đọc được trên điện thoại — **20 điểm**

| # | Tiêu chí | Đạt (2) khi | Cách đo | Node | Căn cứ |
|---|---|---|---|---|---|
| B1 | **Chiều cao chữ** | Chữ thường ≥5% chiều cao ảnh · call-out 7% · số nổi bật 8,5% · đoạn A+ 4% | Đè lưới ô 5% lên ảnh | 2.2.1–2.2.2 | đo được |
| B2 | **Không cắt chữ ở mép** | Toàn bộ chữ nằm trong khung an toàn, không từ nào bị cắt | Xem ảnh ở bề ngang 200px | 2.2 | **đo được** — một ca thật: ảnh cắt chữ, và review khách chê đúng chỗ đó |
| B3 | **Phép thử 3 giây** | Xem 3 giây rồi che: nhớ được sản phẩm là gì và cho ai | Nhờ người chưa biết sản phẩm thử | 2.2.3 | giả định — phụ thuộc người thử |
| B4 | **Mỗi ảnh một câu hỏi** | Không ảnh nào nhồi 2–3 thông điệp | Liệt kê câu hỏi từng ảnh trả lời | 2.1 | giả định |

### Nhóm 3 · Đúng ý định khách — **20 điểm**

| # | Tiêu chí | Đạt (2) khi | Cách đo | Node | Căn cứ |
|---|---|---|---|---|---|
| C1 | **Phủ đủ tầng từ khoá của đúng sản phẩm** | Có thứ hạng ở phần lớn cụm sát ý định, không chỉ cụm lõi | So từ khoá với đối thủ, **lọc theo tên và thuộc tính sản phẩm trước khi đọc** | 3.5 | **đo được, mạnh** — hai ca thật: có thứ hạng ở 4/894 và 141/2.163 cụm |
| C2 | **Không phủ nhầm sang sản phẩm lân cận** | Listing không nhồi từ khoá của sản phẩm nó không phải; và cụm nào đã nhồi thì **có thứ hạng thật** | Đọc từng cụm trong title/backend, hỏi "cụm này người ta đang tìm sản phẩm nào?" — rồi kiểm cụm đó có rank chưa | 3.5 | **đo được** — hai ca thật: một listing có đủ chữ *nana/gigi/mimi* trong title mà **không rank cụm nào**; một danh sách khoảng trống của sản phẩm cho *bà* có cụm đứng đầu là cụm cho *ông* |
| C3 | **Title theo công thức** | 5 từ đầu định danh → cho ai → khác gì | Đọc title | 1.2.1 | giả định |
| C4 | **Trả lời câu khách hỏi trước khi mua** | Listing có **câu trả lời** cho 5 câu hỏi hay gặp nhất của ngách | Hỏi trợ lý mua hàng trên trang đối thủ, ghi lại câu hỏi | 3.3 | giả định |

> **C2 là tiêu chí sinh ra từ một lỗi thật.** Nếu xếp cơ hội từ khoá theo lượt tìm kiếm, cụm đứng đầu
> thường là cụm chung chung hoặc cụm của sản phẩm lân cận. Lượng tìm kiếm **không phải** thứ tự cơ hội.

### Nhóm 4 · Nói bằng ngôn ngữ khách — **15 điểm**

| # | Tiêu chí | Đạt (2) khi | Cách đo | Node | Căn cứ |
|---|---|---|---|---|---|
| D1 | **Dùng nguyên văn cụm từ khách viết trong review** | Copy có ít nhất 3 cụm rút từ review | Đọc review của mình và của đối thủ | 3.2 | đo được |
| D2 | **Nói đúng thứ đối thủ bị chê** | Listing nêu rõ điểm mà review đối thủ phàn nàn | Đọc review 1–3 sao của đối thủ | 3.2 | **đo được** — hai ngách đã rà đều lộ ra một điểm chê chung |
| D3 | **Bullet có móc ở vài chữ đầu** | Vài chữ đầu mỗi bullet đọc được như một lời hứa | Đọc riêng phần đầu 5 bullet | 3.1 | giả định |
| D4 | **Sản phẩm đang được dùng, đúng chân dung khách** | Có ảnh người dùng thật, đúng nhóm khách | Xem bộ ảnh | 2.3 | giả định |

> **Mẫu review quá ít thì để trống, không chấm.** Trong đợt rà đầu, có sản phẩm chỉ 1–2 lượt nhắc
> chủ đề — không đủ để kết luận gì. Ghi "chưa đủ mẫu", đừng cho 0 điểm: 0 điểm nghĩa là *làm sai*,
> khác hẳn *chưa biết*.

### Nhóm 5 · Thuyết phục và thương hiệu — **15 điểm**

| # | Tiêu chí | Đạt (2) khi | Cách đo | Node | Căn cứ |
|---|---|---|---|---|---|
| E1 | **A+ theo mạch Hook → Educate → Persuade** | Ba phần rõ, không phải ba banner rời | Đọc A+ | 4.1 | giả định |
| E2 | **Bảng so sánh nội bộ** | Có bảng so với sản phẩm khác của chính mình | Xem A+ | 4.3 | giả định |
| E3 | **Câu chuyện thương hiệu** | Có, ở cuối A+ | Xem A+ | 4.4 | giả định |
| E4 | **Bằng chứng uy tín** | Chứng nhận / giải thưởng / bao bì quà nếu là ngách quà tặng | Xem bộ ảnh | 2.4 | **đo được** — trong ngách quà, cả hai đối thủ đều bày yếu tố quà ngay ảnh chính |

> **Nhóm 5 để trọng số thấp vì đợt rà chưa tách được ảnh hưởng của nó.** Không có nghĩa A+ không quan
> trọng — có nghĩa mình **chưa đo được**. Khi có dữ liệu, nâng lại.

---

## 3. TẦNG 3 — So đối thủ: quyết thứ tự làm

Chấm lại đúng các tiêu chí tầng 2, nhưng lần này đặt cạnh **hai đối thủ**: **Hơn · Bằng · Kém**.

Ghép hai kết quả lại thành ma trận quyết thứ tự:

| | **Hơn đối thủ** | **Bằng** | **Kém đối thủ** |
|---|---|---|---|
| **Trượt best practice** | hiếm — kiểm lại cách chấm | sửa | **① LÀM TRƯỚC** — mình dở, họ hơn |
| **Yếu** | giữ | sửa khi rảnh | **② làm tiếp** |
| **Đạt** | ④ giữ nguyên, đừng đụng | giữ | **③ CƠ HỘI** — cả ngách đều đạt, ai vượt trước thì tách khỏi đám |

Ô ③ là ô hay bị bỏ sót: **mình đã đạt chuẩn mà vẫn kém đối thủ** nghĩa là chuẩn đó không còn là lợi
thế. Ngược lại, một tiêu chí mà **cả ngách cùng trượt** là cửa rẻ nhất để vượt lên — ai làm trước thì
một mình một kiểu trên trang kết quả.

---

## 4. Cách dùng — 6 bước, một listing khoảng 30 phút

1. **Mở trên điện thoại**, vai khách hàng, không đăng nhập tài khoản bán hàng.
2. **Chạy 9 cổng.** Trượt cái nào → ghi việc, dừng, không chấm tiếp.
3. **Chấm tầng 2**, mỗi tiêu chí 0/1/2. Ô 0 bắt buộc kèm một câu vì sao.
4. **Mở 2 đối thủ**, chấm tầng 3 đúng các tiêu chí đó.
5. **Xếp việc theo ma trận** ở mục 3. Lấy tối đa **3 việc** — nhiều hơn thì không ai làm.
6. **Ghi mốc trước khi sửa** vào sổ theo dõi: chỉ số đo, cửa sổ đo, số hiện tại. Không có mốc trước
   thì hai tuần nữa không đọc được kết quả.

**Mỗi lần một biến.** Đổi ảnh chính và đổi title cùng lúc thì có nhích cũng không biết nhờ cái nào.

---

## 5. Ba chỗ thang này cố tình KHÔNG làm

- **Không quy ra tiền.** Thang không nói "sửa cái này được thêm $X" — không có cơ sở nào cho con số
  đó, mà một con số bịa ra sẽ đi thẳng vào kế hoạch.
- **Không chấm người.** Điểm là điểm của **listing**, không phải của người phụ trách listing. Nếu
  điểm này vào KPI thì người ta sẽ chấm cho đẹp và cả thang mất tác dụng — mất mà không ai biết.
- **Không tự sửa.** Thang chỉ ra chỗ yếu và thứ tự; viết copy và làm ảnh vẫn là việc của người.

---

## 6. Còn nợ

| Nợ gì | Vì sao chưa làm được |
|---|---|
| Hiệu chuẩn trọng số | Cần ≥3 đợt rà có đo trước/sau mới biết nhóm nào thật sự kéo được tỉ lệ chuyển đổi |
| Mốc so cho tỉ lệ chuyển đổi | Không đọc được tỉ lệ của đối thủ. Mốc dùng tạm: chính sản phẩm đó ở kỳ trước |
| Kiểm hai người chấm có ra cùng điểm không | Chưa thử. Tiêu chí nhóm 2 và 4 nhiều phần chủ quan, nghi là lệch nhiều nhất |
| Trọng số nhóm 5 | Đang đặt thấp vì **chưa đo được**, không phải vì đã đo thấy nó ít quan trọng |
