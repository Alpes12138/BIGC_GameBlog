---
name: hugo-new-page
description: Use when creating a new Hugo content page or post folder, especially for Hugo Stack projects. This skill asks the user for a slug, detects whether the current workspace is a Hugo project, creates the page bundle folder and index.md in the correct location, supports importing body content from .md or .docx files (with automatic Word-to-Markdown conversion and image extraction), prompts the user to fill in content, then validates front matter, local resource references, and Markdown conventions before finishing.
---

# Hugo New Page

Use this skill when the user wants to create a new page or post directory with a starter `index.md`, optionally import body content from a Markdown or Word file, then guide the user through content completion and final validation.

## Workflow

1. Ask the user for the folder name (`<slug>`). Do not skip this prompt.
2. Detect the environment.
3. If `content/post/` exists in the current workspace, create the bundle at `content/post/<slug>/`.
4. Otherwise, create the bundle at `<workspace-root>/<slug>/`.
5. Create `index.md` inside that folder using the template in `references/index_template.md`.
6. Set `title` to `<slug>` unless the user explicitly provides another title.
7. Set `date` to the current creation time when the file is generated, using the local timezone offset of the current environment.
8. After creating the file, ask the user exactly: `是否需要从文件导入正文内容？支持 .md 或 .docx 文件（提供文件路径即可）。如无需导入，请回复"跳过"。`
9. Wait for the user to reply with:
   - A file path (e.g. `C:\Users\...\article.docx` or `./my-content.md`): proceed to step 10.
   - A completion signal such as `跳过`, `skip`, `no`: proceed to step 14 (content completion prompt).
10. **File Import — Determine file type:**
    - If the file ends with `.md`: go to step 11 (Markdown import).
    - If the file ends with `.docx`: go to step 12 (Word import).
    - Otherwise: tell the user the format is unsupported and return to step 9.

11. **Markdown Import:**
    - Read the file contents.
    - If the file has YAML front matter (starts and ends with `---`), extract everything after the closing `---` as the body.
    - If the file has no front matter, use the entire content as the body.
    - Replace everything after the front matter in `index.md` (i.e. the `# 正文` placeholder and anything below it) with the extracted body.
    - Copy any local image files referenced in the body into the bundle folder (same directory as `index.md`). Interpret relative image paths relative to the source file's directory. If the source image path does not resolve, skip it (do not copy non-existent files).
    - Update image references in the body to point to the bundle-local copies (e.g. `![alt](image.png)` where `image.png` now lives beside `index.md`).
    - If the source file contains Goldmark `{width="Xin" height="Yin"}` attributes on images, apply the same conversion and centering rules as in "Image Post-Processing Rules" below.
    - Go to step 14.

12. **Word Import (.docx):**
    - Check if `pandoc` is available in the environment by running `pandoc --version`. If pandoc is not found, tell the user: `未检测到 Pandoc，请安装 Pandoc（https://pandoc.org/installing.html）后重新提供文件路径，或回复"跳过"手动填写内容。` and return to step 9.
    - Run pandoc to convert the .docx file to Markdown with image extraction:
      ```bash
      pandoc "<input.docx>" -t markdown --extract-media="content/post/<slug>" -o "<temp-output.md>"
      ```
      where `<temp-output.md>` is a temporary file path (use `<workspace-root>/.kilo-hugo-import.md` or similar).
    - Remove the temp file after reading its contents.
    - Read the converted body text from the temp output.
    - The `--extract-media` flag already places images in the bundle folder under a `media/` subdirectory. If images were extracted there, move them from `media/` up to the bundle root (beside `index.md`) and update image reference paths in the body accordingly.
    - **Convert image syntax**: Replace Pandoc's Goldmark `{width="Xin" height="Yin"}` attributes with HTML `<img>` tags using pixel values, then apply centering (see "Image Post-Processing Rules" below).
    - **Preserve text alignment**: Extract centered paragraph indices from the .docx `word/document.xml`, then wrap corresponding paragraphs in `<div style="text-align:center;">...</div>` (see "Image Post-Processing Rules > Section 4" below).
    - Replace everything after the front matter in `index.md` with the processed body.
    - Go to step 14.

