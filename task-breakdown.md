# Task breakdown — LAB-5 · Product A (桜湾水産市場)

Backlog trên GitHub Project: https://github.com/users/luantv-2208/projects/1 — break từ bộ detail design LAB-4 nộp 2026-10-10, cộng các thay đổi ở `PREMISE.md`.

## 1. Nguyên tắc chia task

**Ba tầng trên Project** (https://github.com/users/luantv-2208/projects/1) — dùng quan hệ sub-issue thật của GitHub, không dùng checklist:

| Tầng | Là gì | Số lượng | Quy tắc |
|---|---|---|---|
| Epic | Một nhóm chức năng LAB-1 (F01…F15), cộng E00 nền tảng dùng chung và E98 quyết định còn mở từ LAB-4 | 17 | Nhãn `epic`; không có điểm riêng — SWP, Giờ = tổng con |
| Issue | Một sub-feature LAB-4 hoặc một khối việc giao được cho khách xem (ví dụ "Lock 業務日 và chặn ghi ngày đã lock") | 92 | Tách thành hai issue khi một nửa chịu ràng buộc 完納奨励金 và nửa kia là CRUD / màn (ví dụ F05-05a / F05-05b, F06-01 / F06-01u) |
| Sub-issue | Một work unit một người làm, thuộc đúng một loại: `db` migration · `fn` DB function · `api` route · `ui` màn · `test` · `doc` · `spike` · `ops` | 273 | Đây là đơn vị được chấm SWP, ước giờ, xếp sprint |

**Độ hạt — chia tới đâu thì dừng:**
- Dừng khi một sub-issue một người làm xong trong **≤ 2 ngày** (≤ 16 giờ chuẩn middle) và có đúng một loại việc. Khảo sát LAB-4 gắn cờ `[LỚN]` cho mục vượt 2 ngày → tách tiếp (AU-09 → AU-09a/b; AI-09 → AI-09a/b; REC-03 → REC-03a…d theo feature đích; IKO-06 → IKO-06a/b/c theo bảng).
- Tách sub-issue khi: khác người làm (DB / API / màn); khác thời điểm deploy được; khác câu hỏi mở chặn; hoặc một phần chịu ràng buộc 完納奨励金 còn phần kia không.
- Không tách khi hai việc phải lên cùng một PR (ví dụ NEN-02 + NEN-06 siết RLS khi thêm vai ngoài; PA-05 + PA-06 đổi hợp đồng API và màn) — giữ hai sub-issue nhưng ghi "deploy cùng" trong thân.
- Sau khi ước giờ (estimate.md mục 3), 15 sub-issue vượt 16 giờ — ghi "cần tách trước khi xếp sprint", không ép giờ cho vừa trần.
- Việc không phải code cũng là task: tài liệu (runbook, sổ tay, kế hoạch UAT), rehearsal migration, diễn tập, làm rõ với khách, quyết định.

**Định nghĩa xong** — in vào thân mọi issue dạng checklist, theo loại:
- Mọi sub-issue code: PR được review + merge, CI xanh; câu thông báo tiếng Nhật đúng error-codes; spec / PREMISE cập nhật nếu cách làm khác thiết kế.
- Riêng theo loại: `db` migration chạy được hai lần + RLS SELECT theo vai + mỗi ràng buộc có test vi phạm · `fn` revoke khỏi anon, mỗi lỗi ở bảng Error có test, lần bị từ chối ghi `kansa_log` · `api` guardApi + zod, hợp đồng khớp API doc · `ui` đúng SPEC theo vai, đi hết bằng bàn phím, chữ ≥ 16px, vùng chạm ≥ 44px · `test` chạy trong CI, có ca làm hỏng có chủ ý · `spike` kết luận một trang có số đo · `ops` người thứ hai làm lại được · `doc` có người duyệt.
- Thêm khi chịu ràng buộc 完納奨励金: test vector biên (thiếu 1 円, trễ 1 ngày, sát ranh giới tháng theo 業務日), không viết cứng tỷ lệ / 税係数 / làm tròn, đối chiếu bảng quy tắc ở mục 3.
- Issue: mọi sub-issue Done + chạy đầu-cuối trên dev. Epic: mọi issue Done hoặc đóng có lý do. Task làm rõ / quyết định: có người quyết, có hạn (trường **Hạn**), kết quả ghi vào PREMISE.md.

**Trường trên Project:** Status · Iteration (sprint 2 tuần từ 2026-11-09) · SWP (chấm bằng skill, đã duyệt) · Giờ · Người phụ trách (vai giả định kèm trình độ) · Hạn. Nhãn: `epic`, `loai:*`, `rang-buoc:kannou-shoreikin`, `option`, `lam-ro`, `quyet-dinh`, `bi-chan`.


## 2. Spec → issue

Số trong ngoặc = số sub-issue. Truy ngược: mỗi sub-issue ghi spec LAB-4 (file:dòng) trong thân; README.md có bảng Spec → epic.

| Spec (LAB-4 / PREMISE) | Epic | Issue (số sub-issue) | Phần spec chưa thành issue và vì sao |
|---|---|---|---|
| `_common/` (ARCH_SYSTEM, ADR-001, api-conventions, error-codes, log-convention), COMMON_DB; PREMISE P-10 | #1 Nền tảng dùng chung — vai, RLS, lỗi, môi trường, tệp, worker | #2 NEN-A (5), #8 NEN-B (4), #13 NEN-C (5), #19 NEN-D (4) | `_templates/` và sample (tài liệu chuẩn, không phải việc); 6 câu hỏi chuyên gia về log-convention — gửi trong NEN-10, chưa đủ để chia thêm |
| `authentication/`, `participant/`, `_common/KAITE_SHIKAKU_TECH` | #24 F01 — Participant, tư cách 許可/承認/登録 và xác thực | #25 F01-04 (8), #34 F01-05 (5), #40 F01-06 (1), #42 F01-01 (4), #47 F01-03 (3), #51 F01-02 (1) | — |
| `lot/`, LOT_DB; PREMISE P-12 | #53 F02 — Lô hàng: tiếp nhận, 目利き, cân, điều chỉnh, lô 仮 | #54 F02-01 (7), #62 F02-02 (1), #64 F02-05 (1), #66 F02-04 (3), #70 F02-06 (2), #73 F02-07 (2) | Phần đã dựng ở LAB-3 (form tiếp nhận, danh sách 200 dòng, 目利き 0003) — chỉ issue phần delta |
| `aitai-trading/`, `seri-auction/`, TORIHIKI_DB, ADR-003; PREMISE P-01, P-02; ADR-004 | #76 F03 — Giao dịch 相対 và せり, nhập tại sàn khi đứt mạng | #77 F03-01 (4), #82 F03-03 (2), #85 F03-06 (3), #89 F03-08 (5), #95 F03-04 (5), #101 F03-07 (9), #111 F03-RB (2), #114 CLR-06 | Phần đã dựng ở LAB-3 (`kakutei_aitai_torihiki`, `kiroku_seri_kekka`, SCR020/021/023/024) — chỉ issue delta. 取消 せり, trạng thái 下書き / 確認待ち (FIG-012) chưa thành việc — chờ CLR-06 |
| `master-data/`, MASTER_DB | #115 F12 — Master data, cấu hình có kiểm soát, lập lịch batch | #116 F12-01 (6), #123 F12-02 (5), #129 F12-03 (6) | — |
| `delivery/`, HIKIWATASHI_DB | #136 F04 — Giao nhận 荷受け / 引渡し, ngoại lệ, 滞留 | #137 F04-01 (4), #142 F04-02 (3), #146 F04-03 (4), #151 F09-01d (1) | — |
| `reconciliation-closing/`, SHIME_DB; PREMISE P-06, P-08 | #153 F05 — Đối chiếu ngày, lock 業務日, tranh chấp, 訂正 sau lock | #154 F05-02 (8), #163 F05-01 (2), #166 F05-03 (4), #171 F05-04 (3), #175 F05-05a (2), #178 F05-05b (2) | — |
| `incentive/`, SHOREIKIN_DB, ADR-002; PREMISE P-03, P-04, P-05, P-07, P-11 | #181 F06 — 完納奨励金 (ràng buộc nghiệp vụ đặc thù Product A) | #182 F06-03 (4), #187 F06-05 (2), #190 F06-K (4), #195 F06-08a (4), #200 F06-08b (5), #206 F06-06 (3), #210 INC-34, #211 F06-01 (5), #217 F06-01u (1), #219 F06-04 (3), #223 INC-33, #224 CLR-01, #225 CLR-02 | Phần đã dựng ở LAB-3 (`/rates`, 0003 / 0005) — chỉ issue delta. Import 支払日 từ kế toán (SCR055 E-1) là issue bị chặn INC-33, không chia sub-issue tới khi có Q-35 |
| `reporting/`, REPORT_DB | #226 F07 — Báo cáo, xuất 照合 cho kế toán, 取引票, báo cáo tỉnh | #227 F07-01 (9), #237 F07-01q (3), #241 F07-RB (2), #244 F07-04 (2), #247 F07-07 (3), #251 F07-05 (4) | — |
| `audit-log/`, COMMON_DB `kansa_log` | #256 F08 — Audit trail: ghi, tra cứu, phiên bản quy tắc đã áp | #257 F08-01 (3), #261 F08-01b (1), #263 CLR-04, #264 F08-02 (5), #270 F08-03 (2) | KAN-02 (khảo sát) trùng với LO-03 và PA-02 — audit cho lô / 参加者 nằm trong hai function đó, không tạo issue riêng |
| `notification/`, TSUCHI_DB; PREMISE P-09 | #273 F09 — Thông báo nghiệp vụ và cấu hình người nhận | #274 F09-02 (6), #281 F09-01n (1), #283 F09-03 (3), #287 F09-02i (1) | TSUCHI-003 (`TSUCHI_SETTEI_KIHYO_API`) đã ngừng trong LAB-4 — không issue |
| `migration/`, MIGRATION_DB | #289 F11 — Migration 7 năm, rehearsal, cutover, UAT, đào tạo | #290 F11-01 (5), #296 F11-02 (2), #299 F11-01w2 (3), #303 F11-01w3 (2), #306 CLR-05, #307 F11-03 (5), #313 F11-04 (4), #318 F11-05 (2), #321 F11-06 (3) | Thực hiện UAT, buổi đào tạo, hypercare (migration README §6–8) là hoạt động theo lịch sau hiện thực — backlog chỉ có kế hoạch / tài liệu (IKO-17a/b, IKO-18a/b, IKO-19) |
| `operations-security/`, OPS_DB | #325 F13 — Vận hành và an ninh: backup, cold archive, alert, dữ liệu cá nhân | #326 F13-03 (5), #332 F13-01 (4), #337 F13-02 (5), #343 F13-05 (4), #348 F13-04 (5) | Phần NFR mô tả chung ở README (không có hành vi kiểm được riêng) — đi vào DoD và checklist OPS-16 |
| `reference-price/` (OPTION) | #354 F10 — 建値 / 相場 (OPTION) | #355 F10-01 (2) | — |
| `consignment-fee/`, TESURYO_DB (OPTION) | #358 F14 — 委託販売手数料 và chốt sổ tháng 出荷者 (OPTION) | #359 F14-01 (4), #364 F14-02 (3) | — |
| `shipper-portal/` (OPTION) | #368 F15 — Cổng tra cứu cho 出荷者 (OPTION) | #369 F15-01 (3) | — |
| AI-report LAB-4 §6 (9 rủi ro High còn mở); phiếu nhận xét LAB-4 | #373 Quyết định còn mở từ LAB-4 — 9 rủi ro High (AI-report LAB-4 §6) | #374 H-01, #375 H-02, #376 H-03, #377 H-04, #378 H-05, #379 H-06, #380 H-07, #381 H-08, #382 H-09 | 3 quyết định đã chốt khi break (H-03, H-05, H-06) — issue đã đóng (Done), không chấm SWP / Giờ |

## 3. Hạng mục chịu ràng buộc đặc thù của đề (Product A — 完納奨励金)

69 issue / sub-issue mang nhãn `rang-buoc:kannou-shoreikin` (lọc trên Project: `label:rang-buoc:kannou-shoreikin`). Quy tắc lấy từ spec LAB-4 (`incentive/`, `SHOREIKIN_DB`, ADR-002) và PREMISE.md; mỗi dòng dưới đây là một nhóm quy tắc và các issue hiện thực nó.

| Ràng buộc | Issue | Vì sao tách riêng |
|---|---|---|
| Đối tượng chỉ 仲卸 / 売買参加者; 区分 khác bị từ chối (`SHOREIKIN_TAISHOGAI`) | #197 INC-09, #208 INC-17, #213 INC-20 | Luật lọc đối tượng nằm trong cả ghi 支払確認, ghi 免除 và hàm tính — gộp vào CRUD thì không ai kiểm đủ ba chỗ |
| 期間購入額 = 相対 確定 + せり + 訂正 đã 承認, theo 業務日 của giao dịch | #113 AI-12, #112 SE-02b, #172 REC-11, #173 REC-12, #213 INC-20 | Đầu vào nằm ở 3 epic khác (F03, F05); quên vế せり là hụt bậc thưởng — tách để các issue đó mang nhãn và test biên riêng |
| Bậc theo 区分 × 購入額; bản tỷ lệ = bản 承認 có 発効日 lớn nhất ≤ 業務日 đầu kỳ | #183 INC-01, #184 INC-02, #186 INC-06, #214 INC-22, #271 KAN-07a | Sai bản áp là sai tiền cả kỳ cho mọi người; cần issue riêng cho ràng buộc DB (UNIQUE 発効日) và cho hiển thị kỳ áp dụng |
| All-or-nothing: thiếu 1 円 hoặc trễ 1 ngày → 0 thưởng cả tháng | #196 INC-08, #197 INC-09, #213 INC-20, #216 INC-26 | Điều kiện nhị phân trên tiền thật — test biên phải là issue riêng (INC-26), không là một dòng trong test chung |
| 税係数 1,10, 猶予, làm tròn, 年4回 là dữ liệu có phiên bản (起票 ≠ 承認), không viết cứng | #191 INC-05, #192 INC-28, #193 INC-29, #194 INC-30, #185 INC-03 | PREMISE P-05: bảng `gyomu_kitei_han` không ai sở hữu ở LAB-4; nay có màn / function / API riêng |
| Kỳ = tháng theo 業務日 JST; 確定 chỉ khi kỳ đã hết, trọn tháng, không chồng kỳ, mọi ngày đã lock, đủ 支払確認 | #215 INC-23, #155 REC-01, #156 REC-02 | PREMISE P-03: thiếu kiểm này là 確定 được tháng đang chạy → lệch tiền lặng lẽ; lock 業務日 là điều kiện tiên quyết |
| Ngày đã lock không mở lại — mọi hàm ghi phải chặn | #157 REC-03a, #158 REC-03b, #159 REC-03c, #160 REC-03d, #162 REC-17a | Giao dịch ghi được vào ngày đã lock sau 確定 là 購入額 đổi mà không sinh 差額 — chia theo từng feature đích để không sót hàm nào |
| Sai sau 確定 → 差額 kỳ sau, tính lại bằng bản của kỳ gốc; kỳ gốc không UPDATE | #220 INC-21, #176 REC-14, #203 INC-14, #221 INC-25, #222 INC-27, #225 CLR-02 | Hai nguồn 差額 nằm ở hai epic (F05 訂正, F06 支払確認訂正) và phải cùng transaction với 承認 — tách để thấy thứ tự REC-11 → INC-21 → REC-14 / INC-14 |
| 支払確認訂正: 依頼 ≠ 承認, ≤ 1 yêu cầu chờ, có 却下 | #201 INC-12, #202 INC-13, #204 INC-32, #205 INC-15 | PREMISE P-07: thiếu 却下 là buộc phải 承認 số sai |
| 免除 theo biên bản: chỉ ghi nhận, không đổi 支給額 tới khi khách trả lời Q-30 | #207 INC-16, #208 INC-17, #209 INC-18, #210 INC-34, #381 H-08 | PREMISE P-04 / D1b: nhánh tiền là quy tắc RFP không nêu — để riêng ở issue bị chặn INC-34, không lẫn vào ghi nhận |
| Ghi đầu vào lúc 確定 vào audit; xem lại bản quy tắc đã áp | #215 INC-23, #271 KAN-07a, #272 KAN-07b, #242 RPT-03d, #243 RPT-03e | Kiểm toán hỏi 'kỳ này áp tỷ lệ nào' — báo cáo và màn audit đọc dữ liệu thưởng nên mang nhãn ràng buộc |
| Nguồn 支払日 (nhập tay / kế toán) | #224 CLR-01, #223 INC-33 | Q-35 chặn tính thưởng (PREMISE LAB-4 G2); tách issue làm rõ + issue bị chặn để không kéo cả epic đứng chờ |
| Cutover vào ranh giới tháng; nạp kết quả thưởng cũ | #315 IKO-16, #304 IKO-07a, #305 IKO-07b, #306 CLR-05 | Thưởng tính trên cả tháng nên ngày chuyển phải là đầu tháng; wave 3 có FK không nguồn (G-M1) — issue làm rõ riêng |

## 4. Chỗ hở / mâu thuẫn của spec LAB-4 phát hiện khi break

Mã B-xx, G-xx là mã khảo sát khi break (3 agent đọc song song 17 feature, người điều phối mở file gốc kiểm lại các mục nặng). Chỗ hở do người chấm LAB-4 chỉ ra mang mã FB-x (xem `bang-chung/tom-tat-review-lab4.md`).

| Spec | Chỗ hở / mâu thuẫn | Xử lý |
|---|---|---|
| aitai-trading AITAI_KAKUTEI_API:77, SCR020 · TORIHIKI_DB:142 (G-05, FB-R) | 相対 không có thiết kế chống ghi trùng; mọi tài liệu đẩy sang F03-07 vốn chỉ phủ せり | Đã sửa thiết kế — PREMISE P-01, ADR-004 |
| ADR-003, TORIHIKI_DB:182 ↔ SERI_CHOTEI_TECH:168, JIKO_TEISEI_TECH:155 (G-04, G-07) | `seri_kekka` 'chỉ ghi qua kiroku' nhưng 調停 INSERT thẳng, 自己訂正 UPDATE thẳng; bảng hàm gọi kiểm tư cách thiếu 調停 | Đã sửa thiết kế — PREMISE P-02 |
| incentive SHOREIKIN_KEISAN_TECH:110-115 (B-02) | 確定 thưởng không kiểm kỳ đã hết, không ép trọn tháng, kỳ chồng lách được | Đã sửa thiết kế — PREMISE P-03 |
| incentive README:92 ↔ master-data README:57 (B-01) | `gyomu_kitei_han` không feature nào có màn / function quản trị | Đã sửa thiết kế — PREMISE P-05 |
| SHOREIKIN_DB:399,427; SCR052 (FB-B, review H8) | 免除 cho 支給 khi không 完納 — quy tắc RFP không nêu; 免除 sau 確定 không có đường | Đã sửa thiết kế — PREMISE P-04 (chỉ ghi nhận); nhánh tiền chờ #381 |
| TEISEI_SHONIN_API ↔ TEISEI_SHONIN_TECH:153 (B-04) | API 承認 訂正 không nêu sinh 差額 thưởng | Đã sửa thiết kế — PREMISE P-06 |
| SHIHARAI_KAKUNIN_TEISEI_TECH:212 (B-05) | 支払確認訂正 không 却下 / rút được | Đã sửa thiết kế — PREMISE P-07 |
| GYOMU_BI_ROKKU_TECH:130-140 (FB-B, review H6) | Lock ngày khi còn giao nhận dở / lô 仮 → việc dở không bao giờ xong | Đã sửa thiết kế — PREMISE P-08 |
| notification README:46 · SCR122:105 (G-T1, G-O4) | Không có chỗ cài người nhận alert hệ thống | Đã sửa thiết kế — PREMISE P-09 |
| ARCH_SYSTEM:94 ↔ :645 · CHOHYO_SAKUSEI_TECH:62 (G-N2, G-R1, G-T4) | Batch 'pg_cron chỉ gọi DB function' nhưng phải sinh PDF, ghi Storage, gửi email | Đã sửa thiết kế — PREMISE P-10; spike #21 |
| SHIHARAI_KAKUNIN_KIROKU_TECH:145 (B-13) | sosa PATCH trước 確定 trùng sosa 訂正 sau 確定 | Đã sửa thiết kế — PREMISE P-11 |
| LOT_LIST_API:58 ↔ SCR020:207, SCR024:198 (FB-K, G-10) | Màn lấy 'lô còn hàng' nhưng API chỉ trả lô hôm nay, cắt 200 dòng | Đã sửa thiết kế — PREMISE P-12 |
| PREMISE LAB-4 T5 ↔ SCR050:127, AI-report LAB-4 (G-N9, review H3) | Kỳ thưởng 'không theo tháng dương lịch' đọc được hai nghĩa | Đã sửa thiết kế — PREMISE mục 3 'Kỳ 完納奨励金' |
| LOT_CREATE_API:43 và 4 API lot khác (G-06) | 5 DB function chỉ mô tả trong API, không có TECH | Giả định — PREMISE A-02 |
| ARCH_SYSTEM:182 (G-N1) | Không có thiết kế CI và môi trường dev / staging | Giả định — PREMISE A-03 |
| api-conventions:45 · error-codes:111 · IKO_*_API (G-N4, G-M5) | 'Không tìm thấy' lúc 404 lúc 409, cùng mã hai HTTP | Giả định — PREMISE A-04 |
| SHOREIKIN_DB, SHIME_DB, HIKIWATASHI_DB, TESURYO_DB (B-06) | 14 bảng mới không có thiết kế RLS policy | Giả định — PREMISE A-05 |
| ADR-002:139 ↔ RYORITSU_SHONIN_TECH:136 (B-09) | 年4回 đếm theo năm nào chưa thống nhất | Giả định — PREMISE A-07 |
| SERI_CHOTEI_TECH:160 ↔ TEISEI_IRAI_TECH:96 (review H7) | Bản ghi đồng bộ tới sau khi ngày đã lock không có lối ra | Giả định — PREMISE A-06; tạo issue làm rõ #380 |
| SHOREIKIN_KEISAN_TECH:163,170 (B-03) | 差額 treo khi người không mua nữa; 差額 âm lớn hơn thưởng | Tạo issue làm rõ #225 |
| KANSA_GHI_TECH:204 ↔ ADR-001:99 (G-A1) | `ghi_kansa_kyohi` cấp cho authenticated — mở đường chèn audit giả | Tạo issue làm rõ #263 |
| IKO_TORIKOMI_TECH:88 ↔ SHOREIKIN_DB:393 (G-M1, G-M2) | Wave 3 nạp kết quả thưởng cần FK tới bản tỷ lệ / 支払確認 không wave nào nạp | Tạo issue làm rõ #306 |
| RFP FIG-012 ↔ TORIHIKI_DB:135,182 (FB-B) | せり không có trạng thái, 相対 chỉ 確定 / 取消 — rút gọn sơ đồ của khách | Tạo issue làm rõ #114; lý do rút gọn ở PREMISE mục 3 |
| aitai-trading README:95 ↔ PREMISE LAB-4 T4 (G-09, review H2) | Q-12 còn mở mà PREMISE ghi 'chốt' 90/10 | Tạo issue làm rõ #375 |
| LOT, KARI_LOT_TOGO_TECH (review H4) | Q-03 bị dẫn nhầm cho câu hỏi lô 仮 ở 9 chỗ | Tạo issue làm rõ #377 |
| KAITE_SHIKAKU_TECH:161 (G-08) | Luật hiệu lực 資格 nằm 3 nơi, dễ lệch khi Q-34 / Q-39 đổi | Để mở — giữ ba nơi, khoá bằng test đối chiếu #52 |
| SANKASHA_LIST_API:42,57 (G-11) · SERI_LIST_API:44 (G-12) | Đổi hợp đồng không tương thích; bật vai ngoài trước khi siết RLS là lộ dữ liệu | Để mở — ràng buộc thứ tự deploy, ghi trong #49 và #5 |
| COMMON_DB:182 · LOT_DB:204 · nhiều TECH (G-13, G-14, G-M6, G-R3, G-R5, G-T5) | Bảng / function cần bảng của feature khác chưa dựng | Để mở — không phải lỗi thiết kế; thành phụ thuộc 'Làm sau' giữa sub-issue |
| KEIRYO_KIROKU_TECH:79 (G-15) | 計量重量 là 'số tính tiền' nhưng không nối vào tiền giao dịch | Để mở — chờ Q-37, ghi trong #65 |
| PREMISE LAB-4 R11 ↔ SCR002:31 (G-16) · SCR001:97 ↔ ROGUIN_TECH:218 (G-17) · SCR001:201 (G-03) | Ngoại lệ 100 dòng, trang đích, mã timeout nói khác giữa SPEC và TECH | Để mở — làm theo TECH trong #48, #28, #29 |
| ROGUIN_TECH:262 (G-18) | Publishable key cho gọi thẳng Supabase Auth, bỏ qua đếm sai | Để mở — rủi ro đã biết, xử lý ở #33 |
| SCR050:222 · error-codes:33,165 (B-12) | Câu `HAN_NASHI`, `SHIHARAI_KAKUNIN_MISHUTOKU` không hợp ngữ cảnh mới | Để mở — sửa câu khi làm #214 |
| SCR055:142 ↔ SHIHARAI_KAKUNIN_KIROKU_TECH:115 (B-14) | Màn coi cả kỳ 確定, server khoá theo từng người | Để mở — màn lấy theo từng dòng (API đã trả), ghi trong #199 |
| TESURYO_KEISAN_TECH:182 ↔ SHOREIKIN_KEISAN_TECH:164 (B-07) | 購入額 thưởng cộng 訂正, 販売額 phí thì không | Để mở — F14 là OPTION, chờ quyết phạm vi #374 |
| HIKIWATASHI_REIGAI_KIROKU_TECH:153 (B-15) | 返却 / 部分キャンセル giao nhận không đổi số giao dịch → 購入額 vẫn đủ | Để mở — chờ câu hỏi 訂正 số giao nhận (SHIME_DB:338) |
| SHIME_DB:211 ↔ TEISEI_IRAI_TECH:122 (B-18) | 'Một yêu cầu chờ / giao dịch' chỉ ép trong function | Để mở — FOR UPDATE trong function là đủ |
| prototype LAB-3 ↔ spec (G-19, B-16, B-17, G-A5, G-N5) | Phần đã dựng lệch thiết kế (INSERT thẳng không audit, thiếu lock gate, câu lỗi trùng…) | Để mở — không phải hở thiết kế; đã thành issue delta (LO-03, PA-02, INC-01…07, REC-03a…d, NEN-08) |
| ARCH_SYSTEM:697 (G-N3, G-R2) | Function gọi từ pg_cron không có danh tính | Để mở — thành việc #6 |
| log-convention:19 (G-N6) · AI-report LAB-4:93 (G-N7) | Quy ước log 'chưa dùng làm chuẩn được'; 137 câu tiếng Nhật chưa người bản ngữ duyệt | Để mở — chờ chuyên gia; DoD `doc` yêu cầu BrSE duyệt câu |
| KAIKEI_SHOGO_SHUTSURYOKU_TECH:118 (G-R4) · CHOHYO_SAKUSEI_TECH §5 (G-R6) · CHOHYO_SHUTSURYOKU_SAKUSEI_API:82 (G-R8) · REPORT-004 (G-R7) | Cột 税額 không có nguồn; thời hạn lưu tệp; sinh đồng bộ chưa đo; hai đường tải tệp | Để mở — chờ Q-07 / hạn giữ tệp; đo ở #236; dùng chung helper #20 |
| TSUCHI_TOROKU_TECH:211 (G-T3) | Ánh xạ 種別 → 重要度 là hằng số trong code | Để mở — hằng số tạm; thành khoá cấu hình nếu khách muốn tự đổi |
| SCR070 / audit-log README (G-A2, G-A3, G-A4) | Vai xem audit 2 hay 4; RLS SELECT bảng thưởng chưa có; `sankasha_id` chỉ một bên | Để mở — chờ câu hỏi vai; RLS làm trong #271 |
| operations-security README:76,128 · SCR121 (G-O1, G-O2, G-O3, G-O5) | Xoá vật lý không có màn; 内部監査 lập yêu cầu (trái luật chỉ đọc); vai duyệt cold; RPO cần PITR theo gói | Để mở — chờ khách chốt actor; spike #333 |
| nhiều README / ADR-002 / ARCH / DB diagram (G-01, G-02, B-08, B-10, B-11, B-19, B-20, G-N8, G-T2, G-A6, G-O6, G-M3, G-M4) | Ghi chú lỗi thời trong LAB-4: README / TECH ghi API 'chưa có ở api-list', mã 'chưa có ở error-codes', ADR-002 nói 差額 / 免除 'chưa thiết kế', ARCH còn hai nơi phát hiện 滞留, DB diagram index cũ, sosa `iko` sai khuôn | Để mở — không đổi việc; nguồn đúng là api-list / error-codes (sinh tự động) và DB doc; sửa ghi chú khi đụng file |
