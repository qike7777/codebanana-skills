🛠️ Excel Assistant
==================

![Category](https://img.shields.io/badge/category-Documents-lightgrey)

Create Excel files from scratch, edit existing spreadsheets, or use AI to batch-process column data — all through natural conversation, no Excel knowledge needed.

## Install

Add this skill to your CodeBanana workspace:

```
Skill URL: https://raw.githubusercontent.com/qike7777/codebanana-skills/main/skills/2-documents/excel-assistant/SKILL.md
```

## Best For

- Writers
- Operators
- Legal teams
- Executives

## Output

- Structured output based on your input

## Capabilities
- Build Excel from description
- Edit existing Excel/CSV
- AI batch column processing
- Formulas & formatting
- Pivot summaries
- Data cleaning

## How It Works
1. Build: describe table → agent generates config → run builder.py → deliver .xlsx
2. Edit: upload file → summarize → confirm changes → run editor.py → deliver .xlsx
3. Batch AI: upload file → pick column → define transform → run processor.py → deliver .xlsx

## How to Use

1. Install the skill using the Skill URL above
2. Describe your task or paste your source material
3. Specify the output format you want
4. Review and refine the result

## Tips

- Give clear context for better results
- Specify the format you want (e.g. "give me a table", "write as bullet points")
- Include examples when possible
- Use focused instructions for more reliable output

## Support

For questions or feedback, open an issue in this repository.