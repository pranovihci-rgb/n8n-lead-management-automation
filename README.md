# 🚀 Lead Management Automation

### Automated Lead Processing with n8n, Telegram & Google Sheets

A complete lead-management automation that receives customer requests, validates and classifies leads, stores them in Google Sheets, and allows managers to process leads directly from Telegram.

---

## 📌 Overview

This project demonstrates a real-world lead management workflow built with **n8n**.

The system automates the full lead lifecycle:

**Lead received → Validation → Classification → Storage → Telegram notification → Manager action → Status update**

The workflow was designed as an automation solution for a renovation company receiving customer requests from a website.

---

## 🏢 Business Problem

A company receiving leads manually has to:

- Copy customer information into spreadsheets
- Check whether the submitted data is valid
- Determine lead priority
- Notify managers
- Track which manager accepted the lead
- Update lead statuses manually

This creates repetitive work and increases the risk of losing or incorrectly processing leads.

---

## ⚙️ Solution

The automation handles the process automatically.

```text
Customer / Website
        │
        ▼
     Webhook
        │
        ▼
  Data Validation
        │
        ▼
  Google Sheets
        │
        ▼
Lead Classification
   │      │      │
   ▼      ▼      ▼
 HOT     NEW   INVALID
   │      │      │
   └──────┴──────┘
          │
          ▼
      Telegram
          │
    ┌─────┴─────┐
    ▼           ▼
  ACCEPT       REJECT
    │           │
    └─────┬─────┘
          ▼
    n8n Callback
          │
          ▼
 Duplicate Check
          │
          ▼
 Google Sheets Update
          │
          ▼
 Telegram Update
```

---

## ✨ Features

### 🔗 Lead Intake

The workflow receives customer requests through an **HTTP POST Webhook**.

Incoming data includes:

- Customer name
- Phone number
- Repair type
- Budget
- Comment

Each request automatically receives a unique Lead ID.

Example:

```text
LEAD-20260902-142602
```

### ✅ Data Validation

Incoming data is validated before processing.

The workflow checks:

- Customer name
- Phone number
- Budget
- Data format

Invalid requests are automatically classified as:

```text
INVALID
```

### 🔥 Automatic Lead Classification

Valid leads are classified according to business rules.

```text
Budget >= 30,000 PLN
→ HOT LEAD

Budget < 30,000 PLN
→ NEW LEAD
```

The classification rules can easily be changed depending on business requirements.

### 📊 Google Sheets CRM

Google Sheets is used as a lightweight CRM/database.

The system stores:

```text
lead_id
name
phone
repair_type
budget
comment
status
manager_status
manager_name
manager_username
manager_id
created_at
updated_at
```

### 🤖 Telegram Notifications

Managers receive lead information directly in Telegram.

Example:

```text
🔥 HOT LEAD

👤 Name: Alex
📞 Phone: 48789123456
🏠 Repair type: Apartment
💰 Budget: 65000 PLN
💬 Comment: Full apartment renovation
```

For actionable leads, the manager can process the lead using Telegram inline buttons:

```text
✅ Take Lead     ❌ Reject
```

### 👨‍💼 Manager Tracking

When a manager processes a lead, the workflow records:

- Manager name
- Telegram username
- Telegram ID
- Processing timestamp
- Lead status

Accepted leads receive:

```text
IN_PROGRESS
```

Rejected leads receive:

```text
REJECTED
```

### 🛡 Duplicate Processing Protection

Before changing a lead status, the workflow checks whether another manager has already processed it.

This prevents the same lead from being handled multiple times.

For example:

```text
⚠️ This lead has already been processed
```

### 🔄 Telegram Message Update

After processing a lead, the original Telegram message is automatically updated.

Example:

```text
────────────
✅ IN PROGRESS
👨‍💼 Manager: Igor
🕐 Taken: 02.09.2026 14:53
```

or:

```text
────────────
❌ REJECTED
👨‍💼 Manager: Igor
🕐 Rejected: 02.09.2026 14:54
```

---

# 📸 Screenshots

## n8n Workflow

![n8n Workflow](screenshots/01-workflow.png)

## Telegram — New HOT Lead

![Telegram HOT Lead](screenshots/02-telegram-hot-lead.png)

## Telegram — Processed Lead

![Telegram Processed Lead](screenshots/03-telegram-processed-lead.png)

## Google Sheets — Lead Database

![Google Sheets CRM](screenshots/04-google-sheets.png)

---

## 🔄 Workflow Logic

### Lead Intake

```text
Webhook
   ↓
Edit Fields
   ↓
Append to Google Sheets
   ↓
Validate
   ↓
HOT / NEW / INVALID
   ↓
Telegram Notification
   ↓
Webhook Response
```

### Manager Processing

```text
Telegram Callback
       ↓
Parse Callback
       ↓
Find Lead
       ↓
Check Already Processed
       ↓
Route Action
     ↙       ↘
   TAKE     REJECT
     ↓         ↓
IN_PROGRESS  REJECTED
     ↓         ↓
Google Sheets Update
     ↓
Telegram Message Update
```

---

## 🛠 Technology Stack

| Technology | Purpose |
|---|---|
| **n8n** | Workflow automation |
| **Telegram Bot API** | Manager notifications and actions |
| **Google Sheets** | Lead storage / lightweight CRM |
| **Webhooks** | Receiving incoming leads |
| **JSON** | Data exchange |
| **JavaScript Expressions** | Validation and workflow logic |

---

## 🧠 n8n Concepts Used

This project demonstrates practical experience with:

- Webhook Trigger
- Telegram Trigger
- Google Sheets integration
- IF conditions
- Switch routing
- Edit Fields
- Expressions
- Callback Queries
- Dynamic data mapping
- Data validation
- Branching
- API responses
- Workflow state management
- Duplicate-processing protection

---

## 📥 Importing the Workflow

The sanitized workflow template is included in this repository:

```text
lead-management-workflow-sanitized.json
```

To use it:

1. Download the JSON workflow.
2. Open n8n.
3. Import the workflow.
4. Connect your Google Sheets credentials.
5. Connect your Telegram Bot credentials.
6. Replace:

```text
YOUR_GOOGLE_SHEET_ID
YOUR_TELEGRAM_CHAT_ID
```

7. Configure the Google Sheets columns.
8. Test the workflow.
9. Activate it.

---

## 🔐 Security

The public version of the workflow has been sanitized.

The repository does **not intentionally include**:

- Telegram Bot tokens
- Google OAuth secrets
- Private credential IDs
- Personal Telegram Chat IDs
- Private Google Sheet IDs

Credentials should always be configured directly inside n8n and should never be committed to a public repository.

---

## 🚀 Future Improvements

Possible production improvements:

- PostgreSQL instead of Google Sheets
- CRM integration
- Automatic manager assignment
- AI-powered lead qualification
- Lead scoring
- Follow-up reminders
- Email/SMS notifications
- Analytics dashboard
- Error handling and logging
- SLA monitoring

---

## 🎯 Project Goal

The goal of this project was to build a practical automation based on a realistic business scenario rather than a simple tutorial workflow.

It demonstrates how **n8n can connect APIs, business logic, data storage and messaging platforms into a complete automated process.**

---

## 👤 Author

**Ihar Pranovich**

Automation / AI Automation Portfolio Project
