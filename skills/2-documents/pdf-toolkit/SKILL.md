---
name: pdf-toolkit
description: |
  Full-featured local PDF toolkit: extract text/tables, merge/split/rotate/crop,
  compress, convert to Word/Excel, fill forms, OCR scans, AI summarize & Q&A,
  images-to-PDF, reorder/delete pages, watermark, password protect — all Python, no uploads.
---

# SKILL.md — PDF Toolkit

> 本地 PDF 全能工具——提取、合并/拆分、压缩、格式转换、表单填写、OCR、AI摘要问答、图片转PDF、页面重排——全部 Python 实现，无需上传文件到任何服务。

## Overview

**核心工具栈：**

| 库 | 用途 | 安装 |
|----|------|------|
| `pypdf` | 合并/拆分/旋转/裁剪/加密/元数据 | `pip install pypdf` |
| `pdfplumber` | 文字提取（保留布局）/ 表格提取 | `pip install pdfplumber` |
| `reportlab` | 从零创建 PDF / 复杂排版 | `pip install reportlab` |
| `pypdfium2` | 高性能渲染 / PDF 转图片 | `pip install pypdfium2` |
| `pdf2image` + `pytesseract` | OCR 扫描件 | `pip install pdf2image pytesseract` |
| `python-docx` | 生成 Word 文档 | `pip install python-docx` |
| `pandas` + `openpyxl` | 表格 → Excel/CSV | `pip install pandas openpyxl` |
| `Pillow` | 图片处理 / 图片转PDF | `pip install Pillow` |
| `qpdf` (CLI) | 高级合并/拆分/修复/压缩/加密 | `apt install qpdf` |
| `poppler-utils` (CLI) | pdftotext / pdftoppm / pdfimages | `apt install poppler-utils` |
| `ghostscript` (CLI) | 高质量压缩 | `apt install ghostscript` |

**scripts（随 Skill 附带）：**

```
.codebanana/.skills/pdf-toolkit/scripts/
├── check_fillable_fields.py           # 检测 PDF 是否有可填字段
├── extract_form_field_info.py         # 导出字段信息 JSON
├── fill_fillable_fields.py            # 填写可填字段
├── convert_pdf_to_images.py           # PDF → PNG（每页一图）
├── create_validation_image.py         # 生成边界框验证图
├── check_bounding_boxes.py            # 校验 fields.json 边界框
├── fill_pdf_form_with_annotations.py  # 填写非可填 PDF（文本注释方式）
├── compress_pdf.py                    # PDF 压缩（多策略）
├── images_to_pdf.py                   # 图片合并为 PDF
├── reorder_pages.py                   # 页面重排 / 删除 / 插入
└── pdf_to_word.py                     # PDF → Word（.docx）
```

---

## Phase 1 — 理解需求

询问用户：

1. **做什么操作？**（从下表选择，可多选）
2. **输入文件**：文件路径（或多个）
3. **输出期望**：文件名、格式、存放位置

### 操作速查表

| 用户说 | 对应操作 | 跳到 |
|--------|---------|------|
| 提取文字 / 读PDF内容 | 文字提取 | Phase 2 |
| 提取表格 / 导出Excel | 表格提取 | Phase 2 |
| 合并几个PDF / 拼在一起 | 合并 | Phase 3 |
| 拆分 / 按页分开 / 提取某几页 | 拆分 | Phase 3 |
| 旋转 / 方向不对 | 旋转 | Phase 3 |
| 加水印 | 水印 | Phase 3 |
| 加密码 / 保护 / 解密 | 密码保护 | Phase 3 |
| 从零生成PDF报告 / 创建PDF | 创建 | Phase 4 |
| 填写表单 / 填表 | 表单填写 | Phase 5 |
| 扫描件 / 图片PDF / OCR / 识别文字 | OCR | Phase 6 |
| 批量处理一批PDF | 批量 | Phase 7 |
| 压缩 / 文件太大 / 发不出去 | 压缩 | Phase 8 |
| 转成Word / 转成Excel / 要编辑 | 格式转换 | Phase 9 |
| 摘要 / 总结 / 问这份PDF | AI摘要问答 | Phase 10 |
| 图片合成PDF / 几张图片变一个PDF | 图片转PDF | Phase 11 |
| 删除某页 / 调换页面顺序 / 重排 | 页面重排 | Phase 12 |

