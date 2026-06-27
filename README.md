#  AI Customer Support Agent — n8n Automation

> Fully automated customer support pipeline with AI memory, smart routing, and personalized responses — built on n8n.

[![n8n](https://img.shields.io/badge/Built%20with-n8n-EA4B71?style=for-the-badge&logo=n8n)](https://n8n.io)
[![Groq](https://img.shields.io/badge/AI-Groq%20LLaMA%203.3-F55036?style=for-the-badge)](https://groq.com)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

---

##  Business Problem

Customer support is one of the most expensive and inefficient operations in any business:

- 📊 **Manual cost per ticket: $8–15** vs **$0.50 with AI** — a 16–30x cost reduction
- ⏱️ **Average response time without AI: 6+ hours** → drops to **under 4 minutes** with automation (Freshworks, 2025)
- 👥 **70% of customer inquiries** can be handled automatically without a human agent
- 💸 **AI automation saves businesses $79 billion annually** in customer service costs (2025)
- 📉 A company handling **10,000 tickets/month** saves **$80,000–140,000/year** by switching to automation
- 🔁 Support agents spend **60–80% of their time** on repetitive, low-value tickets
- 📈 Companies using AI support report **$3.50 return for every $1 invested**, with top performers reaching **8x ROI**
- ⚡ AI-assisted agents resolve issues **47% faster** with **25% higher first-contact resolution rates**

**The result:** Slow responses, burnt-out agents, inconsistent quality — and customers who leave.

---

##  Solution

A fully automated support pipeline that:

- Receives customer messages via Webhook
- Looks up customer history from Google Sheets (memory)
- Uses Groq LLM (Llama 3.3 70B) to classify, analyze sentiment, and respond
- Routes tickets based on priority (Urgent / Human Review / Auto-resolve)
- Logs every ticket automatically with timestamp, category, and AI response
- Sends personalized email responses in seconds

---

##  Workflow 

Customer (HTTP POST)

↓

Webhook

↓

Lookup History (Google Sheets)

↓

Build Context (Code Node)

↓

Groq AI — LLaMA 3.3 70B

↓

Parse & Enrich (Code Node)

↓

Switch Router

├── Urgent       → Sheets Log + Email

├── Human Review → Sheets Log + Email

└── Fallback     → Sheets Log + Email

↓

Error Handler (Email Alert + Retry x3)
---

##  How It Works

1. Customer sends a message via HTTP POST to the Webhook
2. System fetches their **full ticket history** from Google Sheets by email
3. Code node builds customer context — last 3 messages + most repeated issue category
4. Groq AI analyzes message + history and returns structured JSON with category, priority, sentiment, and response
5. Switch node routes to the correct path based on priority
6. Ticket is logged to Google Sheets with all metadata
7. Customer receives a **personalized AI-generated response** via email

---

##  Tech Stack

| Tool | Purpose |
|------|---------|
| n8n | Workflow automation engine |
| Groq — LLaMA 3.3 70B | AI classification, sentiment & response |
| Google Sheets | Data store, logging & customer memory |
| Gmail SMTP | Email delivery |
| Webhook | Entry point for incoming requests |
| JavaScript (Code Node) | Data parsing, context building, error handling |

---

##  Business Value

| Feature | Impact |
|---------|--------|
| ⏱️ Response time | Hours → Seconds |
| 🧠 AI Memory | Remembers customer history across sessions |
| 🎯 Smart Routing | 3 paths based on priority & urgency |
| 🔁 Retry Logic | 3 automatic retries on API failure |
| 🚨 Error Handling | Instant email alert on any failure |
| 📋 Auto Logging | Every ticket logged with full metadata |

---

##  Known Limitations

> Transparency is part of professionalism.

- **Duplicate nodes:** 3 separate Sheets + Email nodes per path — can be merged into one shared path
- **No input validation:** Empty email or message fields are not validated before hitting the AI
- **JSON parsing risk:** If the LLM returns malformed JSON, the workflow errors — structured output would be more robust
- **No Webhook authentication:** Anyone with the URL can send requests — rate limiting or token auth should be added for production
- **No Slack/Discord alerts:** High priority tickets only trigger email — real-time team notifications would improve response speed

---

##  Future Improvements

- [ ] Merge duplicate nodes into a single shared path
- [ ] Add input validation before AI call
- [ ] Add Slack notification for urgent tickets
- [ ] WhatsApp integration via Twilio
- [ ] Dashboard for ticket analytics
- [ ] Webhook authentication (API key or HMAC)
- [ ] Fine-tuned model on company-specific support data

---

##  Screenshots

### Workflow Overview
![Workflow](Screenshot%202026-06-27%20153802.png)

### Successful Execution
![Execution](Screenshot%202026-06-27%20161355.png)

### Switch Node
![Switch](Screenshot%202026-06-27%20153934.png)

### AI Email Response
![Email](Screenshot%202026-06-27%20161712.png)

---

##  Setup

### 1. Import the workflow
Download `ai-customer-support.json` and import it into your n8n instance via **Settings → Import**.

### 2. Configure credentials
- **Groq API** — get a free key at [console.groq.com](https://console.groq.com)
- **Google Sheets OAuth2** — connect your Google account
- **Gmail SMTP** — use an App Password from [myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords)

### 3. Create your Google Sheet
Create a sheet named `AI Customer Support Log` with these columns:
Timestamp | Name | Email | Message | Category | Priority | Sentiment | Needs Human | AI Response | Internal Note
### 4. Activate and test
```bash
curl -X POST https://your-n8n-url/webhook/customer-support \
  -H "Content-Type: application/json" \
  -d '{"name":"Ahmed","email":"ahmed@test.com","message":"My order is late!"}'
```

---

##  Author

Built by **Khaled Fouad🤙🏽** 

[![GitHub](https://img.shields.io/badge/GitHub-Khaled--fouad0-181717?style=flat-square&logo=github)](https://github.com/Khaled-fouad0)

---

##  License

MIT — free to use, modify, and distribute.
