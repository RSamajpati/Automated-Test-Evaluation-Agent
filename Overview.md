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
