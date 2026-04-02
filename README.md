# 🚨 AI-Powered Log Error Analyser — n8n DevOps Portfolio Project 5

An automated server monitoring workflow built with n8n that SSHs into
a live server every 15 minutes, extracts critical log errors, analyses
them using Groq AI (LLaMA 3.3), and delivers intelligent alerts directly to Slack.

---

## 📸 Workflow Overview

![Workflow Canvas](screenshots/workflow-canvas.png)

---

## 🧰 Tech Stack

| Tool | Purpose |
|---|---|
| **n8n** | Workflow automation engine |
| **SSH** | Remote server log fetching |
| **JavaScript (Code Node)** | Log parsing and error extraction |
| **Groq AI (LLaMA 3.3-70b)** | AI-powered error analysis |
| **Slack** | Real-time alert delivery |

---

## ⚙️ How It Works

```
Schedule Trigger (every 15 min)
        ↓
SSH Node — fetches last 15 min of logs from server
        ↓
Code Node — extracts & structures error/fatal/critical lines
        ↓
IF Node — checks if errors exist
        ↓ (true branch)
HTTP Request — sends errors to Groq AI for analysis
        ↓
Slack Node — delivers AI analysis as a formatted alert
```

---

## 🔍 What Gets Monitored

The workflow runs this command on the remote server:

```bash
sudo journalctl --since "15 minutes ago" | grep -iE "error|fatal|critical" | tail -50
```

It captures:
- `ERROR` level log entries
- `FATAL` level log entries  
- `CRITICAL` level log entries

---

## 🤖 AI Analysis Output

Groq AI (LLaMA 3.3-70b-versatile) analyses each batch of errors and returns:

1. **Summary** — what went wrong in plain English
2. **Severity Level** — Low / Medium / High / Critical
3. **Recommended Fix** — actionable steps to resolve the issue

---

## 📲 Slack Alert Format

```
🚨 Server Error Alert 🚨
Server: 16.171.91.105
Time: 2026-04-02T12:55:01.908Z
Errors Found: 3

🤖 AI Analysis:
**Summary of what went wrong:**
The amazon-ssm-agent (AWS Systems Manager Agent) is 
experiencing difficulties connecting to Systems Manager...
```

---

## 🛠️ Setup Instructions

### Prerequisites
- n8n instance (self-hosted or cloud)
- SSH access to a Linux server
- Groq API key ([console.groq.com](https://console.groq.com))
- Slack workspace with a channel for alerts

### Steps

**1. Clone this repo**
```bash
git clone https://github.com/YOUR_USERNAME/n8n-ai-log-error-analyser.git
```

**2. Import the workflow**
- Open your n8n instance
- Click **"Import from file"**
- Select `workflow.json`

**3. Configure Credentials (Best Practice)**
- Go to **n8n Settings → Credentials**
- Add **SSH Password** credential with your server details
- Add **Header Auth** credential for Groq API:
  - Header Name: `Authorization`
  - Header Value: `Bearer YOUR_GROQ_API_KEY`
- Add **Slack** credential with your Bot Token

**4. Update the workflow**
- Set your server IP in the SSH node
- Set your Slack channel in the Slack node

**5. Activate the workflow**
- Toggle the workflow to **Active**
- It will now run automatically every 15 minutes

---

## 📁 Project Structure

```
n8n-ai-log-error-analyser/
├── workflow.json          # n8n workflow export
├── screenshots/
│   ├── workflow-canvas.png
│   ├── ssh-output.png
│   ├── code-node-output.png
│   ├── groq-ai-response.png
│   ├── slack-message.png
│   └── workflow-active.png
└── README.md
```

---

## 🔐 Security Best Practices

- ✅ All credentials stored in **n8n Credential Manager** (never hardcoded)
- ✅ Workflow JSON contains **no API keys or passwords**
- ✅ SSH uses password authentication stored in n8n credentials
- ✅ Safe to share and upload to GitHub

---

## 💡 Potential Improvements

- Add Gmail node for email reports on Critical errors only
- Store error history in a database (PostgreSQL/Airtable)
- Add a dashboard to visualise error trends over time
- Escalate to PagerDuty for on-call alerts
- Monitor multiple servers in parallel

---

## 👨‍💻 Author

**The Present DevOps Engineer**  
🌐 [thepresentdevopsengineer.cloud](https://thepresentdevopsengineer.cloud)  

---

## 📜 License

MIT License — feel free to use and adapt for your own projects.
