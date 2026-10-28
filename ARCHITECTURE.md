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

\- locations: id, name, city, state, weekly\_off\_days

\- departments: id, name

\- employees: location\_id, department\_id, reporting\_manager\_id, clerk\_user\_id, employee\_code, full\_name, email, mobile, dob, gender, date\_of\_joining, last\_working\_day, designation, is\_hr\_admin, status

\- employee\_statutory: employee\_id, pan, aadhaar, uan

\- employee\_bank: employee\_id, account\_number, ifsc, bank\_name

\- holidays: location\_id, date, name, type (National / State / Optional)

\- attendance: id, employee\_id, date, check\_in, check\_out, is\_wfh, status, is\_late

\- regularisation\_requests: id, attendance\_id, requested\_in, requested\_out, reason, status, approver\_id

\- leave\_types: id, code, name, annual\_quota, accrual\_method, carry\_forward\_max, half\_day\_allowed

\- leave\_balances: employee\_id, leave\_type\_id, year, opening, accrued

\- leave\_requests: id, employee\_id, leave\_type\_id, from\_date, to\_date, is\_half\_day, days, reason, status, approver\_id, approver\_comment, attachment\_url

\- audit\_log: id, table\_name, record\_id, field, old\_value, new\_value, changed\_by, changed\_at



\### Constraints

| Table | Constraint | Rule | Why |

|---|---|---|---|

| employees | unique | employee\_code is unique | |

| employees | unique | email is unique | |

| employees | check | dob is at least 18 years before date\_of\_joining | |

| employees | check | mobile matches ^\[6-9]\[0-9]{9}$ | |

| employees | check | reporting\_manager\_id is not the employee's own id | |

| employees | foreign key | location\_id → locations, department\_id → departments, reporting\_manager\_id → employees | |

| employee\_statutory | check | pan matches ^\[A-Z]{5}\[0-9]{4}\[A-Z]$ | |

| employee\_statutory | check | aadhaar is 12 digits, uan is 12 digits or empty | |

| employee\_bank | check | ifsc matches ^\[A-Z]{4}0\[A-Z0-9]{6}$ | |

| attendance | unique | (employee\_id, date) | |

| holidays | unique | (location\_id, date) | |

| leave\_balances | unique | (employee\_id, leave\_type\_id, year) | |

| leave\_requests | check | to\_date is on or after from\_date | |

| leave\_requests | check | status is one of Draft, Pending, Approved, Rejected, Cancelled | |

| leave\_requests | check | approver\_id is not the same as employee\_id | |

| all tables | no delete | employees are never deleted, only deactivated | |



\### ER diagram

```mermaid

erDiagram

LOCATIONS ||--o{ EMPLOYEES : "works at"

DEPARTMENTS ||--o{ EMPLOYEES : "has"

EMPLOYEES ||--o{ EMPLOYEES : "manages"

EMPLOYEES ||--o| EMPLOYEE\_STATUTORY : "has"

EMPLOYEES ||--o| EMPLOYEE\_BANK : "has"

LOCATIONS ||--o{ HOLIDAYS : "has"

EMPLOYEES ||--o{ ATTENDANCE : "marks"

ATTENDANCE ||--o{ REGULARISATION\_REQUESTS : "corrected by"

EMPLOYEES ||--o{ LEAVE\_REQUESTS : "applies"

LEAVE\_TYPES ||--o{ LEAVE\_REQUESTS : "type of"

```



\## 4. Access control

\### Mapping a signed-in user to an employee

The user logs in with Clerk and Clerk gives your server their email.

The server looks up the employees table for that email.

On first login, it saves Clerk’s user id into clerk\_user\_id, so next time it matches by id.

No matching row → what should happen? (Access denied.)

last\_working\_day has passed → what should happen? (Access denied, even though Clerk login worked.)



\### Roles

Employee: who is an employee? (Everyone with an active row.)

HR Admin: how does the app know? (Hint: which column in employees?)

Manager: the brief says don’t store it. How is it worked out instead? (Hint: page 5, reporting\_manager\_id.) Then: why is working it out better than storing it? Think about what happens when someone’s last direct report leaves.



\### Row-level security

Which 2 tables get RLS turned on? (The brief names them on page 15.)

The rule: no access from the browser at all. Only your server code can read them, and it masks the values before sending them to the screen.

