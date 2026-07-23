# SuperKart — Hugging Face Deployment Guide

Everything is prepared. You only need a Hugging Face account and ~15 minutes.

## Prerequisites (once)

1. Sign up / log in at https://huggingface.co
2. Create a **Write token**: Profile → Settings → Access Tokens → Create new token → type "Write". Copy it somewhere safe.

## Part 1 — Backend (Flask API, Docker Space)

1. On Hugging Face click **New Space**.
   - Space name: `superkart-sales-backend`
   - License: any (e.g. mit)
   - SDK: **Docker** → Blank template
   - Visibility: Public → **Create Space**
2. Upload the 4 files from `superkart_backend_files/` (Files tab → Add file → Upload files):
   - `app.py`
   - `requirements.txt`
   - `Dockerfile`
   - `superkart_sales_prediction_model_v1_0.joblib`

   (Alternatively run the "Uploading Files to Hugging Face Space (Docker Space)" cell in the notebook: fill in your token + space id, set `RUN_UPLOAD = True`, re-run.)
3. Wait for the Space to build (few minutes). When it shows **Running**, your API is live at:
   `https://<username>-superkart-sales-backend.hf.space`
4. Test it: open the URL — you should see "Welcome to the SuperKart Sales Prediction API!"

## Part 2 — Frontend (Streamlit Space)

1. **Edit `superkart_frontend_files/app.py` first**: replace `<username>` in the `API_BASE` line with your HF username:
   ```python
   API_BASE = "https://<username>-superkart-sales-backend.hf.space"
   ```
2. **New Space** again:
   - Space name: `superkart-sales-frontend`
   - SDK: **Streamlit**
   - Public → Create Space
3. Upload from `superkart_frontend_files/`:
   - `app.py`
   - `requirements.txt`
4. When it builds, open the Space — fill the form and click **Predict Sales**, or upload a CSV (you can test with `SuperKart.csv` minus the target column) for batch predictions.

## Part 3 — Finish the notebook

Paste your two Space links into the notebook's markdown cells:

- `https://huggingface.co/spaces/<username>/superkart-sales-backend`
- `https://huggingface.co/spaces/<username>/superkart-sales-frontend`

Also fill your token/space ids in the two upload cells and set `RUN_UPLOAD = True` if you want the upload done from the notebook itself (this is what the rubric's "Upload the files to the space" step expects to see).

## Troubleshooting

- Backend build fails → check the Space logs tab; most common cause is a missing file.
- Frontend shows "Could not reach the prediction API" → backend Space is sleeping (open its URL once to wake it) or `API_BASE` has a typo.
- 500 errors on predict → input JSON keys must match the training feature names exactly (see `app.py`).
