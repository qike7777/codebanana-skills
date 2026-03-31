🛠️ PDF Toolkit
==============

Merge, split, OCR, fill forms, add watermarks, compress, and convert PDFs — all processed locally.

## Install

```
https://raw.githubusercontent.com/qike7777/skill-library/main/skills/2-documents/pdf-toolkit/SKILL.md
```

## Best For

- Operations teams
- Legal teams
- Admins
- Anyone who handles PDFs regularly

## Input

- PDF files
- Instructions for the operation you need

## Output

- Structured output based on your input

## Example Use Cases

- Merge 10 reports into one PDF with bookmarks
- Fill in a form PDF with data from a spreadsheet
- OCR a scanned document and extract the text

## How It Works

1. Extract: open PDF → extract text/tables → save .txt or .xlsx
2. Convert: PDF → Word via pdfplumber + python-docx
3. Compress: choose strategy (gs_ebook default) → report size reduction
4. AI Q&A: extract full text → agent reads & answers directly
5. Forms: detect type → fill fillable fields or annotate non-fillable with visual validation
6. Images→PDF: collect images → choose fit mode → merge to single PDF

## How to Use

1. Copy the Skill URL above and add it to your AI workspace
2. Describe your task or paste your source material
3. Specify the output format you want
4. Review and refine the result

## Tips

- Be specific about your goal for better output
- Include examples or context when possible
- Specify format preferences (table, bullets, prose, etc.)
- For best results, provide relevant source material upfront

---

[← Back to Skill Library](../../../README.md)