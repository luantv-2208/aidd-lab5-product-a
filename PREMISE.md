# PREMISE — Backlog LAB-5 khác bản LAB-4 đã nộp ở đâu

Mốc: bộ detail design LAB-4 nộp ngày **2026-10-10** (gói `specs/` 310 tệp — README, PREMISE, specs theo QC-01 v0.28.1, AI-report).
Backlog trên Project break theo bản LAB-4 **cộng các dòng dưới đây**. Chỗ nào không có dòng ở đây thì backlog bám nguyên LAB-4.

Nguồn lý do được dẫn bằng mã:
- **FB-R, FB-K, FB-B, FB-A, FB-AI** — phiếu nhận xét LAB-4 của người chấm (163,5 / 200), theo trục: R độ chi tiết, K nhất quán, B khối nghiệp vụ bắt buộc, A kiến trúc / ADR, AI làm việc với AI.
- **H1–H9, M1–M17** — phiếu tự review chéo LAB-1 → LAB-4 ngày 2026-10-10 (4 agent soát, người điều phối mở file gốc kiểm lại mọi mục High); 9 High được ghi là "còn mở" ở AI-report LAB-4 §6. Tóm tắt tự viết: `bang-chung/tom-tat-review-lab4.md`.
- **B-xx, G-xx** — chỗ hở phát hiện khi break LAB-5 (bảng "Chỗ hở" ở `task-breakdown.md`).
- **D1–D4** — quyết định của người chốt ngày 2026-10-24 khi break.
- Q-xx, A-xx, R-xx — mã câu hỏi / giả định / rủi ro của LAB-1.