**完成条件：** 确认操作类型 + 文件路径

---

## Phase 2 — 文字 & 表格提取

### 2.1 提取文字（保留布局）

```python
import pdfplumber

with pdfplumber.open("document.pdf") as pdf:
    full_text = "\n\n".join(
        f"--- Page {i+1} ---\n{page.extract_text() or ''}"
        for i, page in enumerate(pdf.pages)
    )

with open("output.txt", "w", encoding="utf-8") as f:
    f.write(full_text)
print(f"✅ 提取完成 → output.txt（{len(full_text)} 字符）")
```

**按区域提取（指定 x/y 范围）：**
```python
with pdfplumber.open("document.pdf") as pdf:
    page = pdf.pages[0]
    region_text = page.within_bbox((100, 100, 400, 200)).extract_text()
```

### 2.2 提取表格 → Excel / CSV

```python
import pdfplumber, pandas as pd

with pdfplumber.open("document.pdf") as pdf:
    all_tables = []
    for i, page in enumerate(pdf.pages):
        for j, table in enumerate(page.extract_tables()):
            if table:
                df = pd.DataFrame(table[1:], columns=table[0])
                df["_page"] = i + 1
                all_tables.append(df)

if all_tables:
    with pd.ExcelWriter("extracted_tables.xlsx") as writer:
        for idx, df in enumerate(all_tables):
            df.to_excel(writer, sheet_name=f"Table_{idx+1}", index=False)
    print(f"✅ 提取了 {len(all_tables)} 张表格 → extracted_tables.xlsx")
```

**复杂表格（无线框）：**
```python
table_settings = {
    "vertical_strategy": "text",
    "horizontal_strategy": "text",
    "snap_tolerance": 3,
    "intersection_tolerance": 15,
}
tables = page.extract_tables(table_settings)
```

---

## Phase 3 — 文档操作（合并 / 拆分 / 旋转 / 水印 / 加密）

### 3.1 合并 PDF

```python
from pypdf import PdfWriter, PdfReader

writer = PdfWriter()
for pdf_file in ["doc1.pdf", "doc2.pdf", "doc3.pdf"]:
    reader = PdfReader(pdf_file)
    for page in reader.pages:
        writer.add_page(page)

with open("merged.pdf", "wb") as f:
    writer.write(f)
print("✅ 合并完成 → merged.pdf")
```

CLI（大文件推荐）：
```bash
qpdf --empty --pages doc1.pdf doc2.pdf doc3.pdf -- merged.pdf
```

### 3.2 拆分 PDF

**每页一个文件：**
```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("input.pdf")
for i, page in enumerate(reader.pages):
    writer = PdfWriter()
    writer.add_page(page)
    with open(f"page_{i+1:03d}.pdf", "wb") as f:
        writer.write(f)
```

**提取指定页范围（如第 3–7 页）：**
```python
reader = PdfReader("input.pdf")
writer = PdfWriter()
for i in range(2, 7):   # 0-indexed
    writer.add_page(reader.pages[i])
with open("pages_3_to_7.pdf", "wb") as f:
    writer.write(f)
```

CLI：
```bash
qpdf input.pdf --pages . 3-7 -- pages_3_to_7.pdf
```

### 3.3 旋转页面

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("input.pdf")
writer = PdfWriter()
for page in reader.pages:
    page.rotate(90)   # 90 / 180 / 270
    writer.add_page(page)
with open("rotated.pdf", "wb") as f:
    writer.write(f)
```

### 3.4 裁剪页面

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("input.pdf")
writer = PdfWriter()
page = reader.pages[0]
page.mediabox.left   = 50
page.mediabox.bottom = 50
page.mediabox.right  = 550
page.mediabox.top    = 750
writer.add_page(page)
with open("cropped.pdf", "wb") as f:
    writer.write(f)
```

### 3.5 添加水印

