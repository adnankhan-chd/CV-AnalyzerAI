# 📄 AI Resume ATS Analyzer

A Streamlit app that scores a resume for Applicant Tracking System (ATS) compatibility and
tells you exactly how to improve it, powered by Google's Gemini Flash models.

## Features

- Upload a resume as **PDF, DOCX or TXT** (up to 5 MB)
- Optional **job description** input for targeted keyword analysis
- **ATS score out of 100** with a five-category breakdown
- **Prioritized improvements** (High / Medium / Low)
- **Matched and missing keywords**
- **Before/after rewrite examples** taken from your own resume
- Automatic checks for email, phone, LinkedIn, word count and section headings
- Downloadable Markdown report

## How the score works

The AI rates each category from 0 to 100. The overall score is then calculated **in code**
(not by the AI) as a weighted average, so it is consistent and cannot be talked up by text
hidden inside a resume.

| Category                    | Weight |
|-----------------------------|--------|
| Keyword match               | 25%    |
| Achievements & impact       | 25%    |
| ATS-friendly formatting     | 20%    |
| Section completeness        | 15%    |
| Language & clarity          | 15%    |

> This is an estimate. Every real ATS parses resumes differently, so treat the score as
> guidance, not a guarantee.

## Project structure

```
ats-resume-analyzer/
├── app.py            # Streamlit app + Gemini logic
├── requirements.txt  # Python dependencies
└── README.md
```

## Run locally

1. **Get a Gemini API key** (free tier available): https://aistudio.google.com/apikey

2. **Create a virtual environment and install dependencies**

   ```bash
   python -m venv venv
   source venv/bin/activate        # Windows: venv\Scripts\activate
   pip install -r requirements.txt
   ```

3. **Provide your API key** using either option:

   - Create `.streamlit/secrets.toml`:

     ```toml
     GEMINI_API_KEY = "your-key-here"
     # GEMINI_MODEL = "gemini-3.6-flash"   # optional override
     ```

   - or set an environment variable (`export GEMINI_API_KEY=...`)
   - or paste the key into the app's sidebar at runtime

4. **Start the app**

   ```bash
   streamlit run app.py
   ```

   It opens at http://localhost:8501.

## Changing the Gemini model

Gemini model names change over time. If you see *"Model not found"*, either edit the
**Gemini model** field in the sidebar, set `GEMINI_MODEL` in your secrets, or change
`DEFAULT_MODEL` at the top of `app.py`. See Google's current list at
https://ai.google.dev/gemini-api/docs/models

## Deploy on Streamlit Community Cloud

1. Push this project to a **GitHub repository** (see below).
2. Go to https://share.streamlit.io and sign in with GitHub.
3. Click **Create app**, choose your repository and branch, and set the main file to `app.py`.
4. Open **Advanced settings → Secrets** and paste:

   ```toml
   GEMINI_API_KEY = "your-key-here"
   ```

5. Click **Deploy**. After a minute or two you get a public `*.streamlit.app` URL.

## Push to GitHub

```bash
cd ats-resume-analyzer
printf "venv/\n.venv/\n__pycache__/\n.env\n.streamlit/secrets.toml\n" > .gitignore
git init
git add .
git commit -m "Initial commit: ATS resume analyzer"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

Create the empty repository on https://github.com/new first (do not tick "Add a README").
**Never commit your API key**; the `.gitignore` above keeps `secrets.toml` out of Git.

## Troubleshooting

| Problem | Fix |
|---|---|
| "Very little text could be extracted" | The PDF is a scanned image. Export a text-based PDF or DOCX. |
| "API key was rejected" | Re-copy the key from AI Studio; make sure there are no spaces. |
| "Model not found" | Update the model name (see above). |
| "Rate limit or quota reached" | Wait a minute, or use a key with higher quota. |
| Streamlit Cloud build fails | Confirm the file is named exactly `requirements.txt`. |

## Privacy

Resume text is sent to Google's Gemini API for analysis. The app does not store uploaded
files or results on disk; results exist only in your browser session. Do not deploy this
publicly with your own API key unless you are comfortable with others using your quota.
