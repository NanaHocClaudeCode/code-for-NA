# Visual Flowchart — README

A skill that converts code, text descriptions, and markdown files into clean Mermaid flowcharts, rendered as PNG images.

## Quick Start

Just ask naturally:

- "Vẽ flowchart cho file `login.py`"
- "Visualize this logic" + paste code
- "Diagram này giúp tôi" + paste text
- "Tạo flow từ file `docs/payment-flow.md`"

The skill auto-detects your input, generates a Mermaid flowchart, renders it as PNG, and sends you clickable links to both the image and the editable `.mmd` source.

## Settings Reference

Settings are stored directly in the `## Settings` section of `SKILL.md`. Edit them there.

### `output_base_path`

**Default:** `./visual-flowchart`

The folder where all generated `.mmd` and `.png` files are saved. Can be an absolute or relative path. The folder is created automatically if it doesn't exist.

**Examples:**
- `./visual-flowchart` — relative to your project root
- `D:/Diagrams/flowcharts` — absolute path
- `../shared-diagrams` — relative, outside project

---

### `image_format`

**Default:** `png`

Output image format.

| Value | Description |
|---|---|
| `png` | Lossless, sharp text. Recommended for diagrams. |
| `jpg` | Smaller file size, but text may blur slightly. |

---

### `default_language`

**Default:** `EN`

Fallback language for diagram labels when the input language cannot be detected (e.g., pure code with no comments).

| Value | Description |
|---|---|
| `EN` | English |
| `JA` | Japanese (日本語) |
| `VI` | Vietnamese (Tiếng Việt) |

> **Note:** The skill automatically detects the input's language and uses it for labels. This setting is only the fallback when detection fails.

---

### `default_direction`

**Default:** `TD`

The flow direction of the diagram.

| Value | Description | Best for |
|---|---|---|
| `TD` | Top-Down (vertical) | Mobile + desktop viewing, standard flowcharts |
| `LR` | Left-Right (horizontal) | Wide process flows, timelines |

---

### `default_theme`

**Default:** `default`

Mermaid theme controlling colors and styling.

| Value | Description |
|---|---|
| `default` | Clean, balanced colors |
| `dark` | Dark background, light text |
| `forest` | Green-toned, organic feel |
| `neutral` | Grayscale, minimal |

---

### `default_depth`

**Default:** `high-level`

How much detail the flowchart includes.

| Value | Nodes | Description |
|---|---|---|
| `high-level` | 5–15 | Main flow only. Decision points, major steps, start/end. |
| `detailed` | 20–40+ | Every branch, loop, error path. |

> You can always override per request: "làm detailed hơn" or "chi tiết hơn".

---

### `render_engine`

**Default:** `npx`

The engine used to convert Mermaid syntax to images.

| Value | Requires | Speed | Offline? | Quality |
|---|---|---|---|---|
| `npx` | Node.js installed | Fast | ✅ Yes | ⭐⭐⭐ Best |
| `mermaid-ink` | Internet connection | Medium | ❌ No | ⭐⭐ Good |

> **If you don't have Node.js:** The skill will ask if you want to install it. If you decline, it automatically switches to `mermaid-ink`.

---

## Install Location Guide

When setting up the skill for the first time, you choose where it lives:

### `.agent/skills/` — Project-Specific

- ✅ Only available in the current project
- ✅ Good for: Skills tied to one codebase
- ❌ Won't appear in other projects
- **Path example:** `f:\Code\my-project\.agent\skills\visual-flowchart\`

### `.agents/skills/` — Workspace-Scoped

- ✅ Available to all projects in the current workspace
- ✅ Good for: Dev tools you use across projects in one workspace
- ❌ Won't appear in other workspaces
- **Path example:** `f:\Code\skill-creator\.agents\skills\visual-flowchart\`

### `C:\Users\{user}\.gemini\config\skills\` — Global

- ✅ Available in **every** workspace and project
- ✅ Good for: General-purpose tools you always want available
- ❌ Changes affect all workspaces
- **Path example:** `C:\Users\naman\.gemini\config\skills\visual-flowchart\`

---

## Usage Examples

### Example 1: Code → Flowchart

> "Vẽ flowchart cho đoạn này"

```python
def process_order(order):
    if not order.is_valid():
        return reject(order)
    if order.total > 1000:
        approval = request_manager_approval(order)
        if not approval:
            return reject(order)
    charge_payment(order)
    send_confirmation(order)
    return success(order)
```

**Output:** A flowchart showing: Start → Validate → [Invalid?] → Reject / [>1000?] → Manager Approval → [Denied?] → Reject / Charge → Confirm → Success

### Example 2: Markdown File → Flowchart

> "Tạo flow từ file `docs/login-flow.md`"

The skill reads the markdown, extracts the logical flow, and generates a diagram.

### Example 3: Refine After Generation

> "Thêm node 'Send Email Notification' sau bước Charge Payment"

The skill reads the existing `.mmd`, adds the node, re-renders, and sends updated links.

---

## Troubleshooting

### "Node.js not found"

The skill needs Node.js for local rendering via `npx`. Options:
1. Install Node.js from [nodejs.org](https://nodejs.org/)
2. Or let the skill switch to `mermaid-ink` (online rendering, no install needed)

### "npx mmdc failed"

Common causes:
- Network issue during first-time download of mermaid-cli
- Invalid Mermaid syntax (the skill will try to auto-fix)

Fix: The skill automatically falls back to `mermaid-ink`. Or fix the `.mmd` file manually and ask to re-render.

### "mermaid.ink API not responding"

The online service may be temporarily down. The skill will:
1. Save the `.mmd` file (your diagram is not lost)
2. Suggest retrying later or installing Node.js for local rendering