One sentence: why does this protect Aadhaar even if someone finds your Supabase public key?



\## 5. Business logic location

| Rule | Where it lives | Why |

|---|---|---|

| Count working days (weekly offs + holidays per location) | | |

| Day status (Present / Half Day / Absent / On Leave / Holiday / Weekly Off) | | |

| Late mark (after 10:15 IST) and 3 Lates = 0.5 LOP | | |

| Leave balance (opening + accrued − used − pending) | | |

| Pro-rating CL/SL for mid-year joiners | | |

| EL accrual (1.25 per completed month) | | |

| PAN / Aadhaar / IFSC / mobile format | | |

| One attendance row per employee per day | | |

| Employee code generation (KCF0001) | | |



\## 6. AI agent

\### Model and library

Model: the one you picked in RESEARCH.md (e.g. Gemini Flash), plus its free limits

Library: Vercel AI SDK. Why? (Hint: tool calling, and switching models takes one line.)



\### Tools

| Tool | What it does | Inputs | Output | Who can use | Read/Write |

|---|---|---|---|---|---|

| get\_my\_leave\_balance | User's balance per leave type | | | | Read |

| get\_holidays | Holidays for user's location in a month | | | | Read |

| count\_working\_days | Leave days between two dates | | | | Read |

| explain\_policy | Answers from stored policy text only | | | | Read |

| draft\_leave\_request | Creates a draft, not submitted | | | | Write (confirmed) |

| get\_team\_today | Team present/leave/absent today | | | | Read |

| summarise\_pending\_approvals | Pending requests with clash warnings | | | | Read |

| (your own tool) | | | | | |



\### What the model can and cannot see

Can see: the user’s name, role, location, today’s date in IST, and tool results (balances, day counts, holidays, policy text)

Never sees: PAN, Aadhaar, bank details, database password, API keys, and other employees’ private data



\### How write actions are confirmed

The agent only creates a draft.

The user sees a Confirm button.

Clicking Confirm runs the normal apply-leave action, so all the usual checks run again.

The agent has no tool to approve, reject or cancel. Explain why that matters.



\## 7. Folder structure

konkan-hrms-v2/

├── app/

│   ├── dashboard/

│   ├── employees/

│   ├── attendance/

│   ├── leave/

│   ├── approvals/

│   ├── reports/

│   ├── ask-hr/

│   ├── settings/

│   └── access-denied/

├── lib/

│   ├── auth/

│   ├── rules/

│   ├── db/

│   ├── agent/

│   ├── email/

│   └── validation/

├── supabase/

│   ├── migrations/

│   └── seed/

├── tests/

│   ├── employees/

│   ├── attendance/

│   ├── leave/

│   └── agent/

├── RESEARCH.md

├── ARCHITECTURE.md

├── DESIGN.md

├── CLAUDE.md

├── README.md

└── .env.example



\## 8. Dates and time zone (IST)

Database: check-in and check-out are stored as timestamptz (timestamp with time zone), so the exact moment is never lost.

The attendance date: the server works out the date in Asia/Kolkata, never from the server’s own clock and never from the user’s browser. Say why you don’t trust the browser. (Hint: a user could change their laptop’s time.)

Showing times on screen: always formatted with the Asia/Kolkata time zone, e.g. 09:42 AM IST.

The Late rule: “after 10:15 AM” is checked in IST, not UTC.



\## 9. Risks

| Risk | Why it might break | How I will test it |

|---|---|---|

Risk	Hint for “why it might break”	Hint for “how I will test it”

Wrong leave-day count	Two locations with different weekly offs, plus location-wise holidays	Friday to Monday at Panaji = 2 days, at Verna = 3 days; a leave spanning a holiday

Role check bypassed	Someone types another employee’s URL, or calls a server action directly	Log in as an employee and open another person’s profile URL; try approving your own leave

Time zone bugs	Vercel runs on UTC, India is UTC+5:30	Check in near midnight; change your laptop’s time zone; a 10:20 AM check-in must be Late



\## 10. Decision log

| Date | Change | Reason |

|---|---|---|

| 28-Oct-2026 | Removed "used" column from leave\_balances; added agent\_log, pii\_reveal\_log, policy\_text; added location\_id, department\_id, reporting\_manager\_id to employees and location\_id to holidays | |

