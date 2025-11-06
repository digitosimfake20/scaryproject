Vercel deployment and local development
=====================================

What I changed
- Added `vercel.json` to configure Vercel to serve the React app from `stem-project/build` and expose serverless API endpoints under `/api`.
- Added two serverless API wrappers in `api/`:
  - `api/questions.js` — reuses `stem-project/backend/ai/analyzer.js` to return questions and grouped questions.
  - `api/analyze-quiz.js` — reuses `analyzeQuiz` to run the quiz analysis (POST).
- Updated the root `package.json` to add convenient dev scripts and include `openai` and `dotenv` dependencies required by the analyzer when running on Vercel.
- Added `proxy` to `stem-project/package.json` so CRA dev server forwards `/api` to the local backend.

How to run locally (development)
1. Install dependencies at the repo root. From repo root run:

```powershell
npm install
cd stem-project
npm install
cd ..
```

2. Start both frontend and backend in parallel from the repo root:

```powershell
npm run dev
```

This runs the CRA dev server (port 3000) and the backend server (port 5000). The frontend will proxy `/api` to the backend.

How to build for production (locally)
1. From repo root run:

```powershell
npm run build
```

This runs `npm run build` inside `stem-project` and produces `stem-project/build`.

Vercel deployment notes
- The `vercel.json` instructs Vercel to run the static build for the React app and uses any files in `api/` as Node serverless functions.
- Ensure environment variables used by the backend (for example `OPENAI_API_KEY`) are set in Vercel's project settings (Environment Variables).
- The analyzer will warn and fall back to stub behaviour if `OPENAI_API_KEY` is not present.

Assumptions and limitations
- The serverless wrappers reuse the analyzer located at `stem-project/backend/ai/analyzer.js` — this keeps logic in one place but means Vercel must install the `openai` dependency (already added to root `package.json`).
- I kept the Express server (`stem-project/backend/server.js`) unchanged for local dev and for alternative hosting.
- If you'd prefer the backend to remain an Express server on a single host (not serverless), deploy frontend to Vercel and backend to a separate service (Heroku, Render, Azure, etc.) and update `proxy`/API URLs accordingly.

Next steps (optional)
- Add a small integration test that hits `/api/questions?quizId=random` and `/api/analyze-quiz` to validate serverless functions locally with `vercel dev`.
- Convert Express routes into individual serverless function handlers if you prefer smaller per-endpoint files.
