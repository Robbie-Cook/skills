---
name: canva
description: Use the Canva MCP server to search, create, edit, export, and manage Canva designs, brand templates, folders, comments, and assets. Trigger when a task involves Canva design URLs, design IDs, design generation, template autofill, export to PDF/PNG/PPTX, or Canva MCP setup and troubleshooting.
---

# Canva MCP

Use the Canva MCP server for Canva-driven design workflows. For setup and debugging details (env vars, config, verification), see `references/canva-mcp-config.md`.

## Canva MCP Integration Rules
These rules define how to work with Canva designs and must be followed for every Canva-driven task.

### Required flow (do not skip)
1. **Find the design** — Use `search-designs` to locate existing designs by keyword, or `list-folder-items` to browse folders. If you have a design URL, extract the 11-character design ID (starts with `D`). If you have a shortlink (`canva.link/...`), resolve it with `resolve-shortlink` first.
2. **Inspect the design** — Run `get-design` to retrieve metadata (title, owner, URLs, page count). Use `get-design-content` to read text content, or `get-design-pages` to view page thumbnails.
3. **Edit via transactions** — To make changes, call `start-editing-transaction` to open an editing session. Perform all edits with `perform-editing-operations`, then confirm with the user before calling `commit-editing-transaction`. Use `cancel-editing-transaction` to discard.
4. **Export when needed** — Check `get-export-formats` first, then use `export-design` to produce PDF, PNG, JPG, GIF, PPTX, or MP4.
5. **Generate new designs** — Use `generate-design` for AI-generated designs or `autofill-design` for template-based generation. Always confirm the user's preferred candidate before calling `create-design-from-candidate`.

### Design search and navigation
- Use `search-designs` for keyword-based search across docs, presentations, whiteboards, etc.
- Use `search-brand-templates` when the user wants templates for generation or autofill — never use `search-designs` for templates.
- Use `search-folders` and `list-folder-items` to browse the folder hierarchy.
- Use `get-design-content` (with `content_types: ["richtexts"]`) to read text from a design without opening an edit session.

### Editing rules
- Always call `start-editing-transaction` before any edits. The returned `transaction_id` must be used in all subsequent editing calls.
- Use `replace_text` for replacing entire text elements; use `find_and_replace_text` for partial/substring replacements.
- Use `update_fill` to replace images/videos; use `insert_fill` to add new media.
- Use `delete_element` to remove text or media elements.
- Use `format_text` for styling (color, alignment, font size/weight/style, decoration, lists, links).
- Use `position_element` and `resize_element` for layout adjustments.
- When replacing an image on a page with multiple images, use `get-assets` to view thumbnails and confirm which one to replace.
- Always show the user the returned thumbnail previews after edits.
- Always confirm with the user before committing changes.

### Template and generation rules
- For template-based generation: `search-brand-templates` (with `dataset: "non_empty"`) -> user selects -> `get-brand-template-dataset` to confirm schema -> `autofill-design` with matching data.
- For AI generation: `generate-design` with a detailed query -> user picks a candidate -> `create-design-from-candidate`.
- Use `list-brand-kits` when the user wants on-brand designs, and pass the `brand_kit_id` to `generate-design`.

### Export rules
- Always call `get-export-formats` before `export-design` to verify the format is supported.
- Share the download URL with the user after export.
- Supported formats: PDF, PNG, JPG, GIF, PPTX, MP4.

### Asset handling
- Use `upload-asset-from-url` to import external images/videos into Canva.
- Use `get-assets` to retrieve metadata and thumbnails for assets by ID.
- Asset IDs returned from uploads can be used in `autofill-design`, `update_fill`, and `insert_fill` operations.

### Comments and collaboration
- Use `list-comments` and `list-replies` to read discussion threads on a design.
- Use `comment-on-design` and `reply-to-comment` to participate in design discussions.

### Folder management
- Use `create-folder` and `move-item-to-folder` to organize designs.
- Folder ID `root` refers to the top-level of the user's Canva account.

### Page operations
- Use `merge-designs` for structural page operations: combining designs, inserting pages, reordering, or deleting pages.
- Always confirm with the user before deleting pages (irreversible).

## References
- `references/canva-mcp-config.md` — setup, verification, troubleshooting.
- `references/canva-tools-and-prompts.md` — tool catalog and prompt patterns.
