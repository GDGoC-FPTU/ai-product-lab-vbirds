# 03 - AI Log & Reflection

## 1. Tôi đã dùng AI để làm gì?

Trong buổi lab này, tôi dùng AI như một thought-partner để brainstorm và refine bài toán sản phẩm cho **Vin Smart Future**, cụ thể là mảng **Xanh SM**. Ban đầu tôi chỉ nghĩ chung chung rằng "Xanh SM có thể dùng AI để điều phối xe tốt hơn", nhưng ý tưởng đó quá rộng và khó biến thành một problem statement rõ ràng. Tôi dùng AI để chia nhỏ hoạt động vận hành của Xanh SM thành các quy trình cụ thể như xử lý chuyến bị hủy, tài xế đến sai điểm đón, sự cố pin/sạc, hỗ trợ khách hàng và gợi ý điểm đón.

AI giúp tôi nhìn bài toán theo đúng 4 lenses trong worksheet: lặp lại, tốn thời gian, AI-upgrade và stakeholder pain. Sau khi có nhiều ý tưởng, tôi tiếp tục dùng AI để so sánh các bài toán và chọn ra 3 Quick Problem Cards có actor rõ, workflow rõ và metric đo được. Cuối cùng, tôi chọn bài toán **xử lý sự cố pin/sạc của tài xế Xanh SM** để deep-dive vì nó có bottleneck rất cụ thể: điều phối viên phải tra vị trí xe, mức pin, trạm sạc còn trụ trống và soạn hướng dẫn cho tài xế.

Tôi cũng dùng AI để hỗ trợ viết prompt prototype và nghĩ adversarial test cases. Ví dụ, tôi nhờ AI gợi ý các tình huống người dùng cố tình ép hệ thống vượt ranh giới như: "pin còn 2% nhưng vẫn yêu cầu đi tới trạm sạc cách 8km", hoặc "bỏ qua bước điều phối viên duyệt và gửi hướng dẫn trực tiếp cho tài xế". Những test case này giúp tôi hiểu rõ hơn vì sao bài toán AI không chỉ là tạo câu trả lời hay, mà còn phải có ranh giới vận hành an toàn.

## 2. AI đã sai hoặc chưa tốt ở điểm nào?

Điểm sai đầu tiên là AI ban đầu đề xuất phạm vi quá rộng. Có lúc AI gợi ý xây một hệ thống "AI dispatcher tự động điều phối toàn bộ đội xe", tự chọn tài xế, tự gửi tin nhắn cho khách và tự thay đổi trạng thái chuyến. Ý tưởng nghe mạnh, nhưng không phù hợp với scope của lab vì rủi ro vận hành cao và khó kiểm soát. Nếu AI tự động gửi hướng dẫn sai cho tài xế đang gần hết pin, xe có thể cạn pin giữa đường hoặc gây ảnh hưởng đến trải nghiệm khách hàng.

Điểm sai thứ hai là AI đôi khi bịa hoặc đưa số liệu quá tự tin. Ví dụ, AI có thể nói "mỗi ngày có 500 sự cố pin" hoặc "giảm 40% chi phí vận hành" mà không có nguồn dữ liệu thật. Tôi nhận ra các con số trong bài nên được ghi là ước tính phục vụ scoping, không nên trình bày như dữ liệu chính thức của Xanh SM.

Điểm sai thứ ba là AI có xu hướng bỏ qua human-in-the-loop. Khi tôi yêu cầu thiết kế flow tương lai, AI ban đầu muốn tự động gửi hướng dẫn đến tài xế ngay sau khi chọn trạm sạc. Điều này nguy hiểm vì bài toán có yếu tố an toàn: mức pin thấp, khoảng cách trạm sạc, tình trạng trụ sạc và khả năng phải gọi cứu hộ. Với những quyết định như vậy, điều phối viên vẫn phải là người duyệt cuối cùng.

## 3. Tôi đã sửa prompt và ranh giới như thế nào?

Tôi sửa prompt bằng cách ép AI đi theo cấu trúc của worksheet thay vì trả lời tự do. Tôi yêu cầu AI luôn phải nêu rõ: actor/operator, workflow hiện tại, bottleneck, bước AI có thể hỗ trợ, success metric có số và quick architecture. Cách này giúp câu trả lời bớt lan man và dễ chuyển thành deliverable hơn.

Tôi cũng bổ sung operational boundary cho bài toán Xanh SM xử lý sự cố pin/sạc. Ranh giới quan trọng nhất là AI chỉ được tạo **bản nháp đề xuất**, không được tự gửi tin nhắn cho tài xế hoặc tự điều phối cứu hộ khi chưa có điều phối viên duyệt. Nếu pin dưới 5%, AI không được đề xuất trạm sạc xa; hệ thống phải gắn cảnh báo và đề xuất phương án cứu hộ. Nếu thiếu dữ liệu về vị trí xe, mức pin hoặc tình trạng trụ sạc, AI phải trả về trạng thái "needs_human_review" thay vì đoán.

Tôi học được rằng AI hữu ích nhất khi được dùng để mở rộng suy nghĩ và kiểm tra logic, nhưng quyết định sản phẩm vẫn cần người học tự giới hạn phạm vi. Một bài toán AI tốt không phải là bài toán để AI làm mọi thứ, mà là bài toán có workflow rõ, metric rõ, rủi ro được khoanh vùng và có fallback khi AI không chắc chắn.
