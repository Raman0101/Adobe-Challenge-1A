# PDF Outline Extractor - Team - TechnoidX

A blazing-fast, robust PDF outline extractor built for **Adobe India Hackathon 2025 - Challenge 1a**. This solution extracts a clean and structured **document title** and **table of contents (outline)** including H1, H2, and H3 headings from PDF files and outputs JSON files — ready for downstream tasks like semantic search and intelligent summarization.

---

## 🔍 What This Does

This script processes all PDFs in the `/input` directory and extracts:

- The **document title**
- A hierarchical **outline**:
  - `text`: heading text
  - `level`: heading level (H1, H2, H3)
  - `page`: page number (1-indexed)

It outputs a corresponding `.json` for each input PDF in the `/output` directory, conforming to the required schema.

---

## 🚀 Approach

- Uses `pdfplumber` to extract individual characters with layout, font, size, and position.
- **Heading levels** are inferred using:
  - Font size buckets (relative font size logic)
  - Boldness from font name (e.g., contains 'Bold', 'Heavy')
  - Position-based heuristics (e.g., top 25% of page)
- **Title detection** logic uses the first H1 candidate that is:
  - On page 1
  - Within top 25% of the page height
  - Center-aligned text
- Cleans noisy OCR patterns like headers/footers, timestamps, and page numbers.
- Multilingual-safe via Unicode normalization.
- JSON output strictly follows the schema provided.

---

## 📁 Folder Structure

```
├── Dockerfile
├── README.md
├── requirements.txt
├── process_pdfs.py
├── parser.py
├── json_generator.py
├── input/
│   └── *.pdf
├── output/
│   └── *.json
├── output_schema.json
```

---

## 📊 Output Format

```json
{
  "title": "Document Title",
  "outline": [
    { "level": "H1", "text": "Chapter 1", "page": 1 },
    { "level": "H2", "text": "Section 1.1", "page": 2 },
    { "level": "H3", "text": "Subsection 1.1.1", "page": 2 }
  ]
}
```

---

## ⚙️ Build and Run

### 🔨 Step 1: Build Docker Image

```bash
docker build -t adobe-challenge-1a .
```

### 🚀 Step 2: Run the Container

```bash
docker run --rm \
  -v "$(pwd)/input:/app/input" \
  -v "$(pwd)/output:/app/output" \
  --network none \
  adobe-challenge-1a
```

This will process all PDFs in `/input` and save `.json` outputs to `/output`.

---

## 🎓 Local Development (Optional)

Install dependencies locally using:

```bash
pip install -r requirements.txt
```

---

## 🧩 Requirements

**requirements.txt**
```
pdfplumber
```

---

## ✨ Highlights

- ✅ Accurate heading detection using font size & style heuristics
- ✅ Multilingual-safe via Unicode normalization (e.g., CJK, Devanagari)
- ✅ Filters OCR noise, headers, footers, and spurious lines
- ✅ Supports both CLI & Docker use cases
- ✅ Follows schema strictly and handles multiple PDFs in batch
- ✅ Optimized for CPU-only processing

---

## 🚫 Known Limitations

- Purely scanned/image PDFs (without embedded text) are not supported
- Multi-column layouts are flattened into a linear reading order
- Not all fonts/styles are handled exhaustively — edge cases may require tuning

---

## 💪 Bonus-Readiness (For Challenge 1B)

- Extracted structure is cleanly nested and modular
- Unicode-safe, language-agnostic logic
- Easily extendable for persona-based content filtering

---

## 📖 Example Output

From a PDF titled **South of France - Cities**:

```json
{
  "title": "South of France - Cities",
  "outline": [
    { "level": "H1", "text": "Nice", "page": 1 },
    { "level": "H2", "text": "Promenade des Anglais", "page": 2 },
    { "level": "H1", "text": "Avignon", "page": 3 }
  ]
}
```

---

## 👨‍💼 Author

- **Raman Kumar**  
  GitHub: [@Raman0101](https://github.com/Raman0101)

- **Ram Samujh Singh**  
  GitHub: [@Ramsamujhsingh70](https://github.com/Ramsamujhsingh70)

- **Raman Kumar**  
  GitHub: [@dubeyrishabh123](https://github.com/dubeyrishabh123)
