\# RESEARCH.md — Konkan Coastal Foods HRMS



Author: Vishal Toraskar

Date: 



\## 1. Employee details kept by Indian HR teams

\### Mandatory details

Basic details: full name, date of birth, gender, address, mobile, email, date of joining, designation, department, reporting manager.

PAN: needed because the employer deducts income tax (TDS) from salary and issues Form 16.

UAN: the PF account number. EPF applies to establishments with 20 or more employees, so Konkan (180 people) must have it.

Aadhaar: used for KYC and for linking with the UAN on the EPFO portal.

Bank account + IFSC: needed to pay the salary.

Emergency contact and PF nominee.



\### Sensitive details

Which fields are sensitive: Aadhaar, PAN, bank details, and any medical information (such as the sick-leave certificate).

Why: misuse can lead to identity theft or financial fraud.

The law: the Digital Personal Data Protection (DPDP) Act, 2023. Companies must protect personal data and collect only what they need. Aadhaar should be masked when displayed.

Link this to your app: “That’s why my HRMS keeps these in separate tables, masks them on screen, and logs every Reveal.”



\## 2. Employee code

How it’s formed. Usually a company prefix plus a running number, sometimes with a location or year code. For Konkan it will be KCF + 4 digits, like KCF0001, KCF0002, and so on. Mention one real example you know of: your college roll number, a relative’s employee ID, or your bank’s customer ID.

Why it must never change. Think about where the code gets used: payroll files sent to the vendor, PF records, attendance history, leave history, audit logs and old reports. Ask yourself what would break if it changed. Names can change (after marriage, say), emails can change, and two people can have the same name, but the code stays fixed.

Why it’s never reused. If KCF0045 leaves, a new joiner should not get KCF0045, or old records would get mixed up with the new person.



\## 3. Leave types in Indian companies

What is CL (Casual Leave) used for? Give one example situation.

What is SL (Sick Leave) for?

What is EL/PL (Earned or Privilege Leave)? How is it different from CL?

How long is maternity leave in India? Look it up on the Ministry of Labour site (labour.gov.in) and note which law gives it. India’s new Code on Social Security came into force in late 2025, so check what it says now.

What is paternity leave? Is it required by law for private companies, or is it the company’s choice?

What is LOP (Loss of Pay)? When does it happen?

What is comp-off? Give one example.



\### Statutory entitlement vs company policy

In one sentence: what does “statutory” mean?

Which of the leave types above are statutory, and which are company policy?

Why does this difference matter for an HRMS? (Hint: can the company reduce it?)



\## 4. Leave accrual and year-end treatment

What does “credited at the start of the year” mean? Which Konkan leave types work this way?

What does “monthly accrual” mean? Which Konkan leave works this way, and how much is added each month?

What is pro-rating for someone who joins mid-year? Work out this example yourself:

Ravi joins on 1 July. CL quota is 8 days per year. How many CL should he get for that year?

Do the same for SL (7 days per year).

Hint: count how many months are left in the year, including July.

If the answer comes out as a decimal like 3.5, should you round it? Decide a rule, write it down, and say why.



\## 5. Goa Shops and Establishments Act

\### Working hours

\### Weekly off

\### Leave

Which law, and does it apply to Konkan? Name the Act and its year. Check who it covers: according to one summary, with 20 or more workers the whole Act applies, including registration, hours, overtime at double rate, a weekly holiday, sick, casual and earned leave, and nine paid holidays. Konkan has 180 people, so does it apply? 

righttoinformation

Working hours: what are the daily and weekly maximums? One summary says the new Act caps working hours at 10 hours per day or 48 hours per week for adults. Confirm this in the Act itself. 

lawrbit

Rest break: after how many hours of work must a break be given?

Weekly off: the Act requires a weekly holiday of at least twenty-four consecutive hours of rest; if denied, a compensatory holiday must be given within two months. How does Konkan’s policy (Sat+Sun off at Panaji, Sun off at Verna) meet this? 

righttoinformation

Leave: find the leave chapter in the Act. How many days of earned, casual and sick leave does it give? Is Konkan’s policy (EL 15, CL 8, SL 7) equal to or better than the law?



\## 6. National, state and restricted holidays

National holidays: India has 3 national holidays that every establishment observes. Name them and their dates.

State holidays: these are declared by each state government, so they differ from state to state. Name 2 Goa-specific holidays (hint: think 19 December and 30 May). Where does the Goa government publish its yearly holiday list? (Search “Goa government holiday list 2026” and use the official gazette or government site, not a blog.)