13. **Image Reference Validation (post-import):**
    - Scan the body of `index.md` for image references: `![...](<path>)`, `<img src="<path>">`, and `<img ... src="<path>">`.
    - For each local reference (not starting with `http://` or `https://`), verify the file exists relative to the bundle folder.
    - If a referenced image file is missing, remove the image reference line from the body.
    - If at least one valid local image exists in the bundle and `image:` in front matter is empty, set `image:` to the first valid image filename (e.g. `image: cover.jpg`).

14. Tell the user exactly: `请补充内容。补充完毕后请回复"已完成"。`
15. Wait for the user to reply with a completion signal such as `已完成`, `完成`, `done`, or an equivalent confirmation.
16. Validate the file using the checks below.
17. If the file is compliant, finish.
18. If the file is not compliant, automatically fix clear issues when the intended correction is obvious, then ask the user to confirm the revised result.
19. After the user confirms the revised result, finish.

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

## File Import Rules

When importing body content from an external file:

**Markdown (.md) import:**
- Strip any YAML front matter from the source file; only import the body (content after the second `---`).
- If the source is outside the workspace, images referenced with relative paths must be resolved relative to the source file's parent directory.
- Copy referenced local images into the bundle folder and update paths to just the filename (e.g. `![alt](image.png)`).
- Do NOT overwrite or modify the front matter of the target `index.md`.

**Word (.docx) import:**
- Pandoc 3.x is required. Use `pandoc --version` to verify availability before attempting conversion.
- Use `--extract-media=<bundle-path>` so images land directly in the bundle folder.
- Pandoc may create a `media/` subdirectory inside the extraction target. If so, move all files from `media/` up to the bundle root and remove the empty `media/` directory, then update image paths in the body from `media/filename` to just `filename`.
- After conversion, delete the temporary output file.
- Common pandoc conversion issues to be aware of:
  - Word heading styles become `# Heading` Markdown headings.
  - Word tables become Markdown tables (may need manual adjustment for complex tables).
  - Embedded images are extracted as PNG/JPEG files (pandoc names them automatically, e.g. `media/image1.png`).

**General:**
- If the imported body already starts with a top-level heading (e.g. `# Some Title`), do not add a duplicate `# 正文` heading. The imported content stands alone.
- If the body is empty after import (e.g. empty source file), fall back to the default `# 正文` placeholder.
- Warn the user if any image references in the imported content could not be resolved, listing the unresolved paths.

## Image Post-Processing Rules

After importing body content (whether from .md or .docx), apply the following transformations to all image references:

### 1. Convert Goldmark Attributes to HTML `<img>` Tags

Pandoc exports images with Goldmark attribute syntax: `![](path){width="Xin" height="Yin"}`. Hugo's Stack theme CSS does NOT respect these attributes. The `in` (inch) unit is not a valid HTML `width` attribute value. Always convert to `<img>` HTML tags with pixel dimensions.

**Conversion formula**: `pixels = inches × 96` (standard CSS DPI), rounded to integer.

Example:
```
Pandoc output:   ![](image1.jpeg){width="4.92in" height="2.7668in"}
Converted:       <img src="image1.jpeg" style="width:472px; height:266px;" alt="">
```

Implementation (PowerShell):
```powershell
$content = [regex]::Replace($content, '!\[.*?\]\(([^)]+)\)\{width="([^"]+)in"\s*\r?\nheight="([^"]+)in"\}', {
    param($m)
    $filename = [System.IO.Path]::GetFileName($m.Groups[1].Value)
    $widthPx  = [math]::Round([double]$m.Groups[2].Value * 96)
    $heightPx = [math]::Round([double]$m.Groups[3].Value * 96)
    return "<img src=""$filename"" style=""width:${widthPx}px; height:${heightPx}px;"" alt="""">"
})
```

### 2. Center All Images

After converting to `<img>` tags, apply centering:

- **Standalone images** (one `<img>` per line): Add `display:block; margin:0 auto;` to the style.
- **Paired/side-by-side images** (two `<img>` on the same line): Wrap the pair in `<div style="text-align:center;">...</div>` and add `display:inline-block;` to each `<img>`.

Before centering:
```
<img src="image3.png" style="width:422px; height:225px;" alt="">
<img src="image1.jpeg" style="width:472px; height:266px;" alt=""><img src="image2.png" style="width:472px; height:265px;" alt="">
```

