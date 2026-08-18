---
name: visual-flowchart
description: >-
  Convert code, text descriptions, or markdown files into Mermaid flowcharts
  and render as PNG images. Use when the user says "vẽ flowchart",
  "visualize this logic", "diagram này giúp tôi", "tạo flow",
  "flowchart cho đoạn này", or wants to turn code/requirements into
  visual diagrams.
---

# Visual Flowchart

## Overview

Convert any logic source — code, text explanation, markdown document, or file on disk — into a clean Mermaid flowchart, render it as a PNG image, and keep the `.mmd` source for future edits. The skill auto-detects input type, infers diagram language from the input, and produces a high-level flow by default.

## Settings

Configurable values. Edit directly in this section to change defaults.

```yaml
# ── Output ──────────────────────────────────────────────
output_base_path: ./visual-flowchart   # Folder to save .mmd + .png
image_format: png                       # png | jpg

# ── Diagram Defaults ───────────────────────────────────
default_language: EN                    # EN | JA | VI — fallback when input language is undetectable
default_direction: TD                   # TD (top-down) | LR (left-right)
default_theme: default                  # default | dark | forest | neutral
default_depth: high-level              # high-level | detailed

# ── Rendering ──────────────────────────────────────────
render_engine: npx                      # npx (local, needs Node.js) | mermaid-ink (online, no install)
```

> **Editing**: Change any value above and save. The skill reads this section on every invocation. See `README.md` in this skill folder for detailed explanations of each setting.

## On Activation

### Step 1: Check Settings

Read the `## Settings` code block above. If `output_base_path` is still `./visual-flowchart` and has never been used before (the folder does not exist), run **First-Time Setup**.

### Step 2: First-Time Setup (once only)

Present two options:

1. **Quick Setup** — Install to project-scoped location, all defaults. Create the output folder and proceed.
2. **Custom Setup** — Ask in one round:
   - Output base path (where to save diagrams)
   - Default language (EN / JA / VI)
   - Preferred render engine (npx / mermaid-ink)

Write chosen values back to the `## Settings` code block.

### Step 3: Check Render Engine

- If `render_engine: npx` — verify Node.js is available by running `node --version`.
  - **Not found**: Ask user if they want to install Node.js. If they decline, switch to `render_engine: mermaid-ink` and update Settings.
  - **Found**: Proceed.
- If `render_engine: mermaid-ink` — no prerequisites, proceed.

## Workflow

### Step 1 — Receive Input

Detect input type:

| Input | How to handle |
|---|---|
| **Pasted code** | Read directly from the message |
| **File path** | Read the file from disk |
| **`.md` file** | Read full file; if user specifies a heading/section, read only that section |
| **Text description** | Read directly from the message |

### Step 2 — Detect Language

Detect the natural language of the input content (from comments, text, variable names, surrounding context):

- Japanese detected → labels in Japanese, suffix `-ja`
- Vietnamese detected → labels in Vietnamese, suffix `-vi`
- English detected or undetectable → labels in English, suffix `-en`
- User explicitly requests a language → use that language

When user explicitly requests multi-language output, generate one diagram per language.

### Step 3 — Analyze & Generate Mermaid

Analyze the logic and produce Mermaid flowchart syntax.

**High-level (default):** Capture the main flow — 5 to 15 nodes. Decision points, major steps, start/end. Skip implementation details, individual variable assignments, minor branches.

**Detailed (on request):** Every if/else, loop, error path becomes a node. May produce 20–40+ nodes. User triggers with phrases like "chi tiết", "detailed", "every branch".

**Direction:** Use `{default_direction}` from Settings (TD or LR). User can override per request.

**Theme:** Use `{default_theme}` from Settings. User can override per request.

Output a valid Mermaid flowchart block:

```
flowchart {direction}
    A[Start] --> B{Decision?}
    B -->|Yes| C[Action 1]
    B -->|No| D[Action 2]
    C --> E[End]
    D --> E
```

### Step 4 — Derive File Name & Write .mmd

**File naming:**

- From file input: `{source-filename-without-ext}-{lang}.mmd`
  - Example: input `login.py` → `login-en.mmd`
- From pasted text: Agent derives a kebab-case name from the content's main topic
  - Example: payment processing logic → `payment-processing-en.mmd`
- User can override the name.

Write the `.mmd` file to `{output_base_path}/`.

### Step 5 — Render to Image

**Using npx (default):**

```bash
npx -y @mermaid-js/mermaid-cli mmdc -i "{file}.mmd" -o "{file}.png" -t {theme} -b transparent
```

**Using mermaid.ink (fallback):**

1. Base64-encode the Mermaid syntax
2. Fetch image from: `https://mermaid.ink/img/base64:{encoded}`
3. Save the response as `{file}.png`

### Step 6 — Report

Send the user:

1. ✅ Confirmation message
2. 🖼️ Embedded image preview (if environment supports it)
3. 📎 Clickable hyperlink to the PNG file: `[{filename}.png](file:///{absolute-path})`
4. 📝 Clickable hyperlink to the .mmd source: `[{filename}.mmd](file:///{absolute-path})`

## Refine Flow

When the user requests changes after generation:

1. Read the existing `.mmd` file
2. Apply the requested changes (add/remove/rename nodes, change connections, adjust labels)
3. Overwrite the `.mmd` file
4. Re-render to PNG (same filename, overwrite)
5. Report the updated files with hyperlinks

Common refine requests:
- "Thêm node X" / "Add step X"
- "Đổi label Y" / "Rename Y"
- "Tách branch này" / "Split this branch"
- "Làm detailed hơn" / "More detail"
- "Đổi sang tiếng Nhật" / "Switch to Japanese"

## Error Handling

| Error | Action |
|---|---|
| Node.js not found | Ask to install; if declined → switch to mermaid-ink |
| npx mmdc fails | Show error message; suggest mermaid-ink fallback |
| Invalid Mermaid syntax | Fix syntax automatically; if unfixable, show the error and the `.mmd` for manual edit |
| mermaid.ink API down | Save `.mmd` only; inform user to render manually later |
| Output folder not writable | Ask user for alternative path |
