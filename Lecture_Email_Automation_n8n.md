# Lecture: Smart Email Automation for Digital Marketing
## Auto-Detect Orders vs Queries & Route Them Smartly

---

# PART 1: What Are We Building Today?

## The Problem (Every Business Faces This)

Imagine you run an online store. Every day you get 100+ emails:

- "I want to buy 50 red t-shirts" --> This is an ORDER
- "What sizes do you have?" --> This is a QUERY (question)
- "Please send me price list" --> This is a QUERY
- "I want to order 10 boxes of chocolates, deliver to Lahore" --> This is an ORDER

Right now, someone sits and reads EVERY email, then forwards:
- Orders --> order department
- Questions --> customer support

**We will make a ROBOT that does this automatically. For FREE.**

## What Will Our Robot Do?

```
Customer sends email
        |
        v
Robot reads the email (using AI)
        |
        v
Is it an ORDER or a QUERY?
       / \
      /   \
ORDER      QUERY
  |          |
  v          v
Forward    Forward
to Order   to Customer
Department Support
  |          |
  v          v
Reply:     Reply:
"Order     "We received
received,  your question,
we will    team will
process    reply soon"
soon"
```

## Tools We Will Use (All FREE)

| Tool | What It Does | Cost |
|------|-------------|------|
| **n8n** | The robot brain - connects everything | FREE (on your laptop) |
| **Ollama** | AI that reads emails and decides | FREE (on your laptop) |
| **Gmail** | Email service | FREE |

**No credit card needed. No subscription. Everything runs on YOUR laptop.**

---

# PART 2: Installing Our Tools

## Step 1: Install Ollama (Our AI Brain)

Ollama is like ChatGPT but it runs on YOUR computer. No internet needed after install.

### How to Install:

1. Open your browser (Chrome/Edge)
2. Go to: **ollama.com**
3. Click the big **"Download"** button
4. Choose **Windows**
5. Double-click the downloaded file
6. Click **Next, Next, Next, Install** (like installing any app)
7. Done!

### Download the AI Model:

After installing Ollama:

1. Open **Terminal** (Search "cmd" in Windows Start Menu)
2. Type this and press Enter:

```
ollama pull llama3.2
```

3. Wait for it to download (this may take 10-15 minutes depending on internet)
4. That's it! Your AI is ready.

### Test if it works:

Type this in the same terminal:

```
ollama run llama3.2 "Is this an order or query: I want to buy 5 shirts"
```

If it replies something about it being an order - it works!

---

## Step 2: Install n8n (Our Robot Builder)

n8n is like building blocks for automation. You drag and drop, no coding.

### Method A: Using npm (Recommended)

First install Node.js:
1. Go to: **nodejs.org**
2. Download the **LTS** version (the green button)
3. Install it (Next, Next, Next, Install)

Then install n8n:
1. Open **Terminal** (cmd)
2. Type:

```
npm install -g n8n
```

3. Wait for it to finish

### How to Start n8n:

Every time you want to use n8n, open terminal and type:

```
n8n start
```

Then open your browser and go to:

```
http://localhost:5678
```

You will see the n8n screen! It will ask you to create an account (this is LOCAL, just for your laptop).

**Create a simple username and password you'll remember.**

---

# PART 3: Understanding n8n (The Robot Builder)

## The n8n Screen - What You See

When you open n8n, you see a blank canvas (like a whiteboard).

On this whiteboard, we will place "blocks" (called **Nodes**).
Each block does ONE job.

Think of it like a **factory assembly line**:

```
[Block 1]  -->  [Block 2]  -->  [Block 3]  -->  [Block 4]
 Get Email      Read it        Decide          Send Reply
                with AI        Order/Query
```

## Important Words (Only 4 to Remember)

| Word | Meaning | Example |
|------|---------|---------|
| **Node** | One block that does one job | "Get Email" is one node |
| **Workflow** | All blocks connected together | Our whole robot is one workflow |
| **Trigger** | The block that STARTS everything | "When new email arrives" |
| **Connection** | The line between two blocks | The arrow --> between nodes |

---

# PART 4: Building the Workflow (Step by Step)

## Step 1: Create New Workflow

