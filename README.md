# 🎙️ AI-Powered Presentation Assistant (V2 - Isolated)

[![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)](https://reactjs.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)

An intelligent system that analyzes real-time speech during presentations to automatically navigate slides. This version (V2) is configured to run independently of other instances, ensuring an isolated development and execution environment.

---

## 🏗️ System Architecture (V2)

The system is built on a modern, decoupled microservice architecture, optimized for real-time AI processing and semantic search.

| Service | Technology | Host Port (Local) | Container Port | Description |
| :--- | :--- | :--- | :--- | :--- |
| **Backend** | Python (FastAPI) | `8001` | `8000` | AI logic, RAG pipeline, and WebSockets. |
| **Frontend** | Next.js (React) | `3001` | `3000` | User dashboard, uploads, and live mode. |
| **Database** | PostgreSQL + pgvector | `5436` | `5432` | Vector embeddings & Relational data. |

---

## 🚀 Getting Started

### 1. Repository Setup

Clone the repository and prepare the environment configuration.

```bash
# Clone the repository
git clone https://github.com/oznurkarahasan/ai-presentation-assistant-v2.git
cd ai-presentation-assistant

# Create .env from template
cp .env.example .env
```

### 2. Launch the System (V2)

> [!IMPORTANT]
> Bu proje özel bir dosya ismi kullandığı için şu komutla başlatılmalıdır:

```bash
docker-compose -f docker-compose.yml up -d --build
```

---

## 🔗 Access Points

| Component | URL / Connection String |
| :--- | :--- |
| **Frontend UI** | [http://localhost:3001](http://localhost:3001) |
| **Backend API** | [http://localhost:8001](http://localhost:8001) |
| **API Documentation** | [http://localhost:8001/docs](http://localhost:8001/docs) |
| **Database** | `localhost:5436` (User: `admin` / Pass: `admin`) |

---

## 🛠️ Development Workflow

### Local Python Environment (Backend)

For local development, IntelliSense, and testing, set up a virtual environment:

```bash
cd backend
python -m venv venv

# Windows Activation:
.\venv\Scripts\activate

# Mac/Linux Activation:
# source venv/bin/activate

pip install -r requirements.txt
```

### Useful V2 Commands

| Action | Command |
| :--- | :--- |
| **Stop Project** | `docker-compose down` |
| **View Logs** | `docker-compose logs -f` |
| **Clean Reinstall** | `docker-compose down -v && docker-compose up -d --build` |

### Database CLI Access

Access the PostgreSQL instance directly via Docker:

```bash
docker exec -it presentation_db_v2 psql -U admin -d presentation_db_v2
```

---

## 📊 Database Schema (PostgreSQL + pgvector)

The system relies on a robust **9-Table Relational Structure** designed for data integrity, AI compatibility, and comprehensive audit logging. All relationships utilize `CASCADE DELETE` to ensure that when a user is removed, all associated data (slides, notes, sessions, logs) is automatically wiped to prevent orphan data.

| Table Name | Description | Key Technical Features |
| :--- | :--- | :--- |
| **`users`** | Central identity table. | Supports Age Analysis via `birth_date`. Primary root of all relations. |
| **`user_preferences`** | User-specific settings. | 1:1 Relation. Stores `ideal_presentation_time` and `language`. |
| **`presentations`** | Metadata for uploaded files. | Supports **Guest Mode** (`user_id` is nullable). Tracks file hashes for security. |
| **`slides`** | The "Brain" of the RAG system. | Stores **Vector Embeddings (1536 dim)** for AI semantic search via **pgvector**. |
| **`notes`** | User-specific slide notes. | Strictly for registered users (`user_id` NOT null). |
| **`presentation_analyses`** | AI-generated report card. | **JSONB storage** for flexible AI metrics, feedback, and scoring. |
| **`presentation_sessions`** | Performance logs. | Tracks practice vs live modes, duration, and real-time slide indices. |
| **`activity_logs`** | System-wide audit trail. | Logs all user actions (upload, delete, login) for security and analytics. |
| **`verification_tokens`** | Authentication security. | Time-limited tokens for email verification and secure password resets. |

---

<p align="center">
  Developed with ❤️ for intelligent presentation management.
</p>
