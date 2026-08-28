# kb_03_thresholds.md — Ngưỡng: bạn tự đặt, bot không đặt hộ

**version:** v1.1 · **cập nhật:** 28/08/2026 · **bản:** EGW (học viên tự vận hành)

> Đây là file khác nhiều nhất giữa bản nội bộ và bản này. Bản dùng cho một đội vận hành cụ thể có
> sẵn bảng ngưỡng của họ. Bạn thì **chưa** — và đó là chuyện bình thường, không phải thiếu sót.

---

## 0. Vì sao bot không có sẵn ngưỡng cho bạn

Ngưỡng của người khác **không dùng được cho sản phẩm của bạn**. Ba lý do, không phải giữ bí mật:

1. **Ngưỡng đến từ biên lợi nhuận.** Sản phẩm biên 45% chịu được ROAS thấp hơn hẳn sản phẩm biên 20%.
   Cùng một con số ROAS, một bên đang lời, một bên đang lỗ.
2. **Ngưỡng đến từ mục tiêu đang chạy.** Cùng một ASIN, giai đoạn đẩy rank chấp nhận ROAS thấp hơn
   giai đoạn khai thác lợi nhuận. Đổi mục tiêu là đổi ngưỡng.
3. **Ngưỡng đến từ ngách.** Ngách có giá bấm rẻ và ngách cạnh tranh đắt không thể chung một mốc.

Bot đưa cho bạn một con số nghe có vẻ chuẩn sẽ nguy hiểm hơn là không đưa gì: bạn sẽ cắt nhầm
campaign đang lời, hoặc nuôi campaign đang lỗ.

**Việc của bot:** hỏi bạn ngưỡng, dùng đúng con số bạn đưa, và **nói thẳng khi bạn chưa có**.

---

## 1. Ba con số bot sẽ hỏi

### 1.1. Biên lợi nhuận gộp của sản phẩm

Sau khi trừ giá vốn, phí Amazon và phí vận chuyển — còn lại bao nhiêu phần trăm giá bán?

Từ đây tính ra **ROAS hoà vốn**:

```
ROAS hoà vốn  =  1 ÷ biên lợi nhuận gộp
```

| Biên gộp | ROAS hoà vốn | Nghĩa là |
|---|---|---|
| 20% | 5,0 | Cứ 1 đồng quảng cáo phải ra 5 đồng doanh thu mới huề vốn |
| 30% | 3,3 | |
| 40% | 2,5 | |
| 50% | 2,0 | |

**Đây là điểm huề vốn, không phải mục tiêu.** Chạy đúng ở mức này thì quảng cáo không lỗ nhưng cũng
không đóng góp lợi nhuận. Mục tiêu thật phải cao hơn.

Bot **không tự tính hộ** biên gộp của bạn — nó không thấy giá vốn và phí của bạn.

### 1.2. Mục tiêu đang chạy cho ASIN này

| Mục tiêu | Ngưỡng ROAS đặt ở đâu | Vì sao |
|---|---|---|
| **Đẩy rank** | Dưới điểm hoà vốn cũng chấp nhận được, trong thời hạn định trước | Đang mua vị trí, không mua lợi nhuận. Nhưng phải có hạn — không "đẩy" vô thời hạn |
| **Khai thác lợi nhuận** | Trên điểm hoà vốn, có biên an toàn | Sản phẩm đã có chỗ đứng, giờ phải ra tiền |
| **Giải phóng tồn** | Đặt theo tốc độ bán, không theo lợi nhuận | Mục tiêu là hết hàng trước hạn, lỗ có kiểm soát vẫn hơn ôm tồn |

Chưa xác định được mục tiêu thì **chưa đặt ngưỡng được** — và đó thường là vấn đề thật, không phải
vấn đề dữ liệu.

### 1.3. Ngưỡng cắt

Bao nhiêu lượt bấm không ra đơn thì bạn dừng một campaign?

Con số này phụ thuộc **giá một lượt bấm trong ngách của bạn** và **mức lỗ bạn chấp nhận để học**.
Cách nghĩ:

```
Tiền chấp nhận mất để thử một target  ÷  giá trung bình một lượt bấm  =  số lượt bấm trước khi dừng
```

Ngách bấm rẻ thì con số này lớn hơn; ngách bấm đắt thì nhỏ hơn. **Không có con số chung cho mọi
người.**

### 1.4. Mốc so cho tỉ lệ chuyển đổi — hỏi RIÊNG, không hỏi ở đầu phiên

Ba con số trên hỏi gộp một lần đầu phiên. Con số này **chỉ hỏi khi thật sự cần**: khi đã xuống tới
tầng C3, đã có Business Report, và sắp phải nói tỉ lệ chuyển đổi là cao hay thấp. Hỏi trước là bắt
người dùng trả lời một câu có thể không dùng tới.

```
Tôi tính được tỉ lệ chuyển đổi của listing là <x>%. Để nói con số này cao hay thấp,
tôi cần một mốc để so — bạn có cái nào sau đây không?
  · Tỉ lệ của chính ASIN này ở một kỳ trước (tốt nhất — cùng sản phẩm, cùng khách)
  · Tỉ lệ của một ASIN khác cùng ngách bạn đang bán
  · Mốc ngành hàng bạn đang dùng
Chưa có cũng không sao — tôi sẽ trình bày vị trí thay vì phán cao/thấp.
```

**Không có mốc thì bot KHÔNG được viết "listing kém" hay "chuyển đổi thấp".** Viết đúng là: *"tỉ lệ
chuyển đổi của listing là <x>%; tôi chưa có mốc để nói con số này cao hay thấp."* Và bot **không tự
lấy một mốc ngành hàng nào** — một con số mặt bằng nghe hợp lý sẽ dẫn tới việc sửa listing đang chạy tốt.

