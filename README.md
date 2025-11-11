# 💸 Smart Expenses

A Python automation script that reads expense-related emails from your inbox and automatically records them into a **Google Sheets** spreadsheet.  
It can be executed manually or automatically every day at **20:00 (Madrid time)** using **GitHub Actions**.

---

## 🧠 Overview

This project connects to your email account via **IMAP**, retrieves messages that match specific filters (sender, subject, and date), parses and classifies them into spending categories, and then appends the results into a Google Sheets document using a service account.

The script runs locally or automatically in the cloud, allowing you to maintain a live and up-to-date record of your expenses.

---

## ⚙️ Technologies Used

- **Python 3.13**
- **Google Sheets API**
- **IMAP / Gmail**
- **python-dotenv**
- **GitHub Actions** for automation
- **Logging** for structured runtime reporting

---

## 🧩 Project Structure

gastos_inteligentes/
├── app.py # Main entry point
├── utils/
│ ├── retrieve_emails.py # Fetch and parse emails via IMAP
│ ├── parser.py # Extract expense data from email content
│ ├── classifier.py # Categorize expenses using predefined rules
│ ├── sheets.py # Append parsed data into Google Sheets
│ └── connection.py # Handles IMAP connection and error management
├── config/
│ ├── settings.py # Loads environment variables and constants
│ ├── constants.py # Shared constant values
│ └── categories.json # Category definitions for expense classification
├── secrets/
│ └── key.json # Google service account key (ignored by Git)
├── .github/
│ └── workflows/
│ └── daily.yml # GitHub Actions workflow (runs every day)
└── requirements.txt

---

## 🚀 Local Setup

### 1️⃣ Clone the repository

```bash
git clone https://github.com/your-username/gastos_inteligentes.git
cd gastos_inteligentes```bash

### 2️⃣ Create a virtual environment

python -m venv env
source env/bin/activate     # macOS / Linux
env\Scripts\activate        # Windows

### 3️⃣ Install dependencies

pip install -r requirements.txt

### 4️⃣ Set up environment variables

Create a file named .env.local in the project root:

IMAP_SERVER=imap.gmail.com
EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_app_password
SEARCH_FROM="no-reply@somewhere.com"
SEARCH_SUBJECT="Your Expense Receipt"
SPREADSHEET_ID=your_google_sheet_id
SERVICE_ACCOUNT_KEY_PATH=secrets/key.json

💡 For Gmail, you must enable App Passwords to connect via IMAP securely.

### 5️⃣ Add your Google service account key

1. Create a service account in your Google Cloud Console.
2. Enable the Google Sheets API.
3. Download the JSON key file and save it as secrets/key.json.
4. Share your Google Sheet with the service account email (xxxx@xxxx.iam.gserviceaccount.com).

### 🧪 Run Locally

python app.py

This will:

1. Connect to your Gmail inbox
2. Retrieve yesterday’s expense emails
3. Classify and parse them
4. Append the data into your Google Sheet

### 🤖 Automate with GitHub Actions

The included workflow .github/workflows/daily.yml automatically runs the script every day at 20:00 (Madrid time).

✅ How it works

1. GitHub Actions checks out your repository.
2. It installs Python and dependencies.
3. It securely reconstructs .env.local and secrets/key.json from GitHub Secrets.
4. It runs app.py.
5. It sends you an email notification about success or failure.

### 🔐 Required GitHub Secrets

| Secret name           | Description                             |
| --------------------- | --------------------------------------- |
| `IMAP_SERVER`         | IMAP server (e.g., `imap.gmail.com`)    |
| `EMAIL_USER`          | Your Gmail address                      |
| `EMAIL_PASS`          | Your Gmail App Password                 |
| `SEARCH_FROM`         | Sender filter for expenses              |
| `SEARCH_SUBJECT`      | Subject filter for expenses             |
| `SPREADSHEET_ID`      | Your Google Sheet ID                    |
| `SERVICE_ACCOUNT_KEY` | JSON string of your service account key |

### 🧹 Security Notes

The .env.local and secrets/key.json files are generated dynamically during the workflow and are never committed to Git.
All sensitive data is stored in GitHub Secrets.
The workflow automatically removes temporary credentials after each run.
