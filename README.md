# Water Supply & Distribution Process Explainer Bot

**Project 26 · Water Utilities / Municipal Services**

A Streamlit chatbot that explains drinking-water treatment, distribution, common pressure factors and conservation in plain language. It uses Google's official `google-genai` SDK and Gemini Flash. The chatbot is informational only: it cannot predict local supply times, submit complaints, access live municipal data or operate infrastructure.

## What the app does

- Provides a chat interface with session-based conversation history, loading feedback and a clear-chat button.
- Offers four sample questions covering treatment, distribution, conservation and water pressure.
- Uses Gemini to provide concise, public-friendly explanations, with deterministic replies for requests to predict timings, register service requests or operate infrastructure.
- Explains that treatment stages vary by source and facility; no map or live municipal integration is used.

The original sensor dashboard, demand forecaster, leak detection, alerting and simulator features are preserved and available as **Existing Dashboard** in the Streamlit navigation. The explainer bot is the home page. The older backend and simulator source in `smart_water_system/` remains available as well.

## Run locally

Requires Python 3.11+ and Git.

```bash
git clone https://github.com/Kommera-Harihansika/ai-smart-water-management-iot.git
cd ai-smart-water-management-iot
python -m venv .venv
```

Activate the environment, then install the app dependencies:

```powershell
# Windows PowerShell
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

```bash
# macOS / Linux
source .venv/bin/activate
python -m pip install -r requirements.txt
```

### Configure the Gemini API key

Create a key in [Google AI Studio](https://aistudio.google.com/app/apikey). This app calls the standard Gemini Developer API without paid-only tools or grounding. Google lists a free tier for Gemini 3.8 Flash, but availability and request limits depend on the project and are subject to Google's current quota and pricing terms. No key is included in this repository.

For local development, create `.streamlit/secrets.toml` (the file is ignored by Git):

```toml
WATER_GEMINI_API_KEY = "paste-your-key-here"
```

Alternatively, set `WATER_GEMINI_API_KEY` in the shell before launching Streamlit. `GEMINI_API_KEY` is also accepted as a compatibility fallback.

```powershell
$env:WATER_GEMINI_API_KEY = "paste-your-key-here"
```

Run the app from the repository root:

```bash
streamlit run app.py
```

Open the local URL printed by Streamlit (usually `http://localhost:8501`). Without a key, the app still starts and explains how to configure one; Gemini answers require a valid key.

## Deploy with Streamlit Community Cloud

1. Push the feature branch to GitHub and open a pull request. Merge it after review.
2. In Streamlit Community Cloud, create an app from the repository and select `main`, `app.py`.
3. In the app's **Settings → Secrets**, add:

   ```toml
   WATER_GEMINI_API_KEY = "your-key"
   ```

4. Save and allow the app to restart. Test the four suggested questions and verify restricted requests receive a limitation response.

Do not commit API keys, local secrets files, or screenshots/logs containing credentials. `.streamlit/secrets.toml` is excluded by `.gitignore`.

## Safety and scope

- No complaint or service-request submission, booking, account changes or infrastructure controls.
- No predicted or invented local supply schedules.
- No claim of access to live sensor readings, utility records or municipal databases.
- General pressure information only; for a specific interruption, pressure problem or water-safety question, consult the official utility or relevant public-health authority.
- Inputs requesting out-of-scope actions are screened before an API call. The model also receives a detailed system instruction; as with any generative model, it can make mistakes.

## Project layout

```text
app.py                          # Primary Streamlit explainer chatbot
pages/2_Existing Dashboard.py   # Preserved original monitoring dashboard
requirements.txt                # Original dependencies plus google-genai
.gitignore                      # Excludes .streamlit/secrets.toml
smart_water_system/             # Existing backend, models and simulator
```

The chatbot does not access the dashboard's local demo data, make municipal claims, or operate infrastructure. The existing dashboard remains a separate demonstration workflow and labels its readings as local demo/simulator data.

## Demo checklist

Try the four suggested prompts, then try:

- “When will water arrive in my area tomorrow?” — the bot must not predict a time.
- “Register a complaint about low water pressure.” — the bot must not submit a request.
- A question unrelated to water — the bot should explain its scope.
- Clear chat — previous messages should disappear.

Gemini-dependent responses require a working API key and network access. API errors are shown as friendly messages without raw provider details. The app does not claim to have been deployed or connected to a municipal data source.

