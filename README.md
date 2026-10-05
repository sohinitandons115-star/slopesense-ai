# SlopeSense AI

SlopeSense AI is a landslide-risk monitoring and emergency-response prototype for communities and disaster-response teams in Northeast India. It combines a Streamlit dashboard, a FastAPI backend, a trained Random Forest model, geospatial visualizations, and role-based response workflows.

> **Project status:** The core dashboard and API workflows are implemented and covered by automated tests. This is a development/demo prototype, not an operational early-warning system. Some environmental inputs and notification delivery are simulated or approximate; see [Current capabilities and limitations](#current-capabilities-and-limitations).

## What works today

- **Role-based dashboard:** citizen, volunteer, authority, and admin views.
- **Risk assessment:** loads the included Random Forest model and evaluates terrain, rainfall, vegetation, and river-distance features.
- **Monitoring map:** displays configured Northeast India monitoring locations and risk information.
- **Community response workflows:** registration and approval, hazard reports, SOS responses, alert management, volunteer assignment, and field verification.
- **REST API:** FastAPI endpoints for authentication, risk predictions, alerts, reports, volunteer operations, and authority monitoring.
- **Local development storage:** SQLite by default; a PostgreSQL URL can be supplied through configuration.
- **Notification integration:** Twilio SMS/voice support is configurable; mock mode is enabled by default.

## Current capabilities and limitations

- **Terrain and vegetation data:** Monitoring locations and their terrain/NDVI features are bundled in the application. Custom-location terrain and vegetation values are approximations. Google Earth Engine is not currently queried for live satellite updates by the app.
- **Rainfall:** The app attempts to retrieve precipitation from Open-Meteo. If the request is unavailable, it uses generated fallback values. The current implementation requests a limited recent hourly window, so its displayed cumulative value should not be treated as verified 30-day rainfall.
- **Notifications:** SMS and IVR delivery run in mock mode unless valid Twilio credentials are supplied and mock mode is disabled. Do not use the demo configuration to send real emergency alerts.
- **Model and training data:** The included model and data are for prototyping. The training dataset includes synthetic negative examples, so test accuracy is not evidence of real-world predictive performance. Validate with representative, independently verified, geographically and temporally separated data before making operational claims.
- **Safety:** Predictions and recommendations are experimental and must not replace official warnings, field assessment, or emergency services.

## Requirements

- Python 3.10 or newer
- `pip`

The trained model artifacts are included, so model training is not required to start the app.

## Run locally

Create and activate a virtual environment, then install dependencies:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

On macOS or Linux, activate the environment with:

```bash
source .venv/bin/activate
```

Start the Streamlit dashboard:

```bash
streamlit run app.py
```

The dashboard is normally available at <http://localhost:8501>.

To run the API separately in another terminal:

```bash
uvicorn backend.main:app --reload
```

The API is normally available at <http://localhost:8000>; interactive documentation is at <http://localhost:8000/docs>, with ReDoc at <http://localhost:8000/redoc>.

## Configuration

The app uses SQLite for local development by default and creates its database on startup. Optional settings can be supplied in a local `.env` file; `.env` is ignored by Git.

For local setup, start from the template:

```powershell
Copy-Item .env.example .env
```

Before any shared or production deployment:

- Set a unique, securely generated `JWT_SECRET_KEY`.
- Configure an appropriate `DATABASE_URL`.
- Keep Twilio mock mode enabled unless you have deliberately configured and tested real delivery.
- Never commit `.env`, service-account keys, or personal credentials.

## Tests

Run the automated API and workflow tests from the project root:

```bash
python -m pytest tests/ -v
```

The tests cover API health, authentication, risk prediction, alert and report workflows, location filtering, volunteer dispatch, approval, and field verification.

## Repository layout

```text
app.py                 Streamlit dashboard
backend/               FastAPI app, API routes, models, and services
data/                  Prototype datasets and monitoring data
models/                Trained model and feature-column artifacts
scripts/               Model training and optional GEE export utilities
tests/                 API and workflow tests
docs/                  API, schema, product, and walkthrough documentation
```

See the files in [`docs/`](./docs/) for the API specification, database schema, product requirements, user flows, and project walkthrough.
