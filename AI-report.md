# AI-report — LAB-5 · Product A (桜湾水産市場)

Công cụ: Claude Code (Opus 5.5) trong desktop app, bộ kit Takumi (core v6.2.0, extras v1.1.0), `gh` CLI + GitHub GraphQL, Claude in Chrome (cấu hình view Project). Một người học điều phối; AI làm, người quyết và duyệt mẫu. Mọi bước được ghi vào nhật ký lúc xảy ra (worklog trong không gian làm việc riêng); phần dưới đây tự đứng được, không cần mở nhật ký.

## 1. Skill và prompt cho từng việc

| Việc | Skill / công cụ | Ai làm |
|---|---|---|
| Khảo sát spec LAB-4 để break | 3 agent con (general-purpose) chạy song song, mỗi agent 5–6 feature, context sạch | AI; người điều phối kiểm lại 6 phát hiện nặng trong spec gốc |
| Break task, PREMISE, DoD | Agent chính viết cây backlog dạng YAML + script kiểm (khoá trùng, dep trỏ đúng, vòng phụ thuộc) + script đẩy lên GitHub có dry-run | AI; người chốt 13 quyết định |
| Chấm SWP | **`tkm:sun-estimate`** (kèm `tkm:confidence`) — nguồn là chính Project | AI chấm; người duyệt 5 issue mẫu |
| Ghi SWP lên trường point | Agent con (general-purpose) với ràng buộc một mutation | AI; người điều phối kiểm độc lập |
| Quy giờ | Agent chính ước giờ chuẩn từ nội dung từng issue | AI đề xuất; người đổi phương pháp, duyệt 22 mẫu |
| Xếp sprint, giả lập, báo cáo tiến độ | Script tất định đọc Project; skill `dataviz` cho burndown | AI; người chọn lịch và năng lực |

**Prompt người học giao (nguyên văn)** — viết ngắn, nêu đầu ra và ai duyệt:

> Lập plan LAB-5: soát specs LAB-4 + review LAB-4, nháp PREMISE và cây epic → issue → sub-issue trước; tôi sẽ chạy gh auth refresh -s project song song.

> Tôi không muốn quy ước SWP ra giờ vì Scope × Complexity → SWP. Còn giờ đại diện cho effort cần hoàn thành issue- phụ thuộc vào level của members

> Làm bước ước giờ theo D9b/D12: AI đề xuất giờ chuẩn middle từ nội dung từng mục kèm lý do, tôi duyệt mẫu, rồi nhân hệ số trình độ và ghi vào trường Giờ.

> Xếp issue vào Iteration theo phụ thuộc và năng lực đội, tách các mục >16 giờ nếu rơi vào Sprint 1, rồi giả lập Sprint 1 (9/11–20/11) như sau sprint thật.

> Viết progress-report.md từ chính Project: trạng thái, burndown (ảnh + bảng) kèm giải thích lệch, rủi ro trễ có hành động, người làm, hạn.

**Prompt AI chính giao agent con (trích nguyên văn phần lõi):**

Khảo sát nhóm B — feature chứa ràng buộc đặc thù:

