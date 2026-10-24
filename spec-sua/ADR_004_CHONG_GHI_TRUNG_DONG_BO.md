---
id: SPEC-ADR_004-001
title: ADR-004 — Chống ghi trùng và đồng bộ lại dùng chung cho 相対取引, せり và giao nhận
type: system
subtype: adr
feature: _common
status: draft
version: "0.1.0"
updated_by: tran.van.luan
updated: 2026-10-24
source: bản sửa thiết kế LAB-5 (PREMISE.md P-01, P-02, P-08; quyết định D2, D3)
related: [SPEC-ADR_001-001, SPEC-ADR_003-001, SPEC-ARCH_SYSTEM-001]
---

# ADR-004 — Chống ghi trùng và đồng bộ lại dùng chung cho 相対取引, せり và giao nhận

| | |
|---|---|
| Trạng thái | **Accepted** cho backlog LAB-5 (quyết định D3, 2026-10-24) — chưa dựng |
| Ngày · Người viết | 2026-10-24 · Trần Văn Luận |
| Thay đổi so với LAB-4 | LAB-4 chỉ thiết kế cho せり (SERI_KIROKU_TECH §4.6–4.8, SERI_CHOTEI_TECH). 相対 (SCR020, AITAI_KAKUTEI_API / _TECH) và giao nhận đẩy phần này sang "tài liệu F03-07" vốn loại trừ chúng |
| Liên quan | ADR-001 (luật ở DB function), ADR-003 (hai bảng 相対 / せり); bảng `doki_kyogo` (COMMON_DB §4.4); PREMISE P-01, P-02, P-08 |

## Bối cảnh

1. Thiết bị tại sàn (tablet) có lúc mất kết nối ngắn. Bộ đề xuất LAB-1 (A-06) đã hứa với khách: hàng đợi cục bộ + giải quyết xung đột khi đồng bộ lại. A-26 đặt mức tạm: ≤ 200 bản ghi / thiết bị, giữ ≤ 5 phút (chờ Q-05, Q-18).
2. **相対 là luồng mang phần lớn giá trị giao dịch** nhưng API 確定 tự ghi "không idempotent": gửi lại sau khi mất phản hồi có thể tạo **hai** giao dịch, trừ số lượng lô hai lần, và cộng 期間購入額 hai lần (đầu vào 完納奨励金).
3. Bảng `aitai_torihiki`, `seri_kekka`, `hikiwatashi` **đã có** cột `tanmatsu_sosa_id` UNIQUE trong thiết kế dữ liệu LAB-4; `doki_kyogo.taisho_shubetsu` đã có giá trị `aitai` / `seri` / `hikiwatashi` / `lot`. Chỉ có せり có function và màn dùng chúng.
4. 調停 của せり hiện INSERT thẳng vào `seri_kekka`, 自己訂正 UPDATE thẳng `suryo` — trái ghi chú "chỉ ghi qua function có kiểm" (PREMISE P-02).
5. Phiếu nhận xét LAB-4: đây là chỗ chặn việc duy nhất của bộ spec; người chấm khuyên viết ADR riêng vì ảnh hưởng cả せり lẫn 相対.

## Lựa chọn

Một **hợp đồng ghi chịu được gửi lại** dùng chung cho mọi luồng nhập tại sàn (相対, せり, giao nhận; lô đã có ở LOT-002):

