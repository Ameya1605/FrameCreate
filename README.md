# CreateFrame

CreateFrame is an AI-assisted architecture platform for building full-stack applications and syncing generated architecture to GitHub.

## What this project provides

- **GitHub integration**
  - GitHub OAuth login and user authentication
  - List existing repositories and create new repositories from the app
  - Push generated architecture files and `spec.json` directly to a GitHub repository

- **Architecture-first workspace management**
  - Persist projects as workspaces linked to GitHub repositories
  - Manage features, database schemas, API endpoints, and UI components in one place
  - Toggle AI assistance on or off per project

- **AI-driven code generation**
  - Brainstorm architecture and generate initial specifications from natural language
  - Generate code for backend routes, database schema, and frontend UI components
  - Commit generated project artifacts back to GitHub automatically

- **Full-stack application scaffolding**
  - Backend: FastAPI service with SQLAlchemy, Pydantic, and JWT-based auth
  - Frontend: Next.js app with React 19, Tailwind CSS, and GitHub login flow
  - Project sync features powered by GitHub API utilities in `apps/api/github_utils.py`

## Repository structure

- `apps/api/` — backend service code for authentication, project management, GitHub sync, and AI-driven architecture generation
- `apps/web/` — frontend Next.js app for login, dashboard, and workspace management

## Key backend capabilities

- `POST /auth/github` — authenticate users via GitHub OAuth and issue JWT tokens
- `GET /github/repos` — list GitHub repositories for the authenticated user
- `POST /github/create-repo` — create a new GitHub repo for the current user
- `GET /projects`, `POST /projects`, `DELETE /projects/{project_id}` — manage local project workspaces
- `POST /brainstorm-architecture` — generate an initial architecture spec from a description
- `POST /projects/initialize` — initialize a new repository with baseline architecture files
- `POST /projects/{project_id}/commit` — push architecture files and generated code to GitHub
- `POST /projects/{project_id}/toggle-ai` — enable or disable AI assistance for a project
- `POST /features`, `DELETE /features/{feature_id}` — manage workspace features

## Key frontend capabilities

- Connect and authorize a GitHub account from the landing page
- Select or link an existing GitHub repository as a workspace
- View and manage linked workspaces in a sidebar
- Open a workspace editor and trigger code generation / GitHub commit workflows
- Create new projects with AI-driven scaffolding and repository initialization

## Running the app

### Backend
1. Open `apps/api`
2. Install dependencies from `requirements.txt`
3. Set environment variables such as `DATABASE_URL`, `GITHUB_CLIENT_ID`, and `GITHUB_CLIENT_SECRET`
4. Run the backend with `uvicorn main:app --reload`

### Frontend
1. Open `apps/web`
2. Install dependencies with `npm install`
3. Run the frontend with `npm run dev`

## Notes

- The imported source is stored under `apps/api/` and `apps/web/`.
- This README reflects the actual project functionality available in the repository.
