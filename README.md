
# EPS Ops Starter (v2) — CEO access + HubSpot hooks + JWT + S3 + Postgres-ready

## What’s new
- **CEO role** with full access (bypasses RBAC).
- **JWT login** (`/login` in UI) — demo accounts in `backend/auth.py` (change in prod).
- **S3 uploads** for PO/QC files (fallback to local).
- **HubSpot hooks** on key events (webhook preferred; fallback to CRM Note API).
- **Postgres-ready** (schema + db bootstrap, still using in-memory by default).

## Env (Backend)
- `JWT_SECRET` (required in prod)
- `S3_BUCKET`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_REGION` (optional)
- `HUBSPOT_WEBHOOK_URL` (recommended) **or** `HUBSPOT_TOKEN` (+ optional `HUBSPOT_DEAL_ID`)
- `DATABASE_URL` (optional for Postgres)

## Local dev
Backend:
```
cd backend
pip install -r requirements.txt
uvicorn main:app --reload
```
Frontend:
```
cd frontend
cp .env.example .env.local
npm install
npm run dev
```
Streamlit:
```
cd streamlit
pip install -r requirements.txt
streamlit run app.py
```

## Railway
Create 3 services from this repo:
1) **backend/** → port 8000. Set envs above.
2) **frontend/** → port 3000. Set `NEXT_PUBLIC_API_BASE` to backend public URL.
3) **streamlit/** → port 8501. Set `API_BASE` to backend URL; optionally `API_TOKEN` (grab from `/login`).

## Demo accounts
- ceo@sandbox / pass123  (role: ceo → full access)
- pm@sandbox / pass123
- sales@sandbox / pass123
- accounting@sandbox / pass123
- design@sandbox / pass123
- production@sandbox / pass123
- qc@sandbox / pass123

## HubSpot events fired
- **PO_UPLOADED**
- **WORK_ORDER_CREATED**
- **DESIGN_UPDATED**
- **DESIGN_DECISION**
- **BOM_RELEASED**
- **QC_RESULT**
- **DISPATCH_CREATED**

Use a HubSpot **Webhook** action in a Workflow to receive these (set `HUBSPOT_WEBHOOK_URL`), or set a **Private App token** and optional `HUBSPOT_DEAL_ID` to create Notes automatically.

## Next steps (SAP B1 tomorrow)
- Implement Service Layer calls in a thin client and replace mocks in backend routes.
- Add real persistence (move from in-memory to Postgres via `db.py`).
