# 01 - Khảo Sát Bài Toán

## Thông tin cá nhân

- Họ và tên: Đỗ Thị Huyền
- MSSV: 2A202600880
- Nhóm: ai-product-lab-vbirds
- Lĩnh vực chọn: Vinmec

---

## Giai đoạn 1 - SCAN: Danh sách 5 bài toán vận hành tại Vinmec

| #   | Công ty thành viên | Góc nhìn                       | Mô tả ngắn bài toán                                                                                                                                                         |
| --- | ------------------ | ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Vinmec             | Tốn thời gian                  | Bác sĩ mất nhiều thời gian soạn tóm tắt hồ sơ xuất viện từ bệnh án điện tử, kết quả xét nghiệm, chẩn đoán, thuốc đã dùng và ghi chú điều trị. Mỗi ca có thể mất 20-30 phút. |
| 2   | Vinmec             | Nâng cấp bằng AI               | Tổng đài/chatbot hiện tại khó phân loại triệu chứng ban đầu của bệnh nhân để gợi ý đúng chuyên khoa như Tim mạch, Hô hấp, Tiêu hóa, Nhi khoa hoặc Cấp cứu.                  |
| 3   | Vinmec             | Lặp lại                        | Nhân viên điều phối phải gọi điện và kiểm tra lịch thủ công để sắp xếp lịch tái khám sau điều trị, dễ trùng lịch bác sĩ, thiếu phòng khám hoặc bỏ sót bệnh nhân.            |
| 4   | Vinmec             | Khó khăn của các bên liên quan | Điều dưỡng phải nhập và kiểm tra lặp lại các chỉ số sinh tồn, thuốc đã dùng, phản ứng phụ vào nhiều biểu mẫu khác nhau, gây quá tải trong các ca trực đông bệnh nhân.       |
| 5   | Vinmec             | Tốn thời gian                  | Bộ phận chăm sóc khách hàng phải đọc thủ công phản hồi và khiếu nại sau khám để phân loại vấn đề như chờ lâu, thái độ phục vụ, chi phí, bảo hiểm và quy trình thanh toán.   |

---

## Giai đoạn 2 - Đánh giá nhanh: 3 Thẻ bài toán

### QUICK PROBLEM CARD #1

**Bài toán (1 câu):** Bác sĩ Vinmec mất nhiều thời gian để soạn tóm tắt hồ sơ xuất viện từ bệnh án điện tử.

**Công ty thành viên:**  
[ ] VinFast [ ] Xanh SM [ ] Vinhomes  
[X] Vinmec [ ] Khác:

**Ai đang đau (Actor)?**  
Bác sĩ điều trị, bác sĩ nội trú, điều dưỡng hỗ trợ hồ sơ.

**Workflow thủ công hiện tại (3-5 bước):**

1. Mở bệnh án điện tử.
2. Đọc chẩn đoán, kết quả xét nghiệm, đơn thuốc và ghi chú điều trị.
3. Chọn các thông tin quan trọng.
4. Soạn tóm tắt xuất viện.
5. Kiểm tra và chỉnh sửa trước khi gửi.

**Bước nào tốn thời gian/lỗi nhất?**  
Bước 2-4, mất khoảng 20-30 phút/bệnh nhân.

**AI có thể nhảy vào hỗ trợ ở bước nào?**  
Tự động trích xuất thông tin và tạo bản nháp tóm tắt xuất viện.

**Đo thành công bằng gì (Metric có số)?**  
Giảm thời gian soạn tóm tắt từ 25 phút xuống dưới 8 phút/bệnh nhân; 100% bản tóm tắt được bác sĩ phê duyệt trước khi gửi.

**Quick Architecture:**  
[ ] No AI [ ] Rule [X] LLM [ ] Agent

---

### QUICK PROBLEM CARD #2

**Bài toán (1 câu):** Tổng đài viên Vinmec gặp khó khăn trong việc xác định đúng chuyên khoa khi bệnh nhân mô tả triệu chứng không rõ ràng.

**Công ty thành viên:**  
[ ] VinFast [ ] Xanh SM [ ] Vinhomes  
[X] Vinmec [ ] Khác:

**Ai đang đau (Actor)?**  
Tổng đài viên, nhân viên chăm sóc khách hàng và bệnh nhân đặt lịch khám.

**Workflow thủ công hiện tại (3-5 bước):**

1. Bệnh nhân mô tả triệu chứng.
2. Tổng đài viên hỏi thêm thông tin.
3. Tra cứu chuyên khoa phù hợp.
4. Gợi ý bác sĩ hoặc lịch khám.
5. Chuyển bác sĩ trực nếu có dấu hiệu khẩn cấp.

**Bước nào tốn thời gian/lỗi nhất?**  
Bước 2-4, mất khoảng 8-12 phút/lượt.

**AI có thể nhảy vào hỗ trợ ở bước nào?**  
Đặt câu hỏi sàng lọc, phân loại mức độ khẩn cấp và gợi ý chuyên khoa phù hợp.

**Đo thành công bằng gì (Metric có số)?**  
85% yêu cầu được gợi ý chuyên khoa trong dưới 2 phút; giảm tỷ lệ chuyển sai chuyên khoa xuống dưới 5%.

**Quick Architecture:**  
[ ] No AI [X] Rule [X] LLM [ ] Agent

---

### QUICK PROBLEM CARD #3

**Bài toán (1 câu):** Bộ phận chăm sóc khách hàng phải đọc thủ công phản hồi và khiếu nại sau khám để phân loại và chuyển xử lý.

**Công ty thành viên:**  
[ ] VinFast [ ] Xanh SM [ ] Vinhomes  
[X] Vinmec [ ] Khác:

**Ai đang đau (Actor)?**  
Nhân viên chăm sóc khách hàng, quản lý vận hành phòng khám và điều phối viên dịch vụ.

**Workflow thủ công hiện tại (3-5 bước):**

1. Thu thập phản hồi từ ứng dụng, email hoặc call center.
2. Đọc từng phản hồi.
3. Gắn nhãn chủ đề.
4. Đánh giá mức độ ưu tiên.
5. Chuyển đến bộ phận liên quan.

**Bước nào tốn thời gian/lỗi nhất?**  
Bước 2-4, mất khoảng 5-10 phút/phản hồi.

**AI có thể nhảy vào hỗ trợ ở bước nào?**  
Tóm tắt nội dung, phân loại chủ đề, gắn mức độ ưu tiên và đề xuất đơn vị tiếp nhận.

**Đo thành công bằng gì (Metric có số)?**  
90% phản hồi được phân loại trong dưới 30 giây; khiếu nại mức độ cao được gắn nhãn ưu tiên trong dưới 1 phút.

**Quick Architecture:**  
[ ] No AI [X] Rule [X] LLM [ ] Agent
