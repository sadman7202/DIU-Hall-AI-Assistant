# Dependencies

This document lists runtime and build dependencies for the DIU Hall AI Assistant & Automation Platform.

## Overview

- Backend: Python + FastAPI stack with PostgreSQL and AI/RAG libraries.
- Frontend: React + Vite.
- Infrastructure: Docker Compose with PostgreSQL, backend, and frontend services.

## System Requirements

- Docker and Docker Compose (recommended for local dev).
- Node.js and npm (frontend local dev).
- Python 3.x (backend local dev).

## Backend (Python)

Declared in [backend/requirements.txt](backend/requirements.txt).

- fastapi
- uvicorn[standard]
- sqlalchemy
- psycopg[binary]
- pydantic-settings
- python-multipart
- pwdlib[argon2]
- python-jose[cryptography]
- email-validator
- reportlab
- chromadb==0.5.5
- sentence-transformers==2.7.0
- transformers==4.41.2
- torch==2.3.1+cpu (extra index used)
- groq
- google-genai
- qrcode[pil]

Notes:
- Torch uses the CPU wheel index defined in [backend/requirements.txt](backend/requirements.txt).
- groq and google-genai require API keys in the environment.

## Frontend (Node)

Declared in [frontend/package.json](frontend/package.json).

Dependencies:
- html5-qrcode
- react
- react-dom
- react-router-dom

Dev dependencies:
- @vitejs/plugin-react
- vite

## Infrastructure

Defined in [docker-compose.yml](docker-compose.yml).

- PostgreSQL: postgres:17-alpine image.
- Backend service: built from [backend/Dockerfile](backend/Dockerfile).
- Frontend service: built from [frontend/Dockerfile](frontend/Dockerfile).

## Optional Environment Dependencies

- SMTP credentials for email notifications.
- Public URLs for frontend/backend (used for email links and CORS).
