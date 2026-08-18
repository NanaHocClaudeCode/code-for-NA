# Walkthrough — Visual Flowchart Skill

## Tóm tắt

Đã tạo xong **Visual Flowchart** skill — chuyển code/text/logic thành Mermaid flowchart → render PNG.

## Files đã tạo

| File | Location | Mô tả |
|---|---|---|
| [SKILL.md](file:///C:/Users/naman/.gemini/config/skills/visual-flowchart/SKILL.md) | Global config | Skill chính: frontmatter, settings, workflow 6 bước, refine flow, error handling |
| [README.md](file:///C:/Users/naman/.gemini/config/skills/visual-flowchart/README.md) | Global config | Settings reference, install location guide, 3 usage examples, troubleshooting |
| [implementation_plan.md](file:///C:/Users/naman/.gemini/config/skills/visual-flowchart/implementation_plan.md) | Global config | Design decisions từ grilling session |

## Quy trình

1. **Handoff** từ session trước → nhận context Visual Communication cho BA
2. **Grilling session** (21 câu hỏi) → chốt toàn bộ design tree
3. **Implementation** → tạo SKILL.md + README.md
4. **Verification** → Node.js v24.18.0, npx 11.16.0 sẵn sàng

## Key Design Decisions

- **Settings trong SKILL.md** (không file riêng) — pattern cho mọi skill tương lai
- **Render**: `npx @mermaid-js/mermaid-cli` primary, `mermaid.ink` API fallback
- **Ngôn ngữ**: Theo ngôn ngữ input, fallback EN
- **File naming**: `{source-name}-{lang}.png`
- **Refine loop**: Sửa `.mmd` → re-render
- **Location**: Global (`C:\Users\naman\.gemini\config\skills\`)

## Bước tiếp theo

Skill đã sẵn sàng sử dụng. Thử bằng cách nói:
- "Vẽ flowchart cho đoạn code này"
- "Tạo flow từ file X"
- "Visualize this logic"
