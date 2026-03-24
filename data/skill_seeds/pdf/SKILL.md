---
name: pdf
description: "Comprehensive PDF manipulation toolkit for extracting text and tables, creating new PDFs, merging/splitting documents, and handling forms. Use when processing, generating, analyzing, or manipulating PDF documents, filling PDF forms, extracting text or tables from PDFs, merging or splitting PDF files, adding watermarks, or performing OCR on scanned PDFs."
---

# PDF Processing Guide

Essential PDF processing operations using Python libraries and command-line tools. For advanced features and JavaScript libraries, see `reference.md`. For PDF form filling, read `forms.md` and follow its instructions.

## Workflow

1. **Identify the task** - Extract text/tables, create, merge/split, fill forms, OCR, or manipulate
2. **Select the tool** - Use Quick Reference table below to match task to best library
3. **Implement** - Follow the code patterns below
4. **Validate** - Verify output opens correctly and content is preserved

## Quick Reference

| Task | Best Tool | Key API |
|------|-----------|---------|
| Read/merge/split/rotate | pypdf | `PdfReader`, `PdfWriter` |
| Extract text with layout | pdfplumber | `page.extract_text()` |
| Extract tables | pdfplumber | `page.extract_tables()` |
| Create new PDFs | reportlab | `Canvas` or `SimpleDocTemplate` |
| OCR scanned PDFs | pytesseract + pdf2image | `convert_from_path()` → `image_to_string()` |
| Fill PDF forms | See `forms.md` | pypdf or pdf-lib (JS) |
| CLI merge/split/rotate | qpdf | `qpdf --empty --pages ...` |

## Python Libraries

### pypdf - Read, Merge, Split, Rotate, Encrypt

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("document.pdf")
text = "".join(page.extract_text() for page in reader.pages)

# Merge: writer.add_page(page) for each page from multiple readers
# Split: one PdfWriter per page, write to separate files
# Rotate: page.rotate(90) before adding to writer
# Encrypt: writer.encrypt("userpassword", "ownerpassword")
# Watermark: page.merge_page(watermark_page) before adding
```

### pdfplumber - Text and Table Extraction

```python
import pdfplumber

with pdfplumber.open("document.pdf") as pdf:
    for page in pdf.pages:
        text = page.extract_text()
        tables = page.extract_tables()  # Returns list of list-of-lists
```

Convert tables to DataFrames: `pd.DataFrame(table[1:], columns=table[0])`

### reportlab - Create PDFs

```python
from reportlab.lib.pagesizes import letter
from reportlab.platypus import SimpleDocTemplate, Paragraph, PageBreak
from reportlab.lib.styles import getSampleStyleSheet

doc = SimpleDocTemplate("report.pdf", pagesize=letter)
styles = getSampleStyleSheet()
story = [Paragraph("Title", styles['Title']), Paragraph("Body text", styles['Normal'])]
doc.build(story)
```

For simple single-page PDFs, use `canvas.Canvas` directly.

## Command-Line Tools

```bash
# pdftotext (poppler-utils) - extract text
pdftotext -layout input.pdf output.txt       # Preserve layout
pdftotext -f 1 -l 5 input.pdf output.txt     # Pages 1-5

# qpdf - merge, split, rotate, decrypt
qpdf --empty --pages file1.pdf file2.pdf -- merged.pdf
qpdf input.pdf --pages . 1-5 -- pages1-5.pdf
qpdf input.pdf output.pdf --rotate=+90:1

# pdfimages (poppler-utils) - extract images
pdfimages -j input.pdf output_prefix
```

## OCR for Scanned PDFs

```python
# Requires: pip install pytesseract pdf2image
from pdf2image import convert_from_path
import pytesseract

images = convert_from_path('scanned.pdf')
text = "\n\n".join(pytesseract.image_to_string(img) for img in images)
```

## Reference Files

- **reference.md** - Advanced pypdfium2, JavaScript (pdf-lib), troubleshooting
- **forms.md** - PDF form filling instructions and workflows
