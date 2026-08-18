# Visual Flowchart Skill — Implementation Plan

Skill chuyển code/text/logic thành Mermaid flowchart → render ra PNG, lưu kèm `.mmd` source để chỉnh sửa.

## Design Tree Summary (từ Grilling Session)

| Quyết định | Chốt |
|---|---|
| Input | Code, file path, text, `.md` (phổ biến nhất) |
| Depth | High-level mặc định, detailed khi request |
| Output folder | `visual-flowchart/`, user chỉ định path lần đầu |
| Ngôn ngữ | EN/JA/VI — theo ngôn ngữ input, fallback EN |
| File naming | `{source-name}-{lang}.png` + `.mmd` |
| Render | `npx @mermaid-js/mermaid-cli`, fallback mermaid.ink API |
| Direction | Top-Down (TD) |
| Theme | `default` |
| Invocation | Model-invoked |
| Refine | Có — sửa `.mmd` → re-render |
| Settings | Section trong SKILL.md |
| Setup | First-time hỏi gộp, quick setup = project-scoped + defaults |
| Location | Hỏi user chọn, README giải thích options |

---

## Proposed Changes

### Skill Directory

#### [NEW] `visual-flowchart/SKILL.md`

File chính của skill. Bao gồm:

**Frontmatter:**
```yaml
---
name: visual-flowchart
description: >-
  Convert code, text descriptions, or markdown files into Mermaid flowcharts
  and render as PNG images. Use when the user says "vẽ flowchart",
  "visualize this logic", "diagram này giúp tôi", "tạo flow",
  or wants to turn code/requirements into visual diagrams.
---
```

**Sections:**
1. **Overview** — Mô tả skill: nhận input → sinh Mermaid → render PNG
2. **Settings** — Configurable section trong SKILL.md:
   - `output_base_path`: Đường dẫn thư mục output (default: `./visual-flowchart`)
   - `default_language`: EN | JA | VI (default: EN, fallback khi không detect được)
   - `default_direction`: TD | LR (default: TD)
   - `default_theme`: default | dark | forest | neutral (default: default)
   - `image_format`: png | jpg (default: png)
   - `render_engine`: npx | mermaid-ink (default: npx)
3. **On Activation / First-Time Setup**:
   - Kiểm tra Settings đã configured chưa
   - Nếu chưa → hỏi user chọn (hoặc quick setup = project-scoped + defaults)
   - Kiểm tra Node.js/npx availability
   - Nếu không có → hỏi cài, nếu từ chối → set `render_engine: mermaid-ink`
4. **Workflow**:
   - **Step 1 — Receive Input**: Detect loại input (code/file/text/md), đọc nội dung
   - **Step 2 — Analyze & Generate Mermaid**: Phân tích logic → sinh Mermaid flowchart syntax (high-level mặc định)
   - **Step 3 — Detect Language**: Detect ngôn ngữ input → dùng cho labels. Fallback sang `default_language`
   - **Step 4 — Write .mmd File**: Lưu Mermaid source vào `{output_base_path}/{source-name}-{lang}.mmd`
   - **Step 5 — Render**: `npx -y @mermaid-js/mermaid-cli -i {file}.mmd -o {file}.png` hoặc mermaid.ink API
   - **Step 6 — Report**: Gửi hyperlink tới file PNG + `.mmd` cho user
5. **Refine Flow**: User yêu cầu chỉnh sửa → agent đọc `.mmd`, sửa, re-render
6. **Error Handling**: Node.js missing, invalid syntax, render failure

---

#### [NEW] `visual-flowchart/README.md`

Tài liệu giải thích cho user:

1. **Skill là gì** — Mô tả ngắn
2. **Settings Reference** — Giải thích từng setting:
   - `output_base_path`: Thư mục lưu output. Default `./visual-flowchart`
   - `default_language`: Ngôn ngữ mặc định cho labels khi không detect được
   - `default_direction`: Hướng flowchart (TD = dọc, LR = ngang)
   - `default_theme`: Theme màu sắc
   - `image_format`: Định dạng ảnh output
   - `render_engine`: Engine render (npx local vs mermaid.ink online)
3. **Install Location Guide**:
   - `.agent/skills/` — Project-specific, chỉ dùng trong project hiện tại
   - `.agents/skills/` — Workspace-scoped, dùng trong workspace hiện tại
   - `C:\Users\{user}\.gemini\config\skills\` — Global, mọi workspace
4. **Prerequisites** — Node.js (optional, có fallback)
5. **Usage Examples** — 2-3 ví dụ minh họa
6. **Troubleshooting** — Lỗi thường gặp

---

## Verification Plan

### Manual Verification
1. Tạo skill files → kiểm tra SKILL.md structure hợp lệ
2. Test first-time setup flow: quick setup vs full setup
3. Test với input types:
   - Paste code trực tiếp
   - File path tới `.py` hoặc `.js`
   - File `.md` với logic description
   - Text mô tả từ dev
4. Kiểm tra `.mmd` + `.png` output
5. Test refine loop: sửa → re-render
6. Test fallback mermaid.ink khi không có Node.js

> [!IMPORTANT]
> Skill location sẽ được hỏi user khi tạo. Mặc định tôi sẽ tạo file trước rồi hỏi bạn muốn đặt ở đâu.