```python
from pypdf import PdfReader, PdfWriter

watermark_page = PdfReader("watermark.pdf").pages[0]
reader = PdfReader("input.pdf")
writer = PdfWriter()
for page in reader.pages:
    page.merge_page(watermark_page)
    writer.add_page(page)
with open("watermarked.pdf", "wb") as f:
    writer.write(f)
```

> 若无现成水印 PDF，先用 Phase 4 的 reportlab 创建一个透明文字水印。

### 3.6 密码保护 & 解密

**加密：**
```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("input.pdf")
writer = PdfWriter()
for page in reader.pages:
    writer.add_page(page)
writer.encrypt(user_password="open123", owner_password="admin456")
with open("encrypted.pdf", "wb") as f:
    writer.write(f)
```

**解密：**
```python
reader = PdfReader("encrypted.pdf")
if reader.is_encrypted:
    reader.decrypt("open123")
writer = PdfWriter()
for page in reader.pages:
    writer.add_page(page)
with open("decrypted.pdf", "wb") as f:
    writer.write(f)
```

---

## Phase 4 — 创建新 PDF

### 4.1 简单文字 PDF

```python
from reportlab.lib.pagesizes import A4
from reportlab.pdfgen import canvas

c = canvas.Canvas("output.pdf", pagesize=A4)
width, height = A4
c.setFont("Helvetica-Bold", 18)
c.drawString(72, height - 72, "文档标题")
c.setFont("Helvetica", 12)
c.drawString(72, height - 110, "正文内容第一行")
c.save()
```

### 4.2 多页报告（含表格）

```python
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, Table, TableStyle, PageBreak
from reportlab.lib.styles import getSampleStyleSheet
from reportlab.lib.pagesizes import A4
from reportlab.lib import colors

doc = SimpleDocTemplate("report.pdf", pagesize=A4)
styles = getSampleStyleSheet()
story = []

story.append(Paragraph("季度报告", styles["Title"]))
story.append(Spacer(1, 12))
story.append(Paragraph("正文内容。" * 10, styles["Normal"]))
story.append(Spacer(1, 12))

data = [
    ["产品", "Q1", "Q2", "Q3"],
    ["产品A", "120", "135", "142"],
    ["产品B", "85",  "92",  "98"],
]
table = Table(data, colWidths=[120, 60, 60, 60])
table.setStyle(TableStyle([
    ("BACKGROUND", (0, 0), (-1, 0), colors.grey),
    ("TEXTCOLOR",  (0, 0), (-1, 0), colors.whitesmoke),
    ("FONTNAME",   (0, 0), (-1, 0), "Helvetica-Bold"),
    ("ALIGN",      (0, 0), (-1, -1), "CENTER"),
    ("GRID",       (0, 0), (-1, -1), 0.5, colors.black),
]))
story.append(table)
story.append(PageBreak())

doc.build(story)
print("✅ report.pdf 已生成")
```

---

## Phase 5 — 表单填写

> 先用 check_fillable_fields.py 检测，再选对应流程。

### 5.1 检测表单类型

```bash
python3 .codebanana/.skills/pdf-toolkit/scripts/check_fillable_fields.py input.pdf
```
- "Has fillable fields" → 走 **5.2**
- "No fillable fields" → 走 **5.3**

### 5.2 可填字段流程

```bash
# Step A: 导出字段信息
python3 .codebanana/.skills/pdf-toolkit/scripts/extract_form_field_info.py input.pdf field_info.json

# Step B: PDF 转图片辅助确认
python3 .codebanana/.skills/pdf-toolkit/scripts/convert_pdf_to_images.py input.pdf ./pdf_images/
```

Agent 读取 `field_info.json` + 图片，向用户确认填写内容，生成 `field_values.json`：

```json
[
  {"field_id": "last_name", "description": "姓氏", "page": 1, "value": "张"},
  {"field_id": "Checkbox12", "description": "是否同意", "page": 1, "value": "/On"}
]
```

```bash
# Step C: 填写
python3 .codebanana/.skills/pdf-toolkit/scripts/fill_fillable_fields.py \
  input.pdf field_values.json output_filled.pdf
```

### 5.3 非可填字段流程（文本注释）

