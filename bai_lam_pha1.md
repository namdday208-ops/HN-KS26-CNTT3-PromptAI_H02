# BÀI LÀM PHA 1 – HACKATHON 02

- **Mã đề:** 08
- **Họ tên:** Hoàng Xuân Nam
- **Lớp:**CNTT3
- **Link repository Pha1:**https://github.com/namdday208-ops/HN-KS26-CNTT3-PromptAI_H02

> Pha 1 (70 phút đầu, KHÔNG dùng AI). Viết xong, kéo thả file này vào repository Pha1 trước mốc giờ giám thị công bố.

## 4.1. Bảo vệ dữ liệu và chống ảo giác

### a) Ghi chú đã ẩn danh hoá

**GHI CHU DA AN DANH**

```text
Khách hàng [Ten_khach], số điện thoại [SDT_khach_hang], số CCCD [CCCD_khach_hang], email [Email_khach_hang], đặt lịch cạo vôi răng (mã CAOVOI) ngày 23/12/2026 cho 2 người lớn và 1 trẻ em 10 tuổi. Khách đã chuyển khoản đặt cọc 200.000 đồng. Khách yêu cầu khám buổi chiều.

```

### b) Hai câu lệnh chống ảo giác

**CAU LENH**

```text
câu 1: chỉ trả lời dựa trên file đính kèm , nếu file không có thông tin thì xin lỗi vì không tìm được 
```

## 4.2. Trích xuất tin nhắn đặt lịch khám ra JSON

**PROMPT**

```text
[context] : Bạn là nhân viên mới của Nha khoa Nụ Cười, cung cấp ba dịch vụ: cạo vôi răng, trám
răng thẩm mỹ và tẩy trẳng răng. Giám đốc phòng khám giao cho bạn xây dựng bộ
prompt để dùng AI xử lý thông tin khách, tin nhằn đặt lịch, báo giá và khiếu nại một
cách an toàn, chính xác.

[request] : dựa vào quy tắc chẩn hóa trong thẻ <quy_tac> bạn hã chuẩn hóa và chuyển `TN1`, `TN2`, `TN3`trong thẻ <tin_nhan> thành 1 mảng JSON 3 đối tượng đúng thứ tự đúng 6 trường (`ma_dich_vu`,`ngay_hen`, `nguoi_lon`, `tre_em`,`khung_gio`,`ghi_chu`)

[explanation] : dựa vào quy tắc chẩn hóa trong thẻ <quy_tac> bạn hã chuẩn hóa và chuyển `TN1`, `TN2`, `TN3`trong thẻ <tin_nhan> thành 1 mảng JSON 3 đối tượng đúng thứ tự đúng 6 trường (`ma_dich_vu`,`ngay_hen`, `nguoi_lon`, `tre_em`,`khung_gio`,`ghi_chu`)
<quy_tac>
QUY TẮC CHUẨN HOÁ (5 quy tắc)
1. ma_dich_vu lấy theo bảng mã dịch vụ.
2. ngay_hen dạng YYYY-MM-DD; tin nhắn không ghi năm thì lấy năm 2026; không có ngày cụ thể thì null.
3. Khách không nhắc trẻ em thì tre_em là 0 và mọi khách là người lớn.
4. khung_gio chỉ nhận "sang", "chieu" hoặc null.
5. ghi_chu chỉ ghi yêu cầu đặc biệt về sức khoẻ, bệnh nền, dị ứng, tâm lý; không có thì null.
</quy_tac>
ví dụ :
Tin nhắn mẫu 1:
Đặt lịch cạo vôi răng ngày 12/11/2026 cho 1 người lớn và 2 bé, buổi sáng nha. Một bé rất sợ khoan.
Kết quả đúng:
{"ma_dich_vu": "CAOVOI", "ngay_hen": "2026-11-12", "nguoi_lon": 1, "tre_em": 2, "khung_gio": "sang", "ghi_chu": "1 bé rất sợ khoan"}

Tin nhắn mẫu 2:
Tẩy trắng còn lịch buổi chiều không ạ, em đi 1 mình, hôm nào cũng được.
Kết quả đúng:
{"ma_dich_vu": "TAYTRANG", "ngay_hen": null, "nguoi_lon": 1, "tre_em": 0, "khung_gio": "chieu", "ghi_chu": null}


[action] : dựa vào quy tắc chuẩn hóa=> chuẩn hóa 3 tin nhắn => kết quả là bảng JSON

[tone] :  Ngắn gọn, nghiêm túc, chuyên nghiệp

[extra] :Định dạng đầu ra chỉ là dữ liệu JSON, không chào hỏi , không giải thích.
<tin_nhan>
TN1: Cho mình hẹn trám răng ngày 17/12 nhé, 2 người lớn với 1 bé 9 tuổi, buổi chiều.

TN2: Đặt lịch cạo vôi cho 3 người ngày 06/01/2027 ạ. Một người bị tiểu đường.

TN3: Tẩy trắng răng cuối năm còn lịch không ạ? Nhà em 4 người lớn, một người dị ứng thuốc tê, đi buổi sáng.
</tin_nhan>
```

