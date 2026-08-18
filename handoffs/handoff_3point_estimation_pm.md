# Handoff: 3-Point Estimation Skill cho Project Manager

## Bối cảnh
Người dùng đang khám phá ý tưởng phát triển **agent skill** để cải thiện chất lượng công việc PM trong môi trường phần mềm/IT. Đây là 1 trong 2 nhánh khám phá song song (nhánh kia là Visual Communication cho BA).

## Mục tiêu cuối cùng
Tạo ra một **SKILL.md** có thể tái sử dụng trong Gemini/Claude, giúp PM ước lượng effort/timeline chính xác hơn bằng kỹ thuật 3-Point Estimation (PERT).

## Phạm vi khám phá — 3-Point Estimation cho PM

### Vấn đề cần giải quyết
PM thường gặp khó khăn khi:
- Ước lượng effort cho task/epic dựa hoàn toàn vào gut feeling, dẫn đến sai lệch lớn
- Không có framework chuẩn để thu thập và tổng hợp estimate từ team
- Thiếu cách tính toán risk buffer và confidence interval khoa học
- Không thể giải thích cho stakeholder tại sao timeline lại là X ngày (thiếu data backing)
- Khó phát hiện estimation bias (lạc quan quá mức hoặc bi quan quá mức)

### Kiến thức nền — 3-Point Estimation (PERT)
- **Optimistic (O)**: Best-case scenario, mọi thứ suôn sẻ
- **Most Likely (M)**: Kịch bản thực tế nhất dựa trên kinh nghiệm
- **Pessimistic (P)**: Worst-case scenario, gặp nhiều trở ngại
- **PERT Estimate**: `E = (O + 4M + P) / 6`
- **Standard Deviation**: `σ = (P - O) / 6`
- **Variance**: `σ²`
- Có thể tính **confidence interval** (68%, 95%, 99.7%) dựa trên normal distribution

### Các hướng có thể khám phá
1. **Task-Level Estimation** — Nhập task description, agent hỏi 3 điểm (O, M, P), tính PERT + confidence
2. **Epic/Sprint Estimation** — Tổng hợp estimate nhiều task, tính tổng PERT cho cả epic
3. **Historical Calibration** — So sánh estimate vs actual từ sprint trước để calibrate bias
4. **Team Estimation Facilitator** — Thu thập estimate từ nhiều người, phát hiện disagreement, tổng hợp
5. **Risk-Adjusted Timeline** — Từ PERT estimate, tạo timeline với buffer dựa trên confidence level mong muốn
6. **Estimation Report Generator** — Tạo báo cáo trực quan cho stakeholder (table + chart)

### Câu hỏi cần trả lời trong session này
- Scope: Chỉ tính toán PERT thuần hay kèm thêm các kỹ thuật phụ trợ (planning poker, Wideband Delphi)?
- Input: Nhận task description text hay structured data (JSON/CSV từ Jira/Azure DevOps)?
- Output: Chỉ số liệu + bảng hay kèm visual (bar chart, Gantt-like)?
- Interactive level: Hỏi từng task hay nhận batch input?
- Historical data: Có lưu trữ estimate history để calibrate không?
- Scope creep detection: Có flag khi estimate vượt threshold không?

### Suggested Skills to Reference
- `bmad-agent-pm` — Xem cách John (PM agent) hoạt động, tham khảo pattern
- `bmad-sprint-planning` — Xem sprint planning flow, có thể tích hợp
- `bmad-create-epics-and-stories` — Xem cách break down work, liên quan đến estimation granularity
- `writing-for-agents` — Khi viết SKILL.md cuối cùng

### Workspace
- Workspace root: `f:\Code\skill-creator`
- Skills directory: `f:\Code\skill-creator\.agent\skills\`
- Existing BMAD skills: `f:\Code\skill-creator\.agent\skills\bmad-*`

## Yêu cầu output
Kết thúc session, tạo ra:
1. **Concept brief** — Tóm tắt skill sẽ làm gì, cho ai, giải quyết vấn đề gì
2. **Draft SKILL.md** — Bản nháp đầu tiên của skill file
3. **Estimation template** — Template input/output mẫu cho 1 epic gồm 5-7 tasks
4. **Calibration strategy** — Cách sử dụng historical data để cải thiện accuracy theo thời gian
