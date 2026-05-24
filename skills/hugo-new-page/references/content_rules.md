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

## Local Assets

- Page bundle assets are expected to live beside `index.md`.
- Gallery-style and cover-style references should point to files that exist in the same folder.
- External images should not be substituted for local bundle images when the feature depends on Hugo page bundle behavior.

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
- Empty placeholder metadata should be normalized when it creates malformed or misleading output.
- User-authored prose should be preserved whenever possible.
