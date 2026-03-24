---
name: markitdown
description: "Use when converting files or office documents to Markdown format. Converts PDF, DOCX, PPTX, XLSX, images (OCR), audio (transcription), HTML, CSV, JSON, XML, ZIP, YouTube URLs, and EPubs into clean, token-efficient Markdown using Microsoft's MarkItDown library."
allowed-tools: "Read, Write, Edit, Bash"
license: MIT
source: https://github.com/microsoft/markitdown
---

# MarkItDown - File to Markdown Conversion

Convert documents into clean, structured Markdown optimized for LLM processing. Supports 15+ file formats with optional AI-enhanced image descriptions and OCR.

## Workflow

1. **Install** the library with required format support
2. **Convert** the target file(s) to Markdown
3. **Validate** output quality and structure
4. **Post-process** if needed (clean whitespace, strip metadata)

## Supported Formats

| Format | Notes |
|--------|-------|
| PDF | Full text extraction; use Azure Doc Intel for complex layouts |
| DOCX | Tables and formatting preserved |
| PPTX | Slides with notes; AI image descriptions available |
| XLSX / CSV | Outputs Markdown tables |
| Images (JPEG, PNG, GIF, WebP) | EXIF metadata + OCR |
| Audio (WAV, MP3) | Metadata + speech transcription |
| HTML | Clean conversion |
| JSON / XML | Structured representation |
| ZIP | Iterates and converts contents |
| EPUB | Full text extraction |
| YouTube URLs | Fetches video transcriptions |

## Installation

```bash
# All formats
pip install 'markitdown[all]'

# Specific formats only
pip install 'markitdown[pdf,docx,pptx]'
```

## Usage

### CLI

```bash
markitdown document.pdf -o output.md
```

### Python API

```python
from markitdown import MarkItDown

md = MarkItDown()
result = md.convert("document.pdf")
print(result.text_content)
```

### Stream-based conversion

```python
with open("large_file.pdf", "rb") as f:
    result = md.convert_stream(f, file_extension=".pdf")
```

### AI-enhanced image descriptions

```python
from openai import OpenAI

client = OpenAI(api_key="key", base_url="https://openrouter.ai/api/v1")
md = MarkItDown(
    llm_client=client,
    llm_model="anthropic/claude-sonnet-4.5",
    llm_prompt="Describe this image in detail"
)
result = md.convert("presentation.pptx")
```

### Azure Document Intelligence (complex PDFs)

```python
md = MarkItDown(docintel_endpoint="<endpoint>")
result = md.convert("complex_document.pdf")
```

### Batch conversion

```python
from pathlib import Path

md = MarkItDown()
for pdf in Path("papers/").glob("*.pdf"):
    result = md.convert(str(pdf))
    Path(f"output/{pdf.stem}.md").write_text(result.text_content)
```

## Validation Checklist

- Converted output contains expected content sections
- Tables render correctly in Markdown
- OCR text is legible (install `tesseract` if not: `brew install tesseract`)
- No excessive whitespace (clean with `re.sub(r'\n{3,}', '\n\n', text)`)

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Missing format support | `pip install 'markitdown[pdf]'` for the specific format |
| Binary file errors | Open files in binary mode (`"rb"`) for `convert_stream` |
| OCR not working | Install tesseract: `brew install tesseract` (macOS) or `apt-get install tesseract-ocr` |
| Slow large PDFs | Use `convert_stream` for memory efficiency |

## Resources

- [MarkItDown GitHub](https://github.com/microsoft/markitdown)
- [PyPI](https://pypi.org/project/markitdown/)
- Plugins: search GitHub for `#markitdown-plugin`
- MCP Server: `markitdown-mcp` for Claude Desktop integration
