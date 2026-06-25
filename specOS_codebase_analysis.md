# SpecOS Codebase Analysis

## Project Overview
**SpecOS** - A technical control plane for modern builders. It allows developers to architect their applications (schemas, APIs, UI components, prompts) in one place and sync directly to GitHub.

**Tagline**: "Architect Before You Code"

## Architecture
- **Full-stack monorepo** structure with separated apps
- **Backend**: Python FastAPI + PostgreSQL (with SQLite fallback)
- **Frontend**: Next.js 15 + React 19 + TypeScript + Tailwind CSS
- **Auth**: GitHub OAuth integration
- **AI**: Groq API integration for architectural suggestions

## Backend Structure (apps/api)

### Core Files:
1. **main.py** - FastAPI application
   - CORS middleware configured for all origins
   - GitHub OAuth authentication endpoint
   - JWT token-based authorization
   - Routes for projects, repos, endpoints, schemas, UI components

2. **models.py** - SQLAlchemy ORM models
   - User (GitHub auth)
   - Project (owner, name, repo_url, is_ai_enabled)
   - Feature (name, status: mvp/v2/experimental, description)
   - DatabaseSchema (table_name, fields in JSON, code)
   - ApiEndpoint (method, route, request/response schemas, code)
   - UIComponent (name, type: page/layout/component, code)
   - PromptTemplate (for AI integration)

3. **database.py** - Database configuration
   - Supports PostgreSQL (Render/Railway/Heroku) and SQLite
   - SQLAlchemy ORM setup
   - Foreign key support for SQLite

4. **auth.py** - Authentication
   - JWT token creation/validation
   - Fernet encryption for GitHub tokens
   - Bearer token middleware
   - Token expiry: 7 days

5. **generator.py** - Code generation
   - Generates Prisma schemas from specs
   - Generates FastAPI code from endpoint specs
   - Type mapping: string, integer, boolean, json, uuid, datetime, float

6. **brain.py** - AI-powered suggestions
   - Uses Groq API (llama-3.3-70b-versatile model)
   - Analyzes specs and suggests missing architecture items
   - Provides architectural guidance per layer

7. **github_utils.py** - GitHub API integration
   - OAuth token exchange
   - Fetch user repos
   - Create repos
   - Likely handles repo operations

8. **repo_scanner.py** - Repository scanning
   - Likely parses existing GitHub repos to extract architecture

## Frontend Structure (apps/web)

### Key Pages:
- **page.tsx** - Landing page (hero, GitHub login, feature cards)
- **auth/callback/page.tsx** - OAuth callback handler
- **dashboard/page.tsx** - Main dashboard
- **new-project/page.tsx** - Project creation
- **project/[id]/page.tsx** - Project detail/editor

### Frontend Features:
- GitHub login integration
- Dark theme (zinc-900, blue-500 accent colors)
- Responsive grid layouts
- Feature cards highlighting: Versioned Specs, Instant Sync, AI suggestions

### Dependencies:
- axios (API calls)
- lucide-react (icons)
- tailwindcss (styling)
- typescript

## Database Schema

### Relationships:
```
User (1) -> (many) Project
Project (1) -> (many) Feature
Project (1) -> (many) DatabaseSchema
Project (1) -> (many) ApiEndpoint
Project (1) -> (many) UIComponent
Project (1) -> (many) PromptTemplate
```

### Key Constraints:
- User.github_id unique
- User.username unique
- Project unique constraint: (owner_id, name)
- Feature unique constraint: (project_id, name)
- DatabaseSchema unique: (project_id, table_name)
- ApiEndpoint unique: (project_id, method, route)

## Key Features

1. **GitHub Integration**
   - OAuth login
   - Repository listing and creation
   - Spec syncing to GitHub

2. **Architecture Specification**
   - Define database schemas
   - Define API endpoints with request/response schemas
   - Define UI components and pages
   - Create feature flags (mvp, v2, experimental)

3. **AI-Powered Suggestions**
   - Groq API integration for architectural recommendations
   - Layer-specific suggestions
   - Context-aware improvements

4. **Code Generation**
   - Prisma schema generation from specs
   - FastAPI code generation from endpoint specs
   - Export to GitHub repositories

5. **Project Management**
   - AI enablement toggle per project
   - Feature versioning
   - Prompt templates for AI interactions

## Tech Stack

### Backend:
- FastAPI (web framework)
- SQLAlchemy (ORM)
- PostgreSQL (primary DB)
- SQLite (fallback)
- python-jose + cryptography (auth)
- httpx (HTTP client)
- Groq API (AI suggestions)
- psycopg2 (PostgreSQL driver)

### Frontend:
- Next.js 15.1.7
- React 19.2.3
- TypeScript 5
- Tailwind CSS 4
- lucide-react (icons)
- axios (HTTP)

### Deployment:
- Docker (containerization)
- Hugging Face Spaces compatible (PORT 7860)
- Railway/Render/Heroku PostgreSQL support
- Vercel (frontend suggested from vercel.json)
- render.yaml (Render deployment)
- railway.json (Railway deployment)

## API Endpoints (from main.py)
- POST /auth/github - GitHub OAuth login
- GET /github/repos - List user repos
- POST /github/create-repo - Create repo
- GET /projects - List projects
- (More endpoints likely further in main.py)

## Notable Implementation Details

1. **Security**: JWT tokens with 7-day expiry, encrypted GitHub token storage
2. **Type Safety**: Pydantic models for request/response validation
3. **Scalability**: Relationships with cascade deletes for data integrity
4. **Flexibility**: JSON fields for dynamic schema/endpoint definitions
5. **Multi-deployment**: Supports multiple hosting platforms with flexible DB URLs
