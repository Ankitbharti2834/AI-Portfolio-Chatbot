# AI Portfolio Chatbot — Ankit Bharti

A fully functional AI-powered portfolio assistant built from scratch and deployed
on Vercel. Recruiters and hiring managers can ask any question about my background,
skills, projects, and availability — and get instant, detailed answers powered by
Claude AI.

Live at: https://ankitbharti-portfolio.vercel.app

---

## What It Does

- Answers recruiter questions about work experience, skills, projects, visa status,
  and availability in real time
- Responds to technical interview-style questions about Power BI, DAX, SQL, Python,
  Azure, and ETL pipelines using detailed answers grounded in real work experience
- Auto-opens 2 seconds after a visitor lands on the portfolio
- Fully draggable — the robot button can be moved anywhere on screen
- Works on desktop and mobile

---

## How It Works

The chatbot is built in three parts:

**1. Frontend (HTML/CSS/JavaScript)**
A custom chat window embedded directly in the portfolio site. No frameworks.
The UI includes a Gold Circuit Robot button with blinking eyes and a pulse
animation, a chat window with quick-chip buttons, typing indicators, and
auto-scroll. The system prompt (CB_SYSTEM) contains my full professional profile,
technical Q&A, and answer rules so the AI always responds in context.

**2. Vercel Serverless Proxy (api/chat.js)**
The browser cannot call the Anthropic API directly because that would expose
the API key. Instead every message goes to this serverless function which holds
the key securely in an environment variable, forwards the request to Anthropic,
and returns the response. The function runs on Vercel's edge infrastructure and
responds in under 200ms.

**3. Claude AI (Anthropic API)**
The brain behind the responses. Model used: claude-sonnet-4-6. The system prompt
includes my full background, 48+ interview Q&A answers, DAX code examples, Python
scripts, SQL patterns, and behavioral stories. Claude generates fresh, natural
responses to any question rather than doing keyword matching.

---

## Architecture
```
Visitor asks question
        |
        v
Chat window (index.html)
        |
  POST /api/chat
        |
        v
Vercel Serverless Function (api/chat.js)
        |
  Anthropic API call with system prompt + conversation history
        |
        v
Claude generates response
        |
        v
Response streamed back to chat window
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML, CSS, JavaScript |
| Serverless proxy | Node.js on Vercel |
| AI model | Claude Sonnet (Anthropic API) |
| Hosting | Vercel (free tier) |
| Version control | GitHub |

---

## Key Features

**Draggable Robot Button**
Built with mouse and touch event listeners. Distinguishes between a click
(opens chat) and a drag (moves button) using a 4-pixel movement threshold.
Works on mobile with touch events.

**Auto-open on Visit**
setTimeout fires cbToggle() after 2 seconds. Only fires once per session.

**Context-Aware Answers**
The CB_SYSTEM prompt contains over 2,000 words of structured profile data
including technical Q&A, code examples, and behavioral answers. Claude draws
from this to answer anything a recruiter or technical interviewer might ask.

**Conversation Memory**
The last 12 messages are passed with every API call so the AI remembers
what was said earlier in the session.

**Zero Voice Dependencies**
Pure text-based interface. No Web Speech API, no external audio libraries,
no browser permission requests.

---

## Setup and Deployment

**Prerequisites**
- GitHub account
- Vercel account (free)
- Anthropic API account with credits

**Step 1 — Clone and connect to Vercel**
```bash
git clone https://github.com/Ankitbharti2834/ai-portfolio-chatbot.git
```
Import the repo into Vercel at vercel.com.

**Step 2 — Add API key**
In Vercel project settings → Environment Variables:
```
ANTHROPIC_API_KEY = your_key_here
```

**Step 3 — Deploy**
Commit any change to the repo. Vercel auto-deploys in under 30 seconds.

**Step 4 — Test**
Visit your Vercel URL and open the chatbot. Ask any question. Check Vercel
logs to confirm POST /api/chat returns 200.

---

## Customization

To use this for your own portfolio:

1. Replace the CB_SYSTEM string in index.html with your own profile data
2. Update contact details, project descriptions, and Q&A answers
3. Optionally change the robot SVG color from gold (#F5A623) to match your
   portfolio theme
4. Deploy to your own Vercel project with your own API key

---

## Lessons Learned

**API key exposure** — The first version tried to call the Anthropic API directly
from the browser. That exposes the key in the network tab. The serverless proxy
pattern is the correct solution for any client-side AI integration.

**Model naming** — Several deployments failed because the model string was wrong.
The correct model string for external API calls must match exactly what Anthropic
publishes in their documentation. Internal Claude.ai model IDs are different from
API model IDs.

**Drag vs click detection** — A simple onclick handler on a draggable element
creates conflict. The solution is to track movement distance and only fire the
click action if movement was under 4 pixels.

**System prompt size** — Adding the full interview Q&A brought the system prompt
to around 3,000 words. Claude handles this well but response latency increases
slightly. Condensing the Q&A to key facts and answers rather than full prose
kept response time under 2 seconds.

---

## Project Status

Live and in production at https://ankitbharti-portfolio.vercel.app

Planned additions:
- Session persistence so conversation history survives a page refresh
- Analytics to track which questions are asked most often

---

## Contact

Ankit Bharti
abharti1040@gmail.com
https://www.linkedin.com/in/ankitbharti2834/
