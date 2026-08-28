# DarEvents — Automated Events Platform (Tanzania)

**Jukwaa la matukio linalojisimamia** — data inakusanywa otomatiki kwa Tavily + AI.

## Features

- ✅ **Automated collection** (Tavily + AI structured extraction)
- ✅ **Sponsored / Ads always first**
- ✅ **Anyone can post an event** (“Weka Tukio”) — free for now + duration selector (demo $1 / 2 days)
- ✅ **Like 👍 / Dislike 👎 → Trending**
- ✅ Image support (when available)
- ✅ Self-managing: dedup, expire old, max 300
- ✅ Polished Swahili frontend
- ✅ M-Pesa ready UI, filters, map, digest
- ✅ Ready for cron / Docker / any VPS

## Project Structure

```
dar-events/
├── backend/
│   ├── main.py          # FastAPI app
│   ├── collector.py     # Tavily + AI collector
│   ├── config.py
│   ├── models.py
│   └── requirements.txt
├── frontend/
│   └── index.html       # Dynamic UI
├── data/
│   ├── events.json
│   └── sponsored.json
├── scripts/
│   └── run_collector.sh
├── .env.example
├── Dockerfile
└── README.md
```

## Quick Start (Local)

```bash
cd dar-events/backend
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

python collector.py seed
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

Open http://localhost:8000

## Production

1. Copy `.env.example` → `.env` and add keys:
   ```
   TAVILY_API_KEY=tvly-...
   OPENAI_API_KEY=...   # or xAI
   AI_BASE_URL=https://api.x.ai/v1
   AI_MODEL=grok-beta
   ```

2. Run:
   ```bash
   pip install -r backend/requirements.txt
   cd backend && python collector.py seed
   uvicorn main:app --host 0.0.0.0 --port 8000
   ```

3. Cron (every 6h):
   ```
   0 */6 * * * cd /path/to/dar-events/backend && python collector.py
   ```

## API Highlights

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/events` | List (sponsored first, or `?sort=trending`) |
| POST | `/api/events/submit` | Anyone posts an event (free) |
| POST | `/api/events/{id}/vote?vote=like\|dislike` | Like / Dislike |
| POST | `/api/collect` | Trigger auto-collection |
| POST | `/api/sponsored/{id}` | Mark sponsored |
| GET | `/docs` | Swagger |

## Monetization (planned)

- Free listing + optional duration boost (demo pricing ready)
- Later: low ticket commission (5–7%) optional
- Google Ads after real traffic

## License

MIT — built for Tanzania 🇹🇿
