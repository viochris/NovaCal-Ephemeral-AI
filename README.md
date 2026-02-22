# 📅 NovaCal AI: Ephemeral Google Calendar Assistant (Telegram Bot Edition)

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white)
![Telegram](https://img.shields.io/badge/Telegram-Bot-2CA5E0?logo=telegram&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-Agent-blueviolet?logo=langchain&logoColor=white)
![Google Gemini](https://img.shields.io/badge/Gemini%202.5%20Flash-8E75B2?logo=google&logoColor=white)
![Google Calendar](https://img.shields.io/badge/Google%20Calendar-API-4285F4?logo=googlecalendar&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-success)

> 🛑 **CRITICAL NOTICE: PLEASE READ UNTIL THE END!**  
> Before cloning or deploying this repository, **you MUST read the "Limitations & Disclaimers" section at the bottom of this page.** This specific version utilizes a unique auto-destructing RAM-based memory architecture designed to balance user experience with API token efficiency.

## 📌 Overview
**NovaCal AI** is an enterprise-grade, highly capable Virtual Assistant built for seamless Google Calendar management directly from Telegram.

Powered by a **LangChain Tool-Calling Agent** and **Google Gemini 2.5 Flash**, this edition operates on an **Ephemeral (Slot-Filling) Architecture**. It temporarily retains conversational context in the server's RAM just long enough to gather missing parameters for a calendar task (e.g., waiting for you to provide the time after providing the date). Once the calendar tool is successfully executed, the memory automatically self-destructs (`[TASK_DONE]`), preventing contextual hallucinations and saving massive amounts of API tokens compared to persistent SQL memory.

> **🌐 EXPLORE OTHER VERSIONS OF NOVACAL AI:**
> * **[Stateful Edition (SQL Memory)](https://github.com/viochris/NovaCal-AI-Stateful.git):** If you need persistent, long-term memory across days/weeks.
> * **[Stateless Edition (Zero Memory)](https://github.com/viochris/NovaCal-AI-Telegram.git):** If you prefer lightning-fast, highly strict one-shot prompt execution.
> * **[Streamlit Dashboard Edition](https://github.com/viochris/NovaCal-AI-Streamlit.git):** If you are looking for the Visual Web UI version.

## ⚠️ IMPORTANT: Why This Bot Is Locked To A Single User?
> This bot is authenticated using your personal Google Calendar credentials. If it were open to the public, anyone on Telegram could read your private schedules, add fake events, or delete your important meetings!
>
> To prevent this massive security risk, the bot is strictly locked to YOU (the developer). By matching the user's ID with the `TELEGRAM_CHAT_ID` stored in your environment variables, the system ensures that only your specific Telegram account can give commands or read your Google Calendar. 
>
> If any stranger attempts to chat with the bot, it will instantly block them and send an intrusion alert directly to your DM.

## ✨ Key Features

### 🧠 Tool-Calling Agent Architecture
Using `create_tool_calling_agent`, the system navigates a strict Standard Operating Procedure (SOP):
1.  **Analyze Intent:** Understands complex time-based requests relative to the current system time.
2.  **Execute Tools:** Uses custom-built, highly optimized tools like `get_id_of_schedules` (The Sniper) and `get_all_schedules` to reliably fetch data.
3.  **Self-Correction (Swap Method):** If an event update fails, the Agent seamlessly falls back to creating a new event and deleting the old one.

### ♻️ Auto-Destructing Ephemeral Memory (Slot-Filling)
Equipped with a lightweight Python dictionary store (`ephemeral_store`), NovaCal remembers your ongoing conversation just long enough to complete a specific task. Once a CRUD operation is successful, the AI outputs a hidden `[TASK_DONE]` trigger, instantly purging your session from RAM. This guarantees the next task starts with a clean slate.

### 🛡️ Private Security Bouncer
The bot is hard-locked to a specific `TELEGRAM_CHAT_ID`. Any unauthorized attempts to interact with the bot are immediately blocked, logged, and silently reported to the developer's DM.

### ☁️ Cloud Deployment Ready (Railway)
Features a dynamic credential generator that automatically builds physical `credentials.json` and `token.json` files on the server directly from cloud environment variables upon startup. 

## 🛠️ Tech Stack
* **LLM:** Google Gemini 2.5 Flash (via `ChatGoogleGenerativeAI`).
* **Bot Framework:** `python-telegram-bot`
* **Orchestration:** LangChain (Tool-Calling Agent & `ChatMessageHistory`).
* **Calendar Integration:** Google Calendar API (`google-api-python-client` & `CalendarToolkit`).

## ⚠️ Limitations & Disclaimers
### 1. Volatile RAM Storage
Because memory is stored in the application's runtime dictionary (`ephemeral_store`), any server restart, sleep state, or crash will instantly wipe all ongoing conversational context. Do not use this architecture if you need long-term persistent memory.
### 2. Timezone Hardcoding
The time boundary extraction in the custom fetcher tools currently uses a fixed `+07:00` (WIB/Jakarta) timezone offset for daily queries.
### 3. Native Tool Bypassing
Native LangChain search tools (`CalendarSearchEvents`) are intentionally disabled/banned in the system prompt due to instability, replaced entirely by custom-built extraction functions for maximum reliability.
### 4. Occasional Contextual Amnesia (Over-Caution)
Generative models like Gemini 2.5 Flash can occasionally struggle with multi-turn context correlation. Even with explicit system instructions to check the chat history first, the AI might become overly cautious and ask to re-verify a detail you provided earlier. If this looping behavior occurs, explicitly command it to *"just create it with the provided details"*.
### 5. Trigger Word Omission (Memory Leak Risk)
The auto-destruct mechanism relies entirely on the LLM explicitly outputting the string `[TASK_DONE]` after a successful CRUD operation. While the system prompt strictly enforces this, generative AI models can occasionally hallucinate or fail to append this trigger word. If this happens, the user's conversational memory will remain active in the RAM rather than being purged, which may carry unwanted context into their next scheduling request.

---

> 💡 **DEVELOPER'S VERDICT & RECOMMENDATION**
> 
> **The Balanced Architecture:** This Ephemeral approach offers an optimal compromise between user experience and system efficiency. It maintains the natural conversational flow necessary to answer follow-up questions (e.g., prompting for a missing time or date) while completely avoiding the token bloat and context degradation associated with persistent SQL memory. By purging the memory buffer immediately after a successful API action, the LLM remains highly focused and cost-efficient. **This is the recommended architecture for daily, practical Telegram usage.**

## 📦 Installation & Deployment
1.  **Clone the Repository**
    ```bash
    git clone https://github.com/viochris/NovaCal-Ephemeral-AI.git
    cd NovaCal-Ephemeral-AI
    ```

2.  **Install Dependencies**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Local Environment Setup (`.env`)**
    Create a `.env` file for local testing:
    ```env
    TELEGRAM_TOKEN_Nova_cal_ephemeral=your_telegram_bot_token
    TELEGRAM_CHAT_ID=your_personal_telegram_id
    GOOGLE_API_KEY=your_gemini_api_key
    ```

4.  **Run Locally**
    ```bash
    python NovaCal-Ephemeral-AI-Telegram.py
    ```

### 🖥️ Expected Terminal Output
You will see the bot initialize, listen for commands, and automatically purge memory upon task completion:
```text
2026-02-22 08:30:15,123 - root - INFO - 🚀 NovaCal AI Telegram Bot is currently online and listening...
2026-02-22 08:35:10,001 - httpx - INFO - HTTP Request: POST https://api.telegram.org/bot...
2026-02-22 08:36:20,555 - root - INFO - 💥 MEMORY RESET: Ephemeral slot for User ID 123456 successfully destroyed!
2026-02-22 08:40:45,432 - root - WARNING - 🚨 INTRUSION ATTEMPT: Unauthorized access blocked from User ID: 9876...

```

### 🚀 Cloud Deployment (Railway)

This script is designed to be **Always On** via continuous polling. We highly recommend **Railway (PaaS)** for seamless GitHub integration and Docker deployment.

**Strict Instructions for Railway Deployment:** Do **NOT** upload your physical `credentials.json` or `token.json` files to the cloud. Instead, add these directly into your Railway Variables:

* `GOOGLE_CALENDAR_CREDENTIALS`: Paste the raw JSON content of your `credentials.json`.
* `GOOGLE_CALENDAR_TOKEN`: Paste the raw JSON content of your `token.json`.
* `TELEGRAM_TOKEN_Nova_cal_ephemeral`: Your bot token.
* `TELEGRAM_CHAT_ID`: Your exact developer chat ID.
* `GOOGLE_API_KEY`: Your Gemini API Key.

## 🚀 Usage Guide

Once the bot is running, start a chat on Telegram:

* `/start` - Initializes the bot and shows the welcome guidelines.
* `/info` - Displays technical specifications and architecture details.
* `/howtouse` - Provides the full CRUD operation cheat sheet.
* **Natural Multi-Turn Chat (Slot-Filling):** Talk naturally. The bot will remember context until the task is done.
  * *You:* "Schedule a meeting for tomorrow."
  * *Bot:* "Sure, what time and what is the title?"
  * *You:* "Call it Team Sync, from 2 PM to 3 PM."
  * *Bot creates event, then silently wipes its memory for the next task.*

---

**Authors:** Silvio Christian, Joe
*"Automate your schedule. Experience intelligent time management."*
