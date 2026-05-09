# DIU Hall AI Assistant & Automation Platform v0.2.2

DIU Hall AI Assistant & Automation Platform is a full-stack hall-management system for DIU hall operations. The current v0.2.2 codebase combines a FastAPI backend, a React + Vite frontend, PostgreSQL persistence, Docker-based development, and a hall-rule chatbot that uses semantic retrieval with optional LLM generation.

## What v0.2.2 Covers

- Student and admin authentication with JWT-protected routes
- Student profile management and signature upload
- Gate-pass request submission, approval, rejection, and PDF generation
- Notice publishing and notification delivery
- Complaint submission and admin status updates
- Hall-rule management for admins
- Chat sessions for the hall-rule assistant
- RAG-based hall-rule lookup with ChromaDB and Sentence Transformers
- Optional response generation through Groq or Gemini, with a deterministic fallback answer

## Tech Stack

- Frontend: React 19, Vite, React Router
- Backend: FastAPI, Uvicorn, SQLAlchemy, Pydantic Settings
- Database: PostgreSQL
- AI / Retrieval: ChromaDB, Sentence Transformers, Groq, Google Gen AI
- PDF generation: ReportLab
- Containerization: Docker, Docker Compose

## Repository Layout

```text
backend/
  app/
    core/
    data/
    db/
    models/
    schemas/
    services/
    main.py
  requirements.txt

frontend/
  src/
    components/
    pages/
    App.jsx
    main.jsx
    styles.css
  package.json

docker-compose.yml
README.md
```

## Main Features

### Student Experience

- Register and log in
- Edit profile details
- Upload a signature image
- Submit gate-pass requests
- View notices
- Submit complaints
- Receive in-app notifications
- Chat with the hall-rule assistant

### Admin Experience

- Review gate-pass requests
- Approve or reject gate passes
- Generate gate-pass PDFs automatically
- Publish notices
- Update complaint status
- Create, edit, delete, and refresh hall rules
- Rebuild the chatbot vector index
- Send a development email test when configured

### Chatbot Behavior

- Loads chat sessions from the backend
- Lets users rename and delete chat sessions
- Searches hall rules by exact rule number and semantic similarity
- Uses Groq first when configured, then Gemini, then a rule-based fallback
- Returns matched rules alongside the answer

## Environment Variables

The backend reads configuration from `.env`.

```env
APP_NAME=DIU Hall AI Assistant and Automation Platform
APP_ENV=development
DATABASE_URL=postgresql+psycopg://diu_user:diu_password@localhost:5432/diu_hall
BACKEND_CORS_ORIGINS=["http://localhost:5173","http://127.0.0.1:5173"]

EMAIL_NOTIFICATIONS_ENABLED=false
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USERNAME=
SMTP_PASSWORD=
SMTP_FROM_EMAIL=
SMTP_USE_TLS=true

PUBLIC_BACKEND_URL=http://localhost:8000
PUBLIC_FRONTEND_URL=http://localhost:5173

LLM_ENABLED=true
GROQ_API_KEY=
GROQ_MODEL=llama-3.1-8b-instant
GEMINI_API_KEY=
GEMINI_MODEL=gemini-2.0-flash
LLM_TEMPERATURE=0.2
LLM_MAX_OUTPUT_TOKENS=500
```

## Run With Docker

1. Create a `.env` file in the project root if you want to override any defaults.
2. Start the stack:

```bash
docker compose up --build
```

3. Open the app:

- Frontend: http://localhost:5173
- Backend API: http://localhost:8000
- Swagger docs: http://localhost:8000/docs

Docker Compose starts these services:

- `db` on `localhost:55432` by default
- `backend` on `localhost:8000`
- `frontend` on `localhost:5173`

## Run Manually

### Backend

```bash
cd backend
python -m venv .venv
.venv\Scripts\activate
pip install --upgrade pip setuptools wheel
pip install -r requirements.txt
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

### Frontend

```bash
cd frontend
npm install
npm run dev
```

## Backend API Surface

The backend exposes endpoints for:

- Health checks
- Authentication and current user lookup
- Signature upload
- Gate-pass CRUD and approval actions
- Notices
- Notifications
- Complaints
- Chat sessions and message history
- Hall-rule management
- Chat Q&A
- Development email testing

For the exact request and response shapes, use the Swagger UI at `/docs`.

## Data and Storage

- Uploaded student signatures are stored under `backend/uploads/signatures/students`
- Generated gate-pass PDFs are stored under `backend/uploads/gate_pass_pdfs`
- Hall-rule seed data comes from `backend/app/data/processed/chunks_updated.json`
- The ChromaDB vector store lives under `backend/app/data/vectordb`

## Frontend Pages

- Login
- Register
- Dashboard
- Profile
- Gate Pass
- Notice Board
- Complaints
- Chatbot
- Admin Rules

## Notes

- The project is currently positioned as a functional prototype for hall management workflows.
- If you enable email or LLM features, make sure the matching API keys and SMTP settings are present in `.env`.
