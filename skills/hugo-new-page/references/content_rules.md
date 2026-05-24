# Content Rules

These rules were derived from the project examples:

- `content/post/image-gallery/index.md`
- `content/post/markdown-syntax/index.md`
- `content/post/math-typesetting/index.md`
- `content/post/shortcodes/index.md`

## Front Matter

- Minimal posts may only need `title`, `description`, and `date`.
- This workflow uses a richer default template so new pages are consistent and easy to extend.
- `image` should only be set when the referenced asset really exists.
- `math: true` is only needed for posts that contain math rendering.
- `date` should reflect the real local or UTC timezone accurately. Do not mark a local time as `Z`, or Hugo may treat the post as a future article and omit it from the homepage.
- `categories` and `tags` should use multi-line YAML list formatting, with one list item per line, following `content/post/markdown-syntax/index.md`.

## Local Assets

- Page bundle assets are expected to live beside `index.md`.
- Gallery-style and cover-style references should point to files that exist in the same folder.
- External images should not be substituted for local bundle images when the feature depends on Hugo page bundle behavior.

## File Import

### Markdown (.md) Import

- Source files may have their own YAML front matter. Strip it; only the body is imported.
- Relative image paths in the source file are resolved relative to the source file's directory, not the bundle directory.
- Copied images should be placed directly in the bundle folder (same level as `index.md`).
- After copying, update image references in the body to bare filenames (e.g. `![alt](image.png)`).
- Do not modify the target `index.md` front matter during import.

### Word (.docx) Import

- Pandoc 3.x is required for conversion. Verify with `pandoc --version` before attempting.
- Use `pandoc <input> -t markdown --extract-media=<bundle-path> -o <temp-output>`.
- After conversion, if pandoc created a `media/` subdirectory inside the bundle, flatten it: move all files from `media/` to the bundle root, delete the empty `media/` directory, and update image paths in the body from `media/imageN.ext` to `imageN.ext`.
- Delete the temporary output file after reading its contents.
- Pandoc-generated markdown may need cleanup: extra blank lines, overly complex tables, or auto-generated image filenames.

### General Import Rules

- If the imported body already starts with a top-level `# ` heading, do not add a duplicate heading; the imported content is the body.
- If the imported body is empty, fall back to the `# 正文` placeholder.
- Warn the user about any image references that could not be resolved.

## Markdown Conventions

- Standard Markdown headings, paragraphs, lists, tables, code fences, and blockquotes are acceptable.
- `<!--more-->` is optional. Use it only if the content benefits from an explicit summary split.
- Code fences should include a language tag when practical.

## Math

- Inline or block math content should enable `math: true`.
- Do not enable math if the article does not contain mathematical notation.

## Shortcodes

- Hugo shortcodes are valid when written with normal `{{< ... >}}` or paired shortcode syntax.
- Preserve shortcode syntax exactly; do not rewrite working shortcodes into plain Markdown.

## Validation Heuristics

- Broken local file references should be repaired or removed.
- Empty or inline list metadata should be normalized into multi-line YAML lists when the skill needs to repair formatting.
- User-authored prose should be preserved whenever possible.
