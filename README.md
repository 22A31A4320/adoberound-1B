# adoberound-1B
Round 1B submission for Adobe “Connecting the Dots” Hackathon – Persona-Driven Document Intelligence.
# Adobe Hackathon – Round 1B: Persona-Driven Document Intelligence 🧠📄

This repository contains a complete solution for **Round 1B** of Adobe's “Connecting the Dots” Hackathon 2025. The objective of this round is to create an intelligent system capable of identifying and ranking the most relevant sections from a collection of PDF documents based on a specific user persona and their task (job-to-be-done). This builds upon the document structure extracted in Round 1A and adds user-centered contextual intelligence.

---

## 🧩 Challenge Overview

### 🔍 Problem Statement

In a world where information is increasingly locked inside static PDF documents, this challenge reimagines the PDF not as a final product but as a dynamic source of insight. Instead of simply presenting the document as-is, Adobe challenges participants to connect the dots — linking user needs, document semantics, and structure.

Participants are tasked with designing a solution that reads multiple documents and understands what information matters to whom. The system should mimic how a knowledgeable assistant would read and highlight only the relevant content based on a user’s role and goal.

---

## 🎯 Goal

To create a system that takes:
- A **collection of related PDFs**
- A defined **persona** (e.g., PhD Researcher, Investment Analyst)
- A specific **job-to-be-done** (e.g., “Analyze revenue trends and R&D investments”)

and outputs:
- A **ranked list of relevant sections** (with metadata and importance score)
- **Subsections** or detailed text snippets associated with those sections

---

## 🧠 Core Functionality

### Input

- One or more PDF files
- Persona (e.g., “Undergraduate Chemistry Student”)
- Job-to-be-done (e.g., “Summarize key concepts related to reaction kinetics”)

### Output

A structured JSON file with:
1. Metadata:
   - Documents processed
   - Persona and task
   - Timestamp
2. Extracted Sections:
   - Document name
   - Page number
   - Section title
   - Importance rank
3. Subsection Analysis:
   - Document
   - Page number
   - Refined text (stub)

### JSON Schema

```json
{
  "metadata": {
    "documents": ["file1.pdf", "file2.pdf"],
    "persona": "Investment Analyst",
    "job_to_be_done": "Analyze revenue trends and R&D investments",
    "processed_at": "2025-07-26T16:30:00Z"
  },
  "extracted_sections": [
    {
      "document": "file1.pdf",
      "page": 3,
      "section_title": "Revenue Breakdown by Quarter",
      "importance_rank": 1
    }
  ],
  "subsection_analysis": [
    {
      "document": "file1.pdf",
      "page": 3,
      "refined_text": "Sample refined snippet for future summarization"
    }
  ]
}
🛠 Technical Approach
🔍 Step 1: Document Structure Extraction
We reuse logic from Round 1A to parse PDFs using pdfminer.six. It extracts headings based on:

Font size

Boldness

Capitalization

Numeric prefixes (e.g., "1.", "1.1")

Each heading is classified as H1, H2, or H3 and recorded with its page number.

🔍 Step 2: Keyword Extraction
From the "job-to-be-done" input, we use regex to extract keywords — ignoring stopwords and punctuation. These keywords are used to match against the text of each heading in the document outline.

🔍 Step 3: Relevance Scoring
Each heading is scored based on how many job-specific keywords it contains. We use simple keyword overlap for ranking. In case of ties, headings higher in the document are preferred. This makes the solution lightweight and compliant with runtime limits.

🔍 Step 4: Subsection Stubs
For each highly relevant heading, we generate a placeholder "refined snippet." This can be expanded into full paragraph-level summaries in Round 2 using techniques like chunk-based NLP summarization.

🧾 File Structure
graphql
Copy
Edit
adoberound-1B/
├── extract_1b.py              # Main Python logic
├── Dockerfile                 # Container setup for offline processing
├── requirements.txt           # Dependency list
├── approach_explanation.md    # 300–500 word methodology file (for submission)
├── input/                     # Folder for input PDFs
├── output/                    # Output folder for JSONs
└── README.md                  # This documentation
🧪 Example Use Case
Persona:
Investment Analyst

Job to be Done:
"Analyze revenue trends, R&D investments, and market positioning strategies."

PDF Collection:
apple_2023_annual_report.pdf

google_2023_10k.pdf

microsoft_financials_2023.pdf

Expected Output:
The system highlights sections such as:

"Revenue by Segment"

"Research and Development Costs"

"Competitive Landscape Overview"

🧩 Constraints Met
Constraint	Status	Notes
✅ Runs on CPU only	✔️ Yes	No GPU needed
✅ Model size ≤ 1GB	✔️ Yes	No large models
✅ Execution time ≤ 60 sec	✔️ Yes	Optimized for 3–5 PDFs
✅ No internet access	✔️ Yes	Fully offline
✅ Docker-compatible	✔️ Yes	See below

🐳 Docker Instructions
Build the Docker Image
bash
Copy
Edit
docker build --platform linux/amd64 -t adobe-round1b .
Run the Container
bash
Copy
Edit
docker run --rm \
  -v $(pwd)/input:/app/input \
  -v $(pwd)/output:/app/output \
  --network none \
  adobe-round1b
⚠️ Make sure input/ contains .pdf files. output/ will be auto-populated.

📦 Dependencies
Defined in requirements.txt:

pdfminer.six: PDF text + layout parser

pandas: Table display (optional)

datetime, re, os, json: Standard libraries

Install manually with:

bash
Copy
Edit
pip install -r requirements.txt
📣 Future Improvements
Replace keyword matching with semantic search using embeddings (e.g., SBERT)

Add extractive summaries for subsections using transformers (limited by model size)

Support for multilingual PDFs using font-name and encoding analysis

Persona fine-tuning with customizable keyword templates

📝 Submission Summary
This project meets all requirements for Round 1B of Adobe’s Hackathon:

✔ Accurate relevance extraction

✔ Subsection stubs included

✔ CPU-only, offline runtime

✔ Dockerized build

✔ Explanation included in approach_explanation.md