| # | Câu hỏi | Quyết định |
|---|---|---|
| 1 | Request có mang mã thao tác của thiết bị không? | **Có.** Client sinh `tanmatsu_sosa_id` (UUID v4) **một lần khi người dùng bấm xác nhận** và giữ nguyên qua mọi lần gửi lại; kèm `tanmatsu_kiroku_at` (giờ thiết bị lúc bấm). Thiếu hai trường = nhập trực tiếp như LAB-3 (không chống trùng) — chỉ cho phép trong giai đoạn chuyển đổi, test bắt buộc gửi có mã |
| 2 | Trùng thì trả gì? | Mã đã có ở **bảng nghiệp vụ** → `200 {ok:true, id}` của bản ghi cũ, **không** ghi, **không** trừ số lượng. Mã đã có ở **`doki_kyogo`** → `409 {code = riyu_code đã lưu, detail.doki_kyogo_id}`. Hai lần gửi đồng thời cùng qua bước tra → UNIQUE chặn lần chậm, function bắt `unique_violation` và trả như một lần gửi lại |
| 3 | Có vào hàng xung đột không? | Chỉ ở **chế độ đồng bộ** (`p_doki = true` — bản ghi lấy ra từ hàng đợi): bị từ chối nghiệp vụ (lô hết số lượng, tư cách bên mua, 業務日 đã lock, …) → ghi `doki_kyogo` cùng giao dịch với `kansa_log kyohi`, trả 409, client **xoá** khỏi hàng đợi. Nhập trực tiếp (online) bị từ chối thì báo lỗi trên màn như hiện tại, không vào hàng xung đột. `ROLE_KENGEN_NASHI` không bao giờ vào hàng xung đột |
| 4 | Ai xử lý? | **照合事務** (`shogo_jimu`) 調停 ở SCR024 khối E (せり) và khối 調停 tương ứng ở SCR020 (相対): **採用** = gọi lại **lõi kiểm ghi chung** (khoá lô, tư cách bên mua tại 業務日 của bản ghi, khoá 業務日, số lượng) rồi ghi bản ghi với `tanmatsu_sosa_id` gốc; **棄却** = bắt buộc ghi 責任者 (`sekinin_sha`). Giả định, chờ Q-mới "ai 調停" |
| 5 | 業務日 của bản ghi gửi muộn? | `LEAST(tanmatsu_kiroku_at, now())` quy về ngày JST — ngày thực tế ghi ở sàn, không phải ngày đồng bộ. Quyết định giao dịch rơi vào kỳ 完納奨励金 nào |
| 6 | Bản ghi tới khi 業務日 đó **đã lock**? | Bị từ chối `GYOMU_BI_ROKKU_ZUMI` → vào `doki_kyogo` ở `mitaio`. 採用 sau lock **bị chặn** (lock không mở lại — R7). Lối ra cho ca này là task quyết định H-07 (chờ khách). Để giảm ca này: lock bị **chặn** khi ngày đó còn `doki_kyogo` chưa 調停 (PREMISE P-08) |
| 7 | Hàng đợi ở client | **Một module dùng chung** (IndexedDB) cho SCR020, SCR024, SCR031: gửi theo thứ tự `tanmatsu_kiroku_at`, tự gửi lại khi có mạng, hiện số bản ghi chờ; đầy 200 bản ghi hoặc quá 5 phút thì chặn nhập tiếp và báo người dùng |

**Lõi kiểm ghi chung** (PREMISE P-02): một DB function nội bộ (không grant cho `authenticated`) chứa bước khoá lô + kiểm tư cách + kiểm khoá 業務日 + kiểm / trừ số lượng. `kakutei_aitai_torihiki`, `kiroku_seri_kekka`, `chotei_*_kyogo` và `jiko_teisei_kiroku` đều gọi lõi này; không function nào INSERT / UPDATE bảng giao dịch mà bỏ qua nó.

## Phương án bỏ và vì sao

| # | Phương án | Được gì (thật) | Vì sao bỏ |
|---|---|---|---|
| A | 相対 chỉ "giữ form + gửi lại" có `tanmatsu_sosa_id`, không hàng đợi, không 調停 | Rẻ nhất (khoảng 1 sub-issue); hết ghi đôi khi gửi lại | Mất mạng lâu hơn một lần bấm thì người dùng phải chờ; lệch lời hứa A-06 với khách; luồng chính lại khác せり → hai cách dùng ở cùng một sàn |
| B | Chống trùng bằng khoá tự nhiên (lô + bên mua + số lượng + đơn giá + phút) | Không cần client sinh mã | Hai giao dịch thật có thể trùng hết các trường đó (cùng người mua hai lần một lô); dễ chặn nhầm giao dịch thật — sai về tiền |
| C | Local-first đầy đủ (ghi cục bộ là nguồn chính, đồng bộ hai chiều) | Chịu được mất mạng dài | Ngoài phạm vi báo giá (LAB-1 R-03: +25–40 MD, đổi kiến trúc); PREMISE LAB-4 H4 đã loại |

