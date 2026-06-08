# n8n AI-Powered Recruitment Pipeline

A production-ready recruitment automation system built with n8n and Google Gemini that screens CVs, scores candidates, sends daily HR digest emails with Approve/Reject buttons, and automatically emails candidates based on the HR decision — all secured with one-time-use tokens.

## Demo

> 📺 [Watch the demo video](https://www.linkedin.com/posts/dzakies_n8n-automation-aiautomation-ugcPost-7469427858864685056-Vrpr/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAEkGSHQBQ8_SUmtEy8OMzAQ6je8LEhFimDU)

---

## The Problem It Solves

A company posts a job and receives 200 applications in 3 days. An HR manager has to open every CV, read it, decide if it's worth interviewing, email the candidate, and update a tracker — for every single applicant. This system automates the entire screening and communication layer while keeping humans in control of the final decision.

---

## How It Works

### Workflow 1 — Application Screener (triggers instantly)

```
Candidate submits form + uploads CV (PDF)
       ↓
Google Gemini 2.5 Pro reads the CV and scores candidate 0–100
       ↓
AI generates strengths summary and concerns
       ↓
All data saved to Google Sheets CRM (status: Pending Review)
       ↓
Confirmation email sent to candidate
```

### Workflow 2 — Daily HR Digest (runs every day at 9am)

```
Fetch all candidates with status "Pending Review"
       ↓
Build one digest email with ALL pending candidates
       ↓
Each candidate card shows: score, AI strengths, concerns, recommendation
       ↓
HR clicks Approve or Reject button per candidate
       ↓
Webhook fires → candidate receives interview invite or rejection email
       ↓
Google Sheets updated, token cleared
```

---

## Features

- **AI CV screening** — Google Gemini 2.5 Pro reads uploaded PDF CVs and evaluates candidates against job requirements
- **Automated scoring** — each candidate scored 0–100 with strengths and concerns summary
- **Daily digest email** — HR receives ONE email per day with all pending candidates, not one email per applicant
- **Human-in-the-loop** — AI recommends, HR decides, system executes
- **Token-secured approval links** — each Approve/Reject button is a one-time-use secured link, preventing duplicate actions
- **Duplicate application handling** — token matching by row ensures the correct application is always updated
- **Instant candidate emails** — approved candidates get interview invites, rejected get professional rejection emails
- **Google Sheets CRM** — full candidate tracker with name, email, score, status, strengths, concerns, applied date

---

## Tech Stack

| Tool | Purpose |
|---|---|
| n8n | Workflow automation engine |
| Google Gemini 2.5 Pro | CV reading and candidate scoring |
| Gmail | HR digest emails + candidate communication |
| Google Sheets | Candidate CRM / applicant tracker |
| JavaScript | Token generation, data parsing, digest HTML builder |

---

## Workflows

| File | Description |
|---|---|
| `recruitment-pipeline.json` | Main workflow — form → AI screen → Sheets → candidate email |
| `daily-hr-digest.json` | Digest + decision handler — schedule, digest email, approve/reject webhooks |

---

## How to Use

1. Import both JSON files into your n8n instance
2. Connect credentials: Gmail, Google Sheets, Google Gemini API
3. Update the Google Sheets node to point to your spreadsheet
4. Update HR email address in the Gmail digest node
5. Publish both workflows (required for production webhook URLs)
6. Share the application form URL with candidates

---

## Security — Token System

Each candidate application generates a unique token stored in Google Sheets. The Approve/Reject URLs embed this token as a query parameter:

```
/webhook/approve?email=candidate@gmail.com&name=John&token=abc123xyz
```

When HR clicks the button:
1. Webhook receives the token
2. System fetches the matching row from Sheets by token
3. If token matches → action executes, token cleared
4. If token not found → request rejected

This prevents: duplicate approvals, stale link reuse, and wrong-row updates for duplicate applicants.

---

## Google Sheets Structure

| Column | Filled by |
|---|---|
| Name, Email, Phone, Experience, Skills | Form (automatic) |
| Score, Status, Strengths, Concern, AI Suggest, Applied_at | AI (automatic) |
| Token | System (automatic, cleared after decision) |
| Action, Interview_Date | HR (manual) |

---

## Results

- ✅ ~90% reduction in manual HR screening time
- ✅ Every applicant gets a response in under 30 seconds
- ✅ Zero missed candidates
- ✅ HR reviews all candidates in one daily session instead of constant interruptions
- ✅ One-time token links prevent any duplicate or unauthorized actions

---

## Author

**Dzaki Endraghani Sunarko**
AI Automation Specialist | n8n & Google Gemini Workflow Developer

- LinkedIn: [linkedin.com/in/dzakies](https://linkedin.com/in/dzakies)
- GitHub: [github.com/DzakiES](https://github.com/DzakiES)
- Email: dzakies2003@gmail.com

---

> Built as a portfolio project demonstrating production-ready AI workflow automation with human-in-the-loop decision making.
