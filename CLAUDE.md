\# CLAUDE.md — Konkan Coastal Foods HRMS



\## What this project is

An HRMS for Konkan Coastal Foods (180 employees, Panaji head office and Verna plant, Goa).

Modules: Employee Master, Attendance, Leave, and an HR assistant AI agent.

College assignment: correctness of HR rules and security matter more than looks.



\## Stack

Next.js (App Router, TypeScript), Vercel, Supabase (Postgres + storage), Clerk, \_\_\_\_ (email), Vercel AI SDK + \_\_\_\_ (AI model).



\## Source of truth

\- Business rules: RESEARCH.md section 10 (leave policy rules). Follow them exactly.

\- Data model, access control, folders: ARCHITECTURE.md. Do not change the data model without asking me.

\- Screens, colours, status colours, components: DESIGN.md.



\## Rules you must always follow

\- All dates and times are in IST (Asia/Kolkata). Never use the server's local date.

\- Every page and server action checks the user's role on the server (lib/auth). Hiding buttons is not enough.

\- Business rules live in lib/rules as plain functions with tests in tests/.

\- Never send PAN, Aadhaar or bank details to the browser unmasked, or to the AI model at all.

\- Never commit secrets. Keys go only in .env.local. Keep .env.example with blank values.

\- Never delete employee records; deactivate them.

\- Never run database resets, drop tables, or push to GitHub without asking me first.

\- Write or update tests before implementing a feature, then run them until they pass.

\- Before writing code for a checkpoint, show me a plan and wait for my approval.



\## Folder rules

app/ = screens, lib/rules = HR calculations, lib/auth = role checks, lib/db = database access,

lib/agent = AI tools, lib/validation = Indian field checks, supabase/migrations = SQL, tests/ = tests.