1. Click **"Add workflow"** button (top right area) or the **"+"** icon
2. You see an empty canvas
3. At the top, click on "My Workflow" to rename it
4. Type: **"Email Sorter"**
5. Press Enter

## Step 2: Add the Email Trigger (Block 1)

This block checks your email for new messages.

1. Click the **"+"** button on the canvas
2. Search for **"Gmail Trigger"** (if using Gmail)
   - If using other email: search **"IMAP Email Trigger"** (works with ANY email)
3. Click on it to add it

### Setting up Gmail Trigger:

1. Click on the Gmail Trigger node (block)
2. You'll see settings on the right side
3. Click **"Credential"** > **"Create New"**
4. It will ask you to sign in to your Gmail
5. Sign in and allow access
6. Set **"Poll Times"** to: Every 1 Minute
   (This means the robot checks email every 1 minute)
7. Set **"Event"** to: **"Message Received"**

### If Using IMAP (for any email like Yahoo, Outlook, business email):

1. Click on the IMAP node
2. Click **"Credential"** > **"Create New"**
3. Fill in:
   - **Host**: imap.gmail.com (for Gmail) or your email provider's IMAP server
   - **Port**: 993
   - **User**: your full email address
   - **Password**: your email password (or App Password for Gmail)
   - **SSL**: Turn ON
4. Set check interval to 1 minute

**For Gmail users:** You need an "App Password" not your regular password.
- Go to myaccount.google.com > Security > 2-Step Verification > App Passwords
- Create one for "Mail" and use that password

## Step 3: Add the AI Brain (Block 2)

This block sends the email to our local AI (Ollama) to decide if it's an order or query.

1. Hover over the Gmail Trigger block, you'll see a small **"+"** on the right
2. Click that **"+"**
3. Search for **"Ollama"** or **"HTTP Request"**

### Option A: If you see "Ollama" node (newer n8n versions):

1. Click **Ollama**
2. Set it up:
   - **Model**: llama3.2
   - **URL**: http://localhost:11434 (this is where Ollama runs on your laptop)

### Option B: Using HTTP Request node (works always):

1. Search and add **"HTTP Request"** node
2. Settings:
   - **Method**: POST
   - **URL**: `http://localhost:11434/api/generate`
   - **Body Type**: JSON
   - **JSON Body**:

```json
{
  "model": "llama3.2",
  "prompt": "Read this email and reply with ONLY one word - either ORDER or QUERY. Nothing else. Just one word.\n\nEmail: {{ $json.text }}",
  "stream": false
}
```

**What's happening here:**
- We're sending the email text to our AI
- We're telling AI: "Just tell me one word - ORDER or QUERY"
- The AI reads the email and decides

3. Click the small **"+"** to the right of this node

## Step 4: Clean the AI Response (Block 3)

Sometimes AI adds extra spaces or words. Let's clean it.

1. Add a **"Code"** node (search "Code")
2. Don't worry! Just paste this code exactly:

```javascript
// Get AI response and clean it
let response = "";

// If using HTTP Request to Ollama
if ($input.first().json.response) {
  response = $input.first().json.response;
} else {
  response = JSON.stringify($input.first().json);
}

// Clean it - just get ORDER or QUERY
response = response.toUpperCase().trim();

let result = "QUERY"; // default to query (safer)

if (response.includes("ORDER")) {
  result = "ORDER";
}

return [{ json: {
  decision: result,
  originalEmail: $('Gmail Trigger').first().json.text,
  senderEmail: $('Gmail Trigger').first().json.from.value[0].address,
  senderName: $('Gmail Trigger').first().json.from.value[0].name || "Customer",
  subject: $('Gmail Trigger').first().json.subject
}}];
```

**What this does (in simple words):**
- Takes the AI's answer
- Checks if it contains the word "ORDER"
- If yes -> marks as ORDER
- If no -> marks as QUERY
- Also saves the customer's email, name, and subject for later use

## Step 5: Add the Decision Splitter (Block 4)

This block sends emails in two different directions.

1. Add an **"IF"** node (search "IF")
2. Settings:
   - **Value 1**: Click the small gear icon, then select:
     `{{ $json.decision }}`
   - **Operation**: "equals" (is equal to)
   - **Value 2**: Type: `ORDER`

