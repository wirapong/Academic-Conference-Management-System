# 🎓 Academic Conference Management System

> **A fully-featured, serverless conference management platform built entirely on Google Apps Script + Google Sheets — no backend server or database required.**

[![Google Apps Script](https://img.shields.io/badge/Google%20Apps%20Script-4285F4?style=flat&logo=google&logoColor=white)](https://script.google.com)
[![Google Sheets](https://img.shields.io/badge/Google%20Sheets-34A853?style=flat&logo=google-sheets&logoColor=white)](https://sheets.google.com)
[![Google Drive](https://img.shields.io/badge/Google%20Drive-4285F4?style=flat&logo=google-drive&logoColor=white)](https://drive.google.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [System Architecture](#-system-architecture)
- [File Structure](#-file-structure)
- [Prerequisites](#-prerequisites)
- [Installation & Deployment](#-installation--deployment)
- [Configuration](#-configuration)
- [User Roles & Permissions](#-user-roles--permissions)
- [Module Documentation](#-module-documentation)
  - [Authentication](#authentication)
  - [Paper Submission](#paper-submission)
  - [Peer Review](#peer-review)
  - [Registration & Payment](#registration--payment)
  - [Conference Schedule](#conference-schedule)
  - [Admin Dashboard & Reports](#admin-dashboard--reports)
  - [Settings Management](#settings-management)
- [Database Schema](#-database-schema)
- [API Reference](#-api-reference)
- [Customization Guide](#-customization-guide)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🌟 Overview

This system was originally developed for the **7th National Conference on Thai Information Science Education 2026 (TISE-2026)**, hosted by the Department of Information Science, Faculty of Humanities and Social Sciences, Khon Kaen University.

It provides a complete end-to-end workflow for academic conference management, from paper submission through peer review, author notification, camera-ready collection, and participant registration — all running on Google's free infrastructure with zero hosting costs.

### ✨ Key Highlights

| Feature | Detail |
|---------|--------|
| **Zero hosting cost** | Runs entirely on Google Apps Script (free tier) |
| **No database needed** | Google Sheets acts as the database |
| **File storage** | Google Drive for all uploaded files |
| **Email** | Gmail API for automated notifications |
| **Single-file frontend** | Pure vanilla JS, no frameworks required |
| **Mobile-friendly** | Responsive design works on phones and tablets |
| **Bilingual** | Thai + English interface |

---

## 🚀 Features

### For Authors
- 📝 Paper submission with file upload (.doc / .docx)
- 🏷️ Track selection from admin-configured list
- 🎤 Presentation type selection (Oral / Poster)
- 📊 Paper status tracking dashboard
- 💬 View double-blind reviewer feedback
- 📤 Revised manuscript & Camera-Ready file submission
- 🗓️ Registration with payment slip upload

### For Reviewers
- 📋 Assigned papers list with abstract preview
- ⭐ Structured review form (5 criteria scored 1–5)
- 💡 Recommendation selection (Accept / Reject / Minor Revision / Major Revision)
- 🎓 Automatic Certificate of Appreciation (PDF) issued per review
- 🔄 Edit review after submission (re-issue certificate supported)
- 📜 Review history and detail page

### For Admins
- 👥 Full user management (role assignment, enable/disable)
- 📄 All Papers management with status workflow
- 🔀 Reviewer assignment per paper
- 📆 Conference schedule builder (Add / Edit / Delete sessions)
- 📊 Reports & Analytics dashboard
  - Paper submission statistics
  - Review progress
  - Registration counts and revenue by type
  - Track distribution charts
- 💰 Participant management with payment verification
- 📢 Announcement broadcast to all users
- ⚙️ Settings panel (all conference details editable)

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      USER BROWSER                           │
│  index.html → styles.html + app.html (vanilla JS SPA)       │
└────────────────────┬────────────────────────────────────────┘
                     │  google.script.run.dispatchApi(payload)
                     ▼
┌─────────────────────────────────────────────────────────────┐
│              GOOGLE APPS SCRIPT (Code.gs)                   │
│                                                             │
│  dispatchApi(payloadJson)                                   │
│    └─→ action routing → handler functions                   │
│         ├─ Auth handlers                                    │
│         ├─ Paper handlers                                   │
│         ├─ Review handlers                                  │
│         ├─ Participant handlers                             │
│         ├─ Schedule handlers                                │
│         ├─ Settings handlers                                │
│         └─ Report handlers                                  │
└──────┬──────────────────────────┬───────────────────────────┘
       │                          │
       ▼                          ▼
┌─────────────┐         ┌─────────────────────┐
│ Google      │         │ Google Drive        │
│ Sheets      │         │                     │
│ (Database)  │         │ - Manuscripts       │
│             │         │ - Payment Slips      │
│ 7 sheets:   │         │ - Certificates (PDF) │
│ Users       │         │ - Revised Papers     │
│ Papers      │         └─────────────────────┘
│ Reviews     │
│ Participants│         ┌─────────────────────┐
│ Schedule    │         │ Gmail API           │
│ Settings    │         │                     │
│ EmailLog    │         │ - Registration conf │
└─────────────┘         │ - Review certs      │
                        │ - Payment confirm   │
                        │ - Announcements     │
                        └─────────────────────┘
```

### Request/Response Flow

```
Browser                    GAS Server
   │                           │
   │── api('action', data) ───▶│
   │                           │── dispatchApi(JSON)
   │                           │── validate user session
   │                           │── execute handler
   │                           │── read/write Sheets
   │◀── { success, ...data } ──│
   │                           │
```

---

## 📁 File Structure

```
conference-system/
├── Code.gs          # Backend: all server-side logic (2,300+ lines)
├── index.html       # GAS HTML entry point (loads styles + app)
├── styles.html      # CSS stylesheet (injected via <?!= include() ?>)
├── app.html         # Frontend SPA: 100+ JS functions (3,100+ lines)
└── README.md        # This file
```

### File Responsibilities

| File | Responsibility |
|------|---------------|
| `Code.gs` | API routing, data access, authentication, file upload, email sending, PDF generation |
| `index.html` | HTML shell, loads CSS and JS via GAS templating |
| `styles.html` | All CSS variables, component styles, responsive layout |
| `app.html` | Full SPA logic: routing, API calls, DOM rendering, form handling |

---

## 📦 Prerequisites

- A **Google Account** (Google Workspace or personal Gmail)
- Access to **Google Sheets**, **Google Drive**, and **Google Apps Script**
- Basic familiarity with Google Apps Script deployment
- No external dependencies — everything runs in Google's ecosystem

---

## 🛠️ Installation & Deployment

### Step 1: Create the Google Apps Script Project

1. Go to [script.google.com](https://script.google.com) and click **New project**
2. Rename the project (e.g., `Conference Management System`)

### Step 2: Add the Files

Create the following files in the Apps Script editor:

| File to create | Type | Content from |
|----------------|------|-------------|
| `Code.gs` | Script | `Code.gs` |
| `index.html` | HTML | `index.html` |
| `styles.html` | HTML | `styles.html` |
| `app.html` | HTML | `app.html` |

> **Note:** The default file is called `Code.gs`. For HTML files, click the **+** button → **HTML**.

### Step 3: Enable Required Services

In the Apps Script editor, go to **Services** (➕ icon on left panel) and enable:

- ✅ **Drive API** (for file uploads and PDF storage)

The following are used by default and don't need enabling:
- SpreadsheetApp (built-in)
- MailApp (built-in)
- DriveApp (built-in)
- Utilities (built-in)

### Step 4: Initialize the System

1. In the editor, select the `menuInitSystem` function from the function dropdown
2. Click **▶ Run**
3. Grant the required permissions when prompted
4. This creates:
   - A new Google Spreadsheet (linked to the script)
   - All 7 sheets with correct headers
   - Default settings values
   - A Google Drive folder named `TISE-2026` for file uploads

### Step 5: Create the Admin Account

1. Run the `menuCreateAdmin` function
2. A default admin account will be created:
   ```
   Email:    wirach@kku.ac.th
   Password: Admin@2026
   Role:     admin
   ```
3. **Change the password immediately** after first login

> To customize the admin email/password before running, edit the `menuCreateAdmin` function in `Code.gs`.

### Step 6: Deploy as Web App

1. Click **Deploy** → **New deployment**
2. Select type: **Web app**
3. Configure:
   - **Description:** `v1.0` (or any label)
   - **Execute as:** `Me`
   - **Who has access:** `Anyone` (for public conference access)
4. Click **Deploy** and copy the **Web App URL**
5. Share this URL with conference participants

> ⚠️ **Important:** Every time you modify `Code.gs`, you must create a **New Deployment** — editing an existing deployment does NOT update the running code.

---

## ⚙️ Configuration

All conference settings are managed through the **Settings** page in the admin panel, or directly in the Google Sheets `Settings` tab.

### Conference Information

| Setting Key | Description | Example |
|-------------|-------------|---------|
| `Conference Name` | Full conference name | `7th National Conference on Thai Information Science Education 2026` |
| `Short Name` | Abbreviated name | `TISE-2026` |
| `Conference Dates` | Display date range | `21-22 August 2026` |
| `Venue` | Location | `College of Local Administration, Khon Kaen University` |
| `Chair Name` | Program Committee Chair | `Prof. Dr. Conference Chair` |
| `Chair Email` | Chair's email | `chair@conference.ac.th` |
| `Contact Email` | Public contact email | `info@conference.ac.th` |
| `Website` | Conference website URL | `https://conference.ac.th` |

### Key Dates

| Setting Key | Description |
|-------------|-------------|
| `Submission Deadline` | Paper submission closes (YYYY-MM-DD) |
| `Review Deadline` | Reviewer completion deadline (YYYY-MM-DD) |
| `Notification Date` | Author notification date (YYYY-MM-DD) |
| `Camera Ready Deadline` | Camera-ready submission deadline (YYYY-MM-DD) |

### Registration Fees

| Setting Key | Default | Description |
|-------------|---------|-------------|
| `Fee: Presenter - General` | 2,500 | อาจารย์ นักวิจัย บรรณารักษ์ และบุคคลทั่วไป |
| `Fee: Presenter - Graduate` | 1,500 | นักศึกษาระดับบัณฑิตศึกษา |
| `Fee: Presenter - Undergraduate` | 1,000 | นักศึกษาระดับปริญญาตรี |
| `Fee: Attendee - General` | 2,000 | อาจารย์ นักวิจัย บรรณารักษ์ และบุคคลทั่วไป |
| `Fee: Attendee - Graduate` | 1,000 | นักศึกษาระดับบัณฑิตศึกษา |
| `Fee: Attendee - Undergraduate` | 500 | นักศึกษาระดับปริญญาตรี |

### Tracks (Paper Submission Categories)

Tracks are stored as a comma-separated list in the `Tracks` setting key:

```
Track 1: Information and Library Science Education, Track 2: AI and Emerging Technologies for Information and Education, Track 3: Information Management Information Services and Professional Development
```

Admins can add, remove, or rename tracks from the Settings page without any code changes.

---

## 👥 User Roles & Permissions

The system supports three roles, each with distinct access levels:

```
┌─────────────────────────────────────────────────────────────┐
│  ADMIN                                                      │
│  Full system access                                         │
│  ├── All Author + Reviewer capabilities                     │
│  ├── User management (role, enable/disable)                 │
│  ├── All Papers (view, update status, assign reviewers)     │
│  ├── Participants management (payment verification)         │
│  ├── Schedule management (CRUD sessions)                    │
│  ├── Announcements broadcast                                │
│  ├── Reports & Analytics                                    │
│  └── Settings management                                    │
├─────────────────────────────────────────────────────────────┤
│  REVIEWER                                                   │
│  Review-focused access                                      │
│  ├── View assigned papers                                   │
│  ├── Submit and edit reviews                                │
│  ├── Request Certificate of Appreciation                    │
│  └── View public schedule                                   │
├─────────────────────────────────────────────────────────────┤
│  AUTHOR (default for new registrations)                     │
│  Submission-focused access                                  │
│  ├── Submit papers                                          │
│  ├── Track paper status                                     │
│  ├── View reviewer feedback (double-blind)                  │
│  ├── Upload revised / camera-ready files                    │
│  └── Registration & payment slip upload                     │
└─────────────────────────────────────────────────────────────┘
```

### Paper Status Workflow

```
[Submitted] → [Under Review] → [Accepted] → [Camera Ready]
                    │
                    ├─→ [Revision Required] → [Under Review]
                    │
                    └─→ [Rejected]

[Any status] → [Withdrawn]  (by author or admin)
[Withdrawn]  → [Submitted]  (restore by admin)
```

### Payment Status Workflow

```
[Pending] → [Slip Submitted] → [Paid]
                │
                └─→ [Rejected]

[Paid] → [Pending]  (admin can revert if marked paid by mistake)
```

---

## 📚 Module Documentation

### Authentication

**Files:** `Code.gs` (handleLogin, handleRegister, handleForgotPassword)

Users authenticate via email + password. Sessions are managed using a token stored in the browser's `sessionStorage`. The token is validated on every API call.

**Registration flow:**
1. User submits email + password + name + affiliation
2. System checks for duplicate email
3. Password is hashed (SHA-256 via `Utilities.computeDigest`)
4. Account created with `author` role by default
5. Admin must manually upgrade to `reviewer` or `admin`

**Forgot Password:**
- Generates a 6-digit OTP stored in the Users sheet
- Sends OTP to user's email via Gmail
- User enters OTP + new password to reset

---

### Paper Submission

**Files:** `Code.gs` (handleSubmitPaper, handleUploadManuscript), `app.html` (renderSubmitPaperPage, doSubmitPaper)

**Submission fields:**
- Title, Authors, Affiliation, Email
- Abstract, Keywords
- Track (dropdown from Settings)
- Presentation Type (Oral / Poster) — visual card selector
- File upload (.doc / .docx, max configurable size)

**File upload process:**
1. User selects file → FileReader converts to Base64
2. Frontend calls `uploadManuscript` API with Base64 data
3. Backend decodes Base64 → creates Drive file in conference folder
4. Drive file set to "Anyone with link can view"
5. File URL stored in Papers sheet

**Resubmission:**
- Authors can upload **Revised** version after "Revision Required" decision
- Authors can upload **Camera-Ready** version after acceptance
- Both use the same upload mechanism with different column targets

---

### Peer Review

**Files:** `Code.gs` (handleSubmitReview, handleGetReviewDetail, sendCertificateEmail), `app.html` (loadReviewForm, loadReviewDetail)

**Review criteria (each scored 1–5):**
- Originality
- Technical Quality
- Clarity of Presentation
- Significance of Contribution
- Overall

**Recommendations:**
- Accept
- Accept with Minor Revision
- Major Revision Required
- Reject

**Certificate of Appreciation system:**
- One certificate per review (enforced by `Reviews` sheet col 22 = `Cert Sent` timestamp)
- PDF generated via `HtmlService` → `DriveApp` (A4 landscape)
- Sent as email attachment automatically on review submission
- Re-issue supported via `resendCertificate` API (forceResend=true bypasses duplicate check)
- Reviewers can self-service request certificate from Review Detail page

**Double-blind implementation:**
- Reviewer names are NOT shown to authors
- `handleGetMyPaperReviews` strips reviewer identity from results
- Author sees: scores, summary, comments — but not reviewer name/email

---

### Registration & Payment

**Files:** `Code.gs` (handleRegisterParticipant, handleUploadPaymentSlip, handleUpdatePaymentStatus), `app.html` (loadRegistration, doUploadSlip, openPaymentModal)

**Registration flow:**
1. User navigates to Registration page
2. System fetches fee categories from Settings dynamically
3. User selects registration type → fee displayed instantly
4. User submits → record created in Participants sheet with `Pending` status
5. Confirmation email sent automatically

**Payment slip upload:**
- Accepts: JPG, JPEG, PNG, PDF (max 5 MB)
- Validated client-side (instant feedback) AND server-side
- Uploaded to Google Drive, URL stored in Participants sheet col 17
- Status auto-updated to `Slip Submitted`

**Admin payment management (modal):**
- View slip (opens in new tab)
- Update status: Pending / Slip Submitted / Paid / Rejected
- Email sent automatically:
  - `Paid`: confirmation email with registration details
  - `Rejected`: notification email asking to resubmit
- Admins can revert `Paid → Pending` if confirmed by mistake

---

### Conference Schedule

**Files:** `Code.gs` (handleGetSchedule, handleAddSession, handleUpdateSession, handleDeleteSession), `app.html` (loadSchedule, showAddSessionModal, showEditSessionModal)

**Session types:**
- Keynote
- Technical Session
- Workshop
- Panel Discussion
- Coffee Break
- Lunch

**Known limitation — GAS time conversion:**
Google Sheets auto-converts `HH:MM` strings to Date objects. The system handles this with:
- `setNumberFormat('@STRING@')` applied BEFORE writing time cells
- `safe()` converter in `handleGetSchedule` detects 1899-12-30 epoch dates and formats as `HH:MM`
- Menu item **"🕐 Fix Schedule Times"** repairs existing rows if data was entered before this fix

**Display features:**
- Sessions grouped by date, sorted by start time
- Date displayed as full format: "Thursday, 21 August 2026"
- Session type color-coded badges
- Admin can Edit (✏️) and Delete (🗑️) sessions inline

---

### Admin Dashboard & Reports

**Files:** `Code.gs` (handleGetDashboardStats, handleGetDetailedReport), `app.html` (loadDashboard, loadReports)

**Dashboard stat cards:**
- Total Submitted / Active
- Accepted (includes Camera Ready)
- Rejected (shows Revision Required count)
- Under Review
- Oral presentations
- Poster presentations

**Reports page sections:**

1. **Paper stat cards** — 6 cards with rates
2. **Status Breakdown** — horizontal bar chart per status
3. **Presentation Type** — Oral vs Poster progress bars
4. **Review Progress** — completion percentage + counts
5. **Registration Summary:**
   - 4 stat boxes (Total / Paid / Slip Submitted / Pending)
   - 2 fee summary boxes (Expected total vs Received total)
   - Breakdown table by registration type (7 columns)
   - Bar chart by type showing proportions
6. **Active Submissions by Track** — ranked bar chart
7. **Withdrawn Papers** — table with restore action

---

### Settings Management

**Files:** `Code.gs` (handleGetSettingsAPI, handleSaveSettings), `app.html` (loadSettings, doSaveSettings)

Settings are stored as key-value pairs in the `Settings` sheet (3 columns: Key, Value, Note).

The `handleSaveSettings` function uses **upsert logic** — existing keys are updated in-place; new keys are appended. This means you can add custom settings without breaking existing ones.

**Settings page sections:**
- Conference Information (name, dates, venue, contact)
- Key Dates (submission, review, notification, camera-ready deadlines)
- Tracks (comma-separated, with live preview)
- Registration Fees (6 inputs: Presenter × 3 levels, Attendee × 3 levels)

---

## 🗄️ Database Schema

All data is stored in Google Sheets. Each sheet has a fixed column structure.

### Users Sheet (11 columns)

| Col | Field | Description |
|-----|-------|-------------|
| 1 | User ID | `U-XXXXXXXX` generated ID |
| 2 | Name | Full name |
| 3 | Email | Login email (unique) |
| 4 | Password Hash | SHA-256 hex digest |
| 5 | Role | `author` / `reviewer` / `admin` |
| 6 | Affiliation | Institution/Organization |
| 7 | Country | Country |
| 8 | Created Date | `YYYY-MM-DD HH:MM:SS` |
| 9 | Status | `Active` / `Disabled` |
| 10 | Reset Token | OTP for password reset |
| 11 | Token Expiry | Expiry datetime for OTP |

### Papers Sheet (22 columns)

| Col | Field | Description |
|-----|-------|-------------|
| 1 | Paper ID | `P-XXXXXXXX` |
| 2 | Title | Paper title |
| 3 | Authors | Author names |
| 4 | Affiliation | Author institution |
| 5 | Email | Corresponding author email |
| 6 | Abstract | Full abstract text |
| 7 | Keywords | Comma-separated keywords |
| 8 | Track | Selected track name |
| 9 | File URL | Google Drive URL of manuscript |
| 10 | Submission Date | `YYYY-MM-DD HH:MM:SS` |
| 11 | Status | `Submitted` / `Under Review` / `Accepted` / `Rejected` / `Revision Required` / `Camera Ready` / `Withdrawn` |
| 12 | Author ID | User ID of submitting author |
| 13 | Reviewer 1 | User ID |
| 14 | Reviewer 2 | User ID |
| 15 | Reviewer 3 | User ID |
| 16 | Avg Score | Computed average review score |
| 17 | Camera Ready URL | Google Drive URL |
| 18 | Decision | Text decision note |
| 19 | Decision Date | Date of final decision |
| 20 | Comments | Admin comments to author |
| 21 | Presentation Type | `Oral` / `Poster` |
| 22 | Revised File URL | Google Drive URL of revised manuscript |

### Reviews Sheet (22 columns)

| Col | Field | Description |
|-----|-------|-------------|
| 1 | Review ID | `RV-XXXXXXXX` |
| 2 | Paper ID | Reference to Papers |
| 3 | Paper Title | Cached title |
| 4 | Reviewer ID | Reference to Users |
| 5 | Reviewer Name | Cached name |
| 6 | Assigned Date | Date assigned |
| 7 | Due Date | Review deadline |
| 8 | Submitted Date | When review was submitted |
| 9 | Status | `Assigned` / `Completed` |
| 10 | Originality | Score 1–5 |
| 11 | Technical | Score 1–5 |
| 12 | Clarity | Score 1–5 |
| 13 | Significance | Score 1–5 |
| 14 | Overall | Score 1–5 |
| 15 | Avg Score | Average of 5 criteria |
| 16 | Summary | Review summary text |
| 17 | Strengths | Strengths text |
| 18 | Weaknesses | Weaknesses text |
| 19 | Comments to Author | Public comments |
| 20 | Comments to Chair | Confidential comments |
| 21 | Recommendation | `Accept` / `Reject` / `Accept with Minor Revision` / `Major Revision Required` |
| 22 | Cert Sent | Datetime certificate was issued (empty = not sent) |

### Participants Sheet (17 columns)

| Col | Field | Description |
|-----|-------|-------------|
| 1 | Participant ID | `PT-XXXXXXXX` |
| 2 | Name | Full name |
| 3 | Email | Email address |
| 4 | Affiliation | Institution |
| 5 | Country | Country |
| 6 | Role | Always `Attendee` |
| 7 | Type | e.g., `Presenter - General` |
| 8 | Reg Date | `YYYY-MM-DD HH:MM:SS` |
| 9 | Payment Status | `Pending` / `Slip Submitted` / `Paid` / `Rejected` |
| 10 | Fee | Amount in THB |
| 11 | Dietary | Dietary requirements |
| 12 | T-Shirt | Reserved (currently unused) |
| 13 | Paper | Paper ID if presenter |
| 14 | Session | Assigned session (optional) |
| 15 | Confirmed | `Yes` / `No` |
| 16 | Emergency Contact | Emergency contact info |
| 17 | Payment Slip URL | Google Drive URL of payment evidence |

### Schedule Sheet (11 columns)

| Col | Field | Description |
|-----|-------|-------------|
| 1 | Session ID | `S-XXXXXXXX` |
| 2 | Date | `YYYY-MM-DD` (stored as text) |
| 3 | Start Time | `HH:MM` (stored as text) |
| 4 | End Time | `HH:MM` (stored as text) |
| 5 | Room/Venue | Room name |
| 6 | Session Type | Keynote / Technical Session / Workshop / etc. |
| 7 | Title | Session title |
| 8 | Chair | Session chair name |
| 9 | Speakers | Speaker names |
| 10 | Paper IDs | Comma-separated paper IDs |
| 11 | Description | Additional notes |

### Settings Sheet (3 columns)

| Col | Field | Description |
|-----|-------|-------------|
| 1 | Key | Setting name |
| 2 | Value | Setting value |
| 3 | Note | Human-readable description |

### EmailLog Sheet (7 columns)

| Col | Field | Description |
|-----|-------|-------------|
| 1 | Log ID | `ML-XXXXXXXX` |
| 2 | Date | Datetime sent |
| 3 | To | Recipient email |
| 4 | Name | Recipient name |
| 5 | Type | Email type (Certificate, Registration, etc.) |
| 6 | Subject | Email subject |
| 7 | Status | `Sent+PDF` / `Sent+Text` |

---

## 🔌 API Reference

All API calls go through a single entry point: `dispatchApi(payloadJson)`

**Request format:**
```json
{
  "action": "actionName",
  "token": "session-token",
  "param1": "value1",
  "param2": "value2"
}
```

**Response format:**
```json
{
  "success": true,
  "data": "...",
  "error": null
}
```

### Authentication APIs

| Action | Parameters | Auth Required | Description |
|--------|-----------|---------------|-------------|
| `login` | `email`, `password` | No | Login and get session token |
| `register` | `name`, `email`, `password`, `affiliation`, `country` | No | Create new account |
| `forgotPassword` | `email` | No | Send OTP to email |
| `getProfile` | — | Yes | Get current user profile |
| `updateProfile` | `name`, `affiliation`, `country` | Yes | Update profile |
| `changePassword` | `currentPassword`, `newPassword` | Yes | Change password |

### Paper APIs

| Action | Parameters | Role | Description |
|--------|-----------|------|-------------|
| `submitPaper` | `title`, `authors`, `abstract`, `keywords`, `track`, `presentationType`, `fileUrl` | Author | Submit new paper |
| `getMyPapers` | — | Author | Get own papers |
| `getAllPapers` | — | Admin | Get all papers |
| `getPaperDetail` | `paperId` | Any | Get single paper |
| `updatePaperStatus` | `paperId`, `status`, `track`, `presentationType`, `decision`, `comments` | Admin | Update paper status |
| `withdrawPaper` | `paperId` | Author/Admin | Withdraw paper |
| `restorePaper` | `paperId` | Admin | Restore withdrawn paper |
| `resubmitPaper` | `paperId`, `resubType`, `fileUrl` | Author | Upload revised/camera-ready |
| `uploadManuscript` | `fileName`, `fileData`, `mimeType`, `uploadType` | Any | Upload file to Drive |

### Review APIs

| Action | Parameters | Role | Description |
|--------|-----------|------|-------------|
| `assignReviewer` | `paperId`, `reviewerId`, `slot` | Admin | Assign reviewer to paper |
| `getMyReviews` | — | Reviewer | Get assigned reviews |
| `getReviewDetail` | `reviewId` | Reviewer | Get single review |
| `submitReview` | `reviewId`, `scores`, `recommendation`, `summary`, `strengths`, `weaknesses`, `commentsToAuthor`, `commentsToChair` | Reviewer | Submit/update review |
| `getMyPaperReviews` | `paperId` | Author | Get own paper's reviews (blind) |
| `getReviewsForPaper` | `paperId` | Admin | Get all reviews for paper |
| `resendCertificate` | `reviewId` | Any | Re-issue appreciation certificate |

### Registration APIs

| Action | Parameters | Role | Description |
|--------|-----------|------|-------------|
| `getRegistrationFees` | — | Any | Get fee categories from settings |
| `registerParticipant` | `registrationType`, `dietary`, `presentingPaper`, `emergencyContact` | Author | Register for conference |
| `getMyRegistration` | — | Author | Get own registration |
| `uploadPaymentSlip` | `fileUrl`, `fileName` | Author | Save payment slip URL |
| `getAllParticipants` | — | Admin | Get all participants |
| `updatePaymentStatus` | `participantId`, `paymentStatus` | Admin | Update payment status |

### Schedule APIs

| Action | Parameters | Role | Description |
|--------|-----------|------|-------------|
| `getSchedule` | — | Any | Get all sessions |
| `addSession` | `date`, `startTime`, `endTime`, `room`, `type`, `title`, `chair`, `speakers`, `description` | Admin | Add session |
| `updateSession` | `sessionId`, + all fields | Admin | Update session |
| `deleteSession` | `sessionId` | Admin | Delete session |

### Admin APIs

| Action | Parameters | Role | Description |
|--------|-----------|------|-------------|
| `getDashboardStats` | — | Admin/Author | Get statistics |
| `getDetailedReport` | — | Admin | Full analytics report |
| `getUsers` | — | Admin | All user accounts |
| `updateUserRole` | `userId`, `role` | Admin | Change user role |
| `toggleUserStatus` | `userId` | Admin | Enable/disable user |
| `sendAnnouncement` | `subject`, `message`, `target` | Admin | Broadcast email |
| `getNotifications` | — | Admin | Email log |
| `getSettings` | — | Admin | All settings |
| `saveSettings` | `settings` (object) | Admin | Update settings |

---

## 🎨 Customization Guide

### Changing the Conference Name & Branding

1. Log in as Admin → Settings
2. Update **Conference Name**, **Short Name**, **Dates**, **Venue**
3. Changes appear immediately on all pages

### Adding Custom Tracks

In Settings → Tracks field, add comma-separated track names:
```
Track 1: AI & Machine Learning, Track 2: Data Science, Track 3: Cybersecurity
```
Click "Preview Tracks" to verify before saving.

### Modifying Registration Fees

Settings → Registration Fees section → update the 6 fee inputs.

### Changing the Color Theme

In `styles.html`, modify the CSS variables in `:root`:

```css
:root {
  --navy:       #1e2d4a;  /* Primary dark color */
  --navy-light: #2d4a7a;  /* Lighter navy */
  --gold:       #c8a94a;  /* Accent gold */
  --success:    #10b981;  /* Green */
  --danger:     #ef4444;  /* Red */
  --warning:    #f59e0b;  /* Amber */
  /* ... */
}
```

### Adding a New Review Criterion

1. In `Code.gs`, find `handleSubmitReview` and add the new criterion to the scores object
2. In `app.html`, find `loadReviewForm` and add a new slider input
3. Update `fmtReview` to include the new field
4. Add the criterion to the review detail display in `loadReviewDetail`

### Adding a New Paper Status

1. In `Code.gs`, update `handleUpdatePaperStatus` to allow the new status
2. Update `fmtPaper` if needed
3. In `app.html`, add the status to:
   - `openStatusModal` dropdown options
   - `loadMyPapers` status badge colors
   - `loadReports` status breakdown

### Customizing the Certificate PDF

In `Code.gs`, find `generateReviewerCertificate`. The certificate is built as an HTML string and converted to PDF. You can:

- Change fonts, colors, and layout
- Add institution logo (as Base64 embedded image)
- Add signature image
- Modify the certificate text

```javascript
function generateReviewerCertificate(reviewerName, reviewerEmail, paperTitle, paperId, reviewId, conf) {
  // Edit the htmlParts array to customize the certificate
  const htmlParts = [
    '<!DOCTYPE html><html><body>',
    '  <div class="cert-container">',
    '  <!-- Your custom HTML here -->',
    '</body></html>'
  ];
  // ...
}
```

---

## 🔧 Troubleshooting

### "Session not found" / Forced logout

**Cause:** Browser `sessionStorage` was cleared, or the session token expired.
**Fix:** Log in again. Token is regenerated on each login.

### Schedule times showing `--:--`

**Cause:** GAS auto-converted `HH:MM` strings to Date objects when stored.
**Fix:** From the Google Spreadsheet menu:
```
🚀 Conference App → 🕐 Fix Schedule Times
```

### "Only MS Word files (.doc, .docx) are accepted" when uploading payment slip

**Cause:** Running an old deployment that doesn't have the updated upload handler.
**Fix:** Create a **New Deployment** in Apps Script (not "edit existing").

### Upload failed / File not appearing in Drive

**Cause:** Drive API quota exceeded, or the upload folder wasn't created.
**Fix:**
1. Run `menuSetupFolder` from the Spreadsheet menu
2. Check Apps Script execution logs for error details

### Emails not sending

**Cause:** Gmail daily sending limit reached, or MailApp permissions not granted.
**Fix:**
- Check quota: GAS free tier allows ~100 emails/day
- Re-authorize the script by running any function manually

### "Initialize System" didn't create all sheets

**Cause:** Script ran partially before timeout.
**Fix:** Run `menuInitSystem` again — `mkSheet` is idempotent (won't duplicate existing sheets).

### Data not showing after registration/submission

**Cause:** Using cached API response from old deployment.
**Fix:** Hard-refresh the web app page and create a new deployment.

### Certificate PDF generation fails

**Cause:** Google Apps Script execution timeout (6 min limit) or Drive quota.
**Fix:** Use `resendCertificate` API from the reviewer's review detail page to retry.

---

## 🏃 Quick Start Checklist

```
☐ 1. Copy all 4 files to Google Apps Script
☐ 2. Enable Drive API in Services
☐ 3. Run menuInitSystem (creates Sheets + Drive folder)
☐ 4. Run menuCreateAdmin (creates admin account)
☐ 5. Deploy as Web App (Execute as: Me, Access: Anyone)
☐ 6. Open the Web App URL and log in as admin
☐ 7. Go to Settings and update conference details
☐ 8. Add conference tracks in Settings
☐ 9. Set registration fees in Settings
☐ 10. Create the conference schedule
☐ 11. Share the Web App URL with participants
```

---

## 🤝 Contributing

Contributions are welcome! Here are some areas for improvement:

### Planned Features
- [ ] Export reports as PDF
- [ ] Paper assignment algorithm (auto-assign reviewers by track)
- [ ] Multi-language support (full i18n)
- [ ] Plagiarism check integration
- [ ] Proceedings compilation (generate PDF program booklet)
- [ ] QR code check-in for on-site attendance
- [ ] Mobile app (PWA wrapper)

### Development Guidelines

1. **Fork** this repository
2. **Create** a feature branch: `git checkout -b feature/amazing-feature`
3. **Test** thoroughly in a separate GAS project
4. **Document** any new API endpoints or sheet columns
5. **Submit** a Pull Request with description of changes

### Code Style

- Functions in `Code.gs`: Use camelCase, add `handle` prefix for API handlers
- Functions in `app.html`: Prefix page loaders with `load`, form handlers with `do`
- CSS in `styles.html`: Use BEM-like naming, keep variables in `:root`
- No external JavaScript libraries required

---

## 📄 License

This project is licensed under the **MIT License** — see below for details.

```
MIT License

Copyright (c) 2026 TISE-2026 Conference System

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 🙏 Acknowledgements

- Built for the **7th National Conference on Thai Information Science Education 2026 (TISE-2026)**
- Hosted by the **Department of Information Science, Faculty of Humanities and Social Sciences, Khon Kaen University**
- Developed using **Google Apps Script**, **Google Sheets**, and **Google Drive**

---

<div align="center">

**[⬆ Back to Top](#-academic-conference-management-system)**

Made with ❤️ for the Thai academic community

</div>
