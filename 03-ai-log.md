# Phase 6 — AI Log & Reflection (Cá nhân)

**Họ và tên:** Nguyễn Viết Linh
**MSSV:** 2A202600719

---

## AI giúp gì?

Dùng Claude (Anthropic) cho phần lớn bài lab này. Cụ thể:

Phase 1 (SCAN): Mình không biết gì về vận hành nội bộ của Vingroup, nên nhờ Claude search web tìm pain point thật cho từng công ty con. Nó pull ra được khá ổn — bài phỏng vấn CEO Xanh SM trên CafeBiz, review 1-2 sao Vinpearl trên TripAdvisor, vụ biểu tình cư dân Vinhomes Smart City trên Vietnet24h. Tự search Google thì cũng ra, nhưng nhanh hơn vì nó lọc sẵn rồi, không phải đọc lướt hàng chục tab.

Phase 2-3 (Quick Cards + Deep-Dive): Nhờ Claude viết draft Problem Statement 6-field rồi mình sửa lại cho sát thực tế hơn. Cũng nhờ nó render text-diagram workflow bằng Unicode box — cái này nếu tự gõ thì chắc mất 30 phút chỉ riêng formatting. Metric cụ thể (12-24 giờ → 30 phút, 60% → 90%) thì mình tự đặt dựa trên cảm nhận, Claude chỉ gợi ý thêm.

Phase 4 (Prompt Prototype): Claude viết nháp SYSTEM_PROMPT ban đầu, mình sửa. Adversarial test cases cũng brainstorm chung. Code evaluate_prompt() thì template có sẵn rồi, chỉ cần đổi SYSTEM_PROMPT với test cases cho khớp bài Vinhomes.

---

## AI sai gì?

**Bịa số liệu.** Mình hỏi "Vinhomes mỗi ngày nhận bao nhiêu phản ánh cư dân?", Claude trả lời "~800 ticket/ngày tại VinSmart City" nghe rất thuyết phục, kèm giải thích "ước tính dựa trên quy mô khu đô thị". Nhưng khi mình hỏi nguồn thì nó không đưa được link nào. Hóa ra nó tự đoán. Mấy con số như "45.000 cư dân", "115 giờ lao động/ngày" cũng kiểu tương tự — nghe hợp lý nhưng không có nguồn gốc rõ ràng.

**SYSTEM_PROMPT viết quá dài.** Lần đầu nhờ Claude viết prompt cho Gemini phân loại phản ánh cư dân, nó tạo ra 15 danh mục phân loại, 5 mức urgency, và một đống rule lồng nhau kiểu "nếu X thì trừ khi Y mà Z thì...". Chạy thử thì Gemini bị loạn, phân loại "phản ánh thang máy" thành "pháp lý" vì cư dân có đe dọa "sẽ kiện" trong text. Prompt dài quá nên model không biết ưu tiên rule nào.

---

## Sửa gì?

Số liệu: đổi cách hỏi — thay vì "cho con số", hỏi "tìm bài báo có con số thật". Kéo được nguồn thì mới dùng, không thì ghi "ước tính" hoặc bỏ luôn. Báo cáo cuối cùng chỉ giữ lại số liệu có nguồn (vụ biểu tình có link, case Nguyên Long có link).

SYSTEM_PROMPT: cắt từ 15 danh mục xuống 8, 5 urgency xuống 3, bỏ hết rule lồng nhau. Chạy lại thì cả 3 adversarial test đều pass. Hóa ra prompt ngắn mà rõ ràng thì model tuân thủ tốt hơn prompt dài.

Cũng một lần tự bắt lỗi: code prompt_prototype.py ban đầu vẫn để SYSTEM_PROMPT về Xanh SM (từ example của đề) trong khi report đã chuyển sang Vinhomes. Claude không nhắc, mình tự đọc lại mới phát hiện rồi sửa cho khớp.

---

## Bài học

Điều mình nhớ nhất: Claude tạo ra text nghe rất tự tin và thuyết phục, nhưng "nghe hay" không có nghĩa là "đúng". Mấy con số nó bịa ra rất khó phân biệt với số thật nếu mình không kiểm tra. Từ giờ dùng AI research, mình sẽ treat mọi thứ nó nói như "lead để verify" chứ không phải "fact để dùng luôn".
