# Phase 3 & 5 — DEEP-DIVE & EVALUATE (Nhóm)

---

## 👥 Thành viên nhóm

| STT | Họ và tên | MSSV |
|-----|-----------|------|
| 1 | Nguyễn Viết Linh | 2A202600719 |
| 2 | Bùi Hoàng Linh | 2A202600804 |
| 3 | Đỗ Thị Huyền |2A202600880 |
| 4 | Mai Ngọc Duy | 2A202600736|

---

## 🗳️ Bài toán được chọn để Deep-Dive

**Bài toán:** Phân loại & chuyển tiếp tự động phản ánh cư dân Vinhomes — hiện tại CSKH xử lý thủ công, cư dân chờ 12-24 tiếng mới được hồi âm. Dự kiến dùng LLM để phân loại + route đúng ban quản lý tòa nhà, giảm xuống dưới 30 phút.

**Tại sao chọn bài này (Card #1 — Vinhomes):**

Chọn Vinhomes vì mấy lý do thực tế: (1) Có bằng chứng thật — vụ cư dân Vinhomes Smart City kéo nhau biểu tình vì khiếu nại thang máy không được xử lý, lên cả báo Vietnet24h với VnExpress. Pain point này không phải đoán mò mà đã thành khủng hoảng truyền thông rồi. (2) Metric đo được — "từ 12-24 giờ xuống dưới 30 phút" là con số cụ thể, có baseline từ hệ thống App Vinhomes Resident. (3) Prototype dễ làm — chỉ cần 1 prompt: cư dân gõ text → LLM phân loại + route → JSON ra. Không phải mock API phức tạp như VinFast hay crawl web như Vinpearl. (4) Workflow có nhiều handoff (Cư dân ↔ CSKH ↔ BQL ↔ Kỹ thuật) nên vẽ sơ đồ sẽ đẹp, ăn điểm phần workflow mapping.

**Tại sao bỏ 2 thẻ kia:**
- VinFast CSKH bảo hành: Bài tốt nhưng phải tích hợp 3-4 hệ thống nội bộ (CRM, xưởng dịch vụ, bảo hành) — prototype 30 phút trong lab không demo được. Thông tin VIN, biển số cũng nhạy cảm, boundary phức tạp hơn.
- Vinpearl Review Monitoring: Thú vị nhưng rủi ro thấp — AI phân loại sai review thì Manager đọc lại vẫn xử lý được. Không đủ "nặng đô" để showcase HITL với boundary test như bài Vinhomes (sai phân loại → cư dân biểu tình).

---

## 🏗️ Phase 3 — DEEP-DIVE

### 3.1. Current-State Workflow

Quy trình xử lý phản ánh cư dân hiện tại tại Vinhomes (ước tính từ mô tả thực tế trên App Vinhomes Resident và phản hồi của cư dân):

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Cư dân gửi   │     │ CSKH mở App  │     │ CSKH đọc &   │     │ CSKH tra cứu │
│ phản ánh qua │ ──→ │ Resident,    │ ──→ │ phân loại    │ ──→ │ tìm BQL tòa  │
│ App/Hotline  │     │ tiếp nhận    │     │ thủ công     │     │ nhà liên quan│
│              │     │              │     │              │     │              │
│ Ai: Cư dân   │     │ Ai: CSKH     │     │ Ai: CSKH     │     │ Ai: CSKH     │
│ ⏱ 5 phút     │     │ ⏱ 3 phút     │     │ ⏱ 8 phút 🔴  │     │ ⏱ 7 phút 🔴  │
│ In: Text/Ảnh │     │ In: Ticket   │     │ In: Nội dung │     │ In: Loại P/A │
│ Out: Ticket  │     │ Out: Đã ghi  │     │ Out: Nhãn   │     │ Out: BQL tên │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                         🔄 Handoff 1              🔄 Handoff 2        ▼
                         (Cư dân→CSKH)            (CSKH→BQL)   ┌──────────────┐
                                                                │ Bước 5       │
                                                                │ Chuyển P/A   │
                                                                │ cho BQL, chờ │
                                                                │ xác nhận     │
                                                                │              │
                                                                │ Ai: CSKH→BQL │
                                                                │ ⏱ 12-24 giờ 🔴│
                                                                │ In: Ticket   │
                                                                │ Out: Phản hồi│
                                                                └──────────────┘
                                                                      │
                                                                      ▼
                                                               ┌──────────────┐
                                                               │ Bước 6       │
                                                               │ BQL phản hồi │
                                                               │ cư dân qua   │
                                                               │ App/Điện thoại│
                                                               │              │
                                                               │ Ai: BQL      │
                                                               │ ⏱ 10 phút    │
                                                               └──────────────┘

🔴 = Bottlenecks (Bước 3, 4, 5 chiếm >95% thời gian chờ)
🔄 = Handoff: Cư dân→CSKH (Bước 1-2), CSKH→BQL (Bước 4-5)

⏱ Tổng thời gian xử lý thủ công: 12-24 giờ/lượt
  (Thời gian thao tác thực tế: ~23 phút, nhưng thời gian chờ giữa các bước: 12-24 giờ)
```

**Nói rõ hơn về 3 bottleneck chính:**

Bước 3 là khâu phân loại — nhân viên phải đọc kỹ từng phản ánh (có cái dài cả trang, có khi đính kèm 3-4 ảnh) rồi tự quyết định đây là phản ánh về gì. Nhân viên mới thiếu kinh nghiệm hay phân loại sai (khoảng 40% lần đầu), mà sai thì chuyển nhầm BQL, phải làm lại từ đầu, kéo dài thêm cả ngày.

Bước 4 là tìm đúng BQL — Vinhomes mỗi khu đô thị có hàng chục tòa, mỗi tòa có ban quản lý riêng, có khi còn chia ra nhóm kỹ thuật điện, nhóm kỹ thuật nước, nhóm vệ sinh... tra thủ công khá mệt.

Bước 5 chuyển ticket xong rồi chờ BQL xác nhận. BQL nhận qua email, mà không phải lúc nào cũng check kịp. Cuối tuần hoặc ngoài giờ thì thôi rồi, có khi 48 giờ không ai đả động. Đây chính là lý do cư dân bức xúc đến mức kéo nhau biểu tình ở Smart City.

---

### 3.2. Problem Statement (6-field)

| Field | Nội dung |
|---|---|
| **1. Actor** | Nhân viên CSKH tại Trung tâm Chăm sóc Cư dân Vinhomes. Mỗi khu đô thị lớn (Smart City, OceanPark, Grand Park...) có khoảng 15-30 nhân viên CSKH, hàng ngày xử lý phản ánh từ hàng chục nghìn cư dân. |
| **2. Current Workflow** | Cư dân gửi phản ánh qua App Vinhomes Resident (viết text + đính kèm ảnh) hoặc gọi hotline. CSKH nhận ticket, đọc nội dung, tự xác định đây là phản ánh về gì (thang máy? điện nước? vệ sinh? ồn ào?), tra xem tòa nhà đó thuộc ban quản lý nào, chuyển tiếp rồi chờ BQL xác nhận. Toàn bộ quy trình 6 bước, phần lớn làm bằng tay, phụ thuộc vào kinh nghiệm từng nhân viên. Dụng cụ: App Resident (nhận ticket), email/Excel (chuyển tiếp), hotline (liên lạc khẩn). |
| **3. Bottleneck** | 3 bước ăn gần hết thời gian: (1) Phân loại thủ công — 8 phút/lượt, nhân viên mới hay phân loại sai (~40% lần đầu), chuyển nhầm BQL thì phải làm lại, kéo dài thêm cả ngày. (2) Tìm đúng BQL — 7 phút, Vinhomes mỗi khu có hàng chục tòa, mỗi tòa BQL riêng. (3) Chờ BQL xác nhận — 12-24 giờ, cuối tuần hoặc ngoài giờ có khi lên 48 giờ. Cư dân chờ quá lâu nên bức xúc, gọi hotline CSKH hỏi liên tục khiến nhân viên thêm tải. |
| **4. Business Impact** | Mỗi ngày ở khu đô thị lớn có khoảng 200-500 phản ánh. Mỗi cái CSKH tốn ~23 phút thao tác → ~115 giờ lao động/ngày chỉ cho khâu phân loại & chuyển tiếp (chưa kể xử lý). Chất lượng dịch vụ giảm rõ — cư dân chờ 12-24 giờ không hồi âm thì CSAT tụt. Nghiêm trọng hơn là khủng hoảng truyền thông: vụ Vinhomes Smart City 2023 cư dân biểu tình vì khiếu nại thang máy không được giải quyết, lên cả báo. Chi phí nhân sự CSKH cũng đội lên — ước tính 150-200 triệu/tháng cho mỗi trung tâm lớn. |
| **5. Success Metric** | (1) Thời gian chuyển tiếp: từ 12-24 giờ → dưới 30 phút. (2) Phân loại đúng lần đầu: từ ~60% → ≥90%. (3) Thời gian thao tác CSKH: từ 23 phút → dưới 7 phút/phản ánh. |
| **6. Operational Boundary** | AI được phép: đọc nội dung phản ánh (text + mô tả ảnh), phân loại vào danh mục cố định (thang máy / điện nước / an ninh / vệ sinh / ồn ào / phí dịch vụ / khác), đánh giá mức khẩn cấp, gợi ý BQL phù hợp, và draft phản hồi sơ bộ cho BQL tham khảo. AI KHÔNG được phép: (1) Tự gửi phản hồi cho cư dân — bắt buộc BQL duyệt trước. (2) Đưa ra quyết định về phí dịch vụ, phí bảo trì hay bất kỳ vấn đề tiền bạc nào. (3) Hứa hẹn thời gian khắc phục cụ thể cho cư dân. (4) Hiển thị thông tin cá nhân cư dân (số căn hộ, SĐT) ngoài nội dung phản ánh. |

---

### 3.3. Future-State Flow & AI Fit

**AI Fit:** LLM Feature — không cần Agent hay rule-based.

Lý do: Phản ánh cư dân viết bằng tiếng Việt tự nhiên, mỗi người mỗi kiểu — có người viết "thang máy tầng 15 kêu to quá", người khác viết "lại bị kẹt thang rồi chán quá", người nữa thì "Gửi BQL: đề nghị kiểm tra PCCC". Rule-based keyword matching sẽ bỏ sót nhiều case kiểu này. Nhưng cũng không cần Agentic Loop vì quy trình chỉ có 1 chiều: đọc → phân loại → route, không cần AI tự quyết chuỗi hành động phức tạp. Agent tự trị mà route nhầm thì cư dân nhận phản hồi sai mà không qua BQL — nguy hiểm. LLM Feature là vừa đủ: hiểu tiếng Việt, phân loại được, output JSON cho con người duyệt.

* **Quy trình tương lai (Future-State):**

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Cư dân gửi   │     │ 🔵 LLM Auto- │     │ 🔵 LLM Auto- │     │ 🟢 BQL nhận  │
│ phản ánh qua │ ──→ │ classify:    │ ──→ │ route: Chọn  │ ──→ │ ticket đã    │
│ App Resident │     │ Loại P/A +   │     │ đúng BQL     │     │ phân loại +  │
│              │     │ Mức khẩn cấp│     │ tòa nhà +    │     │ draft phản   │
│ Ai: Cư dân   │     │              │     │ Draft reply  │     │ hồi, duyệt   │
│ ⏱ 5 phút     │     │ Ai: LLM      │     │ Ai: LLM      │     │ & phản hồi   │
│ In: Text/Ảnh │     │ ⏱ < 10 giây  │     │ ⏱ < 5 giây   │     │              │
│ Out: Ticket  │     │ Out: Label   │     │ Out: Route + │     │ Ai: BQL 🟢   │
└──────────────┘     └──────────────┘     │ Draft        │     │ ⏱ 5 phút     │
                                          └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ┌──────────────┐
                                                               │ Bước 5       │
                                                               │ BQL gửi phản │
                                                               │ hồi cho cư   │
                                                               │ dân qua App  │
                                                               │              │
                                                               │ Ai: BQL 🟢   │
                                                               │ ⏱ 2 phút     │
                                                               └──────────────┘

🔵 = AI Step (LLM Feature): Tự động phân loại, đánh giá urgency, route, draft reply
🟢 = Human Step (HITL): BQL duyệt & gửi phản hồi cho cư dân
↩️ = Fallback: Nếu LLM confidence < 70% → chuyển về CSKH xử lý thủ công như cũ

⏱ Tổng thời gian tương lai: Dưới 30 phút (thay vì 12-24 giờ)
   - Thao tác LLM: < 15 giây
   - BQL duyệt & phản hồi: ~7 phút
   - Thời gian chờ trung bình: ~20 phút (giảm mạnh vì BQL nhận ngay ticket đã xử lý sẵn)
```

**↩️ Fallback Plan:**
- LLM không tự tin (confidence < 70%): Ticket đánh dấu `[CẦN XEM XÉT THỦ CÔNG]`, chuyển về cho CSKH xử lý tay. CSKH vẫn thấy gợi ý của LLM nhưng tự quyết định.
- LLM sập hoàn toàn: Quay lại quy trình thủ công cũ, hệ thống bắn alert cho IT team.
- BQL không phản hồi trong 4 giờ: Tự động escalate lên quản lý khu đô thị + gửi nhắc nhở qua Zalo/SMS.

---

## 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist

| # | Checklist | Có/Không | Ghi chú |
|---|-----------|----------|---------|
| 1 | Có dữ liệu mẫu sạch để test? | Có | App Vinhomes Resident đã lưu toàn bộ ticket phản ánh dạng text. Export 500-1000 ticket gần nhất là có thể test ngay. Dữ liệu có sẵn cấu trúc (thời gian, tòa nhà, nội dung). |
| 2 | Rủi ro AI sai có kiểm soát được? | Có | LLM chỉ tạo draft — BQL luôn phải duyệt trước khi gửi cư dân. Lỡ phân loại sai thì BQL nhận ticket, thấy không đúng loại, tự chuyển lại — vẫn nhanh hơn chờ 12-24 giờ. Fallback 3 cấp rõ ràng. |
| 3 | Stakeholders có chịu thay đổi không? | Có | Sau vụ biểu tình cư dân Smart City, Ban lãnh đạo Vinhomes bị áp lực phải cải thiện. Vinmec đã triển khai DrAid™ rồi, cho thấy Vingroup sẵng sàng đổ tiền vào AI nội bộ. Không thiếu động lực. |

### Quyết định cuối cùng

- [x] **GO** — Bắt đầu xây dựng Prototype
- [ ] NOT YET
- [ ] NO-GO

**Lý do:**

Kỹ thuật thì không có gì quá khó. App Vinhomes Resident đã lưu sẵn hàng trăm nghìn ticket có cấu trúc — chỉ cần export ra là có data test, không phải build pipeline mới. LLM xử lý tiếng Việt tự nhiên tốt hơn keyword matching nhiều (rule-based chắc chỉ được ~60% accuracy, LLM như Gemini 2.5 Flash có thể lên 85%+). Kiến trúc cũng đơn giản: text in → JSON out, không cần fine-tune, chỉ cần prompt engineering tốt. VinBrain (công ty con Vingroup) đã triển khai DrAid™ ở Vinmec rồi nên có kinh nghiệm nội bộ để tham khảo.

Chi phí thì prototype gần như 0 đồng (Gemini API miễn phí). Production ước tính 50-80 triệu/tháng cho 300 ticket/ngày — so với chi phí CSKH hiện tại 150-200 triệu/tháng thì ROI dương ngay tháng đầu. Không làm thì mỗi ngày mất 115 giờ lao động chỉ cho khâu phân loại, chưa kể rủi ro lại có vụ biểu tình lần nữa.

Quyết định: GO, nhưng scope hẹp trước. Chỉ áp dụng cho 1 khu đô thị thí điểm (VinSmart City — nơi phản ánh nhiều nhất và đã xảy ra sự cố). 2 tuần prototype → 1 tháng pilot → đánh giá KPI. Sau 1 tháng mà phân loại đúng dưới 85% hoặc thời gian chuyển tiếp không giảm được 50% thì dừng lại, bổ sung data rồi làm tiếp.
