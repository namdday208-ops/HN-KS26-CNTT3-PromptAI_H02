# BÀI LÀM PHA 2 – HACKATHON 02

- **Mã đề:** 08
- **Họ tên:**Hoàng Xuân Nam
- **Lớp:**CNTT3
- **Link repository Pha2:**
- **Công cụ AI đã dùng (ChatGPT/Claude/Gemini):**GEMINI

> Pha 2 (20 phút cuối). Mỗi mục: mở cuộc trò chuyện mới, gửi nguyên văn prompt đã viết trong bai_lam_pha1.md, dán câu trả lời và link chia sẻ vào đây. Xong, kéo thả file này vào repository Pha2.

## 4.2. Trích xuất tin nhắn đặt lịch khám ra JSON

**KET QUA AI**

```text
[
  {
    "ma_dich_vu": "TRAMRANG",
    "ngay_hen": "2026-12-17",
    "nguoi_lon": 2,
    "tre_em": 1,
    "khung_gio": "chieu",
    "ghi_chu": null
  },
  {
    "ma_dich_vu": "CAOVOI",
    "ngay_hen": "2027-01-06",
    "nguoi_lon": 3,
    "tre_em": 0,
    "khung_gio": null,
    "ghi_chu": "1 người bị tiểu đường"
  },
  {
    "ma_dich_vu": "TAYTRANG",
    "ngay_hen": null,
    "nguoi_lon": 4,
    "tre_em": 0,
    "khung_gio": "sang",
    "ghi_chu": "1 người dị ứng thuốc tê"
  }
]
```

**Link trò chuyện:**https://share.gemini.google/se1bWgLv7d43

## 4.3. Báo giá bằng Chain-of-Thought

**KET QUA AI**

```text
{
  "suy_luan_tung_buoc": {
    "tien_nguoi_lon": {
      "phep_tinh": "6 x 350.000 = 2.100.000",
      "so_tien": 2100000
    },
    "tien_tre_em": {
      "phep_tinh": "2 x (350.000 x 80%) + 1 x 0 = 560.000",
      "so_tien": 560000
    },
    "tien_dich_vu": {
      "phep_tinh": "2.100.000 + 560.000 = 2.660.000",
      "so_tien": 2660000
    },
    "giam_gia_nhom": {
      "phep_tinh": "2.660.000 x 10% = 266.000 (con lai: 2.660.000 - 266.000 = 2.394.000)",
      "so_tien_giam": 266000,
      "tien_dich_vu_sau_giam": 2394000
    },
    "phi_x_quang": {
      "phep_tinh": "3 x 150.000 = 450.000",
      "so_tien": 450000
    },
    "tong_tien": {
      "phep_tinh": "2.394.000 + 450.000 = 2.844.000",
      "so_tien": 2844000
    }
  },
  "ket_qua": "[RESULT: 2.844.000 đồng]"
}
```

**Link trò chuyện:**https://share.gemini.google/Cl89fq4GnKVm

### b) Kiểm tra kết quả AI

ĐÚNG

## 4.4. Dây chuyền xử lý khiếu nại – kết quả Trạm 1

**KET QUA AI**

```text
(Dán câu trả lời của AI vào đây (bấm nút Copy dưới câu trả lời) – xoá dòng này)
```

**Link trò chuyện:**

## 4.5. Prompt chấm bài theo Rubric

**KET QUA AI**

```text
(Dán câu trả lời của AI vào đây (bấm nút Copy dưới câu trả lời) – xoá dòng này)
```

**Link trò chuyện:**
