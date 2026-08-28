# SID Course — Bùi Ngọc Bình

Bài làm khoá **SID (Structured Intelligence Design)**. Cả khoá em làm **một project xuyên suốt** thay vì
mỗi buổi một bài rời:

> **Chẩn đoán performance ASIN trên Amazon** — gồm phân tích quảng cáo và tỉ lệ chuyển đổi của listing.

---

## Vào đâu trước

| Bạn là | Vào đây |
|---|---|
| **Thầy / bạn học** — chấm bài, xem em học được gì | [`bai-lam/`](bai-lam/) — bảng buổi nào nộp gì, ở file nào |
| **Người muốn dùng thật** — cài về chạy | [`chan-doan-asin/`](chan-doan-asin/) — README có hướng dẫn cài 5 phút |
| **Muốn xem nó chạy ra gì** | [Xem online](https://claude.ai/code/artifact/83bf2ea9-3f67-4514-85ea-f8b80c4319d0) — bản gộp 5 trang, mở là chạy. Hoặc tải repo về mở [`chan-doan-asin/demo/`](chan-doan-asin/demo/) |

---

## Vì sao gộp cả khoá vào một project

Bốn buổi đầu là bốn giai đoạn của **cùng một thứ**, không phải bốn bài tập:

```
Buổi 1  Framing        →  bài toán là gì, cho ai, ranh giới ở đâu
Buổi 2  Prompt Stack   →  RTC-COE, prompt dựng nên bộ tri thức
Buổi 3  Chatbot        →  Master Instruction + 6 file KB, chạy được
Buổi 4  Decomposition  →  bóc nốt cụt "nghẽn chuyển đổi" → thang chấm listing
```

Tách thành `bai-01/`, `bai-02/`, `bai-03/`, `bai-04/` thì nó trông như bốn bài tập rời và **không ai tải
về dùng được**. Gộp lại thì nó là một sản phẩm có README, có file cấu hình, có bộ tri thức, có demo —
và tài liệu thiết kế của bốn buổi nằm trong [`chan-doan-asin/thiet-ke/`](chan-doan-asin/thiet-ke/) theo
đúng thứ tự sinh ra.

---

## Repo này công khai

Nên mọi thứ chạm dữ liệu thật đều **không nằm ở đây**: mã ASIN thật, số liệu bán hàng, ngưỡng vận hành
nội bộ, bản KB dùng cho đội nội bộ, file export gốc. Chặn bằng `.gitignore` (`*KHONG-NOP*`, `data/`).

Bản demo có số thật nhưng đã đổi mã sản phẩm và bỏ mọi số tuyệt đối — chi tiết ở
[`chan-doan-asin/README.md`](chan-doan-asin/README.md).
