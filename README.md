# 🎙️ AI-Powered Presentation Assistant (V2)

<p align="center">
  <img src="https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi" alt="FastAPI">
  <img src="https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" alt="React">
  <img src="https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL">
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker">
</p>


An intelligent system that analyzes real-time speech during presentations to automatically navigate slides, powered by Edge AI logic and a modern web stack. This version is configured to run on isolated ports (3001/8001) to avoid conflicts with previous installations.

## System Architecture
This project utilizes a decoupled microservice architecture orchestrated via Docker Compose.

| Service | Technology | Host Port (Local) | Container Port | Description |
| :--- | :--- | :--- | :--- | :--- |
| Backend | Python (FastAPI) | 8001 | 8000 | AI logic, RAG pipeline, and WebSockets. |
| Frontend | Next.js (React) | 3001 | 3000 | User interface for uploads and live mode. |
| Database | PostgreSQL + pgvector | 5436 | 5432 | User data and vector embeddings (1536 dim). |

## Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/oznurkarahasan/ai-presentation-assistant-v2.git
cd ai-presentation-assistant
```

### 2. Configure Environment Variables
Create a `.env` file in the root directory by copying the example.
```bash
# Windows
copy .env.example .env
# Mac/Linux
cp .env.example .env
```
**Important:** Open `.env` and fill in your `OPENAI_API_KEY`.

### 3. Start the System
Run the following command to build and start all services:
```bash
docker-compose up --build
```
**Note:** This will use the default `docker-compose.yml` but with V2 isolated settings.

### 4. Local Development Setup
To enable IntelliSense and fix missing imports in VS Code:
- **Navigate to backend:** `cd backend`
- **Create Virtual Environment:** `python -m venv venv`
- **Activate Environment:**
  - **Windows:** `.\venv\Scripts\activate`
  - **Mac/Linux:** `source venv/bin/activate`
- **Install Dependencies:** `pip install -r requirements.txt`
- **VS Code Selection:** `Ctrl + Shift + P` -> `Python: Select Interpreter` -> Select the one inside `./backend/venv/`.

### 5. Access Points
- **Frontend (UI):** [http://localhost:3001](http://localhost:3001)
- **Backend API:** [http://localhost:8001](http://localhost:8001)
- **API Documentation:** [http://localhost:8001/docs](http://localhost:8001/docs)
- **Database:** Localhost port 5436 (User/Pass: admin/admin)

## 6. Database Schema (PostgreSQL + pgvector)
The system relies on a 9-Table Relational Structure designed for data integrity and AI compatibility.

| Table Name | Description | Key Features |
| :--- | :--- | :--- |
| users | Central identity table. | Supports Age Analysis (birth_date). |
| user_preferences | User-specific settings. | Stores ideal_presentation_time, language. |
| presentations | Metadata for files. | Supports Guest Mode (user_id nullable). |
| slides | The "Brain" of the RAG. | Vector Embeddings (1536 dim) for semantic search. |
| notes | Slide-specific notes. | Strictly for registered users. |
| presentation_analyses | AI report card. | JSON-based storage for flexible AI feedback. |
| presentation_sessions | Performance logs. | Tracks practice vs live sessions and duration. |
| activity_logs | System audit trail. | Logs all user actions for security. |
| verification_tokens | Auth security. | Email verification & password reset tokens. |

**Security Note:** All relationships utilize `CASCADE DELETE`. Deleting a user wipes all associated data.

## 7. Shortcuts & Commands

### Docker Management
- **Enter DB Terminal:** `docker exec -it presentation_db_v2 psql -U admin -d presentation_db_v2`
- **Full Cleanup:** `docker-compose down -v` (Deletes volumes/data)
- **System Prune:** `docker system prune -a --volumes`

### Service-Specific Commands
- **Watch Logs:** `docker-compose logs -f [backend/frontend]`
- **Restart one service:** `docker-compose up -d --build backend`
- **Shell inside container:** `docker-compose exec backend bash`

### Python Workflow
If you add new libraries:
```bash
cd backend
pip install new_library
pip freeze > requirements.txt
docker-compose up --build backend
```

---

<p align="center">
  Developed with ❤️ for intelligent presentation management.
</p>

