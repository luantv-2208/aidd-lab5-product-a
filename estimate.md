# Estimate — LAB-5 · Product A (桜湾水産市場)

## 1. Báo cáo HTML của skill chấm SWP

[`reports/sun-estimate-luantv-2208-project-1-20261024-1420.html`](reports/sun-estimate-luantv-2208-project-1-20261024-1420.html) — sinh bằng `tkm:sun-estimate` từ chính GitHub Project (https://github.com/users/luantv-2208/projects/1); SWP đã duyệt được Agent ghi vào trường **SWP**, giờ ghi vào trường **Giờ**.

Tóm tắt: 286 mục chấm trực tiếp, **1289 SWP**, **2106 giờ**; 93 container lấy tổng con; 3 task đã đóng không chấm.

## 2. Issue mẫu skill chọn để kiểm

Bộ chọn tất định của skill lấy 5 mẫu: nhỏ nhất, trung vị, lớn nhất, xa trung vị nhất, độ tin cậy thấp nhất. Người học kiểm từng issue theo hai vòng (chọn mức rất dễ / dễ / vừa / khó / rất khó, rồi chọn điểm cụ thể trong mức) — không chỉnh trực tiếp Scope / Complexity. Kết quả: **không sửa issue nào**, trạng thái hiệu chỉnh `accepted`, 0 issue khác bị kéo theo.

| Issue | Skill chấm (Scope × Complexity → SWP) | Bạn chốt | Lý do |
|---|---|---|---|
| #10 [NEN-09] Chốt envelope lỗi K7 ({code,message,detail} hay {errors:[…]}) và ghi vào api-conv | Scope 1 × Complexity 0 → **1 SWP** (rất dễ) | **1 SWP** — giữ mức rất dễ, giữ điểm | Giữ nguyên: khớp cảm nhận khi tự dựng prototype LAB-3; thứ tự công sức giữa 5 mẫu đúng (việc nhỏ nhất — một quyết định ghi vào tài liệu, đúng 1 điểm). |
| #234 [RPT-08] SCR060 帳票一覧: xem, tải, xuất lại, lịch sử | Scope 3 × Complexity 1 → **4 SWP** (dễ) | **4 SWP** — giữ mức dễ, giữ điểm | Giữ nguyên: khớp cảm nhận khi tự dựng prototype LAB-3; thứ tự công sức giữa 5 mẫu đúng (màn danh sách gọi API đã có, cỡ như màn /rates đã dựng). |
| #305 [IKO-07b] Wave 3 kết quả thưởng lịch sử + đối chiếu số tiền thưởng | Scope 5 × Complexity 5 → **15 SWP** (khó) | **15 SWP** — giữ mức khó, giữ điểm | Giữ nguyên: khớp cảm nhận khi tự dựng prototype LAB-3; thứ tự công sức giữa 5 mẫu đúng (nặng do dữ liệu cũ chưa thấy và FK chưa có nguồn; điểm đã phản ánh, phần chưa chắc nằm ở CLR-05). |
| #215 [INC-23] tinh_shoreikin nhánh 確定: KIKAN_MISHURYO, trọn tháng, chặn kỳ chồng, KIKAN_MIROKKU | Scope 5 × Complexity 5 → **15 SWP** (khó) | **15 SWP** — giữ mức khó, giữ điểm | Giữ nguyên: khớp cảm nhận khi tự dựng prototype LAB-3; thứ tự công sức giữa 5 mẫu đúng (lõi 確定 thưởng — nặng ngang wave migration, đúng rủi ro tiền). |
| #16 [NEN-10] Helper logger phía server theo log-convention (cấm PII/token, timezone, nơi lưu) | Scope 2 × Complexity 1 → **3 SWP** (dễ) | **3 SWP** — giữ mức dễ, giữ điểm | Giữ nguyên: khớp cảm nhận khi tự dựng prototype LAB-3; thứ tự công sức giữa 5 mẫu đúng (helper nhỏ; độ tin cậy thấp do câu hỏi log-convention, không do cỡ việc). |

Vì sao giữ nguyên cả 5: so với phần chính tay dựng ở LAB-3 (`/rates`, ghi せり, ghi 相対), độ lớn tương đối giữa 5 mẫu hợp lý; chỗ chưa chắc (IKO-07b, NEN-10) đến từ câu hỏi mở chứ không do chấm lệch — sẽ chấm lại khi có trả lời (CLR-05, câu hỏi log-convention).

## 3. Cách quy ra giờ

**Cách chọn: ước trực tiếp từ nội dung issue, không đổi SWP ra giờ.**

**Vì sao:** SWP = tra bảng Scope × Complexity — đo độ lớn và độ khó của *issue*, ai làm cũng vậy. Giờ đo *công của người được giao* để làm xong issue, nên phụ thuộc trình độ người đó. Một quy ước "1 SWP = x giờ" sẽ xoá đúng chỗ khác nhau này: sổ tay 10 vai (IKO-18b) chỉ 3 SWP nhưng 32 giờ chuẩn — rộng mà không khó; còn thêm idempotent cho một function (AI-10a) 4 SWP mà chỉ 4 giờ — hẹp mà khó.

**Quy tắc:**
1. AI đọc từng sub-issue (phần LAB-3 đã dựng, số bảng / màn / route, số kịch bản test, câu hỏi mở) và đề xuất **giờ chuẩn cho một người mức middle**, kèm một dòng lý do. Giờ gồm tự test đơn vị của chính việc đó; không gồm review của người khác, họp, sửa sau test hệ thống.
2. **Giờ ghi trên Project = giờ chuẩn × hệ số trình độ của người phụ trách**, làm tròn 0,5 giờ: senior ×0,8 · middle ×1,0 · junior ×1,5 (đội giả định không có junior).
3. Issue và epic = **tổng giờ của con** theo cây cha–con đã kiểm của skill (không ước riêng).
4. Không ép trần: mục nào vượt 16 giờ chuẩn (2 ngày) giữ số thật và ghi **cần tách trước khi xếp sprint** — 15 mục.
5. 3 task quyết định đã đóng (không chấm SWP) để trống giờ.

| Người phụ trách | Hệ số | Số issue chấm trực tiếp |
|---|---|---|
| PM-1 · senior | ×0.8 | 8 |
| BrSE-1 · senior | ×0.8 | 19 |
| BE-1 · senior | ×0.8 | 85 |
| BE-2 · middle | ×1.0 | 101 |
| FE-1 · middle | ×1.0 | 55 |
| QA-1 · middle | ×1.0 | 18 |

**Áp nhất quán — kiểm thế nào:**
- Tỷ lệ giờ chuẩn ÷ SWP phân tán 1.0 – 10.7 (trung vị 1.60, hệ số biến thiên 0.41) — hai đơn vị không bị suy ra từ nhau.
- AI tự khai một lệch của chính nó: bản đầu ép 16 mục vào đúng trần 16 giờ, làm mục khó bị ước thấp (~1,07 giờ / SWP). Người chọn "ước thật, đánh dấu cần tách" → giờ chuẩn 2158 → 2268.
- **Người duyệt mẫu giờ** — 22 mục rút tất định: mỗi loại việc hai đầu tỷ lệ giờ ÷ SWP, cộng 4 mục ràng buộc 完納奨励金 giờ lớn nhất. Kết quả: duyệt cả 22.

| Issue | Loại | SWP | Giờ chuẩn | Người phụ trách | Giờ trên Project | Người duyệt |
|---|---|---|---|---|---|---|
| #17 NEN-19 | api | 3 | 3 | BE-1 · senior | 2.5 | duyệt, giữ |
| #267 KAN-05 | api | 1 | 2 | BE-1 · senior | 1.5 | duyệt, giữ |
| #67 LO-02a | db | 3 | 3 | BE-1 · senior | 2.5 | duyệt, giữ |
| #259 NEN-04 | db | 1 | 2 | BE-1 · senior | 1.5 | duyệt, giữ |
| #346 OPS-16 | doc | 3 | 4 | BrSE-1 · senior | 3.0 | duyệt, giữ |
| #323 IKO-18b | doc | 3 | 32 | BrSE-1 · senior | 25.5 | duyệt, giữ |
| #107 AI-10a | fn | 4 | 4 | BE-1 · senior | 3.0 | duyệt, giữ |
| #254 RPT-14b | fn | 3 | 8 | BE-2 · middle | 8.0 | duyệt, giữ |
| #316 IKO-15a | ops | 10 | 24 | BE-2 · middle | 24.0 | duyệt, giữ |
| #293 IKO-02 | ops | 1 | 3 | BE-2 · middle | 3.0 | duyệt, giữ |
| #114 CLR-06 | pm | 3 | 3 | BrSE-1 · senior | 2.5 | duyệt, giữ |
| #210 INC-34 | pm | 6 | 12 | PM-1 · senior | 9.5 | duyệt, giữ |
| #333 OPS-19 | spike | 2 | 2 | BE-1 · senior | 1.5 | duyệt, giữ |
| #21 NEN-14a | spike | 4 | 8 | BE-1 · senior | 6.5 | duyệt, giữ |
| #12 NEN-22 | test | 3 | 3 | QA-1 · middle | 3.0 | duyệt, giữ |
| #18 NEN-18 | test | 5 | 12 | QA-1 · middle | 12.0 | duyệt, giữ |
| #28 AU-03 | ui | 3 | 4 | FE-1 · middle | 4.0 | duyệt, giữ |
| #186 INC-06 | ui | 1 | 3 | FE-1 · middle | 3.0 | duyệt, giữ |
| #215 INC-23 | fn | 15 | 20 | BE-2 · middle | 20.0 | duyệt, giữ |
| #223 INC-33 | pm | 10 | 24 | PM-1 · senior | 19.0 | duyệt, giữ |
| #305 IKO-07b | fn | 15 | 20 | BE-2 · middle | 20.0 | duyệt, giữ |
| #176 REC-14 | fn | 9 | 14 | BE-2 · middle | 14.0 | duyệt, giữ |

Cần tách trước khi xếp sprint: #320 IKO-17b (32 giờ), #323 IKO-18b (32 giờ), #104 SE-04a (24 giờ), #223 INC-33 (24 giờ), #253 RPT-14a (24 giờ), #297 IKO-04 (24 giờ), #316 IKO-15a (24 giờ), #317 IKO-15b (24 giờ), #72 LO-11 (22 giờ), #97 SE-02a (20 giờ), #122 MD-08 (20 giờ), #164 REC-05 (20 giờ), #215 INC-23 (20 giờ), #301 IKO-06b (20 giờ), #305 IKO-07b (20 giờ).

## 4. Tổng theo epic

Số issue = issue + sub-issue dưới epic (không tính 3 task đã đóng). SWP, Giờ đọc từ trường trên Project (đã đối chiếu: 0 lệch).

| Epic | Số issue | Tổng SWP | Tổng giờ | Ghi chú |
|---|---|---|---|---|
| #1 Nền tảng dùng chung — vai, RLS, lỗi, môi trường, tệp, worker | 22 | 72 | 112.5 | giờ chuẩn 130 |
| #24 F01 — Participant, tư cách 許可/承認/登録 và xác thực | 28 | 92 | 136.5 | giờ chuẩn 158 |
| #53 F02 — Lô hàng: tiếp nhận, 目利き, cân, điều chỉnh, lô 仮 | 22 | 76 | 118.5 | giờ chuẩn 140; cần tách: LO-11 |
| #76 F03 — Giao dịch 相対 và せり, nhập tại sàn khi đứt mạng | 38 | 156 | 217.5 | giờ chuẩn 247; 2 sub-issue ràng buộc 完納奨励金; cần tách: SE-02a, SE-04a |
| #136 F04 — Giao nhận 荷受け / 引渡し, ngoại lệ, 滞留 | 16 | 54 | 98 | giờ chuẩn 98 |
| #153 F05 — Đối chiếu ngày, lock 業務日, tranh chấp, 訂正 sau lock | 27 | 101 | 163 | giờ chuẩn 163; 13 sub-issue ràng buộc 完納奨励金; cần tách: REC-05 |
| #181 F06 — 完納奨励金 (ràng buộc nghiệp vụ đặc thù Product A) | 44 | 173 | 261.5 | giờ chuẩn 270; 28 sub-issue ràng buộc 完納奨励金; cần tách: INC-23, INC-33 |
| #226 F07 — Báo cáo, xuất 照合 cho kế toán, 取引票, báo cáo tỉnh | 29 | 91 | 177 | giờ chuẩn 178; 2 sub-issue ràng buộc 完納奨励金; cần tách: RPT-14a |
| #256 F08 — Audit trail: ghi, tra cứu, phiên bản quy tắc đã áp | 16 | 43 | 61 | giờ chuẩn 69; 2 sub-issue ràng buộc 完納奨励金 |
| #273 F09 — Thông báo nghiệp vụ và cấu hình người nhận | 15 | 40 | 67.5 | giờ chuẩn 76 |
| #354 F10 — 建値 / 相場 (OPTION) | 3 | 8 | 16 | giờ chuẩn 16; OPTION |
| #289 F11 — Migration 7 năm, rehearsal, cutover, UAT, đào tạo | 35 | 145 | 295 | giờ chuẩn 314; 2 sub-issue ràng buộc 完納奨励金; cần tách: IKO-04, IKO-06b, IKO-07b, IKO-15a, IKO-15b, IKO-17b, IKO-18b |
| #115 F12 — Master data, cấu hình có kiểm soát, lập lịch batch | 20 | 67 | 109.5 | giờ chuẩn 127; cần tách: MD-08 |
| #325 F13 — Vận hành và an ninh: backup, cold archive, alert, dữ liệu cá nhân | 28 | 109 | 178 | giờ chuẩn 184 |
| #358 F14 — 委託販売手数料 và chốt sổ tháng 出荷者 (OPTION) | 9 | 34 | 58 | giờ chuẩn 58; OPTION |
| #368 F15 — Cổng tra cứu cho 出荷者 (OPTION) | 4 | 12 | 22 | giờ chuẩn 22; OPTION |
| #373 Quyết định còn mở từ LAB-4 — 9 rủi ro High (AI-report LAB-4 §6) | 6 | 16 | 14.5 | giờ chuẩn 18 |
| **Tổng** | 362 | **1289** | **2106** | giờ chuẩn 2268 |

## 5. So với estimate LAB-1

LAB-1: man-day AIDD của gói hiện thực 3.x (đều là công FE + BE + Infra — QA ở pha 4 riêng), cộng 2.11 (API audit) và 5.1–5.3, 5.5 (migration, đào tạo). LAB-5: tổng giờ trên Project ÷ 8.

| Phần (epic) | LAB-1 (MD) | LAB-5 (MD) | Chênh | Vì sao |
|---|---|---|---|---|
| #1 Nền tảng dùng chung (WBS 3.12) | 17.8 | 14.1 | -3.7 | LAB-1 3.12 gồm cả monitoring / DR (LAB-5 đưa sang E13); LAB-5 thêm worker sinh tệp (PREMISE P-10), vai / RLS / lỗi dùng chung |
| #24 F01 (WBS 3.1) | 28 | 17.1 | -10.9 | Form đăng nhập, route 参加者 đã dựng ở LAB-3 — chỉ tính phần delta; thiết kế MFA nằm ở LAB-1 2.7 (pha thiết kế) |
| #53 F02 (WBS 3.2) | 40.7 | 14.8 | -25.9 | Tiếp nhận lô, danh sách, 目利き đã dựng ở LAB-3; 5 function lot dựng theo API không cần TECH (PREMISE A-02); P-12 chỉ thêm một tham số |
| #76 F03 (WBS 3.3, 3.3b, 3.11) | 67.9 | 27.2 | -40.7 | Lõi 相対 / せり đã dựng ở LAB-3; hàng đợi offline viết một lần dùng chung (SE-04a) dù PREMISE P-01 mở rộng cho cả 相対 + 調停 |
| #136 F04 (WBS 3.4) | 33.1 | 12.2 | -20.9 | LAB-1 ước từ trên xuống trước khi có thiết kế; LAB-4 đã có TECH đủ cho 6 function; offline dùng lại module chung của E03 |
| #153 F05 (WBS 3.5) | 34 | 20.4 | -13.6 | PREMISE P-06, P-08 thêm điều kiện chặn lock và 差額 trong 承認 訂正 — vẫn thấp hơn vì TECH LAB-4 chi tiết tới từng lỗi |
| #181 F06 (WBS 3.6) | 38.3 | 32.7 | -5.6 | Gần LAB-1 nhất: mục ràng buộc ước đủ giờ (INC-23 20 giờ, INC-33 24 giờ thô); PREMISE P-05 thêm quản trị 業務規程, P-07 thêm 却下; D1b (P-04) bỏ nhánh tiền 免除 nhưng giữ issue bị chặn INC-34 |
| #226 F07 (WBS 3.7) | 20.8 | 22.1 | +1.3 | Thêm worker sinh CSV / PDF (P-10) và engine điền mẫu báo cáo tỉnh (RPT-14a 24 giờ, chưa có mẫu) |
| #256 F08 (WBS 3.8, 2.11) | 14.7 | 7.6 | -7.1 | `ghi_kansa` và tra audit cơ bản đã dựng ở LAB-3 — chỉ còn cột mới, trigger, màn tra đủ tiêu chí |
| #273 F09 (WBS 3.9) | 11.7 | 8.4 | -3.3 | Thêm người nhận alert hệ thống (P-09); gửi email nằm trong worker chung |
| #289 F11 (WBS 5.1, 5.2, 5.3, 5.5) | 55.4 | 36.9 | -18.5 | LAB-1 tính cả giờ BrSE / PM và buổi đào tạo; LAB-5 chỉ tính công cụ migration, rehearsal, tài liệu — UAT / hypercare chạy theo lịch, không là issue |
| #115 F12 (WBS 3.0) | 17.5 | 13.7 | -3.8 | Tương đương; LAB-1 có phần Infra 3,2 MD cho lập lịch batch nằm ở E00 / E12 |
| #325 F13 (WBS 3.16) | 18.7 | 22.2 | +3.6 | LAB-5 gom runbook (OPS-17) và diễn tập (OPS-20) vốn ở LAB-1 5.8 / 5.9 vào epic này |
| #358 F14 (WBS 3.15) | 8.4 | 7.2 | -1.2 | OPTION; tương đương |
| #354 F10 | — | 2.0 | — | OPTION — LAB-1 báo giá riêng, không có dòng WBS |
| #368 F15 | — | 2.8 | — | OPTION — LAB-1 báo giá riêng, không có dòng WBS |
| #373 Quyết định còn mở từ LAB-4 | — | 1.8 | — | Việc quyết định / làm rõ của PM, BrSE — LAB-1 nằm trong 6.3 quản trị thay đổi phạm vi |
| **Tổng** | **407** | **263.2** | **-143.7** |  |

**Vì sao LAB-5 thấp hơn nhìn chung:** (1) giờ LAB-5 là công làm từng task của đúng người được giao, chưa có review code, sửa sau test hệ thống, họp — LAB-1 gói các phần này vào MD từng chức năng (buffer 15 % để riêng); (2) LAB-3 đã dựng một phần F01–F03, F06, F08 nên chỉ tính delta; (3) LAB-4 đã có TECH tới từng mã lỗi nên ít bất định hơn ước lúc chào giá. Vì (1), con số LAB-5 **không** dùng thẳng làm cam kết lịch: báo cáo tiến độ theo dõi tỷ lệ giờ thật ÷ giờ ước sau Sprint 1.