Now you'll see the IF block has TWO outputs:
- **true** (green) = It IS an order
- **false** (red) = It is NOT an order (so it's a query)

## Step 6: Handle ORDERS (Block 5a - Green Path)

### 6a: Forward to Order Department

1. From the **green (true)** output of IF node, click **"+"**
2. Add **"Gmail"** node (or **"Send Email"** node)
3. Settings:
   - **To**: `orders@yourcompany.com` (your order department email)
   - **Subject**: `New Order from {{ $json.senderName }} - {{ $json.subject }}`
   - **Message**:

```
New order received!

From: {{ $json.senderName }} ({{ $json.senderEmail }})
Subject: {{ $json.subject }}

Original Message:
{{ $json.originalEmail }}

---
This email was automatically sorted by AI Email Robot.
```

### 6b: Reply to Customer (Order Confirmation)

1. From the SAME green path (you can add another node after the forward)
2. Add another **"Gmail"** node
3. Settings:
   - **To**: `{{ $json.senderEmail }}`
   - **Subject**: `Re: {{ $json.subject }}`
   - **Message**:

```
Dear {{ $json.senderName }},

Thank you for your order!

We have received your email and forwarded it to our Order Processing team.
You will receive order confirmation details within 24 hours.

If you have any questions, feel free to reply to this email.

Best regards,
[Your Company Name]
Customer Service
```

## Step 7: Handle QUERIES (Block 5b - Red Path)

### 7a: Forward to Customer Support

1. From the **red (false)** output of IF node, click **"+"**
2. Add **"Gmail"** node
3. Settings:
   - **To**: `support@yourcompany.com` (your customer support email)
   - **Subject**: `Customer Query from {{ $json.senderName }} - {{ $json.subject }}`
   - **Message**:

```
New customer query received!

From: {{ $json.senderName }} ({{ $json.senderEmail }})
Subject: {{ $json.subject }}

Original Message:
{{ $json.originalEmail }}

---
This email was automatically sorted by AI Email Robot.
```

### 7b: Reply to Customer (Query Acknowledgment)

1. Add another **"Gmail"** node after the forward
2. Settings:
   - **To**: `{{ $json.senderEmail }}`
   - **Subject**: `Re: {{ $json.subject }}`
   - **Message**:

```
Dear {{ $json.senderName }},

Thank you for reaching out to us!

We have received your inquiry and our Customer Support team will
get back to you within 24 hours.

If this is urgent, please call us at [Your Phone Number].

Best regards,
[Your Company Name]
Customer Service
```

---

# PART 5: Your Complete Workflow Should Look Like This

```
                                          [Forward to Orders Dept]
                                         /         |
[Gmail Trigger] -> [AI/HTTP] -> [Code] -> [IF] --(ORDER)-- [Reply: Order Received]
                                           |
                                        (QUERY)
                                           |
                                    [Forward to Support]
                                           |
                                    [Reply: Query Received]
```

Total Blocks: 7 nodes
Time to build: One sitting

---

# PART 6: Testing Your Workflow

## How to Test:

1. Click the **"Test Workflow"** button (at the bottom of the screen)
2. Send a test email to YOUR email from another email:

### Test Email 1 (Should be detected as ORDER):
```
Subject: Order for products
Body: Hi, I want to order 20 pieces of blue widgets.
Please deliver to 123 Main Street. Payment will be COD.
```

### Test Email 2 (Should be detected as QUERY):
```
Subject: Question about products
Body: Hi, can you tell me what colors are available
for your widgets? Also, do you deliver to Karachi?
```

3. Wait for the robot to pick up the email (up to 1 minute)
4. Check if:
   - Order email was forwarded to order department email
   - Query email was forwarded to support email
   - Customer got the right auto-reply

## If Something Doesn't Work:

- Click on any block that shows RED
- Read the error message
- Most common fixes:
  - **"Cannot connect"**: Make sure Ollama is running (type `ollama serve` in terminal)
  - **"Authentication failed"**: Check your email password/app password
  - **"Invalid JSON"**: Check the AI prompt, copy-paste it again carefully

---

# PART 7: Making It Run Automatically

Right now, the workflow only runs when you click "Test".
To make it run ALL THE TIME:

1. Click the toggle switch at the top right (it says **"Inactive"**)
2. Switch it to **"Active"**
3. Now it runs automatically!

**Remember:**
- n8n must be running on your laptop (the terminal window must stay open)
- Ollama must be running (keep that terminal open too)
- Your laptop must be ON

### Quick Start Checklist (Every Day):
1. Open Terminal 1: type `ollama serve` and press Enter
2. Open Terminal 2: type `n8n start` and press Enter
3. Open browser, go to localhost:5678
4. Make sure your workflow shows "Active"
5. Done! Robot is working.

---

# PART 8: When You're Ready to Upgrade (Future)

When your business grows and you have budget:

## Option 1: n8n Cloud (Paid - starts ~$20/month)
- Go to **n8n.io** and sign up
- **Everything looks THE SAME** - same blocks, same design
- Benefit: Runs 24/7 without keeping laptop on
- Your workflow? Just **Export** from local n8n and **Import** to cloud n8n
- That's it - no rebuilding!

## Option 2: Better AI (Using API Keys)
- Instead of Ollama (local AI), use **OpenAI GPT** or **Google Gemini** or **Claude**
- In n8n, just swap the HTTP Request block with the official **OpenAI node**
- Everything else stays the same
- Benefit: More accurate, faster, handles more languages

## Option 3: Self-Host on a Server
- Rent a small server (DigitalOcean, ~$5/month)
- Install n8n and Ollama there
- Runs 24/7 without your laptop
- Same setup, just on a server instead of laptop

**The key point: What you learn today works EVERYWHERE.
Local or cloud, free or paid - the workflow is the same.**

---

# PART 9: More Ideas for Digital Marketing Automation

Now that you understand the concept, here are more things you can build:

| Automation Idea | What It Does |
|----------------|--------------|
| **Social Media Auto-Reply** | AI reads Instagram/Facebook DMs, auto-replies |
| **Lead Scoring** | AI reads form submissions, scores them hot/warm/cold |
| **Review Responder** | AI reads Google reviews, drafts appropriate responses |
| **Content Calendar** | AI generates social media post ideas weekly |
| **Invoice Generator** | When order email arrives, auto-create invoice |
| **CRM Update** | Auto-add new leads to Google Sheets or CRM |
| **WhatsApp Bot** | Connect WhatsApp Business, auto-reply to customers |
| **Competitor Monitor** | Check competitor websites daily, alert on changes |

All of these can be built in n8n with the same drag-and-drop approach!

---

# PART 10: Quick Reference Card

## Starting Everything (Do This Daily)

```
Step 1: Open Terminal -> type: ollama serve
Step 2: Open New Terminal -> type: n8n start
Step 3: Open Browser -> go to: localhost:5678
Step 4: Check workflow is Active
```

## Stopping Everything

```
Step 1: In n8n browser, set workflow to Inactive
Step 2: In terminal windows, press Ctrl+C to stop
Step 3: Close terminals
```

## Useful Links

- n8n website: https://n8n.io
- Ollama website: https://ollama.com
- Node.js download: https://nodejs.org
- n8n community (help): https://community.n8n.io

## Troubleshooting Quick Fixes

| Problem | Solution |
|---------|----------|
| n8n won't start | Restart terminal, type `n8n start` again |
| Ollama not responding | Open new terminal, type `ollama serve` |
| Email not connecting | Check App Password, not regular password |
| AI giving wrong answers | Edit the prompt in HTTP Request node, be more specific |
| Workflow not running | Check if toggle is set to "Active" |
| Browser shows error | Clear browser cache, try localhost:5678 again |

---

# Summary

## What You Learned Today:

1. **Automation** = Making a robot do repetitive work for you
2. **n8n** = Free tool to build automation (drag and drop, no coding)
3. **Ollama** = Free AI that runs on your laptop (like ChatGPT but free and private)
4. **Workflow** = Connecting blocks together to create an automated process
5. How to build a **real email sorting robot** that:
   - Reads incoming emails
   - Uses AI to decide: Order or Query?
   - Forwards to the right department
   - Auto-replies to the customer

## Key Takeaway:

> "You don't need to be a programmer to automate your business.
> You just need to think: What do I do repeatedly? Then let a robot do it."

---

*Lecture prepared for Digital Marketing Students*
*All tools used: FREE and LOCAL (no internet subscription needed)*
*Built with: n8n + Ollama + Gmail*