## 4.3. Báo giá bằng Chain-of-Thought

### a) Prompt

**PROMPT**

```text
[Context] : Bạn là nhân viên mới của Nha khoa Nụ Cười, cung cấp ba dịch vụ: cạo vôi răng, trám
răng thẩm mỹ và tẩy trẳng răng. Giám đốc phòng khám giao cho bạn xây dựng bộ
prompt để dùng AI xử lý thông tin khách, tin nhằn đặt lịch, báo giá và khiếu nại một
cách an toàn, chính xác.

[Request] :đọc và tính tổng tiền cho yêu cầu báo giá, suy luận từng bước theo thứ
tự tiền người lớn - tiền trẻ em - tiền dịch vụ - giảm giá nhóm - phí X-quang
- tổng tiền, mỗi bước ghi phép tính; dòng cuối có dạng [RESULT: <tổng
tiền> đồng].

[Explanation] :khi tính tổng tiền bạn dựa vào phần <bang_gia>
 để xem giá và quy tắc tính giá vé để tính tổng tiền trong thẻ <yeu_cau>
<bang_gia>BẢNG GIÁ DỊCH VỤ (đồng/người lớn)
CAOVOI – Khám và cạo vôi răng: 350.000
TRAM – Trám răng thẩm mỹ (1 răng): 500.000
TAYTRANG – Tẩy trắng răng: 2.500.000

QUY TẮC TÍNH GIÁ
1. Trẻ em từ 6 đến 12 tuổi tính 80% giá người lớn. Trẻ dưới 6 tuổi được khám miễn phí.
2. Nhóm từ 5 người lớn trở lên được giảm 10% trên tiền dịch vụ (tiền người lớn + tiền trẻ em).
3. Phí chụp X-quang: 150.000 đồng/phim. Phí này không được giảm giá.
4. Tổng tiền = tiền dịch vụ sau giảm + phí chụp X-quang.
</bang_gia>
<yeu_cau>
YÊU CẦU BÁO GIÁ
Nhóm nhân viên công ty làm dịch vụ CAOVOI gồm 6 người lớn, 2 trẻ em (7 tuổi và 11 tuổi) và 1 bé 4 tuổi. Có 3 người cần chụp X-quang (mỗi người 1 phim).
</yeu_cau>

[action] : đọc yêu cầu => kiêmt tra bảng giá=> tính tổng tiền

[Tone] : Ngắn gọn, nghiêm túc, chuyên nghiệp

[Extra] : Định dạng đầu ra chỉ là dữ liệu JSON
```

## 4.4. Dây chuyền xử lý khiếu nại (Prompt Chaining)

### a) Prompt Trạm 1

**PROMPT**

```text
(Viết prompt vào đây – xoá dòng này)
```

### b) Hai điều cần kiểm tra ở JSON đầu ra của Trạm 1

1. (Viết câu trả lời vào đây – xoá dòng này)

2.

### c) Câu lệnh viết tiếp

**CAU LENH**

```text
(Viết câu lệnh vào đây – xoá dòng này)
```

## 4.5. Prompt chấm bài theo Rubric

**PROMPT**

```text
(Viết prompt vào đây – xoá dòng này)
```

## 4.6. Hoàn thiện System Prompt

> Phần Vai trò và Chuẩn đầu ra đã cho sẵn. Viết tiếp phần Quy tắc ràng buộc và Xử lý ngoại lệ.

```text
[Vai trò]
Bạn là "Trợ lý Nụ Cười", nhân viên tư vấn trên website của Nha khoa Nụ Cười, giúp khách chọn và đặt lịch một trong 3 dịch vụ: CAOVOI (Khám và cạo vôi răng), TRAM (Trám răng thẩm mỹ), TAYTRANG (Tẩy trắng răng).

[Quy tắc ràng buộc]
1. (Viết quy tắc 1 – xoá dòng này)
2.
3.

[Chuẩn đầu ra]
Trả lời bằng tiếng Việt, lịch sự, tối đa 120 từ. Giá dịch vụ trình bày dạng gạch đầu dòng. Kết thúc bằng một câu hỏi về nhu cầu tiếp theo của khách.

[Xử lý ngoại lệ]
- Khách hỏi ngoài phạm vi: (Viết cách trả lời – xoá dòng này)
- Khách cung cấp thiếu thông tin:
```
