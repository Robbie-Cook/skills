# Canva MCP tools and prompt patterns

Quick reference for the Canva MCP toolset, when to use each tool, and prompt examples.

## Design discovery tools
- `search-designs` — Search docs, presentations, whiteboards, videos, and other designs by keyword. Do NOT use for template searches.
- `search-brand-templates` — Search brand templates. Always use this (not `search-designs`) when the user wants templates or generation from templates.
- `search-folders` — Search folders by name or tags.
- `list-folder-items` — List items (designs, folders, images) inside a folder.
- `resolve-shortlink` — Resolve a `canva.link` shortlink to a full design URL.

## Design inspection tools
- `get-design` — Get metadata: title, owner, URLs, thumbnail, page count.
- `get-design-content` — Read text content (richtexts) from a design without editing.
- `get-design-pages` — Get page thumbnails and indices for paged designs (presentations, etc.).
- `get-presenter-notes` — Read speaker notes from presentation slides.
- `get-export-formats` — Check which export formats a design supports.

## Editing tools (transaction-based)
- `start-editing-transaction` — Open an edit session; returns `transaction_id` and current content.
- `perform-editing-operations` — Execute one or more edit operations in bulk:
  - `replace_text` — Replace entire text element content.
  - `find_and_replace_text` — Replace a substring within a text element.
  - `update_fill` — Replace an image/video element.
  - `insert_fill` — Add a new image/video element to a page.
  - `delete_element` — Remove a text or media element.
  - `position_element` — Move an element on the page.
  - `resize_element` — Resize an element.
  - `format_text` — Style text (color, alignment, font size/weight/style, decoration, lists, links, line height).
  - `update_title` — Change the design title.
- `commit-editing-transaction` — Save all changes (requires user confirmation).
- `cancel-editing-transaction` — Discard all changes.
- `get-design-thumbnail` — Get a page thumbnail during an editing transaction.
- `get-assets` — Get asset metadata/thumbnails by ID (use to identify images before replacing).

## Generation tools
- `generate-design` — AI-generate a design (poster, presentation, doc, social post, etc.) from a text query.
- `create-design-from-candidate` — Convert a generated candidate into an editable Canva design.
- `autofill-design` — Fill a brand template with data (text, images, charts).
- `get-brand-template-dataset` — Get the autofill schema for a brand template.
- `list-brand-kits` — List available brand kits for on-brand generation.

## Export tools
- `export-design` — Export a design to PDF, PNG, JPG, GIF, PPTX, or MP4. Returns a download URL.
- `get-export-formats` — Check supported export formats before exporting.

## Asset tools
- `upload-asset-from-url` — Upload an image/video from a URL into Canva.
- `get-assets` — Get metadata and thumbnails for assets by ID.

## Collaboration tools
- `comment-on-design` — Add a comment to a design.
- `list-comments` — List comments on a design.
- `reply-to-comment` — Reply to an existing comment.
- `list-replies` — List replies on a specific comment.

## Folder and page tools
- `create-folder` — Create a new folder.
- `move-item-to-folder` — Move a design, folder, or image to a folder.
- `merge-designs` — Combine pages from multiple designs, insert/move/delete pages.
- `import-design-from-url` — Import a file (PDF, PPTX, etc.) from a public URL as a new design.

## Prompt patterns

### Searching
- "Find my recent presentations about Q4 results"
- "Search for brand templates for social media posts"
- "List all designs in my Marketing folder"

### Reading content
- "What text is on slide 3 of this presentation?" (use `get-design-content` with `pages: [3]`)
- "Show me the speaker notes for this deck" (use `get-presenter-notes`)

### Editing
- "Change the title on slide 1 to 'Annual Report 2025'" (use `find_and_replace_text` or `replace_text`)
- "Replace the hero image with this new photo" (use `update_fill`)
- "Make the heading text bold and red" (use `format_text`)
- "Delete the footer text from all pages" (use `delete_element`)

### Generating
- "Create a presentation about our product launch" (use `generate-design` with `design_type: "presentation"`)
- "Generate a social media post for Instagram" (use `generate-design` with `design_type: "instagram_post"`)
- "Fill our weekly report template with this data" (use `search-brand-templates` -> `autofill-design`)

### Exporting
- "Export this presentation as a PDF" (use `export-design` with `format.type: "pdf"`)
- "Download slide 2 as a PNG" (use `export-design` with `format.type: "png"` and `format.pages: [2]`)

### Organizing
- "Move this design to the Q4 Reports folder" (use `move-item-to-folder`)
- "Combine these two presentations into one" (use `merge-designs`)

## Best-practice flow reminder
For editing: `get-design` -> `start-editing-transaction` -> `perform-editing-operations` -> show preview -> confirm with user -> `commit-editing-transaction`.
For generation: `generate-design` -> user picks candidate -> `create-design-from-candidate`.
For templates: `search-brand-templates` -> user picks -> `get-brand-template-dataset` -> `autofill-design`.
