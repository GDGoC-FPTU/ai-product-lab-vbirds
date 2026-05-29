# 02 - Deep-Dive Report

## Thông tin nhóm

- Tên nhóm:
- Thành viên 1:
- Thành viên 2:
- Thành viên 3:
- Thành viên 4:

## Bài toán được chọn

Nhóm chọn bài toán: **AI hỗ trợ bác sĩ Vinmec soạn tóm tắt hồ sơ xuất viện từ bệnh án điện tử**.

Bài toán này được chọn vì đây là một bottleneck rõ ràng trong vận hành bệnh viện. Bác sĩ phải đọc lại nhiều nguồn dữ liệu như chẩn đoán, kết quả xét nghiệm, đơn thuốc, ghi chú điều trị và hướng dẫn tái khám để viết bản tóm tắt cho bệnh nhân. Quy trình hiện tại tốn khoảng 20-30 phút/bệnh nhân, dễ gây quá tải khi số lượng bệnh nhân xuất viện trong ngày cao.

---

## 1. Current-State Workflow

Quy trình hiện tại trước khi có AI:

```text
Bước 1: Bác sĩ mở bệnh án điện tử
  Input: Mã bệnh nhân, hồ sơ khám/chữa bệnh
  Output: Truy cập được hồ sơ liên quan
  Thời gian: 2 phút
        |
        v
Bước 2: Đọc lại chẩn đoán, diễn biến điều trị, xét nghiệm và đơn thuốc
  Input: Bệnh án, kết quả xét nghiệm, ghi chú điều trị
  Output: Danh sách thông tin y khoa quan trọng
  Thời gian: 8-10 phút
  Bottleneck: Có, vì dữ liệu dài và nằm ở nhiều mục khác nhau
        |
        v
Bước 3: Chọn thông tin cần đưa vào tóm tắt xuất viện
  Input: Toàn bộ thông tin trong hồ sơ
  Output: Các ý chính về chẩn đoán, điều trị, thuốc, tái khám
  Thời gian: 5-7 phút
  Handoff: Bác sĩ kiểm tra thêm với điều dưỡng nếu thiếu thông tin
        |
        v
Bước 4: Soạn bản tóm tắt xuất viện bằng ngôn ngữ dễ hiểu
  Input: Các ý chính đã chọn
  Output: Bản tóm tắt xuất viện nháp
  Thời gian: 8-10 phút
  Bottleneck: Có, vì phải vừa chính xác y khoa vừa dễ hiểu cho bệnh nhân
        |
        v
Bước 5: Bác sĩ kiểm tra, chỉnh sửa và ký xác nhận
  Input: Bản tóm tắt nháp
  Output: Bản tóm tắt chính thức gửi cho bệnh nhân
  Thời gian: 3-5 phút
```

**Tổng thời gian xử lý thủ công:** khoảng 25 phút/bệnh nhân.

**Bottleneck chính:** Bước 2-4, đặc biệt là đọc nhiều nguồn dữ liệu và viết lại thành bản tóm tắt rõ ràng, đầy đủ, an toàn.

---

## 2. Problem Statement 6-Field

| Field | Nội dung chi tiết |
|---|---|
| 1. Actor / Operator | Bác sĩ điều trị, bác sĩ nội trú và điều dưỡng hỗ trợ hồ sơ tại Vinmec. |
| 2. Current Workflow | Khi bệnh nhân chuẩn bị xuất viện, bác sĩ phải mở bệnh án điện tử, đọc lại chẩn đoán, diễn biến điều trị, kết quả xét nghiệm, đơn thuốc và ghi chú điều trị. Sau đó bác sĩ tự chọn thông tin quan trọng, viết tóm tắt xuất viện, kiểm tra lại và ký xác nhận. |
| 3. Bottleneck | Bước đọc hồ sơ và soạn bản tóm tắt mất nhiều thời gian nhất, khoảng 20-30 phút/bệnh nhân. Nếu hồ sơ dài hoặc bệnh nhân có nhiều kết quả xét nghiệm, bác sĩ dễ bỏ sót thông tin hoặc mất nhiều thời gian chỉnh sửa. |
| 4. Business Impact | Làm tăng thời gian xử lý xuất viện, khiến bác sĩ bị quá tải, bệnh nhân phải chờ lâu hơn để hoàn tất thủ tục. Nếu mỗi ngày có 40 bệnh nhân xuất viện và mỗi hồ sơ mất 25 phút, tổng thời gian bác sĩ dành cho việc soạn tóm tắt có thể lên tới khoảng 1.000 phút/ngày. |
| 5. Success Metric | Giảm thời gian soạn tóm tắt xuất viện từ 25 phút xuống dưới 8 phút/bệnh nhân. 100% bản tóm tắt do AI tạo phải được bác sĩ kiểm tra và phê duyệt trước khi gửi. Tỷ lệ bản nháp cần chỉnh sửa lớn dưới 15% sau giai đoạn thử nghiệm. |
| 6. Operational Boundary | AI chỉ được tạo bản nháp, không được tự gửi cho bệnh nhân. AI không được tự thêm chẩn đoán, thuốc, kết luận điều trị hoặc lời khuyên y khoa nếu thông tin đó không có trong hồ sơ. Nếu thiếu dữ liệu hoặc có thông tin mâu thuẫn, AI phải đánh dấu để bác sĩ kiểm tra. Bác sĩ là người chịu trách nhiệm phê duyệt cuối cùng. |

