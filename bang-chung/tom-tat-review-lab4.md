# Tóm tắt review LAB-4 dùng làm nguồn cho PREMISE.md

Tự viết lại từ hai nguồn, chỉ giữ điểm ảnh hưởng tới backlog LAB-5. Mã ở cột đầu là mã được dẫn trong `PREMISE.md` và trong thân issue.

## 1. Phiếu nhận xét LAB-4 của người chấm

| Mã | Trục | Điểm người chấm nêu (diễn đạt lại) | LAB-5 xử lý |
|---|---|---|---|
| FB-R | Độ chi tiết đủ để làm | Một chỗ chặn việc: SCR020 (ghi 相対取引) không có thiết kế chống ghi trùng khi gửi lại; mọi tài liệu đẩy sang tài liệu F03-07, mà tài liệu đó chỉ thiết kế cho せり | PREMISE P-01; `spec-sua/ADR_004_CHONG_GHI_TRUNG_DONG_BO.md`; epic F03 issue F03-07 |
| FB-K | Nhất quán | SCR020 lấy "lô còn số lượng bán" nhưng API danh sách lô chỉ trả lô của 業務日 hôm nay, không có tham số lọc số lượng còn lại | PREMISE P-12 |
| FB-B | Khối nghiệp vụ bắt buộc | (1) Nhánh 免除 cho trả thưởng khi không nộp đủ hạn — quy tắc tiền không có trong RFP, nên tắt tới khi khách trả lời Q-30. (2) Lần giao đang dở bị kẹt khi 業務日 đã khoá. (3) せり rút gọn sơ đồ trạng thái giao dịch của khách — cần ghi lý do kèm câu hỏi | (1) PREMISE P-04, quyết định D1b, issue bị chặn INC-34, task H-08. (2) PREMISE P-08. (3) PREMISE "Hướng đã chốt — trạng thái giao dịch", issue làm rõ CLR-06 |
| FB-A | Kiến trúc / ADR | Khi thêm chống ghi trùng cho 相対, viết một ADR riêng vì ảnh hưởng cả せり lẫn 相対 | ADR-004 |
| FB-P | Phủ và truy vết | Khi break task, gắn mã task vào bảng feature để biết task nào làm lại khi khách đổi chức năng | README phần "Spec → epic", task-breakdown phần "Spec → issue" |
| FB-AI | Làm việc với AI | Đưa quyết định quan trọng vào chính AI-report thay vì nhật ký ngoài bài; trích nguyên văn yêu cầu đã giao AI con; biến 9 rủi ro High còn mở thành task có người quyết và hạn | AI-report LAB-5; epic E98 (9 task quyết định, trường Người phụ trách + Hạn) |

## 2. Tự review chéo LAB-1 → LAB-4 (2026-10-10, sau khi gom bộ spec)

Bốn agent chỉ đọc soát bốn hướng (LAB-1 ↔ LAB-4, prototype ↔ LAB-4, logic giữa các feature, yêu cầu khách → LAB-1…4); người điều phối mở file gốc kiểm lại mọi mục High. 9 High ghi "còn mở" ở AI-report LAB-4 §6:

| Mã | Vấn đề (diễn đạt lại) | LAB-5 xử lý |
|---|---|---|
| H1 | F14 委託販売手数料: bản đề xuất gửi khách tính trong 基本範囲, bộ spec ghi OPTION | Task quyết định H-01; epic F14 nhãn `option`, không xếp sprint |
| H2 | PREMISE LAB-4 ghi "chốt" cho dòng còn phụ thuộc câu hỏi mở | Task quyết định H-02 |
| H3 | Kỳ 完納奨励金: cách viết T5 đọc được hai nghĩa | PREMISE "Hướng đã chốt — kỳ 完納奨励金"; task H-03 (đã quyết) |
| H4 | Mã câu hỏi Q-03 bị dẫn nhầm cho câu hỏi lô 仮 | Task quyết định H-04 |
| H5 | Chống ghi trùng / mất mạng cho 相対 không có ở đâu (trùng FB-R) | PREMISE P-01, ADR-004; task H-05 (đã quyết, D3) |
| H6 | Khoá 業務日 khi còn giao nhận dở / lô 仮 → việc dở không bao giờ xong (trùng FB-B 2) | PREMISE P-08; task H-06 (đã quyết, D2) |
| H7 | Bản ghi đồng bộ tới sau khi ngày đã khoá không có lối ra | PREMISE A-06; task quyết định H-07 (chờ khách) |
| H8 | 免除 quyết sau 確定 không có đường ra | Hết đối tượng khi 免除 không đổi số tiền (P-04, D1b); câu Q-30 thành task H-08 |
| H9 | Bộ LAB-1 còn số cũ (相対 68 %, "Mục x FR") | Task quyết định H-09 |

Mục Med có ảnh hưởng tới issue: M1–M3 (逆仕訳 / 確定 thưởng thiếu kiểm kỳ / race 確定 ↔ 承認 訂正) → PREMISE P-03; M7 (支払確認訂正 không 却下 được) → PREMISE P-07.
