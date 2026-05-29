# Thông tin cá nhân

- Họ và tên: Mai Ngọc Duy
- MSSV: 2A202600736
- Nhóm: Vbirds

# Nhật ký chiêm nghiệm về việc tương tác với AI trong buổi học

Trong suốt buổi học, em đã sử dụng các công cụ AI như ChatGPT, Gemini và Claude như một **trợ lý đồng hành tư duy** hơn là một công cụ thay thế hoàn toàn cho quá trình suy nghĩ của mình. Qua quá trình làm việc, em nhận ra AI có thể hỗ trợ rất tốt trong việc mở rộng ý tưởng, gợi ý hướng triển khai và kiểm tra lại lập luận, nhưng kết quả của AI vẫn cần được con người đánh giá, chỉnh sửa và đặt giới hạn rõ ràng.

## 1. AI đã giúp gì trong quá trình học?

Trước hết, AI giúp em **brainstorm ý tưởng quy trình** cho bài học và dự án nhóm. Khi cần phân tích một vấn đề liên quan đến ứng dụng AI trong doanh nghiệp, em dùng AI để gợi ý các bước như xác định bài toán, kiểm tra dữ liệu đầu vào, đánh giá rủi ro khi AI trả lời sai, và đề xuất cơ chế Human-in-the-loop hoặc fallback rule-based. Những gợi ý này giúp nhóm có khung suy nghĩ ban đầu nhanh hơn, thay vì bắt đầu từ một trang trắng.

Ngoài ra, em cũng dùng AI để **viết và cải thiện prompt**. Ban đầu, prompt của em còn khá chung chung, ví dụ chỉ yêu cầu AI “đề xuất giải pháp AI cho quy trình chăm sóc khách hàng”. Kết quả trả về thường rộng, thiếu tiêu chí kỹ thuật và khó dùng cho phần đánh giá. Sau đó, em nhờ AI viết lại prompt theo hướng cụ thể hơn, yêu cầu có đầu vào, đầu ra, rủi ro, chi phí, độ khó triển khai và tiêu chí thành công. Nhờ vậy, câu trả lời trở nên có cấu trúc và dễ đưa vào báo cáo hơn.

Một phần quan trọng khác là em đã dùng AI để **tìm hiểu prompt injection và cách phòng thủ**. AI giúp mô phỏng một số tình huống người dùng cố tình yêu cầu hệ thống bỏ qua hướng dẫn ban đầu, tiết lộ thông tin nội bộ hoặc trả lời ngoài phạm vi cho phép. Qua đó, em hiểu rõ hơn rằng khi xây dựng hệ thống AI, không chỉ cần prompt tốt mà còn phải có ranh giới an toàn, kiểm tra đầu vào và cơ chế từ chối hợp lý.

Em cũng sử dụng AI để **hỗ trợ sửa lỗi code Python**. Khi gặp lỗi trong hàm xử lý kết quả hoặc định dạng bảng Markdown, AI giúp em đọc traceback, xác định nguyên nhân có thể nằm ở cấu trúc dữ liệu đầu vào, tên key không thống nhất hoặc hàm format chưa xử lý đủ trường hợp. Dù không phải lúc nào AI cũng sửa đúng ngay lần đầu, nó giúp em thu hẹp phạm vi lỗi và suy nghĩ có hệ thống hơn.

## 2. AI đã sai hoặc gây vấn đề ở điểm nào?

Một điểm em nhận thấy rõ là AI đôi khi **đưa ra câu trả lời có vẻ hợp lý nhưng không hoàn toàn đúng với bối cảnh bài học**. Ví dụ, khi được yêu cầu đề xuất giải pháp cho một quy trình cụ thể, AI có xu hướng đề xuất một hệ thống quá lớn, bao gồm nhiều thành phần như chatbot, phân tích cảm xúc, dashboard thời gian thực, tích hợp CRM và tự động ra quyết định. Tuy nhiên, với phạm vi của một prototype trong buổi học, giải pháp đó quá phức tạp, tốn chi phí và khó đánh giá.

