\# ARCHITECTURE.md — Konkan Coastal Foods HRMS



Author: Vishal Toraskar

Status: Draft (awaiting faculty sign-off)



\## 1. Stack

| Layer | Choice | Why | Alternative rejected | Why rejected |

|---|---|---|---|---|

| Framework | | | | |

| Hosting | | | | |

| Database | | | | |

| Auth | | | | |

| Email | | | | |

| AI model | | | | |

| AI library | | | | |

Framework: Next.js (with TypeScript)



Facts: one project holds both the screens and the server code, so role checks can run on the server. It works with Vercel out of the box, and Claude Code knows it well.

Rejected alternative to consider: plain React + a separate Express server (two projects to deploy and keep in sync).



Hosting: Vercel



Facts: free Hobby plan, auto-deploys every time you git push, made by the same company as Next.js.

Alternative: Netlify or Render. Think about why Vercel is simpler for Next.js.



Database: Supabase (PostgreSQL)



Facts: free tier, real SQL with foreign keys, unique and check constraints, row-level security, and file storage for medical certificates.

Alternative: Firebase (Firestore). Hint: it’s NoSQL. Can it enforce “one attendance row per employee per day” or foreign keys the way SQL can?



Auth: Clerk



Facts: ready-made login screens, free tier, works with Next.js. The brief says roles are stored in your database, not in Clerk.

Alternative: Supabase Auth. Think about why the brief suggests Clerk (ready UI, less code), or argue for Supabase Auth if you prefer.



Email: Resend or Brevo



Facts: Resend is simple to code but needs a verified domain to email other people. Brevo allows sending to anyone after you verify your sender email.

Pick one and note the limitation of the other.



AI model: what you chose in RESEARCH.md Question 11, with the free limits.



AI library: Vercel AI SDK



Facts: supports tool calling, works with both Gemini and Groq, and you can switch models by changing one line.

Alternative: calling the Gemini API directly. More code to handle the tool-calling loop yourself.



\## 2. System diagram

```mermaid

flowchart LR

&#x20; B\[Browser - employee phone or laptop] -->|login| C\[Clerk]

&#x20; B -->|every page and button| S\[Next.js server on Vercel]

&#x20; S -->|read and save data| D\[Supabase database]

&#x20; S -->|medical certificates| F\[Supabase storage]

&#x20; S -->|leave emails| E\[Resend]

&#x20; S -->|chat question + tools| A\[Gemini]

&#x20; S --- R\[Role checks happen here]

```

Why does the browser never talk to the database directly?

Why does the AI model never get the database password?

Where are roles (Employee, Manager, HR) checked?



\## 3. Data model

\### Tables

\### Constraints

\### ER diagram



\## 4. Access control

\### Mapping a signed-in user to an employee

\### Roles

\### Row-level security



\## 5. Business logic location



\## 6. AI agent

\### Model and library

\### Tools

\### What the model can and cannot see

\### How write actions are confirmed



\## 7. Folder structure



\## 8. Dates and time zone (IST)



\## 9. Risks

| Risk | Why it might break | How I will test it |

|---|---|---|



\## 10. Decision log

| Date | Change | Reason |

|---|---|---|