**Mốc tự so tốt nhất là chính ASIN đó ở kỳ trước.** Nó khử được khác biệt ngách, khác biệt giá, khác
biệt mùa — những thứ làm mọi mốc ngành hàng trở nên lỏng lẻo.

---

## 2. Chưa có ngưỡng thì bot làm gì

Bot **vẫn chẩn đoán được**, chỉ là không kết luận được vài chuyện:

| Vẫn làm được | Không làm được |
|---|---|
| Chỉ ra tầng nào trong phễu đang nghẽn | Nói campaign nào nên cắt |
| So chuyển đổi từ quảng cáo với chuyển đổi tổng | Nói ROAS hiện tại là tốt hay xấu |
| Đếm campaign đang tiêu tiền mà không ra đơn | Xếp hạng campaign theo mức đạt |
| Chỉ ra từ khoá chưa có thứ hạng | Nói bạn đang lời hay lỗ |

Khi thiếu ngưỡng, bot **trình bày vị trí** thay vì **phán xét**: *"campaign này ROAS 1,8 — bạn cần
đối chiếu với điểm hoà vốn của mình để biết nó đang lời hay lỗ."*

Bot **tuyệt đối không** tự chọn một con số rồi cắt hộ bạn.

---

## 3. Mọi ngưỡng đều phải gắn cửa sổ thời gian

Một ngưỡng không nói rõ đo trong bao lâu là ngưỡng **thiếu một nửa**.

| Loại ngưỡng | Cửa sổ | Đọc trên cửa sổ ngắn hơn thì |
|---|---|---|
| Số lượt bấm không ra đơn (ngưỡng cắt) | dài — 14 đến 30 ngày | Chỉ được nêu nghi vấn, **không kết luận cắt** |
| ROAS / ACOS | 14 đến 30 ngày | Nêu vị trí, không kết luận |
| Chỉ số cấu trúc (số campaign, có tiêu tiền không) | cửa sổ nào cũng được | Dùng bình thường |

**Ví dụ sai rất dễ mắc:** đặt ngưỡng "15 lượt bấm không ra đơn thì cắt" rồi đọc báo cáo 7 ngày. Trong
7 ngày phần lớn campaign chưa đủ 15 lượt bấm, nên không cái nào chạm ngưỡng — và bạn kết luận "không
có gì đáng cắt". Thực ra chỉ là **cửa sổ ngắn hơn ngưỡng**.

---

## 4. Mẫu tối thiểu — khi số liệu quá ít để nói gì

Không phải ngưỡng nghiệp vụ, mà là ngưỡng thống kê. Bot tự áp để khỏi kết luận trên số nhiễu:

| Mức mẫu | Bot làm gì |
|---|---|
| Dưới 50 lượt bấm **hoặc** dưới 5 đơn trong cửa sổ (**chạm đúng 50 hoặc đúng 5 vẫn tính là chưa đủ**) | **Không đọc tỉ lệ.** ROAS và tỉ lệ chuyển đổi lúc này chênh một đơn là đổi hẳn kết luận. Chỉ đọc chỉ số cấu trúc |
| Một campaign dưới 10 lượt bấm | Không tính tỉ lệ chuyển đổi riêng cho campaign đó |

Gặp mức mẫu nhỏ, bot nói thẳng: *"cửa sổ này chỉ có 38 lượt bấm và 2 đơn — quá ít để đọc tỉ lệ. Nới
cửa sổ ra, hoặc chỉ đọc phần cấu trúc."*

---

## 5. Bảng để bạn tự điền

Điền một lần, đưa cho bot mỗi đầu phiên. Cập nhật khi đổi giá vốn, đổi giá bán, hoặc đổi mục tiêu.

| Mục | Giá trị của bạn | Ghi chú |
|---|---|---|
| Biên lợi nhuận gộp | ____ % | Sau giá vốn + phí Amazon + vận chuyển |
| ROAS hoà vốn | ____ | = 1 ÷ biên gộp |
| Mục tiêu đang chạy | đẩy rank / khai thác lợi nhuận / giải phóng tồn | Ghi kèm hạn nếu là đẩy rank |
| ROAS mục tiêu | ____ | Phải cao hơn điểm hoà vốn |
| Ngưỡng cắt | ____ lượt bấm không ra đơn | Kèm cửa sổ đo |
| Cửa sổ mặc định | ____ ngày | Dùng 14–30 ngày cho quyết định cắt |
| Mốc so tỉ lệ chuyển đổi | ____ % | Của chính ASIN này kỳ trước, hoặc ASIN cùng ngách. Chưa có thì để trống |

---

## 6. Ba cái bẫy hay gặp khi tự đặt ngưỡng

**Đặt ngưỡng bằng con số nghe được ở đâu đó.** Một con số ROAS trong bài viết hay video là ngưỡng của
người khác, tính trên biên lợi nhuận của người khác. Lấy về dùng là đang chấm bài sản phẩm của mình
bằng thước của người lạ.

**Đặt một ngưỡng cho mọi campaign.** Campaign nuôi rank và campaign khai thác lợi nhuận có nhiệm vụ
khác nhau, nên kỳ vọng phải khác nhau. Chấm chung một thước sẽ luôn kết luận rằng nhóm nuôi rank
đang kém — trong khi nó đang làm đúng việc của nó.

**Quên cập nhật khi đổi giá.** Đổi giá bán là đổi biên lợi nhuận, tức đổi luôn điểm hoà vốn. Ngưỡng
cũ lập tức sai mà không có gì báo.
