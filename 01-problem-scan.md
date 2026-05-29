# Problem Scan - Vin Smart Future: Xanh SM

## Bối cảnh: Tôi là ai?

Tôi là Linh, một AI Product Engineer tại **Vin Smart Future**. Trong bài scan cá nhân này, tôi chọn tập trung vào **Xanh SM** vì đây là mảng có nhiều quy trình vận hành thời gian thực: điều phối chuyến, xử lý sự cố tài xế, hỗ trợ khách hàng và tối ưu đội xe điện.

Mục tiêu của tôi là tìm các bottleneck cụ thể trong hoạt động hằng ngày của Xanh SM, nơi nhân viên đang phải đọc nhiều thông tin, thao tác nhiều hệ thống hoặc ra quyết định nhanh trong giờ cao điểm. Các bài toán dưới đây được đánh giá theo 4 lenses: **Lặp lại, Tốn thời gian, AI-upgrade, Stakeholder Pain**.

---

# Phase 1 - SCAN: Tìm kiếm cơ hội (Cá nhân)

Dùng **4 Lenses** quét qua vận hành của **Xanh SM**.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM** | Stakeholder Pain / AI-upgrade | Điều phối viên phải xử lý thủ công các chuyến bị hủy, tài xế đến sai điểm đón, khách đổi điểm đón hoặc kẹt xe giờ cao điểm. Mỗi ca sự cố mất khoảng 5-8 phút, làm khách chờ lâu và tăng tỷ lệ hủy chuyến. |
| 2 | **Xanh SM** | Tốn thời gian | Tài xế báo xe gần hết pin hoặc gặp lỗi sạc giữa ca. Điều phối viên phải tra vị trí xe, trạm sạc gần nhất, tình trạng trụ sạc và hướng dẫn tài xế thủ công, mất khoảng 10-15 phút/lượt. |
| 3 | **Xanh SM** | Lặp lại | Bộ phận CSKH phải đọc ghi chú tài xế, phản ánh khách hàng và lịch sử chuyến để phân loại lý do hủy chuyến. Việc này lặp lại hằng ngày và khó tìm pattern lỗi hệ thống nếu chỉ tổng hợp thủ công. |
| 4 | **Xanh SM** | AI-upgrade | Chatbot/app hỗ trợ khách hàng hiện chỉ trả lời theo kịch bản cố định khi khách hỏi về phí hủy, đồ thất lạc, tài xế đến trễ hoặc đổi điểm đón. Nhiều trường hợp vẫn phải chuyển tổng đài vì thiếu khả năng hiểu ngữ cảnh chuyến. |
| 5 | **Xanh SM** | Stakeholder Pain | Tài xế mới hoặc tài xế chạy khu vực lạ thường không biết điểm đón phù hợp tại trung tâm thương mại, sân bay, bệnh viện. Điểm đón không chính xác khiến khách phải đi bộ xa hoặc gọi lại tổng đài. |
---

# Phase 2 - QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

Chọn top 3 từ danh sách SCAN: **#1 (Điều phối chuyến sự cố), #2 (Sự cố pin/sạc), #5 (Gợi ý điểm đón chính xác cho tài xế)**.

---

