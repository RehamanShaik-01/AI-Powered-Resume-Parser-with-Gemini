# AI-Powered-Resume-Parser-with-Gemini

import streamlit as st
import fitz  # PyMuPDF for PDF
import docx  # python-docx for DOCX
import google.generativeai as genai
import json
import re


genai.configure(api_key="AIzaSyA3Ky4mY8ynjeBaYmHP__udfsqNhZIvgKg") 



model = genai.GenerativeModel("gemini-2.0-flash")


st.set_page_config(page_title="Resume Parser with Gemini", layout="centered")
st.title("📄 AI Resume Parser (Gemini)")


def extract_text_from_pdf(uploaded_file):
    text = ""
    with fitz.open(stream=uploaded_file.read(), filetype="pdf") as doc:
        for page in doc:
            text += page.get_text()
    return text

def extract_text_from_docx(uploaded_file):
    doc = docx.Document(uploaded_file)
    return '\n'.join([para.text for para in doc.paragraphs])


def clean_json_output(text):
    cleaned = re.sub(r"```(?:json)?", "", text, flags=re.IGNORECASE)
    return cleaned.strip("`\n ")


def parse_resume_with_gemini(resume_text):
    prompt = f"""
You are a resume parsing AI.

Extract the following fields from the resume and return ONLY a valid JSON object.

Fields to extract:
- full_name
- email
- phone_number
- linkedin
- skills (as a list)
- education (as a list of institutions and degrees)
- work_experience (as a list of job titles and companies)
- certifications (as a list)
- projects (as a list)

Resume:
\"\"\"
{resume_text}
\"\"\"

⚠️ Instructions:
- Return ONLY a valid JSON object.
- Use double quotes for all keys and string values.
- Do NOT include explanations, markdown, or formatting.
- Exclude any null or missing fields.
"""
    response = model.generate_content(prompt)
    return response.text.strip()


uploaded_file = st.file_uploader("📤 Upload your Resume (.pdf or .docx)", type=["pdf", "docx"])

if uploaded_file:
    if uploaded_file.name.endswith(".pdf"):
        resume_text = extract_text_from_pdf(uploaded_file)
    elif uploaded_file.name.endswith(".docx"):
        resume_text = extract_text_from_docx(uploaded_file)
    else:
        st.error("❌ Unsupported file type.")
        st.stop()

    with st.expander("📄 Extracted Resume Text"):
        st.text(resume_text[:3000]) 

    if st.button("🚀 Parse Resume"):
        with st.spinner("Parsing resume with Gemini..."):
            try:
                parsed_output = parse_resume_with_gemini(resume_text)
                cleaned_output = clean_json_output(parsed_output)

                try:
                    json_data = json.loads(cleaned_output)
                    st.success("✅ Resume parsed successfully!")
                    st.json(json_data)
                except json.JSONDecodeError:
                    st.warning("⚠️ Gemini returned invalid JSON. Showing raw response:")
                    st.text(parsed_output)

            except Exception as e:
                st.error(f"❌ Error: {e}")
