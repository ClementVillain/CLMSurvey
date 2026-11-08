# CLMSurvey — AI-curated science watch

**A personal research-watch dashboard for AI, space, astronomy and physics.**
It collects papers and news every day, ranks them by relevance to a research profile, and summarises them with an LLM.

**Live:** [clmsurvey.vercel.app](https://clmsurvey.vercel.app) · Also packaged as an iOS app (Capacitor).

---

## Highlights

- **20+ sources aggregated**: arXiv API (cs.AI, cs.LG, cs.CV, cs.RO, cs.CL) plus RSS feeds from labs, agencies and specialised press (Google DeepMind, OpenAI, Hugging Face, NASA, ESA, Quanta Magazine…).
- **Relevance scoring**: each article is scored against a research profile (AI research, AI for space) with positive and negative keyword sets, so promotional and K-12 content is pushed down.
- **LLM summaries**: full article text is extracted (Trafilatura) and summarised through the Groq API.
- **Event calendar with cost control**: a free regex pre-filter keeps only articles that mention a date, then an LLM call extracts the exact upcoming event (launch, conference, results release). Most articles never reach the LLM.
- **Scheduled collection**: a daily cron job on Vercel fetches new items automatically.
- **Push notifications** (Web Push) and a native iOS wrapper.
- **Two storage modes**: Turso (libSQL) in production, local SQLite with no configuration.

---

## Tech stack

| Area | Technology |
|---|---|
| Backend | Python, FastAPI, Jinja2 templates |
| Data collection | arXiv API, feedparser, httpx, Trafilatura |
| AI | Groq API (LLM summarisation) |
| Database | Turso (libSQL) in production, SQLite locally |
| Deployment | Vercel (serverless + daily cron) |
| Mobile | Capacitor (iOS) |

---

## Project structure

```
app/
├── main.py         # FastAPI app and routes
├── config.py       # Categories, arXiv categories and RSS sources
├── collector.py    # Collection pipeline
├── fetchers/       # arXiv and RSS fetchers
├── relevance.py    # Relevance scoring for the research profile
├── summarizer.py   # Article extraction + LLM summary
├── events.py       # Upcoming-event extraction for the calendar
├── db.py           # Turso / SQLite storage
├── push.py         # Web Push notifications
├── templates/      # HTML templates
└── static/         # CSS and assets
api/index.py        # Vercel entry point
scripts/            # Daily fetch, icon generation
mobile/             # Capacitor iOS app
```

---

## Getting started

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload
```

Open http://127.0.0.1:8000 and click **Rafraîchir** (Refresh) to run the first collection. The interface is in French.

| Variable | Purpose |
|---|---|
| `GROQ_API_KEY` | LLM summaries and event extraction |
| `TURSO_DATABASE_URL`, `TURSO_AUTH_TOKEN` | Production database (without them, local SQLite is used) |
| `VAPID_PUBLIC_KEY`, `VAPID_PRIVATE_KEY` | Push notifications (optional) |
| `CRON_SECRET` | Protects the daily collection endpoint |

---

## Roadmap

- Semantic relevance ranking with embeddings instead of keywords
- Evaluation of ranking quality against manually labelled articles
