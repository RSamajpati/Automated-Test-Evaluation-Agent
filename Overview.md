# **Data Ingestion Module – Detailed Explanation**  

The **Data Ingestion Module** is responsible for collecting, preprocessing, and structuring student answer sheets and model solutions before evaluation. It must handle various input formats (text, PDFs, images, handwritten answers) and convert them into machine-readable text.

---

## **1. Core Responsibilities of the Data Ingestion Module**
1. **Accept Multiple Input Formats**  
   - Text-based answers (plain text, Word files, PDFs)  
   - Handwritten answers (scanned PDFs, images)  
   - Model solutions in structured form (database, JSON, CSV)  

2. **Preprocessing the Data**  
   - Converting all text formats into structured text  
   - Optical Character Recognition (OCR) for images & scanned documents  
   - Removing noise (headers, footers, extra whitespace)  
   - Handling multiple languages if required  

3. **Structuring the Data**  
   - Extracting student responses and mapping them to questions  
   - Associating answers with metadata (student ID, question number)  
   - Storing structured data for evaluation  

---

## **2. Tech Stack for Data Ingestion**
| Component | Technology |
|-----------|------------|
| **OCR (for handwritten answers)** | `Tesseract OCR`, `EasyOCR`, `AWS Textract` |
| **Text Processing** | `NLTK`, `spaCy`, `LangChain`, `OpenAI API` (for semantic understanding) |
| **File Handling** | `PyMuPDF` (PDFs), `pdfminer.six`, `docx` (Word files) |
| **Data Storage** | `MongoDB` (NoSQL for flexibility), `PostgreSQL` (structured DB) |

---

## **3. Workflow of Data Ingestion**
### **Step 1: Data Collection**
- Upload answers via an interface (web app, cloud storage, API)  
- Extract text from PDFs/Word files  
- Use OCR to process handwritten answers  

### **Step 2: Preprocessing & Cleaning**
- Convert text into standard format (lowercasing, removing special chars)  
- Use NLP techniques for sentence tokenization  
- Handle spelling corrections using `SymSpell` or `Hunspell`  

### **Step 3: Structuring & Storage**
- Map answers to corresponding questions  
- Store structured responses in a database  
- Prepare data for the **Evaluation Module**  

---

## **4. Example Implementation**
### **Extracting Text from PDFs**
```python
import pdfplumber

def extract_text_from_pdf(pdf_path):
    text = ""
    with pdfplumber.open(pdf_path) as pdf:
        for page in pdf.pages:
            text += page.extract_text() + "\n"
    return text

pdf_text = extract_text_from_pdf("student_answers.pdf")
print(pdf_text)
```
###Using OCR for Handwritten Text
```python
import pytesseract
from PIL import Image

image_path = "handwritten_answer.jpg"
text = pytesseract.image_to_string(Image.open(image_path))
print(text)
```

##5. Challenges & Solutions
###Challenge	Solution
Handwritten text accuracy	Fine-tune OCR models, use AI-based handwriting recognition (AWS Textract, Google Vision AI)
Different file formats	Use libraries like pdfplumber, docx, and PyMuPDF for text extraction
Multi-language support	Use LangChain, spaCy, and Google Translate API for language processing
Noisy Text	Apply regex and NLP preprocessing to clean data
6. Next Steps
After data ingestion, the next module is Evaluation, where student answers will be compared against model solutions using NLP techniques like semantic similarity and keyword extraction.
