# Resume ATS Checker

A small Streamlit app that extracts text from a resume and uses Google's Gemini Flash model to provide an **AI-generated ATS score estimate**, strengths, missing keywords, formatting checks, and improvement suggestions.

> **Important:** The score is an estimate, not a real result from an employer's Applicant Tracking System (ATS). ATS tools differ, and this app cannot guarantee interviews or hiring outcomes.

## Features

- Upload PDF, DOCX, or TXT resumes (up to 5 MB).
- Optionally paste a job description for more relevant keyword feedback.
- Estimated score from 0 to 100.
- Resume strengths, improvement suggestions, potential missing keywords, formatting checks, and priority actions.
- Download the review as JSON.
- API key is read from Streamlit secrets or the `GEMINI_API_KEY` environment variable.

## Files

- `app.py` — Streamlit app.
- `requirements.txt` — Python dependencies.
- `README.md` — setup and deployment instructions.

## 1. Get a Gemini API key

Create an API key in Google AI Studio: https://aistudio.google.com/app/apikey

Keep the key private. Never paste it directly into `app.py` or commit it to GitHub.

## 2. Run locally on Windows

Install a current supported Python version from https://www.python.org/downloads/ if needed. During setup, enable the option to add Python to PATH.

Open the project folder in File Explorer, click the address bar, type `cmd`, and press Enter. Then run:

```bash
python -m venv .venv
.venv\Scripts\activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Create a folder named `.streamlit` inside the project folder. Inside it, create a file named `secrets.toml` containing:

```toml
GEMINI_API_KEY = "paste_your_real_key_here"
```

Replace the example value with your key. Do not upload `.streamlit/secrets.toml` to GitHub.

Start the app:

```bash
streamlit run app.py
```

Your browser should open the local app. If it does not, open the local URL shown in the terminal (usually `http://localhost:8501`).

## 3. Push to GitHub using the website UI

1. Sign in at https://github.com/.
2. Click **+** in the top-right corner, then **New repository**.
3. Give the repository a name, such as `resume-ats-checker`. Choose Public or Private, then click **Create repository**.
4. On the new repository page, choose **Add file → Create new file**.
5. Name the file `app.py`, paste the contents of your local `app.py`, and commit the file.
6. Repeat **Add file → Create new file** for `requirements.txt` and `README.md`. Paste each file's contents and commit.
7. Make sure you have **not** uploaded your API key or `.streamlit/secrets.toml`. The repository should contain only the app code, requirements, and README to start.

Tip: You can also upload the three files together using **Add file → Upload files**, then choose **Commit changes**.

## 4. Deploy on Streamlit Community Cloud

1. Visit https://share.streamlit.io/ and sign in using GitHub.
2. Click **Create app** and select your GitHub repository.
3. Set the branch to `main` (or whichever branch contains your files) and the main file path to `app.py`.
4. Open **Advanced settings** before deploying. In **Secrets**, enter:

   ```toml
   GEMINI_API_KEY = "paste_your_real_key_here"
   ```

5. Choose a supported Python version if prompted, then click **Deploy**.
6. Watch the build logs. Once it finishes, open the generated `*.streamlit.app` URL.
7. If it fails, open the app's **Manage app** / logs view and check the error. Verify that `requirements.txt` is in the repository root, the entrypoint is `app.py`, and the secret name is exactly `GEMINI_API_KEY`.

Streamlit's official deployment and secrets documentation:
- https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy
- https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app/secrets-management

## 5. Basic checks before publishing

- Test with a text-based PDF, a DOCX, and a TXT resume.
- Test without a job description and with a job description.
- Try an empty file and a scanned/image-only PDF; the app should explain that no text was found.
- Confirm the API key is configured before analysis.
- Never use a real person's resume without permission. Resume text is sent to Google's Gemini API for processing, so consider privacy and consent before using this with other people.

## Known limitations

- Image-only scanned PDFs are not OCR'd by this app.
- A 0–100 score is an AI estimate and can vary between runs; it is not an official ATS score.
- Keyword recommendations are only as useful as the job description provided.
- The app does not store resumes in a database, but Streamlit session state keeps the generated review during the session. The resume text is sent to Gemini when the Analyze button is clicked.
- Before using this as a public service, add a clear privacy notice, rate limits, and stronger error logging/monitoring.
