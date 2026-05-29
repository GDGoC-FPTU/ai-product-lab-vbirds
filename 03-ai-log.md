# 03 - AI Log / Reflection

## Thông tin cá nhân

- Họ và tên: Đỗ Thị Huyền
- MSSV: 2A202600880
- Nhóm: ai-product-lab-vbirds
- Lĩnh vực chọn: Vinmec
- Chủ đề làm việc: AI hỗ trợ tóm tắt hồ sơ xuất viện và phân tích bài toán vận hành

---

## 1. AI đã giúp gì trong quá trình làm bài?

Trong bài lab này, tôi sử dụng AI như một thought-partner để brainstorm các bài toán vận hành tại Vinmec. AI giúp tôi mở rộng danh sách pain point thay vì chỉ nghĩ đến các bài toán quen thuộc như chatbot đặt lịch. Từ gợi ý ban đầu, tôi chọn ra 5 bài toán phù hợp với các lens của lab: tốn thời gian, lặp lại, AI-upgrade và stakeholder pain.

AI cũng giúp tôi biến các ý tưởng thành cấu trúc rõ ràng hơn trong phần Problem Scan. Ví dụ, với bài toán "tóm tắt hồ sơ xuất viện", AI gợi ý cách mô tả actor, workflow hiện tại, bottleneck, bước AI có thể hỗ trợ và metric thành công. Nhờ đó, tôi có thể viết Quick Problem Card đầy đủ hơn, không chỉ nêu ý tưởng chung chung.

Ngoài ra, AI hỗ trợ tôi stress-test bài toán bằng cách đặt câu hỏi ngược lại: nếu dùng AI trong lĩnh vực y tế thì rủi ro nào có thể xảy ra, đâu là ranh giới AI không được vượt qua, và khi nào cần bắt buộc có bác sĩ phê duyệt.

---

## 2. AI đã sai hoặc chưa hợp lý ở điểm nào?

Một điểm AI dễ sai là đưa ra giải pháp quá tham vọng, gần như muốn để AI tự động xử lý toàn bộ quy trình y tế. Ban đầu, AI có xu hướng đề xuất hệ thống tự động tạo tóm tắt xuất viện và gửi trực tiếp cho bệnh nhân. Điều này không phù hợp với bối cảnh Vinmec vì thông tin y tế có rủi ro cao, nếu AI tóm tắt sai chẩn đoán, đơn thuốc hoặc hướng dẫn tái khám thì có thể ảnh hưởng trực tiếp đến bệnh nhân.

AI cũng có lúc đưa ra metric khá đẹp nhưng thiếu cơ sở, ví dụ "giảm 90% thời gian" hoặc "đạt độ chính xác 99%" mà không nói rõ cách đo. Tôi nhận ra các metric trong bài lab cần thực tế hơn, đo được và gắn với workflow hiện tại, chẳng hạn giảm thời gian soạn tóm tắt từ 25 phút xuống dưới 8 phút mỗi bệnh nhân.

Một điểm nữa là AI có thể nhầm lẫn giữa "gợi ý chuyên khoa" và "chẩn đoán bệnh". Trong bài toán phân loại triệu chứng ban đầu, AI chỉ nên hỗ trợ sàng lọc và gợi ý chuyên khoa, không được đưa ra kết luận chẩn đoán thay bác sĩ.

---

## 3. Tôi đã điều chỉnh prompt và ranh giới như thế nào?

Sau khi thấy AI dễ vượt qua ranh giới an toàn, tôi điều chỉnh prompt theo hướng cụ thể hơn. Tôi yêu cầu AI phải đóng vai trò là trợ lý hỗ trợ vận hành, không phải bác sĩ và không có quyền ra quyết định y khoa cuối cùng.

Với bài toán tóm tắt hồ sơ xuất viện, tôi đặt ranh giới:

- AI chỉ được tạo bản nháp để bác sĩ xem lại.
- AI không được tự ý thêm chẩn đoán, thuốc hoặc lời khuyên điều trị nếu thông tin đó không có trong hồ sơ.
- Tất cả nội dung liên quan đến chẩn đoán, đơn thuốc, lịch tái khám và cảnh báo sức khỏe phải được bác sĩ phê duyệt trước khi gửi cho bệnh nhân.
- Nếu thiếu dữ liệu hoặc thông tin mâu thuẫn, AI phải báo "cần bác sĩ kiểm tra lại" thay vì tự suy đoán.

Với bài toán phân loại triệu chứng, tôi thêm ranh giới:

- AI chỉ gợi ý chuyên khoa hoặc mức độ ưu tiên, không chẩn đoán bệnh.
- Nếu có dấu hiệu nguy hiểm như đau ngực, khó thở, mất ý thức, đau bụng dữ dội hoặc sốt cao kéo dài, AI phải đề xuất chuyển cấp cứu hoặc để nhân viên y tế trực tiếp xử lý.

Qua quá trình này, tôi thấy AI hữu ích nhất khi được dùng để mở rộng suy nghĩ, tạo bản nháp và phản biện ý tưởng. Tuy nhiên, trong lĩnh vực y tế, AI cần có operational boundary rất rõ và bắt buộc Human-in-the-loop để đảm bảo an toàn.
