# Email Automation Workshop (n8n + Ollama)

**Auto-sort customer emails into Orders vs Queries using AI — 100% FREE, runs locally on your laptop.**

Built for digital marketing students with zero technical background.

---

## What This Workshop Teaches

A customer sends an email. Our robot:

```
Customer Email
      |
      v
  AI reads it
      |
  ORDER or QUERY?
    /        \
   v          v
Order Dept   Support Dept
   |          |
   v          v
Auto-reply:  Auto-reply:
"Order       "Query
received!"   received!"
```

- No coding required (drag & drop)
- Runs on YOUR laptop (no cloud, no subscription)
- Uses free AI (Ollama) instead of paid ChatGPT

---

## Tools Used (All Free)

| Tool | Purpose | Download |
|------|---------|----------|
| **n8n** | Automation builder (drag & drop blocks) | Install via `npm install -g n8n` |
| **Ollama** | Local AI brain (reads & classifies emails) | [ollama.com](https://ollama.com) |
| **Gmail** | Email service | Your existing account |
| **Node.js** | Required to run n8n | [nodejs.org](https://nodejs.org) |

---

## Workshop Files

| File | What It Is |
|------|-----------|
| `Email_Automation_Lecture.pptx` | 16-slide presentation (show BEFORE practical) |
| `Lecture_Email_Automation_n8n.md` | Detailed step-by-step practical guide |
| `create_ppt.py` | Python script that generates the PPTX (for reference) |

---

## Quick Setup (One Time)

```bash
# 1. Install Ollama
# Download from ollama.com and install

# 2. Download AI model
ollama pull llama3.2

# 3. Install Node.js
# Download LTS from nodejs.org and install

# 4. Install n8n
npm install -g n8n
```

## Daily Startup (3 Commands)

```bash
# Terminal 1: Start AI
ollama serve

# Terminal 2: Start n8n
n8n start

# Then open browser: http://localhost:5678
```

---

## Workshop Flow

1. **Show the PPT** (16 slides) — explains the concept visually
2. **Open the practical guide** — build the workflow step by step
3. **Test with real emails** — send test order & query emails
4. **Activate** — set workflow to run automatically

---

## Workflow Blocks (7 Nodes)

```
[Gmail Trigger] -> [HTTP Request to Ollama] -> [Code: Clean Response]
      -> [IF: Order?]
            |-- YES --> [Forward to Orders] -> [Reply: Order Received]
            |-- NO  --> [Forward to Support] -> [Reply: Query Received]
```

---

## Future Upgrades

| Level | What Changes | Cost |
|-------|-------------|------|
| **Today** | Ollama + n8n on laptop | Free |
| **Level 2** | Swap Ollama with ChatGPT/Claude API | API costs |
| **Level 3** | Move n8n to cloud (n8n.io) | ~$20/month |

Your workflow stays the SAME at every level. Learn once, upgrade anytime.

---

## Who Is This For

- Digital marketing students
- Small business owners
- Anyone who handles customer emails manually
- Tech knowledge required: almost zero

---

## License

Free to use for educational purposes.
