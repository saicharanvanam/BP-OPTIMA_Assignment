# PDF Invoice Extraction Pipeline

## Overview

This project implements a small pipeline that accepts a PDF document
(invoice, statement, or form) and extracts structured information into JSON format.

The solution does NOT use any LLMs, as per the requirement.
It uses rule-based and layout-based extraction techniques.


## Approach

The pipeline works in the following stages:

### 1. PDF Text Extraction
- Used `pdfplumber` to extract word-level text along with positional coordinates (x, y).
- This allows layout-aware parsing instead of simple plain-text extraction.

### 2. Row Detection
- Words are grouped into rows using Y-coordinate clustering.
- A tolerance value is used to handle minor misalignments.

### 3. Column Detection (Table Without Lines)
Since the table does not contain vertical or horizontal grid lines:
- Column boundaries are detected using X-coordinate clustering.
- Words are assigned to the nearest column center.
- This reconstructs the table structure purely based on alignment.

### 4. Field Extraction
- Invoice number, date, and total amount are extracted using regex patterns.
- Full text is scanned for relevant keywords.

### 5. JSON Output
- Extracted fields and table rows are structured into JSON format.
- Output is saved to `output.json`.


## Technologies Used

- Python 3.x
- pdfplumber
- re (Regex)
- json
- collections (defaultdict)