```bash
# Step A: 转图片
python3 .codebanana/.skills/pdf-toolkit/scripts/convert_pdf_to_images.py input.pdf ./pdf_images/
```

Agent 视觉分析每页 PNG，确定坐标，生成 `fields.json`（格式见 Technical Reference），然后：

```bash
# Step B: 生成验证图（必须检查，不可跳过）
python3 .codebanana/.skills/pdf-toolkit/scripts/create_validation_image.py \
  1 fields.json ./pdf_images/page_1.png validation_page1.png

# Step C: 写入 PDF
python3 .codebanana/.skills/pdf-toolkit/scripts/fill_pdf_form_with_annotations.py \
  input.pdf fields.json output_filled.pdf
```

---

## Phase 6 — OCR（扫描件 / 图片 PDF）

```bash
pip install pdf2image pytesseract Pillow
apt install tesseract-ocr tesseract-ocr-chi-sim   # Linux，含中文
```

```python
import pytesseract
from pdf2image import convert_from_path

images = convert_from_path("scanned.pdf", dpi=300)
full_text = ""
for i, image in enumerate(images):
    text = pytesseract.image_to_string(image, lang="chi_sim+eng")
    full_text += f"\n--- 第 {i+1} 页 ---\n{text}"

with open("ocr_output.txt", "w", encoding="utf-8") as f:
    f.write(full_text)
print(f"✅ OCR 完成，{len(images)} 页 → ocr_output.txt")
```

**高质量 OCR（pypdfium2 渲染）：**
```python
import pypdfium2 as pdfium, pytesseract

pdf = pdfium.PdfDocument("scanned.pdf")
for i, page in enumerate(pdf):
    bitmap = page.render(scale=3.0)
    img = bitmap.to_pil()
    text = pytesseract.image_to_string(img, lang="chi_sim+eng")
```

---

## Phase 7 — 批量处理

```python
import os, glob, logging
from pypdf import PdfReader, PdfWriter

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

def batch_process(input_dir, operation="extract_text"):
    pdf_files = sorted(glob.glob(os.path.join(input_dir, "*.pdf")))
    logger.info(f"找到 {len(pdf_files)} 个 PDF 文件")

    if operation == "merge":
        writer = PdfWriter()
        for f in pdf_files:
            try:
                for page in PdfReader(f).pages:
                    writer.add_page(page)
                logger.info(f"  ✓ {os.path.basename(f)}")
            except Exception as e:
                logger.error(f"  ✗ {os.path.basename(f)}: {e}")
        with open("batch_merged.pdf", "wb") as out:
            writer.write(out)

    elif operation == "extract_text":
        os.makedirs("output_texts", exist_ok=True)
        for f in pdf_files:
            try:
                import pdfplumber
                with pdfplumber.open(f) as pdf:
                    text = "\n".join(p.extract_text() or "" for p in pdf.pages)
                out_path = os.path.join("output_texts",
                                        os.path.basename(f).replace(".pdf", ".txt"))
                open(out_path, "w", encoding="utf-8").write(text)
                logger.info(f"  ✓ {os.path.basename(f)} → {len(text)} 字符")
            except Exception as e:
                logger.error(f"  ✗ {os.path.basename(f)}: {e}")
```

---

## Phase 8 — PDF 压缩

> 用户说"文件太大""发不出去""上传限制"时触发。

**策略选择：**

| 场景 | 推荐策略 | 预期压缩率 |
|------|---------|-----------|
| 含大量图片 | Ghostscript screen/ebook | 50–80% |
| 纯文字/矢量 | qpdf optimize | 10–30% |
| 通用 | 先 qpdf，再 Ghostscript | 视情况 |

```bash
python3 .codebanana/.skills/pdf-toolkit/scripts/compress_pdf.py \
  --input input.pdf \
  --output compressed.pdf \
  --strategy gs_ebook
```

**策略说明（compress_pdf.py 支持）：**
- `qpdf_optimize` — qpdf 内置优化，无损，速度快
- `gs_screen` — Ghostscript 最小文件，72dpi 图片，适合屏幕查看
- `gs_ebook` — Ghostscript 平衡压缩，150dpi，适合发送分享（**推荐默认**）
- `gs_printer` — Ghostscript 高质量，300dpi，适合打印
- `gs_prepress` — Ghostscript 最高质量，300dpi+颜色配置文件