## Hệ quả — kể cả hệ quả xấu

Tốt: một hợp đồng cho mọi luồng tại sàn; gửi lại không bao giờ ghi đôi; bản ghi bị từ chối lúc đồng bộ không mất mà vào hàng có người chịu trách nhiệm; 期間購入額 không bị cộng hai lần.

| # | Hệ quả xấu | Ai chịu · Khi nào lộ ra | Cách phát hiện |
|---|---|---|---|
| 1 | **Client sinh mã sai cách** (sinh lại mỗi lần gửi) làm mất tác dụng chống trùng mà không lỗi nào báo | 照合事務 · lúc đối chiếu ngày thấy hai giao dịch giống hệt | Test E2E: ngắt mạng sau khi server ghi, gửi lại → đếm đúng 1 dòng; truy vấn hằng ngày: cặp giao dịch cùng lô / bên mua / số lượng trong 60 giây |
| 2 | **Hàng xung đột tồn đọng** chặn lock ngày (P-08) → 締め trễ, kéo trễ báo cáo 11:00 | 照合事務, 財務部門 · ngày có nhiều thiết bị mất mạng | SCR041 hiện số `doki_kyogo` `mitaio` của ngày; alert khi còn bản chờ lúc 09:30 |
| 3 | **Giờ thiết bị sai** đưa bản ghi vào 業務日 / kỳ thưởng khác | Người mua (thưởng) · khi bản ghi sát nửa đêm hoặc sát ranh giới tháng | Lưu cả `tanmatsu_kiroku_at` và `kakunin_at`; báo cáo chênh > 10 phút giữa hai cột |
| 4 | **Lõi kiểm chung thành điểm nghẽn**: đổi một luật (vd giao nhiều lần) ảnh hưởng mọi luồng cùng lúc | Dev · mỗi lần đổi luật | Bộ test hồi quy chạy cả 4 đường gọi lõi cho mỗi thay đổi |
| 5 | **Mất dữ liệu nếu xoá trình duyệt / đổi thiết bị** khi hàng đợi còn bản ghi | Người nhập · hiếm | Màn hiện số bản ghi chờ; chặn đăng xuất khi hàng đợi chưa rỗng |

## Cách kiểm

- Test SQL: cùng `tanmatsu_sosa_id` gọi 2 lần → 1 dòng, số lượng lô trừ 1 lần; gọi đồng thời 2 phiên → 1 dòng + 1 lần trả như gửi lại; chế độ đồng bộ + lô hết hàng → 1 dòng `doki_kyogo` + 1 dòng `kansa_log kyohi`.
- Test 調停: 採用 chạy lại kiểm tư cách và khoá 業務日; 棄却 thiếu 責任者 → từ chối.
- Test E2E (Playwright): ngắt mạng ở SCR020 / SCR024 / SCR031, nhập 3 bản ghi, bật mạng → 3 dòng, không trùng.
- Sub-issue thực hiện: SE-11 (tài liệu này), SE-09, SE-04a, SE-04b, SE-05, AI-10a, AI-10b, AI-10c, DEL-09b, SE-10.

## Chỗ không chắc

- Q-05 / Q-18: thời lượng mất mạng phải chịu, số bản ghi tối đa, bên nào thắng khi xung đột — mức 5 phút / 200 bản ghi là tạm.
- Ai 調停 (照合事務 hay 取引運用 / ban quản lý chợ) — giả định 照合事務.
- Lối ra cho bản ghi tới sau khi ngày đã lock — task quyết định H-07.
- Nếu khách xác nhận cần trạng thái 下書き / 確認待ち (FIG-012 — CLR-06) thì bản ghi trong hàng đợi có thể cần lưu ở server như bản nháp thay vì chỉ ở thiết bị.
