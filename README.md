# Fully Automated AI Lead Generation & Follow-Up Pipeline

An n8n workflow that manages personalized B2B cold outreach, tracks email threads dynamically, automates smart follow-ups, and monitors live replies backed by a global error handler.

### Why this exists
Sending generic cold emails gets ignored, and manually tracking who needs a follow-up across hundreds of threads is unscalable. This system acts as a headless CRM. It uses AI to draft hyper personalized emails based on lead data, schedules follow-ups that stay nested in the exact same email chain, and instantly halts the automation the moment a prospect replies.

### How it flows
*   **Intake & Drafting (WF1):** Pulls fresh leads from Google Sheets. Uses LangChain (Gemini/Groq) to generate conversational outreach emails and saves them as Gmail drafts.
*   **Thread Tracking (WF2):** Polls the Gmail 'Sent' folder, captures the unique `THREAD ID` for the outbound email, and calculates precise dates for future follow-ups.
*   **Smart Follow-ups (WF3):** Runs daily to check for due follow-ups. It queries the Gmail API to ensure the prospect hasn't replied independently. If they haven't, it drafts the follow-up. 
*   **Reply Monitoring (WF4):** Watches the inbox live. Matches incoming replies to the database via `THREAD ID`, marks the lead as "Replied" (halting the sequence), and pings the team on Slack.
*   **Error Catching (WF5):** A background workflow catches any broken nodes across the pipeline and sends the exact execution URL to Slack for fast debugging.

### A few decisions worth calling out
*   **Raw MIME construction:** Follow-ups are built using raw Base64 MIME to ensure they nest perfectly into the original email thread, preventing broken or scattered email chains.
*   **Drafting over auto-sending:** Initial AI emails are saved as drafts, allowing for a quick human review before the first touch goes out. 
*   **Independent Reply Watcher:** Decoupling the reply monitor from the daily schedule ensures the system reacts instantly to a response, preventing embarrassing automated follow-ups.

### Stack
n8n · Google Gemini / Groq (LangChain) · Google Sheets API · Gmail API · Slack

### Demo
*   [https://www.loom.com/share/3754e8099bd04fefa5eac256fe78cf7b]

---

## 📸 Architecture

### 1. Initial Campaign Drafter (`WF1`)
![WF1 - Initial Campaign Drafter](Assets/WF1.jpg)

### 2. Sent Email Tracker (`WF2`)
![WF2 - Sent Email Tracker](Assets/WF2.jpg)

### 3. Smart Follow-up Drafter (`WF3`)
![WF3 - Smart Follow-up Drafter](Assets/WF3.jpg)

### 4. Reply Watcher (`WF4`)
![WF4 - Reply Watcher](Assets/WF4.jpg)

### 5. Universal Error Handler (`WF5`)
![WF5 - Universal Error Handler](assets/Error.jpg)
