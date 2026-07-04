# 🌊 Agentic AI Flood Risk Detection System
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21193891.svg)](https://doi.org/10.5281/zenodo.21193891)

An autonomous AI agent that monitors real-time weather conditions, predicts flood risk using a trained Machine Learning model, and proactively sends emergency alerts via Telegram — without requiring step-by-step human instructions at each stage.

Unlike a traditional ML pipeline that only returns a static prediction, this system uses an **LLM-powered reasoning agent** that decides *which tools to call, in what order, and what action to take* based on the situation — including whether an alert needs to be sent at all.

---

## 🎯 Motivation

Bangladesh faces recurring, high-impact flood events. Most existing flood-prediction tools stop at a prediction and leave the "what to do next" step to the user. This project explores an **agentic approach to environmental early-warning systems**, where an AI agent autonomously orchestrates data collection, risk analysis, and emergency communication — a direction relevant to my ongoing PhD research in flood risk management and hydroinformatics at Tulane University's River-Coastal Science and Engineering (RCSE) department.

---

## 🧠 How It Works

```
User/Trigger
     │
     ▼
┌─────────────────────────┐
│   LangGraph ReAct Agent   │  ◄── Powered by Google Gemini
│   (reasoning & planning)  │
└─────────────────────────┘
     │
     ├──► Tool: fetch_weather        → OpenWeatherMap API (rainfall, temp, humidity)
     │
     ├──► Tool: check_flood_risk     → Random Forest ML model (trained classifier)
     │
     └──► Tool: send_alert           → Telegram Bot API (conditional, only if risk is high)
                     │
                     ▼
          Human-readable risk summary
          + automated alert (if needed)
```

The agent independently decides:
1. Whether to fetch weather data for a given area
2. What inputs to pass into the flood-risk model (estimating missing values when necessary)
3. Whether the resulting risk level warrants sending an alert
4. What actionable advice to include in that alert

---

## 🛠️ Tech Stack

| Category | Technology |
|---|---|
| Agent Orchestration | LangGraph (ReAct agent pattern) |
| LLM / Reasoning | Google Gemini API (`gemini-2.5-flash-lite`) |
| Tool Integration | LangChain |
| Machine Learning | scikit-learn (Random Forest Classifier), StandardScaler |
| Weather Data | OpenWeatherMap API |
| Alerting | Telegram Bot API |
| Environment | Python 3, Google Colab |

---

## 📂 Project Structure

```
agentic-flood-risk-ai/
├── flood_agent.ipynb       # Main notebook: agent + tools + demo
├── models/
│   ├── flood_model.pkl     # Pre-trained Random Forest classifier
│   └── scaler.pkl          # StandardScaler used during training
├── requirements.txt
├── .env.example
└── README.md
```

> The Random Forest model and scaler were originally trained and deployed as part of a standalone Flask-based flood prediction web app: [flood-risk-app](https://github.com/rasel-2002/flood-risk-app). This project reuses that trained model as one autonomous tool within a larger agentic reasoning system.

---

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/rasel-2002/agentic-flood-risk-ai.git
cd agentic-flood-risk-ai
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Set up API keys
Copy `.env.example` to `.env` and fill in your own keys:
```
GOOGLE_API_KEY=your_gemini_api_key_here
TELEGRAM_BOT_TOKEN=your_telegram_bot_token_here
TELEGRAM_CHAT_ID=your_telegram_chat_id_here
OPENWEATHER_API_KEY=your_openweathermap_key_here
```

- **Gemini API Key** → [Google AI Studio](https://aistudio.google.com/app/apikey)
- **Telegram Bot Token** → create a bot via [@BotFather](https://t.me/BotFather)
- **Telegram Chat ID** → get yours via [@userinfobot](https://t.me/userinfobot)
- **OpenWeatherMap Key** → [OpenWeatherMap API](https://home.openweathermap.org/api_keys)

### 4. Run the notebook
Open `flood_agent.ipynb` in Jupyter or Google Colab and run all cells.

### 5. Example usage
```python
response = agent.invoke({
    "messages": [("user", "Dhaka এলাকার flood risk check করো এবং প্রয়োজন হলে alert পাঠাও")]
})
print(response["messages"][-1].content)
```

---

## ⚠️ Current Limitations

This is a research/portfolio-stage prototype, not a production early-warning service. Known limitations:
- Alerts currently go to a single Telegram chat ID (no multi-user broadcast yet)
- Requires manual/scheduled triggering (no continuous 24/7 monitoring loop yet)
- Relies on general weather API data rather than localized river-gauge sensors
- Not yet load-tested for concurrent or large-scale use

## 🔭 Future Work

- Multi-region monitoring with scheduled (cron-based) execution
- Integration with real river-gauge / hydrological sensor data
- Telegram group/channel broadcast for community-wide alerts
- Deployment as a persistent service (e.g., on a cloud server) instead of a notebook

---

## 👤 Author

**Md Rasel Uddin**
Incoming PhD Student, River-Coastal Science and Engineering (RCSE)
Hydroinformatics Lab, Tulane University
Research focus: Flood risk management, computational modeling, and Agentic AI for environmental early-warning systems

---

## 📄 License

This project is licensed under the MIT License.