> Cột "Issue bị ảnh hưởng" ghi `#số mã`: số issue trên GitHub Project (https://github.com/users/luantv-2208/projects/1) và mã work unit ở đầu tiêu đề issue.

## 1. Spec thêm / bỏ / sửa so với LAB-4

| # | Spec (mã / file LAB-4) | Thêm/Bỏ/Sửa | Trước → sau | Vì sao | Issue bị ảnh hưởng |
|---|---|---|---|---|---|
| P-01 | `aitai-trading`: AITAI_KAKUTEI_API, AITAI_KAKUTEI_TECH, SCR020_AITAI_TOROKU; `seri-auction` SERI_KIROKU_TECH §4.6–4.8 | Sửa | API 確定 相対 ghi "không idempotent", chống trùng đẩy sang tài liệu F03-07 vốn chỉ phủ せり → AITAI-002 nhận `tanmatsu_sosa_id` (cột UNIQUE đã có ở TORIHIKI_DB); SCR020 dùng **hàng đợi cục bộ + đồng bộ + 調停** như せり; `doki_kyogo` dùng cho cả 相対. Bản sửa: `spec-sua/ADR_004_CHONG_GHI_TRUNG_DONG_BO.md` | **FB-R**: chỗ chặn việc duy nhất của bộ spec — SCR020 không có thiết kế chống ghi trùng, mọi tài liệu đẩy sang F03-07 vốn loại trừ 相対; **FB-A** khuyên viết ADR riêng vì ảnh hưởng cả せり lẫn 相対; review **H5**; LAB-1 A-06 đã hứa khách hàng đợi cục bộ + giải quyết xung đột; quyết định **D3** | #102 SE-11, #107 AI-10a, #108 AI-10b, #109 AI-10c, #104 SE-04a |
| P-02 | `_common/adr` ADR_003, TORIHIKI_DB (ghi chú `seri_kekka`), SERI_CHOTEI_TECH bước 2, `lot` JIKO_TEISEI_TECH bước 3 | Sửa | ADR/DB nói `seri_kekka` "chỉ ghi qua `kiroku_seri_kekka`" nhưng 調停 INSERT thẳng, 自己訂正 UPDATE `suryo` → một **lõi kiểm ghi giao dịch dùng chung** (khoá lô, tư cách bên mua, khoá 業務日, số lượng) được cả 3 function gọi; ADR-003 sửa thành "chỉ ghi qua function có gọi lõi kiểm" | Chỗ hở **G-04** (mâu thuẫn trong chính LAB-4) | #97 SE-02a, #106 SE-05, #72 LO-11 |
| P-03 | `incentive` SHOREIKIN_KEISAN_TECH §4.4, SHOREIKIN_KAKUTEI_API | Sửa | 確定 thưởng chỉ chặn trùng **đúng cặp ngày** + `KIKAN_MIROKKU` → thêm `KIKAN_MISHURYO` (kỳ chưa hết), ép kỳ = trọn một tháng 業務日, chặn kỳ chồng lấn, advisory lock theo kỳ | Chỗ hở **B-02**: 確定 được tháng đang chạy → giao dịch sau đó lệch tiền mà không sinh 差額; review **M2, M3** (trả hai lần, race với 承認 訂正); F14 đã có đủ các kiểm này | #215 INC-23 |
| P-04 | `incentive` SHOREIKIN_DB `shoreikin_keisan` (CHECK, cột `menjo_id`), SHOREIKIN_KEISAN_TECH §4.3, SCR050, SCR052 | Sửa | 免除 cho `shikyu_gaku_jpy` > 0 dù `kanno_hantei = false` (CHECK `kanno_hantei OR menjo_id IS NOT NULL OR shikyu_gaku_jpy = 0`) → 免除 **chỉ ghi nhận** (biên bản, lý do, hiện cờ ở SCR050 / SCR052), **không đổi 支給額**; CHECK bỏ vế `menjo_id`. Nhánh tiền (支給 khi 免除, 差額 nếu quyết sau 確定) tách thành issue bị chặn chờ Q-30 | **FB-B**: nhánh 免除 trả tiền khi không nộp đủ hạn là quy tắc RFP không có — tắt tới khi khách trả lời Q-30. FR §4.3 có nói ban quản lý "xét miễn" bằng biên bản, nhưng không nói 免除 có trả thưởng không, trả bao nhiêu → tác động tiền là suy diễn, không được đặt thành giả định. Review **H8** (免除 sau 確定 không có đường ra) hết đối tượng khi 免除 không đổi số tiền. Quyết định **D1b** | #212 INC-19, #213 INC-20, #218 INC-24, #209 INC-18, #210 INC-34, #381 H-08 |
| P-05 | `incentive` README (mục ngoài phạm vi), SCR051, SHOREIKIN_DB `gyomu_kitei_han` | Thêm | Không feature nào có màn / function / API quản trị `gyomu_kitei_han` (incentive đẩy sang master-data, master-data loại ra) → tab 「業務規程」 ở SCR051, function `kihyo_/shonin_gyomu_kitei_han` (起票 ≠ 承認, 発効日 tương lai), 3 API | Chỗ hở **B-01**; ADR-002 HQX 3 đã cảnh báo người vận hành sẽ sửa thẳng DB; quyết định **D4** | #192 INC-28, #193 INC-29, #194 INC-30 |
| P-06 | `reconciliation-closing` TEISEI_SHONIN_API, SCR044 | Sửa | API 承認 訂正 không nhắc tác dụng sinh 差額 thưởng → response thêm `sagaku[]`, SCR044 hiện dòng 差額, thêm lỗi 500 khi sinh 差額 hỏng (rollback cả 訂正) | Chỗ hở **B-04** (TECH có gọi `sinh_shoreikin_sagaku`, API và màn không có) | #176 REC-14, #180 REC-16 |
| P-07 | `incentive` SHIHARAI_KAKUNIN_TEISEI_TECH, SCR055 hộp G | Thêm | 支払確認訂正 không 却下 / rút được; một yêu cầu sai chặn mọi yêu cầu mới → thêm `kyakka_shiharai_kakunin_teisei` + API + nút 却下 (cùng khuôn `kyakka_teisei_irai`) | Chỗ hở **B-05**; review **M7** | #204 INC-32 |
| P-08 | `reconciliation-closing` GYOMU_BI_ROKKU_TECH §4.2, SCR041 | Sửa | Lock chỉ chặn 紛争 chưa giải quyết → chặn thêm khi ngày đó còn giao nhận chưa 完了, lô 仮, `doki_kyogo` chưa 調停; trả danh sách việc dở cho SCR041 | **FB-B** (việc nên ưu tiên #2): lần giao đang dở bị kẹt khi 業務日 đã khoá; review **H6, H7**: các việc này sau lock không bao giờ xong; quyết định **D2** | #156 REC-02, #161 REC-04, #379 H-06 |
| P-09 | `notification` TSUCHI_SETTEI_TECH, TSUCHI_JUSHINSHA_SET_API, SCR080 | Thêm | Không có chỗ cài người nhận alert hệ thống (F09-04) dù SCR122 bảo người dùng "nhờ 運用管理者 cài" → TSUCHI-002 nhận thêm loại `kanshi_alert`, thêm khối ở SCR080 | Chỗ hở **G-T1 / G-O4** | #285 TSU-11 |
| P-10 | `_common/architecture` ARCH_SYSTEM §9.9, `reporting` CHOHYO_SAKUSEI_TECH | Sửa | Batch "pg_cron chỉ gọi DB function" nhưng `sakusei_chohyo` tự ghi Storage, sinh PDF; gửi email, chuyển cold cũng cần gọi ra ngoài → DB function lo dữ liệu + dòng xuất; **worker** (route Cron trên Vercel, Node) lo tệp / PDF / email / Storage | Chỗ hở **G-N2, G-R1, G-T4** (Postgres không làm được các việc này) | #21 NEN-14a, #22 NEN-14b, #230 RPT-04, #277 TSU-04a, #338 OPS-09 |
| P-11 | `incentive` SHIHARAI_KAKUNIN_KIROKU_TECH (PATCH trước 確定) | Sửa | `sosa = 'shiharai_kakunin_teisei'` trùng với 訂正 sau 確定 → đổi thành `shiharai_kakunin_koshin` | Chỗ hở **B-13**: audit lẫn hai đường | #197 INC-09 |
| P-12 | `lot` LOT_LIST_API (API-LOT-001); `aitai-trading` SCR020; `seri-auction` SCR024 | Sửa | SCR020 / SCR024 lấy "lô còn số lượng bán" nhưng LOT-001 chỉ trả lô của 業務日 hôm nay, cắt 200 dòng, không có tham số lọc số lượng còn lại → LOT-001 thêm tham số chỉ lấy lô `riyo_kano_suryo > 0` (không giới hạn ngày); hai màn gọi tham số này | **FB-K**: màn và API contract nói khác nhau; chỗ hở **G-10** (350–600 lô / ngày, lô cũ còn hàng không chọn được) | #59 LO-06a, #80 AI-04, #99 SE-07 |

## 2. Giả định bổ sung

Chỉ lấp chỗ LAB-4 / RFP để ngỏ. Không dòng nào ghi đè điều RFP đã nêu rõ.

| # | Giả định | Chỗ spec / RFP để ngỏ | Ảnh hưởng tới issue / estimate |
|---|---|---|---|
| A-01 | 支払日 vào hệ thống bằng **nhập tay** ở SCR055 (`nyuryoku_hoho = te_nyuryoku`). Nhận từ kế toán là issue riêng, để chặn | Q-35 — PREMISE LAB-4 G2 ghi "hở — chặn task tính thưởng" | Tính thưởng break và ước lượng được; issue import (#223 INC-33) giữ trạng thái bị chặn, không xếp sprint |
| A-02 | 5 DB function của lot chỉ mô tả trong API doc (`uketsuke_lot`, `sakusei_kari_lot`, `hon_toroku_kari_lot`, `irai_/kyakka_lot_chosei`) — mục quy tắc của API đủ để dựng, không viết TECH riêng | G-06 (thiếu TECH) | #56 LO-03, #68 LO-09, #74 LO-12 tính thêm phần đọc API + viết test bám API |
| A-03 | Ba môi trường: dev, rehearsal (migration), production; CI chạy trên mỗi PR | G-N1 — ARCH chỉ nêu rehearsal + production | #14 NEN-11, #15 NEN-12 |
| A-04 | "Không tìm thấy": GET trả 404, function ghi trả 409 | G-N4, G-M5 (cùng mã hai HTTP) | #9 NEN-08 |
| A-05 | Mỗi migration bảng mới kèm policy RLS SELECT theo vai đọc ghi ở API doc và chặn ghi trực tiếp — là một mục của định nghĩa xong | B-06 — 14 bảng mới không có thiết kế policy | Mọi sub-issue nhãn `loai:db` cộng phần policy + test |
| A-06 | Bản ghi đồng bộ tới **sau** khi 業務日 đã lock: bị từ chối, vào `doki_kyogo` ở trạng thái chờ; cách xử lý tiếp chờ khách | Q-18; phần còn lại sau D2 | Task quyết định #380 H-07 |
| A-07 | 年4回 đếm theo năm của `hakko_bi`, lúc 承認; rollback cũng tốn lượt (theo ADR-002) | B-09; Q-mới về năm đếm | #185 INC-03 |
| A-08 | Hàng đợi client là **một module dùng chung** cho せり, 相対, giao nhận: ≤ 200 bản ghi, giữ ≤ 5 phút (A-26) | D3; A-26 | #104 SE-04a làm một lần, #108 AI-10b và #145 DEL-09b dùng lại |
| A-09 | Ngưỡng chưa có số (失効間近, 滞留, Critical) dùng giá trị tạm trong `settei_han`, thay bằng cấu hình khi khách trả lời | Q-mới ở notification / delivery | #128 MD-07, #282 TSU-03, #152 DEL-06 không bị chặn |

## 3. Hướng đã chốt

| Vấn đề | Các phương án ở LAB-4 | Phương án chốt | Vì sao |
|---|---|---|---|
| Kỳ 完納奨励金 | PREMISE LAB-4 T5 "không theo tháng dương lịch"; spec SCR050 / A-24 dùng tháng dương lịch theo 業務日 — hai cách đọc ngược nhau (review **H3**, G-N9) | Kỳ = một tháng, từ 業務日 ngày 1 tới 業務日 cuối tháng, ngày lấy bằng `jst_gyomu_bi()` | Khớp spec chi tiết và ràng buộc cutover vào ranh giới tháng; T5 chỉ muốn nói "theo 業務日 JST, không theo giờ UTC" |
| Đứt mạng ở 相対 | H4 "giữ form + gửi lại" · SCR024 hàng đợi IndexedDB · A-06 hứa hàng đợi + giải quyết xung đột | Hàng đợi + đồng bộ + 調停 cho cả 相対 (P-01, ADR-004) | **D3** — giữ lời đã hứa khách cho luồng mang ~90 % giá trị; **FB-R** |
| 免除 và số tiền thưởng | LAB-4: 免除 cho 支給 dù không 完納; review H8 đề xuất thêm nguồn 差額 thứ 3 hoặc chặn 確定 | 免除 chỉ ghi nhận, không đổi 支給額; nhánh tiền chờ Q-30 (P-04) | **D1b** theo **FB-B**: không suy ra quy tắc tiền mà RFP không nêu; đề LAB-5 cấm giả định đè RFP |
| Trạng thái giao dịch so với FIG-012 của RFP | RFP FIG-012 có 下書き / 確認待ち / 確定 / 取消 / 更正 cho cả 相対 và せり; LAB-4 rút còn 確定 / 取消 cho 相対, せり không có cột trạng thái | Giữ bản rút gọn cho backlog lần này, có lý do + câu hỏi khách CLR-06; nếu khách cần đủ trạng thái → thêm issue | **FB-B** yêu cầu ghi lý do rút gọn kèm câu hỏi: kết quả せり do せり進行役 nhập ngay khi chốt giá nên không có bước nháp; 更正 đã đi qua 訂正依頼 sau lock (F05-04); 取消 せり trước lock chưa có quyết định |
| Lock khi còn việc dở | Lock chỉ kiểm 紛争 | Chặn lock + danh sách việc dở (P-08) | **D2** — giữ R7 "lock không mở lại" |
| Chủ `gyomu_kitei_han` | incentive ↔ master-data đẩy cho nhau | F06, tab ở SCR051 (P-05) | **D4** — cùng khuôn 起票 ≠ 承認 với bản tỷ lệ, cùng vai ルール管理者 |
| Nơi chạy batch sinh tệp / gửi email | §9.9 pg_cron chỉ gọi DB function · sơ đồ có "worker từng job" | Lai: pg_cron + DB function cho dữ liệu; route Cron Node cho tệp / email (P-10); spike NEN-14a chốt chi tiết | Postgres không sinh PDF / gửi mail; giữ ADR-001 "luật ở DB" |
| Phạm vi F10, F14, F15 | LAB-4 T3 / H5: OPTION; bản đề xuất gửi khách LAB-1 lại xếp F14 trong 基本範囲 (review **H1**) | Có epic và issue trên Project, nhãn `option`, **không xếp sprint** tới khi khách chốt | Không bỏ phần thiết kế đã làm; không tiêu năng lực sprint cho phần chưa chắc mua |