## Thẻ bài toán tiêu biểu: Card #1 - Xanh SM hỗ trợ điều phối chuyến sự cố

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Điều phối viên Xanh SM cần gợi ý nhanh phương án  │
│ xử lý các chuyến bị hủy, sai điểm đón hoặc trễ giờ.         │
│                                                             │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau?                                                │
│ - Điều phối viên tổng đài/operations bị quá tải giờ cao điểm│
│ - Tài xế và khách hàng chờ lâu khi chuyến có sự cố          │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Hệ thống ghi nhận chuyến có vấn đề                     │
│   → 2. Điều phối viên mở bản đồ, đọc ghi chú, gọi xác minh  │
│   → 3. Tìm tài xế gần nhất hoặc đề xuất điểm đón mới        │
│   → 4. Nhắn tin/gọi điện cho tài xế và khách                │
│   → 5. Theo dõi xem chuyến mới có được chấp nhận không      │
│                                                             │
│ Bước tốn nhất? Bước 2-3 (5-8 phút/chuyến sự cố)             │
│ AI hỗ trợ ở đâu? Bước 2-4: đọc trạng thái chuyến, vị trí,   │
│ lịch sử hủy, mức độ ưu tiên khách và đề xuất 2-3 phương án  │
│ kèm lý do. Điều phối viên vẫn là người bấm chọn cuối cùng.  │
│                                                             │
│ Metric có số:                                               │
│ Giảm thời gian xử lý chuyến sự cố từ 7 phút xuống dưới      │
│ 2 phút; giảm tỷ lệ khách hủy sau sự cố từ 18% xuống dưới    │
│ 10%; 90% gợi ý có đủ lý do để điều phối viên quyết định.    │
│                                                             │
│ Quick Architecture: [x] Agentic Loop có Human-in-the-loop   │
└─────────────────────────────────────────────────────────────┘
```

---

## Thẻ bài toán tiêu biểu: Card #2 - Xanh SM xử lý sự cố pin/sạc của tài xế

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Tài xế Xanh SM báo xe gần hết pin hoặc gặp lỗi    │
│ sạc cần được hướng dẫn đến trạm sạc/cứu hộ phù hợp nhanh.   │
│                                                             │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau?                                                │
│ - Tài xế bị gián đoạn ca chạy và mất doanh thu              │
│ - Điều phối viên phải tra cứu nhiều hệ thống cùng lúc       │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tài xế gọi tổng đài báo pin yếu/lỗi sạc                │
│   → 2. Điều phối viên tra vị trí GPS và mức pin hiện tại    │
│   → 3. Tra trạm sạc VinFast gần nhất còn trụ trống          │
│   → 4. Soạn hướng dẫn đường đi gửi qua app/tin nhắn         │
│   → 5. Gọi đội cứu hộ nếu pin quá thấp hoặc xe không chạy   │
│                                                             │
│ Bước tốn nhất? Bước 3-4 (10-12 phút/lượt)                   │
│ AI hỗ trợ ở đâu? Bước 3-4: lấy vị trí xe, lọc trạm sạc phù  │
│ hợp theo khoảng cách/tình trạng trụ/mức pin, rồi tạo bản    │
│ nháp hướng dẫn cho điều phối viên duyệt.                    │
│                                                             │
│ Metric có số:                                               │
│ Giảm thời gian xử lý sự cố pin/sạc từ 15 phút xuống dưới    │
│ 3 phút; 98% hướng dẫn đúng trạm sạc phù hợp; 100% trường    │
│ hợp pin dưới 5% được gắn cảnh báo cứu hộ thay vì đi xa.     │
│                                                             │
│ Quick Architecture: [x] LLM Feature + Rule guardrails       │
└─────────────────────────────────────────────────────────────┘
```

---

## Thẻ bài toán tiêu biểu: Card #3 - Xanh SM gợi ý điểm đón chính xác cho tài xế

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Tài xế Xanh SM cần gợi ý điểm đón chính xác tại   │
│ khu vực phức tạp như trung tâm thương mại, sân bay, bệnh    |
|viện                                                         |
│ hoặc khu đô thị lớn.                                        │
│                                                             │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau?                                                │
│ - Tài xế mới hoặc tài xế chạy khu vực lạ                    │
│ - Khách hàng phải đi bộ xa, gọi lại hoặc hủy chuyến         │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Khách đặt xe và ghim vị trí gần điểm đón               │
│   → 2. Tài xế xem bản đồ, tự đoán cổng/điểm dừng phù hợp    │
│   → 3. Tài xế gọi khách để hỏi lại vị trí cụ thể            │
│   → 4. Nếu sai điểm, khách phải di chuyển hoặc tài xế vòng  │
│                                                             │
│ Bước tốn nhất? Bước 2-3  (4-6 phút/chuyến khó đón)          │
│ AI hỗ trợ ở đâu? Bước 2: gợi ý điểm đón chuẩn dựa trên địa  │
│ điểm, lịch sử chuyến thành công, quy định dừng đỗ và ghi chú│
│ của khách. Có thể tạo câu nhắn ngắn để tài xế xác nhận.     │
│                                                             │
│ Metric có số:                                               │
│ Giảm thời gian tìm điểm đón từ 5 phút xuống dưới 90 giây;   │
│ giảm tỷ lệ khách hủy do không tìm thấy xe từ 8% xuống dưới  │
│ 3%; 85% chuyến tại điểm phức tạp không cần gọi lại khách.   │
│                                                             │
│ Quick Architecture: [x] LLM Feature + Recommendation Rules  │
└─────────────────────────────────────────────────────────────┘
```

---

## Ghi chú đánh giá nhanh

Trong 3 thẻ trên, **Card #2 - Xanh SM xử lý sự cố pin/sạc của tài xế** là ứng viên phù hợp nhất để deep-dive tiếp vì:

* Actor rõ ràng: tài xế và điều phối viên.
* Workflow hiện tại cụ thể, có bottleneck ở bước tra cứu trạm sạc và soạn hướng dẫn.
* Có thể kết hợp rule-based guardrails với LLM, không cần để AI tự quyết định toàn bộ.
* Metric đo được ngay: thời gian xử lý sự cố, tỷ lệ hướng dẫn đúng trạm, số ca pin thấp được chuyển sang cứu hộ.
