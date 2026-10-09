# Water Supply & Distribution Process Explainer Bot

**Project 26 · Water Utilities / Municipal Services**

A Streamlit chatbot that explains drinking-water treatment, distribution, common pressure factors and conservation in plain language. It uses Google's official `google-genai` SDK and Gemini Flash. The chatbot is informational only: it cannot predict local supply times, submit complaints, access live municipal data or operate infrastructure.

## What the app does

- Provides a chat interface with session-based conversation history, loading feedback and a clear-chat button.
- Offers four sample questions covering treatment, distribution, conservation and water pressure.
- Uses Gemini to provide concise, public-friendly explanations, with deterministic replies for requests to predict timings, register service requests or operate infrastructure.
- Explains that treatment stages vary by source and facility; no map or live municipal integration is used.

The former sensor dashboard, demand forecaster, leak detection, alerting, simulators and operational backend remain in `smart_water_system/` for reference, but are not imported or exposed by the main `app.py` workflow. The root `requirements.txt` lists only dependencies needed to run the chatbot.

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

## Project layout and retained source

```text
app.py                          # Primary Streamlit explainer chatbot
requirements.txt                # Streamlit and google-genai
.gitignore                      # Excludes .streamlit/secrets.toml
smart_water_system/             # Legacy prototype retained, not used by app.py
```

The older prototype includes monitoring, forecasting, leak detection, alerting, simulated sensor data and operational endpoints. Those capabilities are not part of this explanation-only application's user flow. No source files were deleted.

## Demo checklist

Try the four suggested prompts, then try:

- “When will water arrive in my area tomorrow?” — the bot must not predict a time.
- “Register a complaint about low water pressure.” — the bot must not submit a request.
- A question unrelated to water — the bot should explain its scope.
- Clear chat — previous messages should disappear.

Gemini-dependent responses require a working API key and network access. API errors are shown as friendly messages without raw provider details. The app does not claim to have been deployed or connected to a municipal data source.