After centering:
```
<img src="image3.png" style="display:block; margin:0 auto; width:422px; height:225px;" alt="">
<div style="text-align:center;"><img src="image1.jpeg" style="display:inline-block; width:472px; height:266px;" alt=""><img src="image2.png" style="display:inline-block; width:472px; height:265px;" alt=""></div>
```

Implementation (PowerShell):
```powershell
# Add centering to standalone images (single img on line)
$content = $content -replace '<img src="(.*?)style="width', '<img src="$1style="display:block; margin:0 auto; width'
# Wrap paired images in centered div and add inline-block
$content = [regex]::Replace($content,
    '<img src="([^"]+)" style="([^"]*)" alt=""><img src="([^"]+)" style="([^"]*)" alt="">',
    '<div style="text-align:center;"><img src="$1" style="display:inline-block; $2" alt=""><img src="$3" style="display:inline-block; $4" alt=""></div>'
)
# Fix ";" duplication (standalone regex applied to second img in pairs) — remove duplicate display
$content = $content -replace 'inline-block; display:block; margin:0 auto;', 'inline-block;'
```

### 3. Inline Images

If an `<img>` tag appears inline within text (e.g. `...完成题目。<img src="image21.jpeg" ...>`), split it onto its own line so it can be centered as a standalone image.

### 4. Preserve Word Text Alignment (Centering)

Word documents may contain centered paragraphs (aligned with `w:jc w:val="center"`). Pandoc does NOT preserve paragraph alignment. Use the following positional approach to restore centering.

**Algorithm:**
1. Unzip the `.docx` file and read `word/document.xml`.
2. Parse all `<w:p>` paragraphs with `<w:jc w:val="center"/>` and record their 1-based indices.
3. After pandoc conversion, split the markdown body into paragraphs (separated by blank lines `\r?\n\r?\n`).
4. For each centered paragraph index, wrap the corresponding markdown paragraph in `<div style="text-align:center;">...</div>`.

**Implementation (PowerShell):**

```powershell
# Given $docxPath and the pandoc-converted markdown body $body:

# 1. Extract centered paragraph indices from docx XML
Add-Type -AssemblyName System.IO.Compression.FileSystem
$zip = [System.IO.Compression.ZipFile]::OpenRead($docxPath)
$entry = $zip.GetEntry("word/document.xml")
$stream = $entry.Open()
$reader = New-Object System.IO.StreamReader($stream)
$xml = $reader.ReadToEnd()
$reader.Dispose(); $stream.Dispose(); $zip.Dispose()

$paraPattern = '<w:p(?:\s[^>]*)?>(.*?)</w:p>'
$paraMatches = [regex]::Matches($xml, $paraPattern, [System.Text.RegularExpressions.RegexOptions]::Singleline)
$centeredIndices = @()
$idx = 0
foreach ($pm in $paraMatches) {
    $idx++
    if ($pm.Value -match '<w:jc\s+[^>]*w:val="center"') {
        $centeredIndices += $idx
    }
}

# 2. Split markdown body into paragraphs
$paras = $body -split '\r?\n\r?\n' | Where-Object { $_.Trim().Length -gt 0 }
$result = @()

# 3. Wrap centered paragraphs
for ($i = 0; $i -lt $paras.Count; $i++) {
    $p = $paras[$i]
    $paraNum = $i + 1  # 1-based
    if ($centeredIndices -contains $paraNum) {
        # Skip empty/image-only paragraphs (already handled by img centering)
        if ($p -notmatch '^\s*<img\s' -and $p.Trim().Length -gt 0) {
            $p = "<div style=`"text-align:center;`">`r`n`r`n$p`r`n`r`n</div>"
        }
    }
    $result += $p
}

$centeredBody = $result -join "`r`n`r`n"
```

**Important notes:**
- This approach relies on positional correspondence (paragraph N in docx → paragraph N in Markdown). Pandoc maintains this for text paragraphs; images may shift indices slightly.
- Skip empty paragraphs and image-only paragraphs (already centered by Section 2 above).
- Wrap the `<div>` with double blank lines (`\r\n\r\n`) before and after to maintain proper Markdown paragraph separation.
- If the paragraph index mapping appears off by a few positions, accept minor misalignment — the user can manually adjust during the content editing phase.
- After wrapping, run the image centering pass (Section 2) so that images inside centered divs still receive proper styling.

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
