---
name: hugo-new-page
description: Use when creating a new Hugo content page or post folder, especially for Hugo Stack projects. This skill asks the user for a slug, detects whether the current workspace is a Hugo project, creates the page bundle folder and index.md in the correct location, prompts the user to fill in content, then validates front matter, local resource references, and Markdown conventions before finishing.
---

# Hugo New Page

Use this skill when the user wants to create a new page or post directory with a starter `index.md`, then guide the user through content completion and final validation.

## Workflow

1. Ask the user for the folder name (`<slug>`). Do not skip this prompt.
2. Detect the environment.
3. If `content/post/` exists in the current workspace, create the bundle at `content/post/<slug>/`.
4. Otherwise, create the bundle at `<workspace-root>/<slug>/`.
5. Create `index.md` inside that folder using the template in `references/index_template.md`.
6. Set `title` to `<slug>` unless the user explicitly provides another title.
7. Set `date` to the current creation time when the file is generated, using the local timezone offset of the current environment.
8. After creating the file, tell the user exactly: `请补充内容。补充完毕后请回复“已完成”。`
9. Wait for the user to reply with a completion signal such as `已完成`, `完成`, `done`, or an equivalent confirmation.
10. Validate the file using the checks below.
11. If the file is compliant, finish.
12. If the file is not compliant, automatically fix clear issues when the intended correction is obvious, then ask the user to confirm the revised result.
13. After the user confirms the revised result, finish.

## Template Rules

Follow this front matter shape by default:

```yaml
---
#标题
title: <slug>
#介绍
description:
#日期
date: <current timestamp with local timezone offset>
#封面图（例如 cover.jpg , 本文件夹必须有cover.jpg这个文件）
image:
math:
license:
comments: false
#分类,一行代表有一个(- example)
categories:
    - example-category1
#标签,一行代表有一个(- example)
tags:
    - example-tag
build:
    list: always
---
```

Additional rules:

- Do not wrap scalar values such as `title` in quotes unless the user explicitly wants them.
- `date` must be the actual creation timestamp at runtime, not a placeholder.
- Do not append `Z` unless the captured time is truly UTC. Prefer a local offset such as `+08:00` when working in a local timezone.
- Avoid generating a misleading UTC timestamp from local wall-clock time, because Hugo may treat the post as a future publication and hide it from the homepage.
- Default `comments` to `false`.
- Format `categories` and `tags` as multi-line YAML lists, with one item per line prefixed by `-`, matching `content/post/markdown-syntax/index.md`.
- Preserve the inline Chinese field comments shown in `references/index_template.md`.
- Leave the body ready for user editing with the heading `# 正文`.

## Validation Checks

After the user says content entry is complete, inspect `index.md` and validate:

- The front matter is present and structurally valid for Hugo.
- `title`, `date`, `comments`, `categories`, `tags`, and `build.list` are present unless the user deliberately removed them.
- `date` does not misrepresent the timezone. If the time was captured from local time, prefer a matching local offset instead of `Z`.
- Any `image:` value that points to a local file must resolve to a file that exists in the same bundle or at the referenced relative path.
- Any Markdown image reference like `![...](...)` that is intended to be local must point to an existing file.
- Any internal Markdown link should use a sensible relative or absolute site path and should not be obviously broken.
- If the post uses math notation, set `math: true`.
- If the post uses shortcodes, preserve valid Hugo shortcode syntax.
- Do not force example-only constructs such as `<!--more-->`, math blocks, galleries, or shortcodes unless the content actually needs them.

## Auto-fix Rules

Apply automatic fixes only when the intended correction is clear:

- If `image:` references a missing local file, clear the field to `image:` rather than leaving a broken reference.
- If `categories` or `tags` use inline array syntax or malformed empty entries, normalize them into multi-line YAML lists with one `-` entry per line.
- If `comments` is missing, set it to `false`.
- If `date` uses `Z` but appears to have been copied from local wall-clock time, rewrite it with the local timezone offset when that correction is clear.
- If the article contains obvious math expressions and `math` is empty, set `math: true`.

When auto-fixes are applied, show the user what was corrected and ask for confirmation before finishing.

## Reference Material

Use `references/content_rules.md` to judge whether the content style aligns with the project's examples.
Use `references/index_template.md` for the starter `index.md` template.

## Notes

- The terminal may display mojibake for Chinese content; do not assume the file contents are broken based only on terminal rendering.
- Prefer UTF-8-safe edits and preserve the user's written content unless a correction is necessary for validity.
