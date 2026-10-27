---
name: indra-ocr
description: Convert PDFs and images (scans, photos, handwriting, 80+ languages including Arabic and French) into Markdown, plain text or JSON with boxes, on CPU, using the open-source Indra OCR tool from Datalab (github.com/datalab-to/indra). Use when the user wants to extract or read the text of a scanned PDF, an image, a photo of a document, or wants a document turned into Markdown/text/JSON.
---

# Indra OCR

Small OCR (about 1M parameters for the main recognizer) that runs on CPU with ONNX Runtime — no GPU, no PyTorch.
Source: https://github.com/datalab-to/indra — code Apache 2.0; model weights under a modified OpenRAIL-M license
(free for research, personal use and startups under $20M; not allowed in competition with Datalab's API).

## Install

```shell
pip install indra-ocr          # Python 3.10+; installs onnxruntime, opencv, pypdfium2
```

The first run downloads the models (about 76 MB) from Hugging Face (`datalab-to/indra-ocr`).
Offline: set `INDRA_OCR_MODEL_DIR` or pass `--model-dir`.

## CLI

```shell
indra-ocr doc.pdf                         # Markdown to stdout
indra-ocr *.pdf --out results/            # one file per input
indra-ocr scan.png --format json          # lines, boxes, layout blocks, confidences
indra-ocr doc.pdf --format text           # plain text, one line per detected line
indra-ocr doc.pdf --mode ocr              # ignore the PDF text layer, OCR everything
indra-ocr doc.pdf --math                  # math as LaTeX (slower)
indra-ocr doc.pdf --keep-furniture        # keep page headers/footers
indra-ocr-viewer --port 8765              # local web viewer
```

Useful options: `--format md|text|json`, `--mode auto|ocr|text`, `--cores N`, `--dpi` (default 200),
`--max-pages N`, `--password`, `--no-image-ocr`, `--timings`. Exits 1 if any file failed.

## Python

```python
import indra_ocr

conv = indra_ocr.load(threads=4)                 # mode="auto" | "ocr" | "text", math=False
for i, page in conv.convert("doc.pdf"):          # PDF, PNG, JPEG, TIFF, BMP, WebP
    print(page.markdown)                         # also page.text, page.lines, page.blocks
```

In-memory image: `conv.ocr(gray_uint8_numpy_array)`.
Many files: `indra_ocr.convert_many([...])` inside an `if __name__ == "__main__":` guard.

Each `Page` has `markdown`, `text`, `lines` (`Line(box, text, conf)`), `blocks` (`{label, bbox, text}`),
`source` (`text` | `ocr` | `hybrid` | `error`), `rotation`, `skew`, `vertical`, `timings`, `note`.

## Limits (tell the user when they apply)

- Complex tables (merged cells, no rules) can come out with cells misplaced; simple grids are fine.
- Handwriting reads poorly (about 25% on the project's benchmark).
- Old, faded or low-resolution scans read poorly.
- Some Latin languages with many diacritics (Czech, Turkish, Lithuanian…) can get wrong accents; French, English, German, Spanish are not affected.
- Arabic/Persian/Hebrew are supported (right-to-left order kept).

For these hard cases the project points to Datalab's paid platform (Chandra model).
