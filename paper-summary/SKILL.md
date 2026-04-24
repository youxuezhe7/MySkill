---
name: paper-summary
description: Generate a structured literature-summary markdown file from an uploaded paper or paper PDF. Use when the user provides a paper, asks for a fixed-format Chinese 文献概要, wants outputs matching an existing summary template, or wants the result saved as a `.md` file.
---

# Paper Summary

Generate a fixed-format Chinese literature summary from a paper. Save the result as a Markdown file, not only inline chat content.

## Workflow

1. Read the user-provided paper first. Prefer the full PDF.
2. When any step touches a PDF file, explicitly use the [pdf](C:\Users\wangx\.codex\skills\pdf\SKILL.md) skill. This includes opening the PDF, checking page layout, extracting text, extracting tables, splitting pages, and OCR for scanned or image-only PDFs. Do not describe or implement a separate ad hoc PDF handling flow inside this skill.
3. Check whether the user gave a target style example such as an existing `文献概要.md`. If so, match that structure and tone. Otherwise use the default template in [references/summary-template.md](references/summary-template.md).
4. Extract only information supported by the paper. If DOI, keywords, metrics, datasets, or other fields are absent, write `文中未提及`.
5. Produce one content artifact:
   - A structured markdown summary
6. Save the file beside the source paper unless the user specifies another directory.
7. Do not leave intermediate extraction artifacts beside the source paper. If PDF handling creates temporary text, images, page splits, OCR caches, or other helper files, delete them before finishing unless the user explicitly asks to keep them.
8. Report the saved file path and mention any missing metadata or OCR uncertainty.

## Output Standard

Use this section order unless the user explicitly asks for another format:

1. `一、基本信息`
2. `二、文章概述`
3. `三、研究背景`
4. `四、研究思路`
5. `五、研究结果`
6. `六、研究结论、不足与展望`
7. `七、读懂研究方法需要掌握的知识方向`

Inside `四、研究思路`, keep these bullets when the paper supports them:

- `提出研究问题`
- `构建研究框架`
- `选择研究方法`
- `分析数据`
- `得出结论`

Inside `六、研究结论、不足与展望`, keep these bullets when the paper supports them:

- `研究结论`
- `研究的创新性`
- `研究的不足之处`
- `研究展望`
- `研究意义`

Inside `七、读懂研究方法需要掌握的知识方向`, summarize only knowledge directions and concrete points that are actually covered by the paper itself. Group them into several high-level domains such as `机械与矿山装备`、`信号处理与故障诊断`、`机器视觉与图像处理`、`人工智能与多模态融合` when applicable. For each domain:

- start with one sentence explaining what this direction solves in the paper
- add `具体包括：`
- list the covered concepts on separate lines
- prefix the most important concepts with `核心 `
- end with one sentence like `你可以把这一方向理解为：...`

Do not include knowledge that is not covered by the paper. If a whole direction is not really present in the paper, omit that direction instead of padding it.

Follow the field wording from [references/summary-template.md](references/summary-template.md).

## File Naming

Use the paper title in the output name when it is safe for the filesystem. Default output:

- `文献概要-<论文标题>.md`

If the title is missing or too long, shorten it conservatively while preserving recognizability.

## Writing Rules

- Write in Chinese unless the user asks otherwise.
- If the source is a PDF, route all PDF operations through the `pdf` skill instead of using standalone PDF instructions in this skill.
- The final deliverable should be the summary markdown file only. Temporary working files are allowed during processing but must be cleaned up before completion unless the user explicitly asks to retain them.
- Keep method names, model names, dataset names, and quantitative metrics close to the paper wording.
- Do not invent conclusions, performance numbers, or experimental settings.
- In the final knowledge-direction section, keep the scope strict: include only content actually discussed in the paper, not generic prerequisite knowledge.
- Compress redundant prose. Prefer dense and structured summary sentences.
- When the paper is weakly structured, infer section placement conservatively and say `文中未明确说明` where needed.

## Failure Handling

- If text extraction is incomplete, say so explicitly and continue with the available content.
- If the paper language is mixed, keep the output language chosen by the user and retain original technical terms where clarity matters.
- If temporary files were created during extraction or OCR, remove them before returning the final result.
