# Job Radar – Remote AI Automation Roles

![Workflow screenshot](images/Screenshot%202026-11-03%20135630.png)

An n8n workflow that scans remote job boards every morning, filters for AI / automation / GTM roles, scores each posting with an AI agent, and sends the good ones to Telegram with a ready-to-send opener.

## How it works

1. **Triggers**: daily at 9 AM (Asia/Kolkata) or manual run
2. **Sources**: Remotive API, RemoteOK API, Hacker News "Who is hiring" thread
3. **Normalize and Prefilter**: Code node unifies the three formats, keeps AI/automation/entry-level keywords, drops senior/sales/admin roles, takes the 6 newest per source
4. **Skip Seen Postings**: dedupes against previous runs so you never get the same job twice
5. **Score Posting**: n8n agent returns `{score, verdict, reason, opener}` per posting
6. **Good Fit?**: score >= 4 passes
7. **Send to Telegram**: formatted alert with score, location, pay, reason, opener, link

## Setup

1. In n8n: **Import from file** → `workflow.json`
2. **Score Posting**: pick your own n8n agent (replace `YOUR_AGENT_ID`). Its system prompt should return JSON: `{"score": 0-10, "verdict": "apply|skip", "reason": "...", "opener": "..."}`
3. **Send to Telegram**: add your Telegram bot credential and set your chat ID (replace `YOUR_TELEGRAM_CHAT_ID`)
4. Tweak the `KEEP` / `DROP` regexes in the *Normalize and Prefilter* node to match the roles you want
5. Activate

## Stack

n8n · Remotive API · RemoteOK API · HN Algolia API · Telegram Bot API