> Your features: incentive (F06 完納奨励金 — the product's special domain constraint, survey it most carefully), reconciliation-closing, delivery, consignment-fee, reference-price, shipper-portal […]
> 6. 完納奨励金 constraint map: list every rule of the incentive […] with its source file:line, and every work unit in ANY of your features that is affected by those rules […]. LAB-5 must put these in separate issues, so be explicit about which pieces are constraint-driven vs generic CRUD.
> Only report what you verified in the text; mark confidence.

**Chấm SWP.** Gọi `tkm:sun-estimate https://github.com/users/luantv-2208/projects/1`. Skill đọc Project chỉ qua lệnh GET / query (mỗi lệnh qua bộ kiểm `tool_policy.py`), chạy bộ chọn tất định (286 việc chấm + 93 container, loại 3 task đã đóng), chấm Scope × Complexity, chọn 5 mẫu, hỏi người hai vòng mỗi mẫu (mức khó → điểm), rồi render một HTML. Skill **không ghi** lên Project.

**Ghi SWP lên Project bằng Agent.** Tách thành bước riêng sau khi người duyệt mẫu:
1. AI chính dựng tệp ánh xạ `issue → SWP` thẳng từ payload đã kiểm của skill (việc chấm trực tiếp = điểm; issue cha, epic = tổng con skill đã cộng), kiểm tổng 286 việc = tổng 17 epic = 1.289.
2. Giao agent con, trích phần lõi:
   > Task: write already-approved Standard Work Point (SWP) numbers into the "SWP" number field of a GitHub Project (v2), then read every value back and report. […] The numbers are final: they come from a validated sun-estimate payload reviewed by a human. Do NOT recompute, round, or change any value. […]
   > Hard constraints: The only mutation allowed is updateProjectV2ItemFieldValue on fieldId = swpFieldId. Do not touch any other field, item, issue, label, comment, view, or the repository. […]
3. Agent tự viết script có tệp tiến độ (chạy lại không ghi trùng), ghi 379 giá trị, đọc lại 0 lệch; chạy lần hai thử nhánh bỏ qua: 0 ghi.
4. AI chính **không tin báo cáo agent**: đọc lại bằng đường khác (`gh project item-list`), so với payload gốc (không so với tệp ánh xạ) → 0 lệch; soi script agent: đúng một mutation.

**Quy giờ.** AI ước giờ cho một người mức middle từ nội dung từng sub-issue (phần đã dựng ở LAB-3, số bảng / màn / route / kịch bản test), một dòng lý do mỗi mục; giờ trên Project = giờ chuẩn × hệ số trình độ người phụ trách (senior 0,8 · middle 1,0). Phương pháp do người đổi (xem mục 2).

**Báo cáo tiến độ.** Xếp sprint bằng thuật toán tham lam theo phụ thuộc + năng lực từng người; giả lập Sprint 1 bằng mô hình tất định (5,6 giờ / người / ngày, giờ thật = giờ ước × hệ số lệch có lý do), không bịa câu trả lời của khách; báo cáo đọc thẳng các trường của Project.

## 2. Review output của AI

**Người review ở đâu — các quyết định người chốt** (đưa vào đây theo góp ý LAB-4 "người đọc chỉ cầm AI-report có hiểu bạn đã quyết gì không"):

| # | Người quyết | Phương án AI đưa | Người chọn |
|---|---|---|---|
| D1 → D1b | 免除 quyết sau khi kỳ đã 確定 | AI khuyên thêm nguồn 差額 thứ 3 (D1) | Người chọn D1, rồi **đổi sang D1b** sau phiếu nhận xét LAB-4: 免除 chỉ ghi nhận, không đổi số tiền tới khi khách trả lời Q-30 — RFP không có quy tắc tiền này |
| D2 | Lock ngày khi còn việc dở | chặn lock / cho ghi dưới ngày hôm nay / để mở | Chặn lock + danh sách việc dở |
| D3 | Chống ghi trùng ở 相対 | AI khuyên "giữ form + gửi lại" (rẻ) | Người chọn **hàng đợi + 調停 như せり** — giữ lời hứa với khách ở bản đề xuất LAB-1 (đắt hơn khuyến nghị) |
| D4 | Ai sở hữu bảng 業務規程 | F06 / chỉ seed / F12 | F06, thêm tab ở SCR051 |
| D5–D8 | Độ hạt, người phụ trách, nguồn review, mã học viên | — | Giữ 273 sub-issue; trường vai giả định; phiếu nhận xét người chấm |
| D9 → D9b | Quy giờ | AI khuyên quy ước SWP → giờ theo loại việc | Người **bác**: SWP đo issue, giờ đo công của người → ước thẳng × hệ số trình độ |
| D10–D13 | Lịch sprint, trình độ đội, ai ước giờ, năng lực | — | 2 tuần từ kickoff; đội trộn (người sửa QA-1 junior → middle); AI đề xuất, người duyệt mẫu |
| Mẫu SWP | 5 issue skill chọn | — | Giữ cả 5: khớp cảm nhận khi tự dựng LAB-3, thứ tự công sức đúng |
| Mẫu giờ | 22 issue rút tất định | AI tự khai lệch trần 16 giờ | Duyệt cả 22; bỏ trần — "ước thật, đánh dấu cần tách" |

**Vấn đề phát hiện trong output AI và cách xử lý:**

| # | AI sai gì | Phát hiện bằng cách nào | Xử lý |
|---|---|---|---|
| 1 | Khuyên D1 (免除 trả tiền qua 差額) — bám thiết kế LAB-4 mà không đặt cạnh RFP | Phiếu nhận xét người chấm trục B; AI kiểm lại tài liệu khách: RFP không có 免除, tài liệu nghiệp vụ có "xét miễn" nhưng không nói tác động tiền | D1b; PREMISE P-04 ghi cả hai nguồn |
| 2 | Ghi enum `nyuryoku_hoho = shudo` theo trí nhớ | Tra DB doc trước khi dùng | Sửa `te_nyuryoku` |
| 3 | Chấm discipline `Unknown` cho 17 task BrSE / PM mà để confidence medium | **Bộ kiểm hợp đồng của skill** từ chối payload | Task làm rõ nhận discipline của tầng bị ảnh hưởng; task tài liệu giữ Unknown + low |
| 4 | Ước giờ ép đúng trần 16 giờ cho 16 mục khó (~1,07 giờ / SWP) | AI tự đo tỷ lệ giờ ÷ SWP và báo người | Người chọn bỏ trần; 15 mục > 16 giờ ghi "cần tách" |
| 5 | Lịch sprint đưa sub-issue test vào Sprint 1 trước code nó kiểm | Đọc danh sách Sprint 1 sau lần chạy đầu | Thêm phụ thuộc ngầm "test sau code cùng issue" |
| 6 | Dự báo ngày xong chia tổng giờ cho tốc độ cả đội → sớm hơn kế hoạch, bỏ qua nút thắt BE-2 | Kết quả trái với chính lịch vừa xếp | Dự báo theo từng dev → BE-2, Sprint 14 |
| 7 | Lời giải thích burndown "ba ngày đầu chậm" không khớp hình | Render SVG ra ảnh để nhìn | Viết lại theo bảng số từng ngày |
| 8 | Đổi mã giả định trong thân issue nhưng quên tiêu đề → 3 issue phụ thuộc còn trích mã cũ | Quét lại cả 382 issue sau khi sửa | Sửa tiêu đề + sinh lại thân |
| 9 | Bộ quét an toàn báo 138 "token" — mẫu `sk-` khớp chữ "task-breakdown" | Đọc từng cảnh báo trước khi tin | Thêm ranh giới từ; thử đột biến (email, token, đoạn JP dài) 3/3 |
| 10 | Script đẩy Project dừng ở issue thứ 21: "Content already exists" | Lỗi chạy thật | GitHub tự thêm sub-issue vào Project khi gắn vào cha — tra item có sẵn, chạy lại từ tệp trạng thái, không tạo trùng |
| 11 | Agent khảo sát nói "repo LAB-5 trống nên không đối chiếu code" — nhìn nhầm repo | Đọc phần Concerns trong báo cáo agent | Chấp nhận: cột "đã dựng" dựa spec LAB-4 (đã đối chiếu code 474 / 476 ở LAB-4), ghi rủi ro |

**Kiểm chứng không dựa vào lời AI tự khai:** script kiểm cây được thử bằng lỗi gài (4 / 4, sau đó 2 / 2 cho luật mới); mọi lần ghi lên Project đều đọc lại từ GitHub (SWP, Giờ, Iteration, Status, Giờ thật: 0 lệch); độ phủ 73 / 73 sub-feature LAB-4 kiểm bằng script có ID giả để chắc vòng lặp thật sự chạy; 6 phát hiện nặng của agent khảo sát được mở spec gốc kiểm lại (5 đúng, 1 là đánh đổi có chủ ý).

## 3. Điểm đặc biệt trong cách dùng AI

**Hiệu quả:**
- **File nộp sinh từ dữ liệu, không chép tay.** task-breakdown, estimate, progress-report sinh bằng script đọc cây backlog, payload skill, trường Project, WBS LAB-1 — mọi #số, SWP, giờ khớp nhau; sửa dữ liệu thì chạy lại.
- **Dry-run và tệp trạng thái cho mọi lần ghi GitHub.** 382 issue, quan hệ cha–con, 6 trường, Iteration: chạy lại an toàn khi lỗi giữa chừng.
- **Skill có hợp đồng tự kiểm.** Bộ kiểm của `tkm:sun-estimate` bắt được lỗi chấm của chính AI (mục 2, #3) — người không cần soát 286 dòng để thấy.
- **Tách "AI làm" khỏi "AI kiểm".** Agent ghi SWP bị kiểm bằng đường đọc khác; agent khảo sát bị kiểm bằng spec gốc.

**Chưa tốt, cần cải thiện khi làm thật:**
- **Chấm SWP inline thay vì 5 worker** như skill đề xuất: worker là tuỳ chọn, nhưng làm vậy mất góc nhìn độc lập của worker; lý do là worker không có tool, phải chép ~80 KB bằng chứng vào prompt. Lần sau: cho skill một cách nạp bằng chứng theo tệp.
- **Giờ ước do một AI ước cho 286 mục** — người chỉ duyệt 22 mẫu. LAB-5 thấp hơn LAB-1 144 MD; Sprint 1 giả lập cho thấy giờ thật ÷ giờ ước 1,12 và chỉ xong 80 % cam kết. Làm thật cần hiệu chỉnh bằng giờ thật của đội sau 2–3 sprint.
- **Dữ liệu sprint là giả lập** (tất định, có lý do cho từng lệch) — không thay được sprint thật; câu trả lời khách không được bịa nên các task chờ khách đều treo.
- **Cây backlog thiếu phụ thuộc test → code** phải vá bằng luật ngầm lúc xếp sprint — nên khai ngay trong cây.
- **Token:** đọc 40 nghìn dòng spec bằng 3 agent và 286 bằng chứng cho skill là phần tốn nhất; khảo sát nên chỉ đọc README feature + TECH, để SPEC màn hình cho bước làm thật.

**Chưa có ý tưởng tốt:** cách đưa "phần đã dựng ở prototype" vào bằng chứng của skill chấm SWP — hiện thân issue chỉ có tiêu đề, đường dẫn spec, phụ thuộc; độ lớn phần đã có nằm trong spec LAB-4 (repo riêng tư) mà skill không đọc. Thêm vào thân issue thì bằng chứng phình, không thêm thì người duyệt mẫu phải tự nhớ.