脚本会在压缩后自动报告：原始大小 → 压缩后大小 → 压缩率。

---

## Phase 9 — 格式转换（PDF → Word / Excel）

> 用户说"我要编辑这个 PDF""转成 Word""导出成表格"时触发。

### 9.1 PDF → Word（.docx）

```bash
python3 .codebanana/.skills/pdf-toolkit/scripts/pdf_to_word.py \
  --input document.pdf \
  --output document.docx
```

**原理：** pdfplumber 提取文字（保留段落结构）→ python-docx 重建文档，保留标题层级和段落。

**注意事项：**
- 复杂排版（多栏、浮动图片）转换后格式会简化，这是 PDF 格式特性限制，无法完美还原
- 扫描件需先走 Phase 6 OCR，再转 Word
- 表格会单独提取并插入 Word 表格

### 9.2 PDF → Excel（表格数据）

直接用 Phase 2.2 提取表格 → Excel，这是最可靠的路径。

### 9.3 PDF → 纯文本 / Markdown

```python
import pdfplumber

with pdfplumber.open("document.pdf") as pdf:
    lines = []
    for page in pdf.pages:
        text = page.extract_text()
        if text:
            lines.append(text)

# 输出纯文本
with open("output.txt", "w", encoding="utf-8") as f:
    f.write("\n\n".join(lines))

# 或输出 Markdown（简单转换）
with open("output.md", "w", encoding="utf-8") as f:
    f.write("\n\n".join(lines))
```

---

## Phase 10 — AI 摘要 & 问答

> 用户说"帮我总结这份 PDF""这里面有没有提到XX""合同里的违约条款是什么"时触发。

**流程：**

**Step 1 — 提取全文**
```python
import pdfplumber

def extract_full_text(pdf_path):
    with pdfplumber.open(pdf_path) as pdf:
        pages_text = []
        for i, page in enumerate(pdf.pages):
            text = page.extract_text()
            if text and text.strip():
                pages_text.append(f"[第{i+1}页]\n{text}")
    return "\n\n".join(pages_text)

full_text = extract_full_text("document.pdf")
print(f"提取完成：{len(full_text)} 字符，约 {len(full_text)//500} 页内容")
```

**Step 2 — Agent 直接分析**

提取文字后，Agent 直接读取内容完成任务，无需调用外部 API：

- **摘要**：Agent 阅读全文，输出结构化摘要（背景、核心内容、关键数字、结论）
- **问答**：Agent 在全文中定位相关段落，直接回答用户问题，并标注来源页码
- **关键信息提取**：日期、金额、人名、条款编号等
- **对比分析**：如果用户上传多份 PDF，Agent 对比异同

**长文档处理（超过 50 页）：**
```python
# 分块处理，每块约 20 页
chunk_size = 20
chunks = [pages_text[i:i+chunk_size] for i in range(0, len(pages_text), chunk_size)]
# Agent 逐块阅读，最后综合输出
```

**交付格式示例（摘要）：**
```
📄 文档摘要：[文件名]
共 X 页 | X 字符

【核心内容】
...

【关键数字/日期】
- ...

【重要条款/结论】
- ...

【需要关注的风险/问题】（如有）
- ...
```

---

## Phase 11 — 图片转 PDF

> 用户说"把这几张图片合成一个 PDF""扫描了几张纸要合并"时触发。

```bash
python3 .codebanana/.skills/pdf-toolkit/scripts/images_to_pdf.py \
  --input "scan_001.jpg" "scan_002.jpg" "scan_003.jpg" \
  --output scans.pdf \
  --page-size A4 \
  --fit-mode fill
```

**支持格式：** JPG / PNG / BMP / TIFF / WebP

**fit-mode 选项：**
- `fill` — 拉伸填满整页（推荐，适合扫描件）
- `fit` — 保持比例，留白边
- `original` — 保持原始尺寸，不缩放

