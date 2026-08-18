# Handoff: Visual Communication Skill cho Business Analyst

## Bối cảnh
Người dùng đang khám phá ý tưởng phát triển **agent skill** để cải thiện chất lượng công việc BA trong môi trường phần mềm/IT. Đây là 1 trong 2 nhánh khám phá song song (nhánh kia là 3-Point Estimation cho PM).

## Mục tiêu cuối cùng
Tạo ra một **SKILL.md** có thể tái sử dụng trong Gemini/Claude, giúp BA truyền đạt ý tưởng, yêu cầu, và phân tích một cách trực quan và hiệu quả hơn.

## Phạm vi khám phá — Visual Communication cho BA

### Vấn đề cần giải quyết
BA thường gặp khó khăn khi:
- Diễn đạt business requirement phức tạp thành hình ảnh/diagram dễ hiểu cho cả dev và stakeholder
- Chuyển đổi giữa các loại diagram (flowchart, sequence, wireframe, ERD) phù hợp với từng audience
- Tạo visual artifacts nhanh mà vẫn đảm bảo chất lượng và tính nhất quán
- Trình bày dữ liệu phân tích (gap analysis, impact analysis) dưới dạng trực quan

### Các hướng có thể khám phá
1. **Requirement Visualization** — Tự động chuyển user story/requirement text thành diagram phù hợp (flow, sequence, state machine)
2. **Stakeholder-Adaptive Visuals** — Cùng 1 requirement nhưng tạo visual khác nhau cho dev vs. business stakeholder vs. QA
3. **Visual Gap Analysis** — So sánh AS-IS vs TO-BE bằng visual diff
4. **Process Mapping Assistant** — Từ mô tả text, tạo BPMN/flowchart chuẩn
5. **Data Flow Visualization** — Tạo DFD từ system description

### Câu hỏi cần trả lời trong session này
- Scope skill nên hẹp (chỉ 1 loại visual) hay rộng (multi-visual toolkit)?
- Input format nào phù hợp nhất? (free text, structured template, existing artifact)
- Output format nào? (Mermaid, PlantUML, SVG, markdown table, image generation)
- Skill nên interactive (hỏi-đáp để refine) hay one-shot (nhận input → xuất output)?
- Làm sao đảm bảo visual output phù hợp với audience (dev vs stakeholder)?

### Suggested Skills to Reference
- `bmad-agent-analyst` — Xem cách Mary (BA agent) hoạt động, tham khảo pattern
- `bmad-ux` — Xem cách tạo UX specifications, có thể tái sử dụng pattern
- `bmad-brainstorming` — Dùng để brainstorm ý tưởng chi tiết hơn
- `writing-for-agents` — Khi viết SKILL.md cuối cùng

### Workspace
- Workspace root: `f:\Code\skill-creator`
- Skills directory: `f:\Code\skill-creator\.agent\skills\`
- Existing BMAD skills: `f:\Code\skill-creator\.agent\skills\bmad-*`

## Yêu cầu output
Kết thúc session, tạo ra:
1. **Concept brief** — Tóm tắt skill sẽ làm gì, cho ai, giải quyết vấn đề gì
2. **Draft SKILL.md** — Bản nháp đầu tiên của skill file
3. **Example workflows** — 2-3 ví dụ minh họa cách skill hoạt động