Restricted / optional holidays: what does “restricted holiday” mean? How many can an employee usually pick from the list? Give 2 festival examples that are usually on it.

Why the difference matters for Konkan: the head office (Panaji) and the plant (Verna) may follow different holiday lists. In one or two lines, explain why the HRMS must store holidays per location and mark each one as National, State or Optional.

One tricky case to think about: if an employee takes an optional holiday, should the app treat it as leave or as a holiday? Write your decision and the reason. You’ll implement exactly this.



\## 7. Late arrivals, half days, WFH and missed punches

Late arrivals



What is a “grace period” in attendance? Do most companies give one?

Konkan’s rule: check-in after 10:15 AM is Late, and 3 Late marks in a month = half a day of LOP. Work this out: Sana is late 7 times in March. How many LOP days does she get? Show your working.

Why do companies convert late marks into LOP instead of just warning people?



Half days



Konkan’s rule: 8+ hours = Present, 4 to under 8 hours = Half Day, under 4 hours = Absent. Classify these:

Checked in 9:30, out 14:00

Checked in 9:00, out 17:30

Checked in 10:00, out 13:30

What happens if someone takes half-day CL and works the other half?



Work from home



How do companies record a WFH day? At Konkan, how does WFH count (Present or something else)?

Should plant workers at Verna be allowed WFH? Give your opinion with a reason.



Missed punches



What is a missed punch? Give one real reason it happens.

What is “regularisation”? Describe the steps: who requests it, who approves it, and when the attendance actually changes.

Why shouldn’t the employee be able to fix their own attendance directly?



\## 8. Interview with a working professional

Name / role:

Date:

HR system used:

What they dislike:

Who: first name/role, company type, date of the call

System used and how they use it day to day

What they dislike: the 3 points, written as they told you

What I’ll do differently in my HRMS: link at least 2 of their complaints to a feature you’re building. For example, if they say “I never know my real balance”, that maps to your live balance on the dashboard.



\## 9. Sandwich rule

What is it? In one or two sentences: what happens to weekends or holidays that fall between leave days under a sandwich rule?

Worked example with a sandwich rule: a Panaji employee takes leave on Friday and Monday. How many leave days are deducted if the company applies the sandwich rule? Why?

Same example at Konkan (no sandwich rule): how many days are deducted? Why?

Same dates at the Verna plant (Saturday is a working day there): how many days? Explain.

Why do some companies use it? Give one reason from the employer’s side.

Why is it unpopular with employees? Give one reason.

One line for your app: where in your code will this rule show up? (Hint: the function that counts working days.)



\## 10. Konkan Coastal Foods leave policy (rules we will implement)

General



R1. Leave year: starts 1 January\_\_\_ and ends 31 December\_\_\_.

R2. Leave counts working days only, based on the employee’s location. Weekly offs: Panaji \_Sunday\_\_, Verna \_Sunday\_\_.

R3. Holidays and weekly offs inside a leave period are \_Not counted\_\_ (no sandwich rule).

R4. Leave cannot start more than \_30\_\_ days in the past.

R5. If the balance is not enough, the app offers to \_Apply for leave without pay\_\_.



Each leave type (one rule each for quota, accrual, carry forward and half day)



CL: \_12\_\_ days, credited on \_\_1 January\_, pro-rated for joiners, lapses on \_31 December\_\_, half day allowed: yes\_\_\_

SL: same format, plus: SL longer than \_3\_\_ consecutive days needs \_\_medical certificate\_

EL: \_\_15\_ days per completed month, carry forward up to \_30\_\_, half day allowed: yes\_\_\_

Maternity: \_\_26\_ weeks (from your Question 3 research, with the law named), half day: \_no\_\_

Paternity: \_5\_\_ days, company policy, half day: \_no\_\_

LOP: unlimited, half day: \_yes\_\_



Decisions you must make yourself (write each one plus a one-line reason; these are viva questions)



D1. Pro-rating formula: how do you count “remaining months” for someone joining on 15 July? Does July count?

D2. Rounding: do you round pro-rated leave to the nearest 0.5, always down, or always up?

D3. EL “completed month”: if someone joins on 20 January, when do they get their first 1.25 days?

D4. Is pending leave subtracted from the available balance straight away? (It should be, or people can over-apply.)

D5. Late rule: 3 Lates = 0.5 day LOP. What do 5 Lates give? (Hint: 3 → 0.5, 6 → 1.0.)



\## 11. Choice of AI model (free-tier limits)



\## Sources

