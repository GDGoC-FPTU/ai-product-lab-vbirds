# Phase 1 & 2 — SCAN & QUICK-ASSESS (Cá nhân)

**Người thực hiện:** VietLinh

---

## 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội

Mình dùng 4 lenses trong đề để quét qua mấy công ty con của Vingroup. Thực ra ban đầu cũng chẳng biết bắt đầu từ đâu, nên lên mạng đọc review khách hàng, báo cáo vận hành với mấy bài phỏng vấn CEO để tìm pain point thật.

| # | Công ty | Lens | Bài toán |
|---|---------|------|----------|
| 1 | Xanh SM | Lặp lại | Khách đổi điểm đến giữa chừng → tài xế gọi tổng đài → dispatch phân bổ lại cuốc bằng tay. Xanh SM đang có 80.000 xe chạy hàng triệu lượt mỗi ngày, cái này lặp suốt mà toàn xử lý thủ công. |
| 2 | VinFast | Tốn thời gian | Gọi tổng đài CSKH để hỏi bảo hành, nhân viên phải mở 3-4 hệ thống khác nhau tra VIN, lịch sử sửa chữa, tình trạng bảo hành — mỗi cuộc mất 15-20 phút. Đọc trên diễn đàn VinFast thấy có người phải gọi đi gọi lại 4-5 lần mới xong (case anh Nguyên Long, xe VF9 Plus). |
| 3 | Vinhomes | AI-upgrade | Cư dân gửi phản ánh qua App Vinhomes Resident, CSKH đọc rồi phân loại thủ công, mất 12-24 tiếng mới chuyển đúng ban quản lý tòa nhà. Phản hồi thì rập khuôn, thiếu minh bạch. Đỉnh điểm là vụ cư dân Vinhomes Smart City kéo nhau biểu tình vì khiếu nại thang máy không ai xử lý. |
| 4 | Vinmec | Tốn thời gian | Bác sĩ viết tóm tắt hồ sơ xuất viện (Discharge Summary) mất 20-30 phút/bệnh nhân — phải đọc lại toàn bộ bệnh án, xét nghiệm, ghi chú rồi viết lại cho bệnh nhân hiểu được. Vinmec có triển khai DrAid™ cho khoa Ung bướu rồi (giảm 80% thời gian), nhưng mới chỉ áp dụng ở 1 trung tâm, chưa mở rộng. |
| 5 | Vinpearl | Pain từ người khác | Manager các resort phải tự đọc hàng trăm review trên TripAdvisor, Booking.com mỗi tuần để tìm review tiêu cực. Mình vào TripAdvisor đọc thử thì thấy nhiều review 1-2 sao rất chi tiết (phòng bẩn, thiếu khăn, toilet hỏng) mà không thấy phản hồi. Khách quốc tế (Anh, Trung, Hàn) viết bằng đủ thứ tiếng, đọc thủ công cực. |
| 6 | Xanh SM | Pain từ người khác | Tài xế VF8 phàn nàn trên diễn đàn rằng chạy đủ chỉ tiêu doanh thu đã khó rồi, hệ thống gợi ý cuốc giờ cao điểm thì không tối ưu. Một số tài xế chuyển từ Grab sang nhưng lại nghỉ vì áp lực. |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Mình chọn top 3: Vinhomes (#3), VinFast (#2), Vinpearl (#5). Chọn Vinhomes vì có case biểu tình thật, metric rõ ràng, và prototype dễ demo nhất. Chọn VinFast vì pain point ai cũng hiểu (gọi tổng đài chờ đợi). Chọn Vinpearl vì review đa ngôn ngữ là bài toán LLM xử lý tốt hơn con người nhiều.

---

### Card #1 — Vinhomes

```
┌──────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                            │
│                                                                  │
│ Bài toán: Phân loại & chuyển tiếp phản ánh cư dân Vinhomes      │
│ đúng ban quản lý tòa nhà, thay vì để CSKH xử lý thủ công       │
│ rồi cư dân phải chờ 12-24 tiếng mới được hồi âm.                │
│ Công ty: [x] Vinhomes                                            │
│                                                                  │
│ Ai đang đau? CSKH (quá tải, phân loại sai nhiều do thiếu       │
│ kinh nghiệm), cư dân (chờ mãi không ai xử lý, bức xúc)          │
│                                                                  │
│ Workflow thủ công hiện tại (5 bước):                             │
│   1. Cư dân gửi phản ánh qua App (text + ảnh)                   │
│   → 2. CSKH mở ticket, đọc nội dung                              │
│   → 3. CSKH tự phân loại (thang máy / điện nước / vệ sinh...)   │
│   → 4. Tra cứu xem tòa nhà đó thuộc BQL nào                    │
│   → 5. Chuyển ticket cho BQL, rồi chờ họ xác nhận               │
│                                                                  │
│ Bước nào tốn nhất? Bước 3-4-5 (8+7 phút thao tác, nhưng chờ    │
│ BQL xác nhận mới là killer — 12-24 giờ, cuối tuần có khi 48h)   │
│ AI nhảy vào bước nào? Bước 3-4: LLM đọc text → phân loại →     │
│ route đúng BQL → draft phản hồi sơ bộ cho BQL sửa nhanh rồi gửi │
│                                                                  │
│ Metric: "Từ 12-24 giờ xuống dưới 30 phút; phân loại đúng        │
│  lần đầu tăng từ 60% lên 90%"                                   │
│                                                                  │
│ Architecture: [x] LLM Feature                                    │
└──────────────────────────────────────────────────────────────────┘
```

---

### Card #2 — VinFast

```
┌──────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                            │
│                                                                  │
│ Bài toán: Khách gọi tổng đài VinFast hỏi bảo hành, nhân viên   │
│ phải mở 3-4 hệ thống tra cứu thủ công — mỗi cuộc mất 15-20     │
│ phút, khách thì gọi đi gọi lại mãi mới xong.                    │
│ Công ty: [x] VinFast                                             │
│                                                                  │
│ Ai đang đau? Nhân viên CSKH tổng đài (tra tay mỏi), khách hàng  │
│ (chờ 15-20 phút, có người gọi 4-5 lần — như case anh Nguyên    │
│ Long bị VinFast phải sa thải 4 nhân viên vì xử lý chậm)          │
│                                                                  │
│ Workflow thủ công hiện tại (5 bước):                             │
│   1. Khách gọi 1900232389, đọc biển số hoặc VIN                 │
│   → 2. Nhân viên tra thông tin xe trên CRM                       │
│   → 3. Mở hệ thống xưởng dịch vụ xem lịch sử sửa chữa          │
│   → 4. Mở hệ thống khác kiểm tra bảo hành còn hạn không          │
│   → 5. Tổng hợp lại, tư vấn cho khách                            │
│                                                                  │
│ Bước nào tốn nhất? Bước 2-3-4 (10-15 phút — phải nhảy qua lại  │
│ 3-4 hệ thống, copy-paste, nhiều lúc thông tin còn không khớp)    │
│ AI nhảy vào bước nào? Bước 2-4: LLM pull data từ các API, tóm   │
│ tắt tình trạng xe + bảo hành dạng bullet cho nhân viên đọc ngay  │
│                                                                  │
│ Metric: "Tra cứu từ 15 phút xuống dưới 2 phút; giảm 70% số     │
│  lần khách phải gọi lại vì thiếu thông tin"                      │
│                                                                  │
│ Architecture: [x] LLM Feature                                    │
└──────────────────────────────────────────────────────────────────┘
```

---

### Card #3 — Vinpearl

```
┌──────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                            │
│                                                                  │
│ Bài toán: Manager Vinpearl phải tự đọc hàng trăm review trên    │
│ TripAdvisor, Booking.com mỗi tuần để tìm review tiêu cực cần    │
│ phản hồi — nhiều review bằng tiếng Anh/Trung/Hàn, đọc thủ công  │
│ hay bỏ sót những cái nghiêm trọng nhất.                          │
│ Công ty: [x] Vinpearl / VinWonders                               │
│                                                                  │
│ Ai đang đau? Duty Manager từng resort (đọc review mỏi mắt),     │
│ khách hàng (phàn nàn mà không thấy ai reply, giận hơn)           │
│                                                                  │
│ Workflow thủ công hiện tại (5 bước):                             │
│   1. Nhân viên marketing mở từng nền tảng review                 │
│   → 2. Đọc từng review, thấy tiêu cực thì chụp lại              │
│   → 3. Copy vào Excel phân loại                                  │
│   → 4. Email/zalo cho Manager cơ sở liên quan                   │
│   → 5. Manager đọc lại rồi quyết định phản hồi                  │
│                                                                  │
│ Bước nào tốn nhất? Bước 2-3 (20-30 phút/ngày — đọc 100-200     │
│ review, đủ thứ tiếng, dễ bỏ sót review 1-2 sao nghiêm trọng)    │
│ AI nhảy vào bước nào? Bước 2-3: LLM tự crawl review mới, phân  │
│ loại sentiment, đánh dấu cái nào là "urgent" (phòng bẩn, hỏng   │
│ hóc, nhân viên thái độ tệ) rồi bắn alert cho Manager ngay       │
│                                                                  │
│ Metric: "Phát hiện review tiêu cực khẩn cấp từ 24-48 giờ xuống  │
│  dưới 1 giờ; 100% review tiêu cực được Manager thấy trong ngày  │
│  (thay vì bị sót ~40% như bây giờ)"                              │
│                                                                  │
│ Architecture: [x] LLM Feature                                    │
└──────────────────────────────────────────────────────────────────┘
```