Ngoài ra, AI đôi khi **hallucinate** bằng cách giả định rằng nhóm đã có sẵn dữ liệu sạch, log lịch sử đầy đủ hoặc API tích hợp ổn định, trong khi thực tế những điều này chưa được xác nhận. Nếu chấp nhận ngay câu trả lời đó, nhóm có thể đưa ra quyết định GO quá sớm mà chưa kiểm tra điều kiện dữ liệu và baseline.

Một vấn đề khác là khi thảo luận về prompt injection, AI có thể tạo ra các ví dụ bypass khá mạnh. Dù mục đích là học cách phòng thủ, một số ví dụ có thể vượt quá mức cần thiết nếu không đặt giới hạn rõ. Điều này cho thấy AI cần được hướng dẫn rõ rằng mục tiêu là phân tích rủi ro và thiết kế phòng vệ, không phải tối ưu cách tấn công.

## 3. em đã sửa đổi prompt và bổ sung ranh giới như thế nào?

Sau khi nhận thấy các vấn đề trên, em điều chỉnh cách đặt câu hỏi với AI. Thay vì hỏi rộng, em bắt đầu thêm **bối cảnh, vai trò, phạm vi và tiêu chí đánh giá** vào prompt. Ví dụ, em yêu cầu AI trả lời trong vai trò cố vấn kỹ thuật cho một prototype nhỏ, không đề xuất hệ thống quá phức tạp, chỉ tập trung vào giải pháp có thể kiểm thử trong thời gian ngắn và có dữ liệu đầu vào rõ ràng.

em cũng bổ sung các ràng buộc như:

- Không được giả định rằng dữ liệu đã sạch nếu đề bài chưa nói rõ.
- Phải phân biệt giữa điều đã biết, điều đang giả định và điều cần kiểm chứng.
- Nếu đề xuất AI, phải nêu rõ khi nào nên dùng rule-based thay vì mô hình AI.
- Khi nói về prompt injection, chỉ được trình bày ở mức phục vụ phòng thủ, không hướng dẫn bypass chi tiết để lạm dụng.
- Phải có tiêu chí đánh giá cuối cùng như chi phí, rủi ro, tính khả thi và mức độ sẵn sàng của stakeholder.

Sau khi thêm các ràng buộc này, câu trả lời của AI trở nên thực tế hơn. AI bắt đầu đưa ra các phương án hẹp hơn, ví dụ chỉ xây dựng một prototype phân loại ticket hoặc gợi ý phản hồi cho nhân viên thay vì tự động xử lý toàn bộ quy trình. Điều này phù hợp hơn với tinh thần của buổi học: AI là công cụ hỗ trợ ra quyết định, nhưng quyết định cuối cùng vẫn phải dựa trên dữ liệu, kiểm chứng và trách nhiệm của con người.

## 4. Kết luận chiêm nghiệm

Qua buổi học, em nhận ra rằng AI là một thought-partner rất hữu ích nếu biết sử dụng đúng cách. AI giúp em nghĩ nhanh hơn, nhìn vấn đề từ nhiều góc độ hơn và phát hiện các điểm cần kiểm tra trong quy trình. Tuy nhiên, AI không nên được xem là nguồn chân lý tuyệt đối. Nó có thể sai, có thể giả định quá mức, hoặc đưa ra giải pháp nghe rất thuyết phục nhưng không phù hợp với thực tế.

Bài học quan trọng nhất là người dùng phải biết **đặt prompt có kiểm soát**, kiểm tra lại đầu ra và bổ sung ranh giới rõ ràng. Khi làm việc với AI, vai trò của con người không giảm đi mà trở nên quan trọng hơn: con người phải đặt mục tiêu, kiểm chứng kết quả, đánh giá rủi ro và chịu trách nhiệm cho quyết định cuối cùng.
