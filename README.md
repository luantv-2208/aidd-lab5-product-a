# 桜湾水産市場 — backlog LAB-5

**Mã học viên:** Trần Văn Luận · **Đề:** Product A (hệ thống vận hành chợ bán buôn thủy sản Vịnh Sakura)

## Tổng quan

- Backlog break từ bộ detail design **LAB-4 nộp ngày 2026-10-10** (thư mục `specs/` theo chuẩn công ty, 17 feature, 73 sub-feature).
- **PREMISE.md có thay đổi** so với LAB-4: 12 sửa thiết kế (theo phiếu nhận xét LAB-4 và chỗ hở phát hiện khi break), 9 giả định, 7 hướng chốt.
- **17 epic** — 15 nhóm chức năng (3 nhóm OPTION), E00 nền tảng dùng chung, E98 quyết định còn mở từ LAB-4 · 92 issue · 273 sub-issue; SWP chấm bằng skill và đã duyệt, giờ ước theo trình độ người phụ trách.
- **Sprint giả lập đã chạy: Sprint 1 (2026-11-09 → 2026-11-20)** — trạng thái trên Project cập nhật như sau một sprint thật; xem `progress-report.md`.

## Link

| | |
|---|---|
| Repo | https://github.com/luantv-2208/aidd-lab5-product-a |
| GitHub Project | https://github.com/users/luantv-2208/projects/1 — view 1 bảng đủ trường, view 2 board theo Status, view 3 nhóm theo Iteration |
| Báo cáo HTML chấm SWP | [`reports/sun-estimate-luantv-2208-project-1-20261024-1420.html`](reports/sun-estimate-luantv-2208-project-1-20261024-1420.html) |

Đã mở thử repo, Project (cả 3 view), một issue epic và các file Markdown bằng **trình duyệt không đăng nhập** — đều xem được.

## Cấu trúc repo

| File / thư mục | Nội dung |
|---|---|
| `.gitignore` | Bỏ tệp hệ điều hành |
| `AI-report.md` | Skill và prompt, cách review output AI, điểm đặc biệt |
| `PREMISE.md` | Backlog khác bộ LAB-4 đã nộp ở đâu: 12 thay đổi thiết kế, 9 giả định, 7 hướng chốt — mỗi dòng có nguồn và #issue |
| `README.md` | Tệp này — tổng quan, link, cấu trúc, Spec → epic |
| `bang-chung/` | Tóm tắt review LAB-4 (phiếu người chấm + tự review) — nguồn của các mã FB-x, H-x trong PREMISE |
| `bao-cao-tien-do/` | Ảnh burndown Sprint 1 (SVG) dùng trong progress-report.md |
| `estimate.md` | Báo cáo SWP, 5 issue mẫu đã duyệt, cách quy giờ, tổng theo epic, so với estimate LAB-1 |
| `progress-report.md` | Báo cáo tiến độ Sprint 1 (giả lập): trạng thái, burndown, rủi ro trễ |
| `reports/` | Báo cáo HTML của skill chấm SWP `tkm:sun-estimate` (một tệp) |
| `spec-sua/` | Bản sửa thiết kế được PREMISE trỏ tới: ADR-004 chống ghi trùng và đồng bộ lại (PREMISE P-01) |
| `task-breakdown.md` | Nguyên tắc chia task, Spec → issue, hạng mục ràng buộc 完納奨励金, chỗ hở spec LAB-4 và cách xử lý |

## Spec → epic

| Spec (LAB-4 / dòng PREMISE) | Epic trên Project | Ghi chú |
|---|---|---|
| `_common/` (ARCH_SYSTEM, ADR-001, api-conventions, error-codes, log-convention), COMMON_DB; PREMISE P-10 | #1 — Nền tảng dùng chung — vai, RLS, lỗi, môi trường, tệp, worker | — |
| `authentication/`, `participant/`, `_common/KAITE_SHIKAKU_TECH` | #24 — F01 — Participant, tư cách 許可/承認/登録 và xác thực | — |
| `lot/`, LOT_DB; PREMISE P-12 | #53 — F02 — Lô hàng: tiếp nhận, 目利き, cân, điều chỉnh, lô 仮 | — |
| `aitai-trading/`, `seri-auction/`, TORIHIKI_DB, ADR-003; PREMISE P-01, P-02; ADR-004 | #76 — F03 — Giao dịch 相対 và せり, nhập tại sàn khi đứt mạng | **ràng buộc 完納奨励金**: 3 issue / sub-issue nhãn `rang-buoc:kannou-shoreikin` |
| `master-data/`, MASTER_DB | #115 — F12 — Master data, cấu hình có kiểm soát, lập lịch batch | — |
| `delivery/`, HIKIWATASHI_DB | #136 — F04 — Giao nhận 荷受け / 引渡し, ngoại lệ, 滞留 | — |
| `reconciliation-closing/`, SHIME_DB; PREMISE P-06, P-08 | #153 — F05 — Đối chiếu ngày, lock 業務日, tranh chấp, 訂正 sau lock | **ràng buộc 完納奨励金**: 16 issue / sub-issue nhãn `rang-buoc:kannou-shoreikin` |
| `incentive/`, SHOREIKIN_DB, ADR-002; PREMISE P-03, P-04, P-05, P-07, P-11 | #181 — F06 — 完納奨励金 (ràng buộc nghiệp vụ đặc thù Product A) | **ràng buộc 完納奨励金**: 39 issue / sub-issue nhãn `rang-buoc:kannou-shoreikin`; epic của ràng buộc đặc thù Product A |
| `reporting/`, REPORT_DB | #226 — F07 — Báo cáo, xuất 照合 cho kế toán, 取引票, báo cáo tỉnh | **ràng buộc 完納奨励金**: 3 issue / sub-issue nhãn `rang-buoc:kannou-shoreikin` |
| `audit-log/`, COMMON_DB `kansa_log` | #256 — F08 — Audit trail: ghi, tra cứu, phiên bản quy tắc đã áp | **ràng buộc 完納奨励金**: 3 issue / sub-issue nhãn `rang-buoc:kannou-shoreikin` |
| `notification/`, TSUCHI_DB; PREMISE P-09 | #273 — F09 — Thông báo nghiệp vụ và cấu hình người nhận | — |
| `migration/`, MIGRATION_DB | #289 — F11 — Migration 7 năm, rehearsal, cutover, UAT, đào tạo | **ràng buộc 完納奨励金**: 4 issue / sub-issue nhãn `rang-buoc:kannou-shoreikin` |
| `operations-security/`, OPS_DB | #325 — F13 — Vận hành và an ninh: backup, cold archive, alert, dữ liệu cá nhân | — |
| `reference-price/` (OPTION) | #354 — F10 — 建値 / 相場 (OPTION) | OPTION — không xếp sprint |
| `consignment-fee/`, TESURYO_DB (OPTION) | #358 — F14 — 委託販売手数料 và chốt sổ tháng 出荷者 (OPTION) | OPTION — không xếp sprint |
| `shipper-portal/` (OPTION) | #368 — F15 — Cổng tra cứu cho 出荷者 (OPTION) | OPTION — không xếp sprint |
| AI-report LAB-4 §6 (9 rủi ro High còn mở); phiếu nhận xét LAB-4 | #373 — Quyết định còn mở từ LAB-4 — 9 rủi ro High (AI-report LAB-4 §6) | **ràng buộc 完納奨励金**: 1 issue / sub-issue nhãn `rang-buoc:kannou-shoreikin` |
