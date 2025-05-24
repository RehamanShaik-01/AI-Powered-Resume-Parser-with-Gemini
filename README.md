# AI-Powered-Resume-Parser-with-Gemini

Project Summary
This project is a smart, AI-driven Resume Parser that extracts essential candidate information from resumes in PDF or DOCX format using Google Gemini (Generative AI) and presents it in structured JSON format via an intuitive Streamlit web interface. It automates the manual process of screening and analyzing resumes, making it ideal for HR professionals and recruitment software tools.
Objectives
- To develop an intelligent system that reads resumes and extracts relevant information like name, email, phone number, skills, education, and work experience.
- To leverage a large language model (LLM) to understand varied resume formats and accurately parse them.
- To present the parsed data in a clean, structured JSON format for further automation or integration.
Technologies Used
Technology	Purpose
Python	Core programming language
Streamlit	Web interface for user interaction
PyMuPDF (fitz)	Reading and extracting text from PDFs
python-docx	Extracting text from DOCX files
Google Gemini API	Large Language Model for parsing logic
JSON	Data format for structured resume output
Key Features
•	📤 File Upload Support: Accepts both PDF and DOCX resume formats.
•	🤖 LLM-Powered Parsing: Uses Google Gemini to understand and extract fields even from unstructured or differently formatted resumes.
•	🧠 Smart Field Detection: Automatically identifies and extracts: Full Name, Email, Phone Number, LinkedIn URL, Skills, Education, Work Experience, Certifications, Projects.
•	⚙️ Data Structuring: Converts unstructured text into structured JSON, suitable for backend pipelines or databases.
•	🧹 Error Handling: Cleans invalid Gemini output and shows fallback output if parsing fails.
•	🖥️ User-Friendly Interface: Built using Streamlit for fast deployment and testing.
Future Scope
- Integration with Applicant Tracking Systems (ATS).
- Export parsed data to Excel/CSV/Database.
- Add support for bulk resume uploads and analytics (e.g., skill match percentage).