也可以用通配符批量转：
```bash
python3 .codebanana/.skills/pdf-toolkit/scripts/images_to_pdf.py \
  --input-dir ./scans/ \
  --pattern "*.jpg" \
  --output merged_scans.pdf
```

---

## Phase 12 — 页面重排 / 删除 / 插入

> 用户说"把第3页删掉""把最后两页移到最前面""调换页面顺序"时触发。

```bash
python3 .codebanana/.skills/pdf-toolkit/scripts/reorder_pages.py \
  --input input.pdf \
  --output output.pdf \
  --pages "2,1,4,5"      # 指定新顺序（1-indexed）
```

**常见操作示例：**

```bash
# 删除第 3 页（共 5 页 → 输出 1,2,4,5）
--pages "1,2,4,5"

# 把最后两页移到最前（共 5 页）
--pages "4,5,1,2,3"

# 只保留奇数页
--pages "1,3,5,7,9"

# 反转所有页面顺序
--pages "reverse"

# 删除范围（如删除第 3-5 页，共 8 页）
--delete "3-5"
```

Agent 在执行前先告诉用户：
> 你的文档共 X 页。我将按以下顺序重排：[新顺序预览]
> 确认后执行。

---

## Technical Reference

### 一键安装全部依赖

```bash
pip install pypdf pdfplumber reportlab pypdfium2 pandas openpyxl \
            pdf2image pytesseract Pillow python-docx

# Linux 系统工具
apt install -y qpdf poppler-utils tesseract-ocr tesseract-ocr-chi-sim ghostscript
```

### 工具选型速查

| 任务 | 最佳工具 | 备注 |
|------|---------|------|
| 合并/拆分 | pypdf 或 qpdf | 大文件用 qpdf |
| 提取文字 | pdfplumber | 保留布局最好 |
| 提取表格 | pdfplumber + pandas | 导出 Excel |
| 创建 PDF | reportlab | 支持复杂排版 |
| PDF 转图片 | pypdfium2 | 最快，高质量 |
| OCR 扫描件 | pdf2image + pytesseract | 需安装 tesseract |
| PDF → Word | scripts/pdf_to_word.py | 布局简化是正常现象 |
| 压缩 | scripts/compress_pdf.py | gs_ebook 推荐 |
| 图片 → PDF | scripts/images_to_pdf.py | 支持批量 |
| 页面重排/删除 | scripts/reorder_pages.py | |
| AI 摘要/问答 | Agent 直接读取提取文字 | 无需 API |
| 填写可填表单 | scripts/fill_fillable_fields.py | |
| 填写非可填表单 | scripts/fill_pdf_form_with_annotations.py | 视觉验证必须 |
| 修复损坏 PDF | qpdf --fix-qdf | CLI |
| 解密 PDF | pypdf.decrypt() 或 qpdf --decrypt | |

### fields.json 格式（非可填表单）

```json
{
  "pages": [
    { "page_number": 1, "image_width": 2480, "image_height": 3508 }
  ],
  "form_fields": [
    {
      "page_number": 1,
      "description": "姓名填写区",
      "field_label": "姓名",
      "label_bounding_box": [72, 200, 130, 220],
      "entry_bounding_box": [135, 200, 400, 220],
      "entry_text": { "text": "张三", "font_size": 12, "font_color": "000000" }
    }
  ]
}
```

### 常见问题

| 问题 | 原因 | 解决 |
|------|------|------|
| 提取文字乱码/空白 | 扫描件，无文字层 | Phase 6 OCR |
| 表格提取不准 | 无线框表格 | 调整 table_settings 策略 |
| PDF 打不开（加密） | 有密码保护 | 先 decrypt，再操作 |
| 合并后文件很大 | 未压缩 | Phase 8 压缩 |
| OCR 中文不准 | 缺中文语言包 | 安装 tesseract-ocr-chi-sim |
| 表单填写位置偏移 | 坐标系不同 | image坐标 ≠ PDF坐标，用验证图校正 |
| 转Word排版乱 | PDF排版复杂 | 正常现象，告知用户手动调整 |
| 压缩后质量太差 | 策略太激进 | 换 gs_printer 或 gs_prepress |
