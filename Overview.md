Data Ingestion Module – Detailed Explanation
The Data Ingestion Module is responsible for collecting, preprocessing, and structuring student answer sheets and model solutions before evaluation. It must handle various input formats (text, PDFs, images, handwritten answers) and convert them into machine-readable text.

1. Core Responsibilities of the Data Ingestion Module
Accepting Inputs

Model Solutions (various correct answers provided by instructors).
Student Answer Sheets (handwritten or typed).
Metadata (Student ID, Exam ID, Question ID, etc.).
Preprocessing Data

Extracting text from different formats (OCR for images/PDFs, direct text parsing).
Cleaning the extracted text (removing noise, correcting spelling).
Normalizing the text (tokenization, lemmatization, stemming).
Storing Preprocessed Data

Organizing extracted text for NLP evaluation.
Storing it in a structured database.
2. Types of Inputs Handled
(a) Text-Based Inputs (Direct Entry / Digital Documents)
JSON / CSV files containing model answers and student responses.
Student answers entered via an online test portal.
PDFs containing digital text (not scanned images).
(b) Scanned Answer Sheets & Images (Handwritten Answers)
Scanned answer sheets (PDFs, JPEGs, PNGs).
Handwritten responses from students.
Images uploaded through an online portal.
(c) Instructor-Provided Model Solutions
Multiple correct answers (short answers, paragraph-based, bullet points).
Reference answers formatted in LaTeX (for math-heavy subjects).

3. Data Extraction Pipeline
Since student answer sheets come in various formats, the Data Extraction Pipeline should be able to handle all of them efficiently.

Step 1: Accepting Model Solutions & Student Answers
API Endpoint to receive input data:
python
Copy
Edit
from fastapi import FastAPI, File, UploadFile
import shutil

app = FastAPI()

@app.post("/upload/")
async def upload_file(file: UploadFile = File(...)):
    with open(f"uploads/{file.filename}", "wb") as buffer:
        shutil.copyfileobj(file.file, buffer)
    return {"filename": file.filename}
Step 2: Extracting Text from Various Formats
(a) Extracting from PDFs (Digital & Scanned)
Digital PDFs (Use pdfplumber to extract text directly).

python
Copy
Edit
import pdfplumber

def extract_text_from_pdf(pdf_path):
    with pdfplumber.open(pdf_path) as pdf:
        text = "\n".join([page.extract_text() for page in pdf.pages if page.extract_text()])
    return text
Scanned PDFs / Images (Use OCR with Tesseract/EasyOCR).

python
Copy
Edit
import pytesseract
from PIL import Image

def extract_text_from_image(image_path):
    text = pytesseract.image_to_string(Image.open(image_path))
    return text
(b) Extracting from Handwritten Responses
Use EasyOCR / PaddleOCR for better recognition of handwritten answers.
python
Copy
Edit
import easyocr

reader = easyocr.Reader(['en'])
text = reader.readtext('handwritten_answer.jpg', detail=0)
print(text)
(c) Extracting from JSON/CSV Files
Model solutions can be provided in JSON format:

json
Copy
Edit
{
    "question_id": "Q1",
    "correct_answers": ["The mitochondria is the powerhouse of the cell.", "Mitochondria generates ATP."]
}
Python function to load JSON:

python
Copy
Edit
import json

def load_model_solutions(json_file):
    with open(json_file, 'r') as file:
        data = json.load(file)
    return data