---

## 3. Future-State Flow & AI Fit

### AI Fit

Giải pháp phù hợp nhất là **LLM Feature có Human-in-the-loop**.

Lý do:

- Bài toán cần xử lý ngôn ngữ tự nhiên và tóm tắt thông tin dài.
- Không nên dùng Agent tự động vì dữ liệu y tế có rủi ro cao.
- Rule-based có thể hỗ trợ kiểm tra trường bắt buộc, nhưng không đủ linh hoạt để tóm tắt nội dung bệnh án đa dạng.
- Bác sĩ phải là người duyệt cuối cùng trước khi bản tóm tắt được sử dụng.

### Future-State Flow

```text
Bước 1: Bác sĩ chọn bệnh nhân chuẩn bị xuất viện
        |
        v
Bước 2: Hệ thống tự động lấy dữ liệu liên quan
  Dữ liệu gồm: chẩn đoán, diễn biến điều trị, xét nghiệm, đơn thuốc, ghi chú, lịch tái khám
        |
        v
Bước 3: AI tạo bản nháp tóm tắt xuất viện
  AI Step:
  - Tóm tắt chẩn đoán
  - Tóm tắt quá trình điều trị
  - Liệt kê thuốc và hướng dẫn dùng thuốc từ hồ sơ
  - Tạo hướng dẫn tái khám dễ hiểu
  - Đánh dấu thông tin thiếu hoặc mâu thuẫn
        |
        v
Bước 4: Rule-based checker kiểm tra ranh giới
  Rule Step:
  - Có đủ các mục bắt buộc chưa?
  - Có nội dung nào AI tự suy đoán không?
  - Có cảnh báo thiếu dữ liệu không?
        |
        v
Bước 5: Bác sĩ review, chỉnh sửa và phê duyệt
  Human-in-the-loop:
  - Bác sĩ kiểm tra tính chính xác
  - Bác sĩ chỉnh sửa nội dung nhạy cảm
  - Bác sĩ ký xác nhận
        |
        v
Bước 6: Gửi bản tóm tắt chính thức cho bệnh nhân
```

### Fallback

Nếu AI không tạo được bản nháp, thiếu dữ liệu quan trọng hoặc phát hiện thông tin mâu thuẫn, hệ thống chuyển sang quy trình thủ công:

- Hiển thị cảnh báo: "Cần bác sĩ kiểm tra lại hồ sơ".
- Không cho phép gửi bản tóm tắt tự động.
- Bác sĩ tự viết hoặc chỉnh sửa từ đầu như quy trình cũ.

---

## 4. Evaluate - AI Readiness Checklist

| Câu hỏi đánh giá | Trạng thái | Ghi chú |
|---|---|---|
| Có sẵn dữ liệu mẫu/logs sạch để test không? | Cần chuẩn bị thêm | Cần trích xuất một tập hồ sơ đã ẩn danh để thử nghiệm an toàn. |
| Rủi ro khi AI sai có nằm trong tầm kiểm soát không? | Có | Vì AI chỉ tạo bản nháp và bắt buộc bác sĩ phê duyệt trước khi gửi. |
| Stakeholders có sẵn sàng thay đổi quy trình không? | Có điều kiện | Bác sĩ sẽ chấp nhận nếu bản nháp giúp tiết kiệm thời gian thật và không làm tăng gánh nặng kiểm tra. |
| Có metric đo hiệu quả rõ ràng không? | Có | Thời gian xử lý/bệnh nhân, tỷ lệ chỉnh sửa lớn, tỷ lệ hồ sơ cần bác sĩ viết lại từ đầu. |
| Có fallback nếu AI lỗi không? | Có | Quay lại quy trình thủ công và cảnh báo bác sĩ. |

---

## 5. Quyết định cuối cùng

**Quyết định:** **NOT YET - Cần tích lũy thêm dữ liệu và xác lập baseline trước khi triển khai rộng.**

### Lý do

Bài toán có giá trị vận hành rõ ràng và phù hợp với LLM Feature, nhưng thuộc lĩnh vực y tế nên cần kiểm soát rủi ro chặt chẽ. Nhóm không nên triển khai trực tiếp trên dữ liệu thật ngay từ đầu. Cần chuẩn bị bộ dữ liệu hồ sơ đã ẩn danh, đo baseline thời gian bác sĩ đang mất hiện tại, và thử nghiệm nội bộ với một nhóm bác sĩ nhỏ.

### Đề xuất bước tiếp theo

1. Thu thập 50-100 hồ sơ xuất viện đã ẩn danh.
2. Xây dựng prompt prototype tạo bản nháp tóm tắt.
3. Cho bác sĩ đánh giá bản nháp theo 3 tiêu chí: đúng thông tin, đủ ý quan trọng, dễ hiểu cho bệnh nhân.
4. Chỉ chuyển sang trạng thái GO nếu thời gian xử lý giảm rõ ràng và không có lỗi y khoa nghiêm trọng trong giai đoạn thử nghiệm.
