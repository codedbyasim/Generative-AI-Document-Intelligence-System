
## Generative AI Document Intelligence System

A complete end-to-end AI-powered system that extracts structured insights (like **Named Entities**, **Summaries**, **Key-Value pairs**) from **unstructured documents** such as PDFs and scanned images.

It combines the power of **OCR**, **NLP**, and **Large Language Models (LLMs)** to turn raw documents into actionable knowledge.

---

## Features

- Upload PDF or image documents
- Detect and extract both **digital** and **image-based** text
- Generate smart **summaries** using BART LLM
- 🏷Extract **Named Entities** (ORG, PERSON, LOCATION, etc.) using BERT NER
- Output text and summary files available for download
- Clean, easy-to-use **Flask web interface**

---

## System Architecture Overview

```bash
                ┌────────────┐
                │  Upload UI │
                └────┬───────┘
                     │
               [Flask Backend]
                     │
     ┌────────────────┴────────────────┐
     │                                 │
[PDF/Image Input]              [Text Analysis]
     │                                 │
 OCR via Tesseract         ┌───────────────┐
 (Image Text)              │ Summarization │  ← LLM (facebook/bart-large-cnn)
 +                         └───────────────┘
 Digital Text via PyMuPDF ┌───────────────┐
 (PDF Text)               │ Named Entities│  ← BERT NER (dslim/bert-base-NER)
                          └───────────────┘
````

---

## Project Structure

```
gen-ai-doc-intelligence/
│
├── app.py                         # Main Flask web server
├── requirements.txt               # All dependencies
├── README.md                      # Documentation
│
├── templates/                     # HTML Pages
│   ├── index.html
│   ├── upload.html
│   ├── about.html
│   ├── project_info.html
│   └── result.html
│
├── ai_engine/                     # Core AI modules
│   ├── ocr_engine.py             # OCR using OpenCV + Tesseract
│   ├── summarizer.py             # LLM summarisation (facebook/bart-large-cnn)
│   └── ner_extractor.py          # Named Entity Recognition (dslim/bert-base-NER)
│
├── pdf_processor/                 # PDF-specific handling
│   ├── pdf_text_handler.py       # Extract both image-based and digital text
│   └── image_preprocessor.py     # Preprocess images to boost OCR quality
│
├── uploads/                      # Uploaded files
└── outputs/                      # Extracted results
```

---

## Module Breakdown (Why & What)

### `app.py` – Flask Web Application

* Acts as the **controller** to connect all modules.
* Routes:

  * `/`: Home page
  * `/upload`: Upload and process files
* Coordinates uploading, processing, and displaying results.

---

### `ocr_engine.py` – Image Text Extraction

* Uses **Tesseract OCR + OpenCV**.
* Preprocessing improves accuracy for noisy or scanned images:

  * Grayscale conversion
  * Denoising
  * Adaptive Thresholding
  * Morphological operations
  * Upscaling low-res images
* Why? Many PDFs or uploads are images (not digital text).

---

### `pdf_text_handler.py` – PDF Text Extraction (Hybrid)

* Uses **PyMuPDF (`fitz`)** to:

  * Extract **digital text** from PDFs
  * Convert **image-based pages** to PNGs for OCR
* Smart fallback: if digital text is short, use OCR instead.
* Why? Some PDFs contain only scanned images; others contain real selectable text.

---

### `image_preprocessor.py`

* Used for enhancing scanned documents before OCR.
* Applies:

  * Sharpening filters
  * Resize/denoise
* Helps clean up blurred or noisy documents for better results.

---

### `summarizer.py` – Generate Summary

* Uses HuggingFace’s `facebook/bart-large-cnn` model.
* Accepts large raw text input and returns a summary.
* Why? To provide a **quick digest** of long documents.

---

### `ner_extractor.py` – Named Entity Recognition

* Uses `dslim/bert-base-NER` model.
* Identifies:

  * Names (PERSON)
  * Organizations (ORG)
  * Locations (LOC), etc.
* Results include: `word`, `entity`, `confidence score`.
* Why? For semantic understanding and search.

---

## ⚙Installation

### 1. Clone the repo

```bash
git clone https://github.com/codedbyasim/Generative-AI-Document-Intelligence-System.git
cd Generative-AI-Document-Intelligence-System
```

### 2. Install requirements

```bash
pip install -r requirements.txt
```

### 3. (Windows Only) Tesseract path

```python
# In ocr_engine.py
pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract.exe'
```

---

## Run Locally

```bash
python app.py
```

* Access at: [http://localhost:5000](http://localhost:5000)

---

## Sample Use Case

Upload a scanned letter →
System extracts text →
LLM summarizes it →
NER shows people/organizations →
Download output files.

---

## License

MIT – Free to use, modify, and improve.

---

## 👤 Author

Built with ❤️ by Asim Hanif
Feel free to connect and contribute!
